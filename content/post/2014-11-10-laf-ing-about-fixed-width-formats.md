---
title: '"LaF"-ing about fixed width formats'
author: Abhijit Dasgupta
date: '2014-11-10'
categories: ['R']
tags:
  - R
slug: laf-ing-about-fixed-width-formats
---

If you have ever worked with US government data or other large datasets, it is likely you have faced fixed-width format data. This format has no delimiters in it; the data look like strings of characters. A separate format file defines which columns of data represent which variables. It seems as if the format is from the punch-card era, but it is quite an efficient format to store large data in (see this [StackOverflow discussion](http://stackoverflow.com/questions/7666780/why-are-fixed-width-file-formats-still-in-use)). However, it requires quite a bit of coding, in terms of defining which columns represent which variables.

A recent project that our group at NIH is working on involves the National Inpatient Sample ([NIS](http://www.hcup-us.ahrq.gov/)) from the Healthcare Cost and Utilization Project ([HCUP](http://www.hcup-us.ahrq.gov/)). The current task is to load the Core data (8.2M records for 2008), filter it for a particular set of [DRG codes](http://en.wikipedia.org/wiki/Diagnosis-related_group), and see how many discharges with these DRG codes are present in each hospital in the set of hospitals included in the sample. Furthermore, we need to categorize each hospital as low, medium and high volume hospitals based on the number of discharges.

<blockquote>Note that to protect our project, I will not use the DRG codes that we're actually using. I actually don't know what DRG=500 is in terms of what the patient undergoes.</blockquote>

## Translating available code

HCUP provides load files for their data sets in SAS, SPSS and Stata. Note that no open-source data software made the cut, possibly for legacy reasons. This post will focus on a [R](http://www.r-project.org) based solution. A follow up post will present a [Python](http://www.python.org) based solution.

First of all, I admit that I am lazy. I don't want to sit with the data dictionary and write out all the format statements in R. However, if HCUP is providing load files, why not use them? Looking at them, I felt that the Stata load file was cleanest in terms of formatting. I wrote the following R script to extract variable names, column widths and formats (string, double, or integer) from the load file.

```r
parseStataCode <- function(statafile){
 require(stringr)
 options(stringsAsFactors=FALSE)
 statacode <- readLines(statafile)
 indStart <- min(grep('infix', statacode))
 indEnd <- min(grep('using', statacode))-1
 codelines <- statacode[indStart:indEnd]
 codelines[1] <- gsub('infix','', codelines[1])
 codelines = str_trim(codelines)
 types <- str_extract(codelines, '^\\w+')
 translate <- c('int'='integer', 'byte'='integer', 'str'='string', 'double'='double', 'long'='double')
 types2 <- translate[types]
 varnames <- str_extract(codelines, '[A-Z]\\w+')
 startcols <- as.integer(str_extract(codelines,' \\d+'))
 maxcols = unlist(str_extract_all(codelines,'\\d+'))
 startcols = c(startcols, as.integer(maxcols[length(maxcols)]))
 widths = diff(startcols)
 return(data.frame(varnames = varnames, widths=widths, classes=types2))
}
```

You can of course add more code to extract missing value definitions and the like, but this sufficed for my current purpose.

<blockquote>My friend Anthony Damico wrote a great little package in R called [SAScii](http://cran.r-project.org/package=SAScii), for the purpose of translating SAS load files provided by different US Government agencies for publicly available data (see his site [ASDFree](http://www.asdfree.com) for several nice examples with large government surveys). However, the HCUP SAS load files contain custom formatting statements that breaks Anthony's code. Hence the need to create my own translation code.</blockquote>

## Ingesting data

Now on to the actual job of reading the data. R provides a function read.fwf, but that is painfully slow, so I immediately went to the Gods of Google. A quick internet search found the package [LaF](http://cran.r-project.org/package=LaF), which uses C++ underneath to make accessing large ASCII files faster. LaF also allows some column selection and filtering as data is loaded into memory in R. The workhorse for this is the [laf_open_fwf](http://www.inside-r.org/packages/cran/LaF/docs/laf_open_fwf) function, which requires specification of the column widths and types.

```r
wt <- parseStataCode('StataLoad_NIS_2008_Core.Do')
d <- laf_open_fwf('NIS_2008_Core.ASC', column_widths=wt$widths,
 column_types=wt$classes, column_names = wt$varnames)
```

The code above does not actually load the data into R, but essentially creates a pointer to the data on disk. Now I decided to use the (awesome) power of [dplyr](http://cran.r-project.org/package=dplyr) and [magrittr](http://cran.r-project.org/package=magrittr) to extract the frequencies I need. This strategy provides very clear and fast code that I'm happy with right now.

```r
d1 <- d[,c("DRG","HOSPID")] %>%                           # Select only columns I need
 dplyr::filter(DRG %in% c(500)) %>%                       # Keep DRGs I want
 group_by(HOSPID) %>%
 summarize(n=n()) %>%                                     # Tabulate volumes by hospital
 mutate(volumeCat = cut(n, c(0,100,400,max(n)+1),
     labels=c('Low','Medium','High'))) %>%                # Categorize hospitals
 group_by(volumeCat) %>%
 summarize(n=n())                                         # Tabulate hospital categories
```

<blockquote>Why am I doing the filtering using dplyr rather than LaF? At least for my initial runs, it seemed that the dplyr strategy is faster. You could, in fact, use LaF as follows: `d[d[,"DRG"] %in% c(500), c("DRG","HOSPID")]. `See the LaF user manual for details about the syntax for row filtering, since it is a bit idiosyncratic.</blockquote>

So how does this actually perform? We're working with 8.2 million records here. I ran my code (after wrapping it in a function) on my desktop (Intel Xeon @2.67 Ghz, 8GB RAM running Windows 7 64-bit Enterprise Edition, running R 3.1.1).

```
system.time(freq <- zip2freq('NIS_2008_Core.ASC', drg=c(500)))
user system elapsed
3.19 1.57 45.56
```

On my MacBook Air (2014) with 8GB memory and a 1.4 GHz Intel Core i5 and a Solid State Disk, this took about 16 seconds (elapsed time).

For contrast, I used the HCUP-supplied [SAS load file](http://www.hcup-us.ahrq.gov/db/nation/nis/tools/pgms/SASLoad_NIS_2008_Core.SAS) (on SAS 9.3), adding the lines

```sas
if DRG=500;
keep DRG HOSPID;
```

to the end of the provided DATA statement to filter the data and keep only the two variables I need. I'm not good enough in SAS to do the remaining data munging efficiently, but the timings were interesting to say the least.

```
NOTE: 8158381 records were read from the infile 'H:\tmp\NIS_2008_Core.ASC'. The minimum record length was 516. The maximum record length was 516. NOTE: The data set WORK.NIS_2008_CORE has 1068 observations and 2 variables. NOTE: DATA statement used (Total process time): real time 2:26.37 cpu time 2:11.15
```
