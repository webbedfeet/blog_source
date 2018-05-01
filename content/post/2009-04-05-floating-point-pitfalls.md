---
title: Floating point pitfalls
author: Abhijit Dasgupta
date: '2009-04-05'
categories:
  - Computation
slug: floating-point-pitfalls
---

John D. Cook over at the [Endeavour](http://www.johndcook.com/blog) has a series of articles talking about floating-point arithmetic and how it can burn us in computing statistics like the standard deviation, correlation and regression coefficients using the book formulae. Specially enlightening for me was the trick of using the Taylor series expansion of log(1+x) for small values of x, since the error is actually quite small. Fantastic points, John!!

A good summary of his points can be found [here](http://www.codeproject.com/KB/recipes/avoiding_overflow.aspx)
