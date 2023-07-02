#  Model Card

### Basic Information

* **Person or organization developing model**: Kylie Loudermilk, `kylieloudermilk@gwmail.gwu.edu`
* **Model date**: June, 2023
* **Model version**: 3.0

### Intended Use
* **Primary intended uses**: This model is an *example* use case for determining customers who will receive high priced mortgage based on Home Mortgage Disclosure Act (HMDA) data.
* **Primary intended users**: Students in GWU DNSC 6290 Responsible Machine Learning.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.

### Training Data

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
| **ROW_ID**| ID | int | unique row identifier |
|**HIGH PRICED**| binary target | int | whether (1) or not (0) the annual percentage rate (APR) charged for a mortgage is 150 basis points (1.5%) or more above a survey-based estimate of similar mortgages. |
| **CONFORMING** | input | int | 1 = conforms to normal mortgage standards; 0 = loan is different
| **DEBT TO INCOME RATIO STD** | input | float | standardized debt-to-income ratio for mortgage applicants |
| **DEBT TO INCOME RATIO MISSING** | input | int | 1 = missing marker for debt to income ratio std |
| **INCOME STD** | input | float | standardized income for mortgage applicants |
| **LOAN AMOUNT STD** | input | float | standardized amount of the mortgage for applicants |
| **INTRO RATE PERIOD STD** | input | float | standardized introductory rate period for mortgage applicants |
| **LOAN TO VALUE RATIO STD** | input | float | ratio of the mortgage size to the value of the property for mortgage applicants |
| **NO INTRO RATE PERIOD STD** | input | int | whether or not a mortgage does not include an introductory rate period |
| **PROPERTY VALUE STD**| input | float | value of the mortgaged property |
| **TERM 360**| input | int | whether the mortgage is a standard 360 month mortgage (1) or a different type of mortgage (0) |
| **BLACK**| demographic information | int | whether the applicant identified themselves as black (1) or not (0) |
| **ASIAN**| demographic information | int | whether the applicant identified themselves as asian (1) or not (0) |
| **WHITE**| demographic information | int | whether the applicant identified themselves as white (1) or not (0) |
| **AMIND**| demographic information | int | whether the applicant identified themselves as amind (1) or not (0) |
| **HIPAC**| demographic information | int | whether the applicant identified themselves as hipac (1) or not (0) |
| **HISPANIC**| demographic information | int | whether the applicant identified themselves as hispanic (1) or not (0) |
| **NON_HISPANIC**| demographic information | int | whether the applicant identified themselves as non hispanic (1) or not (0) |
| **MALE**| demographic information | int | whether the applicant identified themselves as male (1) or not (0) |
| **FEMALE**| demographic information | int | whether the applicant identified themselves as female (1) or not (0) |
| **AGEGTE62**| demographic information | int | whether the applicant identified themselves as over age 62 (1) or not (0) |
| **AGELT62**| demographic information | int | whether the applicant identified themselves as under age 62 (1) or not (0) |


* **Source of training data**: GWU, email `jphall@gwu.edu` for more information
* **How training data was divided into training and validation data**: Numpy rand function used to split and assign data to two datasets according to a 70/30 train/validation ratio. 70% training, 30% validation
* **Number of rows in training and validation data**:
  * Training rows: 112,253
  * Validation rows: 48,085

### Test Data
* **Source of test data**: GWU, email `jphall@gwu.edu` for more information
* **Number of rows in test data**: 19,830
* **State any differences in columns between training and test data**: None

### Model details
* **Columns used as inputs in the final model**: 'intro_rate_period_std', 'debt_to_income_ratio_missing', 'conforming',
               'no_intro_rate_period_std', 'income_std', 'debt_to_income_ratio_std', 'property_value_std', 'term_360'
       
* **Column(s) used as target(s) in the final model**: 'high_priced'
* **Type of model**: XGB2 Classifier
* **Software used to implement the model**: Python, PiML
* **Version of the modeling software**: PiML V0.5.0
* **Hyperparameters or model**: 
```
XGB2Classifier('n_estimators': 80,
               'max_bin': 20,
               'eta': 0.3,
               'gamma': 0.05,
               'random_state': 12345)
```
### Quantitative Analysis

* **Training AUC**: 0.7799
* **Validation AUC**: 0.7826
* **Test AUC (Fold=0)**: 0.836 


#### Correlation Heatmap
![image](https://github.com/klouderm/DS6290_RMS/assets/83142814/90ba2c7d-3a38-4d03-83af-51c9132f0342)


#### Global Feature Importance
* **From origin version of XGB2 model, pre-rememdiation**
* **Taken from assign_1_kylieLoudermilk.ipynb**
![image](https://github.com/klouderm/DS6290_RMS/assets/83142814/2215feb5-c2ff-4c89-9b6b-f392745948b3)


#### Global Feature Importance
* **From final remediated of XGB2 model**
* **Taken from assign_5_kylieLoudermilk.ipynb**
![image](https://github.com/klouderm/DS6290_RMS/assets/83142814/57dd1a8b-8775-417f-948c-2257ea12b387)


#### Partial Dependence
* **Columns for the final remediated version of XGB2 model**
* **Taken from assign_5_kylieLoudermilk.ipynb**
<img width="832" alt="image" src="https://github.com/klouderm/DS6290_RMS/assets/83142814/365605be-253f-4565-a800-9eabae39a004">


#### AIR vs. AUC for 500 models trained for discrimination remediation
* **Taken from assign_3_kylieLoudermilk.ipynb**
<img width="269" alt="image" src="https://github.com/klouderm/DS6290_RMS/assets/83142814/f1cfcbcd-069d-45a9-9140-c5bf3f9d0825">


#### Variable Importance from Black Box Model
* **Security testing shows a bad actor may be able to discover these variables are important to the model**
* **Taken from assign_4_kylieLoudermilk.ipynb**

#### Ethical Considerations
* **Potential Negative Impacts**
    * Based on history we know that financial data is encoded with racial bias. Even when we test for bias and utilize those results to select an
      iteration of the model that performs at an industry standard level of fair (an AIR of above 0.80), we are accepting that this model will predict that
      black mortgage applicants will be more likely to be recipients of annual percentage rates (APR) charged for a mortgage 150 basis points (1.5%) or
      more above a survey-based estimate of similar mortgages than white applicants. By using predictions that are more likely to suggest black applicants
      as recipients of high priced mortgages, we risk hurting this demographic and perpetuating the cycle of bias.

* **Potential Uncertainties**
    * This model has only been tested in the Google Colab environment, further testing would be needed to determine if the results would be affected
      by a change in development environment or hardware.
