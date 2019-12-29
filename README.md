# Predicting Solar Energy Output from Weather Data


### Abstract: 
California’s renewable energy infrastructure has been limited by the natural fluctuations in energy supply and demand. In this project, we examined the relationship between various measures of weather data such as DHI and GHI between the year 2010-2016 with a corresponding solar panel’s energy output on a well-documented, government-funded energy initiative. To analyze and predict the amount of energy that would be produced at different times of the year and the best time for energy storage, we utilized several features from the dataset to train the model. We evaluated the accuracy of each model, and our results show the best model from our project is one of the LASSO models with its MSE value equal to 0.18389.

### Background: 
Many climate stabilization plans have been relying greatly upon the power sector moving towards net 0 carbon dioxide output by 2050, and more and more fossil fuel power plants are closing down worldwide. With a higher sense of the crisis that climate change could bring, more and more solar and wind energy farms have been planted. California, specifically, has been one of the most proactive places in the world. However, due to various factors that could impact the renewable energy generation, such as time of the year, weather, etc, higher demand for proper energy storage is required to supply a constant energy output for people. The minimum battery installation comes out at an astonishing price. In 2016, by just raising the renewable energy resources from 1.9% to 2.5%, it would cost a minimum estimated cost of 18.26 billion dollars. This price cost 4.6 billion more than building the Diablo Canyon nuclear power station while producing less net power. One way to lower the cost for such is to have a better understanding of the energy output based on the fluctuating weather pattern. With a model that could help predict the energy output based on the weather data, energy farms will be able to lower the cost of constantly opening the vault for energy storage in unnecessary time of the day and year. It will help improve the efficiency for energy storage and further the idea of one hundred percent reusable energy.
Introduction: Solar energy plays an increasing role in the supply of clean energy in an era of global warming. The energy output of a solar farm is highly dependent on the weather condition present in the corresponding farm. If the energy output can be predicted with higher accuracy it will drastically increase the efficiency for energy supply and optimization for energy storage which will avoid costly overproductions. In this project, our team takes a data-driven perspective on energy prediction based on publicly available weather and geography data to analyze the important predictors along with their correlation on the energy output. In order to create a model that has both high efficiency and robustness, we used model selection and estimation of different model approaches in Python. Our prediction is implemented through publicly available solar and energy data for various solar farms across California. The final model we generated provides a decent prediction of energy output for a newly given weather dataset. Based on our hypothesis, which is Our team believes that with the help of an accurate model for predicting the solar energy output, one will have much better control of the timing for energy storage. 

### Methods:
1. Acquired 414 different solar panels 15 minutes incremented energy output data across California from 2010 - 2016. (raw data download link). This data comes from a California program designed to incentivize solar panel installations by subsidizing said installation. As part of the program, monitors were installed and their data made public.
2. Used the ID column to link output data to the application dataset which contained meta-information about the installation including county, city and zip code.
3. Used GeoPy clients to find latitude and longitude coordinates based on the aforementioned fields. 
4. Found the corresponding weather data through the National Solar Radiation Database’s API using the solar panel’s physical coordinates. This returns data for the following fields in 30 minutes intervals at a spatial granularity of 4km by 4km. Numerical data for relative humidity, dew point, surface albedo, total precipitable water, surface pressure, air pressure, wind speed, air temperature, solar zenith angle, relative humidity, ghi, dhi, dni, as well as clear sky ghi, dhi, dni. A categorical variable describing cloud type was also included.
Definition for GHI: “The total amount of radiation received from above by a horizontal surface.”
Definition for DHI: “Diffuse Horizontal Irradiation is the amount of radiation received per unit area by a surface (no subject to any shade or shadow) that does not arrive on a direct path from the sun, but has been scattered by molecules and particles in the atmosphere and comes equally from all directions.”
5. The above step was time-consuming due to limitations on the free API and the large amounts of data involved. As such due to time constraints, we limited our analysis to 4 solar panels installation of similar type ( part of Large Commercial installations without Axis Tracking and administered by PG&E). 
2. Wrangled the datasets by first dropping all null values and columns with irrelevant data such as company names that own the solar panels, or columns with only 1 unique value. 
3. We then matched and merged the two datasets (weather and energy output) using their overlapping time frames, using DateTime indexes and the installation ID (as there were 4 rows for each timestamp.
4. We used hot one encoding for the cloud type (our only categorical variable) which represents the sky’s condition.
5. Set 2010-2015 as our training dataset, holding 2016 for our test set since the variance in performance between years is not statistically significant, while the variance in performance across months is significant. 
6. Utilized Forward Selection with a Linear Regression model to sequentially find the best ith predictor.
7. Use k-fold CV (k = 5) to select the number of predictors from the Forward Selection to include in our final linear model.
6. We used k-fold CV again to determine the hyperparameter alpha for a ridge and LASSO regularization on the entire predictor field.
7. Finally, we then tested our 3 models on the 2016 data to choose the best model.

### Results: 
The final forward selection model is heuristic, informing us of the sequentially best ith predictors, providing an efficient and practical model fit.  Using k-folds on the training data we found that we should use the top 16 predictors as this model had the lowest test MSE. This model captured 0.86% of the variance of our data(R^2). These 16 predictors are: “GHI + Temperature + ClearskyGHI + DewPoint + RelativeHumidity + CloudType6 + CloudType2 + Pressure + CloudType3 + PrecipitableWater + WindSpeed + CloudType4 + CloudType0 + SolarZenithAngle + DNI + ClearskyDNI”
For the Ridge a Lasso models we used a similar approach. We split the training data into train and test sets using K-folds and compared the following alpha values: [1e-15, 1e-10, 1e-8, 1e-4, 1e-3, 40, 80, 160, 320, 640, 960, 1280,1920, 1560]. The alpha values with the lowest MSE were  960 for Ridge and 80 for Lasso. Below is the MSE plotted against values of log alpha. 
 
 ![final_mse](https://user-images.githubusercontent.com/49171243/71551754-722a3c00-29a3-11ea-9ebf-9c6835697132.png)

 
We compared our Forward Selection, LASSO, and Ridge Regression models to each other using the 2016 data. We found that the best model which performs bet on our text set is the LASSO model with its MSE value equal to 0.18389 and the second best is one of the Ridge regression with its MSE value equals to 0.2078, the Forward Selection model had an MSE of 0.212997. 

### Examples of final trained models:

![final_models](https://user-images.githubusercontent.com/49171243/71551744-45762480-29a3-11ea-9cfa-d5ea340e7087.png)


### Conclusion:
By predicting the energy output of renewable energy farms, it can help provide precision to balancing the output of alternative energy sources (such as coal) which have latency between start-time and energy output. Since the cost of renewable energy storage is the current limiting factor for growth, predictions on energy output by satellite weather data may allow for efficient use or optimization of energy storage and thus aid in the widespread adoption of solar and wind farms. By evaluating the prediction our best model provides, we can indicate that automatically generating accurate machine learning models that can predict solar energy output based on weather data is a promising area. We found that the best model from our project was the LASSO model with an MSE of 0.18389. We also identified the top 3 most critical model parameters are GHI (absolute t-value of  153.3), Clearsky GHI (abs t-value of 75.9) and Temperature (abs t-value of 63.13). We also found out that if we solely focused on creating a model based on one panel, the model prediction’s accuracy is a lot higher. Moving forward we hope we can revisit this model and further develop it to better accommodate better datasets, as our current model is not able to capture all the variance in performance across many different panels. And if we were to increase the project scope, we would likely train a model with many more panels (thousands plus) and introduce new and accurate predictors based on the short-term performance of the panel. 


References:
 “California Distributed Generation Statistics, ” https://www.californiadgstats.ca.gov/ 
 “National Solar Radiation Database, ” https://nsrdb.nrel.gov/
 “Predicting Solar Generation from Weather Forecasts Using Machine Learning, ” http://www.ecs.umass.edu/~irwin/smartgridcomm.pdf
“Solar Radiation Measurement Definition,” https://www.ammonit.com/en/wind-solar-wissen/solarmessung
Gareth James, Daniela Witten, Trevor Hastie, Robert Tibshirani. An Introduction to Statistical Learning: with Applications in R. New York: Springer, 2013.
Python Software Foundation. Python Language Reference, version 3.8. http://www.python.org
