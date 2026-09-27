# Model Comparison
scripts/comparison.ipynb
# Ways to run it:
1. Open the repository in VSCode
2. Make sure there is a .venv environment with the right requirements.txt installed
3. Open scripts/comparison.ipynb.
4. Select the .venv kernel in the notebook
5. Run the cells in order by using the Run All, or even run line for line per cell
6. The notebook loads both saved models (models/logreg_model.pkl and models/rf_model.pkl) along with the shared test set from datasets/train_test_split.pkl, generates predictions for both models, outputs a side-by-side comparison table, and generates a combined ROC curve chart saved to scripts/visualisations/roc_curve_comparison.png

## Performance Summary

| Metric | Logistic Regression | Random Forest |
|--------|---------------------|---------------|
| Accuracy | 0.830 | 0.870 |
| Precision ('yes') | 0.360 | 0.440 |
| Recall ('yes') | 0.650 | 0.610 |
| F1-Score ('yes') | 0.461 | 0.513 |
| ROC-AUC Score | 0.800 | 0.809 |

# The Accuracy and Class Imbalance
The Random Forest was reaching higher raw accuracy of 87% compared to Logistic Regressions 83%, but accuracy alone is misleading for these comparisons because 88.7% of the clients in the orgininal dataset (Bank Markting data) did not subscribe to the term deposit. Testing the model would predict "no" for every client would be showing 88.7% accuracy while not being practical. its important to assess the performance specififcally on the minority subcribers with "yes" to have a relaistic view.

  # precision. recalla dn trade offs
Logistic Regression has a focus to recall (the 0.65 vs 0.61) which meant correctly identifying 599 out of 928 actual subscriners. The disadvantage about this is that it produces 1069 false positives and would then lead to a low precision score of 0.36
Random Forest then was able to reduce the false alarms down to 705 and this then increases the precision to 0.440 but still being able to maintain the high recall of 0.61 but capturing 564 actual subscribers.
The Random Forest reflects a better balance between precision and recall, the f1 score for the positive class reached 0.513 and did better than the Logistic Regression of 0.461.
  # Model capability
The Random Forest showed a stronger ability for this Bank Marketing dataset and it shows in the ROC AUC score of 0.809 compared to the Logistic Regression 0.800. This improvement then comes from Random Forest ability to get the non linear patterns, interaction terms and nonlinear boundries it picked up in the dempgraphics and macroeconomic features and not just sticking to a linear relationship.
  # Final Model Recommendation
I would recommend the Random Forest model in this case because it is able to keep a balance between showing postential term deposit subscribers and still preventing wasted marketing resources on the nonsubscribers. The higher precision,F1 score and ROC Auc show the nonlinear relationship is better for the data.
