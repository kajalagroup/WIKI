# WIKI
This repository notes all piece of codes that you struggled to solve.


## Write your first post:

1. Checkout master branch:
2. add new markdown file in folder content/posts

This is example how structure of markdown file will look like. You can define title, publish date, description, tags, categories, full content.
```
+++
title = "Your post title"
description = "Short description of your post"
tags = [
    "Django",
    "frontend"
]
date = "2021-11-02"
categories = [
    "Development", 
    "Django",
]
menu = "main"
+++ 

This is full content for web filter

## Introduce

### Step 1:
This is for step 1

### Step 2:
This is for step 2

## Insert image

Added your file static folder:

![Example image](/WIKI/images/test.png)
```

3. If you insert image, add your image to /static folder
4. push to master branch.
5. Waiting for 2 minutes to github Action do its job and visit https://kajalagroup.github.io/WIKI/posts/ your post will be appeared here.




## Install and test local:

Install this package:
```
https://github.com/gohugoio/hugo/releases/download/v0.88.1/hugo_extended_0.88.1_Linux-64bit.tar.gz

```



## Serve:

```
hugo server
```


## Build
```
hugo -D
```
## Theme:

https://github.com/alex-shpak/hugo-book
