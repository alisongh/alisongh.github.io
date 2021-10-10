---
title: SIADS 502 Math Methods for Data Science Assignment 1
date: 2021-02-15
categories:
- Math
comment: true
---
* 目录
{:toc}

# Syllabus
This is the [syllabus](assets/502.pdf) for the SIADS 502.

# Assignment 1
Week1: Vectors and Matrices Computation/Basic image processing 

**Version 1.1**

This assignment is designed to let you apply basic concepts and perform basic algebra operations using vectors and matrices in Python. We will use Jupyter Notebooks to organize and submit your assignment. Please read the directions carefully, as we want to avoid submissions that are marked incorrect due to formatting mistakes. You will use vector and matrix computation using numpy and image processing with openCV. 

- Submit the following code at the beginning of your assignment:
- import pandas as pd
- import numpy as np
- from scipy.ndimage import zoom
- import unittest

### Part 1: Vector  Computation
<strong>1.1</strong> \[1 pt\] Please write a function called <strong>computation</strong> that takes three variables called <strong>a(integer), v1(vector represented as a list)</strong> and <strong>v2(vector represented as a list)</strong> and returns a dictionary that has six keys: <strong>addition</strong> (the sum of v1 and v2, this should be a numpy array), <strong>subtraction</strong> (the difference from subtracting v2 from v1, this should be a numpy array), <strong>scale</strong> (scalling v1 by a, this should be a numpy array), <strong>dot</strong> (dot product of v1 and v2, this should be a float), <strong>length</strong> (norm of v1, this should be a float), and <strong>cross</strong> (cross product of v1 and v2, this should be a numpy array).

```python
import pandas as pd
import numpy as np
from scipy.ndimage import zoom
import unittest
```
```python
def computation(a,v1,v2):
    vector_1 = np.array(v1)
    vector_2 = np.array(v2)
    output = {"addition": vector_1 + vector_2,
    "subtraction": vector_1 - vector_2,
    "scale": vector_1 * a,
    "dot": (vector_1 * vector_2).sum(),
    "length": np.sqrt((vector_1 * vector_1).sum()),
    "cross": np.cross(vector_1, vector_2)}
    return output
    # YOUR CODE HERE
    # raise NotImplementedError()
```
Below is the example of a=1, v1=[1,6] and v2=[8,-3] by the computation method. Please run the code, play around the numbers, and see the result.
```python
v1=[1,6]
v2=[8,-3]
result = computation(1,v1,v2)
print(result)
```
**Output**
```
{'addition': array([9, 3]), 'subtraction': array([-7,  9]), 'scal': array([1, 6]), 'dot': -10, 'length': 6.082762530298219, 'cross': array(-51)}
```

<strong>1.2</strong> \[1 pt\] What is the dimension of v3? Please store the v3 dimension into variable <strong>v3_dimen</strong> using numpy.

For grading purposes, `v2_dimen` should be type `tuple`.

```python
v3 = np.array([5, 4])

v3_dimen=v3.shape
v3_dimen
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
(2,)
```

<strong>1.3</strong> \[1 pt\] What is the v4 dimension? Please store the v4 dimension into variable <strong>v4_dimen</strong> using numpy.

For grading purposes, `v4_dimen` should be type `tuple`.

```python
v4 = np.array([1,  2,  3, 4, 5])

v4_dimen=v4.shape
v4_dimen
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
(5,)
```

<strong>1.4</strong> \[1 pt\] Add the following two vectors by hand.  $v_1 = (5 * a, 4)$ and $v_2 (3*b, 7)$.  Store the results as strings in a python list called added (e.g. the vector $(5 * a, 4)$ would be ["5 * a", "4"].  Make sure that each entry in the list is a valid pyton expression.  So "5 * a" is correct, but "5 a" is not because "5 a" will not be properly evaluated by python.

```python
added=["(5 * a) + (3 * b)", "11"]
added

# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
['(5 * a) + (3 * b)', '11']
```

### Part 2: Matrix Computation 

<strong>2.1</strong> \[1 pt\] Write a function called <strong>Matrices</strong> that takes A matrix and B matrix, and then returns a dictionary that has four keys called <strong>addition</strong> (addition of A and B), <strong>subtraction</strong> (subtract B from A), <strong>multiplication</strong> (multiply A and B), <strong>elementwise</strong> (elementwise product, (a\*b)).  Inputs will be represented as numpy arrays and outputs should also be numpy arrays.     

```python
def Matrices(A,B):
    return {"addition": np.add(A,B),
           "subtraction": np.subtract(A,B),
           "multiplication": np.dot(A,B),
           "elementwise": np.multiply(A,B)}

Matrices(([-2,6,8],[5,-2,8],[2,1,4]), ([10,-4,6],[1,8,-3],[8,-2,4]))
    # YOUR CODE HERE
    # raise NotImplementedError()
```
**Output**

```
{'addition': array([[ 8,  2, 14],
        [ 6,  6,  5],
        [10, -1,  8]]), 'subtraction': array([[-12,  10,   2],
        [  4, -10,  11],
        [ -6,   3,   0]]), 'multiplication': array([[ 50,  40,   2],
        [112, -52,  68],
        [ 53,  -8,  25]]), 'elementwise': array([[-20, -24,  48],
        [  5, -16, -24],
        [ 16,  -2,  16]])}
```

<strong>2.2</strong> \[1 pt\] Assume A=[[-2,6,8],[5,-2,8],[2,1,4]] and B=[[10,-4,6],[1,8,-3],[8,-2,4]]. What are the addition, subtraction, multiplication, and elementwise product of A matrix and B matrix? Please store your answers into a variable <strong>ANS22</strong> formatted as a dictionary as in problem 2.1.  (Use the function from 2.1) 

```python
# YOUR CODE HERE
ANS22 = Matrices(([-2,6,8],[5,-2,8],[2,1,4]), ([10,-4,6],[1,8,-3],[8,-2,4]))
ANS22
# raise NotImplementedError()
```
**Output**

```
{'addition': array([[ 8,  2, 14],
        [ 6,  6,  5],
        [10, -1,  8]]), 'subtraction': array([[-12,  10,   2],
        [  4, -10,  11],
        [ -6,   3,   0]]), 'multiplication': array([[ 50,  40,   2],
        [112, -52,  68],
        [ 53,  -8,  25]]), 'elementwise': array([[-20, -24,  48],
        [  5, -16, -24],
        [ 16,  -2,  16]])}
 ```
 
 <strong>2.3</strong> \[1 pt\] What is the transpose of C? Please store the transpose of C into a variable <strong>C_trans</strong>. 

For grading purposes, `C_trans` should be a numpy array.

```python
C = np.array([[1, 1, 2], [3, 5, 8]])

C_trans=C.transpose()
C_trans
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
array([[1, 3],
       [1, 5],
       [2, 8]])
```

<strong>2.4</strong> \[1 pt\] What is (CD)<sup>T</sup>? Please store the value into <strong>CD_trans.</strong>

For grading purposes, `CD_trans` should be type `numpy.array`.

```python
D = np.array([[13], [21], [34]])
CD = np.dot(C,D)
CD_trans=CD.transpose()
CD_trans
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**

```
array([[102, 416]])
```
<strong>2.5</strong> \[1 pt\] What is the inverse matrix of E? Please store it into a variable <strong>E_inv</strong>.              

For grading purposes, `E_inv` should be type `numpy.matrix`

```python
from numpy.linalg import inv
E = np.array([[1, 1, 2], [3, 5, 8], [13, 21, 50]])

E_inv=inv(np.matrix(E))
E_inv
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
matrix([[ 2.5625, -0.25  , -0.0625],
        [-1.4375,  0.75  , -0.0625],
        [-0.0625, -0.25  ,  0.0625]])
```

<strong>2.6</strong> \[1 pt\] If AD=I, what is matrix D? Please store matrix D into a variable <strong>D</strong>.  (Use A from problem 2.2).

For grading purposes, `D` should be type `numpy.ndarray`.

```python
# A=[[-2,6,8],[5,-2,8],[2,1,4]]
A = ([-2,6,8],[5,-2,8],[2,1,4])
D = inv(A)
D
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
array([[-0.2   , -0.2   ,  0.8   ],
       [-0.05  , -0.3   ,  0.7   ],
       [ 0.1125,  0.175 , -0.325 ]])
```

<strong>2.7</strong> \[1 pt\] What is the determinant of A? Please store the value into a variable <strong>A_deter</strong>.  (Use A from problem 2.2.) 

For grading purposes, `A_deter` should be type `numpy.float64`.

```python
A = np.array([[-2,6,8],[5,-2,8],[2,1,4]])

A_deter=np.linalg.det(A)
A_deter
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
79.99999999999997
```
<strong>2.8</strong> \[1 pt\] What is the determinant of BA? Please store the value into a variable <strong>BA_deter</strong>.  (Use A and B from problem 2.2.)

For grading purposes, `BA_deter` should be type `numpy.float64`.

```python
# A=[[-2,6,8],[5,-2,8],[2,1,4]] and B=[[10,-4,6],[1,8,-3],[8,-2,4]]
A = np.array([[-2,6,8],[5,-2,8],[2,1,4]])
B = np.array([[10,-4,6],[1,8,-3],[8,-2,4]])
BA_deter=np.linalg.det(np.dot(B,A))
BA_deter
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
-1920.0000000000002
```

<strong>2.9</strong> \[1 pt\] Change negative values of A to 1 and store the result into a variable <strong>A_pos</strong>.   (Use A from problem 2.2.)  Hint:boolean matrix 

For grading purposes, `A` should be type `numpy.matrix`.

```python
# a[a < 0] = 0
A = np.matrix([[-2,6,8],[5,-2,8],[2,1,4]])
A[A < 0] = 1
A_pos = A
A_pos
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
matrix([[1, 6, 8],
        [5, 1, 8],
        [2, 1, 4]])
```
<strong>2.10</strong> \[1 pt\] We are going to find the trace of the sum of two scaled matrices. Find the trace of 2\*A + 3\*B and store the value into a variable <strong>trace2A3B</strong>.  (Use A and B from problem 2.2.)

For grading purposes, `trace2A3B` should be type `numpy.int64`.
```python
A = np.array([[-2,6,8],[5,-2,8],[2,1,4]])
B = np.array([[10,-4,6],[1,8,-3],[8,-2,4]])
C = (2*A) + (3*B)
trace2A3B=np.trace(C)
trace2A3B
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
66
```
<strong>2.11</strong> \[1 pt\] What is the value of tr(AB<sup>T </sup>)? Please store the value into a variable <strong>AB_trace</strong>.  (Use A and B from problem 2.2).

For grading purposes, `AB_trace` should be type `numpy.int64`.

```python
# AB_trace = np.trace(np.dot(A, np.transpose(B)))
# AB_trace
ABtrans = np.dot(A, np.transpose(B))
AB_trace = np.trace(ABtrans)
# YOUR CODE HERE
# raise NotImplementedError()
```

### Part 3: Matrix Calculations 

<strong>3.1</strong> \[1 pt\] Let $F = [[1, 2, a], [3, 2, 1 + b], [2, 2, 2 + c]]$ where $a$, $b$, and $c$ are variables.  $G = [[0, 2 + c, -a], [3, b + c, -1], [a, 3, -b]]$ where $a$, $b$, and $c$ are the same variables as in $F$.   What is the value of $F + G$? Please store the value into a string <strong>FG_sum</strong> written with valid python code formatting (e.g. FG_sum = "[[1, 2, a], [3, 2, 1 + b], [2, 2, 2 + c]]").  (Note you are encouraged to do this by hand.)

```python
FG_sum = "[[1, 4 + c, 0],[6, 2 + b + c, b], [2 + a, 5, 2 + c - b]]"
# YOUR CODE HERE
# raise NotImplementedError()
```

<strong>3.2</strong> \[1 pt\] Assume F and G as above.  What is the (matrix) product of F and G? Please store your answers into a string variable <strong>Prod_FG</strong>  written with valid python code formatting (e.g. FG_sum = "[[1, 2, a], [3, 2, 1 + b], [2, 2, 2 + c]]"). (Note you are encouraged to do this by hand.)

```python
Prod_FG = "[[6 + a ** 2, (2 + c) + (2 * (b + c)) + (a * 3), (1 * -a) + (2 * -1) + (a * -b)], [6 + (a * (1 + b)), (3 * (2 + c)) + (2 * (b + c)) + (3 * (1 + b)), (3 * -a) + (2 * -1) + (-b * (1 + b))], [6 + (a * (2 + c)), (2 * (2 + c)) + (2 * (b + c)) + (3 * (2 + c)), (2 * -a) + (2 * -1) + (-b * (2 + c))]]"
```
<strong>3.3</strong> \[1 pt\] What is the transpose of F?  Please store your answers into a string variable <strong>F_trans</strong>  written with valid python code formatting (e.g. F_trans = "[[1, 2, a], [3, 2, 1 + b], [2, 2, 2 + c]]"). (Note you are encouraged to do this by hand.)

```python
# 𝐹=[[1,2,𝑎],[3,2,1+𝑏],[2,2,2+𝑐]] 
F_trans = "[[1, 3, 2], [2, 2, 2], [a, 1 + b, 2 + c]]"
# YOUR CODE HERE
# raise NotImplementedError()
```

 <strong>3.4</strong> \[1 pt\] What is the determinant of F? Please store your answers into a string variable <strong>Det_F</strong>  written with valid python code formatting (e.g. F_trans = "2 + a -b"). (Note you are encouraged to do this by hand.)
 
 ```python
 Det_F = "(1 * ((2 * (2 + c)) - (2 * (1 + b)))) - (2 * ((3 * (2 + c)) - (2 * (1 + b)))) + (a * 2)"
# YOUR CODE HERE
# raise NotImplementedError()
```
<strong>3.5</strong> \[1 pt\] What is the trace of the matrix 3F + 2G?    Please store your answers into a string variable <strong>Trace_FG</strong>  written with valid python code formatting (e.g. Trace_FG = "2 + a -b"). (Note you are encouraged to do this by hand.)

```python
Trace_FG="3 + (6 + (2 * (b + c))) + ((3 * (2 + c)) + (2 * -b))"
Trace_FG
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
'3 + (6 + (2 * (b + c))) + ((3 * (2 + c)) + (2 * -b))'
```
<strong>3.6</strong> \[1 pt\] What is the trace of the matrix $F^T \cdot  G^T$?    Please store your answers into a string variable <strong>Trace_FTGT</strong>  written with valid python code formatting (e.g. Trace_FTGT = "2 + a -b"). (Note you are encouraged to do this by hand.)

Hint: no/very little additional calculation is necessary! 

```python
Trace_FTGT= "(6 + a ** 2) + ((3 * (2 + c)) + (2 * (b + c)) + (3 * (1 + b))) + ((2 * -a) + (2 * -1) + (-b * (2 + c)))"
```

### Part 4: Image 

OpenCV is an open source computer vision library(https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_setup/py_intro/py_intro.html#intro). It is used in deep learning or image processing. Here, we will import cv2 and then use it to read the Michigan_logo and plot the image. If you are unfamilair with the library, you may need to read some of the documentation.

```python
pip install opencv-python
```
```python
import cv2
from matplotlib import pyplot as plt
import matplotlib.image as mpimg
```
<strong>4.1</strong> Please use pyplot to read the image and plot the image below. 
```python
filesource = mpimg.imread('assets/Michigan_logo.jpg')
# img = mpimg.imread('../../doc/_static/stinkbug.png')
# imgplot = plt.imshow(img)
# img = mpimg.imread('your_image.png')
# imgplot = plt.imshow(img)
# plt.show()
img = plt.imshow(filesource)
plt.show()
# YOUR CODE HERE
#raise NotImplementedError()
```
<strong>4.2</strong> \[1 pt\] What are the RGB values of the pixel at 10, 15 within the image? Please store your answer in a string format: <strong>ANS42</strong> = "R:, G:, B:". (example, string may look like the following “R:13, G:2, B:100”.) (Hint: use the python format function)
```python
img1 = mpimg.imread('assets/Michigan_logo.jpg')
RGB = img1[10,15]
ANS42= "R:{}, G:{}, B:{}".format(RGB[0], RGB[1], RGB[2])
ANS42
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
'R:1, G:39, B:76'
```

<strong>4.3</strong> Please resize the image to 20x20 using cv2.
```python
img2 = cv2.resize(img1, (20,20))
plt.show(plt.imshow(img2))
# YOUR CODE HERE
# raise NotImplementedError()
```

<strong>4.4</strong> \[1 pt\] What is the dimension of the image? Please store the result into the variable <strong>resized_dim</strong>. 
```python
resized_dim = img2.shape
resized_dim
# YOUR CODE HERE
# raise NotImplementedError()
```
**Output**
```
(20, 20, 3)
```





