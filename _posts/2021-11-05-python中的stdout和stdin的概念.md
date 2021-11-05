---
layout: post
title: A quick introduction of Python sys.stdin and sys.stdout
description: sys.stdin和sys.stdout
date: 2021-11-05
image: '/images/18.jpg'
tags: [data-science, cybersecurity, python, CWCT]
---

## About `sys.stdin` and `sys.stdout`
`sys.stdin` and `sys.stdout` are file objects which are used by the interpreter for standard input and standard output. 
Data received by `raw_input` comes from `stdin`, which is normally connects to the keyboard. `stdin` can be redirected 
so that input instead comes from a file. This can be very useful for automating scripts.

## About `sys.stdout.write()` and `print()`
When you call the `print(obj)` function, it actually call the `sys.stdout.write(obj + '\n')` like below:

```py
import sys
sys.stdout.write("hello" + "\n")
print(hello)
```

## About `sys.stdin.readline()` and `input()`

The `input()` function first takes the input from the user and then evaluates the expression. Python automatically identifies 
whether the user entered a string or a integer or a list.

  **Note**
  - Whatever you enter as input, input function converts it into a string. 

The `sys.stdin.readline()` reads the escape character entered by the user, like below

```python
import sys

name = sys.stdin.readline()
print(name)

num = sys.stdin.readline(2)
print(num)
```

**Output**
```
# Input Alison
'Alison\n'
# Input 1234
'12'
```

**Differences between `input()` and `sys.stdin.readline()` functions:**
| input() | sys.stdin.readline()|
| ---- | ----|
|The input takes input from the user but doesn't read escape character| The readline() also takes input from the user but also reads the escape character|
|It has a prompt that represents the default value before the user input| Readline has a parameter named size, which is a non-negative number, it actually defines the bytes to be read|


References:

* [Python Docs](https://docs.python.org/3/library/sys.html#sys.stdin)
* [GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-input-and-sys-stdin-readline/)
