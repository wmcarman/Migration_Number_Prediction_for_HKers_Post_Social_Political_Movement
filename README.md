# Migration Number Prediction for HKers Post Social Political Movement

## This project aims to look at the number of people emigrated from Hong Kong for the past 2 years and to predict the number of people that will be leaving Hong Kong in the coming months. 

### Warning ahead: This has not been a successful time series prediction due to many reasons and has been shared as a lesson learnt for myself. 

### Data Collection
Forum discussion data collected from LIHKG and Baby-Kingdom as plenty of threads are discussing different ways to migrate, details of application and life after migration. Data collected from January 2020 to June 2022. 

Statistics on passenger traffic collected from Hong Kong Immigration Department plays another important role in this project when it comes to time series analysis. We have only kept data for HongKongers leaving Hong Kong through Hong Kong International Airport as this can better represent people leaving for migration purposes. 

### Data Pre-Processing - Forum Data
After loading all data from json to dataframe into Python notebook, a function has been used to filter forum posts according to a group of pre-set keywords. These are some common keywords that appear when users are discussing leaving Hong Kong and about life overseas. 

Filtered rows are then run through NLP analysis using Stanza NLP by StanfordNLP to capture all the location and time mentioned. Below is an example of sentence broken down into GPE (Location) and Date (Time)

All these inputs are then grouped and sorted according to the location. Top 20 locations are exacted into a new table for further analysis with airport data. 

Side Note: Along the way, we have tried to apply spaCy for NLP pre-processing and found that they both have similar performance on GPE but Stanza works slightly better for time. 

### Data Pre-Processing - Airport Data
Minimal effort done on airport data to correct duplicate rows, N/A rows (Where there isn’t any Hong Kong residents leaving via Hong Kong International Airport) and lastly matching date format for further handling later on. 

### Data Visualization
Before we kick start any detailed analysis, we have utilized seaborn to get a glimpse of how the data actually looks like and also if there are any visible trends by human eyes. 

Graphic shows that HongKongers leaving Hong Kong peaks around Q1 2022, does it show the same in forum discussion? 

United Kingdom: It shows that there is really a trend with potential window sliding of 5-6 weeks.

Canada: An early indication of spike, however trend after spike could not be followed.

### Data Processing for XGBoost and SKForecaster
We first dropped all the unnecessary columns before pivoting the table as per the requirement of XGBoost. 

Defining X and Y values fitting into the models manually as for this project, there are many features to be included. Lastly separating dataset into training dataset and testing dataset.

Side Note: More detailed coding work could be found on Github to avoid clumsiness of this passage. 

### Data Analysis
For this project, we have utilized both XGBoost and SKForecaster for data analytics related work. XGBoost decision tree algorithm is chosen as it is a powerful tool to make predictions based on multiple features and for every new model built, it has a technique to correct errors made by the previous model. 

R-squared score closer to 1 indicates that X and Y have got strong positive correlation - which is something we are looking forward to in this project. However, it seems that with all the variables we have provided to the model, it has shown a negative correlation between X and Y. 

Direct Multi-step Forecaster allows time series prediction to be done based on its past behavior together with other external variables (In this case, forum discussion rate). Besides providing an actual prediction, SKForecaster has a function to provide a list of features and its importance. For this instance, lag 7 and lag 14 data have had significant impacts to the result of the model. While looking at NER features, Toronto seems to have the highest impact among others. 

Grid search forecaster is another technique used in this project and comparatively, has a better performance. It is actually a tuning method to compute optimum values of hyperparameters. In short, we list out all the possible values for the required parameters and grid search will attempt all combinations in return for an optimal combination of metrics. 

### Result
All in all, the prediction for this project has not been useful to an extent that the number predicted is far from the actual number leaving Hong Kong via Hong Kong International Airport (Assuming that airplane has been the only approach to go to oversea). 

However, it has been a great experience looking into the difference between recursive multi-step forecasting and direct multi-step forecasting in training time series models. Given that the number fluctuates greatly and many different affecting features, the newly predicted T-1 data can impact T value in the wrong direction. Direct multi-step forecasting on the other hand, creates multiple models independent of each other which is more applicable to this project. 

### Challenges Encountered 
There are several challenges encountered and some of them we have foreseen before starting the project however, not aware of the actual impact it has on the final result. 

The limited amount of data collected could potentially be representing only a very minimal group in Hong Kong. Upon investigation, there are Telegram groups, WhatsApp groups, Facebook pages or groups with heated discussion on such topics. Hence, more data should be collected before continuing with the project. 

Secondly, the filtering of discussion topics has resulted in some misleading posts such as surrogate shopping in the mentioned location or enquiries on how to migrate to the place without meeting any of the legal requirements. A stricker filtering rule should be applied before proceeding with data processing. 

Last but not least, there are many other factors impacting decision making on when and how to move out of Hong Kong. Some of the factors are qualitative which would be very difficult to implement into the model. Examples are quarantine requirement and vaccination requirement at the destination, flight status and pets transportation rules of various aviation companies. 

Disclaimer: This article is simply for sharing purpose and is written to the best of my genuine knowledge and belief. This article serves no purpose for any form of historical education or any political standpoint of any person. 


