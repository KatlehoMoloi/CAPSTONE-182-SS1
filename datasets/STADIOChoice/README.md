# STADIOchoice — Trial-to-Paid Conversion: Data Extract

Four tables covering trials activated **1 June 2024 to 30 June 2026**. Joined on `account_id`, which is consistent across all four tables and across platforms, as your request requires.

> **This extract supersedes the earlier three-table version** and is built against your *SS1 Part C: Data Request Additions*. Three things changed:
>
> 1. **`table1` is now one row per viewing session**, not one row per account. This is what `session_id` is for — your note says session-level genre tracking is impossible without it, and that is correct, so the grain of the table had to change to carry it.
> 2. **`search_query_text` and `genre_watched_per_session`** are new on table 1.
> 3. **`table4_trial_user_demographics.csv` is new**, carrying `age_band` and `stated_interests`.

## Files

### `table1_trial_viewer_behaviour.csv`
**One row per viewing session.** Rows: 123,470 sessions across 18,700 accounts.

| Column | Type | Description |
|---|---|---|
| `account_id` | String | Account identifier. **Repeats across sessions** — one row per viewing session now, not one per account. |
| `session_id` | String | **New.** Unique identifier for a single viewing session. This is the key that makes session-level genre tracking possible. |
| `session_start` | Datetime | When the session began, `YYYY-MM-DD HH:MM`. |
| `trial_start_date` | Date | Date the free trial was activated, `YYYY-MM-DD`. Account-level, repeats down the account's sessions. |
| `main_device` | Categorical | Smart TV / Mobile / Web / Tablet. The account's primary device, repeated per session. |
| `genre_watched_per_session` | Categorical | **New.** The genre watched in *this* session — the granular field for feature engineering. |
| `search_query_text` | String | **New.** Semicolon-separated search queries made during this session. Empty where the viewer searched nothing. |
| `session_watch_time` | Numeric | Minutes of content consumed in this session. |
| `total_watch_time` | Numeric | Account total minutes across the whole trial. Aggregate, repeats per session. |
| `genres_watched_list` | String | Comma-separated distinct genres across the whole trial. Aggregate kept as your sanity check. |
| `searches_made` | Integer | Account total search actions across the trial. Aggregate, repeats per session. |
| `no_results_searches` | Integer | Of those, how many returned no results. Aggregate, repeats per session. |

### `table2_subscription_billing.csv`
One row per account, captured at trial sign-up. Rows: 18,756

| Column | Type | Description |
|---|---|---|
| `account_id` | String | Account identifier. One row per account. |
| `payment_plan_filled_in` | Boolean | True where payment details were captured at sign-up. |
| `sign_up_platform` | Categorical | App / Website / Promotion Ad / Referral. |
| `region` | Categorical | SA province code: GP, FS, KZN, NW, NC, WC, MP, LP, EC. |
| `referral_or_promotion_code` | Boolean | True where sign-up was initiated by a referral or promotion code. |
| `trial_plan_chosen` | Categorical | Basic / Standard / Premium. |

### `table3_conversion_outcome.csv`
One row per account. Contains the target. Rows: 18,756

| Column | Type | Description |
|---|---|---|
| `account_id` | String | Account identifier. One row per account. |
| `converted_to_paid_subscription` | Boolean | **Target.** True where the trial converted to a paying subscription; False where it lapsed. |
| `date_of_conversion` | Date | `YYYY-MM-DD`. Empty where no conversion occurred. |
| `early_churn` | Boolean | True where a converted subscriber cancelled within 90 days of converting. **Empty where there was no conversion** — not False. |

### `table4_trial_user_demographics.csv`
**New.** One row per account, captured at trial sign-up. Rows: 18,756

| Column | Type | Description |
|---|---|---|
| `account_id` | String | Account identifier. One row per account. |
| `age_band` | Categorical | 18-24 / 25-34 / 35-44 / 45-54 / 55+. Empty where not lawfully held. |
| `stated_interests` | String | Comma-separated interest categories selected at sign-up. Empty where the viewer selected none. |

## Data quality notes

This is an extract from live operational systems and has **not** been cleaned. Expect:

- Missing values in optional / conditionally-populated fields
- Inconsistent casing and stray whitespace in some free-text and categorical fields
- A small number of duplicate records
- Occasional out-of-range or implausible values from data-capture errors
- Some date fields captured in inconsistent formats

**Target:** `converted_to_paid_subscription` in `table3_conversion_outcome.csv`. The conversion rate in this extract is **47.1%**, and **12.2%** of converted accounts churned early.

## The aggregates really do reconcile

You asked for the trial-level aggregate to stay "as a sanity check", so it is worth being precise that it genuinely is one. The aggregates on table 1 are computed **from** the session rows, not drawn separately:

- `searches_made` equals the total number of queries in `search_query_text` across that account's sessions.
- `no_results_searches` is a subset of `searches_made`, so it is always less than or equal to it.
- `genres_watched_list` is exactly the distinct set of `genre_watched_per_session` values for that account.
- `total_watch_time` is the sum of `session_watch_time` across the account's sessions.

Two caveats before you rely on that. First, the quality notes above still apply — a small number of **duplicate session rows** exist, so re-deriving `searches_made` from a table you have not de-duplicated will overshoot. De-duplicate on `session_id` first. Second, `genre_watched_per_session` carries the usual casing and whitespace variants, so normalise case before taking a distinct set or you will count `Drama` and `DRAMA` separately.

## Modelling notes

**The grain change matters.** Tables 2, 3 and 4 are one row per account; table 1 is one row per session. Join them and you will fan the account-level rows out across sessions — which will silently weight your target towards heavy viewers. Aggregate table 1 to account level first, or be deliberate about modelling at session level with a grouped split.

**Split by account, not by session.** `account_id` repeats across session rows. A random row-level split puts the same account on both sides of the evaluation.

**`early_churn` is empty, not False, where there was no conversion.** It is only defined for accounts that converted. Treating empty as False merges "did not churn" with "never had the chance to", and will distort any model you build on it.

**Leakage warning.** `date_of_conversion` and `early_churn` are both consequences of the outcome — they *are* the label, or come after it. Only tables 1, 2 and 4 are knowable during the trial. Note also that `total_watch_time`, `searches_made`, `no_results_searches` and `genres_watched_list` summarise the **whole** trial, so they are only legitimate for a model scoring at the end of the trial. If you want to predict earlier — say at day 3, which is the more useful business question — rebuild those aggregates yourself from the session rows up to your chosen cut-off, using `session_start`.

_Synthetic data supplied for the CAP182 capstone project. Not real client data._
