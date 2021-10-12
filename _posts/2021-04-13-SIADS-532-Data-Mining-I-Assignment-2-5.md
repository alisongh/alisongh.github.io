---
layout: post
title: SIADS 532 
description: Data Mining I Assignment 2 Part V
date: 2021-04-13
image: '/images/11.jpg'
tags: [data-mining, python, massive datasets]
---
* 目录
{:toc}

## Assignment 2: Mining Itemsets (Part V)

### Itemset Similarity

Recall from Week 1 that pattern and similarity are two basic outputs of data mining. So far, we have been playing with patterns - frequent itemsets and association rules can all be seen as "patterns". In the last part, let's work on itemset similarities.

#### Exercise 6. Jaccard Similarity (20 pts)

Jaccard similarity is a simple but powerful measurement of itemset similarity, defined as follows:

$$
\begin{aligned}
  &\text{Jaccard_similarity(A, B)} = \frac{|A\cap B|}{|A\cup B|}
\end{aligned}
$$

##### Exercise 6(a) (10 pts)

Complete the following function to calculate the Jaccard similarity between two sets, you may assume that at least one of the sets is not empty.
{% highlight python linenos %}
def jaccard_similarity(set_a, set_b):
    # YOUR CODE HERE
    return len(set_a.intersection(set_b)) / len(set_a.union(set_b))
    # raise NotImplementedError()
{% endhighlight %}
{% highlight python linenos %}
# This code block tests whether the `jaccard_similarity` function work as expected.
# We hide some tests, so passing all the displayed assertions does not gurantee the bonus points.

assert abs(jaccard_similarity(set(['🍇', '🍈', '🍉']), set(['🍊', '🍋', '🍌'])) - 0) < 1e-8
assert abs(jaccard_similarity(set(['🍇', '🍈', '🍎']), set(['🍊', '🍋', '🍎'])) - 0.2) < 1e-8
assert abs(jaccard_similarity(set(['🍇', '🍒', '🍎']), set(['🍊', '🍒', '🍎'])) - 0.5) < 1e-8
assert abs(jaccard_similarity(set(['🍓', '🍒', '🍎']), set(['🍒', '🍎', '🍓'])) - 1) < 1e-8
{% endhighlight %}
##### Exercise 6(b) (10 pts)

A few questions to help you better understand the properties of Jaccard similarity:

1.  What is the range of Jaccard similarity?
2.  When is the max/min Jaccard similarity achieved?

Complete the following function to indicate the minimum and maximum value a Jaccard similarity score can achieve. (5 pts each).
{% highlight python linenos %}
# Please change min_value and max_value to your answers

min_value = 0
max_value = 1

# YOUR CODE HERE
# raise NotImplementedError()
{% endhighlight %}
With the Jaccard similarity calculated, we can now calculate the Jaccard similarity between any given Tweet with all other Tweets and find the Tweets that are most similar in terms of the set of food/drink emojis used. (Not graded.)
{% highlight python linenos %}
import pandas as pd
import numpy as np

tweets_df = pd.read_csv("assets/food_drink_emoji_tweets.txt", sep="\t", header=None)
tweets_df.columns = ['text']

emoji_list = "🍇🍈🍉🍊🍋🍌🍍🥭🍎🍏🍐🍑🍒🍓🥝🍅🥥🥑🍆🥔🥕🌽🌶🥒🥬🥦🍄🥜🌰🍞🥐🥖🥨🥯🥞🧀🍖🍗🥩🥓🍔🍟🍕🌭🥪🌮🌯🥙🥚🍳🥘🍲🥣🥗🍿🧂🥫🍱🍘🍙🍚🍛🍜🍝🍠🍢🍣🍤🍥🥮🍡🥟🥠🥡🦀🦞🦐🦑🍦🍧🍨🍩🍪🎂🍰🧁🥧🍫🍬🍭🍮🍯🍼🥛☕🍵🍶🍾🍷🍸🍹🍺🍻🥂🥃"
emoji_set = set(emoji_list)

def extract_uniq_emojis(text):
    return 

tweets_df['emojis'] = tweets_df.text.apply(lambda text:np.unique([chr for chr in text if chr in emoji_set]))

tweets_df['jaccard'] = tweets_df.emojis.apply(lambda x:jaccard_similarity(set(tweets_df.loc[0].emojis), set(x)))
tweets_df.sort_values('jaccard',ascending=False).head(n=10)
{% endhighlight %}
** 🍾🍾 Congratulations 🎉🎉, you have now finished up all the exercises in this assignment!**
