---
title: pandas "transform" using the tidyverse
author: Abhijit Dasgupta
date: '2017-04-11'
categories:
  - Python
tags:
  - pydata
  - R
slug: pandas-transform-using-the-tidyverse
---

Chris Moffit has a nice [blog](http://pbpython.com/pandas_transform.html) on how to use the `transform` function in `pandas`. He provides some (fake) data on sales and asks the question of what fraction of each order is from each SKU.

Being a R nut and a `tidyverse` fan, I thought to compare and contrast the code for the `pandas` version with an implementation using the tidyverse.

First the `pandas` code:
````python
import pandas as pd
dat = pd.read_excel('sales_transactions.xlsx')
dat['Percent_of_Order'] = dat['ext price']/dat.groupby('order')['ext price'].transform('sum')
````

A similar implementation using the tidyverse:
````r
library(tidyverse)
library(readxl)
dat <- read_excel('sales_transactions.xlsx')
dat <- dat %>%
group_by(order) %>%
mutate(Percent_of_Order = `ext price`/sum(`ext price`))
````
