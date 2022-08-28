# Google Data Analytics Certificate

## Capstone Project: Case study II


            




## Introduction

Bellabeat is a high-tech manufacturer of health-focused products for women. Urška Sršen and Sando Mur are the founder of the company. Sršen used her background as an artist to develop beautifully designed technology that informs and inspires women around the world. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their own health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women.


## Ask

> **Objective of the Report**

Analyzing smart device usage data in order to gain insight into how consumers use non-Bellabeat smart devices. Select one Bellabeat product to apply these insights.

What to observe from the insight:

- What are some trends in smart device usage?
- How could these trends apply to Bellabeat customers?
- How could these trends help influence Bellabeat marketing strategy?


## Prepare

> **Data Used**

The data used for this study was downloaded from kaggle. The data contains personal fitness tracker from thirty fitbit users. Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes information about daily activity, steps, and heart rate that can be used to explore users’ habits.

> **Data Characteristics**

1. The data was downloaded as a zip file which contains many csv files on different personal tracker data. Each csv file contain a long formated data.
2. The data was only collected for 33 user with unique ID.
3. After sorting the data, we saw that the it will answer the report objectives.
3. One of the dataset (Sleep data) have some missing users.

## Process

> Tools used : Python and Ms excel



#### Importation of needed Libraries

The imported libraries are:

- Pandas for data manipulation and cleaning
- Numpy for calclation
- Matplotlib and seaborn for visualization.
- Datetime to deal with time variables in the data.


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import datetime as dt
```

### Dataset importaion

The dataset needed for the report were imported as csv file. The analysis focus more on daily dataset. Hourly sleep data was used also in the process.


```python
#Import daily activities to explore the features
daily_act = pd.read_csv("dailyActivity_merged.csv")
daily_cal = pd.read_csv("dailyCalories_merged.csv")
daily_int = pd.read_csv("dailyIntensities_merged.csv")
daily_stp = pd.read_csv("dailySteps_merged.csv")
hb = pd.read_csv("heartrate_seconds_merged.csv")
sleep = pd.read_csv("sleepDay_merged.csv")
weight = pd.read_csv("weightLogInfo_merged.csv")
hourly_stp = pd.read_csv("hourlySteps_merged.csv")
```

#### Data Wrangling

The dataset was preview and it was discovered that, all the important variable were already combine inside the dailyActivity_merged. We checked for the data type, shape, missing values, and summarization of the data characteristics.


```python
#Checking the variable names of some dataset
daily_act.columns , sleep.columns, daily_cal.columns
```




    (Index(['Id', 'ActivityDate', 'TotalSteps', 'TotalDistance', 'TrackerDistance',
            'LoggedActivitiesDistance', 'VeryActiveDistance',
            'ModeratelyActiveDistance', 'LightActiveDistance',
            'SedentaryActiveDistance', 'VeryActiveMinutes', 'FairlyActiveMinutes',
            'LightlyActiveMinutes', 'SedentaryMinutes', 'Calories'],
           dtype='object'),
     Index(['Id', 'SleepDay', 'TotalSleepRecords', 'TotalMinutesAsleep',
            'TotalTimeInBed'],
           dtype='object'),
     Index(['Id', 'ActivityDay', 'Calories'], dtype='object'))




```python
daily = daily_act.copy()
```


```python
#Checking the data in each variables
daily_act.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>ActivityDate</th>
      <th>TotalSteps</th>
      <th>TotalDistance</th>
      <th>TrackerDistance</th>
      <th>LoggedActivitiesDistance</th>
      <th>VeryActiveDistance</th>
      <th>ModeratelyActiveDistance</th>
      <th>LightActiveDistance</th>
      <th>SedentaryActiveDistance</th>
      <th>VeryActiveMinutes</th>
      <th>FairlyActiveMinutes</th>
      <th>LightlyActiveMinutes</th>
      <th>SedentaryMinutes</th>
      <th>Calories</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1503960366</td>
      <td>4/12/2016</td>
      <td>13162</td>
      <td>8.50</td>
      <td>8.50</td>
      <td>0.0</td>
      <td>1.88</td>
      <td>0.55</td>
      <td>6.06</td>
      <td>0.0</td>
      <td>25</td>
      <td>13</td>
      <td>328</td>
      <td>728</td>
      <td>1985</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1503960366</td>
      <td>4/13/2016</td>
      <td>10735</td>
      <td>6.97</td>
      <td>6.97</td>
      <td>0.0</td>
      <td>1.57</td>
      <td>0.69</td>
      <td>4.71</td>
      <td>0.0</td>
      <td>21</td>
      <td>19</td>
      <td>217</td>
      <td>776</td>
      <td>1797</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1503960366</td>
      <td>4/14/2016</td>
      <td>10460</td>
      <td>6.74</td>
      <td>6.74</td>
      <td>0.0</td>
      <td>2.44</td>
      <td>0.40</td>
      <td>3.91</td>
      <td>0.0</td>
      <td>30</td>
      <td>11</td>
      <td>181</td>
      <td>1218</td>
      <td>1776</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1503960366</td>
      <td>4/15/2016</td>
      <td>9762</td>
      <td>6.28</td>
      <td>6.28</td>
      <td>0.0</td>
      <td>2.14</td>
      <td>1.26</td>
      <td>2.83</td>
      <td>0.0</td>
      <td>29</td>
      <td>34</td>
      <td>209</td>
      <td>726</td>
      <td>1745</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1503960366</td>
      <td>4/16/2016</td>
      <td>12669</td>
      <td>8.16</td>
      <td>8.16</td>
      <td>0.0</td>
      <td>2.71</td>
      <td>0.41</td>
      <td>5.04</td>
      <td>0.0</td>
      <td>36</td>
      <td>10</td>
      <td>221</td>
      <td>773</td>
      <td>1863</td>
    </tr>
  </tbody>
</table>
</div>




```python
daily_act.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 940 entries, 0 to 939
    Data columns (total 15 columns):
     #   Column                    Non-Null Count  Dtype  
    ---  ------                    --------------  -----  
     0   Id                        940 non-null    int64  
     1   ActivityDate              940 non-null    object 
     2   TotalSteps                940 non-null    int64  
     3   TotalDistance             940 non-null    float64
     4   TrackerDistance           940 non-null    float64
     5   LoggedActivitiesDistance  940 non-null    float64
     6   VeryActiveDistance        940 non-null    float64
     7   ModeratelyActiveDistance  940 non-null    float64
     8   LightActiveDistance       940 non-null    float64
     9   SedentaryActiveDistance   940 non-null    float64
     10  VeryActiveMinutes         940 non-null    int64  
     11  FairlyActiveMinutes       940 non-null    int64  
     12  LightlyActiveMinutes      940 non-null    int64  
     13  SedentaryMinutes          940 non-null    int64  
     14  Calories                  940 non-null    int64  
    dtypes: float64(7), int64(7), object(1)
    memory usage: 110.3+ KB
    


```python
#Checking for missing value
daily_act.isnull().sum()
```




    Id                          0
    ActivityDate                0
    TotalSteps                  0
    TotalDistance               0
    TrackerDistance             0
    LoggedActivitiesDistance    0
    VeryActiveDistance          0
    ModeratelyActiveDistance    0
    LightActiveDistance         0
    SedentaryActiveDistance     0
    VeryActiveMinutes           0
    FairlyActiveMinutes         0
    LightlyActiveMinutes        0
    SedentaryMinutes            0
    Calories                    0
    dtype: int64




```python
#Discriptive statistics
daily_act.describe().T
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Id</th>
      <td>940.0</td>
      <td>4.855407e+09</td>
      <td>2.424805e+09</td>
      <td>1.503960e+09</td>
      <td>2.320127e+09</td>
      <td>4.445115e+09</td>
      <td>6.962181e+09</td>
      <td>8.877689e+09</td>
    </tr>
    <tr>
      <th>TotalSteps</th>
      <td>940.0</td>
      <td>7.637911e+03</td>
      <td>5.087151e+03</td>
      <td>0.000000e+00</td>
      <td>3.789750e+03</td>
      <td>7.405500e+03</td>
      <td>1.072700e+04</td>
      <td>3.601900e+04</td>
    </tr>
    <tr>
      <th>TotalDistance</th>
      <td>940.0</td>
      <td>5.489702e+00</td>
      <td>3.924606e+00</td>
      <td>0.000000e+00</td>
      <td>2.620000e+00</td>
      <td>5.245000e+00</td>
      <td>7.712500e+00</td>
      <td>2.803000e+01</td>
    </tr>
    <tr>
      <th>TrackerDistance</th>
      <td>940.0</td>
      <td>5.475351e+00</td>
      <td>3.907276e+00</td>
      <td>0.000000e+00</td>
      <td>2.620000e+00</td>
      <td>5.245000e+00</td>
      <td>7.710000e+00</td>
      <td>2.803000e+01</td>
    </tr>
    <tr>
      <th>LoggedActivitiesDistance</th>
      <td>940.0</td>
      <td>1.081709e-01</td>
      <td>6.198965e-01</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>4.942142e+00</td>
    </tr>
    <tr>
      <th>VeryActiveDistance</th>
      <td>940.0</td>
      <td>1.502681e+00</td>
      <td>2.658941e+00</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>2.100000e-01</td>
      <td>2.052500e+00</td>
      <td>2.192000e+01</td>
    </tr>
    <tr>
      <th>ModeratelyActiveDistance</th>
      <td>940.0</td>
      <td>5.675426e-01</td>
      <td>8.835803e-01</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>2.400000e-01</td>
      <td>8.000000e-01</td>
      <td>6.480000e+00</td>
    </tr>
    <tr>
      <th>LightActiveDistance</th>
      <td>940.0</td>
      <td>3.340819e+00</td>
      <td>2.040655e+00</td>
      <td>0.000000e+00</td>
      <td>1.945000e+00</td>
      <td>3.365000e+00</td>
      <td>4.782500e+00</td>
      <td>1.071000e+01</td>
    </tr>
    <tr>
      <th>SedentaryActiveDistance</th>
      <td>940.0</td>
      <td>1.606383e-03</td>
      <td>7.346176e-03</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>1.100000e-01</td>
    </tr>
    <tr>
      <th>VeryActiveMinutes</th>
      <td>940.0</td>
      <td>2.116489e+01</td>
      <td>3.284480e+01</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>4.000000e+00</td>
      <td>3.200000e+01</td>
      <td>2.100000e+02</td>
    </tr>
    <tr>
      <th>FairlyActiveMinutes</th>
      <td>940.0</td>
      <td>1.356489e+01</td>
      <td>1.998740e+01</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>6.000000e+00</td>
      <td>1.900000e+01</td>
      <td>1.430000e+02</td>
    </tr>
    <tr>
      <th>LightlyActiveMinutes</th>
      <td>940.0</td>
      <td>1.928128e+02</td>
      <td>1.091747e+02</td>
      <td>0.000000e+00</td>
      <td>1.270000e+02</td>
      <td>1.990000e+02</td>
      <td>2.640000e+02</td>
      <td>5.180000e+02</td>
    </tr>
    <tr>
      <th>SedentaryMinutes</th>
      <td>940.0</td>
      <td>9.912106e+02</td>
      <td>3.012674e+02</td>
      <td>0.000000e+00</td>
      <td>7.297500e+02</td>
      <td>1.057500e+03</td>
      <td>1.229500e+03</td>
      <td>1.440000e+03</td>
    </tr>
    <tr>
      <th>Calories</th>
      <td>940.0</td>
      <td>2.303610e+03</td>
      <td>7.181669e+02</td>
      <td>0.000000e+00</td>
      <td>1.828500e+03</td>
      <td>2.134000e+03</td>
      <td>2.793250e+03</td>
      <td>4.900000e+03</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Checking the unique count of each variable
daily.nunique(),sleep.nunique()
```




    (Id                           33
     ActivityDate                 31
     TotalSteps                  842
     TotalDistance               615
     TrackerDistance             613
     LoggedActivitiesDistance     19
     VeryActiveDistance          333
     ModeratelyActiveDistance    211
     LightActiveDistance         491
     SedentaryActiveDistance       9
     VeryActiveMinutes           122
     FairlyActiveMinutes          81
     LightlyActiveMinutes        335
     SedentaryMinutes            549
     Calories                    734
     dtype: int64,
     Id                     24
     SleepDay               31
     TotalSleepRecords       3
     TotalMinutesAsleep    256
     TotalTimeInBed        242
     dtype: int64)



### Data Cleaning 
Adding new column to the data, concerting date column from object to datetime, and creating a data base on grouping by user ID.


```python
#Converting the date column into datetime data
daily["ActivityDate"] = pd.to_datetime(daily["ActivityDate"])
```


```python
#Creating the day column
daily['day'] = daily["ActivityDate"].dt.dayofweek
daily.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>ActivityDate</th>
      <th>TotalSteps</th>
      <th>TotalDistance</th>
      <th>TrackerDistance</th>
      <th>LoggedActivitiesDistance</th>
      <th>VeryActiveDistance</th>
      <th>ModeratelyActiveDistance</th>
      <th>LightActiveDistance</th>
      <th>SedentaryActiveDistance</th>
      <th>VeryActiveMinutes</th>
      <th>FairlyActiveMinutes</th>
      <th>LightlyActiveMinutes</th>
      <th>SedentaryMinutes</th>
      <th>Calories</th>
      <th>day</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1503960366</td>
      <td>2016-04-12</td>
      <td>13162</td>
      <td>8.50</td>
      <td>8.50</td>
      <td>0.0</td>
      <td>1.88</td>
      <td>0.55</td>
      <td>6.06</td>
      <td>0.0</td>
      <td>25</td>
      <td>13</td>
      <td>328</td>
      <td>728</td>
      <td>1985</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1503960366</td>
      <td>2016-04-13</td>
      <td>10735</td>
      <td>6.97</td>
      <td>6.97</td>
      <td>0.0</td>
      <td>1.57</td>
      <td>0.69</td>
      <td>4.71</td>
      <td>0.0</td>
      <td>21</td>
      <td>19</td>
      <td>217</td>
      <td>776</td>
      <td>1797</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1503960366</td>
      <td>2016-04-14</td>
      <td>10460</td>
      <td>6.74</td>
      <td>6.74</td>
      <td>0.0</td>
      <td>2.44</td>
      <td>0.40</td>
      <td>3.91</td>
      <td>0.0</td>
      <td>30</td>
      <td>11</td>
      <td>181</td>
      <td>1218</td>
      <td>1776</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1503960366</td>
      <td>2016-04-15</td>
      <td>9762</td>
      <td>6.28</td>
      <td>6.28</td>
      <td>0.0</td>
      <td>2.14</td>
      <td>1.26</td>
      <td>2.83</td>
      <td>0.0</td>
      <td>29</td>
      <td>34</td>
      <td>209</td>
      <td>726</td>
      <td>1745</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1503960366</td>
      <td>2016-04-16</td>
      <td>12669</td>
      <td>8.16</td>
      <td>8.16</td>
      <td>0.0</td>
      <td>2.71</td>
      <td>0.41</td>
      <td>5.04</td>
      <td>0.0</td>
      <td>36</td>
      <td>10</td>
      <td>221</td>
      <td>773</td>
      <td>1863</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Grouping the data base on the ID
daily_gp = daily.groupby("Id").mean().drop(['day'], axis=1)
daily_gp.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>TotalSteps</th>
      <th>TotalDistance</th>
      <th>TrackerDistance</th>
      <th>LoggedActivitiesDistance</th>
      <th>VeryActiveDistance</th>
      <th>ModeratelyActiveDistance</th>
      <th>LightActiveDistance</th>
      <th>SedentaryActiveDistance</th>
      <th>VeryActiveMinutes</th>
      <th>FairlyActiveMinutes</th>
      <th>LightlyActiveMinutes</th>
      <th>SedentaryMinutes</th>
      <th>Calories</th>
    </tr>
    <tr>
      <th>Id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1503960366</th>
      <td>12116.741935</td>
      <td>7.809677</td>
      <td>7.809677</td>
      <td>0.0</td>
      <td>2.858387</td>
      <td>0.794194</td>
      <td>4.152903</td>
      <td>0.000000</td>
      <td>38.709677</td>
      <td>19.161290</td>
      <td>219.935484</td>
      <td>848.161290</td>
      <td>1816.419355</td>
    </tr>
    <tr>
      <th>1624580081</th>
      <td>5743.903226</td>
      <td>3.914839</td>
      <td>3.914839</td>
      <td>0.0</td>
      <td>0.939355</td>
      <td>0.360645</td>
      <td>2.606774</td>
      <td>0.006129</td>
      <td>8.677419</td>
      <td>5.806452</td>
      <td>153.483871</td>
      <td>1257.741935</td>
      <td>1483.354839</td>
    </tr>
    <tr>
      <th>1644430081</th>
      <td>7282.966667</td>
      <td>5.295333</td>
      <td>5.295333</td>
      <td>0.0</td>
      <td>0.730000</td>
      <td>0.951000</td>
      <td>3.609000</td>
      <td>0.004000</td>
      <td>9.566667</td>
      <td>21.366667</td>
      <td>178.466667</td>
      <td>1161.866667</td>
      <td>2811.300000</td>
    </tr>
    <tr>
      <th>1844505072</th>
      <td>2580.064516</td>
      <td>1.706129</td>
      <td>1.706129</td>
      <td>0.0</td>
      <td>0.008387</td>
      <td>0.049032</td>
      <td>1.647419</td>
      <td>0.000000</td>
      <td>0.129032</td>
      <td>1.290323</td>
      <td>115.451613</td>
      <td>1206.612903</td>
      <td>1573.483871</td>
    </tr>
    <tr>
      <th>1927972279</th>
      <td>916.129032</td>
      <td>0.634516</td>
      <td>0.634516</td>
      <td>0.0</td>
      <td>0.095806</td>
      <td>0.031290</td>
      <td>0.507097</td>
      <td>0.000000</td>
      <td>1.322581</td>
      <td>0.774194</td>
      <td>38.580645</td>
      <td>1317.419355</td>
      <td>2172.806452</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Creating user type column base on users' daily step
user_type = []
for i in daily_gp["TotalSteps"]:
    if i < 5000:
        user_type.append("sedentary")
    elif i < 7499:
        user_type.append("lightly active")
    elif i < 9999:
        user_type.append("fairly active")
    else:
        user_type.append("very active")
```


```python
#Adding the new user activity list to the dataframe
daily_gp['user_type'] = user_type
```

## Analyze Phase

In this section, we will explore the hidden patterns in the dataset using plots.

#### Exploratory Data Analysis


```python
#Checking the distribution of user daily's total step
daily['TotalSteps'].hist(bins = 30)
plt.title("Distribution of the total steps")
plt.xlabel("Total steps")
plt.ylabel("Count")
plt.show()
```


    
![png](output_24_0.png)
    



```python
sns.set()
sns.kdeplot(daily['TotalSteps'],bw=0.25)
plt.title("Total steps kernel density plot")
plt.show()
```

    C:\Users\win10\Anaconda3\lib\site-packages\seaborn\distributions.py:1657: FutureWarning: The `bw` parameter is deprecated in favor of `bw_method` and `bw_adjust`. Using 0.25 for `bw_method`, but please see the docs for the new parameters and update your code.
      warnings.warn(msg, FutureWarning)
    


    
![png](output_25_1.png)
    


It can be seen from the total steps kernel density plot that most of the user take between 4000 steps to 10,200 steps. Also, it can be seen that there are some extreme values like 30000 steps to 40000 steps.


```python
sns.set()
sns.kdeplot(daily['TotalDistance'],bw=0.25)
plt.title("Total Distance kernel density plot")
plt.show()
```

    C:\Users\win10\Anaconda3\lib\site-packages\seaborn\distributions.py:1657: FutureWarning: The `bw` parameter is deprecated in favor of `bw_method` and `bw_adjust`. Using 0.25 for `bw_method`, but please see the docs for the new parameters and update your code.
      warnings.warn(msg, FutureWarning)
    


    
![png](output_27_1.png)
    


It can be seen from the total distance kernel density plot that most of the user walks or run an average distance of 2 km to 8km. Also, it can be seen that there are some extreme values like 20km to 30km.

The calories distribution shows that the daily average calories is not normaly distributed. Most of the user average 2000 calories daily.


```python
sns.kdeplot(daily['Calories'],bw=0.25)
plt.title("Calories kernel density plot")
plt.show()
```

    C:\Users\win10\Anaconda3\lib\site-packages\seaborn\distributions.py:1657: FutureWarning: The `bw` parameter is deprecated in favor of `bw_method` and `bw_adjust`. Using 0.25 for `bw_method`, but please see the docs for the new parameters and update your code.
      warnings.warn(msg, FutureWarning)
    


    
![png](output_30_1.png)
    


##### Creating new data base on days of the week. 


```python
#Aggregating the dataframe base on days of the week
days = daily.groupby("day").mean().drop(['Id'], axis=1)
days.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>TotalSteps</th>
      <th>TotalDistance</th>
      <th>TrackerDistance</th>
      <th>LoggedActivitiesDistance</th>
      <th>VeryActiveDistance</th>
      <th>ModeratelyActiveDistance</th>
      <th>LightActiveDistance</th>
      <th>SedentaryActiveDistance</th>
      <th>VeryActiveMinutes</th>
      <th>FairlyActiveMinutes</th>
      <th>LightlyActiveMinutes</th>
      <th>SedentaryMinutes</th>
      <th>Calories</th>
    </tr>
    <tr>
      <th>day</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7780.866667</td>
      <td>5.552917</td>
      <td>5.528750</td>
      <td>0.224552</td>
      <td>1.537333</td>
      <td>0.585833</td>
      <td>3.363083</td>
      <td>0.002583</td>
      <td>23.108333</td>
      <td>14.000000</td>
      <td>192.058333</td>
      <td>1027.941667</td>
      <td>2324.208333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8125.006579</td>
      <td>5.832237</td>
      <td>5.812829</td>
      <td>0.169054</td>
      <td>1.613289</td>
      <td>0.593026</td>
      <td>3.471053</td>
      <td>0.001447</td>
      <td>22.953947</td>
      <td>14.335526</td>
      <td>197.342105</td>
      <td>1007.361842</td>
      <td>2356.013158</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7559.373333</td>
      <td>5.488333</td>
      <td>5.467600</td>
      <td>0.139588</td>
      <td>1.633467</td>
      <td>0.527067</td>
      <td>3.256333</td>
      <td>0.001333</td>
      <td>20.780000</td>
      <td>13.100000</td>
      <td>189.853333</td>
      <td>989.480000</td>
      <td>2302.620000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7405.836735</td>
      <td>5.312245</td>
      <td>5.287415</td>
      <td>0.129283</td>
      <td>1.390476</td>
      <td>0.505170</td>
      <td>3.283129</td>
      <td>0.002313</td>
      <td>19.408163</td>
      <td>11.959184</td>
      <td>185.421769</td>
      <td>961.993197</td>
      <td>2199.571429</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7448.230159</td>
      <td>5.309921</td>
      <td>5.302936</td>
      <td>0.072186</td>
      <td>1.312937</td>
      <td>0.483810</td>
      <td>3.489127</td>
      <td>0.001825</td>
      <td>20.055556</td>
      <td>12.111111</td>
      <td>204.198413</td>
      <td>1000.309524</td>
      <td>2331.785714</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Plotting Total 
days_lab = ["Monday","Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]

plt.bar(days.index,days["TotalSteps"])
plt.ylabel("Total Steps")
plt.xlabel("Days of the week")
plt.title("Total Steps VS Days of the week")
plt.xticks(range(7),days_lab, rotation = 90)
plt.show()
```


    
![png](output_33_0.png)
    


The plot above shows that users take the highest steps on saturday followed by tuesday. Sunday have the lowest total number of steps by users.


```python
plt.bar(days.index,days["Calories"])
plt.xticks(range(7),days_lab)
plt.ylabel("Calories")
plt.xlabel("Days of the week")
plt.title("Calories VS Days of the week")
plt.xticks(range(7),days_lab, rotation = 90)
plt.show()
```


    
![png](output_35_0.png)
    


The calories vs days of the week plot as expected shows that the highest number of calories was on saturday and tuesday. But, the lowest calories was on thursday.


```python
round(daily_gp['user_type'].value_counts()/33*100,2)
```




    lightly active    27.27
    fairly active     27.27
    sedentary         24.24
    very active       21.21
    Name: user_type, dtype: float64



The distribution of the user can be seen to be unbias to some extent. Very active user are the fewest. Now we visualise the distribution.


```python
plt.pie(daily_gp['user_type'].value_counts(),labels = daily_gp['user_type'].value_counts().index, autopct='%1.1f%%')
plt.title("User Type distribution")
plt.show()
```


    
![png](output_39_0.png)
    



```python
sns.boxplot(x="user_type", y = "Calories", data = daily_gp, order = ["very active", "fairly active","lightly active", "sedentary"])
plt.title("Catories by User type")
plt.show()
```


    
![png](output_40_0.png)
    


The calories and user type boxplot shows that active user and fairly user have high calories burn compare to the other two groups. The active user calories varies significant. The lighly active user have the least calories burning.


```python
#Adding new column of week days
sleep['day'] = pd.to_datetime(sleep["SleepDay"]).dt.weekday
```


```python
sleep.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>SleepDay</th>
      <th>TotalSleepRecords</th>
      <th>TotalMinutesAsleep</th>
      <th>TotalTimeInBed</th>
      <th>day</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1503960366</td>
      <td>4/12/2016 12:00:00 AM</td>
      <td>1</td>
      <td>327</td>
      <td>346</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1503960366</td>
      <td>4/13/2016 12:00:00 AM</td>
      <td>2</td>
      <td>384</td>
      <td>407</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1503960366</td>
      <td>4/15/2016 12:00:00 AM</td>
      <td>1</td>
      <td>412</td>
      <td>442</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1503960366</td>
      <td>4/16/2016 12:00:00 AM</td>
      <td>2</td>
      <td>340</td>
      <td>367</td>
      <td>5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1503960366</td>
      <td>4/17/2016 12:00:00 AM</td>
      <td>1</td>
      <td>700</td>
      <td>712</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Daily combined total minute asleep
sns.barplot(x="day", y = "TotalMinutesAsleep", estimator = np.mean , data = sleep)
plt.title("Daily total minutes asleep")
plt.xticks(range(7), days_lab, rotation = 90)
plt.show()
```


    
![png](output_44_0.png)
    


The users have the highest number of combined daily munite asleep on sunday, followed by wednesday. Its not suprising because sunday has the lowest number of steps by users.


```python
#Correlation plot
sns.heatmap(daily_gp[["TotalSteps", "Calories"]].corr(),annot = True)
plt.title("Correlation plot of Calories vs Total step")
```




    Text(0.5, 1.0, 'Correlation plot of Calories vs Total step')




    
![png](output_46_1.png)
    


The correlation value between calories and total step is 0.44. The value is a moderate positive correlation value.


```python
sns.jointplot(x = "TotalSteps", y = "Calories", 
                  data = daily_gp,
                  kind="reg",
                  color="m", height=7)
plt.title('Regression plot')
plt.show()
```


    
![png](output_48_0.png)
    


The trend is positive, and the relationship is not strong.

**Creating a new to count how many times users use the device in a month**


```python
daily_gp["daily_use"] = daily.groupby('Id')['ActivityDate'].count()
```


```python
#Counting the frequency of the number of days 
daily_gp.daily_use.value_counts()
```




    31    21
    30     3
    26     2
    29     2
    4      1
    18     1
    19     1
    20     1
    28     1
    Name: daily_use, dtype: int64



It can be seen that more that 90% of the user uses the devices more that 20 days in a month.


```python
sleep_gr = sleep.groupby('Id').mean()
```


```python
#Combining the sleep dataset and the daily activities dataset
combine = sleep_gr.join(daily_gp, on = 'Id')
```


```python
#Correlation plot
sns.heatmap(combine[["TotalMinutesAsleep", "TotalSteps", "Calories"]].corr(),annot = True)
plt.title("Correlation plot Total Minutes Asleep, Total step, and Calories")
```




    Text(0.5, 1.0, 'Correlation plot Total Minutes Asleep, Total step, and Calories')




    
![png](output_56_1.png)
    


The plot shows that the correlation between the total minutes asleep and calories is very low and negative. Also, the correlation between the total minutes asleep and total step is low and negative.

## Share Phase

### Key Findings

The key findings from the analysis:
- There exist a moderate relationship between daily total steps, and calories burn.
- Users sleep more on Sunday and wednesday
- Also, we find out that the higher the step individual takes daily, the lower the amount of time the person will spend sleeping.
- Majority of the device users are active user.
- People take more steps on saturdays compared to other days.


### Recommendations:

Base on the trend seen from the analysis, the following recommendations will be made:

- The company should focus more on sensitizing the users about the benefits of the devices.
- It was observed that mejority of the users make use of their devices, this shows that adding more futures to the device will help them stay glue to using the devices more.
- Users with the same demography can be paired, and there should be a users' dashboard where they will be able to see how other users' daily task performance.
- Daily target should be considered as a feature to the device.
- Also, more users should be added to the data for the analysis to be inferential on a larger populance.


```python

```
