---
title: SIADS 505 Data Manipulation Assignment 2
date: 2021-01-19
categories:
- Python
comment: true
---
* 目录
{:toc}

**Assignment 1**
[Check previous assignment](https://alisongh.github.io/2021/01/12/SIADS-505-Data-manipulation-assignment1.html)

# Assignment 2
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
