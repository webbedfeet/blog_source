---
title: 'Slidify: Data driven presentations'
author: Abhijit Dasgupta
date: '2014-03-31'
categories:
  - R
slug: slidify-data-driven-presentations
---

**Publishers note: This blog was posted on August 1, 2013 on the Data Community DC blog, http://datacommunitydc.org/blog/2013/08/data-driven-presentations-using-slidify/**

Presentations are the stock-in-trade for consultants, managers, teachers, public speakers, and, probably, you. We all have to present our work at some level, to someone we report to or to our peers, or to introduce newcomers to our work. Of course, presentations are passe, so why blog about it? There's already PowerPoint, and maybe Keynote. What more need we talk about?

Well, technology has changed, and vibrant dynamic presentations are here today for everyone to see. No, I mean literally everybody, if I like. All anyone will need is a web browser to see it. Graphs can be interactive, flow can be nonlinear, and presentations can be fun and memorable again!

[![](http://statbandit.files.wordpress.com/2014/04/screen-shot-2014-04-01-at-11-35-55-pm.png)
](http://slidify.github.io)
But PowerPoint is so easy! You click, paste, type, add a bit of glitz, and you're done, right? Well, as most of us can attest to, not really. It takes a bit more effort and putzing around to really get things in reasonable shape, let alone great shape.

And there **are** powerful alternatives. Which are simple and easy. And do a pretty great job on their own. Oh, and, by the way, if you have data and analysis results to present, super slick and a one-stop-shop from analysis to presentation. Really!! Actually there are a few out there, but I'm going to talk about just one. My favorite. Slidify.

[Slidify](http://www.slidify.com) is a fantastic R package that takes a document written in [RMarkdown](http://www.rstudio.com/ide/docs/r_markdown), which is [Markdown](http://daringfireball.net/projects/markdown/) possibly interspersed with [R](http://www.r-project.org) code that result in tables or figures or interactive graphics, weaves in the results of that code, and then formats it into beautiful web presentations using HTML5. You can decide on the format template (it comes with [quite a few](http://ramnathv.github.io/slidifyExamples/) or brew your own. You can make your presentation look and behave the way you want, even like a [Prezi](http://www.prezi.com) (using [ImpressJS](http://bartaz.github.io/imporess.js/)). You can also make interactive questionnaires and even put in windows to code interactively within your presentation!!

Slidify is obviously feature-rich, and infinitely customizable, but that's not really what attracted me to it. It was the ability to write presentations in Markdown, which is super easy and let's me put down content quickly without worrying about appearance (Between you and me, I'm writing this post in Markdown, on a Nexus 7). And let me weave in results of my analyses easily, keeping the code in one place within my document. So when my data changes, I can create an updated presentation literally with the press of a button. Markdown is geared to create HTML documents. Pandoc and MultiMarkdown let you create HTML _presentations_ from Markdown, but not living, data driven presentations like Slidify. I get to put my presentations up on [Github](http://www.github.com) or on [Rpubs](http://www.rpubs.com), or even in [Dropbox](http://www.dropbox.com), directly using Slidify, share the link, and I'm good to go.

Dr. Ramnath Vaidyanathan created Slidify to help him teach more effectively at McGill University, where he is on the faculty of the School of Business. But, for me, it is now the goto place for creating presentations , even if I don't need to incorporate data. If you're an analyst and live in the R ecosystem, I highly recommend Slidify. If you don't and use other tools, Slidify is a great reason to come and see what R can do for you. Even if it to just create great presentations. There are plenty of great examples of what's possible at <http://ramnathv.github.io/slidifyExamples>.
