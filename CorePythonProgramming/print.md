# 格式化输出
Python的print语句，与字符串操作符(%)组合使用，可实现字符串替换功能，类似于C语言的printf()函数。

    >>>"%s is number %d" % ('Python', 1)
    Python is number 1
    >>> "%s price is %f" % ('Core Python Programming', 31.25)
    Core Python Programming price is 31.250000

## 字符串格式化符号

* `%c` 转换成字符(ASCII 码值，或者长度为一的字符串)
* `%r` 优先用repr()函数进行字符串转换
* `%s` 优先用str()函数进行字符串转换
* `%d / %i` 转成有符号十进制数
* `%u` 转成无符号十进制数
* `%o` 转成无符号八进制数
* `%x/%X` (Unsigned)转成无符号十六进制数(x/X 代表转换后的十六进制字符的大小写)
* `%e/%E` 转成科学计数法(e/E 控制输出e/E)
* `%f/%F` 转成浮点数(小数部分自然截断)
* `%g/%G` %e 和%f/%E 和%F 的简写
* `%%` 输出%

## 格式化操作符辅助指令

* `*` 定义宽度或者小数点精度
* `-` 用做左对齐
* `+` 在正数前面显示加号( + )
* `<sp>` 在正数前面显示空格
* `#` 在八进制数前面显示零('0')，在十六进制前面显示'0x'或者'0X'(取决于用的是'x'还是'X')
* `0` 显示的数字前面填充‘0’而不是默认的空格
* `(var)` 映射变量(字典参数)
* `m.n` m 是显示的最小总宽度,n 是小数点后的位数(如果可用的话)

十六进制输出:

    >>> "%x" % 108
    '6c'
    >>> "%X" % 108
    '6C'
    >>> "%#X" % 108
    '0X6C'
    >>> "%#x" % 108
    '0x6c'

浮点数和科学记数法形式输出:

    >>> '%f' % 1234.567890
    '1234.567890'
    >>>
    >>> '%.2f' % 1234.567890
    '1234.57'
    >>>
    >>> '%E' % 1234.567890
    '1.234568E+03'
    >>>
    >>> '%e' % 1234.567890
    '1.234568e+03'
    >>>
    >>> '%g' % 1234.567890
    '1234.57'
    >>>
    >>> '%G' % 1234.567890
    '1234.57'
    >>>
    >>> "%e" % (1111111111111111111111L)
    '1.111111e+21'

整数和字符串输出:

    >>> "%+d" % 4
    '+4'
    >>>
    >>> "%+d" % -4
    '-4'
    >>>
    >>> "we are at %d%%" % 100
    'we are at 100%'
    >>>
    >>> 'Your host is: %s' % 'earth'
    'Your host is: earth'
    >>>
    >>> 'Host: %s\tPort: %d' % ('mars', 80)
    'Host: mars Port: 80'
    >>>
    >>> num = 123
    >>> 'dec: %d/oct: %#o/hex: %#X' % (num, num, num)
    'dec: 123/oct: 0173/hex: 0X7B'
    >>>
    >>> "MM/DD/YY = %02d/%02d/%d" % (2, 15, 67)
    'MM/DD/YY = 02/15/67'
    >>>
    >>> w, p = 'Web', 'page'
    >>> 'http://xxx.yyy.zzz/%s/%s.html' % (w, p)
    'http://xxx.yyy.zzz/Web/page.html'

上面的例子都是使用的元组类型的参数作转换.下面我们将把字典类型的参数提供给格式化操作符.

    >>> 'There are %(howmany)d %(lang)s Quotation Symbols' % \
    ...     {'lang': 'Python', 'howmany': 3}
    'There are 3 Python Quotation Symbols'

print语句默认会给每一行添加一个换行符。

```python
for eacnNum in range(3):
    print eacnNum
```

输出：

    0
    1
    2

只要在print语句的最后添加一个逗号(,)，就可以改变它这种行为。但带逗号的print语句输出的元素之间会自动添加一个空格。

```python
for eacnNum in range(3):
    print eacnNum,
```

输出：

    0 1 2

在按行读文件时，尤其需要注意这一点。下面使用了逗号来抑制自动生成换行符，因为按行读文件本身也会自动添加换行符。如果不添加逗号，会出现读出来的每行内容中间都会空一行的情况。

```python
with open('test_main.py', 'r') as f:
    for line in f:
        print line,
```
