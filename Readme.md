# Find the best NYC Zipcodes to Invest in! Using AirBnB & Zillow Datasets #
---
## Executive Statement
---
Table of Content:
1. [Problem Statement](#problem)
2. [Importing Libraries](#procedure)
3. [Developed Metrics](#metrics)
4. [Conclusions and Insights](#conclusion)


<a id = problem> </a>
# Problem Statement #
The purpose of this analysis is to provide the best zip codes within NYC for investment on short-term rental two-bedroom properties for a real estate company. 

Based on the analysis, the zip codes within NYC are ranked based on their expected return on investment and zip codes with the highest ROIs in the list, are expected to be the most profitable to invest in.
The initial assumption of the analysis are as follows:

1- The investor will pay for the properties in cash, there would be no mortgage and interest rate to be accounted for.

2- The inflation rate is not considered.

3- Occupancy of all the properties within NYC is the same as 75%.

4- All properties within each locale are assumed to be homogeneous.

Besides the analysis steps and results provided, the limitations of this analysis and some insights for further future analysis are also discussed within the report. Please refer to requirements.txt file for any packages and libraries installation. .ipynb file is the code with all the details and at the end there is an interactive folium map to show the different NYC zip codes and their ROI/ expected ROI. Please refer to PDF format of the report if the folium map did not open. Screen shots of the folium map also have been included in the file. Two json files are for folium map and in order to work with folium, they need to be in the same folder.

Below, the steps taken for this analysis are listed:

***
<a id = procedure> </a>
# Procedure #
1- The two datasets were fully investigated to identify the key features, complementary fields, and the common field between the two datasets. 

2- Since only two-bedroom properties were of interest and the Zillow dataset only provided data for two-bedroom properties the Airbnb dataset was also restricted to two-bedroom properties only in order to obtain comparable data.

3- Missing data were investigated. When dealing with missing data, the best approach is to fill in or impute the missing data reasonably when possible. Data exploration showed that Airbnb dataset had records with missing zip codes, but all those records had GPS coordinates that can be used for populating the corresponding location zip code. Since missing Zip Codes are imputable using latitude and longitude information, a code was developed using uszipcode package from PyPI and was used to impute the missing/null zip codes in the Airbnb dataset.

4- Data from both Airbnb and Zillow datasets was restricted to NYC properties only. A list of NYC zip codes, extracted from the website of the [**New York State Department of Health**](https://www.health.ny.gov/statistics/cancer/registry/appendix/neighborhoods.htm), was used to identify and filter records with zip codes matching the NYC zip codes.

5- Since the Airbnb dataset lacked the actual occupancy information for rental properties, it was assumed that all rental properties have an occupancy rate of 75%. However, having the actual occupancy data is critical for such analysis and will result in more accurate results.

6- Since return on investment for a rental property depends on: 1. rental income, and 2. change in property value, the average yearly change of two-bedroom property value in each zip code was considered in the analysis by addition of a Home Price Appreciation Rate column to the data.

7- The two datasets were checked to make sure all records are unique, and duplicate records do not exist. Then, the Airbnb dataset data were averaged for each NYC zip code (using groupby and mean) to obtain one record for each zip code with averaged nightly prices. The Zillow dataset already had unique zip codes and did not need averaging. This step is critical before merging the two datasets.

8- For the following reasons it was decided to use 2014 to 2017 average property values from Zillow dataset to calculate the annual property value appreciation rate for each zip code:
•	It was deemed that the 2008 recession disrupted the normal real state market and the disruption in the market affected the property values for the following couple of years. Thus, it was decided to use a range of years far from 2008.
•	The most recent available property value data are expected to better represent the current and near future market trends.
•	The most recent data are expected to be more accurate and the Zillow dataset had no missing data between years 2014 and 2017 for the NYC zip codes.

9- After the needed data cleaning, imputation, exploration, and manipulations were performed on the Airbnb (limited to 2 bedroom NYC properties) and Zillow datasets (limited to NYC zip codes), the two datasets were merged using the “zip code” field. The final merged table is based on unique zip codes and contains information such as zip code, average nightly price of rental properties, and average values of properties in different years.

10- Several metrics were developed to investigate the return on investment within each NYC zip code based on the available data in the two datasets.
***
<a id = metrics> </a>
# Developed Metrics #
It should be noted that since data such as rental property fees and maintenance costs, actual occupancy rates, and proximity and access to public transportation, attractions and so forth are not available, it is assumed that:

• All rental properties have a similar occupancy rate.

• The rental income is directly proportional to the net rental income. That is, the ownership fees and costs are proportional to the rental income. 

• Rental Two-bedroom properties are consistent within each zip code in terms of return on investment. 

However, to provide better recommendations and make better decisions for rental property investments it would be vital to consider all the influential parameters and variables.

**Average Annual Occupancy**
The Average Annual Occupancy is calculated by assuming an Occupancy Rate of 75% for all rental properties. This assumption was made due to the lack of actual occupancy data in the Airbnb dataset and also lack of any other meaningful data in the dataset that would help to estimate the occupancies.

**Annual Rental Income**
The Annual Rental Income is calculated by multiplying the number of annual occupancy nights by the average nightly rental Price of properties within each zip code ("price" column in Airbnb dataset).

**Property Value Appreciation Rate**
Property value appreciation rate for each zip code is calculated using the following formula:

 $$FV = PV * [1+ {i}] ^{(n)} $$

Where,
  
  FV: future value
  
  PV: past value
  
  i: annual appreciation rate
  
  n: number of years

**Payback Years:**
Payback years is simply the number of years it takes to cover the purchase value (2017 value in the Zillow dataset) using the rental income (assuming the rental income is the net income).

**ROI:**
Return on Investment (ROI) is the percentage of the investment that will return annually and is calculated by dividing the Annual Rental Income by initial property cost (value when purchased).

**Total Expected Return on Investment:**
This metric considers both rental income and property value appreciation/depreciation to provide a more comprehensive return on investment metric. Since the rental properties are assets of the real estate company, their value appreciation/depreciation could affect their favorableness and thus should be taken into account. This metric is calculated by adding the Return on Investment and Property Value 
Appreciation Rate together.

***
<a id = conclusion> </a>
# Conclusions and Insights #
This analysis used a Zillow dataset including historic average two-bedroom property prices and an Airbnb dataset containing short term rental property information such as type, location, and rental prices to develop several metrics for ranking the zip codes based on their expected return on investment. The following **assumption** were made to perform the analysis:

1. **No mortgage:** Properties are fully paid by cash without mortgage.

2. **75% occupancy:** Since occupancy data was not available it was assumed that all rental properties in NYC have a 75% occupancy rate. *However, the occupancy is affected by many factors such as proximity and access to public transportation, attractions, shops and restaurants and so on.*

3. **Nightly prices proportional to net rental income:** Since the rental property expenses such as tax, fees, and maintenance costs were not available it was assumed that the nightly rental income and the net rental income are proportional with a linear relationship. That is, it was assumed that using the nightly rental income instead of net rental income would not change the relative rankings when comparing return on investments.

4. **Property prices and nightly rentals are consistent within each zip code:** It was assumed that the average two-bedroom property prices and nightly rental prices are consistent within each zip code and the average value can fairly represent the rental properties located within the zip code. *However, even within one zip code, factors such as the area covered by the zip code, change of population density, location of attractions, and ease of access to public transportation can considerably affect property values and nightly rental prices.*

5. **Property price in each zip code is proportional to square footage and the effect of number of bathrooms is proportionally reflected in property value and nightly rental price.**
 
Based on the analysis presented and the assumptions made, the **conclusion** is:
1.	Zip codes **11434, 10303, and 10304** are respectively expected to have **the highest rate of return on investment.**

2.	Zip codes with the highest expected ROI are located in **Queens and Staten Island neighborhoods.**

3.	The results suggest that rental properties located in zip codes with lower prices outside of the densely populated neighborhoods have the highest potential Return on Investment. 

However, this analysis was performed using *limited data* and in a *shorter than ideal timeframe.* In order to better analyze the NYC zip codes favorableness in terms of real estate investment on short term rental properties more *insights* and a deeper analysis is needed. More data can be collected and used to address some of the *uncertainties* and *questions* such as:
1.	What are the actual occupancy rates for different zip codes?

2.	Which zip codes tend to favor shorter or longer (nightly, weekly and monthly) rental periods and how that would affect the rental income?

3.	How does population density of the area affect property prices, rental prices, and occupancy rates?

4.	How does proximity to the attractions or ease of access to public transportation affect property prices, rental prices, rental periods, and occupancy rates?

5.	What is the effect of taxes, fees, and maintenance costs on the net rental income (net ROI) and how do these expenditures change with the change of property value and location? 

6.	The annual rate of property value appreciation was considered in this analysis. Additional data on average percent increase in rental prices for each zip code can provide more insights on short term rental real estate market trends and help to provide better predictions.

7.	Rental properties closer to the attractions and located in densely populated areas are expected to have higher values and higher rental prices. Properties located outside the densely populated areas in the proximity of public transportation stations can have much lower property values but be favorable for renters since they can offer lower rental prices. A deeper analysis is needed to investigate the effect of all these parameters on rental periods, occupancy rates, net rental income, and the ROI.
 


