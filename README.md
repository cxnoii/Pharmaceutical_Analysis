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

### Calculate Quartiles, Find Outliers, and Create a Box Plot 
We can then calculate quartiles for the the four most promising treament regimens and identify any outliers that may be worth taking a look at. We can do this by taking the last time point for each mouse and grouping them by Mouse ID. Once each treatment is grouped by Mouse ID, we can calculate quartiles and print outliers. 

<p align="center">
<img width="1053" alt="Screenshot 2023-07-05 at 4 08 16 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/c625d3f6-4711-4c73-b8e8-33b21be6363f">
</p>

The preceeding code prints the following: 

Regimen: Capomulin\
The lower quartile of tumor volumes is: 32.37735684 \
The upper quartile of tumor volumes is: 40.1592203 \
The interquartile range of tumor volumes is: 7.781863460000004\
Values below 20.70456164999999 could be outliers. \
Values above 51.83201549 could be outliers.
No potential outliers! 

Regimen: Ramicane\ 
The lower quartile of tumor volumes is: 31.56046955 \
The upper quartile of tumor volumes is: 40.65900627 \
The interquartile range of tumor volumes is: 9.098536719999998 \
Values below 17.912664470000003 could be outliers. \
Values above 54.30681135 could be outliers. \
No potential outliers! 

Regimen: Propriva \
The lower quartile of tumor volumes is: 49.12296898 \
The upper quartile of tumor volumes is: 62.57087961 \
The interquartile range of tumor volumes is: 13.447910629999996 \
Values below 28.95110303500001 could be outliers. \
Values above 82.742745555 could be outliers. \
No potential outliers! \

Regimen: Ceftamin \
The lower quartile of tumor volumes is: 48.72207785 \
The upper quartile of tumor volumes is: 64.29983003 \
The interquartile range of tumor volumes is: 15.577752179999997 \
Values below 25.355449580000002 could be outliers. \
Values above 87.66645829999999 could be outliers. \
No potential outliers! 
    

### Create a Line Plot and a Scatter Plot

1. Select a mouse that was treated with Capomulin and generate a line plot of tumor volume vs. time point for that mouse.

2. Generate a scatter plot of tumor volume versus mouse weight for the Capomulin treatment regimen.

### Calculate Correlation and Regression

1. Calculate the correlation coefficient and linear regression model between mouse weight and average tumor volume for the Capomulin treatment. 

2. Plot the linear regression model on top of the previous scatter plot.

### Submit Your Final Analysis

Review all the figures and tables that you generated in this assignment. Write at least three observations or inferences that can be made from the data. Include these observations at the top of your notebook.

## Hints and Considerations

* Use the code comments in the provided starter file to guide you through this assignment. 

* Use proper labeling for your plots, that is, include plot titles, axis labels, legend labels, _x_-axis and _y_-axis limits, etc.

* While working on this assignment, refer to Stack Overflow and the Matplotlib documentation as needed. These are essential tools in every data analyst's tool belt.

* Remember that there are many ways to approach a data problem. One way to break up your task into micro tasks. For example, ask yourself questions like the following:

  * How does my DataFrame need to be structured in order to have the right _x_-axis and _y_-axis?

  * How do I build a basic scatter plot?

  * How do I add a label to a scatter plot?

  * Where in the DataFrame can I find the names that will go into the labels?


## References

Mockaroo, LLC. (2021). Realistic Data Generator. [https://www.mockaroo.com/](https://www.mockaroo.com/)

- - -

© 2022 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.

