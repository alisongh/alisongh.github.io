---
layout: post
title: SIADS 532 
description: Data Mining I Assignment 2 Part II
date: 2021-04-09
image: '/images/11.jpg'
tags: [data-mining, python, massive datasets]
---
* 目录
{:toc}

## Assignment 2: Mining Itemsets (Part II)

### Finding Frequent Itemsets with Apriori

In Part I of this assignment, the summary statistics gave us a brief view of what the data look like. Now it is time for the real business - let's use the  _Apriori_  algorithm to find the frequent itemsets. First, let's import the packages and dependencies that will be used in this part of the assignment.
{% highlight python linenos %}
import pandas as pd
import numpy as np
from sklearn.preprocessing import MultiLabelBinarizer

from mlxtend.frequent_patterns import apriori
{% endhighlight %}
**NOTE: These are all the imports we need to make for this assignment. You should not make other imports in your submitted notebook. You will receive 0 points for the exercises if your solution includes additional imports.**

Before you practice Apriori on the tasty emojis, let's use another dataset as an example to get familiar with the algorithm. As you can see, this is a data set of shopping history - every row is a shopping basket and every column is a product item.
{% highlight python linenos %}
market_df  =  pd.read_csv('assets/shopping_basket.csv')  
market_df.head()
{% endhighlight %}
{% highlight python linenos %}
print(market_df.shape)
{% endhighlight %}
`(7501, 119)`
We can call the Apriori API now and specify the minimal support we want. You may learn more about this API from its  [documentation](http://rasbt.github.io/mlxtend/user_guide/frequent_patterns/apriori/).
{% highlight python linenos %}
market_frequent_itemsets  =  apriori(market_df,  min_support=0.005,  use_colnames=True)  market_frequent_itemsets.head()
{% endhighlight %}
With the above command, we find all itemsets with a min support of 0.005 (half percent of the shopping baskets). We can now use the following command to extract the frequent itemsets with length 2 and beyond.
{% highlight python linenos %}
market_frequent_itemsets[market_frequent_itemsets['itemsets'].apply(lambda x: len(x)) > 1].head()
{% endhighlight %}
** Now, it is time to apply the Apriori algorithm to the emoji dataset.**
Since we have already shown you how to transform the Tweets into emoji itemsets, we concatenate the data preprocessing code into one block. Please run the following code block to load and preprocess the data.
{% highlight python linenos %}
tweets_df = pd.read_csv("assets/food_drink_emoji_tweets.txt", sep="\t", header=None)
tweets_df.columns = ['text']

emoji_list = "🍇🍈🍉🍊🍋🍌🍍🥭🍎🍏🍐🍑🍒🍓🥝🍅🥥🥑🍆🥔🥕🌽🌶🥒🥬🥦🍄🥜🌰🍞🥐🥖🥨🥯🥞🧀🍖🍗🥩🥓🍔🍟🍕🌭🥪🌮🌯🥙🥚🍳🥘🍲🥣🥗🍿🧂🥫🍱🍘🍙🍚🍛🍜🍝🍠🍢🍣🍤🍥🥮🍡🥟🥠🥡🦀🦞🦐🦑🍦🍧🍨🍩🍪🎂🍰🧁🥧🍫🍬🍭🍮🍯🍼🥛☕🍵🍶🍾🍷🍸🍹🍺🍻🥂🥃"
emoji_set = set(emoji_list)

tweets_df['emojis'] = tweets_df.text.apply(lambda text:np.unique([chr for chr in text if chr in emoji_set]))

mlb = MultiLabelBinarizer()
emoji_matrix = pd.DataFrame(data=mlb.fit_transform(tweets_df.emojis), index=tweets_df.index, columns=mlb.classes_)
{% endhighlight %}
{% highlight python linenos %}
emoji_matrix.sample(5)
{% endhighlight %}

### Exercise 2. (20 pts)
Complete the following  `emoji_frequent_itemsets`  function to find all the  **frequent  _k_-itemsets**  with a minimal support of  **`min_support`**  in the emoji dataset.

Your function should return a Pandas DataFrame object similar to the  `market_frequent_itemsets`  object above. The DataFrame object should have two columns:

-   The first one is named  `support`  and stores the support of the frequent itemsets.
-   The second column is named  `itemsets`  and stores the frequent itemset as a  _frozenset_  (the default return type of the apriori API).

Make sure that you are only returning the frequent itemsets that have the specified number of emojis.
{% highlight python linenos %}
market_frequent_itemsets.sample(5)
{% endhighlight %}
{% highlight python linenos %}
def emoji_frequent_itemsets(emoji_matrix, min_support=0.005, k=3):
    itemsets = apriori(emoji_matrix, min_support, use_colnames=True)
    itemsets = itemsets[itemsets['itemsets'].apply(lambda x: len(x)) == k]
    return itemsets
    # YOUR CODE HERE
    # raise NotImplementedError()
{% endhighlight %}
If you implemented this function correctly, we can obtain all frequent 3-itemsets with a min support of 0.005 by running the following command.
{% highlight python linenos %}
emoji_frequent_3itemsets = emoji_frequent_itemsets(emoji_matrix, min_support=0.005, k=3)
# You can uncomment the following line to view the obtained frequent itemsets.
# emoji_frequent_3itemsets
{% endhighlight %}
{% highlight python linenos %}
  
# This cell test whether the `emoji_frequent_itemsets` function is implemented correctly.  # We hide some tests, so passing all the displayed assertions does not guarantee full points.  emoji_frequent_3itemsets  =  emoji_frequent_itemsets(emoji_matrix,  min_support=0.005,  k=3)  for  row  in  emoji_frequent_3itemsets.itertuples():  assert  row.support  >=  0.005,  f"[Exercise 2] The support of the itemset {row.itemsets} is below the threshold."  assert  len(row.itemsets)  ==  3,  f"[Exercise 2] The itemset {row.itemsets} is not a 3-itemset."
{% endhighlight %}
If you are interested, you may also examine what the frequent 4-itemsets look like. Does the result make sense to you? (This part will not be graded.)
{% highlight python linenos %}
emoji_frequent_4itemsets = emoji_frequent_itemsets(emoji_matrix, min_support=0.005, k=4)
emoji_frequent_4itemsets
{% endhighlight %}
