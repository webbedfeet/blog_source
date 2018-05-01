---
title: Creating new data with max values for each subject
author: Abhijit Dasgupta
date: '2014-12-01'
tags:
  - Python
  - R
slug: creating-new-data-with-max-values-for-each-subject-2
---

We have a data set _dat_ with multiple observations per subject. We want to create a subset of this data such that each subject (with **ID** giving the unique identifier for the subject) contributes the observation where the variable **X** takes it's maximum value for that subject.

## R solutions

### Hadleyverse solutions

Using the excellent R package _[dplyr](http://cran.r-project.org/package=dplyr)_, we can do this using windowing functions included in _dplyr_. The following solution is available on [StackOverflow](http://stackoverflow.com/questions/21308436/dplyr-filter-get-rows-with-minimum-of-variable-but-only-the-first-if-multiple), by [junkka](http://stackoverflow.com/users/1557026/junkka), and gets around the real possibility that multiple observations might have the same maximum value of **X** by choosing one of them.

```r
library(dplyr)
dat_max <- dat %>% group_by(ID) %>% filter(row_number(X)==n())
```

To be a bit more explicit, `row_number` is a wrapper around `rank`, using the option `ties.method="first"`. So you can use the `rank` function explicitly here as well.

A solution using the [plyr](http://cran.r-project.org/package=plyr) package might be

```r
library(plyr)
dat_max <- ddply(dat, .(ID), function(u) u[rank(u$X, ties.method='first')==nrow(u),])
```

### A  data.table solution

The [data.table](http://cran.r-project.org/package=data.table) package also has great munging capabilities like dplyr. The same StackOverflow thread linked above also provides this solution using `data.table`, provided by  [Arun](http://stackoverflow.com/users/559784/arun):

```r
dt <- as.data.table(dat)
dt[dt[, .I[which.max(X)], by=ID]$V1]
```

### Using SQL statements via sqldf

The [sqldf](http://cran.r-project.org/package=sqldf) package allows an easy implementation of using SQL statements on data frame objects. As Ryan mentions in the comments, the possibilities of solving our problem using _sqldf_ is straightforward:

```r
library(sqldf)
bl <- sqldf("select ID, max(X) from dat group by ID")
names(bl)[2] <- 'X'
sqldf('select * from bl LEFT JOIN dat using(ID, X)')
```

## Python solutions

### Using pandas

In Python, the excellent [pandas](http://pandas.pydata.org) package allows us to do similar operations. The following example is from this [thread](http://stackoverflow.com/questions/19818756/extract-row-with-maximum-value-in-a-group-pandas-dataframe) on StackOverflow.

```python
import pandas as pd
df = DataFrame({'Sp':['a','b','c','d','e','f'], 'Mt':['s1', 's1', 's2','s2','s2','s3'], 'Value':[1,2,3,4,5,6], 'count':[3,2,5,10,10,6]})
df.iloc[df.groupby(['Mt']).apply(lambda x: x['count'].idxmax())][/code]
```

You could also do (from the same thread)

````
df.sort('count', ascending=False).groupby('Mt', as_index=False).first()
````

but it is about 50% slower.

### Using pandasql

The package [pandasql](https://pypi.python.org/pypi/pandasql) is a port of `sqldf` to Python developed by
[yhat](https://yhathq.com/). Using this package, we can mimic the `sqldf` solution given above:

````python
from pandasql import sqldf
sqldf('select Sp, Mt, Value, max(count) from df group by Mt', globals())
````
