---
title: SIADS 505 Data Manipulation Assignment 1
date: 2021-01-12
categories: 
- Python
---
* 目录
{:toc}

## Syllabus
This is the [Syllabus](https://github.com/alisongh/alisongh.github.io/blob/main/assets/501.pdf) of SIADS 505.
## Assignment 1

Before you start working on the problems, here is a small example to help you understand how to write your answers. The solution should be written within the function body given, and the final result should be returned. Then the autograder will try to call the function and validate your returned result accordingly. 

```python
def example_word_count():
    # This example question requires counting words in the example_string below.
    example_string = "Amy is 5 years old"

    # You should fix the given code only between ### BEGIN CODE and ### END CODE. 
    # Your code should return a result. 
    # You can use "Show Hint" button to see helpful information for solving problems.    
    # You can comment out or delete the NotImplementedError below.
    
    ### BEGIN CODE 
    result = example_string.split(" ")
    ### END CODE 
    #raise NotImplementedError()
    return len(result)
 ```
 
### Part 1: Names
Fix the incorrect regex between `### BEGIN CODE` and `### END CODE` to generate a list of names in the simple_string.
