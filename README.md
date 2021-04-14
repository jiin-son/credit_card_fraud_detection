# credit_card_fraud_detection

### In this project, I'm building a fraud model using data from Kaggle. I'm currently reviewing fraud models at my job and I wanted to build one by my own. My goal is to update github everyday with my progress. I hope this project will lead me to further growth in my career. 

### the data was retrived from: https://www.kaggle.com/mlg-ulb/creditcardfraud

***
### variables:
- V1-V28: transfomred by PCA
- Time: seconds elapsed between that particular transaction and the first transaction in the data
- Amount: transaction amount
- Class: 1-fraudulent transaction, 0-genuine transaction

***
### from EDA Analysis:
Variable V17, V14, V12, V10, V16 show high correlation value with the 'Class' (target) variable. Also, KDE plots shows the distinct shape between 0 and 1 for these variables. 

***
### Feature Selection Analysis:
