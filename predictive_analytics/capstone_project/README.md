## Project: Predictive Analytics Capstone
![pic](https://cdn.pixabay.com/photo/2017/03/05/20/08/grocery-store-2119702_1280.jpg)
A company has 85 grocery stores and is planning to open 10 new stores at the beginning of the year. Currently, all stores use the same store format for selling their products. Up until now, the company has treated all stores similarly, shipping the same amount of product to each store. This is the beginning to cause problems as stores are suffering from product surpluses in some product categories and shortages in others. The company would like to make decisions about store formats and inventory planning.

The data are stores in `StoreSalesData.csv` and `StoreInformation.csv` files.

## 1. Determine Store Formats for Existing Stores
To remedy the product surplus and shortages, the company wants to introduce different store formats. Each store format will have a different product selection in order to better match local demand. The actual building sizes will not change, just the product selection and internal layouts. The terms formats and segments will be used interchangeably throughout this project. 

To reach the recommendation, I will:
1.   Determine the optimal number of store formats based on sales data.
2.  Segment the 85 current stores into the different store formats.

#### 1.1 What is the optimal number of store formats? How did you arrive at that number? 
The steps to determine the optimal number of store formats is:
- Sum sales data by StoreID and Year
- Use percentage sales per category per store for clustering (category sales as a percentage of total store sales).
- Use a K-means clustering model.

Based on my model, the optimal number of store formats is 3. I arrived this result by using K-Centroids Diagnostics tool with Clustering Method – K-Means on the category sales percentage data. The corresponding AR and CH indices showed that when number of clusters is 3, they have highest median and mean values.

![AR and CH indices](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/capstone_project/pics/1_ar_ch_indices.png)

#### 1.2 How many stores fall into each store format?
The result showed that 23 stores in Cluster 1, 29 stores in Cluster 2 and 33 stores in Cluster 3.

![Clustering Result](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/capstone_project/pics/2_cluster_result.png)

#### 1.3 Based on the results of the clustering model, what is one way that the clusters differ from one another?
Cluster 1 has the smallest number of stores. Cluster 2 has the highest Average Distance, it is the least compact cluster with largest Separation value. Cluster 3 owns the greatest number of stores with the smallest Average Distance and Separation value. It is the most compact cluster.

**1.4 The Tableau visualization that shows the location of the stores**

![Store Locations](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/capstone_project/pics/3_store_location.png)

The store locations of each cluster is shown in [Tableau Public](https://public.tableau.com/profile/rickyzhang3885#!/vizhome/StoreLocationsofDifferentClusters/2?publish=yes).

## 2. Formats for New Stores
The grocery store chain has 10 new stores opening up at the beginning of the year. The company wants to determine which store format each of the new stores should have. However, we don’t have sales data for these new stores yet, so we’ll have to determine the format using each of the new store’s demographic data.

#### 2.1 What methodology is used to predict the best store format for the new stores? Why did you choose that methodology?
The prediction is a kind of Non-Binary Classification due to 3 clusters there is. Therefore, Forest Model, Decision Tree and Boosted Model are the possible models for prediction.

Finally, after model comparison I chose Boosted Model to predict the best store format for the new stores, because Boosted Model performed better than Forest Model and Decision Tree Model. Boosted Model has same Accuracy (0.8235) with Forest Model, higher than that (0.7059) of Decision Tree Model. Moreover, the F1 Score (0.8889) of Boosted Model is higher than 0.8426 of Forest Model.
![PIC4](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/capstone_project/pics/4_model_comparison.PNG)


#### 2.2 What format do each of the 10 new stores fall into?
After prediction by Boosted Model, 3 stores fall into Cluster 1, 6 stores in Cluster 2 and 1 store in Cluster 3.

| Store Number | Segment |
|--------------|:-------:|
| S0086        | 3       |
| S0087        | 2       |
| S0088        | 1       |
| S0089        | 2       |
| S0090        | 2       |
| S0091        | 1       |
| S0092        | 2       |
| S0093        | 1       |
| S0094        | 2       |
| S0095        | 2       |


## 3. Predicting Produce Sales
Fresh produce has a short life span, and due to increasing costs, the company wants to have an accurate monthly sales forecast. I need to prepare a monthly forecast for produce sales for the full year of 2016 for both existing and new stores.

As it is a time-series data analysis, I will build ETS and ARIMA model, compare the results of the two models and then choose the better one to forecast the monthly sales for existing and new stores.

#### 3.1 What type of ETS or ARIMA model is used for each forecast?

ETS(M,N,M) model is chosen for produce sales forecast.

Before choosing the best model, I compared the performance of ETS and ARIMA Model in predicting monthly produce sales.

**ETS(M, N, M) Model with no dampening is used.**
![PIC5](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/capstone_project/pics/5_ets_result.png)

The error changes irregularly, so it is Multiplicative **(M)**.

The trend goes down firstly then up, down and up. It is a no trend line **(N)**.

The peaks of seasonality decrease slightly overtime. It is Multiplicative **(M)**.

**ARIMA(1,0,0)(1,1,0)[12] Model is used.**
Due to the bug of Alteryx, the ACF and PACF plots cannot be displayed in my computer. I can’t analyze seasonal differencing and choosing the parameters for ARIMA based on ACF and PACF plots. I’ve already posted this issue in [Student Hub](https://study-hall.udacity.com/rooms/community:nd008t:645590-cohort-2871-project-437/community:thread-11423639959-614865?contextType=room) and [Alteryx Community](https://community.alteryx.com/t5/Alteryx-Designer-Discussions/TS-plot-doesn-t-display-ACF-and-PACF-plot/m-p/396847/highlight/false#M72856). I used ARIMA Model without specifying non-seasonal components (p,d,q) and seasonal components (P,D,Q). The Model automatically returned (1,0,0) and (1,1,0) as best parameters.

**Model Comparison**
![PIC6](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/capstone_project/pics/6_ets_arima_result.png)
![PIC7](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/capstone_project/pics/7_result_plot.png)

ETS Model has lower values in RMSE and MASE than ARIMA Model, indicating that the accuracy of ETS Model is better than ARIMA Model. From the plot above, the result could be verified as the forecast values of ETS are closer to actual values.

#### 3.2 The table of forecasts for existing and new stores sales, and the visualization of forecasts that includes historical data, existing stores forecasts, and new stores forecasts.
The Forecast Monthly Produce Sales Data Table

<img src="https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/capstone_project/pics/8_forecast_sales.png" width = "50%" height = "50%" alt="sales table" align=center />

Visualization of Forecast Produce Sales
![PIC9](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/capstone_project/pics/9_forecast_plot.png)

The forecast visualization could be found at [Tableau Public](https://public.tableau.com/profile/rickyzhang3885#!/vizhome/ProduceSalesOvertime/Sheet1?publish=yes).
