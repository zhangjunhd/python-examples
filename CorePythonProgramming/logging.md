# logging
python的logging主要有四个组件：
* logger: 日志类，应用程序往往通过调用它提供的api来记录日志；
* handler: 对日志信息处理，可以将日志发送(保存)到不同的目标域中；
* filter: 对日志信息进行过滤；
* formatter:日志的格式化；

logging是线程安全的。

## 通过logging.basicConfig函数对日志的输出格式及方式做相关配置
```py
# -*- coding: utf-8 -*-
import logging

logging.basicConfig(level=logging.DEBUG,
                    format='%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s',
                    datefmt='%a, %d %b %Y %H:%M:%S',
                    filename='test.log',
                    filemode='w')

logging.debug('This is debug message')
logging.info('This is info message')
logging.warning('This is warning message')
```

当前目录下test.log内容：
```
Tue, 07 Feb 2017 19:51:36 python-logging.py[line:10] DEBUG This is debug message
Tue, 07 Feb 2017 19:51:36 python-logging.py[line:11] INFO This is info message
Tue, 07 Feb 2017 19:51:36 python-logging.py[line:12] WARNING This is warning message
```

logging.basicConfig函数各参数:
- filename: 指定日志文件名
- filemode: 和file函数意义相同，指定日志文件的打开模式，'w'或'a'
- format: 指定输出的格式和内容，format可以输出很多有用信息，如上例所示:
  - %(levelno)s: 打印日志级别的数值
  - %(levelname)s: 打印日志级别名称
  - %(pathname)s: 打印当前执行程序的路径，其实就是sys.argv[0]
  - %(filename)s: 打印当前执行程序名
  - %(funcName)s: 打印日志的当前函数
  - %(lineno)d: 打印日志的当前行号
  - %(asctime)s: 打印日志的时间
  - %(thread)d: 打印线程ID
  - %(threadName)s: 打印线程名称
  - %(process)d: 打印进程ID
  - %(message)s: 打印日志信息
- datefmt: 指定时间格式，同time.strftime()
- level: 设置日志级别，默认为logging.WARNING
- stream: 指定将日志的输出流，可以指定输出到sys.stderr,sys.stdout或者文件，默认输出到sys.stderr，当stream和filename同时指定时，stream被忽略

## 将日志同时输出到文件和屏幕
```py
# -*- coding: utf-8 -*-
import logging

logging.basicConfig(level=logging.DEBUG,
                    format='%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s',
                    datefmt='%a, %d %b %Y %H:%M:%S',
                    filename='test.log',
                    filemode='w')

#################################################################################################
# 定义一个StreamHandler，将INFO级别或更高的日志信息打印到标准错误，并将其添加到当前的日志处理对象
console = logging.StreamHandler()
console.setLevel(logging.INFO)
formatter = logging.Formatter('%(name)-12s: %(levelname)-8s %(message)s')
console.setFormatter(formatter)
logging.getLogger('').addHandler(console)
#################################################################################################

logging.debug('This is debug message')
logging.info('This is info message')
logging.warning('This is warning message')
```
当前目录下test.log内容：
```
Tue, 07 Feb 2017 19:56:23 python-logging.py[line:19] DEBUG This is debug message
Tue, 07 Feb 2017 19:56:23 python-logging.py[line:20] INFO This is info message
Tue, 07 Feb 2017 19:56:23 python-logging.py[line:21] WARNING This is warning message
```

控制台输出：
```
root        : INFO     This is info message
root        : WARNING  This is warning message
```

## 日志回滚
```py
# -*- coding: utf-8 -*-
import logging
from logging.handlers import RotatingFileHandler

#################################################################################################
# 定义一个RotatingFileHandler，最多备份5个日志文件，每个日志文件最大10M
r_handler = RotatingFileHandler('test.log', maxBytes=10*1024*1024, backupCount=5)
r_handler.setLevel(logging.INFO)
formatter = logging.Formatter('%(name)-12s: %(levelname)-8s %(message)s')
r_handler.setFormatter(formatter)
logging.getLogger('').addHandler(r_handler)
```

## 通过logging.config模块配置日志
```
#logger.conf
###############################################
[loggers]
keys=root,example01,example02

[logger_root]
level=DEBUG
handlers=hand01,hand02

[logger_example01]
handlers=hand01,hand02
qualname=example01
propagate=0

[logger_example02]
handlers=hand01,hand03
qualname=example02
propagate=0
###############################################
[handlers]
keys=hand01,hand02,hand03

[handler_hand01]
class=StreamHandler
level=INFO
formatter=form02
args=(sys.stderr,)

[handler_hand02]
class=FileHandler
level=DEBUG
formatter=form01
args=('test.log', 'a')

[handler_hand03]
class=handlers.RotatingFileHandler
level=INFO
formatter=form02
args=('test.log', 'a', 10*1024*1024, 5)
###############################################
[formatters]
keys=form01,form02

[formatter_form01]
format=%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s
datefmt=%a, %d %b %Y %H:%M:%S

[formatter_form02]
format=%(name)-12s: %(levelname)-8s %(message)s
datefmt=%a, %d %b %Y %H:%M:%S
```

将日志同时输出到文件和屏幕的例子：
```py
# -*- coding: utf-8 -*-

import logging.config

logging.config.fileConfig("logger.conf")
logger = logging.getLogger("example01")

logger.debug('This is debug message')
logger.info('This is info message')
logger.warning('This is warning message')
```

日志回滚的例子：
```py
# -*- coding: utf-8 -*-

import logging.config

logging.config.fileConfig("logger.conf")
logger = logging.getLogger("example02")

logger.debug('This is debug message')
logger.info('This is info message')
logger.warning('This is warning message')
```

## 项目中配置日志模板
项目根目录logger.json

```json
{
  "version": 1,
  "disable_existing_loggers": false,
  "formatters": {
    "standard": {
      "format": "[%(asctime)s] [%(levelname)s]  [%(module)s:%(lineno)d]  [%(thread)d] %(message)s"
    }
  },
  "handlers": {
    "default": {
      "level": "INFO",
      "formatter": "standard",
      "class": "logging.handlers.TimedRotatingFileHandler",
      "filename": "./server.log",
      "when": "midnight",
      "interval": 1
    },
    "streamerror": {
      "formatter": "standard",
      "class": "logging.StreamHandler",
      "level": "INFO",
      "stream": "ext://sys.stdout"
    }
  },
  "loggers": {
    "": {
      "handlers": [
        "default", "streamerror"
      ],
      "level": "DEBUG",
      "propagate": true
    }
  }
}
```

项目根目录的 `__init__.py`

```py
#!/bin/env python

import os
import json
import logging
import logging.config
from . import definition


def init():
    with open(os.path.join(definition.ROOT_DIR, 'logger.json')) as f:
        logging.config.dictConfig(json.loads(f.read()))


init()

```

项目根目录的 `definition.py`

```py
#!/bin/env python
# -*- coding: utf-8 -*-

import os

ROOT_DIR = os.path.dirname(os.path.abspath(__file__))  # Project Root Directory
STAT_DIR = os.path.join(ROOT_DIR, 'statistics')
DB_DIR = os.path.join(ROOT_DIR, 'db')

```

项目中使用日志模块：

```py
import logging

logger = logging.getLogger(__name__)

 logger.info('xxx')
```