File for Model 1

scripts/model1_logistic_regression.ipynb

Way to Run it

1. Open the repository in VSCode
2. Make sure there is a .venv environment with the right requirements.txt installed
3. Open scripts/model1_logistic_regression.ipynb.
4. Select the .venv kernel in the notebook
5. Run the cells in order by using the Run All, or even run line for line per cell
6. The notebook then loads datasets/train_test_split.pkl (the output from feature engineering), trains the model, then saves the trained model to models/logreg_model.pkl

Reason for the notebook:

1. Technique used

I used Logistic Regression as my first model. Its a simple, interpretable baseline classifier thats commonly used for binary classification problems like this one, predicting whether a client subscribes (yes) or doesnt (no).

2. Loading the processed data

I loaded train_test_split.pkl, the processed train and test data that was already scaled and one hot encoded in the feature engineering notebook, so the model can be trained directly without repeating that work.

3. Hyperparameters

I set up the model with these hyperparameters:
- max_iter=1000 - this gives the model enough iterations to properly converge. The default of 100 isnt always enough, especially with 62 features after encoding.
- class_weight="balanced" - this helps with the imbalance i found during preprocessing (88.7% no, 11.3% yes), since it makes the model weight the minority "yes" class more heavily rather than just favouring the majority class.
- random_state=42 - this makes sure i can reproduce the same results if i run the notebook again.

4. Training the model

I trained (fit) the model on X_train and y_train, the processed training data from feature engineering.

5. Inference

I generated predictions on the test set (X_test) using two methods:
- predict() - gives the class label prediction, "yes" or "no"
- predict_proba() - gives the probability of subscribing, which is needed for metrics like ROC AUC

6. Saving the model

I saved the trained model to models/logreg_model.pkl using joblib, so it can be reloaded later without retraining, for use in the Part C performance evaluation.
