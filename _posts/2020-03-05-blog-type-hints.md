---
title: 'Using Type Hints in Python'
date: 2020-03-05
permalink: /posts/2020/03/blog-post-2/
tags:
  - python
  - types
  - type-hints
---

Type hints are annotations in python that indicate the type(s) that are expected as input or return.

# Type Hints

Type hints were introduced in Python 3.5 to provide a framework for static type analysis (see [PEP 484](https://www.python.org/dev/peps/pep-0484/).
Tpye hints provide information about expected types to programmers when code is being written and to static analysis tools such as pylint and mypy.

In this post, I will go through all the common type hint situations I have used.

## A Simple Example

The following example shows the use type hints to indicate `int`s.
Both the inputs and return value are annotated with `int`.

```python
def add(x: int, y: int) -> int:
    """Add two numbers: x, y
    """
    return x + y
```

## Included Base Classes

The annotations for the most basic types such as `None`, `int`, `float`, and `str` are a available by default.
However, the annotations for other included classes must be imported from `typing`.
The following example shows how to used type hints with `list`s.

```python
from typing import List

def average(values: List[float]) -> float:
    """Calculate the average of list of values
    """
    return sum(values) / len(values)
```

Note how the type of element is specified for the `list` passed to `values`.
The other included classes follow a similar form - summarized in the following table.

**Some Included Type Annotations**

| type | Annotation | Form |
|------|------------|------|
| `set`  | `Set`   | `Set[<value_type>]` |
| `list` | `List`  | `List[<value_type>]` |
| `dict` | `Dict`  | `Dict[<key_type>, <value_type>]` |
| `tuple` | `Tuple`  | `Tuple[<value_type>, <value_type>, ...]` |

Annotation is the name imported from `typing` and form shows how the annotation would be used.

If a function uses a combination of these classes, the annotations are chained together as expected.
For instance, a `list` of `tuple`s where the `tuple` contains two `int`s could be annotated as shown.

```python
List[Tuple[int,int]]
```

## Custom Types

For custom classes, the name of the class can be used directly as an annotation.
Here is an example using `ndarray` from NumPy.

```python
from numpy import ndarray

def reverse_array_1d(arr: ndarray) -> ndarray:
    """Reverse an ndarray
    """
    return arr[::-1]
```

Simple as that. Any custom class can be used as a type hint.

## Multiple Types

Since Python is dynamically typed, multiple allowed types is expected.
Inputs and outputs that allow multiple types can be annotated with `Union` from `typing`.
The following example shows a common case where 'list-like' arguments are accepted.

```python
from numpy import ndarray
from pandas import Series

def average(values: Union[List[float], ndarray, Series]) -> float:
    """Get the average of a list like object
    """
    if isinstance(values, list):
        return sum(values) / len(values)
    else:
        return values.mean()
```

## Optional Arguments

Annotations can be applied to optional arguments as well.
Using the previous example, but adding an optional argument to round the result is shown.

```python
from numpy import ndarray
from pandas import Series

def average(values: Union[List[float], ndarray, Series], round_dec: bool = False) -> float:
    """Get the average of a list like object.
    Result can be returned raw or rounded to two decimal places.
    """
    if isinstance(values, list):
        mean = sum(values) / len(values)
    else:
        mean = values.mean()

    if round_dec:
        return round(mean, 2)
    else:
        return mean
```

## References

* \[1\] PEP 484, "Type Hints", https://www.python.org/dev/peps/pep-0484/
* \[2\] PEP 8, "Style Guide for Python Code", https://www.python.org/dev/peps/pep-0008/
