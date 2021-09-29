# Taxi_Fare_Prediction
We were given pickup and drop-off locations, the pickup timestamp, and the passenger count, the objective was to predict the fare of the taxi ride using Random Forest.

## Approach:

### Data Reading: Reading Data from the drive/system.

### Data Preparation: 
  
#### Missing Value Treatment:
     There was no null value present in data set.
  
#### Outlier Treatment: 
       
       Amount: We capped the minimum value at 2.5 because the amount/fare can’t be negative. The least it can be is 2.5. The maximum value we capped according to the 99th
       percentile. 
       
       No. of Passenger: As in case of passengers it can’t be more than 4 people excluding the driver in one ride.
       
       All the other capping were done according to the 99th percentile of the dataset.
  
#### Feature Engineering: 
      
      Calculating the Distance: There are three methods to calculate the distance.
          
          1.	Euclidean Distance: Euclidean Distance represents the shortest distance between two points.
          
          
  ![image](https://user-images.githubusercontent.com/89060175/135294159-32e14da5-01ad-4307-a14c-2d64da1f9795.png)

           We use this formula when we are dealing with 2 dimensions. We can generalize this for an n-dimensional space as:
           
   ![image](https://user-images.githubusercontent.com/89060175/135294234-c949355a-ce5c-48de-80bb-f59726137682.png)

            Where, n = number of dimensions
                   pi, qi = data point
                   
        2.	Manhattan Distance: Manhattan Distance is the sum of absolute differences between points across all the dimensions.
        
   ![image](https://user-images.githubusercontent.com/89060175/135294664-4c07fdff-c3fb-467d-bcb7-62a162d3bb4d.png)
   
            And the generalized formula for an n-dimensional space is given as:
            
   ![image](https://user-images.githubusercontent.com/89060175/135294761-0f9d7236-9cd5-4cc3-bf11-1316356341b0.png)

             Where, n = number of dimensions
                    pi, qi = data point

        3.	Minkowski Distance: Minkowski Distance is the generalized form of Euclidean and Manhattan Distance.
        
   ![image](https://user-images.githubusercontent.com/89060175/135295215-8367f612-f74b-4451-894f-938e122263d6.png)
            
            We derived both Euclidean and Manhattan Distance formula to calculate the distance between pickup and drop-off locations.

            From Date_time_of_pickup we extracted min, hour, day, weekday, time of day, month and year.

#### Dropping of useless variables

### Dummy Creation: 
    
    Machine Learning Algorithms cannot work on Object type datatypes so for that reason we have created dummies for categorical variables. We created dummy of time of day and
    day. Also we created other variables such as Is_weekend, Is_starting_of_month etc for better analysis.

### Graphical Analysis
![Distribution of Fare](https://user-images.githubusercontent.com/89060175/135296080-8f827918-6504-442f-875e-f830ffbe41d6.jpg)

![Fare $USD_mile](https://user-images.githubusercontent.com/89060175/135296198-d320910c-c540-436e-b8ae-bf15f596a847.jpg)

![Average_Fare_vs_Time_of_Day](https://user-images.githubusercontent.com/89060175/135296276-5fddda38-a2db-4a09-a2b4-76ae60fa2725.jpg)

![Average_Fare_vs_Day](https://user-images.githubusercontent.com/89060175/135296391-3f4dbf57-0ffd-4dac-ba9e-b7b0ea702a33.jpg)

![Time_of_Day_vs_Count_of_Passengers](https://user-images.githubusercontent.com/89060175/135296519-82b252a3-9930-41e8-98e8-ec1e9e422345.jpg)

![Time_of_Day_vs_Average_Fare](https://user-images.githubusercontent.com/89060175/135296680-eb997a55-7f48-4f58-97fa-455b979a4802.jpg)

![Day Vs Average Distance](https://user-images.githubusercontent.com/89060175/135296748-5cf271b8-4bac-482b-aeb4-2b76aa9db228.jpg)

![Year vs Count of Passengers](https://user-images.githubusercontent.com/89060175/135296810-17d0eb32-55d4-4ee4-adf2-4091f3729dd5.jpg)

![Year vs No of Passengers](https://user-images.githubusercontent.com/89060175/135296858-2cad3075-171c-46f0-bf72-f9d07aecb3a6.jpg)

### Splitting the data into train and test: 
    Split the data into train and test datasets and feed it into our model. Here we have split the data into 80% train and 20% test.

### Applying the model: 
    Applied RandomForest Regression.

### Predicting y: 
    Predict the y_train using x_train through the model and same goes for y_test.

### Calculating MSE, RMSE and MAE
