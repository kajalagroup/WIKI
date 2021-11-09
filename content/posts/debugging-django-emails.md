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
    
After that:

    EMAIL_HOST = 'localhost'
    EMAIL_HOST_PASSWORD = ''
    EMAIL_HOST_USER = ''
    EMAIL_PORT = 1025
    EMAIL_USE_SSL = False
    
Then you get email contents printed to terminal:

![Example image](django-email-debug.png)
