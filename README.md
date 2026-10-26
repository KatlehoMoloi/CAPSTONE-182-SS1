# CAPSTONE-182-SS1
SS1 - STADIOchoice- Trial conversion
# Motivation
StadioChoice is an a challenging transitional phase where they are experiencing less reliability on profits coming from a declining satellite customer base to moving towards building a streaming service. StadioStream is still in the early stages of growth and not self sufficient yet. It is a challenging time in the market for such a transition as high valued international investors in the streaming fields are also compete for the same subscribers. Putting a free trial period helps in having an entry point to attracting new streaming customers. The concern is that the percentage of the trial using customers who end up paying subscribers has dropped from 41% to 33% in the last two years. The business also faced an increase in the cost of acquiring new streaming subscriber, the cost increased from R240 to R355. Having that inverted relationship of decreased converted trial .customer and increased acquiring cost per customer results to a growing financial loss and add pressure on the streaming division while already operating in a loss. While customers are on the trial period, STADIOChoice would collect the behavioural data per household such as the plays, pausing, searches and abandonment actions. The data collected in that period is not being used as predictive indicator tool for identifying the customers who are likely to convert and which would be cancelling. If the business puts a predictive model for the trial customer behaviour in place, they would be able to identify customer outcome earlier and which allow an early intervention. Early trial customer intervention could be high customer engagement to fit customer needs, or better onboarding process when trial period ends. This initiative would be beneficial to the business's strategic focus of being ahead of the streaming market through retention and not only high sign ups. This will be an initiative will show a proactive trial to subscription conversion process.
# Problem Statement
Even though there is a clear fall in trial to paid conversion rate while the cost per streaming subscriber acquired is rising, there is still uncertainty on whether the trial period customer behaviour can be a determining factor whether trial subscribers are likely to convert to paying customers. Therefore, the aim in my report is to determine whether classification techniques are reliable enough to accurately predict trial to paid conversion using the STADIOChoice steaming viewing logs and the subscription and the customer billing information. Using this as evidence will assist in having proactive interventions during the trial period. 
# RAAIDD LOG
|RAAIDD|Description|
|------|--------------------|
|Risk  |A high risk of not requesting the right information in my columns on the data request and only realising in the final phase of completing the project|
|Risk  |The information from households could be considered personal information and make StadioChoice not willing to provide the full data to that extent.|
|Risk  |There is only 33% of trial users who convert which might risk my model to only lean to predict a result as "no conversion" most of the time while still seeming to perform well.|
|Risk  |If my planning phase takes too long, there might be time pressures in being able to model and validating.|
|Action|I need to have a well thought data request table to submit to StadioChoice to ensure I am not missing any important information and they also know which fields will need to be filled in on their side|
|Action|Make sure I understand the working on my GitHub platform in a well structure set up now so that it is ready once I receive the data.|
|Action|Proofread and better my motivation and problem statement against the objective of the project.|
|Action|Have my READMEcontent backed up and saved on my laptop separately, to ensure I have access to the work offline.|
|Assumption|Assuming StadioChoice is able to link the viewing of the data and billing for households because my data request is depenet on this.|
|Assumption|Assuming that viewing the behaviour during the trial period is what influences whether customers convert or not, rather than having a focus on conversion being mainly influenced by pricing and competitors actions.|
|Assumption|Using classification is the most fitting approach to the problem instead of predicting the time to converting.|
|Assumption|Assuming StadioChoice to be sending the raw data in the standard required format and in a reasonable time frame.|
|Issue| I have not faced any issues yet, only took time in understanding the GitHub platform which I will be working on.|
|Decision|I have decided to use the trial to paid conversion instead of the Churn because it was more direct in data available and the best fitting to a retention strategy StadioChoice would want.|
|Dependency|I wont be able to finalise the data request until I have a clear problem statement since the request will need to match my goal of the project to predict.|
|Dependency|Compiling a reliable model is dependent on StadioChoice providing the correct data I have requested.|
|Dependency|My RAAIDD log being complete was reliant on my motivation and problem statement being done and clear.|
|Dependency|Having a final report for the project will be dependent on a model being built and evelatated first since it will be based on results.|
 
 # SS2 Part A- Literature Review
 - [SS2 PART A](./pdf_docs/SS2%20PARTA%20CAPSTONE182%20(1).pdf)



# SS2 PART B - Modelling

- [Preprocessing](./Preprocessing.MD)
- [Feature Engineering](./FeatureEngineering.MD)
- [Model 1](./Model1.MD)
- [Model 2](./Model2.MD)

# SS2 PART C - Performance
- [Model 1 Performance](./Model1Performance.MD)
- [Model 2 Performance](./Model2Performance.MD)
- [Comaparison](./Comparison.MD)

# SS2 Part D- Recommendation Report
 - [SS2 PART D](./pdf_docs/SS2%20PARTD%20CAPSTONE182%20(1).pdf)

# SS1 PART C Additiional Data request 
- [SS1 PART C Additions](./SS1%20PART%20C%20Additions%20CAPSTONE182%20(1).pdf)

# SS3 Part B - Application to Client Data
STADIOchoice's free-trial to paid conversion for STADIOstream has fallen from 41% to 33% in two years, while the cost of acquiring a subscriber has risen from R240 to R355. This part applies my final SS2 method, a Random Forest classifier, to the full STADIOchoice data extract to test whether trial-period behaviour, combined with billing and customer data, can predict which trial users will convert. I cleaned the data, checked for leakage(removing the post conversion sessions), put one row per account and made a 70/15/15 split and tuned the model by using max_depth=5 in trying to prvent overfitting.

- [Notebook: SS3_PartB.ipynb](./SS3_PartB.ipynb)
- [Results](./results/)
- [Saved model](./models/)

** How to run:**
Needs Python 3 with `pandas`, `numpy`, `scikit-learn`, `matplotlib` and `joblib` (install with `pip install pandas numpy scikit-learn matplotlib joblib`). Open `SS3_PartB.ipynb`, choose **Kernel then --> Restart** then **Run All**

**Results on the unseen test set**

| Set | Accuracy | F1 score | ROC AUC |
|---|---|---|---|
| Train | 0.657 | 0.625 | 0.719 |
| Validation | 0.633 | 0.589 | 0.684 |
| Test | 0.659 | 0.622 | 0.707 |

# SS3 Part C - Video Presentation
- [Video presentation](https://stadiomultiversity-my.sharepoint.com/personal/26303337_stadio_ac_za/_layouts/15/stream.aspx?id=%2Fpersonal%2F26303337%5Fstadio%5Fac%5Fza%2FDocuments%2FCAP182%20SS3%2FSTADIOchoice%5FPresentationKM%2Emp4&referrer=StreamWebApp%2EWeb&referrerScenario=AddressBarCopied%2Eview%2E66080279%2Da094%2D45b1%2Db002%2D2f4e525058fa )