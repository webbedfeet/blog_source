---
title: Quick notes on file management in Python
author: Abhijit Dasgupta
date: '2014-04-10'
categories:
  - Python
slug: quick-notes-on-file-management-in-python
---

## This is primarily for my recollection

To expand `~` in a path name:
````
os.path.expanduser('~')
````

To get the size of a directory:
````
import os
def getsize(start<em>path = '.'):
    totalsize = 0
    for dirpath, dirnames, filenames in os.walk(start</em>path):
        for f in filenames:
            fp = os.path.join(dirpath, f)
            totalsize += os.path.getsize(fp)
    return totalsize
````
