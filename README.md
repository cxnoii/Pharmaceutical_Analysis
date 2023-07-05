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

1. Calculate the final tumor volume of each mouse across four of the most promising treatment regimens: Capomulin, Ramicane, Infubinol, and Ceftamin. Then, calculate the quartiles and IQR and determine if there are any potential outliers across all four treatment regimens. Follow these substeps:

    * Create a grouped DataFrame that shows the last (greatest) time point for each mouse. Merge this grouped DataFrame with the original cleaned DataFrame.

    * Create a list that holds the treatment names, as well as a second, empty list to hold the tumor volume data.

    * Loop through each drug in the treatment list, locating the rows in the merged DataFrame that correspond to each treatment. Append the resulting final tumor volumes for each drug to the empty list. 

    * Determine outliers by using the upper and lower bounds, and then print the results.
    
2. Using Matplotlib, generate a box plot of the final tumor volume for all four treatment regimens. Highlight any potential outliers in the plot by changing their color and style.

  **Hint**: All four box plots should be within the same figure. Use this [Matplotlib documentation page](https://matplotlib.org/gallery/pyplots/boxplot_demo_pyplot.html#sphx-glr-gallery-pyplots-boxplot-demo-pyplot-py) for help with changing the style of the outliers.

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

