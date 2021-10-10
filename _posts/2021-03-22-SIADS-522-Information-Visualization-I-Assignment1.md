---
layout: post
title: SIADS 522 
description: Information Visualization I Assignment 1
date: 2021-03-22
image: '/images/09.jpg'
tags: [data visualization, python, altair]
---
* 目录
{:toc}

## Syllabus
This is the [Syllabus](https://github.com/alisongh/MADS/blob/0481b2d86b94485c805d1381a8ce7906290e247e/SIADS%20522/SIADS_522_Information_Visualization_I_W21_Adar.pdf) of SIADS 522.

sBefore you turn this problem in, make sure everything runs as expected. First, **restart the kernel** (in the menubar, select Kernel&#8594;Restart) and then **run all cells** (in the menubar, select Cell&#8594;Run All). Make sure your notebook executed to the end.

Make sure you fill in any place that says `YOUR CODE HERE` or `YOUR ANSWER HERE`. Please remember that homeworks are to be completed independently. You may not share code with others.

## Week 1: 
- Domain identification vs Abstract Task extraction
- Pandas Review 

## Assignment Overview
### The objectives for this week are for you to:

- Review, reflect, and apply the concepts of Domain Tasks and Abstract Tasks. Specifically, given a real context, identify the expert's goals and then abstract the visualization tasks. 

!["Drawing"](https://github.com/alisongh/MADS/blob/0481b2d86b94485c805d1381a8ce7906290e247e/SIADS%20522/Assignment%201/assets/domain-abstraction.png)



- Review and evaluate the domain of [Pandas](https://pandas.pydata.org/) as a tool for reading, manipulating, and analyzing datasets in Python.

### The total score of this assignment will be 100 points consisting of:
- Case study reflection: Car congestion and crash rates (20 points)
- Pandas programming exercise (80 points)


### Resources:
- We're going to be recreating parts of this article by [CMAP](https://www.cmap.illinois.gov/) available [online](https://www.cmap.illinois.gov/updates/all/-/asset_publisher/UIMfSLnFfMB6/content/crash-scans-show-relationship-between-congestion-and-crash-rates) (CMAP, 2016)  
- We'll need the datasets from the city of Chicago. We have downloaded a subset to the local folder [/assets](assets/)
    - If you're curious, the original dataset can be found on [Chicago Data Portal](https://data.cityofchicago.org/)
        - [Chicago Traffic Tracker - Historical Congestion Estimates by Segment - 2011-2018](https://data.cityofchicago.org/Transportation/Chicago-Traffic-Tracker-Historical-Congestion-Esti/77hq-huss)
        - [Traffic Crashes - Crashes](https://data.cityofchicago.org/Transportation/Traffic-Crashes-Crashes/85ca-t3if)
- Altair
    - We will use a python library called [Altair](https://altair-viz.github.io/) for the visualizations. Don't worry about understanding this code. You will only need to prepare the data for the visualization in Pandas. If you do it correctly, our code will produce the visualization for you.
    
    
### Important notes:
1) When turning in your PDF, please use the File -> Print -> Save as PDF option ***from your browser***. Do ***not*** use the File->Download as->PDF option. Complete instructions for this are under Resources in the Coursera page for this class.

2) Pay attention to the return types of your functions. Sometimes things will look right but fail later if you return the wrong kind of object (e.g., Array instead of Series)

## Part 1. Domain identification vs Abstract Task extraction (20 points)
Read the following article by CMAP [Crash scans show the relationship between congestion and crash rates](https://www.cmap.illinois.gov/updates/all/-/asset_publisher/UIMfSLnFfMB6/content/crash-scans-show-relationship-between-congestion-and-crash-rates) and answer the following questions.

Remember: Domain tasks are questions an analyst (or reader) might need to figure out. For example, a retail analyst might want to know: how many fruit did we sell? or what’s the relationship between temperature and fruits rotting? A learning analyst would have the domain task: how often do students pass the class? or how does study time correlate with grade? An advertising analyst would ask: how many people clicked on an ad? or what’s the relationship between time of day and click through rate?  

Abstract tasks are generic: What’s the sum of a quantitative variable? or what’s the correlation between two variables? Notice we gave two examples for each analyst type and these roughly map to the two abstract questions. You should not use domain language (e.g., accidents) when describing abstract tasks. 

### 1.1 Briefly describe who you think performed this analysis. What is their expertise? What is their goal for the article? Give 3 examples of domain tasks featured in the article. (10 points)

This analysis should be performed by the analyst and employees of the Department of Transportation of Chicago, especially for the specific employees focusing on the ground transporation. They expertise on the knowledge of ground transporation, such as the surface transportation networks and public way safe for users. They have mature knowledge about the priciple of designing of transportation networks and they are skilled in choosing and using the approprate tools to collect and analyze data, for example why we need to build an expressway in the specific area and how does it relieve the traffic. 
The main goal is to build and improve the transporation system to improve the traffic efficiency and decrease accident rates. 

**Domain tasks**
<ol>
<li>What's the relationship between crash rate and congestion?</li>
<li>Is there any relationship between the travel time and crash rate? If so, how?</li>
<li>Does the expressway decrease the crash rate? Why?</li>
</ol>

### 1.2  For each domain task describe the abstract task (10 points)

* What's the relationship between crash rate and congestion?
<ol>
<li>How many variables we need to consider?</li>
<li>What are the max and min of the quantitive variables?</li>
</ol>
* Is there any relationship between the travel time and crash rate? If so, how?
<ol>
<li>What are variables we use to consider the relationship between the travel time and crash rate?</li>
</ol>
* Does the expressway decrease the crash rate? Why?
<ol>
<li>What is the sum of the quantitive variables?</li>
</ol>

## Part 2. Pandas programming exercise (80 points)
We have provided some code to create visualizations based on these two datasets:
1. [Historic Congestion](assets/Pulaski.small.csv.gz) 
2. [Traffic Crashes](assets/Traffic.Crashes.csv.gz)

Complete each assignment function and run each cell to generate the final visualizations

```python
import pandas as pd
import numpy as np
import altair as alt

# enable correct rendering
alt.renderers.enable('default')

# uses intermediate json files to speed things up
alt.data_transformers.enable('json')
```

### PART A: Historic Congestion ( 55 points)
For parts 2.1 to 2.5 we will use the Historic Congestion dataset. This dataset contains measures of speed for different segments. For this subsample, the available measures are limited to traffic on Pulaski Road in 2018.

### 2.1 Read and resample (15 points)
Complete the `read_csv` and `get_group_first_row` functions.
Since our dataset is large we want to only grab one measurement per hour for each segment. To do this, we will resample by selecting the first measure for each month, day, hour on each segment. Complete the `get_group_first_row` function to achieve this. Note that the file we are loading is compressed--depending on how you load the file, this may or may not make a difference ([you'll want to look at the API documents](https://pandas.pydata.org/pandas-docs/stable/reference/index.html)).

```python
def read_csv(filename):
    """Read the csv file from filename (uncompress 'gz' if needed)
    return the dataframe resulting from reading the columns
    """
    filestring = "gz"
    if filename in filestring:
        df = pd.read_csv(filename, compression='gzip')
    else:
        df = pd.read_csv(filename)

    return df
    # YOUR CODE HERE
    # raise NotImplementedError()
```
```python
# Save the congestion dataframe on hist_con
hist_con = read_csv('assets/Pulaski.small.csv.gz')
print(hist_con.shape)
assert hist_con.shape == (3195450, 10)
assert list(hist_con.columns) == ['TIME','SEGMENT_ID','SPEED','STREET','DIRECTION','FROM_STREET','TO_STREET',
                                  'HOUR','DAY_OF_WEEK','MONTH']
```
**Output**
```
(3195450, 10)
```
```python
def get_group_first_row(df, grouping_columns):
    """Group rows using the grouping columns and return the first row belonging to each group
    (you can look at first() for reference). We'll write this function to be more general in case
    we want to use it for a different resample.
    return a dataframe without a hierarchical index (use default index)
    
    See the example link below if you want a better sense of what this should return
    """
    df = df.groupby(grouping_columns)
    # res.columns = list(map(''.join, res.columns.values))
    df = df.first()
    return df.reset_index()
    # YOUR CODE HERE
    # raise NotImplementedError()
```
```python
# test your code, we want segment_rows to be resampled version of hist_con where we've grouped by the
# properties month, day_of_week, hour, and segment_id and returned the first measure of each group
segment_rows = get_group_first_row(hist_con, ['MONTH','DAY_OF_WEEK', 'HOUR', 'SEGMENT_ID'])
```
The table should look something like ![this](https://github.com/alisongh/MADS/blob/6126b0c9d0a8339521b7108368b90969de74a75d/SIADS%20522/Assignment%201/assets/segment_rows.png). 

***Note** When we show examples like this, we are sampling (e.g., ```segment_rows.sample(5)```) so your table may look different.

If you want to build your own tests from our example tables, you can create an assert test for one of the rows and make sure the values match what you expect. For example we see that the row id 68592 in the example is for 8/27/2018 at 1:50:21 PM. So we could write the test:

```assert segment_rows.loc[68952].TIME == '08/27/2018 01:50:21 PM'```

If this assertion failed, you'd get an error message.

### 2.2 Basic Bar Chart Visualization (10 points)
We want to create a visualization for the *average speed* of each segment (across all the samples). To do this, we're going to want to group by each segment and calculate the average speed on each. Complete this code on the `average_speed_per_segment` function. Make sure your function returns a ***series***.

```python
def average_speed_per_segment(df):
    """Group rows by SEGMENT_ID and calculate the mean of each
    return a *series* where the index is the segment id and each value is the average speed per segment
    """
    df = df.groupby('SEGMENT_ID')['SPEED'].mean()
    # ts = pd.Series(df['Value'], index=df['Date']) 
    return df

    # YOUR CODE HERE
    # raise NotImplementedError()
```
```python
# calculate the average speed per segment
average_speed = average_speed_per_segment(segment_rows)

# # create labels for the visualization
labels = average_speed.index.astype(str)

# grab the values from the table
values = pd.DataFrame(average_speed).reset_index()

# check what's in average_speed
average_speed
```
**Output**
```
SEGMENT_ID
19    12.251926
20    15.274452
21    12.141079
22    12.346769
23    12.716657
        ...    
93    13.503260
94    14.560759
95    14.959099
96    21.659751
97    18.714286
Name: SPEED, Length: 78, dtype: float64
```

If you got things right, the **series** should look something like 

![this](https://github.com/alisongh/MADS/blob/6126b0c9d0a8339521b7108368b90969de74a75d/SIADS%20522/Assignment%201/assets/average_speed.png). 

You might want to write a test to make sure you are returning the expected type. For example:

```assert type(average_speed) == pd.core.series.Series```

```python
# let's generate the visualization

# create a chart
base = alt.Chart(values)

# we're going to "encode" the variables, more on this next assignment
encoding = base.encode(
    x= alt.X(                # encode SEGMENT_ID a sa quantiative variable on the X axis
            'SEGMENT_ID:Q',
            title='Segment ID',
            scale=alt.Scale(zero=False)   # we don't need to start at 0
    ),
    y=alt.Y(
            'sum(SPEED):Q',   # encode the sum of speed for the segment as a quantitative variable on Y
            title='Speed Average MPH'
    ),
)

# we're going to use a bar chart and set various parameters (like bar size and title) to make it readable
encoding.mark_bar(size=7).properties(title='Average Speed per Segment',height=300, width=900)
```

**Output**

![](https://github.com/alisongh/MADS/blob/b8b9d89e90fd5ae71293dec8cf6623f5127d979f/SIADS%20522/Assignment%201/assets/1.png)

### 2.3 Create a basic pivot table (10 points)
For the next visualization, we need a more complex transformation that will allow us to see the average speed for each month. To do this, we will create a pivot table where the index is the month, and each column is a segment id. We will put the average speed in the cells. From the table, we'll be able to find the month (by index)--giving us the row, and pick the column corresponding to the segment we care about.

Complete the `create_pivot_table` function for this
```python
def create_pivot_table (df):
    """return a pivot table where:
    each row i is a month
    each column j is a segment id
    each cell value is the average speed for the month i in the segment j
    """
    df = df.pivot_table('SPEED',index=['MONTH'], columns=['SEGMENT_ID'], aggfunc='mean')
    return df
    # YOUR CODE HERE
    # raise NotImplementedError()
```
```python
# run the code and see what's in the table
pivot_table = create_pivot_table(segment_rows)
```

The table should look something like 

![this](https://github.com/alisongh/MADS/blob/b8b9d89e90fd5ae71293dec8cf6623f5127d979f/SIADS%20522/Assignment%201/assets/pivot_table.png)

As before, we can write a "test" based on this example.  For example, here we see that in March (Month 3) segment 21 had a value of ~11.696, so we could write the test:

```assert round(pivot_table.loc[3,21],3) == 11.696```

```python
# we're going to implement a transformation to put the pivot table into a 'long form' because it
# is easier to specify the visualization. You can print out hm_pivot_table to see what it looks like

hm_pivot_table = pivot_table.copy().unstack().reset_index()
hm_pivot_table['SPEED'] = hm_pivot_table[0]
hm_pivot_table.drop(0,axis=1,inplace=True)

# create the visualization. We're going to use rectangles (a heat map of sorts). We'll use the segment_id to
# figure out the horizontal placement (x), the month as the vertical (y) and use color to encode the speed.
encoding = alt.Chart(hm_pivot_table).mark_rect().encode(
    x='SEGMENT_ID:O',
    y='MONTH:O',
    color='SPEED:Q'
)

encoding.properties(title='Average Speed per Segment per Month',height=300, width=800)
```
**Output**

![](https://github.com/alisongh/MADS/blob/492bcc055715a0a210e67b2758fc2276a96dab3d/SIADS%20522/Assignment%201/assets/2.png)

```python
# test function
pivot_table = create_pivot_table(segment_rows)
# check that the rows are months and columns are segments
assert pivot_table.shape == (11, 78), "Problem 2.3, first test"
# check that the value is the average
assert int(pivot_table.loc[2,19]) == 6, "Problem 2.3, second test"
assert int(pivot_table.loc[3,19]) == 10, "Problem 2.3, third test"
```

### 2.4 Sorting, Transforming, and Filtering (20 points)
Without telling you too much about the visualization we want to create next (that's part of the bonus below), we need to get the data into a form we can use. 
- We're going to need to sort the dataframe by one or more columns (this is the `sort_by_col` function). 
- We'll want to create a derivative column that is the time of the measurement rounded to the nearest hour (`time_to_hours`)
- We need to "facet" the data into groups to generate different visualizations. 
- We need a function that selects part of the dataframe that matches a specific characteristic (`filter_orientation`)
- Grab a specific column from the dataframe (`select_column`)

```python
def sort_by_col(df, sorting_columns):
    """Sort the rows of df by the columns (sorting_columns)
    return the sorted dataframe
    """
    df = df.sort_values(by = sorting_columns)
    return df
    # YOUR CODE HERE
    # raise NotImplementedError()
```
```python
segment_rows = sort_by_col(segment_rows, ['SEGMENT_ID'])
segment_rows
```

![](https://github.com/alisongh/MADS/blob/e672a946d1dd62ae09cfcd85588b9a8600593914/SIADS%20522/Assignment%201/assets/3.png)

```python
def time_to_hours(df):
    """ Add a column (called TIME_HOURS) based on the data in the TIME column and rounded up 
    the value to the nearest hour.  For example, if the original TIME row said: 
    ‘02/28/2018 05:40:00 PM’ we want ‘2018-02-28 18:00:00’  
    (the change is that 5:40pm was rounded up to 6:00pm and the TIME_HOUR column is 
    actually a proper datetime and not a string).
    """
    df['TIME_HOURS'] = df['TIME']
    df['TIME_HOURS'] = pd.to_datetime(df['TIME_HOURS']).dt.round('H').dt.strftime("%Y-%m-%d %I:%M:%S %p")
    # df.astype({'col1': 'int32'}).dtypes
    df['TIME_HOURS'] = pd.to_datetime(df['TIME_HOURS'])
    return df
    
segment_rows = time_to_hours(segment_rows)
    
    # YOUR CODE HERE
    # raise NotImplementedError()
```    

```python
def filter_orientation(df, traffic_orientation):
    """ Filter the rows according to the traffic orientation
    return a df that is a subset of the original with the desired orientation
    """
    # YOUR CODE HERE
    df = df[df['DIRECTION'] == traffic_orientation]
    return df
    
sb = filter_orientation(segment_rows, 'SB')
nb = filter_orientation(segment_rows, 'NB')
    
    # raise NotImplementedError()
```    

The sb table should look like ![this](https://github.com/alisongh/MADS/blob/32a8d055b6d9e47f6a62546ec6b8f7211baa6a21/SIADS%20522/Assignment%201/assets/sb.png)

```python
def select_column(df, column_name):
    """ Select a column from the df
    return a series with the desired column
    """
    return df[column_name]
    # YOUR CODE HERE
    # raise NotImplementedError()
    
# we're going to remove speeds of -1 (no data)
sb = sb[sb.SPEED > -1]
nb = nb[nb.SPEED > -1]

alt.data_transformers.disable_max_rows()
alt.Chart(sb.append(nb)).mark_rect().encode(
    x='month(TIME_HOURS):T',
    y='FROM_STREET:N',
    color='mean(SPEED):Q',
    facet='DIRECTION:N'
).properties(
    width=300,
    height=400
)
```
![](https://github.com/alisongh/MADS/blob/00ec7f429d2e0fa7c76a742ff605347a5341f05e/SIADS%20522/Assignment%201/assets/4.png)

### 2.5 (Bonus)  Traffic heatmap visualization (up to 2 points)
Looking at the visualization above (the one showing Northbound versus Southbound facets), what domain/abstract tasks are fulfilled by this visualization? List at least one domain task and the corresponding abstract task.

### PART B: Crashes (25 points)
For parts 2.6 and 2.7 we will use the Crashes dataset. This dataset contains crash entries recording the time of the accident, the street, and the street number where the accident occurred. You will work with accidents recorded on Pulaski Road
```python
crashes = read_csv('assets/Traffic.Crashes.csv.gz')
crashes_pulaski = crashes[crashes.STREET_NAME == 'PULASKI RD']
```
### 2.6 Calculate summary statistics for grouped streets (15 points)
- Group the streets every 300 units (street numbers). Hint: You can use the `pd.cut` function
- Calculate the number of accidents (count rows) and the total of injuries (sum injuries total) for each of these 300-chunk road segments. Do this *for each direction*.

Complete bin_crashes and calculate_group_aggregates functions for this

```python
def bin_crashes(df):
    """ Assign each crash instance a category (bin) every 300 house number units starting from 0
    Return a new dataframe with a column called BIN where each value is the start of the bin
    i.e. 0 is the label for records with street number n, where 1 <= n <= 300
    300 is the label for records with n at 301 <= n <= 600, and so on.
    """
    bins = list(range(0, crashes_pulaski.STREET_NO.max()+1000, 300))
    # b = [i for i in range(11480)]
    # labels = b[0::300]
    df = crashes_pulaski
    df['BIN'] = pd.cut(df['STREET_NO'], bins, labels=bins[:-1])
    return df

    # YOUR CODE HERE
    # raise NotImplementedError()
    
binned_df = bin_crashes(crashes_pulaski)

# sample the values to see what's in your new DF
binned_df.sample(5)[['STREET_NO','BIN']]
```
A sample of the relevant columns from the table would look something like 

![this](https://github.com/alisongh/MADS/blob/00ec7f429d2e0fa7c76a742ff605347a5341f05e/SIADS%20522/Assignment%201/assets/binned_df.png). 

We can also create a histogram of street numbers to see which are the most prevalent. It should look something like ![this](https://github.com/alisongh/MADS/blob/32394bc2dfd91d69c9d91abfe8c8d62b805349ba/SIADS%20522/Assignment%201/assets/street_no.png).

```python
# create this vis
alt.Chart(binned_df).mark_bar().encode(
    alt.X('BIN'),
    alt.Y('count()')
)
acc_count = df.groupby(['BIN','STREET_DIRECTION']).count().fillna(0)['INJURIES_TOTAL'].reset_index(name='ACCIDENT_COUNT')
```

```python
def calculate_group_aggregates(df):
    """ 
    There are *accidents* and *injuries* (could be 0 people got hurt, could be more). 
    There’s one row per accident at the moment, so we want to know how many accidents 
    happened in each BIN/STREET_DIRECTION (this will be the count) and how many injuries (which will be the sum). 
    
    Return a df with the count of accidents in a column named 'ACCIDENT_COUNT' (how many accidents happened in each 
    bin (the count) and how many injuries (the sum) in a column named 'INJURIES_SUM'
    
    Replace NaN with 0
    """
    acc_count = df.groupby(['BIN','STREET_DIRECTION']).count().fillna(0)['INJURIES_TOTAL'].reset_index(name='ACCIDENT_COUNT')
    inj_count = df.groupby(['BIN','STREET_DIRECTION'])['INJURIES_TOTAL'].sum().reset_index(name='INJURIES_SUM')
    df = pd.merge(acc_count, inj_count, how='outer').fillna(0.0)
    return df

    # YOUR CODE HERE
    # raise NotImplementedError()
```
```python
aggregates = calculate_group_aggregates(binned_df)

# check the data
#aggregates.head(15)

aggregates.sample(15)
```
The table should look like 

![this](https://github.com/alisongh/MADS/blob/32394bc2dfd91d69c9d91abfe8c8d62b805349ba/SIADS%20522/Assignment%201/assets/2.6_aggregate_1.png)

Just for fun, here's a plot of injuries in the North and South directions based on bin. This may also help you debug your code. Depending on whether you removed N/A or if you hardcoded things, you may see slight differences. Here's what it might  [look like](assets/direction_injuries.png)

```python
alt.Chart(aggregates).mark_point().encode(
    alt.Color('STREET_DIRECTION'),
    alt.X('BIN'),
    alt.Y('INJURIES_SUM')
)
```
![](https://github.com/alisongh/MADS/blob/fce8eb28382e6a954ff207e1fe1030bc8d00a8c0/SIADS%20522/Assignment%201/assets/5.png)

### 2.7 Sort the street ranges (10 points)
- Sort the dataframe so North streets are in descending order and South streets are in ascending order
- You are provided with a 'sort' arrray that contains this desired order. Use a categorical (pd.Categorial) column to order the dataframe according to this array.

```python
crashed_range = list(range(0, crashes_pulaski.STREET_NO.max()+1000, 300))
sort_order = ['N ' + str(s) for s in crashed_range[::-1]] + ['S ' + str(s) for s in crashed_range]
def categorical_sorting(df, sorder):
    """ Create a column called ORDER_LABEL that contains a concatenation of the street direction and the street range
    Set the sort order of this column to the provided sort array (sorder: the elements of this column should be in the same order of the array)
    Sort the dataframe (df) by this column
    """
    # df['CATEGORY'] = pd.Categorical(df['CATEGORY'], categories=sort_order, ordered=True)
    # df.sort_values('CATEGORY',inplace=True)
    # df= df[df['INJURIES_SUM'] != 0]
    df['ORDER_LABEL'] = df[['STREET_DIRECTION','BIN']].astype(str).agg(' '.join,axis=1)
    df['ORDER_LABEL'] = pd.Categorical(df['ORDER_LABEL'], categories=sorder, ordered=True)
    df.sort_values('ORDER_LABEL',inplace=True)
    return df
    # YOUR CODE HERE
    # raise NotImplementedError()
```
```python
sorted_groups = categorical_sorting(aggregates, sort_order)

# check the values
sorted_groups.sample(15)
```
**Output**

![](https://github.com/alisongh/MADS/blob/8cc6cf832b7e8d738722974b46b7582892f1a2aa/SIADS%20522/Assignment%201/assets/6.png)

```python
sorted_groups['ORDER_LABEL'].iloc[0] > sorted_groups['ORDER_LABEL'].iloc[1]
```
**Output**
```
True
```

```python
sorted_groups
```
**Output**

![](https://github.com/alisongh/MADS/blob/84e67764096300adb84fabd78148882d22a1234c/SIADS%20522/Assignment%201/assets/7.png)

The table should look like 

![this](https://github.com/alisongh/MADS/blob/a1f826efbf49b4bd8fc5ee7f70113ec7ca102422/SIADS%20522/Assignment%201/assets/sorted_groups.png)

You can test your code a few ways. First, we gave you the sort order, so you know what the ORDER_LABEL of the first row should be:

```assert sorted_groups['ORDER_LABEL'].iloc[0] == sort_order[1]```

(it might be `sort_order[0]` depending on how you did the label)

You also know that the first item should be "greater" than the second, so you can test:

```assert sorted_groups['ORDER_LABEL'].iloc[0] > sorted_groups['ORDER_LABEL'].iloc[1]```

Again, just for kicks, let's see where injuries happen. We're going to color bars by the bin and preserve our ascending/descending visualization. We can probably imagine other (better) ways to visualize this data, but this may be useful for you to debug. The visualization should look something like [this](assets/order_injuries.png)

If your X axis cutoffs are a bit different, that's fine. 

```python
alt.Chart(sorted_groups).mark_bar().encode(
    alt.X('ORDER_LABEL:O', sort=sort_order),
    alt.Y('INJURIES_SUM:Q'),
    alt.Color('BIN:Q')
).properties(
    width=400
)
```

Ok, let's actually make a useful visualization using some of the dataframes we've created. As a bonus, we're going to ask you what you would use this for.

```python
# to make the kind of chart we are interested in we're going to build it out of three different charts and
# put them together at the end

# this is going to be the left chart
bar_sorted_groups = sorted_groups[['ACCIDENT_COUNT','INJURIES_SUM']].unstack().reset_index() \
    .rename({'level_0':'TYPE','level_1':'SPEED',0:'COUNT'},axis=1)

# Note that we cheated a bit. The actual speed column (POSTED_SPEED) doesn't have enough variation for this
# example, so we're using the level_1 variable (it's an index variable) as a fake SPEED. 
# Just assume this actually is the speed at which the accident happened.

a = alt.Chart(bar_sorted_groups).mark_bar().transform_filter(alt.datum.TYPE == 'ACCIDENT_COUNT').encode(
    x=alt.X('COUNT:Q',sort='descending'),
    y=alt.Y('SPEED:O',axis=None),
    color=alt.Color('TYPE:N', 
                    legend=None,
                    scale=alt.Scale(domain=['ACCIDENT_COUNT', 'INJURIES_SUM'],
                                    range=['blue', 'orange']))
).properties(
    title='ACCIDENT_COUNT',
    width=300,
    height=600
)

# middle "chart" which actually won't be a chart, just a bunch of labels
b = alt.Chart(bar_sorted_groups).mark_bar().transform_filter(alt.datum.TYPE == 'ACCIDENT_COUNT').encode(
    y=alt.Y('SPEED:O', axis=None),
    text=alt.Text('SPEED:Q')
).mark_text().properties(title='SPEED',
                         width=20,
                         height=600)

# and the right most chart
c = alt.Chart(bar_sorted_groups).mark_bar().transform_filter(alt.datum.TYPE == 'INJURIES_SUM').encode(
    x='COUNT:Q',
    y=alt.Y('SPEED:O',axis=None),
    color=alt.Color('TYPE:N', 
                    legend=None,
                    scale=alt.Scale(domain=['ACCIDENT_COUNT', 'INJURIES_SUM'],
                                    range=['blue', 'orange']))
).properties(
    title='INJURIES_SUM',
    width=300,
    height=600
)

# put them all together 

a | b | c
```
**Output**

![](https://github.com/alisongh/MADS/blob/3bd91df32ee316b18705d530fe931a5bc6916323/SIADS%20522/Assignment%201/assets/8.png)


