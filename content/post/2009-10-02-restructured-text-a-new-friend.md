---
title: Restructured text, a new friend
author: Abhijit Dasgupta
date: '2009-10-01'
slug: restructured-text-a-new-friend
---

A problem I alluded to in an earlier post is my inability to transform LaTeX documents generated using Sweave into MS Word documents which are nicely formatted (or even readable!). I took a step back and looked at a rather easy text markup system called Restructured Text (rst). This markup is used extensively for python documentation using the python _docutils_ module and allows transformation of the text file into both LaTex and OpenOffice.org formats (via a user-supplied script). The LaTeX file can then be transformed to PDF using _pdflatex_. The OpenOffice.org file can easily be translated to MS Word using either the internal converters or (as I have done) adapting some python scripts from [Danny's python modules](http://www.oooforum.org/forum/viewtopic.phtml?t=14409). I now maintain a Makefile which can automate both sets of conversions, generating high-definition png graphics from pdf graphics for the OpenOffice.org conversion.

I use Frank Harrell's _Hmisc_ library extensive for summary tables. These tables are printed on screen in a format perfectly compatible with rst. Other tables are quite easy to format as well.

The resultant Word and PDF files are quite nicely formatted, much better than letting LaTeX take care of things itself. So with a bit more automating scripts, this should generate beautiful reports pretty soon.
