---
title: RStudio 0.94.92 visited
author: Abhijit Dasgupta
date: '2011-07-30'
tags:
  - R
  - RStudio
slug: rstudio-0-94-92-visited
---

I just updated my RStudio version to the latest, v.0.94.92 (will this asymptotically approach 1, or actually get to 1?). It was nice to see the number of improvements the development team has implemented, based I'm sure on community feedback. The team has, in my experience, been extraordinarily responsive to user feedback, and I'm sure this played a large part in the development path taken by the team.

First and foremost, I was happy to see most of my wants met in this version:

  * There now is a keyboard shortcut for <- that is easy and intuitive (Alt+_/Option+_)
  * The File window now allows sorting by modification date  in addition to name, which was becoming an issue for one of my projects
  * Plots can be saved as BMP, TIFF, JPEG and Postscript in addition to PNG and PDF
  * Bracket completion and matching, very much similar to the R Mac GUI, and actually better than Emacs/ESS, specially when deleting.
  * An easy shortcut to repeat blocks of text or transpose two lines of text (though this appears mistakenly overloaded with another shortcut on Windows/Linux)
  * Keyboard shortcuts are reasonably consistent with OS-specific shortcuts, though the Ctrl key is used in Mac more than generally seen in the OS. It is however convenient for those of us migrating from Emacs/ESS, who use the Ctrl key often.

My wishlist for RStudio is pretty much fulfilled with respect to R development. However, a few improvements need to be made in the TeX/Sweave interface to allow for autocompletion, templates, and fuller functionality in line with Emacs/Auctex and Texmate. Currently writing LaTeX and Sweave feels like writing in Wordpad, albeit with R-specific word completion and R functionality. This can be a bit more polished. Of course TeX and Sweave are still used by a minority of R users, so the fact that this functionality hasn't developed is no surprise.

All in all, the current version of RStudio feels like a very usable IDE for R, and certain features and similarities make migrating from Emacs pretty easy (provided you don't miss Emacs' overall power and flexibility too much)
