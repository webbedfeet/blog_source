---
title: Genz-Bretz multivariate normal in Python
author: Abhijit Dasgupta
date: '2009-03-06'
tags:
  - Fortran
  - Python
slug: genz-bretz-multivariate-normal-in-python
---

I've been fighting for some time to try and get Genz-Bretz's method for calculating orthant probabilities in multivariate normal distributions imported into Python. I downloaded the fortran code from  [Alan Genz's site](http://www.math.wsu.edu/faculty/genz/homepage) and was unsuccessful in using f2py to link it with Python. However, I discovered the usefulness of the Python_ ctypes_ module in linking with shared libraries (see [here](http://statbandit.wordpress.com/2009/03/06/easy-way-to-tack-fortran-onto-python/)). So, I compiled the fortran code using

    gfortran mvtdstpack.f -shared -o libmvt.so

and then, within Python, did

    from ctypes import *
    libmvn = cdll.LoadLibrary('./libmvt.so')
    pmvn = libmvn.mvtdst_  # the underscore is added while compiling

This allows us access to the function.  The inputs have to be formatted properly, with the use of _c_int, c_double _and _numpy.ctypeslib.ndpointer_, and placed in _pmvn.argtypes_ to prototype the function. The variables can then be initialized and passed through the function or subroutine.

For my purposes this took a bit of a learning curve, but I found a nice example online to make the formatting easier, and the results are really quite fast.  I may actually create a larger Fortran library for this project to speed things up, once I profile my program.
