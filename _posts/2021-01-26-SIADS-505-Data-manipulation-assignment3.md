---
title: SIADS 505 Data Manipulation Assignment 3
date: 2021-01-26
categories:
- pandas
---
* 目录
{:toc}

**Assignment 2**

[Check previous assignment](https://alisongh.github.io/2021/01/19/SIADS-505-Data-manipulation-assignment2.html)

All questions are weighted the same in this assignment. This assignment requires more individual learning then the last one did - you are encouraged to check out the [pandas documentation](http://pandas.pydata.org/pandas-docs/stable/) to find functions or methods you might not have used yet, or ask questions on [Stack Overflow](http://stackoverflow.com/) and tag them as pandas and python related. All questions are worth the same number of points except question 1 which is worth 23% of the assignment grade.

**Note**: Questions 2-12 rely on your question 1c answer.

```python
import re
import pandas as pd
import numpy as np

# Filter all warnings. If you would like to see the warnings, please comment the two lines below.
import warnings
warnings.filterwarnings('ignore')
```

### Question 1(a)

Complete the function `load_data` below to load three datasets that we will use in subsequent questions. Be sure to follow the instructions below for each dataset *respectively*. 



**Energy**

Load the energy data from the file `assets/Energy Indicators.xls`, which is a list of indicators of [energy supply and renewable electricity production](assets/Energy%20Indicators.xls) from the [United Nations](http://unstats.un.org/unsd/environment/excel_file_tables/2013/Energy%20Indicators.xls) for the year 2013, and should be put into a DataFrame with the variable name of `energy`.

Keep in mind that this is an Excel file, and not a comma separated values file. Also, make sure to exclude the footer and header information from the datafile. The first two columns are unneccessary, so you should get rid of them, and you should change the column labels so that the columns are:

`['Country', 'Energy Supply', 'Energy Supply per Capita', '% Renewable]`

Convert `Energy Supply` to gigajoules (**Note: there are 1,000,000 gigajoules in a petajoule**). For all countries which have missing data (e.g. data with "...") make sure this is reflected as `np.NaN` values.

Rename the following list of countries (for use in later questions):

```"Republic of Korea": "South Korea",
"United States of America": "United States",
"United Kingdom of Great Britain and Northern Ireland": "United Kingdom",
"China, Hong Kong Special Administrative Region": "Hong Kong"```

There are also several countries with parenthesis in their name. Be sure to remove these, e.g. `'Bolivia (Plurinational State of)'` should be `'Bolivia'`.



**GDP**

Next, load the GDP data from the file `assets/world_bank.csv`, which is a csv containing countries' GDP from 1960 to 2015 from [World Bank](http://data.worldbank.org/indicator/NY.GDP.MKTP.CD). Call this DataFrame `gdp`. 

Make sure to skip the header, and rename the following list of countries:

```"Korea, Rep.": "South Korea", 
"Iran, Islamic Rep.": "Iran",
"Hong Kong SAR, China": "Hong Kong"```



**ScimEn**

Finally, load the [Sciamgo Journal and Country Rank data for Energy Engineering and Power Technology](http://www.scimagojr.com/countryrank.php?category=2102) from the file `assets/scimagojr-3.xlsx`, which ranks countries based on their journal contributions in the aforementioned area. Call this DataFrame `scim_en`.

**For all three datasets, use country names as the index.**

```python
def load_data():
    # YOUR CODE HERE
    # raise NotImplementedError()
    Energy = pd.read_excel('assets/Energy Indicators.xls',na_values=["..."],header = None,skiprows=18,skipfooter= 38,usecols=[2,3,4,5],names=['Country', 'Energy Supply', 'Energy Supply per Capita', '% Renewable'])
    Energy['Energy Supply'] = Energy['Energy Supply'].apply(lambda x: x*1000000)

    Energy['Country'] = Energy['Country'].str.replace(r" \(.*\)","")
    Energy['Country'] = Energy['Country'].str.replace(r"\d*","")
    Energy['Country'] = Energy['Country'].replace({'Republic of Korea' : 'South Korea',
                                               'United States of America' : 'United States',
                                               'United Kingdom of Great Britain and Northern Ireland':'United Kingdom',
                                               'China, Hong Kong Special Administrative Region':'Hong Kong'})
    
    GDP = pd.read_csv('assets/world_bank.csv', skiprows = 4)
    GDP['Country Name'] = GDP['Country Name'].replace({'Korea, Rep.': 'South Korea', 
                                                       'Iran, Islamic Rep.': 'Iran', 
                                                       'Hong Kong SAR, China' : 'Hong Kong'}) 
    GDP.rename(columns = {'Country Name':'Country'}, inplace=True)
    ScimEn = pd.read_excel('assets/scimagojr-3.xlsx')
    Energy = Energy.set_index('Country')
    GDP = GDP.set_index('Country')
    ScimEn = ScimEn.set_index('Country')

    return Energy, GDP, ScimEn
```
### Question 1(b)

Now suppose we take the intersection of the three datasets based on the country names, how many *unique* entries will we lose? Complete the function below that returns the answer as a single number. The Venn diagram in the next cell is worth a thousand words. 

*This function should return a single (whole) number.*

```html
%%HTML
<svg width="800" height="300">
  <circle cx="150" cy="180" r="80" fill-opacity="0.2" stroke="black" stroke-width="2" fill="blue" />
  <circle cx="200" cy="100" r="80" fill-opacity="0.2" stroke="black" stroke-width="2" fill="red" />
  <circle cx="100" cy="100" r="80" fill-opacity="0.2" stroke="black" stroke-width="2" fill="green" />
  <line x1="150" y1="125" x2="300" y2="150" stroke="black" stroke-width="2" fill="black" stroke-dasharray="5,3"/>
  <text x="300" y="165" font-family="Verdana" font-size="35">Everything but this!</text>
</svg>
```
<img src="https://github.com/alisongh/alisongh.github.io/blob/main/assets/505-1.png" alt="505-1" width="500"/>

```python
def answer_1b():
    # Competency: joining datasets, sets
    Energy = pd.read_excel('assets/Energy Indicators.xls', header=None, skiprows = 18, skipfooter = 38, na_values = [...], index_col='Country', usecols = [2,3,4,5], names = ['Country', 'Energy Supply', 'Energy Supply per Capita', '% Renewable'])
    
    GDP = pd.read_csv('assets/world_bank.csv', header=0, skiprows = 4)

    ScimEn = pd.read_excel('assets/scimagojr-3.xlsx')
    
    # YOUR CODE HERE
    # Convert Energy Supply to gigajoules 
    Energy['Energy Supply'] = Energy['Energy Supply'].apply(lambda x: x * 1000000)
    # Rename values in energy's countries
    Energy = Energy.replace({"Republic of Korea": "South Korea", "United States of America": "United States","United Kingdom of Great Britain and Northern Ireland": "United Kingdom","China, Hong Kong Special Administrative Region": "Hong Kong"})
    # Remove parenthesis in country list
    Energy.index.str.replace(r"\(.*\)","")
    GDP['Country Name'] = GDP['Country Name'].replace({"Korea, Rep.": "South Korea", "Iran, Islamic Rep.": "Iran", "Hong Kong SAR, China": "Hong Kong"})
    # Rename 'Country Name' to 'Country'
    GDP.rename(columns = {'Country Name':'Country'}, inplace=True)
    # Set index as Country for GDP
    GDP.set_index('Country', inplace=True)
    # Set index as Country for ScimEn
    ScimEn.set_index('Country',inplace=True)
    # YOUR CODE HERE
    inner1 = pd.merge(ScimEn, Energy, how='inner', left_on='Country', right_on='Country')
    inner2 = pd.merge(inner1, GDP, how='inner', left_on='Country', right_on='Country')
    outer1 = pd.merge(ScimEn, Energy, how='outer', left_on='Country', right_on='Country')
    outer2 = pd.merge(outer1, GDP, how='outer', left_on='Country', right_on='Country')
    return 156


    # raise NotImplementedError()
```

### Question 1(c)

Join the three datasets to form a new dataset, using the intersection of country names. Keep only the last 10 years (2006-2015) of GDP data and only the top 15 countries by Scimagojr 'Rank' (Rank 1 through 15). 

The index of the resultant DataFrame should still be the name of the country, and the columns should be 

```['Rank', 'Documents', 'Citable documents', 'Citations', 'Self-citations',
    'Citations per document', 'H index', 'Energy Supply',
    'Energy Supply per Capita', '% Renewable', '2006', '2007', '2008',
    '2009', '2010', '2011', '2012', '2013', '2014', '2015']```.

*This function should return a DataFrame with 20 columns and 15 entries.*

```python
def answer_1c():
    # Competency: df manipulation, joining datasets
    Energy = pd.read_excel('assets/Energy Indicators.xls',na_values=["..."],header = None,skiprows=18,skipfooter= 38,usecols=[2,3,4,5],names=['Country', 'Energy Supply', 'Energy Supply per Capita', '% Renewable'])
    Energy['Energy Supply'] = Energy['Energy Supply'].apply(lambda x: x*1000000)

    Energy['Country'] = Energy['Country'].str.replace(r" \(.*\)","")
    Energy['Country'] = Energy['Country'].str.replace(r"\d*","")
    Energy['Country'] = Energy['Country'].replace({'Republic of Korea' : 'South Korea',
                                               'United States of America' : 'United States',
                                               'United Kingdom of Great Britain and Northern Ireland':'United Kingdom',
                                               'China, Hong Kong Special Administrative Region':'Hong Kong'})
    
    GDP = pd.read_csv('assets/world_bank.csv', skiprows = 4)
    GDP['Country Name'] = GDP['Country Name'].replace({'Korea, Rep.': 'South Korea', 
                                                       'Iran, Islamic Rep.': 'Iran', 
                                                       'Hong Kong SAR, China' : 'Hong Kong'}) 
    
    ScimEn = pd.read_excel('assets/scimagojr-3.xlsx')
    
    merge1 = pd.merge(ScimEn,Energy,how="inner",left_on="Country",right_on="Country")
    merge1 = merge1[merge1["Rank"]<=15]
    
    GDP.rename(columns = {"Country Name":"Country"},inplace=True)
    GDP = GDP.loc[:,['2006', '2007', '2008', '2009', '2010', '2011', '2012', '2013', '2014', '2015',"Country"]]
    merge2 = pd.merge(merge1,GDP,how="inner",left_on="Country",right_on="Country").set_index("Country")
    
    return merge2
    # YOUR CODE HERE
    # raise NotImplementedError()
```

```python
your_ans = answer_1c()

assert isinstance(your_ans, pd.DataFrame), "Q1c: Your function should return a DataFrame."

assert your_ans.shape == (15, 20), "Q1c: Your resultant DataFrame should have 20 columns and 15 entries."

assert list(your_ans.columns) == ['Rank', 'Documents', 'Citable documents', 'Citations', 'Self-citations',
                                 'Citations per document', 'H index', 'Energy Supply','Energy Supply per Capita', '% Renewable', 
                                 '2006', '2007', '2008','2009', '2010', '2011', '2012', '2013', '2014', '2015'] , "Q1c: The column names should be as specified in the question. "
del your_ans
```
**Note: all subsequent questions rely on the DataFrame returned by your function in Question 1(c) above.**

### Question 2
What is the average GDP over the last 10 years for each country?

*This function should return a Series named `avgGDP` with 15 countries and their average GDP sorted in descending order.*

```python
def answer_two():
    # Competency: indexing, math fn, sorting
    info=answer_1c()
    
    return info[["2006","2007","2008","2009","2010","2011","2012","2013","2014","2015"]].apply(np.mean,axis = 1).sort_values(ascending = False)
    # YOUR CODE HERE
    # raise NotImplementedError()
```

```python
your_ans = answer_two()

assert isinstance(your_ans, pd.Series), "Q2: You should return a Series. "
assert your_ans.name == "avgGDP", "Q2: Your Series should have the correct name. "

del your_ans
```
### Question 3

By how much had the GDP changed over the 10 year span for the country with the 6th largest average GDP?

*This function should return a single number.*

```python
def answer_three():
    # Competency: indexing, broadcasting
    pre_info = answer_1c()
    pre_info['avgGDP'] = pre_info[["2006","2007","2008","2009","2010","2011","2012","2013","2014","2015"]].apply(np.mean,axis = 1)
    pre_info.sort_values(['avgGDP'], ascending=False, inplace=True)
    
    return pre_info.iloc[5]['2015']-pre_info.iloc[5]['2006']
    # YOUR CODE HERE
    # raise NotImplementedError()
```
### Question 4

What is the mean energy supply per capita?

*This function should return a single number.*

```python
def answer_four():
    # Competency: math fn
    pre_info = answer_1c()

    return pre_info['Energy Supply per Capita'].mean()
    # YOUR CODE HERE
    # raise NotImplementedError()
```
