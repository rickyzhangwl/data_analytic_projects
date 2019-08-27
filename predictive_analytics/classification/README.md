# Project: Predicting Default Risk
![pic](https://www.incimages.com/uploaded_files/image/970x450/business-loan-illustration-1940x900_35212.jpg)

## The Business Problem
A small bank got about 500 loan applications and I am responsible for determining if customers are creditworthy to give a loan to. For this project, I will analyze the business problem using the classification model and provide a list of creditworthy customers to the manager of the bank.

I have the following information to work with:
1.  Data on all past applications
2.  The list of customers that need to be processed

## Step 1: Business and Data Understanding
#### Key Decisions:
**1. What decisions needs to be made?**

The decisions to be made is which customers are creditworthy to give a loan to.

**2. What data is needed to inform those decisions?**

The past loan applicants’ data is needed to train the predictive model, while the new set of loan applicants’ data is required to make prediction based on the predictive model. The two data sets have below common variables that possibly useful for the prediction and informing decisions.

| Variable                          | Data Type |
|-----------------------------------|-----------|
| Account-Balance                   | String    |
| Duration-of-Credit-Month          | Double    |
| Payment-Status-of-Previous-Credit | String    |
| Purpose                           | String    |
| Credit-Amount                     | Double    |
| Value-Savings-Stocks              | String    |
| Length-of-current-employment      | String    |
| Instalment-per-cent               | Double    |
| Guarantors                        | String    |
| Duration-in-Current-address       | Double    |
| Most-valuable-available-asset     | Double    |
| Age-years                         | Double    |
| Concurrent-Credits                | String    |
| Type-of-apartment                 | Double    |
| No-of-Credits-at-this-Bank        | String    |
| Occupation                        | Double    |
| No-of-dependents                  | Double    |
| Telephone                         | Double    |
| Foreign-Worker                    | Double    |

**3. What kind of model do we need to use to help make these decisions?**

As the answer of whether a loan applicant is creditworthy is binary (yes or no), so we need to use Binary Classification Model, such as Logistical Regression and Decision Tree, to make the decisions. The Forest Model and Boosted Model, usually for Non-Binary Model, are also applicable here.

## Step 2: Building the Training Set
By verifying which fields are useful for analysis, we can remove or impute the fields in cleanup process.

**2.1 Removed fields**

The field `Duration-in-current-address` should be removed as its portion of missing value is 69%, too much to be helpful on predictive model.

![pic1](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/classification/pics/1_removed_fields.png)

Four fields including `Concurrent`, `Guarantors`, `Foreign-Worker`, `No-of-dependents` and `Occupation`were removed due to low variability.

![pic2](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/classification/pics/2_removed_fields.PNG)

![pic3](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/classification/pics/3_removed_fields.png)

Besides, the field `Telephone`should be removed as there is no logical relation to credit application result.

**2.2 Imputed fields**

The portion of missing value in this field `Age-years` is only 2%, so it could be imputed by the median value (33.0).

![pic4](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/classification/pics/4_imputed_fields.png)

## Step 3: Train Classification Models
- Which predictor variables are significant or the most important? Please show the p-values or variable importance charts for all of your predictor variables.

- Validate your model against the Validation set. What was the overall percent accuracy? Show the confusion matrix. Are there any bias seen in the model’s predictions?

### 3.1 Significant Predictor Variables of Each Model
#### 3.1.1 Logistic Regression
**The 7 Significant Predictor Variables from Alteryx model**
1. `Account-Balance`
2. `Payment-Status-of-Previous-Credit`
3. `Purpose`
4. `Credit-Amount`
5. `Length-of-current-employment`
6. `Instalment-per-cent`
7. `No-of-Credit-at-this-bank`

![pic5](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/classification/pics/5_lr_coef.png)

#### 3.1.2 Decision Tree
**The 4 Significant Predictor Variables from Alteryx model**
1. `Account-Balance`
2. `Most-valuable-available-asset`
3. `Purpose`
4. `Value-Savings-Stocks`
![pic6](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/classification/pics/6_dt_vars.png)

#### 3.1.3 Forest Model
**Most Important Predictor Variables from Alteryx model**
1. `Credit-Amount`
2. `Age-Years`
3. `Duration-of-Credit-Month`
![pic7](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/classification/pics/7_forest_vars.png)

#### 3.1.4 Boosted Model
**Most Important Predictor Variables from Alteryx model**
1. `Account-Balance`
2. `Payment-Status-of-Previous-Credit`
![pic8](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/classification/pics/8_boost_vars.png)

### 3.2 Overall Accuracy & Confusion Matrix
![pic9](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/classification/pics/9_model_compare.PNG)

By the comparison of the 4 prediction techniques, the Forest Model shows the highest overall accuracy.

Both Forest Model and Boosted are good at predicting creditworthy with the accuracy of 0.9619, while Logistic Regression and Decision Tree are about 0.10 less accurate. The four techniques perform not very well in predicting Non-creditworthy. The corresponding accuracy rates are all below 0.50. Logistic Regression performs best and Boosted Model has lowest accuracy in predicting Non-creditworthy. All the four models have bias towards correctly predicting creditworthy individuals because their accuracy in this segment is higher than correctly predicting non-creditworthy individuals, as we can see in below table:
![pic10](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/classification/pics/10_model_compare.png)

The ROC graph shows Logistic Regression, Forest Model and Boosted Model have similar ROC curve. By checking the AUC, it is found that Boosted Model has highest AUC while Decision Tree has lowest one. AUC of Forest Model and Logistic Regression are close but a little bit lower than Boosted Model.
![pic11](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/classification/pics/11_roc.PNG)

## Step 4: Conclusion
**1. Which model did you choose to use? Please justify your decision using **all** of the following techniques. Please only use these techniques to justify your decision:**

- Overall Accuracy against your Validation set
- Accuracies within “Creditworthy” and “Non-Creditworthy” segments
- ROC graph
- Bias in the Confusion Matrices

Forest Model will be chosen to predict the customers qualified for a loan based on overall model performance. Firstly, as mentioned in Step 3, Forest Model has highest overall accuracy against validation set, especially in predicting “Creditworthy”, with 0.9619 accuracy within “Creditworthy” and 0.4222 accuracy within “Non-creditworthy”, indicating a bias in predicting “Creditworthy” much better than “Non-creditworthy”. Moreover, it has the second highest AUC among the four models.

**2. How many individuals are creditworthy?**

Based on Forest Model, it predicts that 408 individuals are creditworthy among the 500 loan applicants.
![pic12](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/classification/pics/12_result.png)
