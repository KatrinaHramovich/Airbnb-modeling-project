What influences Airbnb prices? Does property description tell us about rent price? Light Gradient Boosting vs. Random Forest.
Katrina Hramovich 
September 24, 2023

Outline
1.	Introduction
2.	What month has the highest rent prices?
3.	What other features can be important for rent price prediction? Feature selection, Random Forest vs. Light Gradient Boosting.
4.	What performs better Random Forest or Light Gradient Boosting?
5.	Can property name and description tell us rental price? Mixed model.
6. Conclusions
Introduction
Airbnb day prices in Seattle are predicted using feature engineering, random forest and light gradient boosting feature selection and regression. These models aim to help hosts establish appropriate rental prices and to find out what features can influence rental price the most. Text features such as name and description are also considered.
Here we also compare random forest and light gradient boosting performance. Light Gradient Boosting (LGB) is relatively fast algorithm comparing with other boosting methods. In the final test the sum of two LGB models was applied to reduce overfitting. Final best R-squared score on test is 0.74
Data are downloaded from kaggle, Seattle Airbnb open data.
Notebook with code is at github.
What month has the highest rental prices?
First, we check the dependence between prices and time on monthly bases. The time series data are in file calendar.csv. In figure below the dependence of mean price over all listings on time over 12 months is shown.
 
Mean price dependence on time
In Seattle the highest rental prices are in the summer, especially in June. The price change can be noticeable during the year. The month is added to other features merging simplified table from calendar.csv with data from listings_seattle.csv. However, first data were split into train and test to have different ids in test and train set. On train data score is shown on 5-fold cross validation.
What other features can be important for rental price prediction? Random forest vs Light Gradient Boosting
In figures below the feature importance in Random Forest and Light Gradient Boosting is shown after removing weakest features that improved cross validation test result, see notebook for details.
As we know too many features can result to overfitting or some can be even irrelevant. Some highly correlated features are also removed to have best score.
As we see from figure for Random Forest (RF) feature importance number of total rooms (it includes bathrooms and bedrooms) has the largest score has number of total rooms. The importance score of other features drops fast.
 
For Light Gradient Boosting the most important feature is latitude. However, the importance of other features decreases slowly.
 
One of most optimal models after features selections is shown for the same number of features in both models. In Light Gradient Boosting all features contribute more homogeneously to the model.
The most important features in both models are total number of rooms, month, latitude, longitude, accommodations (total number of people to accommodate), room type, reviews per month, host total listening counts and street.
What performs better Random Forest or Light Gradient Boosting?
Results for best numerical and categorical features together:
Random forest: R-squared cross validation: 0.63, test: 0.68
LGB: cross validation: 0.69, test: 0.71
We see that light gradient boosting gives best scores. From what we see above the fact that for Light Gradient Boosting all features contribute homogeneously must have a good influence on result.
Can property name and description text tell us about rent price? Mixed model.
As text features, name, description, amenities, transit, summary are used after Tfidf vectorizer transformation. In this case the Random Forest was very slow, therefore only LGB regression was applied. First LGB is applied to these text features only. R2 error for this model is about 0.3. To add these features to other that we discussed above makes the result worse because model starts to overfit.
The way to use extra features is to use superposition of two models with less number of features, the model, described in previous section and the model with text features only. The predicted values are:
y_pred = a1*y_pred1+a2*y_pred2,
y_pred1 is value predicted in previous LGB regression model, y_pred2 is predicted in LGB model with text features only, a1+a2=1. For described models a1=0.8, a2=0.2
As the result we got:
Cross validation R-squared score on text features: 0.28Test score: 0.3Superposition of models on test: 0.74
In this model text features contribute to the test score.
Conclusions
The Airbnb rent prices noticeably depend on month of the year. The highest prices are in the summer.
The other features that are influence prices the most are total number of rooms, month, latitude, longitude, accommodates (total number of people to accommodate), room type, reviews per month, host total listening counts, street.
The light gradient boosting model has better prediction score than Random Forest for our most optimal feature selection. In the Light Gradient Boosting Regression features importance weights are distributed more homogeneously.
Property description can also give information about price.
Combining two models with a smaller number of features can improve resulting test score.


![image](https://github.com/KatrinaHramovich/Airbnb-modeling-project/assets/71725731/ccc067eb-428b-4724-88fb-1b22ace2b849)
