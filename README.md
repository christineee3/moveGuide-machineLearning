# moveGuide.Machine-Learning

Data gathering, Pre processing, Model Building for moveGuide FYP @ QMUL

Datasets used:

Postcode > LSOA >LAD > Region mappings:
ONS Directory https://geoportal.statistics.gov.uk/datasets/c76505c063a94c7482cd2de996856e2d/about

School Statistics:
Ofsted Reports https://www.gov.uk/government/statistics/state-funded-schools-inspections-and-outcomes-as-at-31-august-2024/main-findings-state-funded-schools-inspections-and-outcomes-as-at-31-august-2024

Accessibilty Indicators:
https://github.com/urbanbigdatacentre/access_uk_open

Zoopla property listings (UBDC):
https://researchdata.gla.ac.uk/1913/

Key steps taken in Data Pre processing:

1. Data Cleaning:
Filter for properties in England from a reduced list of property types
Handling Missing Values: Impute number of bedrooms, bathrooms, receptions with mode of the property type
Filling in price_last column with price_first if null
Removing Outliers: Use percentiles to remove outliers 
Correcting Inconsistencies: Rename features to match columns from Gen 1 and Gen 2 datasets
Mapped Gen 2's LSOA, Gen 1's TTWA to common LAD22CD column

2. Data Transformation:
Encoding Categorical Data:
One-hot encoding the Property type column
Target encoded with smoothing on Postcode and LAD columns
Converting date to numeric Unix timestamp
Merged accessibility data (30-min travel time) using LSOA codes.
Log transform  columns to reduce skew
Data Reduction:
Group property types to reduce the number of categories

3. Feature Engineering:
Creating New Features:
Extracting amenities from the Bullet list column (one-hot encoded) 

4. Data Integration: 
Merged accessibility to local amenities data (30-min travel time) using LSOA code

EDA - Plotted histograms, box plots, graphs to view the distribution, skew of data, and correlation analysis

House Price Prediction

Machine learning models:
Linear regression, Support Vector Regression (SVR),
Random Forests,
Xgboost

Splitted the dataset into training and testing sets

Normalising the columns

Used Random Search cross validations function from sckit-learn to test parameter space for linear regression (including Ridge and Lasso), SVR, random forests

Xgboost model was also trained using random search of set parameters, making use of learning rate, and parallelisation with GPUs.

Evaluation

Train RMSE,          Test RMSE,          R^2
                    
Linear regression:		0.23467,  	    0.23478,  	 0.8690

XGboost:		        0.15960,  	    0.17343,  	 0.928521

1. Performance of Ridge and Lasso regularisation was almost identical to linear regression and optimal regularisation strengths selected through cross-validation are quite small. 
Suggests the features in the dataset not having high multicollinearity, and all features are relevant to the prediction, however this could also be a result of the models being too simple and lacking the complexity in capturing patterns and relationships between features. Small differences in Test and Training set RMSE indicates minimal overfitting.

2. Best model - Xgboost, The optimal parameter set was:
 •	max_depth = 19 •	n_estimators = 600•	min_child_weight = 14•	colsample_bytree = 1•	subsample = 0.6•	reg_alpha = 0.168•	reg_lambda = 0.219•	learning_rate = 0.046 

  These settings resulted in deep ensemble of 600 trees with max depth of 19,more capable of capturing complex underlying patterns between features. The use of a learning rate improved generalisation, subsample of 60% training data used in each round introduced randomness, along with regularisation parameters prevented overfitting.




