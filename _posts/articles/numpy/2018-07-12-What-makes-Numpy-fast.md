---
layout: post
title: What Makes Numpy Fast
categories: [Numpy]
tags: [Numpy]
modified: 2018-07-12
---

NumPy is an open-source Python package for scientific computing, which provides large, 
multi-dimensional array object, various derived objects, and an assortment of routines for 
fast operations on arrays. It's the fundamental of many other scientific packages in Python, 
like SciPy, OpenCV, scikit-learn, etc.

### Is Numpy Fast?

Short answer: Yes, because of *vectorization* and *broadcasting*.

NumPy is written in Python and C. At the core of NumPy, is the `ndarray` object which 
encapsulates n-dimentional arrays of homogeneous data types, and has operations in pre-compiled 
C code for performance. Behind the scenes in optimization, vectorizattion describes the absence 
of any explicit operations, like looping, indexing, etc. 

Besides, NumPy use broadcasting which provides a means of implicit element-by-element behavior 
of array operations in pre-compiled C code, i.e., all operations in NumPy are not just arithmetic, 
but also logical, bit-wise, functional, etc.


### What is Vectorization?
Vectorization normally refers to the practice of replacing explicit loops with array expressions. In
this way, the operations occur on entire arrays rather than their individual elements.

### What is Broadcasting?
Numpy operates arrays (of equal size) element-wise.
```python
In [1]: import numpy as np                     

In [2]: a = np.array([1, 2, 3, 4, 5])          

In [3]: b = np.array([6, 7, 8, 9, 10])         

In [4]: a * b                                  
Out[4]: array([ 6, 14, 24, 36, 50])
```

However,what about operations on unequally sized arrays? This is where broadcasting comes to play.