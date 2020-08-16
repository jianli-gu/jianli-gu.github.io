---
layout: post
title: The Standard Type Hierarchy in Python Data Models
categories: [Python]
tags: [Python]
modified: 2020-08-15
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

`Ellipsis` is an instance of `ellipsis` class.
```python
>>> ...
Ellipsis
>>> Ellipsis
Ellipsis
>>> type(...)
<class 'ellipsis'>
>>> type(Ellipsis)
<class 'ellipsis'>
```

The truth value of `Ellipsis` is `True`,
```python
>>> bool(...)
True
```

It's singleton, as shown below:
```python
>>> new_ellipsis = type(...)()
>>> new_ellipsis == ...
True
>>> new_ellipsis is ...
True
>>> id(...)
9373728
>>> id(new_ellipsis)
9373728
```

What `Ellepsis` could be used for? It can be used for slicing multidimensional arrays. For example,
```python
>>> import numpy as np
>>> x = np.arange(16).reshape(2, 2, 4)
>>> x
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7]],

       [[ 8,  9, 10, 11],
        [12, 13, 14, 15]]])

>>> x[...]
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7]],

       [[ 8,  9, 10, 11],
        [12, 13, 14, 15]]])

>>> x[..., 1]
array([[ 1,  5],
       [ 9, 13]])

>>> x[1, ...]
array([[ 8,  9, 10, 11],
       [12, 13, 14, 15]])

>>> x[1, ..., 1]
array([ 9, 13])
```
The ellipsis syntax indicates slicing in full any remaining unspecified dimensions.

## 4. Numbers

Numbers in Python consit of integral numbers, floating point numbers, and complex numbers.

```python
>>> from numbers import Number
>>> x = 10000000000000000000000000000000000000000000
>>> isinstance(x, Number)
True
>>> y = '1000'
>>> isinstance(y, Number)
False
```

`numbers.Number` is the root of the numeric hierarchy.


**4.1 `numbers.Integral`**

* `int` - Unlimited range, subject to available memory only.
* `bool` - Boolean objects `True` and `False`, a subtype of the integer type, behave like `1` and `0`.

**4.2 `numbers.Real`**

* `float` - Machine-level double precision floating point numbers.

**4.3 `numbers.Complex`**

* `complex` - A pair of machine-level double precision floating point numbers, containing real `z.real` and imaginary `z.imag` parts.

For more details, please refer to the [numbers](https://python.readthedocs.io/en/stable/library/numbers.html) module, 
and the [PEP 3141](https://www.python.org/dev/peps/pep-3141/).

## 5. Collections

**5.1 Sequences**

Immutable sequences
* Strings
* Tuples
* Bytes

Mutable sequences
* Lists
* Byte Arrays

**5.2 Set**

There are currently two intrinsic set types:
* Sets
* Frozen sets

The relationship between frozenset and set is like what between tuple and list. 
They are using the same hashtable implementation behind, and sets provide fast data insertion,
deletion and membership testing. Frozenset is immutable and hashable, so can be used as ditionary 
keys.

**5.3 Mappings**

* Dictionaries

## 6. Callables

**6.1 User-defined functions**

Let's define a function, like this:

```python
def run(a: int, b: float, c: float = 1.0) -> float:
    """Process numbers"""
    return a + b + c
```

We can get access many special attributes about this function, for example,
* `__doc__`
* `__name__`
* `__qualname__`
* `__module__`
* `__defaults__`
* `__code__`
* `__gloabals__`
* `__dict__`
* `__closure__`
* `__annotations__`
* `__kwdefaults__`

```python
>>> run.__doc__
'Process numbers'
>>> run.__name__
'run'
>>> run.__qualname__
'run'
>>> run.__module__
'__main__'
>>> run.__defaults__
(1.0,)
>>> run.__code__
<code object run at 0x7f5fae0085b0, file "<stdin>", line 1>
>>> run.__globals__
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'run': <function run at 0x7f5fadee2d30>}'
>>> run.__dict__
{}
>>> run.__closure__
>>> run.__annotations__
{'a': <class 'int'>, 'b': <class 'float'>, 'c': <class 'float'>, 'return': <class 'float'>}'
>>> run.__kwdefaults__
```

**6.2 Instance methods**

Here's an `User` class,

```python
class User:
    """Customized user class"""

    def __init__(self, name: str, password: str, active: bool=True):
        """Initialize user class"""
        self.name = name
        self.password = password
    
    def __repr__(self):
        """User string representation"""
        return f"<User: {self.name}>"

    def change_password(self, new_password):
        """User changes password"""
        self.password = new_password
```

We can also access the special attributes attached to instance methods,

```python
In [2]: user = User(name="tester", password="123456")                                          

In [3]: user.change_password.__doc__           
Out[3]: 'User changes password'

In [4]: user.change_password.__module__        
Out[4]: '__main__'

In [5]: user.change_password.__func__          
Out[5]: <function __main__.User.change_password(self, new_password)>

In [6]: user.change_password.__func__.__name__ 
Out[6]: 'change_password'

In [7]: user.change_password.__func__.__module__                                               
Out[7]: '__main__'

In [8]: user.change_password.__func__.__doc__  
Out[8]: 'User changes password'

In [9]: user.change_password.__self__          
Out[9]: <User: tester>

```

**6.3 Generator functions**

**6.4 Coroutine functions**

**6.5 Asynchronous generator functions**

**6.6 Built-in functions**

**6.7 Built-in methods**

**6.8 Classes**

**6.9 Class Instances**

## 7. Modules

## 8. Internal Types

---

**References**

[1] [https://docs.python.org/3.8/reference/datamodel.html](https://docs.python.org/3.8/reference/datamodel.html)

[2] [https://realpython.com/null-in-python/](https://realpython.com/null-in-python/)