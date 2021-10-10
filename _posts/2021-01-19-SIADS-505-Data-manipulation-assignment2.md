---
layout: post
title: SIADS 505 Data Manipulation Assignment 2
date: 2021-01-19
image: '/images/04.jpg'
tags: [data, datamanipulation, python]
---
* 目录
{:toc}

**Assignment 1**

[Check previous assignment](https://alisongh.github.io/2021/01/12/SIADS-505-Data-manipulation-assignment1.html)

For this assignment you'll be looking at 2017 data on immunizations from the CDC. Your datafile for this assignment is in [assets/NISPUF17.csv](assets/NISPUF17.csv). A data users guide for this, which you'll need to map the variables in the data to the questions being asked, is available at [assets/NIS-PUF17-DUG.pdf](assets/NIS-PUF17-DUG.pdf). **Note: you may have to go to your Jupyter tree (click on the Coursera image) and navigate to the assignment 2 assets folder to see this PDF file).**

You can also use hint provided for this particular questions by clicking **"Show Hint"** button.

## Question 1
Write a function called `proportion_of_education` which returns the proportion of children in the dataset who had a mother with the education levels equal to less than high school (<12), high school (12), more than high school but not a college graduate (>12) and college degree.

*This function should return a dictionary in the form of (use the correct numbers, do not round numbers):* 
```
    {"less than high school":0.2,
    "high school":0.4,
    "more than high school but not college":0.2,
    "college":0.2}
```

```python
import pandas as pd
import numpy as np

df = pd.read_csv('assets/NISPUF17.csv', index_col = 0)
def proportion_of_education():
    edu = df['EDUC1']
    mum_edu = np.sort(edu.values)
    prop_edu = {"less than high school" : 0,
               "high school" : 0,
               "more than high school but not college" : 0,
               "college" : 0}
    yr = len(mum_edu)
    prop_edu["less than high school"] = np.sum(mum_edu == 1)/yr
    prop_edu["high school"] = np.sum(mum_edu == 2)/yr
    prop_edu["more than high school but not college"] = np.sum(mum_edu == 3)/yr
    prop_edu["college"] = np.sum(mum_edu == 4)/yr
    return prop_edu

    # your code goes here
    # YOUR CODE HERE
    # raise NotImplementedError()
```

```python
assert type(proportion_of_education())==type({}), "You must return a dictionary."
assert len(proportion_of_education()) == 4, "You have not returned a dictionary with four items in it."
assert "less than high school" in proportion_of_education().keys(), "You have not returned a dictionary with the correct keys."
assert "high school" in proportion_of_education().keys(), "You have not returned a dictionary with the correct keys."
assert "more than high school but not college" in proportion_of_education().keys(), "You have not returned a dictionary with the correct keys."
assert "college" in proportion_of_education().keys(), "You have not returned a dictionary with the correct keys."
```

## Question 2
It would be interesting to see if there is any evidence of a link between vaccine effectiveness and sex of the child. Calculate the ratio of the number of children who contracted chickenpox but were vaccinated against it (at least one varicella dose) versus those who were vaccinated but did not contract chicken pox. Return results by sex. 

*This function should return a dictionary in the form of (use the correct numbers):* 
```
    {"male":0.2,
    "female":0.4}
```

Note: To aid in verification, the `chickenpox_by_sex()['female']` value the autograder is looking for starts with the digits `0.00779`.

```python
import numpy as np
import pandas as pd

df = pd.read_csv('assets/NISPUF17.csv', index_col=0)

def chickenpox_by_sex():
    chc_sex = df[df['HAD_CPOX'].lt(3) & df['P_NUMVRC'].gt(0)].loc[:,['HAD_CPOX','SEX']]
    # HAD_CPOX: 1-Y; 2-N
    # SEX: 1-M; 2-F
    chc_1 = len(chc_sex[(chc_sex['HAD_CPOX'] == 1) & (chc_sex['SEX'] == 1)])
    chc_2 = len(chc_sex[(chc_sex['HAD_CPOX'] == 1) & (chc_sex['SEX'] == 2)])
    chn_1 = len(chc_sex[(chc_sex['HAD_CPOX'] == 2) & (chc_sex['SEX'] == 1)])
    chn_2 = len(chc_sex[(chc_sex['HAD_CPOX'] == 2) & (chc_sex['SEX'] == 2)])
    # chc_1: the number of male children who contracted chickenpox
    # chc_2: the number of female children who contracted chickenpox
    # chn_1: the number of male children who didn't contracted chickenpox
    # chn_2: the number of female children who didn't contracted chickenpox
    
    ratio_chc = {'male': 0,
                'female': 0}
    
    ratio_chc['male'] = chc_1/chn_1
    ratio_chc['female'] = chc_2/chn_2
    return ratio_chc
    
chickenpox_by_sex()['female']
    # YOUR CODE HERE
    # raise NotImplementedError()
```
**Output**
```
0.0077918259335489565
```

```python
assert len(chickenpox_by_sex())==2, "Return a dictionary with two items, the first for males and the second for females."
```

## Question 3
A correlation is a statistical relationship between two variables. If we wanted to know if vaccines work, we might look at the correlation between the use of the vaccine and whether it results in prevention of the infection or disease [1]. In this question, you are to see if there is a correlation between having had the chicken pox and the number of chickenpox vaccine doses given (varicella).

Some notes on interpreting the answer. The `had_chickenpox_column` is either `1` (for yes) or `2` (for no), and the `num_chickenpox_vaccine_column` is the number of doses a child has been given of the varicella vaccine. A positive correlation (e.g., `corr > 0`) means that an increase in `had_chickenpox_column` (which means more no's) would also increase the values of `num_chickenpox_vaccine_column` (which means more doses of vaccine). If there is a negative correlation (e.g., `corr < 0`), it indicates that having had chickenpox is related to an increase in the number of vaccine doses. 

Also, `pval` is the probability that we observe a correlation between `had_chickenpox_column` and `num_chickenpox_vaccine_column` which is greater than or equal to a particular value occurred by chance. A small `pval` means that the observed correlation is highly unlikely to occur by chance. In this case, `pval` should be very small (will end in `e-18` indicating a very small number).

[1] This isn't really the full picture, since we are not looking at when the dose was given. It's possible that children had chickenpox and then their parents went to get them the vaccine. Does this dataset have the data we would need to investigate the timing of the dose?

```python
def corr_chickenpox():
    import scipy.stats as stats
    import numpy as np
    import pandas as pd
    
    # this is just an example dataframe
    # df=pd.DataFrame({"had_chickenpox_column":np.random.randint(1,3,size=(100)),
                   #"num_chickenpox_vaccine_column":np.random.randint(0,6,size=(100))})

    # here is some stub code to actually run the correlation
    # corr, pval=stats.pearsonr(df["had_chickenpox_column"],df["num_chickenpox_vaccine_column"])
    
    # just return the correlation
    #return corr

    # YOUR CODE HERE
    df = pd.read_csv('assets/NISPUF17.csv', index_col = 0)
    df = df[df['HAD_CPOX'].lt(3)].loc[:,['HAD_CPOX','P_NUMVRC']].dropna()
    # Get Less than of dataframe and other, element-wise (binary operator lt)
    # Equivalent to ==, !=, <=, <, >=, > with support to choose axis (rows or columns) and level for comparison
    df.columns = ['had_chickenpox_column','num_chickenpox_vaccine_column']
    
    corr, pval = stats.pearsonr(df["had_chickenpox_column"], df["num_chickenpox_vaccine_column"])
    return corr
    # raise NotImplementedError()
```
```python
assert -1<=corr_chickenpox()<=1, "You must return a float number between -1.0 and 1.0."
```
