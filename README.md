## Introduction 
We utilize data which pertains to the major outages witnessed by different states in the continental U.S. during January 2000–July 2016. There are a total of 1534 total outages reported in the dataset. Further details regarding the raw data and its collection process can be found in the original published [data article](https://doi.org/10.1016/j.dib.2018.06.067).  

We were initially interested in exploring the question: How are different socioeconomic areas affected by power outages?  (i.e. duration, severity, frequency, # of customers affected, cause, electricity price). However upon an exploration of the data we found that it would be difficult to isolate outages by socioeconomic area, this is because data is aggregated by state, which is too large of a lens through which to observe socioeconomic differences. From this we pivoted to exploring the ‘severity’ of an outage, in terms of its effect on medically vulnerable individuals. We understand that individuals living off of devices such as ventilators or CPAP machines there is limited backup battery. Many individuals rely on refrigerated medcation such as insulin, losing refrigeration power can cause these medicines to lose viability. Some elderly or heat-sensitive populations can face adverse when HVAC systems are out. In each of these scenarios after several hours of power outage individuals may need to be transported to an area with power or a medical center Therefore we aim to explore the question: **How do states with more urban or rural land area correlate with power outage duration?** 

### Table 1. Power Outage Column Descriptions

<table>
  <thead>
    <tr>
      <th>Variable Name</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan="2"><strong>OUTAGE EVENTS INFORMATION</strong></td>
    </tr>
    <tr>
      <td>CAUSE.CATEGORY</td>
      <td>Categories of all the events causing the major power outages</td>
    </tr>
    <tr>
      <td>OUTAGE.DURATION</td>
      <td>Duration of outage events (in minutes)</td>
    </tr>
    <tr>
      <td>CUSTOMERS.AFFECTED</td>
      <td>Number of customers affected by the power outage event</td>
    </tr>
    <tr>
      <td colspan="2"><strong>REGIONAL LAND-USE CHARACTERISTICS</strong></td>
    </tr>
    <tr>
      <td>AREAPCT_URBAN</td>
      <td>Percentage of the land area of the U.S. state represented by the land area of the urban areas (in %)</td>
    </tr>
    <tr>
      <td>AREAPCT_UC</td>
      <td>Percentage of the land area of the U.S. state represented by the land area of the urban clusters (in %)</td>
    </tr>
    <tr>
      <td colspan="2"><strong>REGIONAL CLIMATE INFORMATION</strong></td>
    </tr>
    <tr>
      <td>CLIMATE.REGION</td>
      <td>U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)</td>
    </tr>
  </tbody>
</table>
*Note: “NA” in the data file indicates that data was not available.*

## Data Cleaning and Exploratory Data Analysis
After extracting it from the downloaded excel, we store and manipulate the data within a Pandas dataframe. From the master dataframe we construct a new dataframe where we select our relevant columns and remove rows with null values. Data conversions are made to ensure numerical data can be used appropriately in subsequent steps.  

| semester    | Count |
|-------------|-------|
| Fall 2020   | 3     |
| Winter 2021 | 2     |
| Spring 2021 | 6     |
| Summer 2021 | 4     |
| Fall 2021   | U.S. Climate regions as specified by National Centers for Environmental Information (9 consistent U.S. zones)   |

We begin by investigating the OUTAGE.DURATION column 

 <iframe
 src="assets/outage_dur_distr.html"
 frameborder="0"
 width="800"
 height="400"
 ></iframe>

This isn't a great depiction... the scale of the x-axis isn't very telling. Let's try to frame this on a more comprehensible scale. We create a new categorical variable, `OUTAGE.DURATION.DESCRIPTION` which places the values of `OUTAGE.DURATION` in the following bins aka our severity categories:

**Short-term (0–2 hours):** Minimal risk for most

**Moderate (2–4 hours):** Begins affecting medical equipment

**Critical (4–8 hours):** Medication spoilage risk, food safety, health risks escalate

**Severe (8+ hours):** Systemic risk to vulnerable populations, economic losses


 <iframe
 src="assets/outage_distr.html"
 frameborder="0"
 width="800"
 height="400"
 ></iframe>

### Bivariate Analysis 
Now we explore the `OUTAGE.DURATION.DESCRIPTION` relative to the other columns. We began with the `AREAPCT_...` columns because our research question focuses on the relation of these columns to outage duration. 


 <iframe
 src="assets/biv_outage_urb.html"
 frameborder="0"
 width="800"
 height="400"
 ></iframe>
 
 <iframe
 src="assets/outage_outage_uc.html"
 frameborder="0"
 width="800"
 height="400"
 ></iframe>

Note that the above columns explored were `AREAPCT_URBAN` and `AREAPCT_UC`, these are both percentages of land area within a state. We have created our own column `AREAPCT_RURAL` which is assumed to make up the remaining percantage of land area within the state.

<iframe
 src="assets/outage_outage_rural.html"
 frameborder="0"
 width="800"
 height="400"
 ></iframe>

## Framing a Prediction Problem
We seek to make a multiclass classification prediction problem. We are predicting on the column we have created earlier: `OUTAGE.DURATION.DESCRIPTION`. 
We chose weighted average as a way to evaluate our model for this multi-class classification problem. Weighted average  calculates the average of the per-class scores, but it weights each class's contribution by the number of samples in that class. Our target variable (OUTAGE.DURATION.DESCRIPTION) is multi-class and imbalanced—some outage duration categories are far more frequent than others. A macro average would treat all classes equally, regardless of size, potentially giving too much influence to rare categories. In contrast, weighted average accounts for class frequency, giving more influence to the performance on more common classes. While it still includes all classes, weighted average provides a middle ground between favoring dominant classes (as micro average might) and overemphasizing rare ones (as macro average might). This ensures our model’s precision isn’t unfairly penalized for not predicting  rare events.

## Baseline Model 


Based on these results we want to create
## Final Model 