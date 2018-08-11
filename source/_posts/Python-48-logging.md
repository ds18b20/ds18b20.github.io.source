---
title: Python-48-logging
date: 2018-08-11 09:25:51
categories: Python
tags:
- logging

---

# Python logging

在调试过程中，除了使用print函数打印到终端之外，还可以考虑使用自带的logging模块。
相比于直接print到终端，

- logging可以更方便的指定日志等级，比如在release版本中只输出重要信息，而屏蔽大量的调试信息；
- 可以指定输出到格式以及输出位置。

# 基本使用

```python
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

logger.debug('Logging debug...')
logger.info('Logging info...')
logger.debug('Logging debug...')
logger.warning('Logging warning...')
logger.error('Logging error...')
logger.critical('Logging critical...')
```

输出：

```commandline
INFO:__main__:Logging info...
WARNING:__main__:Logging warning...
ERROR:__main__:Logging error...
CRITICAL:__main__:Logging critical...
```

如果`logging.basicConfig(level=logging.INFO)`表示只输出INFO及其以上的日志；
level参数改成`level=logging.NOTSET`表示输出所有；
若去掉level参数，默认只输出等于或高于WARNING等级的日志。

# 日志格式化

`logger.info('%s opens port %s', 'app', '8000')`
输出为：`INFO:__main__:app opens port 8000`

# 加入调试信息

除了level，在logging.basicConfig()中还可以设置日志文件保存位置/文件打开方式，和输出格式。

## format常用格式：

%(levelno)s: 打印日志级别的数值
%(levelname)s: 打印日志级别名称
%(pathname)s: 打印当前执行程序的路径，其实就是sys.argv[0]
%(filename)s: 打印当前执行程序名
%(funcName)s: 打印日志的当前函数
%(lineno)d: 打印日志的当前行号
%(asctime)s: 打印日志的时间
%(thread)d: 打印线程ID
%(threadName)s: 打印线程名称
%(process)d: 打印进程ID
%(message)s: 打印日志信息
Example:

```python
import logging

logging.basicConfig(level=logging.NOTSET,
                    format='%(asctime)s - %(filename)s[line:%(lineno)d] - %(levelname)s: %(message)s')
logger = logging.getLogger(__name__)

logger.info('Logging info...')
logger.debug('Logging debug...')
logger.warning('Logging warning...')
logger.error('Logging error...')
logger.critical('Logging critical...')

logger.info('%s opens port %s', 'app', '8000')
```

得到：

```commandline
2018-07-29 15:09:39,614 - test.py[line:7] - INFO: Logging info...
2018-07-29 15:09:39,614 - test.py[line:8] - DEBUG: Logging debug...
2018-07-29 15:09:39,614 - test.py[line:9] - WARNING: Logging warning...
2018-07-29 15:09:39,614 - test.py[line:10] - ERROR: Logging error...
2018-07-29 15:09:39,614 - test.py[line:11] - CRITICAL: Logging critical...
2018-07-29 15:09:39,614 - test.py[line:13] - INFO: app opens port 8000
```

