---
title: 'IPython notebooks: the new glue?'
author: Abhijit Dasgupta
date: '2014-04-09'
categories:
  - Python
  - R
slug: ipython-notebooks-the-new-glue
---

[IPython notebooks](http://www.ipython.org/notebook.html) have become a defacto standard for presenting Python-based analyses and talks, as evidenced by recent [Pycon](https://us.pycon.org) and [PyData](http://www.pydata.org) events. As anyone who has used them knows, they are great for "reproducible research", presentations, and sharing via the [nbviewer](http://nbviewer.ipython.org). There are [extensions](http://ipython.org/ipython-doc/dev/config/extensions/index.html) connecting IPython to R, Octave, [Matlab](https://github.com/ipython/ipython/wiki/Extensions-Index#matlab), [Mathematica](https://github.com/ipython/ipython/wiki/Extensions-Index#mathematica), [SQL](https://pypi.python.org/pypi/ipython-sql/0.3.1), among others.

However, the brilliance of the design of IPython is in the modularity of the underlying engine (3 cheers to [Fernando Perez](http://fperez.org) and his team). About a year ago, a [Julia engine](https://github.com/JuliaLang/IJulia.jl) was written, allowing Julia to be run using the IPython notebook platform (named, appropriately, [IJulia](http://nbviewer.ipython.org/url/jdj.mit.edu/~stevenj/IJulia%20Preview.ipynb)). More recently, an [R engine](https://github.com/takluyver/IRkernel) has been developed to enable R to run natively on the IPython notebook platform. Though the engines cannot be run interchangeably during the same session of the notebook server, it shows that a common user-facing interface now exists for running the three most powerful open-source scientific and data-centric software systems.

Another recent advancement in the path of IPython notebooks as the common medium for reporting data analyses is my friend [Ramnath](http://ramnathv.github.io)'s [proof-of-concept](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0CCsQtwIwAA&url=http%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DAv_M3f4XTTM&ei=bFdEU6idIdTKsATiuIHoCA&usg=AFQjCNFKdaYxOVygyuWmUIOaARtQtH3OZA&sig2=3KLeB9K7acCor9PKFQo0ng&bvm=bv.64367178,d.cWc) [work](https://gist.github.com/ramnathv/9334834) in translating R Markdown documents to IPython notebooks.

I encourage you to explore IPython notebooks, as well as the extensions to R and Julia, specially my colleagues using R and/or Python in the data space.
