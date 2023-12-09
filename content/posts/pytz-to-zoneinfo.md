+++
title = "Migrating from pytz to ZoneInfo (python 3.9+)"
description = "Migrating from pytz to ZoneInfo (python 3.9+)"
tags = [
    "Django",
    "python",
    "pytz",
    "ZoneInfo"
]
date = "2023-12-09"
categories = [
    "Development", 
    "Django",
]
+++ 

Django 5.0 removed pytz in favor of python 3.9+ supported ZoneInfo.

Adding tzinfo to naked datetime using pytz:

        pytz.timezone("Europe/Helsinki").localize(non_tz_datetime)

becomes

        non_tz_datetime.replace(tzinfo=ZoneInfo("Europe/Helsinki"))

Converting to timezone using pytz:

        timestamp.astimezone(pytz.timezone("Europe/Helsinki"))

becomes

        timestamp.astimezone(ZoneInfo("Europe/Helsinki"))


Note that ZoneInfo is not available before python 3.9 so to support 3.8 (we have couple of old servers) need to import ZoneInfo with

        try:
            import zoneinfo  # noqa
        except ImportError:
            from backports import zoneinfo  # type: ignore  # noqa
        from zoneinfo import ZoneInfo

and in requirements.txt install it with

        backports.zoneinfo;python_version<"3.9"

;python_version<"3.9" skips installing backports.zoneinfo that line with when python >= 3.9

