---
title: web-flask-logging
date: 2018-12-17 22:34:01
categories: web
tags:
- flask
- logging

---

# flask logging

By default the level for logging is warning. So you won't see a logging message of level `DEBUG`. To fix this just enable debug logging with the `basicConfig()` function of the logging module:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

# Reference

https://stackoverflow.com/questions/44405708/flask-doesnt-print-to-console

