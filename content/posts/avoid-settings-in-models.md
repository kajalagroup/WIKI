+++
title = "Using settings.XXX as defaults in model fields"
description = "Avoid direct settings.XXX"
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

When you have settings.XXXX defined in settings.py, and if you use it in a model e.g.

        country = CharField(max_length=3, default=settings.COUNTRY)

.. makemigrations will generate alter-field migration always if you change COUNTRY.
This can be problem if you have multiple instances of the app deployed with different COUNTRY setting.

Better way is to use function to abstract away actual value:


        def get_system_country() -> str:
            return settings.COUNTRY

        ...

        country = CharField(max_length=3, default=get_system_country)

This will generate only one migration even if you change the COUNTRY in settings.
