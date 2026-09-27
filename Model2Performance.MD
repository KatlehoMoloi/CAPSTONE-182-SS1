# Model 2 Performance
"scripts/model2_performance.ipynb"

# How to run it 
1. Open the repository in VSCode
2. Make sure there is a .venv environment with the right requirements.txt installed
3. Open scripts/model2_performance.ipynb.
4. Select the .venv kernel in the notebook
5. Run the cells in order by using the Run All, or even run line for line per cell
6. The notebook loads the already trained models/rf_model.pkl and the test set from datasets/train_test_split.pkl, then generates predictions and evaluation metrics. It doesnt retrain the model, since that already happened in scripts/model2_random_forest.ipynb

## Performance results
# Metrics:
Accuracy: 0.870

F1 score (yes class): 0.513

ROC AUC score: 0.809
# Confusion Matrix
Actual No | Predicted No: 6603 | Predicted Yes: 705
Actual Yes | Predicted No: 364 | Predicted Yes: 564
# Classification Report
No (0): precision = 0.95, recall = 0.90, f1 Score = 0.93, support = 7308
Yes (1): precision = 0.44, recall = 0.61, f1 Score = 0.51, support = 928

# View
The Random Forest had achieved an accuracy of 87% which outperformed the Logistic Regression of 83%. The main cause of the improvement comes from the lower number oif false positives. Model 1(Random Forest) has misclassified 705 majority class (no) instances as (yes) compared to 1069 for Logistic Regression. This is what increses precision for the minority class from 0.36 up to 0.44.
Another factor to highlight is that Random Forest managed to reduce false alarms while still keeping the high recall. It correctly recorded 564 actual subscribers (the 61% recall for "yes") which is very close to the Logistic Regression recall of 65%. With the precision improving while recall resmained strong, the F1score for the positive class then rose up to 0.513. 
The ROC AUC score for Random Forest is 0.809. 

