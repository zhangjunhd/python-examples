# 使用chardet检测字符编码
使用chardet.detect检测字符编码:

    >>> import urllib
    >>> rawdata = urllib.urlopen('http://www.baidu.com').read()
    >>> import chardet
    >>> chardet.detect(rawdata)
    {'confidence': 0.99, 'encoding': 'GB2312'}

对于大量文本，chardet提供了可以渐进检测字符编码的相关方法，并且在满足可信度后自动停止检测。具体通过UniversalDetector类的feed方法不断检测每个文本块,当达到最小可信度阈值时,就标记UniversalDetector类的done值为True,表示完成检测。已经读取完待检测文本之后若调用UniversalDetector类的close方法，它会在没有达到最小可信度的情况下再做一些计算，以便返回最大程度上准确的值，该值和chardet.detect函数一样的返回字典结构。
```python
import urllib
from chardet.universaldetector import UniversalDetector

usock = urllib.urlopen('http://www.baidu.com')
detector = UniversalDetector()
for line in usock.readlines():
    detector.feed(line)
    if detector.done:
        break
detector.close()
usock.close()
print detector.result
```

 终端输出：

    {'confidence': 0.98999999999999999, 'encoding': 'GB2312'}

如果想对多个独立文本进行编码检测，可以重复使用同一个UniversalDetector对象，只要在每个文本开始检测前调用reset()，以表明接下来读取的是与之前文本相独立的字符串，然后就可以在类似于上述例子中调用feed()，最后调用close()结束。
```python
import glob
from chardet.universaldetector import UniversalDetector

detector = UniversalDetector()
for filename in glob.glob('*.py'):
    print filename.ljust(60),
    detector.reset()
    for line in file(filename, 'rb'):
        detector.feed(line)
        if detector.done:
            break
    detector.close()
    print detector.result
```

终端输出：

    try.py      {'confidence': 1.0, 'encoding': 'ascii'}
    try2.py    {'confidence': 1.0, 'encoding': 'ascii'}
