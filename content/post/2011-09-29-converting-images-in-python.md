---
title: Converting images in Python
author: Abhijit Dasgupta
date: '2011-09-29'
categories:
  - Computation
tags:
  - PIL
  - Python
slug: converting-images-in-python
---

I had a recent request to convert an entire folder of JPEG images into EPS or similar vector graphics formats. The client was on a Mac, and didn't have ImageMagick. I discovered the [Python Image Library ](http://www.pythonware.com/products/pil/) to be enormously useful in this, and allowed me to implement the conversion in around 10 lines of Python code!!!

````
import Image
from glob import glob

jpgfiles = glob('*.jpg')
for u in jpgfiles:
    out = u.replace('jpg','eps')
    print "Converting %s to %s" % (u, out)
    img=Image.read(u)
    img.thumbnails((800,800)) # Changing the size
    img.save(out)

````

What an elegant solution from Python ---- "batteries included"

To be sure, using ImageMagick is more powerful, and Python wrappers ([PyMagick](http://pymagick.sourceforge.net/)), albeit old, do exist.
