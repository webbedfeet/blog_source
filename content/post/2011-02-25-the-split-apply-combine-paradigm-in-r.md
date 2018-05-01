---
title: The split-apply-combine paradigm in R
author: Abhijit Dasgupta
date: '2011-02-25'
tags:
  - data.table
  - meetup
  - plyr
  - R
slug: the-split-apply-combine-paradigm-in-r
---

Last night at the [DC R Users meetup](http://www.meetup.com/R-users-DC/events/16530752/), which was our largest meetup to date, I gave an introductory presentation on data munging, and spent a bit of time on the split-apply-combine paradigm that I use almost daily in my work. I talked mainly about the packages `plyr` and `doBy`, which I use a lot now. David Smith posted a link on the Revolution blog to this [article](http://www.information-management.com/blogs/open_source_R_bygroup_processing_data_management-10019778-1.html) by Steve Miller, talking about the virtues of the `data.table` package for doing "by-group processing". It got me thinking about changing my workflow yet again and engaging this package in my computational workflow. I also noticed that Hadley Wickham [tweeted](http://twitter.com/#!/hadleywickham/status/41150539232190464) that he wants to make plyr faster as well in the near future, which will of course be a very welcome development.
