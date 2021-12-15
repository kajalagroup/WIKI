+++
title = "Set redirect URL after updated Django model in admin"
description = "Set redirect URL after updated Django model in admin"
tags = [
    "Django",
    "backend"
]
date = "2021-12-15"
categories = [
    "Development", 
    "Django",
]
+++ 
Currently if you do a filter or search in list view in admin in Django, after update a row in given list (Django will open edit page). After that model is updated, Django will redirect to list url without keep search or filter condition that you made before.

Here is the solution to make Django redirect to previous list view url.

Firstly, you need define own ChangeList.

```py 

from jutil.admin import admin_obj_url
from django.contrib.admin.views.main import ChangeList
class MyChangeList(ChangeList):
    def __init__(self, request, *args):
        super().__init__(request, *args)
        self.current_url = request.get_full_path()  # Store  current url

    def url_for_result(self, result):
        return admin_obj_url(result) + "?next=" + mark_safe(self.current_url)  # Add current url to next param when open update url.
```

Next here is code in your model admin class.

```py

from django.contrib import admin
from django.shortcuts import redirect
class MyAdmin(admin.ModelAdmin):
    def get_changelist(self, request, **kwargs):
        return MyChangeList

    def response_post_save_change(self, request, obj):
        if 'next' in request.GET:  # Check if next is passed via url, redirect to previous url
            next_url = request.GET['next']
            return redirect(next_url)
        return super().response_post_save_change(request, obj)
```
