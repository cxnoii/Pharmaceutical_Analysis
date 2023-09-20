
## Background
The purpose of this project is to analyze a mock dataset of pharmaceutical anti-cancer treatments for sqamous cell carcinoma (SCC), a commonly occuring form of skin cancer, using Python. This analysis will compare the performance of Capomulin versus other treatment regimens listed in the data set. The dataset contains 249 mice identified with SCC tumor growth that were treated with a variety of drug regimens. Tumor development for these mice were observed and measured for 45 days.

<p align="center">
<img width="255" alt="Screenshot 2023-07-07 at 1 39 22 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/8c5ec445-a7e3-4003-80fd-f8b5b0f8a8a7">
</p>

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
```python
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import scipy.stats as st

# Declaring file paths
mouse_metadata_path = "data/Mouse_metadata.csv"
study_results_path = "data/Study_results.csv"

# Reads in mouse data and study results
mouse_metadata = pd.read_csv(mouse_metadata_path)
study_results = pd.read_csv(study_results_path)

# Combines data into single data set
mouse_results = pd.merge(study_results, mouse_metadata)

# Display the data table for preview
mouse_results.head()
```
| x | Mouse ID   |   Timepoint |   Tumor Volume (mm3) |   Metastatic Sites | Drug Regimen   | Sex    |   Age_months |   Weight (g) |
|---:|:-----------|------------:|---------------------:|-------------------:|:---------------|:-------|-------------:|-------------:|
|  0 | b128       |           0 |              45      |                  0 | Capomulin      | Female |            9 |           22 |
|  1 | b128       |           5 |              45.6513 |                  0 | Capomulin      | Female |            9 |           22 |
|  2 | b128       |          10 |              43.2709 |                  0 | Capomulin      | Female |            9 |           22 |
|  3 | b128       |          15 |              43.7849 |                  0 | Capomulin      | Female |            9 |           22 |
|  4 | b128       |          20 |              42.7316 |                  0 | Capomulin      | Female |            9 |           22 |


2. Before conducting any analysis, the dataset must be cleaned; duplicated rows will affect the integrity of the results. The function duplicated returns an array with mouse_id g898, indicating that it is a duplicate. 

```python
# Getting the duplicate mice by ID number that shows up for Mouse ID and Timepoint. 
dupe_id = mouse_results.loc[mouse_results.duplicated(subset=['Mouse ID', 'Timepoint']), 'Mouse ID'].unique()
dupe_id

output:
array(['g989'], dtype=object)
```

The following code will create a dataframe without the duplicated record. Accuracte summaracy statistics can now be generated. 
```python
# Creates a clean dataframe with duplicate mouse removed. 
#This dateframe has just one entry for each time point for each mouse_id
clean_results_df = mouse_results.drop_duplicates(['Mouse ID','Timepoint'])
clean_results_df
```

### Summary Statistics
The purpose of this analysis is observe how effective each treatment is to treat SCC. The key metric of interest is Tumor Volume (mm3) and we can generate statistics for each treatment using pandas' groupby function, specifying the statistics that we want to observe: mean, median, variance, standard deviation, and standard error of mean. 

```python
# Generates a summary statistics table
tumor_mean = clean_results_df['Tumor Volume (mm3)'].groupby(clean_results_df['Drug Regimen']).mean()
tumor_median = clean_results_df['Tumor Volume (mm3)'].groupby(clean_results_df['Drug Regimen']).median()
tumor_var = clean_results_df['Tumor Volume (mm3)'].groupby(clean_results_df['Drug Regimen']).var()
tumor_std = clean_results_df['Tumor Volume (mm3)'].groupby(clean_results_df['Drug Regimen']).std()
tumor_sem = clean_results_df['Tumor Volume (mm3)'].groupby(clean_results_df['Drug Regimen']).sem()

# Assemble the resulting series into a single summary dataframe.
summary_table = pd.DataFrame({'Mean Tumor Volume': tumor_mean
,'Median Tumor Volume' : tumor_median
,'Tumor Volume Variance' : tumor_var
,'Tumor Volume Standard Deviation' : tumor_std
,"Tumor Volume Standard Error" : tumor_sem
})

summary_table
```

| Drug Regimen   |   Mean Tumor Volume |   Median Tumor Volume |   Tumor Volume Variance |   Tumor Volume Standard Deviation |   Tumor Volume Standard Error |
|:---------------|--------------------:|----------------------:|------------------------:|----------------------------------:|------------------------------:|
| Capomulin      |             40.6757 |               41.5578 |                 24.9478 |                           4.99477 |                      0.329346 |
| Ceftamin       |             52.5912 |               51.7762 |                 39.2902 |                           6.26819 |                      0.469821 |
| Infubinol      |             52.8848 |               51.8206 |                 43.1287 |                           6.56724 |                      0.492236 |
| Ketapril       |             55.2356 |               53.6987 |                 68.5536 |                           8.27971 |                      0.60386  |
| Naftisol       |             54.3316 |               52.5093 |                 66.1735 |                           8.13471 |                      0.596466 |
| Placebo        |             54.0336 |               52.2889 |                 61.1681 |                           7.821   |                      0.581331 |
| Propriva       |             52.3935 |               50.91   |                 43.1388 |                           6.56801 |                      0.525862 |
| Ramicane       |             40.2167 |               40.6732 |                 23.4867 |                           4.84631 |                      0.320955 |
| Stelasyn       |             54.2331 |               52.4317 |                 59.4506 |                           7.71042 |                      0.573111 |
| Zoniferol      |             53.2365 |               51.8185 |                 48.5334 |                           6.96659 |                      0.516398 |

These values are then sorted by a specific metric to find treatments of interest. In this case, we are interested in the average tumor size of each mice undergoing certain drug regimens.
```python
# Producing summary statistics table
summary_table2 = clean_results_df['Tumor Volume (mm3)'].groupby(clean_results_df['Drug Regimen']).aggregate(['mean','median','var', 'std','sem'])
summary_table2.sort_values(by="mean")
```

| Drug Regimen   |    mean |   median |     var |     std |      sem |
|:---------------|--------:|---------:|--------:|--------:|---------:|
| Ramicane       | 40.2167 |  40.6732 | 23.4867 | 4.84631 | 0.320955 |
| Capomulin      | 40.6757 |  41.5578 | 24.9478 | 4.99477 | 0.329346 |
| Propriva       | 52.3935 |  50.91   | 43.1388 | 6.56801 | 0.525862 |
| Ceftamin       | 52.5912 |  51.7762 | 39.2902 | 6.26819 | 0.469821 |
| Infubinol      | 52.8848 |  51.8206 | 43.1287 | 6.56724 | 0.492236 |
| Zoniferol      | 53.2365 |  51.8185 | 48.5334 | 6.96659 | 0.516398 |
| Placebo        | 54.0336 |  52.2889 | 61.1681 | 7.821   | 0.581331 |
| Stelasyn       | 54.2331 |  52.4317 | 59.4506 | 7.71042 | 0.573111 |
| Naftisol       | 54.3316 |  52.5093 | 66.1735 | 8.13471 | 0.596466 |
| Ketapril       | 55.2356 |  53.6987 | 68.5536 | 8.27971 | 0.60386  |

Based on the summary statistics, the four treatment regimens that yield the smallest average tumor size are Ramicane, Capomulin, Propriva, and Ceftamin.

### Quartiles, Outliers, and Boxplot
We can then calculate quartiles for the the four most promising treament regimens and identify any outliers that may be worth taking a look at. We can do this by taking the last time point for each mouse and grouping them by Mouse ID. Once each treatment is grouped by Mouse ID, we can calculate quartiles and print outliers. 

```python
# Put treatments into a list for for loop (and later for plot labels)
treatments =  ['Capomulin', 'Ramicane', 'Propriva', 'Ceftamin']
potential_outliers = 0

# Calculate the IQR and quantitatively determine if there are any potential outliers. 
for regimen in treatments:
    regimen_df = regimen_tumor_max[regimen_tumor_max['Drug Regimen'] == regimen]

    quartiles = regimen_df['Tumor Volume (mm3)'].quantile([.25,.5,.75])
    lowerq = quartiles[0.25]
    upperq = quartiles[0.75]
    iqr = upperq-lowerq

    print(f'Regimen: {regimen}')
    print(f"The lower quartile of tumor volumes is: {lowerq.round(3)}")
    print(f"The upper quartile of tumor volumes is: {upperq.round(3)}")
    print(f"The interquartile range of tumor volumes is: {iqr.round(3)}")

    lower_bound = lowerq - (1.5*iqr)
    upper_bound = upperq + (1.5*iqr)
    print(f"Values below {lower_bound.round(3)} could be outliers.")
    print(f"Values above {upper_bound.round(3)} could be outliers.")

    potential_outliers = regimen_df.loc[(regimen_df['Tumor Volume (mm3)'] >= upper_bound) | (regimen_df['Tumor Volume (mm3)'] <= lower_bound)]
    if potential_outliers.empty:
        print("No potential outliers!")
    else:
        print(f"{regimen}'s potential outliers:")
        print(potential_outliers[['Mouse ID','Tumor Volume (mm3)']])
        print('\n')

```

output:\
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

Plotting the final tumor volumes 
<p align='center'>
<img width="584" alt="Screenshot 2023-07-07 at 1 07 37 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/42d1bf9e-ed37-4fec-8b0c-2e25610bf5a4">
</p>

The boxplot suggests that Capomulin remains the best treatment for SCC as the IQR of the Final Tumor Volumes is smaller than that of Ramicane. 

### A Closer Look at Tumor Volume and Weight
The correlation between Tumor Volume and Weight was analyzed as it was observed that weight was dereasing as treatment time proceeded. In order to verify if there is a relationship between these two variables, a linear regression model was created for mice treated with Capomulin, the most effective treatment.
<p align='center'>
<img width="696" alt="Screenshot 2023-07-07 at 1 24 02 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/fb6edba6-bc5e-4792-9c9d-787cc8a8fa65">
</p>

<p align='center'>
<img width="629" alt="Screenshot 2023-07-07 at 1 24 13 PM" src="https://github.com/cxnoii/pymaceuticals/assets/114107454/d4782ea2-9662-407c-8a2b-9e5ebe0966b4">
</p>

The correlation coefficient has a value of 0.842.\
This suggests that there is a moderate correlation between average weight and tumor volume of mice treated with Capomulin.\
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

