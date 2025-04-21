## Introduction 
We utilize data which pertains to the major outages witnessed by different states in the continental U.S. during January 2000–July 2016. There are a total of 1534 total outages reported in the dataset. Further details regarding the raw data and its collection process can be found in the original published [data article](https://doi.org/10.1016/j.dib.2018.06.067). A description of the data columns is located in Table 1. 

### Table 1. Power Outage Column Descriptions
| Variable Name         | Description                                                                                                   |
|-----------------------|---------------------------------------------------------------------------------------------------------------|
| **OUTAGE EVENTS INFORMATION** |                                                                                                       |
| CAUSE.CATEGORY        | Categories of all the events causing the major power outages                                                  |
| OUTAGE.DURATION       | Duration of outage events (in minutes)                                                                        |
| CUSTOMERS.AFFECTED    | Number of customers affected by the power outage event                                                        |
| **REGIONAL LAND-USE CHARACTERISTICS** |                                                                                               |
| AREAPCT_URBAN         | Percentage of the land area of the U.S. state represented by the land area of the urban areas (in %)          |
| AREAPCT_UC            | Percentage of the land area of the U.S. state represented by the land area of the urban clusters (in %)       |
| **REGIONAL CLIMATE INFORMATION** |                                                                                                     |
| CLIMATE.REGION        | U.S. Climate regions as specified by National Centers for Environmental Information (9 consistent U.S. zones) |
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
 width=1000
 height=500
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