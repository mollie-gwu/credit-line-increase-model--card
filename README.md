# Group-22-DNSC-6301-Project

# Credit Line Increase Model Card

### Basic Information

* **Person or organization developing model**: Katie, Lindsay, Mallika, Wenxuan, `kmcquiddy@edu`,`lneff@edu`,`mallika@edu`,`wenxuan@edu`
* **Model date**: August 27, 2022
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [DNSC_6301_Project.ipynb](https://colab.research.google.com/github/lindsayneff/Group-22-DNSC-6301-Project/blob/main/DNSC_6301_Project.ipynb)

### Intended Use
* **Primary intended uses**: This model is an *example* probability of default classifier, with an *example* use case for determining eligibility for a credit line increase.
* **Primary intended users**: Students in GWU DNSC 6301 bootcamp.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.

### Training Data

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**ID**| ID | int | unique row indentifier |
| **LIMIT_BAL** | input | float | amount of previously awarded credit |
| **SEX** | demographic information | int | 1 = male; 2 = female
| **RACE** | demographic information | int | 1 = hispanic; 2 = black; 3 = white; 4 = asian |
| **EDUCATION** | demographic information | int | 1 = graduate school; 2 = university; 3 = high school; 4 = others |
| **MARRIAGE** | demographic information | int | 1 = married; 2 = single; 3 = others |
| **AGE** | demographic information | int | age in years |
| **PAY_0, PAY_2 - PAY_6** | inputs | int | history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above |
| **BILL_AMT1 - BILL_AMT6** | inputs | float | amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005 |
| **PAY_AMT1 - PAY_AMT6** | inputs | float | amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005 |
| **DELINQ_NEXT**| target | int | whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time |

* **Source of training data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **How training data was divided into training and validation data**: 50% training, 25% validation, 25% test
* **Number of rows in training and validation data**:
  * Training rows: 15,000
  * Validation rows: 7,500

### Test Data
* **Source of test data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **Number of rows in test data**: 7,500
* **State any differences in columns between training and test data**: None

### Model details
* **Columns used as inputs in the final model**: 'LIMIT_BAL',
       'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1',
       'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6',
       'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'
* **Column(s) used as target(s) in the final model**: 'DELINQ_NEXT'
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: Python version: 3.7.13
sklearn version: 1.0.2
* **Hyperparameters or other settings of your model**: 
```
Utilized a Decision Tree Classifier: Parameters Below   
{'ccp_alpha': 0.0,
 'class_weight': None,
 'criterion': 'gini',
 'max_depth': 6,
 'max_features': None,
 'max_leaf_nodes': None,
 'min_impurity_decrease': 0.0,
 'min_samples_leaf': 1,
 'min_samples_split': 2,
 'min_weight_fraction_leaf': 0.0,
 'random_state': 12345,
 'splitter': 'best'}
```
### Quantitative Analysis
#### Correlation Heatmap
![image](https://user-images.githubusercontent.com/111469896/186174958-6faa4ef6-80bc-492e-b1de-5138dd9d7b87.png)
* **Heatmap Description**: Visual representation of Pearson correlation matrix. Plot shows the correlation between all variables with darker colors indicating high negative correlation while lighter colors indicate strong positive correlation. Notable correlations include the race-credit_delinq correlation which is highly negatively correlated.

#### Iteration Plot: Final Model 
![image](https://user-images.githubusercontent.com/111469896/186176145-4f4bd87c-b0dd-4bea-b3e2-53b9f9666e5d.png)
* **Plot Description**: The above plot demonstrates the AUC lines for both training and validation data as well as the AIR curve for Hispanic-White selection. The training data approaches values close to one, which indicate that increases in model depth potentially can cause overfitting of the model using training data. This is made apparent as the validation AUC peaks and then drops off and becomes downward sloping. The AIR score for hispanic to white was selected because it initially had a low level which fell below 0.80 ratio guidelines for adverse selection. It is necessary to select a model depth with a ratio above 0.80 with possible choices made clear in the above graph.

#### Metrics Used to Evaluate Final Model:
* **Model Depth**: 6
* **Training AUC**: 0.783722
* **Validation AUC**: 0.749610
* **Test AUC**: 0.7438
* **5-Fold SD Score**: 0.017665
* **AIR Score**: 0.833205
* **Accuracy at Cutoff 0.18 for Confusion Matrix**: 0.7384
* **Justification for Model Selection**: The iteration plot demonstrates peak model fairness, judged by the Hispanic to White AIR curve between a tree depth of 6 or 7. In this segment, accuracy also peaks, as shown by the validation AUC curve around a depth of 6. Both of these scores indicate that a tree of depth 6 or 7 could be considered as the final model. For the purposes of this project, a final model of depth 6 was selected, prioritizing slight accuracy over fairness as the model of depth 7 demonstrated slightly higher fairness with a higher Hispanic to White AIR score of 0.835886, but a lower validation AUC score of 0.742115. 

### Ethical Considerations:
#### Potential negative impacts of using model:
* **Math or software considerations**: 
* The model was evaluated using a 0.18 probability cutoff for lending, meaning those with a 0.18 or smaller probability of default will be extended credit while those with predicted values above would not be extended credit. This cutoff could be changed depending on the firms desired cutoff range. While higher cutoffs would result in increased accuracy of the model, the resulting impact would be increased lending which could be a negative impact for the firm but not consumers. Conversely, firms could lower the desired cutoff range of lending, resulting in decreased accuracy and less lending.
* An additional math or software consideration is the split of training, testing, and validation data. Currently, the model divides data using a random seed which divides training data as 50% of the data and validation and testing as 25% each. While the split is appropriate for model development, depending on the data in each segment there could be changes in the model. Typically it is desirable to use past data for training and more current data for testing. If the split used in this model happens to use past data to test the model, it might overestimate the success of the model. 

* **Real world risks**:
* Although the model scores relatively well, the model could be used for credit lending and still has racial bias. Despite ensuring that the final model scores above 0.80 for protected groups AIR and has an appropriate fairness score, this measure of selection is not necessarily a good score if just hitting the .80 threshold. This still indicates that there is bias in the model and those protected groups could be prevented from accessing credit.
* Additionally, the model does not consider intersectional protected groups and is extremely one dimensional in its evaluation of bias. The confusion matrices and subsequent AIR scores are only considered for single identity groups. This proves to be an issue to only evaluate race alone or gender alone rather than the compound impact of race, gender, sexuality, religion, socioeconomic status, ability etc and other factors which interact to form a person's lived experience. The failure to consider intersectional groups and the ways that the model deems their credit worthiness could result in further marginalization and has real life implications for wealth accummulation in the US for those individuals.


#### Potential Uncertainties:
* **Math or software considerations**:
* It is uncertain whether changes in the software could increase model effectiveness over time. It is possible to generate a different model using other forms of software which might result in different results and nodes for decision making in the tree. This would require increased testing and creating a tree or model using different software. 

* **Real world risks**:
* While the model has been tested on various data sets, there is not understanding of how the model performs in the real world with real individuals. It is recommended that the model continues to be monitored if deployed to ensure that it performs fairly and accurately.
* As mentioned previously, there is uncertainty with respect to how the model performs on certain identity intersections. Protected groups are only tested to one dimension so it is unclear whether the model has increased bias against those with multiple intersecting identies, for example, Black women. It is unclear if this would additionally have a negative effect on the firm as the model may deny credit and future financial services to potential customers due to the current model's method of predicting credit delinquency. This could cause the firm to lose customers depending on the methods of evaluation and cutoff measures.
* The model data could be improved in prediction of credit delinquency. The model does not indicate whether certain factors in the economy or natural events could contribute to credit delinquency or failure to pay bills for certain year or time periods. For example, a unique effect like the global pandemic could affect ability to pay or past payment behavior. An event like this might not be indicative of overall credit history but would not be accounted for in the model. If used for lending, humans should consider external factors which might impact behavior. Credit score might be considered, however, credit scores are highly racialized and might introduce increased bias to the model.

#### Any Unexpected Results:
* One unexpected result was that the AIR score for female-male was 1.02 which indicated that women were accepted at a higher proportion than men. While the proportion is small, this could be a step in the right direction of helping women accumulate credit. Historically, women were not able to access their own lines of credit in the US until the 70's, so it was positive to see that the selection was good for women with the final model.

