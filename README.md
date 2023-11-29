# ⚡Power Outages Analysis⚡
This is a project for DSC 80 at UCSD. 

**Name**: Essie Cheng

**Website Link**: https://essiecheng.github.io/Power-Outage-Analysis/

## Introduction
This [dataset](https://engineering.purdue.edu/LASCI/research-data/outages/outage.xlsx) created by the Laboratory for Advancing Sustainable Critical Infrastructure at Purdue University contains information about power outage data in the U.S. that occurred from January 2000 to July 2016.  The information in the dataset include general information about when and where power outages occur, regional climate information, outage events information, regional electricity consumption information, regional economic characteristics, and regional land-use characteristics. With this information available, a question of interest is where major power outages are more likely to occur and be severe. 
## Question: Are certain locations more prone to major power outages?
Finding regional patterns of where severe power outages may occur can provide crucial insight into areas that require better preparation for future power outages. It can also help people in areas where major outages are likely to occur be better informed about risks and prepare accordingly. The dataset contains 1534 rows and 54 columns. Since our question concerns power outage durations and climate regions, we only need columns in the dataset that include information related to outage duration, geographical information, and other potentially contextual variables. The chosen relevant columns and their [descriptions](https://www.sciencedirect.com/science/article/pii/S2352340918307182) are:
- YEAR: Indicates the year when the outage event occurred
- MONTH: Indicates the month when the outage event occurred
- U.S._STATE: Represents all the states in the continental U.S.
- POSTAL.CODE: Represents the postal code of the U.S. states
- NERC.REGION: The North American Electric Reliability Corporation (NERC) regions involved in the outage event
- CLIMATE.REGION: U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)
- CLIMATE.CATEGORY: This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI)
- OUTAGE.START.DATE: This variable indicates the day of the year when the outage event started (as reported by the corresponding Utility in the region)
- OUTAGE.START.TIME: This variable indicates the time of the day when the outage event started (as reported by the corresponding Utility in the region)
- OUTAGE.RESTORATION.DATE: This variable indicates the day of the year when power was restored to all the customers (as reported by the corresponding Utility in the region)
- OUTAGE.RESTORATION.TIME: This variable indicates the time of the day when power was restored to all the customers (as reported by the corresponding Utility in the region)
- CAUSE.CATEGORY: Categories of all the events causing the major power outages
- CAUSE.CATEGORY.DETAIL: Detailed description of the event categories causing the major power outages
- OUTAGE.DURATION: Duration of outage events (in minutes)
- CUSTOMERS.AFFECTED: Number of customers affected by the power outage event
- TOTAL.CUSTOMERS: Annual number of total customers served in the U.S. state

## Cleaning and EDA
### Data Cleaning
The following was done to clean the data:
- Fix formatting: it appears that due to formatting issues from converting the xlsx file to a csv file, the first 4 rows of the resulting dataframe were all NaN values and the correct column names appeared in row 5. Those rows were dropped and the column names were reassigned.
- Keep relevant columns: all columns are dropped except for the relevant columns listed above.
- Typcasting: All columns are stored as strings, but it would make more sense for numerical information to be stored as floats. The power outages start date and time were merged into one pd.Timestamp column, and power outages restoration date and time were merged into one pd.Timestamp column.

The resulting dataframe has 1534 rows and 14 columns.

### Univariate Analysis
#### 'CLIMATE.REGION' Distribution
A barchart was created to get an idea of the distribution of climate regions.

<iframe src="climate_regions_dist.html" width=800 height=600 frameBorder=0></iframe>

The Northeast region appears to be the most prevalent region with power outages in the data set, and the West North Central region least.

#### 'OUTAGE.DURATION' Distribution
A histogram was created to get an idea of the distribution outage durations.

<iframe src="outage_duration_dist.html" width=800 height=600 frameBorder=0></iframe>

The histogram appears to be heavily skewed right with some outliers of extreme outage durations.

A choropleth was also created to look at the distribution of average outage duration, per state. It's important to note that the average durations may be skewed higher because of the outliers of longer outage durations.

<iframe src="choropleth.html" width=800 height=600 frameBorder=0></iframe>

Indeed, it appears that similar average durations appear in the same regions. For example, higher mean power outage durations are generally distributed among areas such as New York, New Jersey, and West Virgina, all of which are in the northwest region. Lower mean power outage durations are generally distributed among areas such as Montana, Wyoming, and South Dakota, all of which are in the west north central region. Thus, the bivariate analysis will illustrate the relationship between 'OUTAGE.DURATION' and 'CLIMATE.REGION'.

### Bivariate Analysis
#### Outage Duration and Climate Region
In the bivariate analysis, we'll use more course granularity in outage duration and use the mean outage duration by state instead in order to avoid noisy visualization and make it easier to identify patterns and understand bigger-picture trends between climate regions.

<iframe src="boxplots.html" width=800 height=600 frameBorder=0></iframe>

Most notable is the East North Central climate region boxplot, as it has the highest median and largest max for mean outage duration of states in that region, suggesting that outage durations are longer in that climate region. Although the Northeast and Central regions have a relatively low median, they have a more skewed distribution and wider spread, indicating that they could be prone to longer outage durations.

#### Outage Duration and Customers Affected
Observations from the previous choropleth about similar average durations generally being distributed in the same regions indicate another possible relationship between outage duration and customers affected, as some regions are more densly populated than others. For example, lower mean power outage durations are generally distributed among areas such as Idaho, Wyoming, and Montana, all of which are less populated areas. A scatterplot is thus used to observe the relationship between 'OUTAGE.DURATION' and 'CUSTOMERS.AFFECTED'.

<iframe src="scatterplot.html" width=800 height=600 frameBorder=0></iframe>

The association between average outage duration and average number of customers affected appears to be positive and somewhat linear, with a few outliers. The scatterplot indicates that generally the longer the outage duration, the more customers affected.

### Interesting Aggregates
#### Outage Duration, Climate Region, and Month
In addition to investigating where longer power outages occur, it can also investigated when the outages tend to be longer using a pivot table.

| MONTH | Central            | East North Central  | Northeast         | Northwest          | South             | Southeast          | Southwest          | West              | West North Central |
|-------|---------------------|---------------------|-------------------|--------------------|-------------------|--------------------|--------------------|-------------------|--------------------|
| 1.0   | 5320.423077         | 10983.25            | 1161.290323       | 846.357143         | 3473.4375         | 1731.533333        | 53.833333          | 3903.615385       | NaN                |
| 2.0   | 3728.6              | 2728.8              | 5398.942857       | 1041.583333        | 999.6             | 1425.888889        | 174.533333         | 1543.318182       | NaN                |
| 3.0   | 709.8               | 13200.4             | 2501.88           | 539.875            | 2022.615385       | 1153.666667        | 1276.333333        | 3540.235294       | 56                 |
| 4.0   | 2621.214286         | 4592.909091         | 819.105263        | 1004.2             | 1234.115385      | 474.5              | 124                | 853.307692        | NaN                |
| 5.0   | 2924.45             | 7395.8              | 1409.384615       | 222.666667         | 1690.181818       | 3359.6             | 84.5               | 342.733333        | 0                  |
| 6.0   | 2112.771429         | 4457.4              | 2565              | 370.090909         | 1944.965517       | 1243.277778        | 220.5              | 504.777778        | 40.4               |
| 7.0   | 1412.071429         | 3136.095238         | 2616              | 326                | 1643.130435       | 569.8              | 9023.75            | 992.777778        | NaN                |
| 8.0   | 1500.75             | 2730.923077         | 3652.83871        | 1148.076923       | 2496.28125        | 3986.909091        | 109                | 382.583333        | 100                |
| 9.0   | 5428.285714         | 3908.888889         | 2786.954545       | 902.285714         | 10335.058824      | 3763               | 300                | 337               | NaN                |
| 10.0  | 4355                | 3356.428571         | 5488.255814       | 1703.25            | 887               | 5701.6             | 566.75             | 2048.409091       | 106                |
| 11.0  | 1634.3              | 4442.555556         | 876.944444        | 4171               | 1720.6            | 203.666667        | 16                 | 1410.166667       | 30.5               |
| 12.0  | 1110.375            | 4213.7              | 3992.357143       | 2988.6             | 7403.666667      | 1241.222222       | 1450.571429       | 2687.52           | 5160               |


One observation is that colder months seem most frequent for major power outages, as the longest outage durations occur during fall and winter months for several regions (Central, East North Central, West, Northwest, Southeast). Another notable observation is that the West North Central region row contains many NaN values, indicating that power outages occur much less frequently in that region (as many months don't have data). This makes sense as the months that do have data display very short outage durations in comparison to other regions.

#### Outage Duration, Climate Region, and Causes
The average outage duration in each region can also be compared with the causes behind those outages, which can reveal what kind of recovery resources to focus on for each region.

It can be seen that the East North Central region faces the longest power outages, particularly due to equipment failure and fuel supply emergency. In the Southwest region though, those causes played much less of a role and the longest power outages were due to severe weather instead.

It's worth noting that the columns worked with in the EDA contained NaN values that have not been dealt with yet, which may have implications on analyses that skew/bias results. Missingness will thus be assessed next.

## Assessment of Missingness
### NMAR Analysis
NMAR (not missing at random) missingness is where the missingness of the missing value is related to the actual, unreported value. A column in the power outages dataset that I believe is NMAR is the ‘CAUSE.CATEGORY’ column, which describes categories of events that cause the major power outages. A possibility for value to be missing is that the cause itself is unknown, or so weird that it doesn’t fit into a category. Another possibility is that it’s missing due to the nature of the cause, where “bad” causes are more likely to be unreported as it reflects poorly on certain groups. For example, cause categories like “equipment failure” may be a bad look for those who built and maintain the power grids, and cause categories like “intentional attack” may implicate that those responsible for the grids failed to keep citizens safe and are bad at their job. Though we can’t conclude that the ‘CAUSE.CATEGORY’ column is NMAR for certain, it seems likely that its missingness is dependent on the actual cause itself. Additional data we might want to obtain that could explain the missingness (thereby making it MAR) could be data on reporting policies, in order to see if there are specific criteria for reporting certain types of causes. 

### Missingness Dependency
A column with non-trivial missingness is 'CUSTOMERS.AFFECTED'. The missingness of this column is analyzed by performing permutation tests to analyze whether the column missingness depends on 'CAUSE.CATEGORY' and whether it depends on 'TOTAL.CUSTOMERS'

#### 'CUSTOMERS.AFFECTED' vs 'CAUSE.CATEGORY'
A column with non-trivial missingness is 'CUSTOMERS.AFFECTED'. We'll analyze the missingness of this column by performing permutation tests to analyze whether the column missingness depends on 'CAUSE.CATEGORY' and whether it depends on 'TOTAL.CUSTOMERS'

<iframe src="affected_vs_cause.html" width=800 height=600 frameBorder=0></iframe>

The distribution of cause category appears very different, indicating the missingness of 'CUSTOMERS.AFFECTED' is dependent on 'CAUSE.CATEGORY'.
A permutation test will be used to analyze the dependency of the missingness.

**Null hypothesis**: The distribution of 'CAUSE.CATEGORY' when 'CUSTOMERS.AFFECTED' is missing is the same as the distribution of 'CAUSE.CATEGORY' when 'CUSTOMERS.AFFECTED' is not missing.

**Alternative hypothesis**: The distribution of 'CAUSE.CATEGORY' when 'CUSTOMERS.AFFECTED' is missing is different than the distribution of 'CAUSE.CATEGORY' when 'CUSTOMERS.AFFECTED' is not missing.

**Test statistic**: Total variation distance

**Significance level**: 0.05

<iframe src="affected_cause_emperical.html" width=800 height=600 frameBorder=0></iframe>

The resulting p-value of 0.0 leads us to reject the null hypothesis and conclude that the missingness of 'CUSTOMERS.AFFECTED' is dependent on 'CAUSE.CATEGORY'.

#### 'CUSTOMERS.AFFECTED' vs 'CLIMATE.CATEGORY'

<iframe src="affected_cause_emperical.html" width=800 height=600 frameBorder=0></iframe>

The distribution of climate category appears to be very similar, indicating the missingness of 'CUSTOMERS.AFFECTED' is not dependent on 'CLIMATE.CATEGORY'.

A permutation test will be used to analyze the dependency of the missingness.

**Null hypothesis**: The distribution of 'CLIMATE.CATEGORY' when 'CUSTOMERS.AFFECTED' is missing is the same as the distribution of 'CLIMATE.CATEGORY' when 'CUSTOMERS.AFFECTED' is not missing.

**Alternative hypothesis**: The distribution of 'CLIMATE.CATEGORY' when 'CUSTOMERS.AFFECTED' is missing is different than the distribution of 'CLIMATE.CATEGORY' when 'CUSTOMERS.AFFECTED' is not missing.

**Test statistic**: Total variation distance

**Significance level**: 0.05

<iframe src="affected_climate_emperical.html" width=800 height=600 frameBorder=0></iframe>

The resulting p-value of 0.3504 leads us to fail to reject the null hypothesis and conclude that the missingness of 'CLIMATE.AFFECTED' is not dependent on 'CLIMATE.CATEGORY'.

Because the missingness of 'CUSTOMERS.AFFECTED' depended on at least 1 column ('CAUSE.CATEGORY'), we conclude that the overall missingness of 'CUSTOMERS.AFFECTED' is MAR (missing at random)

## Hypothesis Testing
Now that we have an understanding and assessment of our data, we can address our question: **Are certain locations more prone to major outages?**

We will define **major outages** as outage durations above the national average duration. More specifically, we'd like to determine which climate regions have longer-than-average power outages more frequently. To do so, we will perform hypothesis tests for each of the climate regions using the following hypotheses:

**Null hypothesis**: The region's proportion of major outages is equal to the national proportion of major outages.

**Alternative hypothesis**: The region’s proportion of major outages is greater than the national proportion of major outages

**Test statistic**: The proportion of major outages

**Significance level**: 0.01
- Because multiple testing is occurring, the risk of committing a Type I error (rejecting the null hypothesis when it's actually true) is increased. Therefore, the significance level is set lower to reduce the Type I error probability.

#### Final adjustments
Before conducting the hypothesis tests, missingness should be dealt with as the relevant columns 'CLIMATE.REGION' and 'OUTAGE.DURATION' both have NaN values. 
There are only 6 NaN values in 'CLIMATE.REGION', a trivial portion of the dataset. Therefore, listwise deletion can be used to get rid of THE NaN values for 'CLIMATE.REGION'. There are 58 NaN values in 'OUTAGE.DURATION'. Imputation will be used to fill in the NaN values, specifically median imputation, since earlier exploration revealed that there are outliers in the dataset and the median is more robust to outliers.

#### Testing
The result:

| Region      | P-value | Reject Null Hypothesis |
| ----------- | ----------- | ----------- |
| East North Central      | 1.0       | False       |
| Central      | 1.0       | False       |
| South      | 1.0       | False       |
| Southeast     | 0.0       | True       |
| Northwest      | 1.0       | False       |
| Southwest      | 0.0       | True       |
| Northeast      | 0.0       | True       |
| West North Central      | 0.0       | True       |
| West      | 0.0       | True       |

It looks like the Southeast, Southwest, Northeast, West North Central, and West regions have a proportion of major outages greater than that of the greater United States, as the null hypothesis is rejected for those regions. It can be suggested that regional factors impact power outage durations and those regions are more prone to major power outages.

An observation of interest is that the East North Central region isn't on the list of regions that are more prone to major power outages, despite having the highest median and max outage duration out of all regions. This may be because the East North Central region has major outages that are longer compared to other regions, but its proportion of major outages is actually similar to the proportion of major outages nationwide.

One last note is that since statistical tests were performed and not randomized controlled trials, the results of the test aren't proven to be 100% true.
