---
title: A ggplot trick to plot different plot types in facets
author: Abhijit Dasgupta
date: '2011-07-29'
categories:
  - R
slug: a-ggplot-trick-to-plot-different-plot-types-in-facets
---

At the DC useR meetup last week, Marck Vaisman (@wahalulu) showed me a neat trick he'd learned to allow different facets in a faceted ggplot graph to have different plot types. The basis for this trick is [this blog post](http://learnr.wordpress.com/2009/05/18/ggplot2-three-variable-time-series-panel-chart/) in the [Learn-R blog](http://learnr.wordpress.com). Marck was trying to plot different statistics on our Meetup group's membership on a faceted plot. Some of the variables were amenable to a step plot while others were more amenable to plotting using vertical lines.

The interesting trick in this example is to use the subset command within each geom to only layer one facet at a time. The source code is given below:

````r
meetup <- read.csv('MeetupDates.csv', as.is=T)
names(meetup) <- 'Dates'
meetup$Dates <- as.Date(meetup$Dates,format='%m/%d/%y')
files  <- dir(pattern='DC_useR')
bl <- list()
for(f in files){
  bl[[f]] <- read.csv(f, as.is=T)
  bl[[f]]$Date <- as.Date(bl[[f]]$Date,format='%m/%d/%y')
}
dat <- Reduce(function(x,y) merge(x,y), bl) # Merge the data frames by Date
dat2 <- melt(dat,id=1)

# Here comes the trick !!
f1 <- ggplot(dat2, aes(x=Date,y=value,ymin=0,ymax=value))+facet_grid(variable~., scales='free')
f2 <- f1+geom_step(subset=.(variable=='Total.Members'))
f3 <- f2+geom_step(subset=.(variable=='Active.Members'))
f4 <- f3+geom_linerange(subset=.(variable=='Member.Joins'))
f5 <- f4+geom_linerange(subset=.(variable=='RSVPs'))
f5+geom_vline(xintercept=meetup$Dates, color='red',alpha=.3)+ylab('')

````

This produces the following plot:

[caption id="attachment_188" align="aligncenter" width="530" caption="A faceted ggplot object with different plot types"]![](http://statbandit.files.wordpress.com/2011/07/rplot01.png)[/caption]
