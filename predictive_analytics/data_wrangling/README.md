## Project: Create an Analytical Dataset

## The Business Problem
Pawdacity is a leading pet store chain in Wyoming with 13 stores throughout the state. This year, Pawdacity would like to expand and open a 14th store. I need to perform an analysis to recommend the city for Pawdacity’s newest store, based on predicted yearly sales.

My first step in predicting yearly sales is to first format and blend together data from different datasets and deal with outliers.

The information I have:
1.  The monthly sales data for all of the Pawdacity stores for the year 2010.
2.  NAICS data on the most current sales of all competitor stores where total sales is equal to 12 months of sales.
3.  A partially parsed data file that can be used for population numbers.
4.  Demographic data (Households with individuals under 18, Land Area, Population Density, and Total Families) for each city and county in the state of Wyoming. For people who are unfamiliar with the US city system, a state contains counties and counties contains one or more cities.

## Step 1: Business and Data Understanding
#### Key Decisions:
**1. What decisions needs to be made?**
The decision should be made to recommend the city for opening Pawdacity’s newest store based on predicted yearly sales.

**2. What data is needed to inform those decisions?**
To inform the decision, the current Pawdacity sales data and corresponding demographic information of the cities having Pawdacity stores is needed, and the demographic data of those cities treated as potential new store location is needed.

## Step 2: Building the Training Set

|          Column          |    Sum    |   Average  |
|------------------------|----------:|----------:|
| Census Population        |   213,862 |  19,442.00 |
| Total Pawdacity Sales    | 3,773,304 | 343,027.64 |
| Households with Under 18 |    34,064 |   3,096.73 |
| Land Area                |    33,071 |   3,006.49 |
| Population Density       |        63 |       5.71 |
| Total Families           |    62,653 |   5,695.71 |

## Step 3: Dealing with Outliers
Using the IQR method, I can determine if there are outlier cities for each of the variables and then justify which city that has at least one outlier value should be removed.

**IQR Steps**
To calculate the upper fence and the lower fence, here are the exact steps:
1 . Calculate 1st quartile Q1 and 3rd quartile Q3 of the dataset. 
2 . Calculate the Interquartile Range: IQR = Q3 - Q1
3 . Add 1.5IQR to Q3 to get the upper fence: Upper Fence = Q3 + 1.5IQR
4 . Subtract 1.5IQR to Q1 to get the lower fence: Lower Fence = Q1 - 1.5IQR
5 . Values above the Upper Fence and values below the Lower Fence are outliers

#### Are there any cities that are outliers in the training set? Which outlier have you chosen to remove or impute?
After training set building, the training set is created as below:
![pic here](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/data_wrangling/pics/cleaned_training_set.png)

It can be found that Cheyenne has 4 outliers, in Sales, 2010 Census, Population Density and Total Families. Especially, Cheyenne’s Sales and Population Density are much higher than upper fence based on IQR method. To have more typical data to build prediction model, the row of Cheyenne should be removed. 

Both Gillette and Rock Springs only have one outlier and the values of outliers don’t exceed the upper fence very much. Therefore, the two outliers are acceptable.
