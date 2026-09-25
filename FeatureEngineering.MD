## File for feature engireering
`scripts/feature_engineering.ipynb`

## Way to Run it
1. Open the repository in VSCode
2. Make sure there is a `.venv` environment with the right `requirements.txt` installed
3. Open `scripts/feature_engineering.ipynb`.
4. Select the `.venv` kernel in the notebook
5. Run the cells in order by using the Run All, or even run line for line per cell
6. The notebook then loads `datasets/bank_cleaned.csv` (the output from preprocessing) then saves two outputs: `datasets/preprocessor.pkl` and `datasets/train_test_split.pkl`

## Reason for the notebook:

### 1. Load the cleaned dataset
I loaded the `bank_cleaned.csv` file that came from the preprocessing notebook, then split it into the features (`X`) and the target (`y`).

### 2. Identify the column types
I separated the columns into numeric and categorical groups so they could be handled differently:
- Numeric columns: `age`, `campaign`, `pdays`, `previous`, `emp.var.rate`, `cons.price.idx`, `cons.conf.idx`, `euribor3m`, `nr.employed`
- Categorical columns: `job`, `marital`, `education`, `default`, `housing`, `loan`, `contact`, `month`, `day_of_week`, `poutcome`

### 3. Encoding the categorical variables
I used `OneHotEncoder` instead of Label Encoding. Label Encoding would lead to an artificial order on the categories such as 0<1<2, which would not be right for nominal variables like `job`, `marital` and `education` that dont have a real order between them. One Hot Encoding avoids this by creating a separate binary column for each category.

### 4. Scaling the numeric features
I used `StandardScaler` on the numeric columns because Logistic Regression is sensitive to feature scale, so standerising the numeric values means no single column can dominate the model just because it has a bigger range of numbers.

### 5. Splitting before fitting, to avoid leakage
I split the data into train and test sets before fitting the encoder and scaler. The `ColumnTransformer` was only fit on the training data using `fit_transform`, then the test data was transformed using `transform` only, without fitting it again. This keeps the test set truly unseen during preprocessing, matching the same leakage concern i already flagged in my preprocessing notebook when dropping `duration`.

I also used `stratify=y` in the split so that the class balance (88.7% no, 11.3% yes) stayed the same in both the train and test sets, since i already knew from preprocessing that the data was imbalanced.

After this, the shapes were:
- Train set: 32 940 rows, 62 columns
- Test set: 8 236 rows, 62 columns

(the 62 columns comes from 9 scaled numeric columns plus 53 one hot encoded categorical columns)

### 6. Saving the outputs
I saved two files so the modelling notebooks can use them directly:
- `datasets/preprocessor.pkl` - the fitted `ColumnTransformer`, so any new data can be transformed the same way using what was learned from the training set
- `datasets/train_test_split.pkl` - the processed `X_train`, `X_test`, `y_train` and `y_test`, ready to be loaded straight into the Model 1 and Model 2 notebooks