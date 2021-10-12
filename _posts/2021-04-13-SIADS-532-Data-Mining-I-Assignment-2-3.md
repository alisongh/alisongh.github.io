---
layout: post
title: SIADS 532 
description: Data Mining I Assignment 2 Part III
date: 2021-04-13
image: '/images/11.jpg'
tags: [data-mining, python, massive datasets]
---
* 目录
{:toc}

## Assignment 2: Mining Itemsets (Part III)

### Apriori Algorithm under the Hood

In part III of the assignment, we are going to continue our exploration in mining frequent itemsets. Specifically, we are going to examine a few key steps in the Apriori algorithm.

First, let's import the packages and dependencies that will be used in this part of the assignment.
{% highlight python linenos %}
import  pandas  as  pd  
import  numpy  as  np  
from  sklearn.preprocessing  import  MultiLabelBinarizer
{% endhighlight %}
**NOTE: These are all the imports we need to make for this assignment (Part III). You should not make other imports in your submitted notebook. You will receive 0 points for the exercises if your solution includes additional imports.**

##### A critical step of the Apriori algorithm is  _candidate generation_. That is, candidate  _(k+1)_-itemsets should be generated from frequent  _k_-itemsets. In the following exercise, we want you to generate candidate 3-itemsets based on the frequent 2-itemsets.

### Exercise 3 (25 pts)
**Candidate Generation**  (20 pts)
You will need to complete the  `generate_candidate_3_itemsets`  function, which takes in a list of frequent 2-itemsets and returns a list of the candidate 3-itemsets ("candidate" means that they may or may not be frequent). Please represent an itemset as a  `set`  in Python. Make sure that for each candidate 3-itemset in your returned list,  **at least one**  of its size-2 subset is a frequent 2-itemset, and your list does not contain duplicated itemsets.

We have prepared the frequent 2-itemsets for you, which we will help you load from the file named  `food_emoji_frequent_2_itemsets.csv`. We will evaluate your function by feeding in the loaded 2-itemsets and examining the return value. Note that the loaded 2-itemsets are different from what you will get with the  `emoji_frequent_itemsets`  function you implemented in Part II, as we have eliminated the drink-related emojis to make the exercise more trackable.

You will receive 20 points if every candidate 3-itemset in your returned list is a  **superset of at least one**  frequent 2-itemset, every 3-itemset that has a frequent size-2 subset is already in your list, and your list does not contain duplicated sets.

Note that this pruning procedure won't give us the smallest set of candidates. In the following challenge exercise, we will further prune the candidate itemsets.

**Challenge Exercise**  (5 pts)
We can further prune the candidate itemsets by requiring  **all**  size-2 subsets of each candidate 3-itemset to be frequent.

Think about the example you've seen in the lecture. Suppose {🍺, 🍼}, {🍺, 🍋}, {🍼, 🍭}, {🍼, 🍋} are all the frequent 2-itemsets. In the lecture, we said {🍺, 🍼, 🍋}, {🍺, 🍼, 🍭}, {🍼, 🍭, 🍋} are candidate 3-itemsets because each 3-itemset is a superset of  **at least one**  frequent 2-itemset. However, a larger itemset can never be frequent as long as one of its subset is not frequent. In this case, {🍺, 🍼, 🍭} can never been frequent because {🍺, 🍭} is not frequent. Neither can {🍼, 🍭, 🍋}, as the subset {🍭, 🍋} is not frequent. Ideally, we should be able to exclude the two candidate 3-itemsets {🍺, 🍼, 🍭} and {🍼, 🍭, 🍋} even without scanning the database for counting.

For this challenge exercise, please prune your generated candidate 3-itemsets by requiring all their subsets to be frequent. You will receive 5 points if the pruning is done correctly.
{% highlight python linenos %}
def combination_k(s, k):
    if not isinstance(s, list):
        s = list(s)

    if k == 0:
        return []

    if k == 1:
        return s

    sub_sets = []
    for i in range(len(s)):
        for item in combination_k(s[i+1:], k-1):
            sub_set = [s[i]]
            sub_set += item
            sub_sets.append(sub_set)

    return sub_sets

for i in combination_k(list("ABCD"), 3):
    print(i)
{% endhighlight %}
{% highlight linenos %}
['A', 'B', 'C'] 
['A', 'B', 'D'] 
['A', 'C', 'D'] 
['B', 'C', 'D']
{% endhighlight %}
{% highlight python linenos %}
def  generate_candidate_3_itemsets(base_itemsets):  
	candidates  =  []  candidate_items  =  set()  
	for  itemset  in  base_itemsets:  
		for  i  in  itemset:  
			candidate_items.add(i)  
			
	for  candidate  in  combination_k(candidate_items,  3):  
		one_not  =  False  
		for  twoitem  in  combination_k(candidate,  2):  
			if  set(twoitem)  not  in  base_itemsets:  
				one_not  =  True  
				break  
			if  not  one_not:  
				candidates.append(set(candidate))  
	return  candidates
{% endhighlight %}
{% highlight python linenos %}
test_itemsets = [
    {'A', 'B'},
    {'A', 'E'},
    {'B', 'D'},
    {'B', 'E'}
]
generate_candidate_3_itemsets(test_itemsets)
{% endhighlight %}
{% highlight linenos %}
[{'A', 'B', 'E'}]
{% endhighlight %}
{% highlight python linenos %}
frequent_2_itemsets = []
with open('assets/food_emoji_frequent_2_itemsets.csv', 'r', encoding='utf8') as fin:
    for line in fin:
        frequent_2_itemsets.append(set([line[0], line[1]]))

candidate_3_itemsets = generate_candidate_3_itemsets(frequent_2_itemsets)
# You can uncomment the following line to preview the generated candidate itemsets.
candidate_3_itemsets
{% endhighlight %}
{% highlight python linenos %}
[{'🍉', '🍊', '🍋'},
 {'🍊', '🍋', '🍓'},
 {'🍉', '🍋', '🍓'},
 {'🍊', '🍍', '🥝'},
 {'🍇', '🍊', '🥝'},
 {'🍉', '🍊', '🥝'},
 {'🍇', '🍊', '🍍'},
 {'🍉', '🍊', '🍍'},
 {'🍊', '🍍', '🍓'},
 {'🍊', '🍍', '🍏'},
 {'🍉', '🍊', '🍑'},
 {'🍊', '🍑', '🍓'},
 {'🍇', '🍉', '🍊'},
 {'🍇', '🍊', '🍓'},
 {'🍇', '🍊', '🍎'},
 {'🍉', '🍊', '🍌'},
 {'🍊', '🍌', '🍓'},
 {'🍉', '🍊', '🍓'},
 {'🍉', '🍊', '🍎'},
 {'🍊', '🍏', '🍓'},
 {'🍊', '🍎', '🍓'},
 {'🍊', '🍎', '🍏'},
 {'🍦', '🍧', '🍨'},
 {'🍦', '🍧', '🎂'},
 {'🍦', '🍧', '🍰'},
 {'🍦', '🍩', '🍪'},
 {'🍦', '🍩', '🍫'},
 {'🍦', '🍩', '🍰'},
 {'🍦', '🍨', '🎂'},
 {'🍦', '🍨', '🍫'},
 {'🍦', '🍨', '🍰'},
 {'🍔', '🍕', '🍦'},
 {'🍦', '🍭', '🎂'},
 {'🍦', '🍫', '🍭'},
 {'🍦', '🍭', '🍰'},
 {'🍦', '🍫', '🎂'},
 {'🍦', '🍰', '🎂'},
 {'🍦', '🍪', '🍫'},
 {'🍦', '🍪', '🍰'},
 {'🍦', '🍫', '🍰'},
 {'🍇', '🍍', '🥝'},
 {'🍉', '🍍', '🥝'},
 {'🍇', '🍉', '🥝'},
 {'🍇', '🍉', '🍍'},
 {'🍇', '🍍', '🍓'},
 {'🍉', '🍍', '🍓'},
 {'🍍', '🍏', '🍓'},
 {'🍉', '🍑', '🍒'},
 {'🍑', '🍒', '🍓'},
 {'🍉', '🍑', '🍓'},
 {'🍬', '🍭', '🎂'},
 {'🍫', '🍬', '🍭'},
 {'🍬', '🍭', '🍰'},
 {'🍫', '🍬', '🎂'},
 {'🍬', '🍰', '🎂'},
 {'🍫', '🍬', '🍰'},
 {'🍧', '🍨', '🎂'},
 {'🍧', '🍨', '🍰'},
 {'🍧', '🍰', '🎂'},
 {'🍇', '🍉', '🍒'},
 {'🍇', '🍒', '🍓'},
 {'🍇', '🍎', '🍒'},
 {'🍇', '🍉', '🍓'},
 {'🍇', '🍉', '🍎'},
 {'🍇', '🍎', '🍓'},
 {'🍉', '🍒', '🍓'},
 {'🍉', '🍎', '🍒'},
 {'🍎', '🍒', '🍓'},
 {'🍔', '🍕', '🍟'},
 {'🍔', '🍗', '🍟'},
 {'🍕', '🍗', '🍟'},
 {'🌭', '🍔', '🍕'},
 {'🍩', '🍪', '🍫'},
 {'🍩', '🍪', '🍰'},
 {'🍩', '🍫', '🍰'},
 {'🍨', '🍫', '🎂'},
 {'🍨', '🍰', '🎂'},
 {'🍨', '🍫', '🍰'},
 {'🍔', '🍕', '🍗'},
 {'🌮', '🍔', '🍕'},
 {'🍉', '🍌', '🍓'},
 {'🍫', '🍭', '🎂'},
 {'🍭', '🍰', '🎂'},
 {'🍫', '🍭', '🍰'},
 {'🍉', '🍎', '🍓'},
 {'🍎', '🍏', '🍓'},
 {'🍫', '🍰', '🎂'},
 {'🍪', '🍫', '🍰'}]
{% endhighlight %}
{% highlight python linenos %}
# This code block tests whether the `generate_candidate_3_itemsets` function is implemented correctly.
# We hide some tests, so passing all the displayed assertions does not guarantee full points.

# load from file
frequent_2_itemsets = []
with open('assets/food_emoji_frequent_2_itemsets.csv', 'r', encoding='utf=8') as fin:
    for line in fin:
        frequent_2_itemsets.append(set([line[0], line[1]]))

# obtain return value
candidate_3_itemsets = generate_candidate_3_itemsets(frequent_2_itemsets)

# assertions
examined = []
for candidate in candidate_3_itemsets:
    assert len(candidate) == 3, f"[Exercise 3] Candidate {candidate} not a 3-itemset"
    assert candidate not in examined, f"[Exercise 3] Duplicate 3-itemsets: {candidate}"
    emoji1, emoji2, emoji3 = list(candidate)
    assert (set([emoji1, emoji2]) in frequent_2_itemsets or 
                set([emoji1, emoji3]) in frequent_2_itemsets or
                set([emoji2, emoji3]) in frequent_2_itemsets),\
        f"[Exercise 3] Candidate {candidate} does not contain frequent 2-itemsets"
    examined.append(candidate)
{% endhighlight %}
{% highlight python linenos %}
# This code block tests whether the `generate_candidate_3_itemsets` function can perform the 
# additional pruning for the challenge exercise.  
# We hide some tests, so passing all the displayed assertions does not guarantee full points.  # load from file  
frequent_2_itemsets  =  []  
with  open('assets/food_emoji_frequent_2_itemsets.csv',  'r',  encoding='utf=8')  as  fin:  
	for  line  in  fin:  
		frequent_2_itemsets.append(set([line[0],  line[1]]))  
# obtain the return value  
candidate_3_itemsets  =  generate_candidate_3_itemsets(frequent_2_itemsets)  
# assertions  
examined  =  []  
for  candidate  in  candidate_3_itemsets:  
	if  len(candidate)  !=  3:  
		assert  False,  f"[Exercise 3 (Challenge Exercise)] Candidate {candidate} not a 3-itemset"  
		break  
	assert  candidate  not  in  examined,  f"[Exercise 3 (Challenge Exercise)] Duplicate 3-itemsets: {candidate}"  emoji1,  emoji2,  emoji3  =  list(candidate)  
	assert  (set([emoji1,  emoji2])  in  frequent_2_itemsets  and  set([emoji1,  emoji3])  in  frequent_2_itemsets  and  set([emoji2,  emoji3])  in  frequent_2_itemsets),\ f"[Exercise 3 (Challenge Exercise)] Candidate {candidate} contains infrequent 2-itemsets"  
	examined.append(candidate)
{% endhighlight %}
Before heading to the next exercise, please run the following code block to load the data set prepared in Part I of this assignment.
{% highlight python linenos %}
tweets_df  =  pd.read_csv("assets/food_drink_emoji_tweets.txt",  sep="\t",  header=None)  
tweets_df.columns  =  ['text']  
emoji_list  =  "🍇🍈🍉🍊🍋🍌🍍🥭🍎🍏🍐🍑🍒🍓🥝🍅🥥🥑🍆🥔🥕🌽🌶🥒🥬🥦🍄🥜🌰🍞🥐🥖🥨🥯🥞🧀🍖🍗🥩🥓🍔🍟🍕🌭🥪🌮🌯🥙🥚🍳🥘🍲🥣🥗🍿🧂🥫🍱🍘🍙🍚🍛🍜🍝🍠🍢🍣🍤🍥🥮🍡🥟🥠🥡🦀🦞🦐🦑🍦🍧🍨🍩🍪🎂🍰🧁🥧🍫🍬🍭🍮🍯🍼🥛☕🍵🍶🍾🍷🍸🍹🍺🍻🥂🥃"  
emoji_set  =  set(emoji_list)  

def  extract_uniq_emojis(text):  
	return  
tweets_df['emojis']  =  tweets_df.text.apply(lambda  text:np.unique([chr  for  chr  in  text  if  chr  in  emoji_set]))  

mlb  =  MultiLabelBinarizer()  
emoji_matrix  =  pd.DataFrame(data=mlb.fit_transform(tweets_df.emojis),  index=tweets_df.index,  columns=mlb.classes_)
{% endhighlight %}
#### Exercise 4 (15 pts)
With the candidate itemsets ready, the final step in one iteration of the Apriori algorithm is to scan the database (in our case, you can either scan the  `emoji_matrix`  or  `tweets_df`) and count the occurrence of each candidate itemset, divide it by the total number of records to derive the support, and output candidate itemsets whose support meets the threshold.

Please complete the  `calculate_frequent_itemsets`  function below. The input (`candidate_itemsets`) is a list of candidate 3-itemsets. Your function should return a complete list of frequent 3-itemsets with a minimal support of  `min_support`. The returned list should not contain duplicated itemsets. The order of the list does not matter.
{% highlight python linenos %}
def func(itemset, tweets_emojis):
    count = 0
    for emojis in tweets_emojis:
        if (itemset.issubset(emojis)):
            count += 1
    return count
    

def calculate_frequent_itemsets(candidate_itemsets, min_support=0.005):
    data = {'itemsets': candidate_itemsets}
    df = pd.DataFrame(data)
    df['count'] = df.apply(lambda x: func(x['itemsets'], tweets_df['emojis']), axis=1)
    total_count = len(tweets_df.index)
    df['support'] = df['count'].apply(lambda x: x / total_count)
    df = df[df['support'] >= min_support]
    return df['itemsets'].tolist()
{% endhighlight %}
{% highlight python linenos %}
calculate_frequent_itemsets(candidate_3_itemsets)
{% endhighlight %}
{% highlight linenos %}
[{'🍇', '🍉', '🍊'}, 
{'🍇', '🍉', '🍍'}, 
{'🍇', '🍉', '🥝'}, 
{'🍇', '🍊', '🍍'}, 
{'🍇', '🍊', '🥝'}, 
{'🍇', '🍍', '🥝'}, 
{'🍉', '🍊', '🍍'}, 
{'🍉', '🍊', '🥝'}, 
{'🍉', '🍍', '🥝'}, 
{'🍊', '🍍', '🥝'}, 
{'🍔', '🍕', '🍟'}, 
{'🍦', '🍧', '🍨'}, 
{'🍫', '🍬', '🍭'}, 
{'🍫', '🍰', '🎂'}]
{% endhighlight %}
{% highlight python linenos %}
# This code block tests whether the `calculate_frequent_itemsets` function work as expected.
# We hide some tests, so passing all the displayed assertions does not guarantee full points.

answer = calculate_frequent_itemsets(candidate_3_itemsets)

assert len(answer) == 14
assert {'🍇', '🍉', '🍊'} in answer
{% endhighlight %}
