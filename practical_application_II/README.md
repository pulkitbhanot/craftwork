

# Factors affecting price of used cars

This folder contains an application of CRISP-DM Framework to a real world problem of predicting car prices for used cars. The used dataset contains information on 426K cars which is available in the data folder as [vehicles.csv](./vehicles.csv]).

## Features

The dataset contains multiple numericals and categorical features. Some of the categorical features have not been used due to large cardinality of values.

Feature name | DataType | Category | Non-Null values | Unique values
--- | --- | --- | --- |--- 
id | Int64 | Numerical| 426880 | 426880
region | string | Categorical | 426880 | 404
price | Int64 | Numerical | 426880 | 15161
year | Int64 | Numerical | 425675 | 89
manufacturer | string | Categorical | 409234 | 42
model | string | Categorical | 421603 | 23382
condition | string | Categorical | 252776 | 7
cylinders | string | Categorical | 249202 | 8
fuel | string | Categorical | 423867 | 6
odometer | Int64 | Numerical| 422480 | 102353
title_status | string | Categorical | 418638 | 7
transmission | string | Categorical | 424324 | 4
VIN  | string | Categorical | 265838 | 118247
drive | string | Categorical | 296313 | 4
size  | string | Categorical | 120519 | 5
type | string | Categorical | 334022 | 14
paint_color | string | Categorical | 296677 | 13
state | string | Categorical | 426880 | 51


## Phases
The following are the set of activities done for the different phases in the CRISP-DM framework

#### Business Understanding

We want to identify which factors can help us predict more acurately the prices of used cars. As part of this analysis we want to understand if the cars from a given manufacturer sell for higher prices than similar cars(similar age, mileage and car type). Two user segments that will benefit from this analysis are

- used car dealership to stock inventory of cars
- consumers to be able to understand what a fair price for a given car should be.

#### Data Understanding

To Understand the data we do EDA (Exploratory Data Analysis) which led towards some conclusion

- Dataset has columns that needs to be removed.
- Dataset has columns with missing values and a strategy needs to be devised to deal with the missing values, some columns can be dropped while for other we need to impute the values.
- Due to the quality of data, none of the numerical features seems to be strongly correlated to price.

#### Data Preparation

As part of data Preparation, we have cleaned the data and kept only the rows for which the values for price, year and odometer is <= .999 percentile of the original data.
- We have JamesSteinEncoder to encode the categorical features.
- We have imputed the null values to 1 nearest neighbor values using KNNImputer and saved the imputed values to knn_imputed_all.csv.
- Post imputation we do not see any null values in the dataset and the mean price for the entire dataset has shifted by < 1%

#### Modeling

Performing a correlation analysis we found that there are features that seem to have both positive and negative correlation. Some of the intial observations from the above analysis are

Price seems to have postive correlation with manufacturer, fuel, type, cylinders and region.
Price seems to have negative correlation with age and odometer. This is also a general observation that older vehicles or vehicles with more mileage tend to be cheaper than the other.

#### Evaluation

As part of evaluation we have trained 6 different models including some with Grid search cross validation
- We are using Median Absolute Error(MAE) to measure the error on test set from the different models as there are outliers and other error computation strategies seem to be less relevant.
- We did permutation importance across multiple models to understand which features impact the prediction the most and also compared the feature importance with the coefficients for these features for the best models as reported from Grid search.

We have found that Linear Regression, polynomial feature degree 6 has the lowest Median Absolute Error(MAE) of 3537.9110 for training and 3537.9110 for test dataset.

## Conclusion

There are several factors that determine the price of a used car.

- Mileage of the car influences the price of the car the most. So if given a choice between 2 cars a consumer is going to prefer cars that are driven less and hence you can sell low mileage cars for a better profit.
- Fuel type of the cars also influence the price of the car. Gasoline cars seems to be by far the most selected option in used car market.
- Type of the car(Sedan, SUV, Pickup etc) also impact the price of the cars. There however maybe a regional impact on the type of the car a consumer might decide to purchase. This is also evident from the fact that region a car is sold in has a positive correlation to the price of the car.
- Vehicles with bigger enginer tend to negatively influence the price of the car. Hence in used car market people are looking for more fuel efficient vehicles.
- Manufacturer of the car also influences the price of the car, hence there are manufacturers whose cars are more popular among the buyers than the others.
- As is the known fact age of the car(or the year the car was manufactured) also influences the price of the car. However compared to other factors age has little impact on the price of the car i.e a car with less mileage but older might sell for a higher price than a more recent car with higher mileage.

## Artifacts

Notebook for the assignment can be referenced at [problem_II.ipynb](./problem_II.ipynb). Imputation process using KNNImputer takes a few hours to run. The Imputed values have been saved in [knn_imputed_all.csv](./data/knn_imputed_all.csv).


## Appendix

Any additional information goes here

- https://contrib.scikit-learn.org/category_encoders/jamesstein.html
- Numerous stack overflow questions.
- https://seaborn.pydata.org/index.html