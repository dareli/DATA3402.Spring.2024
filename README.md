# **DATA 3402 - Tabular Kaggle Project : Prediction Chirrosis Outcome**

## **Overview** 
In this kaggle project, the challenge was to make a multi-class approach to predict the the outcomes of patients with cirrhosis. Approaches utilized for the challenge includes Random Forest and XGBoost. 

## **About the Dataset** 
Link to Dataset : [Multi-Class Prediction of Cirrhosis Outcomes](https://www.kaggle.com/competitions/playground-series-s3e26/overview)
- Tabular
- Came with a train.csv and a test.csv
- Evaluated using the multi-class logarithmic loss, where each ID in the test set is assigned a single true class label named "Status."
- The submission containing predicted probabilities for 3 potential outcomes: Status_C, Status_CL, and Status_D for each ID.

## **Outline of Work Done** 
### Data Loading & Initial Look

#### First looking at the Training set
- Size : (7905, 20)
- Columns are numerical and categorical
  - Drug column had sepcified names
  - Sex column categorized as M/F
  - Columns like Ascites, Hepatomegaly, Spiders, & Edema arr also categorized into N/Y
  
- No missing values
- No duplicate rows

- Target column (Status) is encoded
  - C = alive at N_Days
  - D = deceased after N_Days
  - CL = alive at N_Days due to liver a transplant

- Outliers
  - Implemented IQR to find outliers of the numerical columns
  - Also added a lower and upper bounds
  - Over half of the numerical columns had outliers


For example: Alk_Phos had the most outliers (792)

![Alk_Phos_Outlier](https://github.com/dareli/DATA3402.Spring.2024/assets/123596270/620b788c-bc4e-4765-bea5-d50922e4ed69)

 #### Quick look at the Test Set
 - Size : (7905, 19)
   - The target column ('Status') is dropped
    
- Had similar results to the training set looking into the test csv

### Data Visualization 
- Looked at distributions of numerical columns
  - Over half of the numerical columns had a right skew
![distsBEFORE](https://github.com/dareli/DATA3402.Spring.2024/assets/123596270/d3255cb4-749d-437f-baeb-cd24839ea7dc)

- Created KDE histograms for every feature between the target classes (D/C/CL)
  - Noticed that Age was in days and not in years
![kde_hists](https://github.com/dareli/DATA3402.Spring.2024/assets/123596270/a264775c-d030-4031-85ea-eb11acaff12e)

 

### Data Cleaning and Preperation for Machine Learning
- Since Age was set in days I converted it to years
  
- To handle the numerical columns having the right skew
  - I did log transformation to have a better scale since this technique compress the range of data.

Distributions after cleaning & transformation

### Machine Learning 
- Training :
  - I dropped the 'id' column
  - Split the training set using train_test_split
  - Used LabelEncoder to encode the categorical columns
  - Also used LabelEncoder to encode target column ('Status') 

- Algorithms chosen :
  - Random Forest : The outcome & time is considered until a status is met.
  - XG Boost : Can perform classification and regression tasks.
    - Since the "Status" is categorical in can also take into factor the amount of time ('N_Days') for predictions.

- Model Evaluations : 
  - Metrics included 
    - Logarithmic Loss 
    - Classification report (Precision, Recall, F1)
    <img width="443" alt="RFM" src="https://github.com/dareli/DATA3402.Spring.2024/assets/123596270/04d2f411-231b-48d6-9252-0cd8b20c71ad">
    <img width="443" alt="XGBM" src="https://github.com/dareli/DATA3402.Spring.2024/assets/123596270/75712e6a-176e-447b-8d0e-de07cf533730">

## **Conclusion:**
  - XGBoost appears to be the better model overall in metric comparison
  - Has a lower log loss, implies more confidence and accuracy in probability predictions
  - Recall and macro average F1-score, XGBoost performs slightly better

## **Future Work** 
- Apply hyper parameter tuning to improve model accuracy
  - Considerg Bayesian optimization as it finds the point that achieves the maximal result.
  - Makes predictions on validation data & calculating validation metrics
  - Faster than GridSreach
- Create more visuals for better analysis

## **Overview of Files in Repository**
- cirrhosis_project.csv : notebook for all data understanding, preprocessing, transformation, training, models and model evaluations
- cirrhosis_submission.csv : submission file for containing predicted probabilities for 3 potential outcomes and id
- cirrhosis_train : provided model training file
- cirrhosis_test : provided model test file

## **Software Setup**
- Python3.11 through Jupyter Notebook

- Packages used :
  - NumPy
  - Pandas
  - Matplotlib
  - Seaborn
  - SciPy
  - scikit-learn
  - XGBoost

## **Citations**
[1] Kaggle. "Playground Series S3E26 - Data." Accessed on May 3, 2024. Available at: https://www.kaggle.com/competitions/playground-series-s3e26/data

[2] Run.AI Guides. "Bayesian Hyperparameter Optimization." Accessed on May 3, 2024. Available at: https://www.run.ai/guides/hyperparameter-tuning/bayesian-hyperparameter-optimization

[3] scikit-learn. "Gradient Boosting Classifier — scikit-learn 0.24.1 documentation." Accessed on May 3, 2024. Available at: https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html

[4] scikit-learn. "Logistic Regression — scikit-learn 0.24.1 documentation." Accessed on May 3, 2024. Available at: https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html
