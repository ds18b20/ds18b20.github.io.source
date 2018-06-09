---
title: Python-37-RenameFiles
date: 2018-05-06 13:25:10
categories: Python
tags:
- rename files

---

# rename file

these code will rename all .jpg file to img-No.

```python
import glob, os

def rename(dir, pattern, titlePattern):
    for i, pathAndFilename in enumerate(glob.iglob(os.path.join(dir, pattern))):
        title, ext = os.path.splitext(os.path.basename(pathAndFilename))
        os.rename(pathAndFilename, os.path.join(dir, titlePattern % i + ext))
                  
# rename(r'', r'*.jpg', r'img-%d')
rename(r'', r'*.png', r'img-%d')
```

# reference

Batch Renaming of Files in a Directory

https://stackoverflow.com/questions/225735/batch-renaming-of-files-in-a-directory