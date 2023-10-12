+++
title = "Customizing Django Admin search"
description = ""
tags = [
    "Django",
    "backend"
]
date = "2023-10-11"
categories = [
    "Development", 
    "Django",
]
menu = "main"
+++ 

Steps to override search form in Django admin:

1. Add custom search form
2. Override ChangeList and implement get_filters_params() so that it excludes the new search form fields (otherwise filters don't work)
3. Override search_form.html because it has hardcoded single search form field
4. Override change_list.html to customize styling (e.g. with flexbox)
5. If search form has many fields, split fields to multiple divs by using cl.search_form.visible_fields|slice:"0:5" and rendering fields manually

