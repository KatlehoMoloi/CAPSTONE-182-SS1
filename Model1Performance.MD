File using for Model 1 performance

## Process to run it
1. Open the repository in VSCode
2. Make sure there is a .venv environment with the right requirements.txt installed
3. Open scripts/model1_performance.ipynb.
4. Select the .venv kernel in the notebook
5. Run the cells in order by using the Run All, or even run line for line per cell
6. The notebook loads the already trained models/logreg_model.pkl and the test set from datasets/train_test_split.pkl, then generates predictions and evaluation metrics. It doesnt retrain the model, since that already happened in scripts/model1_logistic_regression.ipynb

### Performance Results
## Metrics
- Accuracy : 0.830
- F1 score (yes class): 0.461
- ROC AUC score: 0.800

# Confusion Matrix 
|           | Predicted No | Predicted Yes |
|-----------|--------------|---------------|
|Actual NO  | 6239         | 1069          |
|Actual Yes | 329          | 599           |

# Classification report
|           | Precision| Recall | f1 score | support|
|-----------|----------|--------|----------|--------|
|NO (0)     | 0.95     | 0.85   |   0.90   | 7308   |
|Yes (1)    | 0.36     | 0.65   |   0.46   | 928    |

# View
Although seeing 83% accuracy looks seems like a strong numbe to rely on, it would not be a true relection because the dataset is imbalanced with 88% no's and 11.3% yes. Accuracy alone would be misleading because a model that just predicted no every time would also still score around 88.77% accuracy without being useful.
What would give a better picture would be looking at class specific numbers because the model then correctly identifies 65% of clients who actually subscribed which would be the more business relevant number. Going back to the StadioChoice project, I would rather catch the most likely converters even though there might be catching some who wont convert in between. This is a precision trade off with only 36% of the clients being flagged by the model to "yes" actually convert which means most flagged clients wont subscribe. The trade off is what steams from setting class_weight=balanced during the training phase, so that the model is pushed to catch more of the minority class at the cost of mroe false positives.
The ROC AUC of 0.800 shows that the model overall has a good ability to separate the two classes across the different probability thresholds. 

