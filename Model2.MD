## File for Model 2
`scripts/model2_random_forest.ipynb`

## Way to Run it
1. Open the repository in VSCode
2. Make sure there is a `.venv` environment with the right `requirements.txt` installed
3. Open `scripts/model2_random_forest.ipynb`.
4. Select the `.venv` kernel in the notebook
5. Run the cells in order by using the Run All, or even run line for line per cell
6. The notebook loads `datasets/train_test_split.pkl` (the same processed split used for Model 1), trains the model, then saves the trained model to `models/rf_model.pkl`

## Reason for the notebook:

### 1. Technique used
I used Random Forest as my second model. Its an ensemble method that builds many decision trees and combines their predictions, which usually handles non linear relationships and feature interactions better than a single Logistic Regression model.

### 2. Loading the processed data
I loaded the same `train_test_split.pkl` file that i used for Model 1, so both models are trained and tested on the exact same data. This keeps the comparison between them fair in Part C.

### 3. Hyperparameters
I set up the model with these hyperparameters:
- `n_estimators=100` - the number of trees in the forest. Reduced from an earlier version to keep the saved model file size manageable, while more trees generally gives more stable predictions.
- `max_depth=15` - limits how deep each tree can grow. This was capped after the first version (with no limit) produced an overly large model file, and capping the depth actually improved generalisation too.
- `min_samples_split=2` - the minimum number of samples needed to split a node, this is the sklearn default.
- `min_samples_leaf=1` - the minimum number of samples allowed in a leaf node, also the sklearn default.
- `max_features="sqrt"` - each tree only considers the square root of the total number of features when looking for the best split, this adds randomness between trees and helps avoid overfitting.
- `class_weight="balanced"` - same as Model 1, this helps with the imbalance i found during preprocessing (88.7% no, 11.3% yes), by making the model weight the minority "yes" class more heavily.
- `random_state=42` - makes sure i can reproduce the same results if i run the notebook again.
- `n_jobs=-1` - uses all available CPU cores to train faster, since Random Forest can be slower to train than Logistic Regression.

### 4. Training the model
I trained (fit) the model on `X_train` and `y_train`, the same processed training data used for Model 1.

### 5. Inference
I generated predictions on the test set (`X_test`) using two methods, same as Model 1:
- `predict()` - gives the class label prediction, "yes" or "no"
- `predict_proba()` - gives the probability of subscribing, which is needed for metrics like ROC AUC

### 6. Saving the model
I saved the trained model to `models/rf_model.pkl` using `joblib`, so it can be reloaded later without retraining, for use in the Part C performance evaluation
