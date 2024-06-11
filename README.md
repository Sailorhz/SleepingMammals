# SleepingMammals
this project embarks on a fascinating exploration of sleep patterns across mammalian species. The dataset at the heart of our analysis provides a wealth of information, inviting us to uncover insights into the following questions and more:

Size Matters?
Does the size of a mammal correlate with the quality of its sleep?
Predator vs. Prey?
Do predators experience sleep differently than their prey?
Dreaming Duration?
Who indulges in longer dreams, and what factors contribute to dream duration?

Our project is poised to decipher these mysteries by leveraging the rich dataset at our disposal. Join us on this intellectual journey!

Preprocessing

Libraries deployed

-	Pandas: For data manipulation and analysis.
-	Numpy: For numerical computing and handling NaN values.
-	Sklearn.preprocessing: For data preprocessing tasks like encoding and scaling.
-	Sklearn.model_selection: For model selection, evaluation, and data splitting.
-	Sklearn.linear_model, sklearn.ensemble, sklearn.tree: For various machine learning models.
-	Sklearn.metrics: For evaluating model performance.
-	Plotly and Seaborn: For interactive data visualization.
-	Warnings: For suppressing specific warning messages.
-	IPython.display: For displaying outputs.
-	Bcolors: For formatting console text.

This comprehensive set of libraries and modules equips the project with tools for data preprocessing, model building, evaluation, and visualization, facilitating robust analysis and interpretation of results. 

Data preprocessing

Approach:
-	Replacing 0 values of BrainWt by Nan's 
-	Fillna Dreaming & NonDreaming by the family/order mean values
-	Computing Family, Order, Vore means of "Predation, Exposure, Danger"
-	Fillna Predation, Exposure, Danger by the family/order/vore mean values
-	conservation feature
-	BodyWt kg to g
-	LifeSpan years to days
-	Round/Ceil values
-	Summary of variables processing
-	Removing columns for autoML
-	Dataset Summary
-	Drop lines containing outliers (using masks)
-	Pipeline, ColumnTransformer

Result: 
          autoML was used to test the cleaned dataset. Its performance is highly evaluated by autoML. And it is well-prepared for EDA and training later. https://automl.runmercury.com/, https://mljar.com/automl/

Exploratory data analysis

Approach:

-	Exploration of Feature Distributions
-	Pairing Sleep Hours and Dreaming Time with Key Features to uncover potential relationships among them.
-	Correlation Analysis using Linear Regression
-	Utilization of Interactive Charts for Data Exploration and Presentation

Result: 

Based on our chart analysis, several insights have emerged:
-	Body Size and Sleep Quality: Our charts indicate that body size alone does not have a direct impact on sleep quality.
-	Predator Sleep Patterns: While some hints suggest that predators may experience better sleep quality, our analysis does not conclusively demonstrate that predators have longer deep sleep time compared to other species.
-	Species with Longer Dreaming Time: two species stand out for their notably longer dreaming times compared to others:

-	Water Opossum: 6.6 hours
-	Thick-tail Opossum: 6.6 hours

Based on our correlation analysis, we have observed the following relationships:
-	Total Sleep Hours and Body/Brain Weight: There exists a moderate negative correlation between total sleep hours and body/brain weight. Additionally, body and brain weight exhibit even weaker correlations with sleep amount.
-	Danger, Exposure, and Life Span: These factors display moderate negative correlations with both sleep amount and dreaming time.
-	Predation: While predation demonstrates a weak correlation with both sleep amount and dreaming time, the correlation with dreaming time is slightly higher.
-	Conservation: Conservation shows the weakest correlation with total sleep and dreaming time among all the features analyzed.

Machine Learning Models
Approach:  

Initially, our focus was on multi-output models due to the correlation between dreaming and total sleep. However, Mr. BECAVIN suggested prioritizing the prediction of total sleep before dreaming. So that is how and what we choose:

-	Selecting a model that naturally handles missing values encoded as NaN is pivotal for efficient data processing and model training. 
-	For supervised learning tasks, consider using sklearn.ensemble.HistGradientBoostingClassifier and sklearn.ensemble.HistGradientBoostingRegressor from Scikit-learn. These models handle missing values encoded as NaNs without needing explicit preprocessing, making them suitable for datasets with incomplete information.

-	Ensemble methods combine the predictions of several base estimators built with a given learning algorithm to improve generalizability / robustness over a single estimator.

-	Linear regression 
-	Decision Tree Regressor – gridSearch
-	Gradient Boosting Regressor - gridSearch / RandomizedSearch
-	RandomForest – gridSearch
And, While retaining outliers in the lifespan data, the model exhibits a higher R2 score of 0.692 compared to 0.642 without outliers. This suggests that the model with outliers better captures the variance in the data. However, it comes at the cost of increased mean absolute error (MAE) and mean squared error (MSE), which measure the average discrepancy between predicted and actual values.

with outliers : 
-	R2 score :  0.692    MAE:  2.347    MSE: 8.934
without :
-	R2 score: 0.642      MAE: 1.794     MSE: 4.634

Result:


	P R E D I C T I O N	MSE : 	Mean Squared ERROR
	TotalSleep	Dreaming	MAE : 	Mean Absolute ERROR
SCORE \ MODEL	LR	RF	RF Gs	GB	LR	RF	GB	LR : 	Linear Regression 
R2	0.611	0.692	0.643	0.671	0.691	0.831	0.853	RF : 	Random Forest Regressor
MAE	2.79	2.347	0.851	2.22	0.814	0.524	0.485	RF Gs : 	Random Forest Regressor – GridSearchCV
MSE	11.3	8.934	1.165	9.55	1.009	0.552	0.478	GB :	Gradient Boosting


In the end, Joblib library and pickle library are applied to persist the models. 
![image](https://github.com/Sailorhz/SleepingMammals/assets/129844601/d1719112-f4ea-4ac5-8a47-b015f8d422e2)

