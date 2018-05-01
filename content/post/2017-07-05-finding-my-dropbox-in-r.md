---
title: Finding my Dropbox in R
author: Abhijit Dasgupta
date: '2017-07-05'
categories:
  - R
slug: finding-my-dropbox-in-r
---

I'll often keep non-sensitive data on Dropbox so that I can access it on all my machines without gumming up git. I just wrote a small script to find the Dropbox location on each of my computers automatically. The crucial information is available [here](https://www.dropbox.com/help/desktop-web/find-folder-paths), from Dropbox.

My small snippet of code is the following:

````
if (Sys.info()['sysname'] == 'Darwin') {
  info <- RJSONIO::fromJSON(
    file.path(path.expand("~"),'.dropbox','info.json'))
}
if (Sys.info()['sysname'] == 'Windows') {
  info <- RJSONIO::fromJSON(
```r
if (file.exists(file.path(Sys.getenv('APPDATA'), 'Dropbox','info.json'))) {
  file.path(Sys.getenv('APPDATA'), 'Dropbox', 'info.json')
} else {
file.path(Sys.getenv('LOCALAPPDATA'),'Dropbox','info.json')
}
```
  )
}

dropbox_base <- info$personal$path
````

I haven't included the Linux option since I don't really use a Linux box, but the Dropbox link above will show you where the info.json file lies in Linux. Also, if you have a business Dropbox account, you'll probably need `info$business$path`.

Hope this helps!!!
