# Project: A/B Test a New Menu Launch
![new_menu](https://images.ctfassets.net/com0r9vws8o2/1biEmbwiS7xmcMBpjLQLJN/4960df1ff8e97ca6fb4b577dda24af30/new_menu_desktop.jpg)

## The Business Problem
Round Roasters is an upscale coffee chain with locations in the western United States of America. The executive team conducted a market test with a new menu and needs to figure whether the new menu can drive enough sales to offset the cost of marketing the new menu. **This article is to analyze the A/B test and write up a recommendation to whether the coffee restaurant chain should launch this new menu.**

The three data files to use for analysis:
-   Transaction data for all stores from 2015-January-21 to 2016-August-18
-   A listing of all Round Roasters stores
-   A listing of the 10 stores (5 in each market) that were used as test markets.

The test ran for a period of 12 weeks (2016-April-29 to 2016-July-21) where five stores in each of the test markets offered the updated menu along with television advertising.

The comparative period is the test period, but for last year (2015-April-29 to 2015-July-21).

The predicted impact of new menu to profitability should be enough to justify the increased marketing budget: at least **18% increase in profit growth** compared to the comparative period while compared to the control stores. In the data, profit is represented in the _gross_margin_ variable.

## Step 1: Plan Analysis
**1. What is the performance metric you’ll use to evaluate the results of your test?**

The gross_margin variable will be used to evaluate the results. At least 18% increase in profit growth of treatment stores compared to control stores is required to justify the marketing budget for new menu.

**2. What is the test period?**

The test ran for a period of 12 weeks (2016-April-29 to 2016-July-21) where five stores in each of the test markets offered the updated menu along with television advertising.

**3. At what level (day, week, month, etc.) should the data be aggregated?**

The data should be aggregated at `week` level.

## Step 2: Data Preparation
As we need to have weekly transaction data for all stores, in this step, I aggregated the transaction data to the weekly level and filter on the appropriate data ranges. 

- **Test Period:** 2016-04-29 – 2016-07-21 (12 weeks)
- **Comparative Period:** 2015-04-29 – 2015-07-21 (12 weeks)
- **Transaction Data Period:** 2015-01-21 – 2016-08-18
- **Required Data Period for A/B Test:** 2015-02-06 – 2016-07-21 (52 + 12 + 12 = 76 weeks)

The steps to get weekly transaction data for all stores:
1. Change the data type of some variables from v-string to appropriate types
2. Filter the rows with invoice date between Feb 06 2015 to Jul 21 2016.
3. Add “week”, “week_start” and “week_end” variables to the data set
4. Aggregate data set to week level to get weekly invoice count and gross margin of each store

After data manipulation, the first a few rows of data are shown as below:
![pic 1](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/ab_test/pics/1_cleaned_data.png)

## Step 3: Match Treatment and Control Stores
To match treatment and control store, we need to create the trend and seasonality variables, and use them along with other control variable(s) to match two control units to each treatment unit. Therefore, we can calculate the number of transactions per store per week to calculate trend and seasonality.

Apart from trend and seasonality, looking at the variables in RoundRoasterStore file, `Sq_Ft` and `AvgMonthSales` and `Region` seems to be considered as potential control variables. Stores in different regions may result in different sales trend which may influence the gross margin trend. `AvgMonthSales` should be considered as a continuous control variable because usually sales has strong correlation with gross margin. Regarding `Sq_Ft`, the more square foot area the store has, the more sales/gross margin it may get.

To verify the potential control variables, the correlation matrix is generated as below:
![pic 2](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/ab_test/pics/2_corr_matrix.png)

It can be found that `AvgMonthSales` has very great correlation with gross margin, while `Sq_Ft` has weak correlation with gross margin.

Based on above analysis, `AvgMonthSales` is selected as one control variable, Trend and Seasonality of A/B Controls are another 2 control variables to match treatment and control stores.

After matching,  we get treatment and control stores pairs as below:

| Treatment Store | Control Store 1 | Control Store 2 |
|:---------------:|:---------------:|:---------------:|
|       1664      |       7162      |       8112      |
|       1675      |       1580      |       1807      |
|       1696      |       1964      |       1863      |
|       1700      |       2014      |       1630      |
|       1712      |       8162      |       7434      |
|       2288      |       9081      |       2568      |
|       2293      |      12219      |       9524      |
|       2301      |       3102      |       9238      |
|       2322      |       2409      |       3235      |
|       2341      |      12536      |       2383      |

## Step 4: Analysis and Conclusion
As per the data preparation and store matching, the A/B test analysis model is built in Alteryx and the result is shown as below.

**1. What is the recommendation - Should the company roll out the updated menu to all stores?**

Based on the A/B test result, the company should roll out the updated menu to all stores, because the overall lift in profit growth is 40.7%, much higher than the threshold - 18%.

**2. What is the lift from the new menu for West and Central regions?**

The lift from new menu for West regions is 37.9% or 526.5 increase in weekly gross margin, with very high significance level 99.5%.
![3](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/ab_test/pics/3_west_result.png)

The lift from new menu for Central regions is 43.5% or 835.9 increase in weekly gross margin, with very high significance level 99.6%.
![4](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/ab_test/pics/4_central_result.png)

Overall, the lift from the new menu is 40.7% or 681.2 increase in weekly gross margin.
![5](https://github.com/rickyzhangwl/data_analytic_projects/blob/master/predictive_analytics/ab_test/pics/5_overall_result.png)
