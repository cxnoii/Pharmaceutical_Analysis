## Background
The purpose of this project is to analyze a mock dataset of pharmaceutical anti-cancer treatments for sqamous cell carcinoma (SCC), a commonly occuring form of skin cancer. This analysis will compare the performance of Capomulin versus other treatment regimens listed in the data set. The dataset contains 249 mice identified with SCC tumor growth that were treated with a variety of drug regimens. Tumor development for these mice were observed and measured for 45 days.

## Data
The dataset contains the following metrics
* Mouse ID
* Timepoint: Time of observation ranging from 0-45 minutes
* Tumor Volume (mm3)
* Metastatic Sites
* Drug Regimen
* Sex
* Age (Months)
* Weight (g)

## Methods


### Data Preparation
1. The dataset was first read into a dataframe from a csv using Pandas.
<p align="center">
<img width="802" alt="Screenshot 2023-07-05 at 3 08 15 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/1c27160c-3fa4-4b12-875f-8e42d3f68683">
</p>



3. Before conducting any analysis, the dataset must be cleaned; duplicated rows will affect the integrity of the results.
<p align="center">
<img width="820" alt="Screenshot 2023-07-05 at 3 13 35 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/febbf9ee-0d98-4602-8c16-c3b77c0453c4">
</p>

Using pandas dataframe function, .duplicated, reveals that mouse_id g989 has duplicated rows that must be removed from the dataset.
<p align="center">
<img width="576" alt="Screenshot 2023-07-05 at 3 15 45 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/8146b385-8466-424f-bad3-1b0da66398d7">
</p>

With the dataset cleaned, accurate summary statistics can now be generated. 

### Summary Statistics
The purpose of this analysis is observe how effective each treatment is to treat SCC. The key metric of interest is Tumor Volume (mm3) and we can generate statistics for each treatment using pandas' groupby function, specifying the statistics that we want to observe: mean, median, variance, standard deviation, and standard error of mean. 
<p align="center">
<img width="969" alt="Screenshot 2023-07-05 at 3 25 43 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/c96fb93f-fca7-4fb8-ad0c-39ef64f540b5">
</p>

These values can then be sorted by a specific metric to find treatments of interest, here they are sorted my the average mean tumor size.
<p align="center">
<img width="510" alt="Screenshot 2023-07-05 at 3 43 18 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/38e84528-c22b-49d8-bc6e-43cbc6422699">
</p>

Based on the summary statistics, the four treatment regimens that yield the smallest average tumor size are Capomulin, Ceftamin, Propriva, and Ramincane.

### Quartiles, Outliers, and Boxplot
We can then calculate quartiles for the the four most promising treament regimens and identify any outliers that may be worth taking a look at. We can do this by taking the last time point for each mouse and grouping them by Mouse ID. Once each treatment is grouped by Mouse ID, we can calculate quartiles and print outliers. 

<p align="center">
<img width="1053" alt="Screenshot 2023-07-05 at 4 08 16 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/c625d3f6-4711-4c73-b8e8-33b21be6363f">
</p>

The preceeding code prints the following: 

Regimen: Capomulin\
The lower quartile of tumor volumes is: 32.377\
The upper quartile of tumor volumes is: 40.159\
The interquartile range of tumor volumes is: 7.782\
Values below 20.705 could be outliers.\
Values above 51.832 could be outliers.\
No potential outliers!

Regimen: Ramicane\
The lower quartile of tumor volumes is: 31.56\
The upper quartile of tumor volumes is: 40.659\
The interquartile range of tumor volumes is: 9.099\
Values below 17.913 could be outliers.\
Values above 54.307 could be outliers.\
No potential outliers!

Regimen: Propriva\
The lower quartile of tumor volumes is: 49.123\
The upper quartile of tumor volumes is: 62.571\
The interquartile range of tumor volumes is: 13.448\
Values below 28.951 could be outliers.\
Values above 82.743 could be outliers.\
No potential outliers!

Regimen: Ceftamin\
The lower quartile of tumor volumes is: 48.722\
The upper quartile of tumor volumes is: 64.3\
The interquartile range of tumor volumes is: 15.578\
Values below 25.355 could be outliers.\
Values above 87.666 could be outliers.\
No potential outliers!

Pandas .plot function was then used in order visualize the results.
<p align='center'>
<img width="584" alt="Screenshot 2023-07-07 at 1 07 37 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/42d1bf9e-ed37-4fec-8b0c-2e25610bf5a4">
</p>

The boxplot suggests that Capomulin remains the best treatment for SCC as the IQR of the Final Tumor Volumes is smaller than that of Ramicane. 

### A Closer Look at Tumor Volume and Weight
<p align='center'>
<img width="696" alt="Screenshot 2023-07-07 at 1 24 02 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/fb6edba6-bc5e-4792-9c9d-787cc8a8fa65">
</p>

<p align='center'>
<img width="629" alt="Screenshot 2023-07-07 at 1 24 13 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/d4782ea2-9662-407c-8a2b-9e5ebe0966b4">
</p>

The correlation coefficient has a value of 0.842
This suggests that there is a moderate correlation between average weight and tumor volume of mice treated with Capomulin.
This process was repeated with mice treated with Ramicane for comparison.

<p align='center'>
<img width="641" alt="Screenshot 2023-07-07 at 1 31 50 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/e8272728-0661-4057-82c2-e98e025907f7">
</p>

## Insights
Three Insights on Capomulin Treatment:
1. Capomulin is the best treatment in this sample at treating SCC; by observing the final tumor volumes, we see that mice treated with capomulin have significantly lower final tumor volumes than mice treated with the other drug regimen.
2. Mice treated with capomulin tend to lose weight as the tumor volume decreases; the correlation coefficient for Average Weight vs Average Tumor Volume is 0.842, suggesting that there is strong correlation between the two variables.
3. The Ramicane drug has similar results as the Capomulin treatment. Average Weight and Average Tumor Volume also have a correlation coefficient of 0.806, suggesting strong correlation between the two variables. This could imply that the two drugs work in similar fashion. Final average tumor volumes for the Ramicane treatment are significantly lower than the other two treatments in the sample.


## References

Mockaroo, LLC. (2021). Realistic Data Generator. [https://www.mockaroo.com/](https://www.mockaroo.com/)

- - -

© 2022 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.

