---
layout: post
title: Compare Data Types in Numpy to Python
categories: [PyData]
tags: [Numpy, Python]
modified: 2019-12-14
---

### NumPy Data Types
In NumPy, the primitive types supported are tied closely to data types in C. 
For details, please refer to [basics.types](https://numpy.org/devdocs/user/basics.types.html).

### Python Built-in Types
The principal built-in types in Python are numerics, sequences, mappings, classes, 
instances and exceptions. For details, please refer to Python's 
[stdtypes](https://docs.python.org/3/library/stdtypes.html).

### Data Types Comparison
Here are some basic types checking,

#### bool

```python
In [1]: import numpy as np                                                                                                

In [2]: bool                                                                                                              
Out[2]: bool

In [3]: np.bool                                                                                                           
Out[3]: bool

In [4]: np.bool_                                                                                                          
Out[4]: numpy.bool_

In [5]: bool is np.bool                                                                                                   
Out[5]: True

In [6]: bool is np.bool_                                                                                                  
Out[6]: False
```

#### int

```python
In [1]: import numpy as np                                                                                                

In [2]: int                                                                                                               
Out[2]: int

In [3]: np.int                                                                                                            
Out[3]: int

In [4]: np.int_                                                                                                           
Out[4]: numpy.int64

In [5]: np.int64                                                                                                          
Out[5]: numpy.int64

In [6]: int is np.int                                                                                                     
Out[6]: True

In [7]: int is np.int_                                                                                                    
Out[7]: False

In [8]: np.int is np.int_                                                                                                 
Out[8]: False

In [9]: np.int_ is np.int64                                                                                               
Out[9]: True
```

#### float

```python
In [1]: import numpy as np                                                                                                

In [2]: float                                                                                                             
Out[2]: float

In [3]: np.float                                                                                                          
Out[3]: float

In [4]: np.float_                                                                                                         
Out[4]: numpy.float64

In [5]: np.float64                                                                                                        
Out[5]: numpy.float64

In [6]: np.double                                                                                                         
Out[6]: numpy.float64

In [7]: np.float is float                                                                                                 
Out[7]: True

In [8]: np.float_ is float                                                                                                
Out[8]: False

In [9]: np.float_ == np.float64                                                                                           
Out[9]: True

In [10]: np.float_ == np.double                                                                                           
Out[10]: True

In [11]: np.float64 == np.double                                                                                          
Out[11]: True
```

#### complex

```python
In [1]: import numpy as np                                                                                                

In [2]: complex                                                                                                           
Out[2]: complex

In [3]: np.complex                                                                                                        
Out[3]: complex

In [4]: np.complex64                                                                                                      
Out[4]: numpy.complex64

In [5]: np.complex_                                                                                                       
Out[5]: numpy.complex128

In [6]: complex is np.complex                                                                                             
Out[6]: True

In [7]: complex is np.complex_                                                                                            
Out[7]: False

In [8]: complex is np.complex64                                                                                           
Out[8]: False

In [9]: np.complex_ is np.complex128                                                                                      
Out[9]: True

In [10]: np.dtype(np.complex)                                                                                             
Out[10]: dtype('complex128')

In [11]: np.complex is np.complex128                                                                                      
Out[11]: False

In [12]: np.dtype(complex)                                                                                                
Out[12]: dtype('complex128')
```

From the checking, we can know that NumPy preserves bool, int, float and complex types 
to be same as what in Python. And, it also provides fixed-size aliases.

### Overflow Errors
The fix size of numeric types in NumPy may cause overflow errors. For example, 
the int type in NumPy differ significantly what is Python. 
Python integers may expand to accommodate any integer and will not overflow.

```python
In [1]: import numpy as np                                                                                                

In [2]: i = 1000000000000                                                                                                 

In [3]: np.array(i, dtype=np.int32)                                                                                       
Out[3]: array(-727379968, dtype=int32)

In [4]: np.array(i, dtype=np.int64)                                                                                       
Out[4]: array(1000000000000)
```

### Extended Precision
Please refer to Numpy's [extended-precision](https://numpy.org/devdocs/user/basics.types.html#extended-precision)