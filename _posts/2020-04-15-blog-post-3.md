---
title: 'Creating NumPy Arrays'
date: 2020-05-08
permalink: /posts/2020/05/creating-numpy-arrays/
tags:
  - python
  - numpy
  - ndarray
  - arrays
---

NumPy arrays are a core part of the numerical computing stack in Python.
NumPy provides a large number of functions for creating these arrays, 
of which `np.array` is most well known (due to ubiquitous use in tutorials).
However, `np.array` is not always the right function to use.
This post will explore some other common array creation functions.

# Creating NumPy Arrays

NumPy is at the base of must numerical computing in Python.
The `ndarray` is the central class provided by NumPy, which implements a C-style array.

## Background

Unlike the List object from base Python, `ndarrays` are statically typed,
 containing only one type.
In most cases, this makes `ndarrays` significantly faster than base Python objects.
The static type of an `ndarray` is termed the 'data-type' of the array and is denoted with the keyword `dtype`. 

In addition to a data-type, an array has a 'shape'.
An array shape is a Tuple representing how the array is structured.
The shape primarily has to do with how the `ndarray` interpreted and
 the types of operations that are allowed.
Array shape will not be discussed in detail here.
For the purposes of this post, I will focus 1-D arrays as the ideas are easily extended to other array shapes.


## Basic Creation Functions

The most common array creation functions from NumPy can be put three categories: 

* Arrays with uninitialized values
* Arrays with initialized values
* Arrays built from iterable objects

### Arrays with Uninitialized Values

These are the most basic type of functions for creating NumPy arrays.

* `np.empty`: creates a `ndarray` with uninitialized values i.e. its empty.
* `np.empty_like`: create an empty `ndarray` based on another array (same shape and `dtype`)

These functions are typically used when an array must be filled with a loop.
In the typical way that `list.append()` might be used.
However, it should be used when the size of the array is known (like in a for-loop) as iteratively resizing a NumPy array is slow.

As an example, `np.empty` could be used to collect the silhouette scores of k-means clustering for given values of k.
In the code below, the empty array is built based on the number of k values that will be scored.
The silhouette score is calculated for each k and stored in the array (indexed on the enumeration of k).

```python
# initialize variables
k_values = [k for k in range(2, 7)]
silhouette_scores = np.empty(len(k_values))

# iterate over k values, enumerate to access array
for i, k in enumerate(k_values):
  # init KMeans with k clusters
  km = KMeans(n_clusters = k, random_state = 42)
  # get cluster labels
  labels = km.fit_predict(X)
  # calc and store score
  silhouette_scores[i] = silhouette_score(X, labels)
```

### Arrays with Initialized Values

#### Single-Valued

The following functions create arrays and initialize the values to a particular value.

* `np.zeros`: create an array and initialize all values to zero.
* `np.zeros_like`: create an array like another (shape and `dtype`) and initialize all values to zero.
* `np.ones`: create an array and initialize all values to one.
* `np.ones_like`: create an array like another (shape and `dtype`) and initialize all values to one.
* `np.full`: create an array and initialize all values to a given value.
* `np.full_like`: create an array like another (shape and `dtype`) and initialize all values to a given value.

These functions would be used to create arrays with a given initial value.
`np.full` is the most general of this set of functions,
 while `np.zeros` and `np.ones` are essentially special cases of `np.full`.
The usage is straightforward.

```python
# create a 2 element vector with values initialized to 10
>>> np.full(2, 10)
array([10., 10.])

# create a 3 element vector with values initialized to 0
>>> np.zeros(3)
array([0., 0., 0.])

# create a ones vector like the 3 element zeros vector
>>> np.ones_like(np.zeros(3))
array([1., 1., 1.])
```

#### Sequences and Spaces

The following functions create array containing sequences or numeric spaces.

* `np.arange`: create an array containing a sequence of numbers.
* `np.linspace`: create an array containing evenly spaced numbers over an interval.
* `np.logspace`: create an array containing evenly spaced numbers over an interval on a log scale.

`np.arange` can be used to create a general sequence of numbers.
Often, `np.arange` will be used to generate sequences of model hyperparameters for tuning like k values for k-Means clustering.
For instance, the variable creation section in the example above could be written with `np.arange`:

```python
# initialize variables
k_values = np.arange(2, 7)
# note: dtype is overwritten because 
silhouette_scores = np.empty_like(k_values, dtype = 'float')
```

The following list comprehension, `np.array([v for v in range(start, stop)])`, is sometimes found in data science code.
These instances are a good oppertunity to use `np.arange` instead of using list comprehension and `np.array`.

An example use case of `np.linspace` and `np.logspace` is building sequences for model hyperparameters searches.
For instance, it is common to search for a good regularization value when using Ridge or LASSO regression.
The code example below shows creation of a logspace that is used for grid searching the regularization hyperparameter in logistic regression.

```python
# create a logspace from 0.00001 to 10000 with 20 evenly spaced values
tuned_parameters = [{'C':np.logspace(-4, 4, 20)}]

# grid search 'C' with logistic regression
clf = GridSearchCV(
        LogisticRegression(random_state = random_state),
        tuned_parameters,
        scoring='%s_macro' % 'recall'
    )
clf.fit(X_train, y_train)
```

## Arrays Built from Iterable Objects

The following functions will create arrays from iterables.

* `np.array`: creates an `np.ndarray` from an existing object. For instance coverting a `List` to an `ndarray`.
* `np.asarray`: converts an object to an `ndarray`, but does notcreate a copy of an existing `ndarray`.

`np.array` is by far the most well known array creation function in NumpP.
An example it provided below.

```python
# create an ndarray from a list
>>> np.array([2, 1, 4])
array([2, 1, 4])
```

`np.asarray` is a really useful function for coercing arguments of a function to `ndarrays`.
In functions that typically accept arrays, it is common also accept any 'list\_like' or 'array\_like' object.
Any function that takes an 'array\_like' object, expecting to use it like an `ndarray`, must convert it to an array internally.
However, if an `ndarray` is passed to the function, there is no need to use `np.array` which will create a copy of the array.
Instead, use `np.asarray` which will check if the input object is an array or exposes an `__array__` 
 (i.e. it contains an array representation like pd.DataFrame, pd.Series)
 and will only create a new `ndarray` when necessary.

```python
def to_array(iterable_object):
  """Convert a list like object to a numpy array
  """
  # do not create a new array if object is an ndarray
  # or contains an ndarray representation
  array = np.asarray(iterable_object)
  return array

```

Note: 'list\_like' or 'array\_like' are colloquialisms used in the PyData stack to describe an object that 'looks' like a list or an array.

## References

* \[1\] NumPy Developers, "NumPy v1.18", https://numpy.org/doc/stable/
* \[2\] Jake VanderPlas, "Understanding Data Types in Python", https://jakevdp.github.io/PythonDataScienceHandbook/02.01-understanding-data-types.html
