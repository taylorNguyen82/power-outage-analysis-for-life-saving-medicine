## Introduction 
We utilize data which pertains to the major outages witnessed by different states in the continental U.S. during January 2000–July 2016. There are a total of 1534 total outages reported in the dataset. Further details regarding the raw data and its collection process can be found in the original published [data article](https://doi.org/10.1016/j.dib.2018.06.067).  

We were initially interested in exploring the question: How are different socioeconomic areas affected by power outages?  (i.e. duration, severity, frequency, # of customers affected, cause, electricity price). However upon an exploration of the data we found that it would be difficult to isolate outages by socioeconomic area, this is because data is aggregated by state, which is too large of a lens through which to observe socioeconomic differences. From this we pivoted to exploring the ‘severity’ of an outage, in terms of its effect on medically vulnerable individuals. We understand that individuals living off of devices such as ventilators or CPAP machines there is limited backup battery. Many individuals rely on refrigerated medcation such as insulin, losing refrigeration power can cause these medicines to lose viability. Some elderly or heat-sensitive populations can face adverse when HVAC systems are out. In each of these scenarios after several hours of power outage individuals may need to be transported to an area with power or a medical center Therefore we aim to explore the question: **How do states with more urban or rural land area correlate with power outage duration?** 

### Table 1. Power Outage Column Descriptions
<iframe
 src="assets/table1.html"
 frameborder="0"
 width="800"
 height="600"
 ></iframe>
*Note: “NA” in the data file indicates that data was not available.*

## Data Cleaning and Exploratory Data Analysis
After extracting it from the downloaded excel, we store and manipulate the data within a Pandas dataframe. From the master dataframe we construct a new dataframe where we select our relevant columns and remove rows with null values. Data conversions are made to ensure numerical data can be used appropriately in subsequent steps.  

| semester    | Count |
|-------------|-------|
| Fall 2020   | 3     |
| Winter 2021 | 2     |
| Spring 2021 | 6     |
| Summer 2021 | 4     |
| Fall 2021   |  |

We begin by investigating the `OUTAGE.DURATION` column 

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

We decided to use a decision tree classifier for our baseline model. We used the features `AREAPCT_URBAN`, `AREAPCT_RURAL` (quantitative variables representing the percentage of area that is urban and rural)   in order to find our target OUTAGE.DURATION.DESCRIPTION, which describes the severity of power outages (Critical, Minimal, Moderate, Severe). 
here were:


In short, we had: 

2 quantitative features: AREAPCT_URBAN, AREAPCT_RURAL

0 ordinal features

0 nominal (categorical) features

Since both features are numeric and continuous, we  applied standard scaling using StandardScaler to normalize their values before feeding them into the classifier. No encoding was needed since there were not any categorical variables. 
We split the data into training and test sets using an 80/20 ratio, and trained the decision tree with default hyperparameters (except setting random_state=42 for reproducibility). The pipeline had a preprocessing step as well. 
Below is a summary of the model’s performance: 
Weighted Average: 61%


Overall, while the weighted average  may seem moderate, the model is not yet good because it suffers from significant class imbalance and poor performance on minority classes. This indicates the model is biased toward the dominant class (‘Severe’), and may not generalize well in more balanced or real-world situations.

## Final Model 

To enhance the predictive performance of the model, we added several features. 

Features: 
FunctionTransfomer: log_customers: We  transformed the CUSTOMERS.AFFECTED variable using a log transformation (log1p). This stabilizes variance and reduces the skewness from extreme outliers (e.g., major outages affecting thousands of customers), enabling the model to better differentiate between typical and large-scale events without being overwhelmed. 

StandardScaler: We applied standardization to our numerical features using StandardScaler within our pipeline. This step ensures that all numeric input variables are transformed to have a mean of 0 and a standard deviation of 1, enabling the model to treat all features on a comparable scale.



Categorical Features: 
CAUSE.CATEGORY: The cause of an outage (e.g., severe weather, fuel supply emergency, equipment failure )) is relevant to its expected duration. This was included using one-hot encoding to retain categorical distinctions without creating ordinal bias.
CLIMATE.REGION: Different climate zones (e.g., Southeast vs. Northwest) are subject to distinct weather events—like hurricanes, snowstorms, or droughts—that can significantly impact both the frequency and severity of outage. The CLIMATE.REGION column was processed using OneHotEncoder within the pipeline, enabling the model to leverage it without introducing ordinal bias while maintaining compatibility with cross-validation.


We selected a Random Forest Classifier for its robustness to overfitting, ability to handle both numerical and categorical data, and strong performance on imbalanced classification problems when paired with class weighting. To fine-tune our model, we used GridSearchCV to explore different combinations of hyperparameters for the RandomForestClassifier. Specifically, we varied:

n_estimators (number of trees): 50, 100, 200

max_depth (maximum depth of the trees): 5, 10, 20, and None (unlimited)

min_samples_split (minimum samples to split a node): 2, 5, 10

The grid search used 5-fold cross-validation to evaluate the performance of each combination.

The best-performing combination was:

n_estimators = 100

max_depth = 10

min_samples_split = 2

This configuration offered a good balance between model complexity and generalization. A moderate depth (max_depth = 10) helped prevent overfitting, while using 100 trees ensured stability in predictions without making the model too slow or heavy.

Compared to the Baseline Model (a simple Decision Tree), the tuned Random Forest showed notable improvements in the weighted average. It increased from 61% to 72%, highlighting better generalization across all duration types, not just the dominant “Severe” class.