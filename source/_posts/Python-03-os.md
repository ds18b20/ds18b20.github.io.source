---
title: Python-03-os
date: 2017-11-12 23:31:34
categories: Python
tags:
- Python
- os
---

# os Lib

## os.path.dirname()

Return the path of present Py script file. (without filename)

- This command must be used in a real Py script file. If under command line , you will get an error of  `NameError: name '__file__' is not defined`
- If you type in  `python Script.py` under command line, nothing will be returned.
- If you type in  `python C:\Users\...\Script.py` under command line, present file path will be returned.

## os.path.abspath()

Return the absolute path of present Py script file. (with filename)

- No matter you type in *absolute path* or *relative path*, you will get present file path with filename.

  ```
  os.path.abspath(__file__)
  ```

- If you do not need the file name, you can combine with os.path.dirname()

  ```python
  os.path.dirname(os.path.abspath(__file__))
  ```

