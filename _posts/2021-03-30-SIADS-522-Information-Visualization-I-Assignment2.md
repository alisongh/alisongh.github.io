---
layout: post
title: SIADS 522 
description: Information Visualization I Assignment 2
date: 2021-03-30
image: '/images/09.jpg'
tags: [data-visualization, python, altair]
---
* 目录
{:toc}

**Assignment 1**

[Check previous assignment](https://alisongh.com/altair/2021/03/22/SIADS-522-Information-Visualization-I-Assignment1/)

Before you turn this problem in, make sure everything runs as expected. First, **restart the kernel** (in the menubar, select Kernel&#8594;Restart) and then **run all cells** (in the menubar, select Kernel&#8594;Run All). Make sure your notebook executed to the end.

Make sure you fill in any place that says `YOUR CODE HERE` or "YOUR ANSWER HERE." Please remember that homeworks are to be completed independently. You may not share code with others.

## Week 2: 
- Expressiveness and Effectiveness
- Grammar of Graphics

## Assignment Overview
### Our objectives for this week:

- Review, reflect, and apply the concepts of encoding. Given a visualization recreate the data that was encoded.
- Review, reflect, and apply the concepts of Expressiveness and Effectiveness. Given a visualization, evaluate alternatives with the same expressiveness.

!["Drawing"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/data-table-resized.png?raw=true)


<p style="text-align: center;">Two visualizations, same expressiveness </p>

- Review and evaluate an implementation of Grammar of Graphics using [Altair](https://altair-viz.github.io/) 

### The total score of this assignment will be 100 points consisting of:
- Case study reflection: Next Bechdel Test (30 points)
- Altair programming exercise (70 points)
- Bonus (5 points)


### Resources:
- This article by [FiveThirtyEight](https://fivethirtyeight.com) available [online](https://projects.fivethirtyeight.com/next-bechdel/) (Hickey, Koeze, Dottle, Wezerek 2017)  
- Datasets from FiveThirtyEight, we have downloaded a subset of these datasets available in the folder for your use into [./assets](./assets)
    - The original dataset can be found on [FiveThirtyEight Next Bechdel Dataset](https://github.com/fivethirtyeight/data/tree/master/next-bechdel)
    
    
### Important notes:
1) Grading for this assignment is largely done by manual inspection. The autograder checks the basic details but is imperfect. Sometimes it will pass the test but not look right, sometimes it will look right and fail the test. That's fine! You should guide your answer by the look of our examples. It doesn't need to be pixel perfect (e.g., you may not always know what our example is scaled by), but it should be pretty close.

2) When turning in your PDF, please use the File -> Print -> Save as PDF option ***from your browser***. Do ***not*** use the File->Download as->PDF option. Complete instructions for this are under Resources in the Coursera page for this class.

3) Pay attention to the return types of your functions. 

## Part 1. Expressiveness and Effectiveness (30 points)
Read the following article [*Creating the next Bechdel Test*](https://projects.fivethirtyeight.com/next-bechdel/) and answer the following questions:

### 1.1 Recreate the table (by hand or excel) needed to create the following visualization (7 points)

You *should* consider the interactive parts of the visualization in your answer. Take a picture or screenshot of your table and add it to the answer below

!["Drawing"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/article_1.png?raw=true)


An easy way to upload images is to jump into the [./assets](./assets) directory (or use the Coursera notebook explorer and navigate to it) and then use the upload button to save your image:

![upload](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/upload.png?raw=true)

Once you have the image, you can link to it using the markdown command: `![answer1.2](assets/my_image_1.2.png)`

YOUR ANSWER HERE

!["answer1.2"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/my_image_1.2.png?raw=true)

### 1.2 Sketch an alternative visualization with the same expressiveness (7 points)
By hand is fine, but you can also use a tool. This is a sketch, the data need not be perfectly accurate or to scale. Again, upload a picture or screenshot below. Make sure there is enough annotation so it's clear why your picture has the same expressiveness.

YOUR ANSWER HERE

!["answer1.2"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/my_image_1.2_1.png?raw=true)


<p style="text-align: center;">When the mouse cursors move to the mark, it shows the name of movie and % of female automatically </p>

### 1.3 Sketch an alternative visualization with the same expressiveness of the following visualization (10 points)

!["article"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/article_2_resized.png?raw=true)


Same deal as last question: by hand or with a tool is fine. The data need not be perfectly accurate or to scale. Make sure there is enough annotation so it's clear why your picture has the same expressiveness. Again, upload a picture or screenshot below. 

!["answer1.2"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/my_image_1.2_2.png?raw=true)

<p style="text-align: center;">When the mouse cursors moves to the mark, it shows the list of tests automatically </p>

### 1.4 Reflect on which visualization you think is more *effective* and why? (6 points)

You are comparing the original figure in 1.3 and the one you created.

I think the figure I created is more effective. The domain of this project is to determine the relationship between number of actors and genders. For the original figure in 1.3, the author/researcher just make a flat list to check which tests the movie passed. The length of the list is a little bit long and the reader has to go back and forth to recheck the movie on the top of the list. 

Through visualization I created the reader can easily read the portion of test passing. When you move the mouse cursors to the chart, it shows up the list of tests automatically. Besides, my visualization saves space and the audience doesn't need to go back and forth. Last but not least, my visualization is very straghtforward and visualizaed even though it's not colorful.

## Part 2. Altair programming exercise (70 points)
We have provided some code to create visualizations based on the following datasets:

1. [all_tests](assets/nextBechdel_allTests.csv) Is a collection of different Bechdel test results for the 50 top-grossing films [at the domestic box office in 2016](https://www.the-numbers.com/box-office-records/domestic/all-movies/cumulative/released-in-2016)
2. [cast_gender](assets/nextBechdel_castGender.csv) Is the gender for all the cast member of each movie in the Bechdel rankings
3. [top_2016](assets/top_2016.csv) Is the date, box office and theater count for each top 2016 movie.

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
```python
# read all the tables
all_tests_df = pd.read_csv('assets/nextBechdel_allTests.csv')
cast_gender = pd.read_csv('assets/nextBechdel_castGender.csv')
top_2016 = pd.read_csv('assets/top_2016.csv')
```
```python
# set up the tables for use
actors_movies = top_2016.set_index('Movie').join(cast_gender.set_index('MOVIE')).join(all_tests_df.set_index('movie')).reset_index().dropna()
movies_order = top_2016.sort_values(by=['Rank'])['Movie'].tolist()
```

### 2.1 Variables encoded (5 points)
Warmup: complete the `variables_encoded` function.
Return the number of variables encoded in the following example

```python
base = alt.Chart(actors_movies).transform_filter(
    (alt.datum.TYPE != 'Unknown') & (alt.datum.GENDER != 'Unknown') & (alt.datum.GENDER != 'null')
)
```
```python
encoding = base.transform_filter(
    alt.datum.GENDER == 'Female'
).encode(
    y= alt.Y(
        'index:N',
        sort= movies_order
    ),
    x=alt.X('count(index):Q',
            title='cast count'),
)

def m_bar():
    return encoding.mark_bar().properties(title='Female')

bar = m_bar()
```
**Output**

![](https://github.com/alisongh/MADS/blob/86ffe7a29ea4bc8b4a5aaa46c721bba3f64959a8/SIADS%20522/Assignment%202/assets/m_bar.png?raw=true)

### 2.2 Alternative encoding (5 points)
Complete the `m_circle` function. Change the encoding used in the previous example from a bar to a circle. Your visualization should look like 

!["Drawing"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/alt_enc_1_resized.png?raw=true)


```python
base = alt.Chart(actors_movies).transform_filter(
    (alt.datum.TYPE != 'Unknown') & (alt.datum.GENDER != 'Unknown') & (alt.datum.GENDER != 'null')
)
encoding = base.transform_filter(
    alt.datum.GENDER == 'Female'
).encode(
    y= alt.Y(
        'index:N',
        sort= movies_order
    ),
    x=alt.X('count(index):Q',
            title='cast count'),
)
def m_circle():
    """
    return the call to altair function that uses the circle mark for the variables encoded in the previous example 
    """
    return encoding.mark_circle().properties(title='Female')

bar = m_circle()
    # return encoding. (...)
    # YOUR CODE HERE
    # raise NotImplementedError()
```
**Output**

![](https://github.com/alisongh/MADS/blob/27cbed59d588ecafff7774029634408d7b3879d1/SIADS%20522/Assignment%202/assets/m_circle.png?raw=true)

### 2.3 Increase variables encoded (5 points)
Complete the `female_actors` function. Modify the first bar chart encoding the type of the actor with the color of the bar. Your visualization should look like the following (note that we don't have labels yet):
!["Drawing"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/alt_enc_2_resized.png?raw=true)

- _Partial credit can be granted for each visualization (up to 2 points) if you provide a description of what the missing piece of the function is supposed to do without need for an Altair working version_

```python
def female_actors():
    """
    return the call to Altair function that uses the bar mark for the variables and the color for the TYPE 
    """

    
    # this is the original chart, you should replace this code
    encoding = base.encode(
        y= alt.Y(
            'index:N',
            sort= movies_order,
            axis=None
        ),
        x=alt.X('count(index):Q',
                title='cast count'
                ),
        color='TYPE',
        order=alt.Order(
            'TYPE',
            sort='descending'
        )
        # add color encoding
    )
 
    # YOUR CODE HERE
    # raise NotImplementedError()
    
   
    return encoding.mark_bar().properties(title='Female')
```
```python
female = female_actors()
female
```
**Output**

![](https://github.com/alisongh/MADS/blob/4f47a02da8a83e68d4495a83c565f8dbdc758af1/SIADS%20522/Assignment%202/assets/female_actors.png?raw=true)

### 2.4 Change filter transform (5 points)
Complete the male_actors function, modify the previous visualization so that the actors visualized have Male gender. Use the Altair transform function for this, not Pandas.


!["Drawing"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/alt_enc_3_resized.png?raw=true)

- _Partial credit can be granted for each visualization (up to 2 points) if you provide a description of what the missing piece of the function is supposed to do without need for an altair working version_

```python
def male_actors():
    """
    return an altair-defined chart that uses the bar mark for the variables and the color for the TYPE 
    this function filters the actors to be male
    """
    #add filter transform
    # This is the starting point. Again, modify or replace this code to get the encoding we describe above
    encoding = base.transform_filter(
        alt.datum.GENDER == 'Male'
        ).encode(
            y= alt.Y(
            'index:N',
            sort= movies_order
        ),
        x=alt.X('count(index):Q',
                sort='descending',
                title='cast count'),
        color='TYPE',
        order=alt.Order(
            'TYPE',
            sort='descending'
        )
         # add color encoding
    ).mark_bar().properties(title='Male')
    
    # YOUR CODE HERE
    # raise NotImplementedError()
    
    return encoding.mark_bar().properties(title='Male')
```
```python
male = male_actors()
male
```
**Output**

![](https://github.com/alisongh/MADS/blob/8ee563e90a73a9cab6e54307b9b849ae66c8fbc7/SIADS%20522/Assignment%202/assets/male_actor.png?raw=true)


### 2.5 Variables encoded 2 (5 points)
Complete the `variables_encoded_2` function.
Return the number of variables encoded in the following plot (again, something like `return 5000;`... we're using this for automated testing). If you have been able to complete the previous examples, the plot should look like this

!["Drawing"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/visualization_resized.png?raw=true)

```python
middle = base.encode(
    y=alt.Y('Rank:O', axis=None),
    text=alt.Text('Rank:Q'),
    color=alt.Color('bechdel:N')
).mark_text().properties(width=20)

# merge together the three charts, male, middle, female
male | middle | female
```
**Output**

![](https://github.com/alisongh/MADS/blob/c77103efbb32da10e3f2ef2f3e358013ed18a745/SIADS%20522/Assignment%202/assets/middle.png?raw=true)

### 2.6 Alternative encoding 1 (20 Points)
Create a new visualization within the `alternative_encoding_one` function with the following encoding:
- Use circles as the mark
- Use the scale of the circles to encode the number of actors on each category
- Use the y position of the circle to encode the movie
- Use the x position of the circle to encode the type of actor
- Use the color of the circle to encode the gender of the actor
- Match the styling of the example


!["Drawing"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/visualization_2_resized.png?raw=true)

- _Partial credit can be granted for each visualization (up to 5 points) if you provide a description of what the missing piece of the function is supposed to do_

```python
def alternative_encoding_one():
    """
    return call to altair function for the new visualization
    """
    #change this
    plot = base.mark_circle(
       opacity=0.8,
       stroke='black',
       strokeWidth=1
    ).encode(
        alt.X('TYPE:O'),
        alt.Y('index:N',
              sort= movies_order
              ),
        alt.Size('count(index):Q',
        scale=alt.Scale(range=[0,4500]),
        legend=alt.Legend(title='Count of actors', symbolFillColor='white')),
        alt.Color('GENDER')
        #complete this
    ).properties(
        width=350,
        height=880
    )
    # YOUR CODE HERE
    # raise NotImplementedError()
    return plot
```
```python
al_enc_one = alternative_encoding_one()
middle | al_enc_one
```
**Output**

![](https://github.com/alisongh/MADS/blob/c77103efbb32da10e3f2ef2f3e358013ed18a745/SIADS%20522/Assignment%202/assets/middle.png?raw=true)

### 2.7 Alternative encoding 2 (25 Points)
complete `female_actors_1()` and `male_actors_1()` functions to create a new visualization with the following encoding:
- The left and right plot should filter male and female actors respectively
- Use rectangles as the mark
- Use the text inside each rectangle to encode the count of actors one each category (gender, type and movie)
- Use the y position of the rectangle to encode the movie
- Use the x position of the rectangle to encode the type of actor
- Use the color of the rectangle to encode whether that movie passes the Bechdel test or not (bechdel variable)
- Note that only the female vis has labels on the left, the male vis does not

The top of the female plot would look like this:

!["Drawing"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/visualization_3_resized.png?raw=true)

If you've done everything correctly, your final visualization should look like the one below.

!["Drawing"](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/visualization_3.png?raw=true)

- _Partial credit can be granted for each visualization (up to 4 points for each function) if you provide a description of what the missing piece of the function is supposed to do without need for an Altair working version_

```python
def female_actors_1():
    """
    return call to altair function for the new visualization
    """
    # modify to add filter transform
    female_p = base.transform_filter(
    alt.datum.GENDER == 'Female').mark_rect().encode(
        alt.X('TYPE:O'),
        alt.Y('index:N',
            sort=movies_order),
        color=alt.Color('bechdel:N')
    )

    text = base.transform_filter(
        alt.datum.GENDER == 'Female').mark_text(align='center',baseline='middle').encode(
        y = alt.Y('index:N',
                sort=movies_order),
        x = 'TYPE:O',
        text='count(index)')

    # YOUR CODE HERE
    # raise NotImplementedError()
    return female_p + text
```
```python
f_a_1 = female_actors_1()
f_a_1
```
**Output**

![](https://github.com/alisongh/MADS/blob/c77103efbb32da10e3f2ef2f3e358013ed18a745/SIADS%20522/Assignment%202/assets/visualization_3_fullSize.png?raw=true)

```python
def male_actors_1():
    """
    return call to altair function for the new visualization
    """
    # modify to add filter transform
    male_p = base.transform_filter(
    alt.datum.GENDER == 'Male').mark_rect().encode(
        alt.X('TYPE:O'),
        alt.Y('index:N',
            sort=movies_order,
            axis=None),
        color=alt.Color('bechdel:N')
    )

    text = base.transform_filter(
        alt.datum.GENDER == 'Male').mark_text(align='center',baseline='middle').encode(
        y = alt.Y('index:N',
                sort=movies_order),
        x = 'TYPE:O',
        text='count(index)')

    # YOUR CODE HERE
    # raise NotImplementedError()
    return male_p + text
```
```python
m_a_1 = male_actors_1()
m_a_1
```
**Output**

![](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/vis_3_full.png?raw=true)

```python
# create the visualization
f_a_1 | middle | m_a_1
```
**Output**

![](https://github.com/alisongh/MADS/blob/3397f734fb072aaf9e684e4177014b29771da900/SIADS%20522/Assignment%202/assets/visualization_3_full.png?raw=true)


