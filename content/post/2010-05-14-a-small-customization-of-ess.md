---
title: A small customization of ESS
author: Abhijit Dasgupta
date: '2010-05-14'
categories:
  - Computation
tags:
  - emacs
  - ESS
  - R
slug: a-small-customization-of-ess
---

JD Long (at [Cerebral Mastication](http://www.cerebralmastication.com)) posted a question on Twitter about an artifact in ESS, where typing "_" gets you "<-". This is because in the early days of S+, "_" was an allowed assignment operator, and ESS was developed in that era. Later, it was disallowed in favor of "<-" and "=", so ESS was modified to map "_" to "<-". Now I like the typing convenience of this map, and I don't use underscores in my variable names, so I was fine. JD probably was using underscores in his variable names, so this was rather frustrating. There are, I discovered, three ways around this:

  1. Type "_" twice, which puts in the underscore

  2. Use "C-q _", i.e. Ctrl-q then underscore

  3. Put `(setq ess-S-assign "_")` in your .emacs file

The last fix obviously customizes ESS permanently for your emacs setup, while the first two allow you to get to underscore using the default ESS setup.

**Update:** Seth Falcon posted his .emacs on Twitter, which allows C-= to map the assignment operator, and leaves _ alone :)

` (setq ess-S-assign-key (kbd "C-="))
(ess-toggle-S-assign-key t) ; enable above key definition
;; leave my underscore key alone!
(ess-toggle-underscore nil) `

Nice, Seth!!

FYI, ESS is Emacs Speaks Statistics, an emacs addon developed by Tony Rossini and others to enable intelligent editing of statistical scripts in S+, R, SAS and Stata, as well as scripts for the Gibbs Sampling programs BUGS and JAGS, and can be found [here](http://ess.r-project.org)
