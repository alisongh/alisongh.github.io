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

Most interestingness measurements, including the four shown in the data frame (_confidence_,  _lift_,  _leverage_, and  _conviction_), can be derived from the three support values. For example,![$$\text{confidence}=\frac{\text{support}}{\text{antecedent_support}},$$](https://render.githubusercontent.com/render/math?math=%5Ctext%7Bconfidence%7D%3D%5Cfrac%7B%5Ctext%7Bsupport%7D%7D%7B%5Ctext%7Bantecedent_support%7D%7D%2C&mode=display)and![$$\text{lift} =\frac{\text{confidence}}{\text{consequent_support}}=\frac{\text{support}}{\text{antecedent_support} * \text{consequent_support}}$$](https://render.githubusercontent.com/render/math?math=%5Ctext%7Blift%7D%20%3D%5Cfrac%7B%5Ctext%7Bconfidence%7D%7D%7B%5Ctext%7Bconsequent_support%7D%7D%3D%5Cfrac%7B%5Ctext%7Bsupport%7D%7D%7B%5Ctext%7Bantecedent_support%7D%20%2A%20%5Ctext%7Bconsequent_support%7D%7D&mode=display)

### Exercise 5. (15 pts)[](https://render.githubusercontent.com/view/ipynb?color_mode=light&commit=3e2b370d2327decf05dfd498ec3a2e10d42fa780&enc_url=68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f616c69736f6e67682f4d4144532f336532623337306432333237646563663035646664343938656333613265313064343266613738302f53494144532532303533322f41737369676e6d656e74253230322f61737369676e6d656e74325f70617274342e6970796e62&nwo=alisongh%2FMADS&path=SIADS+532%2FAssignment+2%2Fassignment2_part4.ipynb&repository_id=337142879&repository_type=Repository#Exercise-5.-(15-pts))

In this exercise, we are going to implement another interestingness measurement, the (full) mutual information, and add a 'mutual information' column to the data frame. The measurement is defined as

![$$I(X;Y)=\sum_{x\in\mathcal{X}}\sum_{y\in\mathcal{Y}} P(X=x, Y=y)\log_2\frac{P(X=x,Y=y)}{P(X=x)P(Y=y)}.$$](https://render.githubusercontent.com/render/math?math=I%28X%3BY%29%3D%5Csum_%7Bx%5Cin%5Cmathcal%7BX%7D%7D%5Csum_%7By%5Cin%5Cmathcal%7BY%7D%7D%20P%28X%3Dx%2C%20Y%3Dy%29%5Clog_2%5Cfrac%7BP%28X%3Dx%2CY%3Dy%29%7D%7BP%28X%3Dx%29P%28Y%3Dy%29%7D.&mode=display)

Note that the logorithm requirest that the joint probability  ![$P(X=x, Y=y) &gt; 0$](https://render.githubusercontent.com/render/math?math=P%28X%3Dx%2C%20Y%3Dy%29%20%26gt%3B%200&mode=inline), which does not hold for some  ![$(x, y)$](https://render.githubusercontent.com/render/math?math=%28x%2C%20y%29&mode=inline). However, since we know that when  ![$P(X=x, Y=y) = 0$](https://render.githubusercontent.com/render/math?math=P%28X%3Dx%2C%20Y%3Dy%29%20%3D%200&mode=inline), it would not contribute to the sum, you may assume  ![$P(X=x, Y=y)\log_2\frac{P(X=x,Y=y)}{P(X=x)P(P=y)} = 0$](https://render.githubusercontent.com/render/math?math=P%28X%3Dx%2C%20Y%3Dy%29%5Clog_2%5Cfrac%7BP%28X%3Dx%2CY%3Dy%29%7D%7BP%28X%3Dx%29P%28P%3Dy%29%7D%20%3D%200&mode=inline)  in that case.

![$x$](https://render.githubusercontent.com/render/math?math=x&mode=inline),  ![$y$](https://render.githubusercontent.com/render/math?math=y&mode=inline)  are possible values of  ![$X$](https://render.githubusercontent.com/render/math?math=X&mode=inline)  and  ![$Y$](https://render.githubusercontent.com/render/math?math=Y&mode=inline); in the case of appearance or absence of an item, 1 or 0. Therefore, we need to consider all possible combinations of  ![$x$](https://render.githubusercontent.com/render/math?math=x&mode=inline)  and  ![$y$](https://render.githubusercontent.com/render/math?math=y&mode=inline), that is,  ![$(X=1, Y=1)$](https://render.githubusercontent.com/render/math?math=%28X%3D1%2C%20Y%3D1%29&mode=inline),  ![$(X=1, Y=0)$](https://render.githubusercontent.com/render/math?math=%28X%3D1%2C%20Y%3D0%29&mode=inline),  ![$(X=0, Y=1)$](https://render.githubusercontent.com/render/math?math=%28X%3D0%2C%20Y%3D1%29&mode=inline),  ![$(X=0, Y=0)$](https://render.githubusercontent.com/render/math?math=%28X%3D0%2C%20Y%3D0%29&mode=inline).

Please complete the following function that uses the three support values to compute the mutual information. All the three parameters are in [0, 1], and you can assume the validity of the input.  **Use 2 as the log base.**  We have created some auxilary variables for you, each represent a joint or marginal (let  ![$X$](https://render.githubusercontent.com/render/math?math=X&mode=inline)  denote the antecedent item and  ![$Y$](https://render.githubusercontent.com/render/math?math=Y&mode=inline)  denote the consequent item) probability.
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
