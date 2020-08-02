---
layout: post
title: The Standard Type Hierarchy
categories: [Python]
tags: [Python]
modified: 2020-07-29
---

In Python, everything is an object. In this article, we are going to explore the standard types of 
Python, and its hierarchy.

* TOC
{:toc}


## 1. None

**1.1 Keywords**

`None` is a keywords in Python, and can not be used as ordinary identifier. 

```python
>>> None = 100
  File "<stdin>", line 1
SyntaxError: cannot assign to None
```
Follow link [here](https://docs.python.org/3/reference/lexical_analysis.html#keywords) for all keywords.

**1.2 Constant**

`None` is built into Python, can check the `__builtins__` module using `dir` method.

```python
>>> dir(__builtins__)
['ArithmeticError', ..., 'None', ..., 'zip']
>>> getattr(__builtins__, 'None')
>>> print(getattr(__builtins__, 'None'))
None
```
Follow link [here](https://docs.python.org/3/library/constants.html) for a small number of constants.

**1.3 `NoneType`**

`None` is an object and a first-class citizen, it's type is `NoneType`. 

```python
>>> type(None)
<class NoneType>
```

`None` is singleton, 
as `NoneType` only gives the same single instance of `None` in Python.

```python
>>> another_none = type(None)()
>>> print(another_none)
None

>>> another_none is None
True
>>> another_none == None
True
>>> id(another_none)
9447968
>>> id(None)
9447968
```

`NoneType` could not be inherited,

```python
>>> class AnotherNone(type(None)):
...     pass
... 
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: type 'NoneType' is not an acceptable base type
```

**1.4 False**

`None` is falsy,

```python
>>> bool(None)
False
```

## 2. NotImplemented

It's a type in Python.
```python
>>> NotImplemented
NotImplemented
>>> type(NotImplemented)
<class 'NotImplementedType'>
```

It's a constant living in the built-in namespace, see [here](https://docs.python.org/3/library/constants.html#NotImplemented).
Unlike `None`, it can be reassinged, for example,

```python
>>> NotImplemented = "something else"
>>> NotImplemented
'something else'
```
However, we should never reassign it.

Also, the boolean value of `NotImplemented` is `True`.

```python
>>> bool(NotImplemented)
True
```


What does `NotImplemented` mean? It's a special value which should be returned by binary special
methods, like `__eq__`, `__lt__`, `__add__`, etc. to indicate that the operation is not implemented
with respect to other type. Let's look at this example:
```python
class SmallFloat:
    def __init__(self, value):
        self.value = value
    
    def __lt__(self, other):
        if isinstance(other, (SmallFloat, SmallInt)):
            print("Call SmallFloat.__lt__...")
            return self.value < other.value
        return NotImplemented
    
    def __gt__(self, other):
        print("Not implement SmallFloat.__gt__")
        return NotImplemented

>>> f1 = SmallFloat(10.0)
>>> f2 = SmallFloat(20.0)
>>> f1 > f2
Not implement SmallFloat.__gt__
Call SmallFloat.__lt__...
False
```

You might be asking: "Why should not raise a `NotImplementedError` when `__gt__` is not implemented?".
Okay, let do that,
```python
class SmallFloat:
    def __init__(self, value):
        self.value = value
    
    def __lt__(self, other):
        if isinstance(other, (SmallFloat, SmallInt)):
            print("Call SmallFloat.__lt__...")
            return self.value < other.value
        return NotImplemented
    
    def __gt__(self, other):
        print("Not implement SmallFloat.__gt__")
        raise NotImplementedError

>>> f1 = SmallFloat(10.0)
>>> f2 = SmallFloat(20.0)
>>> f1 > f2
Not implement SmallFloat.__gt__
---------------------------------------------------------------------------
NotImplementedError                       Traceback (most recent call last)
 in 
      1 f1 = SmallFloat(10.0)
      2 f2 = SmallFloat(20.0)
----> 3 f1 > f2

 in __gt__(self, other)
     11     def __gt__(self, other):
     12         print("Not implement SmallFloat.__gt__")
---> 13         raise NotImplementedError

NotImplementedError: 
```
See, `NotImplementedError` breaks out of the code, where `NotImplemented` does not get raised and 
the runtime would cleverly use `__lt__` to finish the comparison.

## 3. Ellipsis

## 4. Numbers

## 5. Sequences

## 6. Set Types

## 7. Mappings

## 8. Callable Types

## 9. Modules

## 10. Internal Types

---

**References**

[1] [https://docs.python.org/3.8/reference/datamodel.html](https://docs.python.org/3.8/reference/datamodel.html)

[2] [https://realpython.com/null-in-python/](https://realpython.com/null-in-python/)