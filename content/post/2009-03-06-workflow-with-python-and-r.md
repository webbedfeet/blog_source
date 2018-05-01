---
title: Workflow with Python and R
author: Abhijit Dasgupta
date: '2009-03-06'
categories:
  - Computation
tags:
  - Python
  - R
slug: workflow-with-python-and-r
---

I seem to be doing more and more with Python for work over and above using it as a generic scripting language. R has been my workhorse for analysis for a long time (15+ years in various incarnations of S+ and R), but it still has some deficiencies. I'm finding Python easier and _faster_ to work with for large data sets. I'm also a bit happier with Python's graphical capabilities via _matplotlib_, which allows dynamic updating of graphs _a la _Matlab, another drawback that R has despite great graphical capabilities.

Where am I finding Python useful? Mainly in reading, cleaning and transforming data sets, and a bit of analysis using _scipy_. Python seems more efficient in reading and working through large data sets than R ever was.  Data cleaning using the string utilities and the _re _ module and exploration also seems pretty easy. I'll probably have to right a few utilities, or just pass that stuff into R. I'm more comfortable doing the analysis within R, so I'm using _rpy2_ quite a bit. Gautier has done a nice upgrade of the old _rpy_ which I used quite a bit.

One thing that Python doesn't have well yet is a literate programming interface. _Sweave_ is one of the strengths of R (and _StatWeave_ looks interesting as an interface with other software like SAS, Stata, etc) which I use almost on a daily basis for report writing. _pyreport 0.3_ seems promising, and does allow for the report to be written in LaTeX, but I need to play with it some more before I can make a call on it. pyreport does allow the simplicity of _reStructured Text_ for documentation, which I wish Sweave was capable of. I figure this can be easily remedied in R by modifying the RweaveHTML driver written by my old friend Greg Snow. [**Addendum, 3/22/09**: I recently found a python package for LaTeX (python.tex), which allows one to embed python code in a LaTeX document, then run latex using the --shell-escape flag. This then runs the python code and embeds the results into the LaTeX document. Sort of the opposite of Sweave, but I figure it will be quite useful as well. It should even work within Sweave documents, since the Sweave parser will take out the R/S parts, then running latex will take care of the python parts.]

Speaking of report writing, this in another place I use Python a lot in my workflow to convert file formats. I use the Python API for OpenOffice.org to transform formats, both for Writer documents and for spreadsheets. I've written small Python scripts in my ~/bin so that I can, on the fly, convert HTML to odt or doc. This is proving quite useful and seems to preserve formats reasonably well. So my reporting workflow is to use Sweave to create a LaTeX document, which I then convert to PDF and HTML, and then transform the HTML to doc using Python. I also create all my graphics as PDF, EPS and SVG formats for subsequent editing by clients. These formats produce the least loss on transformation (the vector formats EPS and SVG have no loss), which is great for large, multicolored heatmaps I produce. I will also create PNG graphics if I have to provide a Word document for the client.
