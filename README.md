# credit_card_fraud_detection

In this project, I built a model to detect the credit card fraudulent transaction using data from Kaggle. I have been reviewing fraud models at my job and I wanted to build one by my own. I updated github with my progress. I hope this project will lead me to further growth in my career. 

The data used in this project was retrived from: https://www.kaggle.com/mlg-ulb/creditcardfraud

See my medium post (link to be here) for more detail description on the model building process. 

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
In this file, I built the models using some modeling techniques (i.e. splitting train/test dataset, standardization, oversapling, feature selection).

Here are the steps I took to build the model: 
1. **Spliting Train/Test dataset**: Splitting train/test the original data set to prepare feature selection process. The reason why I split the data before the feature selection is not to have any access in any test set during the model fitting process. In this process, train_test_split was used from sklearn.model_selection. See https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html for more details.

2. **Transformation/standardization on "Amount" and "Time" variable only**: Other variables are already provided with standardized format from the PCA process (initial data processing). In this process, I used **StandardScaler** from sklearn.preprocessing. This standardize feature removes the mean and scales to unit variance. See https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html for more details.

3. **Oversampling**: The data used in this project is the imbalanced data. To reduce the classification error for fraudulent transactions, I oversampled the data. The reason why I oversamped before the feature selection is because most variable selection methods assume that the samples are independent. To oversample the fraud transactions, I used **SMOTE** from imblearn.over_sampling. The SMOTE function oversamples the rare event by using bootstrapping and k-nearest neighbor to synthetically create additional observations of that event. See https://imbalanced-learn.org/stable/over_sampling.html for more details.

4. **Feature Selection**: To select the most important variables, I used **SelectFromModel** from sklearn.feature_selection. SelectFromModel is a Meta-transformer for selecting features based on importance weights. See https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectFromModel.html for more details.

    Selected variables: ['V2', 'V3', 'V4', 'V10', 'V11', 'V12', 'V14', 'V16', 'V17']

5. **Spliting Train/Test dataset only with the selected features**: In this process, I used the same random_state (12) parameters in the function to keep the train/test data same with the previous steps.  

6. Skipping transformation/standardization process: no "Amount" or "Time" variable were selected from the Feature Selection process. 

7. **Oversampling**: Oversampled the new train data from Step 5. 

8. **Train/fit the model **: Binary Classification
Here is the list of models I built and compared:
- Logistic Regression: Used **LogisticRegression** from sklearn.linear_model. - simple, highly interpretable \
https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html
- Random Forest: Used **RandomForestClassifier** from sklearn.ensemble. - tree-based model (ensamble model), extendibility (can be used in both classification and regression problem), good prediction power (usually used in banking), feature importance analysis (ranks relative importance of each variable\
https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html
- XGBoost: Used **GradientBoostingClassifier** from sklearn.ensemble. - tree-based model, extendivility, high flexibility (custom objective function and custom evaluation metrics), feature importance analysis\
https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html

9. **Evaluating the metrics**
- F1 score: combination of the precision and recall of the model (harmonic mean of the model's precision and recall). I used F1 score as a main metric to select the best model. 
- Precision (Sensitivity): what proportion of positive identifications (predicted as fraud) was actually correct? 
- Recall: what proportion of actual positives (fraud) was identified correctly?
- AUROC (Area under the receiver underlying characteristic): measures the discriminatory power
<table style="width:100%">
  <tr>
    <th>Metrics</th>
    <th>RandomForest</th>
    <th>LogisticRegression</th>
    <th>XGBoost</th>
  </tr>
  <tr>
    <td>F1 score</td>
    <td>78.57%</td>
    <td>10.81%</td>
    <td>15.40%</td>
  </tr>
  <tr>
    <td>Precision</td>
    <td>77.34%</td>
    <td>5.76%</td>
    <td>8.44%</td>
  </tr>
  <tr>
    <td>Recall</td>
    <td>79.84%</td>
    <td>88.71%</td>
    <td>87.90%</td>
  </tr>
  <tr>
    <td>AUROC</td>
    <td>89.90%</td>
    <td>93.09%</td>
    <td>93.12%</td>
  </tr>
</table>

***
### Conclusion
I used feature selection process is to 1) reduce the runtime, 2) improve the model performance, 3) reduce the overfitting, and 4) increase interpretability. To see if the model overfits, I used different random_state parameters. Using different random_state did not impact the metrics; thus, no concern on the overfitting issue. Interpretability can be improved if the data has actual variables (data limitation). Although the models with feature selection process didn't improve the model importance much (I also evaluated each model with all the variables but it didn't help much to increase F1 score), the run time was significantly reduced. 

Thus, the best model is the random forest model with F1 score 78.57%, Precision 77.34%, Recall 79.84%, and AUROC 89.90%. 

***
### Further Consideration
- using different parameters in the model
- considering more advanced models: CNN, RNN, SVM, Reinforcement Learning, Random Forest/GBM, ensemble
- considering different feature selection methods
- using dataset with raw variables for interpretability
