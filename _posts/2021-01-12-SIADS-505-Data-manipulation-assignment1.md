---
layout: post
title: SIADS 505 
description: Data Manipulation Assignment 1
date: 2021-01-12
image: '/images/04.jpg'
tags: [data, data-manipulation, python]
---
* 目录
{:toc}

## Syllabus
This is the [Syllabus](https://github.com/alisongh/alisongh.github.io/blob/main/assets/501.pdf) of SIADS 505.

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

```python
assert len(student_grades()) == 16
```

### Part 3: Logs
Consider the variable 'logdata' which is a string containing a standard web log. This variable records the access a user makes when visiting a web page (like this one!). Each line of the log has the following items: 

* a host (e.g., ‘146.204.224.152’) 
* a username (e.g., ‘feest6811’ or sometime '-' since it is missing) 
* the time a request was made (e.g., ‘21/Jun/2019:15:45:24 -0700’) 
* the post request type (e.g., ‘POST /incentivize HTTP/1.1’)

Your task is to fix the incorrect regex between `### BEGIN CODE` and `### END CODE` to convert the given data into a list of dictionaries, where each dictionary looks like the following:

```
example_dict = {"host":"146.204.224.152",
                "user_name":"feest6811",
                "time":"21/Jun/2019:15:45:24 -0700",
                "request":"POST /incentivize HTTP/1.1"}
```

```python
import re
def logs():
    with open("assets/logdata.txt", "r") as file:
        logdata = file.read()
    
        
    ### BEGIN CODE    
    pattern = """
        (?P<host>[\d]*.[\d]*.[\d]*.[\d]*)
        (\ -\ )
        (?P<user_name>[\w-]*)
        (\ \[)
        (?P<time>\w*/\w*/.*)
        (\]\ \")
        (?P<request>.*)
        (")
        """
    result = []
    for item in re.finditer(pattern, logdata, re.VERBOSE):
        result.append(item.groupdict())
    return result
logs()
```

```python
assert len(logs()) == 979
```
