## Introduction 
We utilize data which pertains to the major outages witnessed by different states in the continental U.S. during January 2000–July 2016. There are a total of 1534 total outages reported in the dataset. Further details regarding the raw data and its collection process can be found in the original published data article: https://doi.org/10.1016/j.dib.2018.06.067. A description of the data columns is located in Table 1. 
Table 1. 
We were initially interested in exploring the question: How are different socioeconomic areas affected by power outages?  (i.e. duration, severity, frequency, # of customers affected, cause, electricity price). However upon an exploration of the data we found that it would be difficult to isolate outages by socioeconomic area, this is because data is aggregated by state, which is too large of a lens through which to observe socioeconomic differences. From this we pivoted to exploring the ‘severity’ of an outage, in terms of its effect on medically vulnerable individuals. Therefore we aim to explore : 

## Data Exploration 
After extracting it from the downloaded excel, we store and manipulate the data within a Pandas dataframe. From the master dataframe we construct a new dataframe where we select our relevant columns and remove rows with null values. Data conversions are made to ensure numerical data can be used appropriately in subsequent steps.  
[INSERT HEAD OF CLEANED DATA]

We begin by investigating the OUTAGE.DURATION column 

 <iframe
 src="assets/outage_dur_distr.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
 
This isn't a great depiction... the scale of the x-axis isn't very telling. Let's try to frame this on a more comprehensible scale. We create a new categorical variable, OUTAGE.DURATION.DESCRIPTION which places the values of OUTAGE.DURATION in the following bins aka our severity categories:
Short-term (0–2 hours): Minimal risk for most

Moderate (2–4 hours): Begins affecting medical equipment

Critical (4–8 hours): Medication spoilage risk, food safety, health risks escalate

Severe (8+ hours): Systemic risk to vulnerable populations, economic losses



## Baseline Model 

## Final Model 