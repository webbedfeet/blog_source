---
title: 'Changing names in the tidyverse: An example for many regressions'
author: Abhijit Dasgupta
date: '2017-03-09'
categories:
  - R
slug: changing-names-in-the-tidyverse-an-example-for-many-regressions
---

A collaborator posed an interesting R question to me today. She wanted to do
several regressions using different outcomes, with models being computed on
different strata defined by a combination of experimental design variables. She then just wanted to extract the p-values for the slopes for each of the models, and then
filter the strata based on p-value levels.

This seems straighforward, right? Let's set up a toy example:

````
library(tidyverse)

dat <- as_tibble(expand.grid(letters[1:4], 1:5))
d <- vector('list', nrow(dat))
set.seed(102)
for(i in 1:nrow(dat)){
x <- rnorm(100)
d[[i]] <- tibble(x = x, y1 = 3 - 2*x + rnorm(100), y2 = -4+5*x+rnorm(100))
}
dat <- as_tibble(bind_cols(dat, tibble(dat=d))) %>% unnest()
knitr::kable(head(dat), format='html')
````

<table >

<tr >
Var1
Var2
x
y1
y2
</tr>

<tbody >
<tr >

<td style="text-align:left;" > a
</td>

<td style="text-align:right;" > 1
</td>

<td style="text-align:right;" > 0.1805229
</td>

<td style="text-align:right;" > 4.2598245
</td>

<td style="text-align:right;" > -3.004535
</td>
</tr>
<tr >

<td style="text-align:left;" > a
</td>

<td style="text-align:right;" > 1
</td>

<td style="text-align:right;" > 0.7847340
</td>

<td style="text-align:right;" > 0.0023338
</td>

<td style="text-align:right;" > -2.104949
</td>
</tr>
<tr >

<td style="text-align:left;" > a
</td>

<td style="text-align:right;" > 1
</td>

<td style="text-align:right;" > -1.3531646
</td>

<td style="text-align:right;" > 3.1711898
</td>

<td style="text-align:right;" > -9.156758
</td>
</tr>
<tr >

<td style="text-align:left;" > a
</td>

<td style="text-align:right;" > 1
</td>

<td style="text-align:right;" > 1.9832982
</td>

<td style="text-align:right;" > -0.7140910
</td>

<td style="text-align:right;" > 5.966377
</td>
</tr>
<tr >

<td style="text-align:left;" > a
</td>

<td style="text-align:right;" > 1
</td>

<td style="text-align:right;" > 1.2384717
</td>

<td style="text-align:right;" > 0.3523034
</td>

<td style="text-align:right;" > 2.131004
</td>
</tr>
<tr >

<td style="text-align:left;" > a
</td>

<td style="text-align:right;" > 1
</td>

<td style="text-align:right;" > 1.2006174
</td>

<td style="text-align:right;" > 0.6267716
</td>

<td style="text-align:right;" > 1.752106
</td>
</tr>
</tbody>
</table>

Now we're going to perform two regressions, one using `y1` and one using `y2` as the dependent variables, for each stratum defined by `Var1` and `Var2`.

````
out <- dat %>%
nest(-Var1, -Var2) %>%
mutate(model1 = map(data, ~lm(y1~x, data=.)),
model2 = map(data, ~lm(y2~x, data=.)))
````

Now conceptually, all we do is tidy up the output for the models using the `broom` package, filter on the rows containg the slope information, and extract the p-values, right? Not quite....

````
library(broom)
out_problem <- out %>% mutate(output1 = map(model1, ~tidy(.)),
output2 = map(model2, ~tidy(.))) %>%
select(-data, -model1, -model2) %>%
unnest()
names(out_problem)
````

[1] "Var1" "Var2" "term" "estimate" "std.error"
[6] "statistic" "p.value" "term" "estimate" "std.error"
[11] "statistic" "p.value"

We've got two sets of output, but with the same column names!!! This is a problem! An easy solution would be to preface the column names with the name of the response variable. I struggled with this today until I discovered the _secret function_.

````
out_nice <- out %>% mutate(output1 = map(model1, ~tidy(.)),
output2 = map(model2, ~tidy(.)),
output1 = map(output1, ~setNames(., paste('y1', names(.), sep='_'))),
output2 = map(output2, ~setNames(., paste('y2', names(.), sep='_')))) %>%
select(-data, -model1, -model2) %>%
unnest()
````

This is a compact representation of the results of both regressions by strata, and we can extract the information we would like very easily. For example, to extract the stratum-specific slope estimates:

````
out_nice %>% filter(y1_term=='x') %>%
select(Var1, Var2, ends_with('estimate')) %>%
knitr::kable(digits=3, format='html')
````

<table >

<tr >
Var1
Var2
y1_estimate
y2_estimate
</tr>

<tbody >
<tr >

<td style="text-align:left;" > a
</td>

<td style="text-align:right;" > 1
</td>

<td style="text-align:right;" > -1.897
</td>

<td style="text-align:right;" > 5.036
</td>
</tr>
<tr >

<td style="text-align:left;" > b
</td>

<td style="text-align:right;" > 1
</td>

<td style="text-align:right;" > -2.000
</td>

<td style="text-align:right;" > 5.022
</td>
</tr>
<tr >

<td style="text-align:left;" > c
</td>

<td style="text-align:right;" > 1
</td>

<td style="text-align:right;" > -1.988
</td>

<td style="text-align:right;" > 4.888
</td>
</tr>
<tr >

<td style="text-align:left;" > d
</td>

<td style="text-align:right;" > 1
</td>

<td style="text-align:right;" > -2.089
</td>

<td style="text-align:right;" > 5.089
</td>
</tr>
<tr >

<td style="text-align:left;" > a
</td>

<td style="text-align:right;" > 2
</td>

<td style="text-align:right;" > -2.052
</td>

<td style="text-align:right;" > 5.015
</td>
</tr>
<tr >

<td style="text-align:left;" > b
</td>

<td style="text-align:right;" > 2
</td>

<td style="text-align:right;" > -1.922
</td>

<td style="text-align:right;" > 5.004
</td>
</tr>
<tr >

<td style="text-align:left;" > c
</td>

<td style="text-align:right;" > 2
</td>

<td style="text-align:right;" > -1.936
</td>

<td style="text-align:right;" > 4.969
</td>
</tr>
<tr >

<td style="text-align:left;" > d
</td>

<td style="text-align:right;" > 2
</td>

<td style="text-align:right;" > -1.961
</td>

<td style="text-align:right;" > 4.959
</td>
</tr>
<tr >

<td style="text-align:left;" > a
</td>

<td style="text-align:right;" > 3
</td>

<td style="text-align:right;" > -2.043
</td>

<td style="text-align:right;" > 5.017
</td>
</tr>
<tr >

<td style="text-align:left;" > b
</td>

<td style="text-align:right;" > 3
</td>

<td style="text-align:right;" > -2.045
</td>

<td style="text-align:right;" > 4.860
</td>
</tr>
<tr >

<td style="text-align:left;" > c
</td>

<td style="text-align:right;" > 3
</td>

<td style="text-align:right;" > -1.996
</td>

<td style="text-align:right;" > 5.009
</td>
</tr>
<tr >

<td style="text-align:left;" > d
</td>

<td style="text-align:right;" > 3
</td>

<td style="text-align:right;" > -1.922
</td>

<td style="text-align:right;" > 4.894
</td>
</tr>
<tr >

<td style="text-align:left;" > a
</td>

<td style="text-align:right;" > 4
</td>

<td style="text-align:right;" > -2.000
</td>

<td style="text-align:right;" > 4.942
</td>
</tr>
<tr >

<td style="text-align:left;" > b
</td>

<td style="text-align:right;" > 4
</td>

<td style="text-align:right;" > -2.000
</td>

<td style="text-align:right;" > 4.932
</td>
</tr>
<tr >

<td style="text-align:left;" > c
</td>

<td style="text-align:right;" > 4
</td>

<td style="text-align:right;" > -2.033
</td>

<td style="text-align:right;" > 5.042
</td>
</tr>
<tr >

<td style="text-align:left;" > d
</td>

<td style="text-align:right;" > 4
</td>

<td style="text-align:right;" > -2.165
</td>

<td style="text-align:right;" > 5.049
</td>
</tr>
<tr >

<td style="text-align:left;" > a
</td>

<td style="text-align:right;" > 5
</td>

<td style="text-align:right;" > -2.094
</td>

<td style="text-align:right;" > 5.010
</td>
</tr>
<tr >

<td style="text-align:left;" > b
</td>

<td style="text-align:right;" > 5
</td>

<td style="text-align:right;" > -1.961
</td>

<td style="text-align:right;" > 5.122
</td>
</tr>
<tr >

<td style="text-align:left;" > c
</td>

<td style="text-align:right;" > 5
</td>

<td style="text-align:right;" > -2.106
</td>

<td style="text-align:right;" > 5.153
</td>
</tr>
<tr >

<td style="text-align:left;" > d
</td>

<td style="text-align:right;" > 5
</td>

<td style="text-align:right;" > -1.974
</td>

<td style="text-align:right;" > 5.009
</td>
</tr>
</tbody>
</table>
