# Capital-Bikeshare

## Introduction
Capital Bikeshare is a bike-sharing program in the Washington D.C. metropolitan area. It allows users to rent a bike from one of the many stations located throughout the city and surrounding suburbs. The program was launched in 2010. It is a popular transportation option for both locals and tourists, as it provides a convenient and affordable way to get around the area, and also solves the matters of parking and traffic. The program has also helped to promote cycling as a healthy and sustainable mode of transportation. 

## Business situation

Capital Bikeshare, with over 5,000 bikes and 600 stations in the Washington metropolitan area, faces a challenge of ensuring that bikes are properly maintained and accessible to users. The number of users can vary daily, making accurate predictions critical to adjust operations in a cost-effective and efficient way. To achieve this, Capital Bikeshare needs to monitor and analyze bike and dock availability at each station and rebalance bikes to prevent over or understocking. For instance, by predicting the number of pickups and drop-offs at stations such as 21st & I St NW and 21st St & Pennsylvania Ave NW, Capital Bikeshare can use the final model's results to determine how many bicycles to deploy to each station. This my team project for the Machine Learning class of Spring 2023 at George Washington University. I was in charge of the machine learning models, evaluation, and interpretation of the prediction models.

## Data collection

The dataset we use is partially from [Capital Bikeshare system data](https://ride.capitalbikeshare.com/system-data), specifically taking the period from January to April 2022. Another part is [weather data](https://www.visualcrossing.com/weather-history/washington,%20dc/us/2022-01-01/2022-12-31) of Washington, D.C in 2022. One factor we would consider is the university’s calendar which might impact the usage of this service.The reason is that two stations experimented here are nearby The George Washington University. Therefore, in our model, we decide to put this data into the overall dataset. Particularly, day off could be no-class days based on the university's calendar (https://www.gwu.edu/academic-calendar) or weekend days (Saturday and Sunday).

## Data exploration
### Visualize pickups and dropoffs during the chosen time period
![image](https://github.com/tuananh28394/Capital-Bikeshare/blob/main/Pickup_%26_Dropoff_count.png)

### Check missing values
Overall, the dataset appears to be of good quality, with minimal missing data records. However, there is a column in the file named "preciptype" contains numerous missing values. As these missing values might have the possibility impact the accuracy of the model and the final outcome of the prediction, we have made the decision to remove it.

### Drop columns
We have decided to drop several columns from the weather data, including "name", "stations", "description", "sunrise", "sunset", "conditions", "severerisk", "preciptype", and "windgust". These columns might not provide enough valuable information for prediction and analysis or not affect the overall model training and testing. By removing these columns, we can reduce the impact of noise on the data, leading to a more accurate final prediction.

## Assumption
In terms of the quality and limitations of the data, we have made some assumptions to guide our analysis.
* There is no seasonal or hourly effect on the number of bike users, but that national holidays and spring breaks may have some impact.
* Temperature and weather conditions could directly affect ridership and bike usage.
* There are no limitations on the number of bikes or docks available at each station.

## Prediction models
Linear Regression, Ridge Regression, LASSO, Elastic Net, and KNN Regressor are commonly used regression models and we use such machine learning models to assess the predictive performance. We split the dataset for each station into training and testing data. We fit each model on the training data and evaluate their performance on the validation or test data using the MSE metric, applying hyperparameter tuning and crossing validation if any. The model with the lowest MSE value on the validation or test set should be chosen as the best model.

We apply all five machine learning algorithms to each station, separately analyzing both pickups and dropoffs. Initially, we run the models without calendar data, and subsequently, we add the calendar data to the dataset. Our analysis shows that the addition of calendar data improves the performance of the models with regard to MSE values. Thus, the role of calendar data must be taken into consideration. Overall, the results for most types of models are improved after adding the calendar data into the original dataset.

After running all algorithms, we organize and compare the MSE for different regression models. This is a summary table with all MSE and respective values.

Model                                | 21st St & I St NW Pickup | 21st St & I St NW Dropoff | 21st St & Pennsylvania Ave NW Pickup | 21st St & Pennsylvania Ave Dropoff
------------------------------------ | ------------------------ | ------------------------- | ------------------------------------ | ----------------------------------
Linear Regression MSE                | 151.697                  | 159.792                   | 44.093                               | 54.633
LASSO MSE                            | 123.692                  | 95.868                    | 30.722                               | 38.135
LASSO Best Alpha for LassoCV         | 1.519911083              | 1.047615753               | 0.236448941                          | 0.869749003
Ridge Regression MSE                 | 130.488                  | 90.072                    | 29.883                               | 41.068
Ridge RBest Alpha for RidgeCV        | 12.91549665              | 15.19911083               | 29.15053063                          | 24.77076356
Elastic Net MSE                      | 129.57                   | 90.072                    | 25.824                               | 41.068
Elastic Net Best Alpha for ElasticCV | 0.245042271              | 0.22326242                | 0.419259475                          | 0.392175961
KNN Regressor MSE                    | 226.997                  | 210.544                   | 33.744                               | 60.203

## Model evaluation

Overall, based on the MSE values of all the models, we have several points to indicate.
* The linear regression model, although simple and easy to interpret, does not perform well in this dataset. Its MSE values are considerably larger compared to the other four machine learning algorithms. Meanwhile, for the 21st & I St NW station, the MSE values reach 44.093 for pickups and 54.633 for drop-offs.

* The KNN regressor model also produces high MSE values, despite attempts to adjust the nearest neighbors values. This may be due to the large distances between observations in the dataset, making it difficult to find an appropriate nearest neighbor value.

* LASSO, Ridge and Elastic Net models are better options. We apply regularization in a balance between model fit and simplicity, making the MSE small enough to consider.  LASSO, in particular, gives the lowest MSE values for both pickup and dropoff cases in both stations. For instance, in the 21st & I St NW station, LASSO's MSE value for pickups is 123.692 and 38.135 for dropoffs in the 21st St & Pennsylvania Ave NW station. This suggests that LASSO selects the most important features, excludes irrelevant factors, and produces a less likely overfitting model. Therefore, we conclude that LASSO is the most suitable choice for this task based on the given data.

## Train final model with LASSO

We train the final LASSO model with the best value of alpha and take R-squared functions to find out how much of the variation in the outcome can be explained by the set of the features.

Station                        |  Pickup          | Dropoff 
------------------------------ | ---------------- | ---------------- 
21st St & I St NW              | 0.593            | 0.681                  
21st St & Pennsylvania Ave NW  | 0.335            | 0.465         

## Interpretation of prediction

With a list of predicted values from the test data, we pick particular days to demonstrate our findings by proving figures:
Station                        |  Pickup/Dropoff  | January 7 (rain/snow, day-off) |  April 27 (nondayoff)     
------------------------------ | ---------------- | ------------------------------ | ---------------------- 
21st St & I St NW              | Pickup           | 3.85                           | 52.36                
21st St & I St NW              | Dropoff          | 1.68                           | 59.58              
21st St & Pennsylvania Ave NW  | Pickup           | 9.69                           | 18.40               
21st St & Pennsylvania Ave NW  | Dropoff          | 15.75                          | 28.48           
* The usage of bikes is significantly influenced by temperature. During weather in January, such as snowy or rainy conditions, the ridership is notably low. However, in April, as the temperature slightly increases, more people tend to use biking as their transportation choice.
* Day-offs also play a significant role in bike share usage. More people tend to use the service for work and study during normal days, while the usage decreases during off days.
* The number of pickups is mostly lower than the number of drop-offs in the bike share system. This can be attributed to people living in nearby stations or adjacent areas who bring bikes from other stations to these two stations. Additionally, there are metro stations and other bus routes around these two stations, providing commuters with various transportation options to leave for their home.
* Some days exhibit more pickups than drop-offs. On such days, it is important to assess the maximum difference between the two values and consider adding more bikes to the stations for people to use.

## Recommendation
* Given that the dataset shows a relatively small number of users, it would be beneficial for Capital Bikeshare to prioritize customer acquisition efforts.
* To better understand bike and dock availability, it may be more useful to analyze data on an hourly basis rather than a daily basis. This is because the number of pickups and drop-offs per day may not fully reflect the availability of bikes and docks at different times of the day.

## Limitation
### Model & dataset
* For some situations, LASSO may not be the best model to use when calculating R-squared values based on the dataset. If R-squared values fall below 0.5, it might not provide a good explanation of the prediction.
* Although MSE is a commonly used metric to evaluate the model's performance, it's not the only metric that should be considered. It's beneficial to have additional metrics to compare and determine which model is performing the best one.
* The size of the dataset may not be large enough to make accurate predictions. For instance, the weather data used  only covers Jan to Apr, which may yield different results than using a full year's weather data.
### External elements
* Capital Bikeshare may face competition from other bike-sharing services, such as Lime and Bird, which could affect its market share and growth strategy.
* The budget constraints could impact its ability to optimize the number of bikes and docks in each station, which may affect the user experience and overall performance of the system.
