---
title: Easy (?) way to tack Fortran onto Python
author: Abhijit Dasgupta
date: '2009-03-06'
categories:
  - Computation
tags:
  - Fortran
  - Python
slug: easy-way-to-tack-fortran-onto-python
---

A recent post on  [Walking Randomly](http://www.walkingrandomly.com/?p=85) gave a nice example of using the Python **ctypes** module to load Fortran functions that have been compiled into a shared library (*.so) or DLL (*.dll). This seems an easier option than using f2py or pyfort, which have not been working well for me.
