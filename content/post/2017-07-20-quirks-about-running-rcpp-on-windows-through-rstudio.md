---
title: Quirks about running Rcpp on Windows through RStudio
author: Abhijit Dasgupta
date: '2017-07-20'
categories:
  - Data Science
tags:
  - R
  - RStudio
  - Windows
slug: quirks-about-running-rcpp-on-windows-through-rstudio
---

# Quirks about running Rcpp on Windows through RStudio

This is a quick note about some tribulations I had running Rcpp (v. 0.12.12) code through RStudio (v. 1.0.143) on a Windows 7 box running R (v. 3.3.2). I also have RTools v. 3.4 installed. I fully admit that this may very well be specific to my box, but I suspect not.

I kept running into problems with Rcpp complaining that (a) RTools wasn't installed, and (b) the C++ compiler couldn't find `Rcpp.h`. First, `devtools::find_rtools` was giving a positive result, so (a) was not true. Second, I noticed that the wrong C++ compiler was being called. Even more frustrating was the fact that everything was working if I worked on a native R console rather than RStudio. So there was nothing inherently wrong with the code or setup, but rather the environment RStudio was creating.

After some searching the interwebs and StackOverflow, the following solution worked for me. I added the following lines to my global .Rprofile file:

````
Sys.setenv(PATH = paste(Sys.getenv("PATH"), "C:/RBuildTools/3.4/bin/",
            "C:/RBuildTools/3.4/mingw_64/bin", sep = ";"))
Sys.setenv(BINPREF = "C:/RBuildTools/3.4/mingw_64/bin/")
````

Note that `C:/RBuildTools` is the default location suggested when I installed RTools.

This solution is indicated [here](https://github.com/rwinlib/r-base/wiki/Testing-Packages-with-Experimental-R-Devel-Build-for-Windows), but I have the reverse issue of the default setup working in R and not in the latest RStudio. However, the solution still works!!

Note that instead of putting it in the global .Rprofile, you could put it in a project-specific .Rprofile, or even in your R code as long as it is run before loading the `Rcpp` or derivative packages. Note also that if you use binary packages that use `Rcpp`, there is no problem. Only when you're compiling C++ code either for your own code or for building a package from source is this an issue. And, as far as I can tell, only on Windows.

Hope this prevents someone else from 3 hours of heartburn trying to make Rcpp work on a Windows box. And, if this has already been fixed in RStudio, please comment and I'll be happy to update this post.
