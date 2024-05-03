# **DATA 3402 - Tabular Kaggle Project : Prediction Chirrosis Outcome**

## **Overview** 
In this kaggle project, the challenge was to make a multi-class approach to predict the the outcomes of patients with cirrhosis. 

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
  - D = alive at N_Days
  - C = deceased after N_Days
  - CL = alive at N_Days due to liver a transplant
- Outliers
  - Implemented IQR to find outliers of the numerical columns
  - Also added a lower and upper bounds
  - Over half of the numerical columns had outliers

 #### Quick look at the Test Set
 - Size : (7905, 19)
   - The target column ('Status') is dropped
- Had similar results to the training set looking into the test csv

### Data Visualization 
- Looked at distributions of numerical columns
  - Over half of the numerical columns had a right skew
- Created histograms for every feature between the target classes (D/C/CL)
  - Noticed that Age was in days and not in years
 

### Data Cleaning and Preperation for Machine Learning
- Since Age was set in days I converted it to years
- To handle the numerical columns having the right skew
  - I did log transformation to have a better scale since this technique compress the range of data. 

### Machine Learning 
- Before doing any further machine learning techniques
  - I dropped the 'id' column
  - Split the training set using train_test_split
  - Used LabelEncoder to encode the categorical columns
- Algorithms chosen :
  - Random Forest : The outcome & time is considered until a status is met.
  - XG Boost : Can perform classification and regression tasks.
    - Since the "Status" is categorical in can also take into factor the amount of time ('N_Days') for predictions.
- Model Evaluation
  - Metrics included 
    - Logarithmic Loss 
    - Classification report (Precision, Recall, F1)
  - When compared :
    - XGBoost appears to be the better model overall in metric comparison
      - Has a lower log loss, implies more confidence and accuracy in probability predictions
      - Recall and macro average F1-score, XGBoost performs slightly better
