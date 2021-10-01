**Taxi Dataset**

**Problem Statement:**

To predict the fare of the taxi ride using Random Forest given pickup and drop-off locations, the pickup timestamp, and the passenger count.

Abstract:

In last few years, it has been observed that number of operating hire vehicles and number of trips in app-based vehicle increased drastically. However, taxi trips have fallen in same proportion in which number of vehicles increased.

The purpose of doing this exercise is two-fold:

1. To facilitate the cab organisation data centric outcome so that they can provide the optimize overall taxi fare by incorporating the flexibility in some of the fixed cost like waiting time in pickup, drop off and traffic jam along with peak hours.
2. To increase the Net promoting Score (NPS) by winning the confidence of customers in competitive market which resulted in improving taxi trips.

**Roadmap:**

The quality of results depends on the data quality and features used for modelling. Given below are the steps, I followed while building the model

1. Data Integration and massaging
2. Exploratory Analysis
3. Building Model
4. Testing and validation of Model
5. Result and Interpretation

_ **Data Integration and massaging** __ **:** _This step involves data reading, data preparing by attempting missing value/outlier treatment, creation of derived variable, dropping unwanted columns, converting the categorical variables to machine-understandable format, and finally splitting of training data into train and validation sets.

1. Loading Dataset: The dataset consists of 50,000 rows and 7 Columns.


![image](https://user-images.githubusercontent.com/89060175/135584400-1cb4960e-86e4-42ef-958f-d3785d362470.png)

1. Data Preparation:

1. Missing Value Treatment: There was no null value present in data set.
2. Outlier Treatment:

    - Amount: We capped the minimum value at 2.5 because the amount/fare can&#39;t be negative. The least it can be is 2.5. The maximum value we capped according to the 99th percentile.

          - No. of Passenger: As in case of passengers it can&#39;t be more than 4 people excluding the driver in one ride.
          - All the other capping were done according to the 99th percentile of the dataset.

| **Table-2: Statistical Information of Raw Dataset** |
| --- |
| **Columns** | **Statistics** |
| --- | --- |
|   | count | mean | std | min | Q1(25%) | Q2(50%) | Q3(75%) | max | Coeff Of variation |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| amount | 50000 | 11.4 | 9.7 | -5.0 | 6.0 | 8.5 | 12.5 | 200.0 | 1.2 |
| longitude\_of\_pickup | 50000 | -72.5 | 10.4 | -75.4 | -74.0 | -74.0 | -74.0 | 40.8 | -7.0 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| latitude\_of\_pickup | 50000 | 39.9 | 6.2 | -74.0 | 40.7 | 40.8 | 40.8 | 401.1 | 6.4 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| longitude\_of\_dropoff | 50000 | -72.5 | 10.4 | -84.7 | -74.0 | -74.0 | -74.0 | 40.9 | -7.0 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| latitude\_of\_dropoff | 50000 | 39.9 | 6.0 | -74.0 | 40.7 | 40.8 | 40.8 | 43.4 | 6.6 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| no\_of\_passenger | 50000 | 1.7 | 1.3 | 0.0 | 1.0 | 1.0 | 2.0 | 6.0 | 1.3 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

1. Feature Engineering:

          - Travel Distance Calculation: There are three methods to calculate the distance.

      1. Euclidean Distance: Euclidean Distance represents the shortest distance between two points.

![](RackMultipart20211001-4-11n4h4n_html_a009753769a9cfa8.png)

![](RackMultipart20211001-4-11n4h4n_html_b674edaf216bad6c.png)

Where, n = number of dimensions

pi, qi = data point

      1. Manhattan Distance: Manhattan Distance is the sum of absolute differences between points across all the dimensions.

![](RackMultipart20211001-4-11n4h4n_html_7905f45fc95db66a.png)

![](RackMultipart20211001-4-11n4h4n_html_841afc804ae59626.png)

Where, n = number of dimensions.

pi, qi = data point

      1. Minkowski Distance: It is the generalized form of Euclidean and Manhattan Distance.

![](RackMultipart20211001-4-11n4h4n_html_b1fe1482f9b83b5c.png)

    - We derived both Euclidean and Manhattan Distance formula to calculate the distance between pickup and drop-off locations.

    - From variable Date\_time\_of\_pickup we extracted minutes, hour, day, weekday, time of day, month and year for classifying/stratifying the utilization effect in different time and days.

1.

| **Table-3: List of Discarded Variables** |
| --- |
| **Column** | **Description** |
| date\_time\_of\_pickup | The time when the ride started |
| Unique Id | A unique identifier or key for each record in the dataset |
| Time | Time in hh:mm:ss |
| Date | Date in DD/MM/YYYY |
| Second |   |
| longitude\_of\_pickup | Longitude of the taxi ride pickup point |
| latitude\_of\_pickup | Latitude of the taxi ride pickup point |
| longitude\_\_of\_dropoff | Longitude of the taxi ride dropoff point |
| latitude\_of\_dropoff | Latitude of the taxi ride dropoff |

Discarding Redundant Variables: Given below variables have been dropped which are not relevant or have been used in deriving new variables before model building.

1. Dummy Variable Creation: Machine Learning Algorithms cannot work on object type datatypes so for that reason we have created dummies for categorical variables. We created dummy of time of day and day. Also, we created other variables such as Is\_weekend, Is\_starting\_of\_month etc for better analysis.

1. Splitting the data into train and test: Split the data into train and test datasets and feed it into our model. Here we have split the data into 80% train and 20% test. The training and testing dataset consist of 39155 rows and 9789 rows correspondingly with 23 Columns in both datasets. Below is the list of final variables in training and testing dataset.

| **Table-4: Final list of variables used in Model** |
 |
| --- | --- |
| no\_of\_passenger | time\_of\_day\_Evening | day\_Tuesday |
| Distance\_Travel | time\_of\_day\_Morning | day\_Wednesday |
| hours | time\_of\_day\_Night | Is\_Weekend |
| year | day\_Friday | Is\_starting\_of\_month |
| day\_of\_the\_month | day\_Monday | Is\_middle\_of\_month |
| day\_no | day\_Saturday | Is\_end\_of\_month |
| manhattan\_d | day\_Sunday | Is\_leap\_year |
| time\_of\_day\_Afternoon | day\_Thursday |   |

| **Table-5: Statistics of Training and Testing Dataset** |
| --- |
| **Statistic** | **Training Dataset** |
| --- | --- |
| **Distance\_Travel** | **manhattan\_d** | **no\_of\_passenger** | **Amount** |
| --- | --- | --- | --- |
| **count** | 39155 | 39155 | 39155 | 39155 |
| --- | --- | --- | --- | --- |
| **mean** | 0.034 | 0.045 | 1.560 | 11.252 |
| --- | --- | --- | --- | --- |
| **std** | 0.038 | 0.055 | 0.998 | 8.965 |
| --- | --- | --- | --- | --- |
| **min** | 0 | 0 | 0 | 3 |
| --- | --- | --- | --- | --- |
| **Q1** | 0 | 0 | 1 | 6 |
| --- | --- | --- | --- | --- |
| **Q2** | 0.02 | 0.03 | 1 | 8.5 |
| --- | --- | --- | --- | --- |
| **Q3** | 0.04 | 0.05 | 2 | 12.5 |
| --- | --- | --- | --- | --- |
| **max** | 0.228 | 1.538 | 4 | 52.0 |
| --- | --- | --- | --- | --- |
| **Coeff Of variation** | 0.893 | 0.807 | 1.563 | 1.255 |
| **Statistic** | **Testing Dataset** |
| --- | --- |
| **Distance\_Travel** | **manhattan\_d** | **no\_of\_passenger** | **Amount** |
| --- | --- | --- | --- |
| **count** | 9789 | 9789 | 9789 | 9789 |
| --- | --- | --- | --- | --- |
| **mean** | 0.034 | 0.044 | 1.562 | 11.245 |
| --- | --- | --- | --- | --- |
| **std** | 0.038 | 0.055 | 0.999 | 8.926 |
| --- | --- | --- | --- | --- |
| **min** | 0 | 0 | 0 | 3 |
| --- | --- | --- | --- | --- |
| **Q1** | 0 | 0 | 1 | 6 |
| --- | --- | --- | --- | --- |
| **Q2** | 0.02 | 0.03 | 1 | 8.5 |
| --- | --- | --- | --- | --- |
| **Q3** | 0.04 | 0.05 | 2 | 12.5 |
| --- | --- | --- | --- | --- |
| **max** | 0.228 | 1.448 | 4 | 52.0 |
| --- | --- | --- | --- | --- |
| **Coeff Of variation** | 0.898 | 0.810 | 1.563 | 1.260 |

_ **Exploratory Analysis** _

1. Graphical Analysis

![](RackMultipart20211001-4-11n4h4n_html_6dcce8fd58ecf48e.jpg)

We checked the distribution of the amount/fare and as it can be seen the distribution is highly skewed. In addition, the density of fare in the range of 0 to 20 is significantly higher. This could indicate that individuals prefer not to use taxi services for lengthy distances because long distances imply a high fare.

![](RackMultipart20211001-4-11n4h4n_html_634e9a4ce5136cb0.jpg)

For different years, this graph displays the link between fare/mile and hour. Each crooked line represents a distinct year&#39;s fare movement. Every line has some ascend between 5 and 7 a.m., which is often the commencement of business hours, and therefore a spike in fare can be expected.

![](RackMultipart20211001-4-11n4h4n_html_4f479713c1a80ce8.jpg)

We had now divided the entire 24-hour period into four pieces. Namely:

1. Morning: 6 a.m. to 12 p.m.

2. Afternoon: 12 p.m. to 17 p.m.

3. Afternoon: 17 to 21

4. Evening: 21 to 6 p.m.

In comparison to the other times, the fare is significantly higher in the afternoon and at night. This increase could be related to the unusual hours, as most cab firms impose a night premium so that drivers don&#39;t cancel rides and lose out on the odd hour incentives.

![](RackMultipart20211001-4-11n4h4n_html_bdcd4146fa38b6d6.jpg)

We see an unusual pattern here, as the average fare on weekends should be lower than on other days. A portion of this can be seen in the Saturday drop in average fare. However, Sunday is the day when most people plan intercity journeys to shops and restaurants, so the small increase on Sunday could be related to that. ![](RackMultipart20211001-4-11n4h4n_html_16e2c31bb2e765cd.jpg)

The suitable trend may be seen when looking at the time of day vs. the number of passengers on different days. The majority of passengers travelling at night do so on weekends, particularly on Saturday and Sunday, while for other times of day, particularly in the morning, less people use taxi services on Saturday and Sunday.

![](RackMultipart20211001-4-11n4h4n_html_337d9c0693a67249.jpg)

It&#39;s merely a different version of the graph above. With respect to different days, time of day vs. average fare. Wednesday has the lowest average fare for evening and morning travel, while Friday and Monday have the highest average fare for afternoon and night travel.

![](RackMultipart20211001-4-11n4h4n_html_a418870f25012dba.jpg)

The number of passengers is falling at the end of 2014, as seen in the graph above, and the trend of each day for the given years is similar.

_ **Model Building** __ **:** _

Random Forest is far more flexible having lower bias, and it can fit the data better.

1. Applying the model: Applied RandomForest Regression.
2. Predicting y: Predict the y\_train using x\_train through the model and same goes for y\_test.
3. Calculating MSE, RMSE and MAE

_ **Testing and validation of Model** _

The validation of model helps in understanding how the model fit using training data and works on any unseen data. This facilitate to gauge whether the model is overfitting or underfitting. Overfitting is the term used when training error is low, but the testing error is high. This is a common problem with complex models because they tend to memorize the underlying data and hence perform poorly on unseen data.

_ **Result and Interpretation** _

R2 is a measure of how well a model predicts the target variables. The error is calculated using the RMSE and MAE methods. As can be seen, our model has good accuracy and a low bias/variance trade-off. We can conclude from this analysis that our model is performing well.

| **Table-6: Statistics of Predicted variable (Taxi fare)** |
| --- |
| **Statistic** | **Training Dataset** | **Testing Dataset** |
| --- | --- | --- |
| **Amount** | **Amount** |
| **count** | 39155 | 9789 |
| --- | --- | --- |
| **mean** | 11.26 | 11.21 |
| --- | --- | --- |
| **std** | 8.10 | 8.00 |
| --- | --- | --- |
| **min** | 5.31 | 5.31 |
| --- | --- | --- |
| **Q1** | 6.85 | 6.85 |
| --- | --- | --- |
| **Q2** | 8.78 | 8.85 |
| --- | --- | --- |
| **Q3** | 12.68 | 12.57 |
| --- | --- | --- |
| **max** | 50.88 | 50.88 |
| --- | --- | --- |
| **Coeff Of variation** | 1.39 | 1.40 |
| --- | --- | --- |

| **Table-7: Model Statistics of Training and Testing dataset** |
| --- |
| **Model Parameter** | **Training Dataset** | **Testing Dataset** |
| R square(R2) | 0.8277 | 0.8211 |
| MAE | 2.0792 | 2.1125 |
| RMSE | 3.7211 | 3.7755 |
