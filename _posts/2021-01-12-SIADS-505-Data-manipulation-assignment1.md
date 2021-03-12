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

```python
import re
def names():
    simple_string = """Amy is 5 years old, and her sister Mary is 2 years old. 
    Ruth and Peter, their parents, have 3 kids."""

    ### BEGIN CODE  
    #pattern = r'[A-Za-z]?'
    #match = re.finditer(pattern, simple_string)
    ### END CODE  
    
    # YOUR CODE HERE
    pattern = '[A-Z][a-z]*'
    match = re.findall(pattern, simple_string)
    #raise NotImplementedError()
    
    return match
    
names()
```

```python
assert len(names()) == 4, "There are four names in the simple_string"
```

### Part 2: Grades
The dataset file in [assets/grades.txt](assets/grades.txt) contains multiple lines of people along with their grades in a class. Fix the incorrect regex between `### BEGIN CODE` and `### END CODE` to generate a list of just those students who received a B in the course (e.g., \[\'John Doe\', \'Jane Doe\'\].)

```python

import re

def student_grades():
    with open ("assets/grades.txt", "r") as file:
        grades = file.read()

    ### BEGIN CODE
    # pattern = """(\w+)"""
    # matches = re.findall(pattern,grades)
    ### END CODE
        
        
    # YOUR CODE HERE
    pattern = "[\w]*\ [\w]*(?=:\ B)"
    matches = re.findall(pattern, grades)
    #raise NotImplementedError()

    return matches  
    
student_grades()
```
