由于新式的字符串Template 对象的引进使得string 模块又重新活了过来，Template 对象有两个方法,substitute()和safe_substitute().前者更为严谨,在key 缺少的情况下它会报一个KeyError 的异常出来，而后者在缺少key 时，直接原封不动的把字符串显示出来.

    >>> from string import Template
    >>> s = Template('There are ${howmany} ${lang} Quotation Symbols')
    >>>
    >>> print s.substitute(lang='Python', howmany=3)
    There are 3 Python Quotation Symbols
    >>>
    >>> print s.substitute(lang='Python')
    Traceback (most recent call last):
    File "<stdin>", line 1, in ?
    File "/usr/local/lib/python2.4/string.py", line 172, in substitute
    return self.pattern.sub(convert, self.template)
    File "/usr/local/lib/python2.4/string.py", line 162, in convert val =
    mapping[named]
    KeyError: 'howmany'
    >>>
    >>> print s.safe_substitute(lang='Python')
    There are ${howmany} Python Quotation Symbols

也可以使用一个字典来作为替代key的数据结构传入

```python
from string import Template

s = Template('There are ${howmany} ${lang} Quotation Symbols')
repDict = {'lang' : 'Python', 'howmany' : 3}
print s.substitute(repDict)  #There are 3 Python Quotation Symbols
```
