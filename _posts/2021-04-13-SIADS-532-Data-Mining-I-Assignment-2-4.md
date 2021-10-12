---
layout: post
title: SIADS 532 
description: Data Mining I Assignment 2 Part IV
date: 2021-04-13
image: '/images/11.jpg'
tags: [data-mining, python, massive datasets]
---
* 目录
{:toc}

## Assignment 2: Mining Itemsets (Part IV)

### Evaluating Frequent Itemsets

Even though we have found all the frequent itemsets, not all of them are interesting. In Part IV of this assignment, we will practice how to evaluate the frequent itemsets.

First, let's import the packages and dependencies that will be used in this part of assignment 2.
{% highlight python linenos %}
import pandas as pd
import numpy as np
from sklearn.preprocessing import MultiLabelBinarizer

from mlxtend.frequent_patterns import apriori
from mlxtend.frequent_patterns import association_rules
{% endhighlight %}
**NOTE: These are all the imports we need to make for this assignment (Part IV). You should not make other imports in your submitted notebook. You will receive 0 points for the exercises if your solution includes additional imports.**

People have developed various measurements of the interestingness of patterns. Most of them split the itemset into an antecedent item(set) and a consequent item(set), and then measure the correlation between the antecedent and the consequent. Let's try some of such measurements implemented by the  `mlxtend.frequent_patterns.association_rules`  API, which we have imported. For more information about the API, visit the  [documentation](http://rasbt.github.io/mlxtend/user_guide/frequent_patterns/association_rules/)  of the  `mlxtend`  package.

Let's again use the shopping basket dataset as an example.
{% highlight python linenos %}
market_df = pd.read_csv('assets/shopping_basket.csv')
market_frequent_itemsets = apriori(market_df, min_support=0.005, use_colnames=True)
{% endhighlight %}
{% highlight python linenos %}
interestingness_measurements = association_rules(market_frequent_itemsets, metric="lift", min_threshold=0)
interestingness_measurements.head()
{% endhighlight %}
  
In the returned data frame, each row examines one (antecedent -> consequent) pair.  _Antecedent support_  and  _consequent support_  measure  _P_(antecedent) and  _P_(consequent), while  _support_  measures  _P_(antecedent, consequent). In fact, these three values help us characterize the  ![$2\times2$](https://render.githubusercontent.com/render/math?math=2%5Ctimes2&mode=inline)  contingency table, as illustrated in the following table:

|  |  |  |  |
|--|--|--|--|
|  |X=1|X=0|sum(row)|
|Y=1|`support`|  |`cons_support`|
|Y=0|  |  |  |
|sum(col.)|`ante_support`|  |  |

Most interestingness measurements, including the four shown in the data frame (_confidence_,  _lift_,  _leverage_, and  _conviction_), can be derived from the three support values. For example,$$\text{confidence}=\frac{\text{support}}{\text{antecedent_support}},$$and$$\text{lift} =\frac{\text{confidence}}{\text{consequent_support}}=\frac{\text{support}}{\text{antecedent_support} * \text{consequent_support}}$$
### Exercise 5. (15 pts)

In this exercise, we are going to implement another interestingness measurement, the (full) mutual information, and add a 'mutual information' column to the data frame. The measurement is defined as

$$I(X;Y)=\sum_{x\in\mathcal{X}}\sum_{y\in\mathcal{Y}} P(X=x, Y=y)\log_2\frac{P(X=x,Y=y)}{P(X=x)P(Y=y)}.$$

Note that the logorithm requirest that the joint probability  $P(X=x, Y=y) &gt; 0$, which does not hold for some  $(x, y)$. However, since we know that when  $P(X=x, Y=y) = 0$, it would not contribute to the sum, you may assume  $P(X=x, Y=y)\log_2\frac{P(X=x,Y=y)}{P(X=x)P(P=y)} = 0$ in that case.

$x$,  $y$ are possible values of  $X$  and  $Y$; in the case of appearance or absence of an item, 1 or 0. Therefore, we need to consider all possible combinations of  $x$  and  $y$, that is,  $(X=1, Y=1)$,  $(X=1, Y=0)$,  $(X=0, Y=1)$,  $(X=0, Y=0)$.

Please complete the following function that uses the three support values to compute the mutual information. All the three parameters are in [0, 1], and you can assume the validity of the input.  **Use 2 as the log base.**  We have created some auxilary variables for you, each represent a joint or marginal (let  $X$ denote the antecedent item and  $Y$  denote the consequent item) probability.
{% highlight python linenos %}
  
# PMI = log(p(a,b) / ( p(a) * p(b) ))  
def  pmi(pab,  pa,  pb): 
	if  pab  ==  0:  
		return  0  
	else:  
		return  pab  *  np.log2(pab  /  (pa  *  pb))  

def  mi(antecedent_support,  consequent_support,  support):  
	px1  =  antecedent_support  
	px0  =  1  -  antecedent_support  
	py1  =  consequent_support  
	py0  =  1  -  consequent_support  
	px1y1  =  support  
	px1y0  =  px1  -  px1y1  
	px0y1  =  py1  -  px1y1  
	px0y0  =  1  -  px1  -  py1  +  px1y1  
	mutual_information  =  pmi(px1y1,  px1,  py1)  +  pmi(px1y0,  px1,  py0)  +  pmi(px0y1,  px0,  py1)  +  pmi(px0y0,  px0,  py0)  
# YOUR CODE HERE  
# raise NotImplementedError()  return  mutual_information
{% endhighlight %}
{% highlight python linenos %}
  
# This code block tests whether the `mi` function work as expected.  
# We hide some tests, so passing all the displayed assertions does not guarantee the bonus points.  

assert  np.abs(mi(0.6,  0.75,  0.4)  -  0.04287484674660057)  <  1e-8  
assert  np.abs(mi(0.5,  0.5,  0.25)  -  0)  <  1e-8  
# If you fail the following assertion, double check if your function 
# handles the scenarios in which a joint probability is zero.  
assert  np.abs(mi(0.5,  0.5,  0.5)  -  1)  <  1e-8
{% endhighlight %}
Mutual information is a classical measure of interestingness. We encourage you to think about the following questions (not graded):

1.  What is the maximum of mutual information in this setting? How to reach it?
2.  What is the minimum of mutual information in this setting? How to reach it?

What does the max/min value imply?

With the  `mi`  function, we can now compute the mutual information for each (antecedent -> consequent) pair and attach it to the data frame. Does the result make sense?
{% highlight python linenos %}
interestingness_measurements['mi'] = \
    interestingness_measurements.apply(lambda pair: mi(pair['antecedent support'], 
                                              pair['consequent support'], 
                                              pair['support']),
                                       axis=1)
interestingness_measurements.sort_values('mi', ascending=False).head(n=5)
{% endhighlight %}
