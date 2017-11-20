### time.time()
直接使用time.time()获取当前时间戳，它提供浮点型数据，易于存储与做日期间的比较。

```python
import time

print 'The time is:', time.time() # The time is: 1309664371.7
```

### time.ctime()
为了获取更易阅读，可使用time.ctime。

```python
# -*- coding: utf-8 -*-
import time

print 'The time is:', time.ctime() # The time is: Sun Jul  3 11:42:19 2011
later = time.time() + 15
print '15 secs from now:', time.ctime(later) # 15 secs from now: Sun Jul  3 11:42:34 2011
```

### time.clock()
time()函数返回的是现实世界的时间，而clock()函数返回的是cpu的时钟。clock()函数返回值常用作性能测试，benchmarking等。它们反映的是程序运行的真实时间，比time()函数返回的值要精确。

```python
# -*- coding: utf-8 -*-
import hashlib
import time

#计算md5校验和的数据
data = open(__file__, 'rt').read()

for i in range(5):
    h = hashlib.sha1()
    print time.ctime(), ': %0.3f %0.3f' % (time.time(), time.clock())
    for i in range(100000):
        h.update(data)
    cksum = h.digest()
```

输出：

    Sun Jul  3 11:47:26 2011 : 1309664846.171 0.010
    Sun Jul  3 11:47:26 2011 : 1309664846.399 0.240
    Sun Jul  3 11:47:26 2011 : 1309664846.615 0.460
    Sun Jul  3 11:47:26 2011 : 1309664846.828 0.670
    Sun Jul  3 11:47:27 2011 : 1309664847.041 0.880

### struct_time
有时须要获取日期的单独部分，time模块定义了struct_time来存储日期和时间值作为它的一部分以便获取。并提供了多种函数将struct_time转换为float。

```python
# -*- coding: utf-8 -*-
import time

print 'gmtime   :', time.gmtime() #gmtime   : time.struct_time(tm_year=2011, tm_mon=7, tm_mday=3, tm_hour=3, tm_min=54, tm_sec=18, tm_wday=6, tm_yday=184, tm_isdst=0)
print 'localtime:', time.localtime() #ocaltime: time.struct_time(tm_year=2011, tm_mon=7, tm_mday=3, tm_hour=11, tm_min=54, tm_sec=18, tm_wday=6, tm_yday=184, tm_isdst=0)
print 'mktime   :', time.mktime(time.localtime()) #mktime   : 1309665258.0

t = time.localtime()
print 'Day of month:', t.tm_mday #Day of month: 3
print 'Day of week :', t.tm_wday #Day of week : 6
print 'Day of year :', t.tm_yday #Day of year : 184
```

### 解析与格式化时间
函数strptime()和strftime()可以使struct_time和时间值字符串相互转化。

```python
# -*- coding: utf-8 -*-
import time

now = time.ctime()
print now #Sun Jul  3 11:57:47 2011
parsed = time.strptime(now)
print parsed #time.struct_time(tm_year=2011, tm_mon=7, tm_mday=3, tm_hour=11, tm_min=57, tm_sec=47, tm_wday=6, tm_yday=184, tm_isdst=-1)
print time.strftime("%a %b %d %H:%M:%S %Y", parsed) #Sun Jul 03 11:57:47 2011
```

### 使用Time Zone
无论是你的程序，还是为系统使用一个默认的时区，检测当前时间的函数依赖于当前时区的设置。改变时区设置不会改变实际时间，只会改变表示时间的方法。通过设置环境变量TZ可以改变时区，然后调用tzset()。环境变量TZ可以对时区作详细的设置，比如白天保存时间的起始点。下面这个示例改变了time zone中的一些值，展示了这种改变如何影响time模块中的其他设置。

```python
# -*- coding: utf-8 -*-
import time
import os

def show_zone_info():
    print '\tTZ    :', os.environ.get('TZ', '(not set)')
    print '\ttzname:', time.tzname
    print '\tZone  :, %d (%d)' % (time.timezone, (time.timezone / 3600))
    print '\tDST   :', time.daylight
    print '\tTime  :', time.ctime()
    print

print 'Default :'
show_zone_info()

for zone in ['US/Eastern', 'US/Pacific', 'GMT']:
    os.environ['TZ'] = zone
    time.tzset()
    print zone, ':'
    show_zone_info()
```

输出：

    Default :
     TZ    : (not set)
     tzname: ('CST', 'CST')
     Zone  :, -28800 (-8)
     DST   : 0
     Time  : Sun Jul  3 12:10:28 2011

    US/Eastern :
     TZ    : US/Eastern
     tzname: ('EST', 'EDT')
     Zone  :, 18000 (5)
     DST   : 1
     Time  : Sun Jul  3 00:10:28 2011

    US/Pacific :
     TZ    : US/Pacific
     tzname: ('PST', 'PDT')
     Zone  :, 28800 (8)
     DST   : 1
     Time  : Sat Jul  2 21:10:28 2011

    GMT :
     TZ    : GMT
     tzname: ('GMT', 'GMT')
     Zone  :, 0 (0)
     DST   : 0
     Time  : Sun Jul  3 04:10:28 2011
