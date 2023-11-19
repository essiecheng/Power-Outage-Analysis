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
- Fix formatting: it appears that due to formatting issues from converting the xlsx file to a csv file, the first 4 rows of the resulting dataframe were all NaN values and the correct column column names appeared in row 5. Those rows were dropped and the column names were reassigned.
- Keep relevant columns: all columns are dropped except for the relevant columns listed above.
- Typcasting: All columns are stored as strings, but it would make more sense for numerical information to be stored as floats. The power outages start date and time were merged into one pd.Timestamp column, and power outages restoration date and time were merged into one pd.Timestamp column.

The resulting dataframe has 1534 rows and 14 columns.

### Univariate Analysis
#### 'CLIMATE.REGION' Distribution
A barchart was created to get an idea of the distribution of climate regions.
<iframe src="climate_regions_dist.html" width=800 height=600 frameBorder=0></iframe>

