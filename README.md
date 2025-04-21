## Introduction 
We utilize data which pertains to the major outages witnessed by different states in the continental U.S. during January 2000–July 2016. There are a total of 1534 total outages reported in the dataset. Further details regarding the raw data and its collection process can be found in the original published [data article](https://doi.org/10.1016/j.dib.2018.06.067). A description of the data columns is located in Table 1. 

### Table 1. Power Outage Column Descriptions
| Variable types                        | Variable names         | Description |
|--------------------------------------|------------------------|-------------|
| **GENERAL INFORMATION**              |                        |             |
| Time of the outage event             | YEAR                   | Indicates the year when the outage event occurred |
|                                      | MONTH                  | Indicates the month when the outage event occurred |
| Geographic areas                     | U.S._STATE             | Represents all the states in the continental U.S. |
|                                      | POSTAL.CODE            | Represents the postal code of the U.S. states |
|                                      | NERC.REGION            | The North American Electric Reliability Corporation (NERC) regions involved in the outage event |
| **REGIONAL CLIMATE INFORMATION**     |                        |             |
| U.S. Climate regions                 | CLIMATE.REGION         | U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.) |
| El Niño/La Niña                      | ANOMALY.LEVEL          | Oceanic El Niño/La Niña (ONI) index as a 3-month running mean of SST anomalies in the Niño 3.4 region |
|                                      | CLIMATE.CATEGORY       | “Warm”, “Cold”, or “Normal” episodes of the climate based on the ONI threshold of ± 0.5 °C |
| **OUTAGE EVENTS INFORMATION**        |                        |             |
| Event start and end information      | OUTAGE.START.DATE      | Day of the year when the outage event started |
|                                      | OUTAGE.START.TIME      | Time of the day when the outage event started |
|                                      | OUTAGE.RESTORATION.DATE| Day of the year when power was restored |
|                                      | OUTAGE.RESTORATION.TIME| Time of the day when power was restored |
| Cause of the event                   | CAUSE.CATEGORY         | Categories of events causing the outages |
|                                      | CAUSE.CATEGORY.DETAIL  | Detailed description of causes |
|                                      | HURRICANE.NAMES        | Name of the hurricane (if applicable) |
| Extent of outages                    | OUTAGE.DURATION        | Duration of outage events (in minutes) |
|                                      | DEMAND.LOSS.MW         | Amount of peak demand lost (in Megawatts) |
|                                      | CUSTOMERS.AFFECTED     | Number of customers affected |
| **REGIONAL ELECTRICITY CONSUMPTION INFORMATION** |              |             |
| Electricity price                    | RES.PRICE              | Residential sector price (cents/kWh) |
|                                      | COM.PRICE              | Commercial sector price (cents/kWh) |
|                                      | IND.PRICE              | Industrial sector price (cents/kWh) |
|                                      | TOTAL.PRICE            | Average electricity price in the state (cents/kWh) |
| Electricity consumption              | RES.SALES              | Residential sector consumption (MWh) |
|                                      | COM.SALES              | Commercial sector consumption (MWh) |
|                                      | IND.SALES              | Industrial sector consumption (MWh) |
|                                      | TOTAL.SALES            | Total consumption in the state (MWh) |
|                                      | RES.PERCEN             | Residential % of total consumption |
|                                      | COM.PERCEN             | Commercial % of total consumption |
|                                      | IND.PERCEN             | Industrial % of total consumption |
| Customers served                     | RES.CUSTOMERS          | Annual number of residential customers |
|                                      | COM.CUSTOMERS          | Annual number of commercial customers |
|                                      | IND.CUSTOMERS          | Annual number of industrial customers |
|                                      | TOTAL.CUSTOMERS        | Total annual number of customers |
|                                      | RES.CUST.PCT           | % of residential customers |
|                                      | COM.CUST.PCT           | % of commercial customers |
|                                      | IND.CUST.PCT           | % of industrial customers |
| **REGIONAL ECONOMIC CHARACTERISTICS**|                        |             |
| Economic outputs                     | PC.REALGSP.STATE       | Per capita real GSP in the state (2009 chained $) |
|                                      | PC.REALGSP.USA         | Per capita real GSP in the U.S. (2009 chained $) |
|                                      | PC.REALGSP.REL         | Relative per capita real GSP (state vs U.S.) |
|                                      | PC.REALGSP.CHANGE      | % change from previous year |
|                                      | UTIL.REALGSP           | Real GSP from utility industry |
|                                      | TOTAL.REALGSP          | Real GSP from all industries |
|                                      | UTIL.CONTRI            | Utility industry’s % of total GSP |
|                                      | PI.UTIL.OFUSA          | State utility income as % of U.S. utility income |
| **REGIONAL LAND-USE CHARACTERICS**   |                        |             |
| Population                           | POPULATION             | State population in a year |
|                                      | POPPCT_URBAN           | % of population in urban areas |
|                                      | POPPCT_UC              | % of population in urban clusters |
|                                      | POPDEN_URBAN           | Urban population density (persons/sq. mile) |
|                                      | POPDEN_UC              | Urban cluster density (persons/sq. mile) |
|                                      | POPDEN_RURAL           | Rural population density (persons/sq. mile) |
| Land area                            | AREAPCT_URBAN          | % of land area as urban |
|                                      | AREAPCT_UC             | % of land area as urban clusters |
|                                      | PCT_LAND               | % of U.S. land area in state |
|                                      | PCT_WATER_TOT          | % of U.S. water area in state |
|                                      | PCT_WATER_INLAND       | % of U.S. inland water area in state |

*Note: “NA” in the data file indicates that data was not available.*

We were initially interested in exploring the question: How are different socioeconomic areas affected by power outages?  (i.e. duration, severity, frequency, # of customers affected, cause, electricity price). However upon an exploration of the data we found that it would be difficult to isolate outages by socioeconomic area, this is because data is aggregated by state, which is too large of a lens through which to observe socioeconomic differences. From this we pivoted to exploring the ‘severity’ of an outage, in terms of its effect on medically vulnerable individuals. We understand that individuals living off of devices such as ventilators or CPAP machines there is limited backup battery. Many individuals rely on refrigerated medcation such as insulin, losing refrigeration power can cause these medicines to lose viability. Some elderly or heat-sensitive populations can face adverse when HVAC systems are out. In each of these scenarios after several hours of power outage individuals may need to be transported to an area with power or a medical center Therefore we aim to explore the question: **How do states with more urban or rural land area correlate with power outage duration?** 

## Data Cleaning and Exploratory Data Analysis
After extracting it from the downloaded excel, we store and manipulate the data within a Pandas dataframe. From the master dataframe we construct a new dataframe where we select our relevant columns and remove rows with null values. Data conversions are made to ensure numerical data can be used appropriately in subsequent steps.  

| semester    | Count |
|-------------|-------|
| Fall 2020   | 3     |
| Winter 2021 | 2     |
| Spring 2021 | 6     |
| Summer 2021 | 4     |
| Fall 2021   | 55    |

We begin by investigating the OUTAGE.DURATION column 

 <iframe
 src="assets/outage_dur_distr.html"
 frameborder="0"
 ></iframe>

This isn't a great depiction... the scale of the x-axis isn't very telling. Let's try to frame this on a more comprehensible scale. We create a new categorical variable, `OUTAGE.DURATION.DESCRIPTION` which places the values of `OUTAGE.DURATION` in the following bins aka our severity categories:

**Short-term (0–2 hours):** Minimal risk for most

**Moderate (2–4 hours):** Begins affecting medical equipment

**Critical (4–8 hours):** Medication spoilage risk, food safety, health risks escalate

**Severe (8+ hours):** Systemic risk to vulnerable populations, economic losses


 <iframe
 src="assets/outage_distr.html"
 frameborder="0"
 width=800
 height=800
 ></iframe>

### Bivariate Analysis 
Now we explore the `OUTAGE.DURATION.DESCRIPTION` relative to the other columns. We began with the `AREAPCT_...` columns because our research question focuses on the relation of these columns to outage duration. 


 <iframe
 src="assets/biv_outage_urb.html"
 frameborder="0"
 width=800
 height=800
 ></iframe>
 
 <iframe
 src="assets/outage_outage_uc.html"
 frameborder="0"
 width=800
 height=800
 ></iframe>

Note that the above columns explored were `AREAPCT_URBAN` and `AREAPCT_UC`, these are both percentages of land area within a state. We have created our own column `AREAPCT_RURAL` which is assumed to make up the remaining percantage of land area within the state.
<iframe
 src="assets/outage_outage_rural.html"
 frameborder="0"
 width=800
 height=800
 ></iframe>

## Framing a Prediction Problem
We seek to make a multiclass classification prediction problem. We are predicting on the column we have created earlier: `OUTAGE.DURATION.DESCRIPTION`. 
## Baseline Model 


Based on these results we want to create
## Final Model 