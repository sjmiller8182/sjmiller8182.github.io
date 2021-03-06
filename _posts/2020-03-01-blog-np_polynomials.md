---
title: 'Working with Polynomials in Numpy'
date: 2020-03-01
permalink: /posts/2020/03/polynomials_in_numpy/
tags:
  - numpy
  - python
  - polynomials
---

Polynomials are an important mathematical building block used in many science and engineering fields.
In python, NumPy can be used to perform operations on polynomials.

**Contents**

* [Polynomials](#polynomials)
  * [Polynomial Orders](#polynomial_orders)
  * [Finding Roots](#polynomial_roots)
    * [Finding Roots by Factoring](#polynomial_roots_by_factor)
    * [The Quadratic Equation](#quad_equ)
    * [High Order Terms](#higher_orders)
* [Numpy](#numpy)
  * [Polynomial Representation](#poly_rep)
  * [Operations](#poly_ops)
* [Further Reading](#additional)

## Polynomials  <a name="polynomials"></a>

A polynomial is an algebraic expression typically consisting of two or 
  more terms of multiple variables raised to different powers summed together.
A simple example of a polynomial in a single variable is

$$\left( x + 1 \right)$$

### Polynomial Orders <a name="polynomial_orders"></a>

-----------------

A bit of vocabulary first.
Polynomials are described by the highest power term, called the "order" of the polynomial.
The example shown above would be called a first order polynomial.
If we added an $x^2$ term, for example, then it would be a second order polynomial.

$$\left( x^2 + x + 1 \right)$$

There are several typical operations that are performed on polynomials.
Likely, the most common are finding the roots, factoring, and evaluating polynomials at certain values of the variable(s).

### Finding Roots <a name="polynomial_roots"></a>

-----------------

Polynomials are also said to have roots.
When the polynomial is set equal to zero, the value(s) that solve the equation are the roots of the polynomial.
For instance, consider the first polynomial example (1).

Letting the polynomial equal 0,

$$x + 1 = 0$$

The solution to this equation is -1, which is the root of this polynomial.

#### Finding Roots by Factoring <a name="polynomial_roots_by_factor"></a>

-----------------

The root finding process is trival for first order polynomials.
However, finding roots is more difficult for higher order polynomials.
Some second order polynomials can be factored.
Factoring is the process of breaking the polynomial into a set of algebraic terms that are equivalent to the original polynomial when multiplied together.

So an example of factoring, consider the following second order polynomial,

$$x^2 - 4x - 12$$

This polynomial can be factored into the following expression

$$\left( x - 6 \right) \left( x + 2 \right)$$

When the factored terms are multiplied together, the result is the same as the original polynomial.

$$\left( x - 6 \right) \left( x + 2 \right) = x^2 - 4x - 12$$

It is much easier to find the roots of a second order polynomial when it is written in factored form.
Its only necessary to determine the values to make each factor equal zero.
For example, the roots of the polynomial above are 6 and -2 (for the factors, respectively).

#### The Quadratic Equation <a name="quad_equ"></a>

-----------------

Only some second order polynomials can be factored easily by hand over the real numbers.
In cases where the polynomial cannot be factored, the roots can be found with a formula known as the quadratic equation.

$$x = \frac{-b \pm  \sqrt{b^2 - 4ac} }{ 2a }$$

where the coefficents (a, b, c) are defined as follows

$$a x^2 + b x + c = 0$$

So why is this equation necessary?
Consider the following polynomial equation,

$$x^2 + x + 1 = 0$$

This polynomial equation cannot be factored over the real numbers.
The complex valued roots are $-0.5 + 0.866025i$ and $-0.5 - 0.866025i$.

#### High Order Terms <a name="higher_orders"></a>

-----------------

While the roots to first and second order polynomials can be found relatively easily,
there are no easy ways to find the roots of higher order polynomials.
These types of polynomials are typically solved with some kind of computational algebraic framework such as [WolframAlpha](https://www.wolframalpha.com/).
In python, the package, [Numpy](https://numpy.org/), can be used to work with polynomials.

## Numpy <a name="numpy"></a>

Numpy is the primary numerical computing package used in python.
It contains a large number of submodules for all kinds of numerical computations.
We will look at the using the basic polynomial module 
([`numpy.polynomial.polynomial`](https://docs.scipy.org/doc/numpy/reference/routines.polynomials.polynomial.html))
to performing common polynomial operations such as root finding, factoring, and evaluation.

**Note**: this is written based on [numpy v1.17](https://docs.scipy.org/doc/numpy/index.html).
If you encounter issues, check if something has changed in the API.

### Polynomial Representation <a name="poly_rep"></a>

-----------------

In Numpy, polynomials are represented as arrays of the polynomial coefficients using the numpy array object (`np.ndarray`).
The coefficients of the polynomials are listed in increasing order of the variable, the inverse of standard form.
For example, the example polynomial from (1) would be entered in the following way:

```python
>>> import numpy as np
>>> np.array([1, 1])
```

The class Polynomial can be used to display the polynomial in polynomial form.
For example, the previous example would be displayed in the following way:

```python
>>> from numpy.polynomial.polynomial import Polynomial
>>> Polynomial(np.array([1, 1]))
[1]: 𝑥 ↦ 1.0 + 1.0𝑥
```

### Operations <a name="poly_ops"></a>

-----------------

Numpy can perform the common operations discussed above.
Examples for finding roots, factoring, and evaluation are shown here.

**Root finding**

The function [`polyroots`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.polynomial.polynomial.polyroots.html#numpy.polynomial.polynomial.polyroots)
 is used to calculate the roots of a polynomial.
An example of finding the roots of the polynomial in (9) is shown below.
The expected roots are returned in a numpy array.
Note that numpy uses a special data type, np.complex128, to represent complex numbers
 (see [data types](https://docs.scipy.org/doc/numpy/user/basics.types.html)).

```python
>>> from numpy.polynomial.polynomial import polyroots
>>> polyroots(np.array([1, 1, 1]))
[1]: array([-0.5-0.8660254j, -0.5+0.8660254j])
```

**Factoring**

Once the roots of a polynomial have been found, it is possible to get the factored form of the polynomial.
The following polynomial (shown in factored and expended form) has roots at $1$ and $-1$:

$$ \left( x^2 - 1 \right) = \left( x - 1 \right) \left( x + 1 \right) $$

This is how it could be factored with numpy using 
[`polyfromroots`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.polynomial.polynomial.polyfromroots.html#numpy.polynomial.polynomial.polyfromroots):

```python
>>> # import polyfromroots
>>> from numpy.polynomial.polynomial import polyfromroots
>>>
>>> # get the roots of the polynomial (as before)
>>> roots = polyroots(np.array([-1, 0, 1]))
>>> roots
[1]: array([-1.,  1.])
>>>
>>> # get the factored form of the polynomial from the roots
>>> # note that the input to polyfromroots must iterable
>>> for root in roots:
...     print(polyfromroots([root]))
[1. 1.]
[-1.  1.]
```

The two output arrays represent the factors shown above.
These arrays could be collected and expanded to get the original polynomial using 
[`polymul`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.polynomial.polynomial.polymul.html#numpy.polynomial.polynomial.polymul).

**Evalulation**

If evaluation of the polynomial is needed, this can be accomplished with two functions:
 [`polyval`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.polynomial.polynomial.polyval.html#numpy.polynomial.polynomial.polyval)
 and [`polyvalfromroots`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.polynomial.polynomial.polyvalfromroots.html#numpy.polynomial.polynomial.polyvalfromroots).

Using the polynomial from the previous example, the polynomail will be evaluated at $x = 5$ both ways in this example.
For an evaluation at $x = 5$, the expected result is $24$.

```python
>>> from numpy.polynomial.polynomial import polyval, polyvalfromroots
>>> # store the polynomial
>>> poly = np.array([-1, 0, 1])
>>> # eval at x = 5
>>> polyval(5, poly)
[1]: 24.0
>>> 
>>> # get the roots again (as before)
>>> roots = polyroots(np.array([-1, 0, 1]))
>>> polyvalfromroots(5, roots)
[1]: 24.0
```

## Further Reading <a name="additional"></a>

-----------------

There are many other functions in numpy for working with polynomials,
 including additional basics, algebra, calculus, and fitting.
Check these out in the numpy documentation for 
[`polynomial.polynomial`](https://docs.scipy.org/doc/numpy/reference/routines.polynomials.polynomial.html).



