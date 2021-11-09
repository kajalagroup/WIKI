+++
title = "Debugging Django emails"
description = "Debug emails sent by Django locally"
tags = [
    "Django",
    "backend"
]
date = "2021-11-02"
categories = [
    "Development", 
    "Django",
]
menu = "main"
+++ 

Run in terminal:

        python -m smtpd -n -c DebuggingServer localhost:1025
    
After that in Django's settings.py:

        EMAIL_HOST = 'localhost'
        EMAIL_HOST_PASSWORD = ''
        EMAIL_HOST_USER = ''
        EMAIL_PORT = 1025
        EMAIL_USE_SSL = False
    
Then you get all sent email contents printed to terminal:

![django-email-debug.png]((/wiki/images/django-email-debug.png)
