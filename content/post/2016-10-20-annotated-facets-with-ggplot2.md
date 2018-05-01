---
title: Annotated Facets with ggplot2
author: Abhijit Dasgupta
date: '2016-10-20'
categories:
  - Data Science
  - R
slug: annotated-facets-with-ggplot2
---

I was recently asked to do a panel of grouped boxplots of a continuous variable,
with each panel representing a categorical grouping variable. This seems easy enough with ggplot2 and the `facet_wrap` function, but then my collaborator wanted p-values on the graphs! This post is my approach to the problem.

First of all, one caveat. I'm a huge fan of Hadley Wickham's [tidyverse](https://blog.rstudio.org/2016/09/15/tidyverse-1-0-0/) and so most of my code will reflect this ethos, including packages and pipes. There are, of course, other approaches to doing this.

````rlibrary(broom)
library(ggplot2)
library(tidyverse)
````

I will use the `warpbreaks` dataset as an example. This data looks at the number of breaks in yarn, for two types of yarn (A and B) and three loom tensions (L, M, H)

````rdata(warpbreaks)
glimpse(warpbreaks)
````

````## Observations: 54
## Variables: 3
## $ breaks  <dbl> 26, 30, 54, 25, 70, 52, 51, 26, 67, 18, 21, 29, 17, 12...
## $ wool    <fctr> A, A, A, A, A, A, A, A, A, A, A, A, A, A, A, A, A, A,...
## $ tension <fctr> L, L, L, L, L, L, L, L, L, M, M, M, M, M, M, M, M, M,...
````

### A single panel

Let's first start with creating a grouped boxplot of breaks by wool type. This is pretty straightforward in ggplot2.

````rplt1 <- ggplot(warpbreaks, aes(x=wool, y=breaks))+
geom_boxplot()
plt1
````

![plot of chunk plt1](https://dl.dropboxusercontent.com/u/36792950/wp/figure/plt1-1.png)

Adding the p-value to this is also rather simple. We will perform a two-sample t.test and report the p-value on the plot

````rtst = t.test(breaks ~ wool, data=warpbreaks)
pval = tidy(tst)$p.value # from broom
plt1 + geom_text(aes(x=2, y = 60, # Where do we put the annotation
label=paste('P-value:', format.pval(pval, digits=2))))
````

![plot of chunk plt1-1](https://dl.dropboxusercontent.com/u/36792950/wp/figure/plt1-1-1.png)

### Multiple panels

I'll now show how to do multiple panels. First, we have to transform the data into long format so that ggplot2 can leverage it.

````rwarp2 <- gather(warpbreaks, variable, value, -breaks)
head(warp2)
````

````##   breaks variable value
## 1     26     wool     A
## 2     30     wool     A
## 3     54     wool     A
## 4     25     wool     A
## 5     70     wool     A
## 6     52     wool     A
````

Now we can plot this using ggplot2

````rggplot(warp2, aes(x = value, y = breaks))+geom_boxplot()+facet_wrap(~variable, nrow=1)
````

![plot of chunk fig1](https://dl.dropboxusercontent.com/u/36792950/wp/figure/fig1-1.png)

Ooops!! When we used `gather` to melt the data frame and create one variable for the two categorical variables, with the levels all shared in the `variable` column. So the plot shows all the possible categories. We can easily fix this.

````rplt2 <- ggplot(warp2, aes(x=value, y=breaks))+geom_boxplot()+facet_wrap(~variable, nrow=1, scales='free_x')
````

Next, we want to put a p-value on each plot. The trick is to create a new data frame for the p-values, with one row per level of the facetting variable. In our example, wool has two levels while tension has three, so to create one p-value per plot I'll run ANOVA models and provide the p-value of the model effect, i.e., is there any difference in the average number of breaks by levels of each predictor. I'll use the template Wickham suggests about [running multiple models](http://r4ds.had.co.nz/many-models.html) here.

````rpvalues <- warp2 %>%
nest(-variable) %>%
mutate(mods = map(data, ~anova(lm(breaks ~ value, data = .))),
pval = map_dbl(mods, ~tidy(.)$p.value[1])) %>%
select(variable, pval)
pvalues
````

````## # A tibble: 2 × 2
##   variable        pval
##      <chr>       <dbl>
## 1     wool 0.108397092
## 2  tension 0.001752817
````

I'll also include in this data frame (or `tibble` as the tidyverse calls it) the location of the annotation on each graph, using the same variable names as the graph

````rpvalues <- pvalues %>%
mutate(value = c(1.5,2.5), breaks = 60)
pvalues
````

````## # A tibble: 2 × 4
##   variable        pval value breaks
##      <chr>       <dbl> <dbl>  <dbl>
## 1     wool 0.108397092   1.5     60
## 2  tension 0.001752817   2.5     60
````

Now, let's plot these onto the facetted plot

````rplt2_annot <- plt2 +
geom_text(data=pvalues, aes(x=value, y=breaks,
label = paste('P-value:',
format.pval(pval, digits=1))))
plt2_annot
````

![plot of chunk annotated](https://dl.dropboxusercontent.com/u/36792950/wp/figure/annotated-1.png)

Obviously, if we wanted to make this graph publication quality, we'd need to modify the labels and perhaps the background, but this example gets you to the point where you can put p-values onto facetted plots. In fact, there's nothing special about p-values here; you could add any facet-specific annotation onto your facetted plots using the same template.
