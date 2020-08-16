---
layout: post
title: The Magic Methods in Python Data Models
categories: [Python]
tags: [Python]
modified: 2020-07-30
---

Python class can implement certain operations that are invoked by special syntax, that is, 
methods surrounded by double underscores, for example, `__str__`. 
People normally call these special methods magic methods which give us access to 
Python's built-in syntax features. 

In the following sections, we are going to walk through the magic methods in different categories.


### 1. Class and Metaclass

`__new__`

`__init__`

`__del__`

`__class__`

`__init_subclass__`

`__mro_entries__`

`__prepare__`

`__clsss__`

`__classcell__`

`__set_name__`

`__class_getitem__`

### 2. Instance and Subclass

`__instancecheck__`:

`__subclasscheck__`:

### 3. Attribute Access and Descriptors

`__getattr__`

`__setattr__`

`__delattr__`

`__getattribute__`

`__setattribute__`

`__dir__`

`__get__`

`__set__`

`__delete__`

`__set_name__`

`__slots__`

### 4. String/Bytes Representation

`__repr__`

`__str__`

`__bytes__`

`__format__`


### 5. Container Types

`__len__`

`__length_hint__`

`__getitem__`

`__setitem__`

`__delitem__`

`__missing__`

`__contains__`

`__iter__`

`__yield__`

`__reversed__`


### 6. Callable

`__call__`

### 7. Context Management

`__enter__`

`__exit__`

### 8. Rich-comparison Operators

`__lt__`: `<`

`__le__`: `<=`

`__eq__`: `==`

`__ne__`: `!=`

`__gt__`: `>`

`__ge__`: `>=`


### 9. Coroutines

`__await__`

`__aiter__`

`__anext__`

`__aenter__`

`__aexit__`


### 10. Unary Arthmetic Operations

`__pos__`: `+`

`__neg__`: `-`

`__abs__` `abs()`

`__invert__` `~`

### 11. Biult-in Functions

`__complex__`: `complex()`

`__int__`; `int()`

`__long__`: `long()`

`__float__`: `float()`

`__bool__`: `bool()`

`__oct__`: `oct()`

`__hex__`: `hex()`

`__index__`: `operator.index()`

`__round__`: `round()`

`__trunc__`: `truc()`

`__floor__`: `floor()`

`__ceil__`: `ceil()`

`__hash__`: `hash()`

### 12. Binary Arithmetic Operations

`__add__`: `+`

`__sub__`: `-`

`__mul__`: `*` 

`__matmul__`: 

`__truediv__`: `/`

`__floordiv__`: `//`

`__mod__`: `%`

`__divmod__`: `divmod()`

`__pow__` `**` or `pow()`

`__and__`: `&`

`__or__`: `|`

`__xor__`: `^`

`__lshift__`: `<<`

`__rshift__`: `>>`


### 13. Binary Arithmetic Operations with reflected Operands


`__radd__`

`__rsub__`

`__rmul__` 

`__mratmul__`: 

`__rtruediv__` 

`__rfloordiv__` 

`__rmod__` 

`__rdivmod__`

`__rpow__` 

`__rand__`: 

`__ror__`:

`__rxor__`: 

`__rlshift__`: 

`__rrshift__`:


### 14. Augumented Arithmetic Assignments


`__iadd__`: `+=`

`__isub__`: `-=`

`__imul__`: `*=`

`__imatmul__`: `@=`

`__itruediv__`: `/=`

`__ifloordiv__`: `//=`

`__mod__`: `%=`

`__ipow__` `**=` 

`__iand__`: `&=`

`__ior__`: `|=`

`__ixor__`: `^=`

`__ilshift__`: `<<=`

`__irshift__`: `>>=`
