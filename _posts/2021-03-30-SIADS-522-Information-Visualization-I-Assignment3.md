---
layout: post
title: SIADS 522 Information Visualization I Assignment 3
date: 2021-07-01
image: '/images/09.jpg'
tags: [data visualization, python, altair]
---
* 目录
{:toc}

**Assignment 2**

[Check previous assignment](https://alisongh.com/altair/2021/03/22/SIADS-522-Information-Visualization-I-Assignment2/)

Before you turn this problem in, make sure everything runs as expected. First, **restart the kernel** (in the menubar, select Kernel&#8594;Restart) and then **run all cells** (in the menubar, select Kernel&#8594;Run All). Make sure your notebook executed to the end.

Make sure you fill in any place that says `YOUR CODE HERE` or "YOUR ANSWER HERE." Please remember that homeworks are to be completed independently. You may not share code with others.

## Week 3
- Perception / Cognition

## Assignment Overview
### This assignment's objectives include:

- Review, refect, and apply the concepts of the perception pipeline. Justify how different encodings impact the effectiveness of a visualization depending on the human perception process.

!["Drowing"](assets/preattentive_resized.png)

<p style="text-align: center;"> Preattentive Processing </p>

- Recreate visualizations and propose new and alternative visualizations using [Altair](https://altair-viz.github.io/) 

### The total score of this assignment will be 100 points consisting of:
- Case study reflection: America’s Favorite ‘Star Wars’ Movies (And Least Favorite Characters) (30 points)
- Altair programming exercise (70 points)

### Resources:
- Article by [FiveThirtyEight](https://fivethirtyeight.com) available  [online](https://fivethirtyeight.com/features/americas-favorite-star-wars-movies-and-least-favorite-characters/) (Hickey, 2014)  
- Datasets from FiveThirtyEight, we have downloaded a subset of this data in the folder [./assets](assets)
    - The original dataset can be found at [FiveThirtyEight Star Wars Survey](https://github.com/fivethirtyeight/data/tree/master/star-wars-survey)
    
    
### Important notes:
1) Grading for this assignment is entirely done by manual inspection. Focus on getting the visualization to look like our example. It doesn't need to be pixel perfect (e.g., you may not always know what our example is scaled by), but it should be pretty close. Hint: go back to lab in week 2 on altair for some styling help. A *lot* of the look and feel can be done in one line of code.

2) There will be a couple of places where the numbers you get when you select rows may be a little different than 538, but the percents should still work (e.g., 828 instead of 834). You'll see this in our examples. If you can somehow get the data to match exactly, that's great too.

3) When turning in your PDF, please use the File -> Print -> Save as PDF option ***from your browser***. Do ***not*** use the File->Download as->PDF option. Complete instructions for this are under Resources in the Coursera page for this class.

## Part 1. Perception and Cognition (30 points)
Read the article ["America’s Favorite ‘Star Wars’ Movies (And Least Favorite Characters),"](https://fivethirtyeight.com/features/americas-favorite-star-wars-movies-and-least-favorite-characters/) and answer the following questions:

### 1.1 List the different data types in the following visualizations and their encodings (10 points)
Look at the following visualizations. For each, list the variable, their type, and the encoding used (e.g., Weight, quantitative, color, ...)

!["Drowing"](assets/how_rate_resized.png)
!["Drowing"](assets/have_seen_resized.png)

### 1.2 Propose an alternative encoding for the following visualization. Compare the visualizations based on perception. (10 points)
Either hand-draw or use an application to create a sketched solution. Upload an image and describe the differences between your solution and the FiveThirtyEight image in terms of perception (specifically for the task of comparing one movie to another).
!["Drowing"](assets/have_seen_resized.png)

I create an alternative encoding as a radial chart. The only difference between the bar chart and the radial chart is that the radial chart integrates all information in single image. They have same function of comparison and the audiences can access the number of each movie.

!["Drowing"](assets/my_image_1.png)

### 1.3 Propose an alternative encoding for the following visualization. Compare the visualizations based on perception. (10 points)
Again, either-hand draw or use an application to create a sketched solution. Upload an image and describe the differences between your solution and the FiveThirtyEight image in terms of perception (specifically for the task of comparing one movie to another).
!["Drowing"](assets/how_rate_resized.png)

Same as 1.2, I still chose radial chart to represent this aspect. However, I don't believe radial chart performs well comparing with the bar chart. Even though all information integrates into one single image, it is difficult to understand for audiences because it takes time to distinguish meanings of different colors.

!["Drowing"](assets/my_image_1_1.png)

## Part 2. Altair programming exercise (70 points)
We have provided you with some code and parts of the article [America’s Favorite ‘Star Wars’ Movies (And Least Favorite Characters)](https://fivethirtyeight.com/features/americas-favorite-star-wars-movies-and-least-favorite-characters/). This article is based on the dataset:

1. [StarWars](data/StarWars.csv) Created by FiveThirtyEight based on a survey ran through SurveyMonkey Audience, surveying 1,186 respondents from June 3 to 6 2014. Available [online] (https://github.com/fivethirtyeight/data/tree/master/star-wars-survey)

To earn points for this assignment, you must:

- Recreate the visualizations in the article (replace the images in the article with a code cell that creates a visualization). We provide one example. Each visualization is worth 10 points (40 points/ 10 each x 4 total ).

    - _Partial credit can be granted for each visualization (up to 5 points) if you provide the grammar of graphics description of the visualization without a functional Altair implementation_


- Propose one alternative visualization for one of the article visualizations. Add a short paragraph describing why your visualization is more *effective* based on principles of perception/cognition. (15 points/ 10 points plot + 5 justification)


- Propose a new visualization to complement a part of the article. Add a short paragraph justifying your decisions in terms of Perception/Cognition processes.  (15 points/ 10 points plot + 5 justification)

```python
import pandas as pd
import altair as alt
import numpy as np
import math
```

```python
# enable correct rendering
alt.renderers.enable('default')

# uses intermediate json files to speed things up
alt.data_transformers.enable('json')
```
```python
sw = pd.read_csv('assets/StarWars.csv', encoding='latin1')
```
```python
# Some format is needed for the survey dataframe, we provide the formatted dataset in a dataframe 
sw = sw.rename(columns={'Have you seen any of the 6 films in the Star Wars franchise?':'seen_any_movie',
                        'Do you consider yourself to be a fan of the Star Wars film franchise?': 'fan',
                        'Which of the following Star Wars films have you seen? Please select all that apply.' : 'seen_EI',
                        'Unnamed: 4' : 'seen_EII',
                        'Unnamed: 5' : 'seen_EIII',
                        'Unnamed: 6' : 'seen_EIV',
                        'Unnamed: 7' : 'seen_EV',
                        'Unnamed: 8' : 'seen_EVI',
                        'Please rank the Star Wars films in order of preference with 1 being your favorite film in the franchise and 6 being your least favorite film.' : 'rank_EI',
                        'Unnamed: 10' : 'rank_EII',
                        'Unnamed: 11' : 'rank_EIII',
                        'Unnamed: 12' : 'rank_EIV',
                        'Unnamed: 13' : 'rank_EV',
                        'Unnamed: 14' : 'rank_EVI',
                        'Please state whether you view the following characters favorably, unfavorably, or are unfamiliar with him/her.' : 'fav_HS',
                        'Unnamed: 16' : 'fav_LS',
                        'Unnamed: 17' : 'fav_PLO',
                        'Unnamed: 18' : 'fav_AS',
                        'Unnamed: 19' : 'fav_OWK',
                        'Unnamed: 20' : 'fav_EP',
                        'Unnamed: 21' : 'fav_DV',
                        'Unnamed: 22' : 'fav_LC',
                        'Unnamed: 23' : 'fav_BF',
                        'Unnamed: 24' : 'fav_C3',
                        'Unnamed: 25' : 'fav_RD',
                        'Unnamed: 26' : 'fav_JJB',
                        'Unnamed: 27' : 'fav_PA',
                        'Unnamed: 28' : 'fav_Y',
                       })
sw = sw.drop([0])
```
```python
# take a peak to look at the data
sw.sample(5)
```
# America’s Favorite ‘Star Wars’ Movies (And Least Favorite Characters)_Original article available at [FiveThirtyEight](https://fivethirtyeight.com/features/americas-favorite-star-wars-movies-and-least-favorite-characters/)_

By [Walt Hickey](https://fivethirtyeight.com/contributors/walt-hickey/)

Filed under [Movies](https://fivethirtyeight.com/tag/movies/)

Get the data on [GitHub](https://github.com/fivethirtyeight/data/tree/master/star-wars-survey)

This week, I caught a sneak peek [of the X-Wing fighter](http://www.wired.com/2014/07/star-wars-episode-vii-x-wing/) from the new “Star Wars” films in production. The forthcoming movies — and the middling response to the most recent trilogy — provide a perfect excuse to examine some questions I’ve long wanted answers to: How many people are “Star Wars” fans? Does the rest of America realize that “The Empire Strikes Back” is clearly the best of the bunch? Which characters are most well-liked and most hated? And who shot first, Han Solo or Greedo?

We ran a poll through [SurveyMonkey Audience](https://www.surveymonkey.com/mp/audience/), surveying 1,186 respondents from June 3 to 6 (the [data](https://github.com/fivethirtyeight/data/tree/master/star-wars-survey) is available [on GitHub](https://github.com/fivethirtyeight/data)). Seventy-nine percent of those respondents said they had watched at least one of the “Star Wars” films. This question, incidentally, had a substantial difference by gender: 85 percent of men have seen at least one “Star Wars” film compared to 72 percent of women. Of people who have seen a film, men were also more likely to consider themselves a fan of the franchise: 72 percent of men compared to 60 percent of women.

We then asked respondents which of the films they had seen. With 835 people responding, here’s the probability that someone has seen a given “Star Wars” film given that they have seen any Star Wars film:

!["Sol1"](assets/have_seen_resized.png)

```python
# Sample visualization

# We're going to fix the labels a bit so will create a mapping to the full names
episodes = ['EI', 'EII', 'EIII', 'EIV', 'EV', 'EVI']
names = {
    'EI' : 'The Phantom Meanance', 'EII' : 'Attack of the clones', 'EIII' : 'Revenge of the Sith', 
    'EIV': 'A New Hope', 'EV': 'The Empire Strikes Back', 'EVI' : 'The Return of the Jedi'
}

# we're also going to use this order to sort, so names_l will now have our sort order
names_l = [names[ep] for ep in episodes]

print("sort order: ",names_l)
```
**Output**

total who have seen at least one:  835

```python
# let's do some data pre-processing... sw (star wars) has everything

# We want to only use those people who have seen at least one movie, let's get the people, toss NAs
# and get the total count

# find people who have at least on of the columns (seen_*) not NaN
seen_at_least_one = sw.dropna(subset=['seen_' + ep for ep in episodes],how='all')
total = len(seen_at_least_one)

print("total who have seen at least one: ", total)
```
```python
# ok, time to make the chart... let's make a bar chart (use mark_bar)
bars = alt.Chart(seen_per_df).mark_bar(size=20).encode(
    # encode x as the percent, and hide the axis
    x=alt.X(
        'Percentage',
        axis=None),
    y=alt.Y(
        # encode y using the name, use the movie name to label the axis, sort using the names_l
        'Name:N',
         axis=alt.Axis(tickCount=5, title=''),
         # we give the sorting order to avoid alphabetical order
         sort=names_l
    )
)

# at this point we don't really have a great plot (it's missing the annotations, titles, etc.)
bars
```
```python
# we're going to overlay the text with the percentages, so let's make another visualization
# that's just text labels

text = bars.mark_text(
    align='left',
    baseline='middle',
    dx=3  # Nudges text to right so it doesn't appear on top of the bar
).encode(
    # we'll use the percentage as the text
    text=alt.Text('Percentage:Q',format='.0%')
)

# finally, we're going to combine the bars and the text and do some styling
seen_movies = (text + bars).configure_mark(
    # we don't love the blue
    color='#008fd5'
).configure_view(
    # we don't want a stroke around the bars
    strokeWidth=0
).configure_scale(
    # add some padding
    bandPaddingInner=0.2
).properties(
    # set the dimensions of the visualization
    width=500,
    height=180
).properties(
    # add a title
    title="Which 'Star Wars' Movies Have you Seen?"
)

seen_movies

# note that we are NOT formatting this in the Five Thirty Eight Style yet... we'll leave that to you to figure out
```

So we can see that “Star Wars: Episode V — The Empire Strikes Back” is the film seen by the most number of people, followed by “Star Wars: Episode VI — Return of the Jedi.” Appallingly, more people reported seeing “Star Wars: Episode I — The Phantom Menace” than the original “Star Wars” (renamed “Star Wars: Episode IV — A New Hope”).

So, which movie is the best? We asked the subset of 471 respondents who indicated they have seen every “Star Wars” film to rank them from best to worst. From that question, we calculated the share of respondents who rated each film as their favorite.

!["Sol1"](assets/best_movie_article_resized.png)


** Homework note: Click [here](assets/best_movie.png) to see a version of this plot generated in Altair.

### 2.1 What's the best 'Star Wars' movie? Recreate the above image using altair (10 POINTS)

```python
# let's do some data pre-processing... sw (star wars) has everything

# We want to only use those people who have seen at least one movie, let's get the people, toss NAs
# and get the total count

# find people who have at least on of the columns (seen_*) not NaN
seen_all_movies = sw.dropna(subset=['seen_' + ep for ep in episodes],how='any')
total = len(seen_all_movies)

print("total who have seen all movies: ", total)
```
```python
# for each movie, we're going to calculate the percents and generate a new data frame
percs = []

# loop over each column and calculate the number of people who have seen the movie
# specifically, filter out the people who are *NaN* for a specific episode (e.g., ep_EII), count them
# and divide by the percent
for rank_ep in [f'rank_' + ep for ep in episodes]:
    perc = seen_all_movies[seen_all_movies[rank_ep] == '1'].shape[0] / total
    percs.append(perc)
    
# at this point percs is holding our percentages

# now we're going use a trick to make tuples--pairing names with percents--using "zip" and then make a dataframe
tuples = list(zip([names[ep] for ep in episodes],percs))
rank_per_df = pd.DataFrame(tuples, columns = ['Name', 'Percentage'])
rank_per_df
```

```python
# ok, time to make the chart... let's make a bar chart (use mark_bar)
bars = alt.Chart(rank_per_df).mark_bar(size=20).encode(
    # encode x as the percent, and hide the axis
    x=alt.X(
        'Percentage',
        axis=None),
    y=alt.Y(
        # encode y using the name, use the movie name to label the axis, sort using the names_l
        'Name:N',
         axis=alt.Axis(tickCount=5, title=''),
         # we give the sorting order to avoid alphabetical order
         sort=names_l
    )
)

# at this point we don't really have a great plot (it's missing the annotations, titles, etc.)
bars
```

```python
# we're going to overlay the text with the percentages, so let's make another visualization
# that's just text labels

text = bars.mark_text(
    align='left',
    baseline='middle',
    dx=3  # Nudges text to right so it doesn't appear on top of the bar
).encode(
    # we'll use the percentage as the text
    text=alt.Text('Percentage:Q',format='.0%')
)

# finally, we're going to combine the bars and the text and do some styling
best_movies = (text + bars).configure_view(
    # we don't want a stroke around the bars
    # remove border
    strokeWidth=0
).configure_scale(
    # add some padding
    bandPaddingInner=0.2
).properties(
    # set the dimensions of the visualization
    width=500,
    height=180
).properties(
    # add a title
    title={
    "text":["What's the best 'Star War' Movie?"],
    "subtitle":["Of 471 respondents who have seen all six films"]},
).configure(
    # customize background color
    background="whitesmoke"
).configure_title(
    # customize title and sub-title
    fontSize=20, anchor='start', fontWeight=700, subtitleFontWeight=300
).configure_view(
    # remove border
    strokeOpacity=0
).configure_axis(
    # remove axis line
    grid=False, domain=False
).configure_mark(
    color='#008FD5'
)


best_movies

# note that we are NOT formatting this in the Five Thirty Eight Style yet... we'll leave that to you to figure out
```

## Make sure to *style* your visualization to match the original the best you can

We can also drill down and find out, generally, how people rate the films. Overall, fans broke into two camps: those who preferred the original three movies and those who preferred the three prequels. People who said “The Empire Strikes Back” was their favorite were also likely to rate “A New Hope” and “Return of the Jedi” higher as well. Those who rated “The Phantom Menace” as the best film were more likely to rate prequels higher.

This chart shows how often each film was rated in the top third (best or second-best), the middle third (third or fourth) or the bottom third (second-worst or worst). It’s a more nuanced take on the series:

!["Sol1"](assets/how_rate_resized.png)

** Homework note: Click [here](assets/people_rate.png) to see a version of this plot generated in Altair.

### 2.2 How people rate the 'Star Wars' movie? Recreate the above image using altair (10 POINTS)

```python
# let's do some data pre-processing... sw (star wars) has everything

# We want to only use those people who have seen at least one movie, let's get the people, toss NAs
# and get the total count

# find people who have at least on of the columns (seen_*) not NaN
seen_all_movies = sw.dropna(subset=['seen_' + ep for ep in episodes],how='any')
total = len(seen_all_movies)

print("total who have seen all movies: ", total)
```

```python
# for each movie, we're going to calculate the percents and generate a new data frame
percs = []

# loop over each column and calculate the number of people who have seen the movie
# specifically, filter out the people who are *NaN* for a specific episode (e.g., ep_EII), count them
# and divide by the percent
for rank_ep in [f'rank_' + ep for ep in episodes]:
    perc = (seen_all_movies[seen_all_movies[rank_ep] == '1'].shape[0] + seen_all_movies[seen_all_movies[rank_ep] == '2'].shape[0]) / total
    percs.append(perc)
    
# at this point percs is holding our percentages

# now we're going use a trick to make tuples--pairing names with percents--using "zip" and then make a dataframe
tuples = list(zip([names[ep] for ep in episodes],percs))
top3_df = pd.DataFrame(tuples, columns = ['Name', 'Percentage'])
top3_df
```
```python
# ok, time to make the chart... let's make a bar chart (use mark_bar)
top3_bars = alt.Chart(top3_df).mark_bar(size=20, color='#77AB43').encode(
    # encode x as the percent, and hide the axis
    x=alt.X(
        'Percentage',
        axis=None),
    y=alt.Y(
        # encode y using the name, use the movie name to label the axis, sort using the names_l
        'Name:N',
         axis=alt.Axis(tickCount=5, title=''),
         # we give the sorting order to avoid alphabetical order
         sort=names_l
    )
)

# at this point we don't really have a great plot (it's missing the annotations, titles, etc.)
top3_bars
```

```python
# we're going to overlay the text with the percentages, so let's make another visualization
# that's just text labels

text = top3_bars.mark_text(
    align='left',
    baseline='middle',
    dx=3  # Nudges text to right so it doesn't appear on top of the bar
).encode(
    # we'll use the percentage as the text
    text=alt.Text('Percentage:Q',format='.0%')
)

# finally, we're going to combine the bars and the text and do some styling
top3_movies = (text + top3_bars).properties(
    # set the dimensions of the visualization
    width=50,
    height=150
).properties(
    # add a title
    title={
    "text":["Top third"]},
)


top3_movies

# note that we are NOT formatting this in the Five Thirty Eight Style yet... we'll leave that to you to figure out
```

```python
# for each movie, we're going to calculate the percents and generate a new data frame
percs = []

# loop over each column and calculate the number of people who have seen the movie
# specifically, filter out the people who are *NaN* for a specific episode (e.g., ep_EII), count them
# and divide by the percent
for rank_ep in [f'rank_' + ep for ep in episodes]:
    perc = (seen_all_movies[seen_all_movies[rank_ep] == '3'].shape[0] + seen_all_movies[seen_all_movies[rank_ep] == '4'].shape[0]) / total
    percs.append(perc)
    
# at this point percs is holding our percentages

# now we're going use a trick to make tuples--pairing names with percents--using "zip" and then make a dataframe
tuples = list(zip([names[ep] for ep in episodes],percs))
mid3_df = pd.DataFrame(tuples, columns = ['Name', 'Percentage'])
mid3_df
```

```python
# ok, time to make the chart... let's make a bar chart (use mark_bar)
mid3_bars = alt.Chart(mid3_df).mark_bar(size=20, color='#008FD5').encode(
    # encode x as the percent, and hide the axis
    x=alt.X(
        'Percentage',
        axis=None),
    y=alt.Y(
        # encode y using the name, use the movie name to label the axis, sort using the names_l
        'Name:N',
         axis=None,
         # we give the sorting order to avoid alphabetical order
         sort=names_l
    )
)

# at this point we don't really have a great plot (it's missing the annotations, titles, etc.)
mid3_bars
```

```python
# we're going to overlay the text with the percentages, so let's make another visualization
# that's just text labels

text = mid3_bars.mark_text(
    align='left',
    baseline='middle',
    dx=3  # Nudges text to right so it doesn't appear on top of the bar
).encode(
    # we'll use the percentage as the text
    text=alt.Text('Percentage:Q',format='.0%')
)

# finally, we're going to combine the bars and the text and do some styling
mid3_movies = (text + mid3_bars).properties(
    # set the dimensions of the visualization
    width=50,
    height=150
).properties(
    # add a title
    title={
    "text":["Middle third"]},
)


mid3_movies

# note that we are NOT formatting this in the Five Thirty Eight Style yet... we'll leave that to you to figure out
```

```python
# for each movie, we're going to calculate the percents and generate a new data frame
percs = []

# loop over each column and calculate the number of people who have seen the movie
# specifically, filter out the people who are *NaN* for a specific episode (e.g., ep_EII), count them
# and divide by the percent
for rank_ep in [f'rank_' + ep for ep in episodes]:
    perc = (seen_all_movies[seen_all_movies[rank_ep] == '5'].shape[0] + seen_all_movies[seen_all_movies[rank_ep] == '6'].shape[0]) / total
    percs.append(perc)
    
# at this point percs is holding our percentages

# now we're going use a trick to make tuples--pairing names with percents--using "zip" and then make a dataframe
tuples = list(zip([names[ep] for ep in episodes],percs))
bot3_df = pd.DataFrame(tuples, columns = ['Name', 'Percentage'])
bot3_df
```

```python
# ok, time to make the chart... let's make a bar chart (use mark_bar)
bot3_bars = alt.Chart(bot3_df).mark_bar(size=20, color='red').encode(
    # encode x as the percent, and hide the axis
    x=alt.X(
        'Percentage',
        axis=None),
    y=alt.Y(
        # encode y using the name, use the movie name to label the axis, sort using the names_l
        'Name:N',
         axis=None,
         # we give the sorting order to avoid alphabetical order
         sort=names_l
    )
)

# at this point we don't really have a great plot (it's missing the annotations, titles, etc.)
bot3_bars
```

```python
# we're going to overlay the text with the percentages, so let's make another visualization
# that's just text labels

text = bot3_bars.mark_text(
    align='left',
    baseline='middle',
    dx=3  # Nudges text to right so it doesn't appear on top of the bar
).encode(
    # we'll use the percentage as the text
    text=alt.Text('Percentage:Q',format='.0%')
)

# finally, we're going to combine the bars and the text and do some styling
bot3_movies = (text + bot3_bars).properties(
    # set the dimensions of the visualization
    width=50,
    height=150
).properties(
    # add a title
    title={
    "text":["Bottom third"]},
)

bot3_movies

# note that we are NOT formatting this in the Five Thirty Eight Style yet... we'll leave that to you to figure out
```
```python
# Recreate this image using altair here (10 POINTS)

# YOUR CODE HERE
# raise NotImplementedError()
alt.hconcat(top3_movies, mid3_movies, bot3_movies).properties(
    # add a title
    title={
    "text":["How People Rate the 'Star Wars' Movies"],
    "subtitle":["How often each film was rated in the top, middle and bottom third",'(by 471 respondents who have seen all six films)',' ',' ']}
).configure_scale(
    # add some padding
    bandPaddingInner=1
).configure(
    # customize background color
    background="whitesmoke"
).configure_view(
    strokeWidth=0
).configure_axis(
    # remove axis line
    grid=False, domain=False
).configure_axis(
    labelFontSize=14
).configure_legend(
    gradientLength=400,
    gradientThickness=30
)
```

Finally, we took a boilerplate format used by political favorability polls — “Please state whether you view the following characters favorably, unfavorably, or are unfamiliar with him/her” — and asked respondents to rate characters in the series.

!["Sol1"](assets/char_ranking_resized.png)


** Homework note. Here's an example solution generated in Altair:
!["Sol1"](assets/people_rate_s.png)

### 2.3 Star Wars' Characters Favorability Ratings. Recreate the above image using altair (10 POINTS)

```python
actors = ['LS', 'HS','PLO','OWK','Y','RD','C3','AS','DV','LC','PA','BF','EP','JJB']
names = {
    'LS' : 'Luke Skywalker','HS' : 'Han Solo', 'PLO':'Princess Leia Organa', 'OWK': 'Obi Wan Kenobi', 'Y':'Yoda', 'RD':'R2 D2', 'C3':'C-3P0', 'AS' : 'Anakin Skywalker', 'DV' : 'Darth Vader', 'LC':'Lando Calrissian', 'PA':'Padme Amidala', 'BF':'Boba Fett', 'EP': 'Emperor Palpatine', 'JJB':'Jar Jar Binks'
}

# we're also going to use this order to sort, so names_l will now have our sort order
names_l = [names[ac] for ac in actors]

print("sort order: ",names_l)
```
```python

# let's do some data pre-processing... sw (star wars) has everything

# We want to only use those people who have seen at least one movie, let's get the people, toss NAs
# and get the total count

# find people who have at least on of the columns (seen_*) not NaN

seen_at_least_one = sw.dropna(subset=['fav_' + ac for ac in actors],how='all')
total = len(seen_at_least_one)

print("total who have seen at least one: ", total)
```
```python
# for each movie, we're going to calculate the percents and generate a new data frame
percs = []

# loop over each column and calculate the number of people who have seen the movie
# specifically, filter out the people who are *NaN* for a specific episode (e.g., ep_EII), count them
# and divide by the percent
for actor_ep in ['fav_' + ac for ac in actors]:
    perc = (seen_at_least_one[seen_at_least_one[actor_ep] == 'Very favorably'].shape[0] + seen_at_least_one[seen_at_least_one[actor_ep] == 'Somewhat favorably'].shape[0]) / total
    percs.append(perc)
    
# at this point percs is holding our percentages

# now we're going use a trick to make tuples--pairing names with percents--using "zip" and then make a dataframe
tuples = list(zip([names[ac] for ac in actors],percs))
fav_df = pd.DataFrame(tuples, columns = ['Name', 'Percentage'])
fav_df
```
```python

# ok, time to make the chart... let's make a bar chart (use mark_bar)
fav_bars = alt.Chart(fav_df).mark_bar(size=20, color='#77AB43').encode(
    # encode x as the percent, and hide the axis
    x=alt.X(
        'Percentage',
        axis=None),
    y=alt.Y(
        # encode y using the name, use the movie name to label the axis, sort using the names_l
        'Name:N',
         axis=alt.Axis(tickCount=5, title=''),
         # we give the sorting order to avoid alphabetical order
         sort=names_l
    )
)

# at this point we don't really have a great plot (it's missing the annotations, titles, etc.)
fav_bars
```
```python
# we're going to overlay the text with the percentages, so let's make another visualization
# that's just text labels

text = fav_bars.mark_text(
    align='left',
    baseline='middle',
    dx=3  # Nudges text to right so it doesn't appear on top of the bar
).encode(
    # we'll use the percentage as the text
    text=alt.Text('Percentage:Q',format='.0%')
)

# finally, we're going to combine the bars and the text and do some styling
fav_movies = (text + fav_bars).properties(
    # set the dimensions of the visualization
    width=50,
    height=400
).properties(
    # add a title
    title={
    "text":["Favorable"]},
)

fav_movies

# note that we are NOT formatting this in the Five Thirty Eight Style yet... we'll leave that to you to figure out
```
```python
# for each movie, we're going to calculate the percents and generate a new data frame
percs = []

# loop over each column and calculate the number of people who have seen the movie
# specifically, filter out the people who are *NaN* for a specific episode (e.g., ep_EII), count them
# and divide by the percent
for actor_ep in ['fav_' + ac for ac in actors]:
    perc = seen_at_least_one[seen_at_least_one[actor_ep] == 'Neither favorably nor unfavorably (neutral)'].shape[0] / total
    percs.append(perc)
    
# at this point percs is holding our percentages

# now we're going use a trick to make tuples--pairing names with percents--using "zip" and then make a dataframe
tuples = list(zip([names[ac] for ac in actors],percs))
neu_df = pd.DataFrame(tuples, columns = ['Name', 'Percentage'])
neu_df
```
```python

# ok, time to make the chart... let's make a bar chart (use mark_bar)
neu_bars = alt.Chart(neu_df).mark_bar(size=20, color='#008FD5').encode(
    # encode x as the percent, and hide the axis
    x=alt.X(
        'Percentage',
        axis=None),
    y=alt.Y(
        # encode y using the name, use the movie name to label the axis, sort using the names_l
        'Name:N',
         axis=None,
         # we give the sorting order to avoid alphabetical order
         sort=names_l)
)

# at this point we don't really have a great plot (it's missing the annotations, titles, etc.)
neu_bars
```
```python
# we're going to overlay the text with the percentages, so let's make another visualization
# that's just text labels

text = neu_bars.mark_text(
    align='left',
    baseline='middle',
    dx=3  # Nudges text to right so it doesn't appear on top of the bar
).encode(
    # we'll use the percentage as the text
    text=alt.Text('Percentage:Q',format='.0%')
)

# finally, we're going to combine the bars and the text and do some styling
neu_movies = (text + neu_bars).properties(
    # set the dimensions of the visualization
    width=50,
    height=400
).properties(
    # add a title
    title={
    "text":["Neutral"]},
)

neu_movies

# note that we are NOT formatting this in the Five Thirty Eight Style yet... we'll leave that to you to figure out
```
```python
# for each movie, we're going to calculate the percents and generate a new data frame
percs = []

# loop over each column and calculate the number of people who have seen the movie
# specifically, filter out the people who are *NaN* for a specific episode (e.g., ep_EII), count them
# and divide by the percent
for actor_ep in ['fav_' + ac for ac in actors]:
    perc = (seen_at_least_one[seen_at_least_one[actor_ep] == 'Very unfavorably'].shape[0] + seen_at_least_one[seen_at_least_one[actor_ep] == 'Somewhat unfavorably'].shape[0]) / total
    percs.append(perc)
    
# at this point percs is holding our percentages

# now we're going use a trick to make tuples--pairing names with percents--using "zip" and then make a dataframe
tuples = list(zip([names[ac] for ac in actors],percs))
unfav_df = pd.DataFrame(tuples, columns = ['Name', 'Percentage'])
unfav_df
```
```python

# ok, time to make the chart... let's make a bar chart (use mark_bar)
unfav_bars = alt.Chart(unfav_df).mark_bar(size=20, color='red').encode(
    # encode x as the percent, and hide the axis
    x=alt.X(
        'Percentage',
        axis=None),
    y=alt.Y(
        # encode y using the name, use the movie name to label the axis, sort using the names_l
        'Name:N',
         axis=None,
         sort=names_l
    )
)

# at this point we don't really have a great plot (it's missing the annotations, titles, etc.)
unfav_bars
```
```python
# we're going to overlay the text with the percentages, so let's make another visualization
# that's just text labels

text = unfav_bars.mark_text(
    align='left',
    baseline='middle',
    dx=3  # Nudges text to right so it doesn't appear on top of the bar
).encode(
    # we'll use the percentage as the text
    text=alt.Text('Percentage:Q',format='.0%')
)

# finally, we're going to combine the bars and the text and do some styling
unfav_movies = (text + unfav_bars).properties(
    # set the dimensions of the visualization
    width=50,
    height=400
).properties(
    # add a title
    title={
    "text":["Unfavorable"]},
)

unfav_movies

# note that we are NOT formatting this in the Five Thirty Eight Style yet... we'll leave that to you to figure out
```
```python

# for each movie, we're going to calculate the percents and generate a new data frame
percs = []

# loop over each column and calculate the number of people who have seen the movie
# specifically, filter out the people who are *NaN* for a specific episode (e.g., ep_EII), count them
# and divide by the percent
for actor_ep in ['fav_' + ac for ac in actors]:
    perc = seen_at_least_one[seen_at_least_one[actor_ep] == 'Unfamiliar (N/A)'].shape[0] / total
    percs.append(perc)
    
# at this point percs is holding our percentages

# now we're going use a trick to make tuples--pairing names with percents--using "zip" and then make a dataframe
tuples = list(zip([names[ac] for ac in actors],percs))
unfamiliar_df = pd.DataFrame(tuples, columns = ['Name', 'Percentage'])
unfamiliar_df
```
```python
# ok, time to make the chart... let's make a bar chart (use mark_bar)
unfamiliar_bars = alt.Chart(unfamiliar_df).mark_bar(size=20, color='#A0A0A0').encode(
    # encode x as the percent, and hide the axis
    x=alt.X(
        'Percentage',
        axis=None),
    y=alt.Y(
        # encode y using the name, use the movie name to label the axis, sort using the names_l
        'Name:N',
         axis=None,
         sort=names_l
    )
)

# at this point we don't really have a great plot (it's missing the annotations, titles, etc.)
unfamiliar_bars
```
```python
# we're going to overlay the text with the percentages, so let's make another visualization
# that's just text labels

text = unfamiliar_bars.mark_text(
    align='left',
    baseline='middle',
    dx=3  # Nudges text to right so it doesn't appear on top of the bar
).encode(
    # we'll use the percentage as the text
    text=alt.Text('Percentage:Q',format='.0%')
)

# finally, we're going to combine the bars and the text and do some styling
unfamiliar_movies = (text + unfamiliar_bars).properties(
    # set the dimensions of the visualization
    width=50,
    height=400
).properties(
    # add a title
    title={
    "text":["Unfamiliar"]},
)

unfamiliar_movies

# note that we are NOT formatting this in the Five Thirty Eight Style yet... we'll leave that to you to figure out
```
```python
# Recreate this image using altair here (10 POINTS)

# # YOUR CODE HERE
# raise NotImplementedError()
alt.hconcat(fav_movies, neu_movies, unfav_movies, unfamiliar_movies).properties(
    # add a title
    title={
    "text":["'Star Wars' Characters Favorability Ratings"],
    "subtitle":["By 834 respondents",' ',' ']}
).configure_scale(
    # add some padding
    bandPaddingInner=1
).configure(
    # customize background color
    background="whitesmoke"
).configure_view(
    strokeWidth=0
).configure_legend(
    gradientLength=400,
    gradientThickness=30
).configure_axis(
    # remove axis line
    grid=False, domain=False,
    titleFontSize=12,
    labelFontSize=12  
)
```

You read that correctly. Jar Jar Binks has a lower favorability rating than the actual personification of evil in the galaxy.

And for those of you who want to know the impact that [historical revisionism](http://en.wikipedia.org/wiki/Han_shot_first) can have on a society:

!["Sol1"](assets/shot_first_article_resized.png)


** Homework note: Click [here](assets/shot_first.png) to see a version of this plot generated in Altair. You may find that you don't get 834 rows (as 538 did) but the percents should still work.

### 2.4 Who shot first? Recreate the above image using altair (10 POINTS)

```python
total = sw['Which character shot first?'].count()
seen_any_movies = sw.dropna(subset=['seen_' + ep for ep in episodes],how='all')

print("{} respondents".format(total))
```
```python
# for each movie, we're going to calculate the percents and generate a new data frame
Han = seen_any_movies["Which character shot first?"] == 'Han'
count_Han = sum(Han)
Greedo = seen_any_movies["Which character shot first?"] == 'Greedo'
count_Greedo = sum(Greedo)
idk = seen_any_movies["Which character shot first?"] == "I don't understand this question"
count_idk = sum(idk)
# loop over each column and calculate the number of people who have seen the movie
# specifically, filter out the people who are *NaN* for a specific episode (e.g., ep_EII), count them
# and divide by the percent

perc_Han = count_Han / total
perc_Greedo = count_Greedo / total
perc_idk = count_idk / total
    
# at this point percs is holding our percentages
d = {'Name':["Han", "Greedo", "I don't understand this question"], "percentage":[perc_Han, perc_Greedo, perc_idk]}
char_df = pd.DataFrame(data=d)
char_df
```
```python
names =['Han','Greedo',"I don't understand this question"]
```
```python
# ok, time to make the chart... let's make a bar chart (use mark_bar)
shot_first_bars = alt.Chart(char_df).mark_bar(size=20, color='#008FD5').encode(
    # encode x as the percent, and hide the axis
    x=alt.X(
        'percentage',
        axis=None),
    y=alt.Y(
        # encode y using the name, use the movie name to label the axis, sort using the names_l
        'Name:N',
         axis=alt.Axis(tickCount=5, title=''),
         sort=names
    )
)

# at this point we don't really have a great plot (it's missing the annotations, titles, etc.)
shot_first_bars
```
```python
# Recreate this image using altair here (10 POINTS)

# YOUR CODE HERE
# raise NotImplementedError()

text = shot_first_bars.mark_text(
    align='left',
    baseline='middle',
    dx=3  # Nudges text to right so it doesn't appear on top of the bar
).encode(
    # we'll use the percentage as the text
    text=alt.Text('percentage:Q',format='.0%')
)

# finally, we're going to combine the bars and the text and do some styling
shot_first_movies = (text + shot_first_bars).configure_mark(
    # we don't love the blue
    color='#008FD5'
).configure_scale(
    # add some padding
    bandPaddingInner=0.1
).properties(
    # set the dimensions of the visualization
    width=500,
    height=100,
    # add a title
    title={
    "text":["Who shot first?"],
    "subtitle":["By 828 respondents",' ']}
).configure(
    # customize background color
    background="whitesmoke"
).configure_title(
    fontSize=20, anchor='start'
).configure_view(
    # we don't want a stroke around the bars
    strokeWidth=0
).configure_axis(
    # remove axis line
    grid=False, domain=False
)

shot_first_movies

# note that we are NOT formatting this in the Five Thirty Eight Style yet... we'll leave that to you to figure out
```
```python
print(total)
```
### 2.5.1 Make your own (15 points/ 10 points plot + 5 justification)

Propose and code an alternative visualization for one of the visualizations *already in the article*. Add a short paragraph describing why your visualization is more (or less) *effective* based on principles of perception/cognition. 

If you feel your visualization is worse, that's ok! Just tell us why.

```python
# Sample visualization

# We're going to fix the labels a bit so will create a mapping to the full names
episodes = ['EI', 'EII', 'EIII', 'EIV', 'EV', 'EVI']
names = {
    'EI' : 'The Phantom Meanance', 'EII' : 'Attack of the clones', 'EIII' : 'Revenge of the Sith', 
    'EIV': 'A New Hope', 'EV': 'The Empire Strikes Back', 'EVI' : 'The Return of the Jedi'
}

# we're also going to use this order to sort, so names_l will now have our sort order
names_l = [names[ep] for ep in episodes]

print("sort order: ",names_l)
```
```python

bot3_gender = pd.DataFrame({'Name':['The Phantom Meanance','The Phantom Meanance', 'Attack of the clones','Attack of the clones', 'Revenge of the Sith','Revenge of the Sith', 'A New Hope','A New Hope', 'The Empire Strikes Back','The Empire Strikes Back', 'The Return of the Jedi', 'The Return of the Jedi'], 'Gender':['Male','Female','Male','Female','Male','Female','Male','Female','Male','Female','Male','Female'], 'percentage':[0.125265, 0.193206, 0.116773, 0.369427, 0.080679, 0.475584, 0.012739, 0.316348, 0.019108, 0.184713, 0.023355, 0.212314]})
bot3_gender
```
```python

mid3_gender = pd.DataFrame({'Name':['The Phantom Meanance','The Phantom Meanance', 'Attack of the clones','Attack of the clones', 'Revenge of the Sith','Revenge of the Sith', 'A New Hope','A New Hope', 'The Empire Strikes Back','The Empire Strikes Back', 'The Return of the Jedi', 'The Return of the Jedi'], 'Gender':['Male','Female','Male','Female','Male','Female','Male','Female','Male','Female','Male','Female'], 'percentage':[0.046709, 0.433121, 0.053079, 0.343949, 0.093418, 0.316348, 0.059448, 0.265393, 0.027601, 0.146497, 0.097665, 0.248408]})
mid3_gender
```
```python
top3_gender = pd.DataFrame({'Name':['The Phantom Meanance','The Phantom Meanance', 'Attack of the clones','Attack of the clones', 'Revenge of the Sith','Revenge of the Sith', 'A New Hope','A New Hope', 'The Empire Strikes Back','The Empire Strikes Back', 'The Return of the Jedi', 'The Return of the Jedi'], 'Gender':['Male','Female','Male','Female','Male','Female','Male','Female','Male','Female','Male','Female'], 'percentage':[0.016985, 0.027601, 0.019108, 0.021231, 0.014862, 0.016985, 0.116773, 0.036093, 0.142251, 0.050955, 0.067941, 0.029724]})
top3_gender
```
```python
bar = alt.Chart(top3_gender).mark_bar().encode(
    x=alt.X('Name',title='Movie Name',sort=names_l),
    y=alt.Y('percentage:Q',title='Percentage',axis=None),
    color=alt.Color('Gender', title='Gender')
).properties(height=400, width=150)

text = alt.Chart(top3_gender).mark_text(color='black').encode(
    x=alt.X('Name:N'),
    y=alt.Y('percentage:Q', stack='zero'),
    text=alt.Text('percentage',format='.0%')
)

top3_bygender=(bar + text).properties(
    title=['Top Third']
)
top3_bygender
```
```python
bar = alt.Chart(mid3_gender).mark_bar().encode(
    x=alt.X('Name',title='Movie Name'),
    y=alt.Y('percentage:Q',axis=None,title='Percentage',),
    color='Gender'
).properties(height=400, width=150)

text = alt.Chart(mid3_gender).mark_text(color='black').encode(
    x=alt.X('Name:N'),
    y=alt.Y('percentage:Q', stack='zero'),
    text=alt.Text('percentage',format='.0%')
)

mid3_bygender=(bar + text).properties(
    title=['Middle Third']
)
mid3_bygender
```
```python

bar = alt.Chart(bot3_gender).mark_bar().encode(
    x=alt.X('Name',title='Movie Name'),
    y=alt.Y('percentage:Q',title='Percentage',axis=None),
    color='Gender'
).properties(height=400, width=150)

text = alt.Chart(bot3_gender).mark_text(color='black').encode(
    x=alt.X('Name:N'),
    y=alt.Y('percentage:Q', stack='zero'),
    text=alt.Text('percentage',format='.0%')
)

bot3_bygender=(bar + text).properties(
    title=['Bottom Third']
)
bot3_bygender
```
```python
alt.hconcat(top3_bygender, mid3_bygender, bot3_bygender).properties(
    title={
    "text":["How People Rate the 'Star Wars' Movies"],
    "subtitle":["How often each film was rated in the top, middle and bottom third by gender",'(by 471 respondents who have seen all six films)',' ',' ']}
).configure_view(
    # remove border
    strokeOpacity=0
).configure_axis(
    # remove axis line
    grid=False, domain=False
)
```
*Provide your justification here*

I add gender into the chart which contains more information. We can read that more male audiences vote for middle and bottom characters than female audiences, but it's probably caused by less female respondents than males. Besides, it is a little bit hard to interpret for human beings because movie names displays in vertically.

### 2.5.2 Make your own (15 points/ 10 points plot + 5 justification)
Propose and code a *new visualization* to complement a part of the article. Add a short paragraph justifying your decisions in terms of Perception/Cognition processes.

If you feel your visualization is worse, that's ok! Just tell us why.

