# credit_card_fraud_detection

### In this project, I'm building a fraud model using data from Kaggle. I'm currently reviewing fraud models at my job and I wanted to build one by my own. My goal is to update github everyday with my progress. I hope this project will lead me to further growth in my career. 

### The data used in this project was retrived from: https://www.kaggle.com/mlg-ulb/creditcardfraud

***
### Variables:
- V1-V28: transfomred by PCA
- Time: seconds elapsed between that particular transaction and the first transaction in the data
- Amount: transaction amount
- Class: 1-fraudulent transaction, 0-genuine transaction

***
### from Explanatory Data Analysis (EDA):
Before modeling process, I briefly explored data using correlation heatmap and KDE plots to understand the data. 

Some findings from EDA Analysis:
- Variable V17, V14, V12, V10, V16 show high correlation value with the 'Class' (target) variable. Also, KDE plots show the distinct shapes for 0s and 1s for these variables.
- The target variable (Class) is highly imbalanced, where geneuine transaction is 284315 and the fraudulent transaction is 492. Classification were designed around the assumption of an equal number of examples for each class. This results in models that have poor predictive performance, specifically minority class (fraudulent transaction, in this case). For this kind of fraud problems, we are looking for predictive power especially for minority class (fraud cases). To reduce the classification error for minority class, **oversampling** technique will be used during the modeling process. 

***
### from Feature Selection Analysis: 
To select the most important variables, I used **SelectFromModel** from sklearn.feature_selection. SelectFromModel is a Meta-transformer for selecting features based on importance weights. See https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectFromModel.html for more details.

Here is the steps I took to build the model: 
1. **Spliting Train/Test dataset**: Splitting train/test data with all variables for feature selection process. The reason why we split the data before the feature selection is not to have any access in any test set during the model fitting process. 
2. **Transformation/standardization on "Amount" and "Time" variable only**: Other variables are already provided with standardized format from the PCA process (initial data processing). In this process, I used StandardScaler from sklearn.preprocessing. https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html
3. **Oversampling**: The data provided is the imbalanced data and to reduce the classification error for fraudulent transactions, we oversample the data. The reason why we are oversampling before the feature selection is because most variable selection methods assume that the samples are independent. To oversample the fraud transactions, I used **SMOTE** from imblearn.over_sampling.
4. **Feature Selection**: To select the most important variables, I used **SelectFromModel** from sklearn.feature_selection. SelectFromModel is a Meta-transformer for selecting features based on importance weights. See https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectFromModel.html for more details.\ Selected variables: ['V2', 'V3', 'V4', 'V10', 'V11', 'V12', 'V14', 'V16', 'V17']
5. **Spliting Tran/Test dataset only with the selected features**
6. Skipping transformation/standardization process: no "Amount" or "Time" variable were selected from the Feature Selection process
7. **Oversampling**
8. **Train/fit the model**
9. evalate the metrics

***
### from Feature Selection Analysis:
- Random Forest: how to decide how many variables should be selected? -> evaluated metrics and compared with the model with all the variables (rf with top 5 variables were slightly performed better (comparison result is saved in the excel file) -> how to generate roc_curve? 

### oversampling due to unbalanced dataset 
- SMOTE: the precision score is not improving
- standardized "Amount" and "Time" variable before oversampling and had the best F1 value  
- caution: use random foreset classifier not the regressor

### Best metrics
- rfc with oversampling - standardization - top9 variable (F1 = 79.67%) -> how do you know if this is overfitting
- rfc with oversampling - standardization - all variables (F1 = 82.91%)
- logistic regression is not performing well
- standardization is necessary for "Amount" and "Time" variable
- using all variables doesn't improve F1 score too much
- How to select the most valuable variables? -> Key: random forest feature selection 
- CNN, RNN, SVM, Reinforcement Learning, Random Forest/GBM, ensemble
