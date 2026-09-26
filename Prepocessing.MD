## File for preprocessing
"scripts/preprocessing.ipynb"

## Way to Run it
1. Open the repository in VSCode
2. Makesure there is a ".venv" envirenmoent with the right "requirment.txt installed
3. Open "screipts/preprocessing.ipynb".
4. Selete the ".venv" kernel in the notebook
5. Run the cells in order by using the Run All, or even run line for line per cell
6. The notebook then loads" datasets/bank-additional-full.csv" then saves a cleaned version to "datasets/bank_cleaned.csv"

## Reason for the notebook:

### 1. To Load the dataset
The Banking Marketing dataset named "bank_additional-full.csv" has 41 188 rows and 21 columns. It was has a semicolon because usually thats how it was formatted.

### 2. Initial inspection 
I was able to check what the data had by shape, column names, data tyoes and the first 5 rows to make sure everything had loaded correctly while matching the descripiton given from UCI.

### 3. Checking Missing Values
The dataset didnt use bank or "NaN" for missing data data. The data set instead has categorical columns that use the string "unknown" as the values that are missing or unspecifed informattion. The counted unknown values per column shows : 
| Column   | Unknown count |
|----------|---------------|
| job      | 330           |
| marital  | 80            |
| education|1731           |
| default  | 8597          |
| housing  | 990           |
| loan     | 990           |

I kept the term unknown as its own valid category instead of dropping or imputing these rows because that meant having to remove a big part of the data, especially for the default that makes 21% of the rows. The "unknown might also be informative incases wgere a client now disclsoing the default status could contribute to their likelihood of subscribing.

### 4. Duplicate rows 
I had to check for the duplicated rows by using "duplicated()" and that found 12 duplicates
I decided to drop these because they wont be adding any new information. After removing them the dataset went to 41176 rows with 21 columns.

### 5. Dropped the duration column
The duration column showed the record of the length of the last contact call. This only shows after the called has happened so it would not be realisitclly reliable to use a on a model that has to still decide who to contact in the first place. It can also be leaking future inormation into the model and give it a misleading high performance. So, dropping the column matched with the liakage issue which was highlisted in my part A literature (Moro et al., 2014)
### 6. Target variable class balance
I checked the distrubution of the target variable which was "y", which sees whether the client subscribed to a term deposit or not
- No: 88.7% ; - Yes: 11.3%

The data has a clear imbalance which more "no's"(88.7%) than "yes's"(11.3%). Its important for me to keep in mind for my modelling and evaluation process because it will influence choices like using "class_weighted=balanced" and rather focusing on F1 score and ROC-AUC instead of just accuarcy.

### 7. Saved the cleaned dataset
After the process and steps taken abouve to clean the data, the cleaned data set was saved on "datasets/bank_cleaned.csv". This will be the file i use as input for the feature engineering notebook in my next step. 


