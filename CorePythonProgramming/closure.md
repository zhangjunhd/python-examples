# 闭包编程
## 1.闭包(closure)定义
如果在一个内部函数里，对在外部作用域(但不是在全局作用域）的变量进行引用，那么内部函数就被认为是`closure`。定义在外部函数内的但由内部函数引用或者使用的变量被称为`自由变量`：
```python
def counter(start_at=0):
        count = [start_at]
        def incr():
            count[0] += 1
            return count[0]
        return incr
```

counter()做的唯一一件事就是接受一个初始化的的值来开始计数，并将该值赋给列表count 唯一一个成员。然后定义一个incr()的内部函数。通过在内部使用变量count，我们创建了一个闭包因为它现在携带了整个counter()作用域。incr()增加了正在运行的count 然后返回它。然后最后的魔法就是counter()返回一个incr，一个（可调用的）函数对象。调用：

    >>> count = counter(5)
    >>> print count()
    6
    >>> print count()
    7
    >>> count2 = counter(100)
    >>> print count2()
    101
    >>> print count()
    8
## 2.闭包和装饰器的例子
下面这个例子演示了带参数的装饰器，该参数最终决定哪一个闭包会被用的。这也是闭包的威力的特征。
```python
#!/usr/bin/env python

from time import time

def logged(when):
    def log(f, *args, **kargs):
        print '''''Called:
function: %s
args: %r
kargs: %r''' % (f, args, kargs)

    def pre_logged(f):
        def wrapper(*args, **kargs):
            log(f, *args, **kargs)
            return f(*args, **kargs)
        return wrapper

    def post_logged(f):
        def wrapper(*args, **kargs):
            now = time()
            try:
                return f(*args, **kargs)
            finally:
                log(f, *args, **kargs)
                print "time delta: %s" % (time()-now)
        return wrapper

    try:
        return {"pre": pre_logged, "post": post_logged}[when]
    except KeyError, e:
        raise ValueError(e), 'must be "pre" or "post"'

@logged("pre")
def hello(name):
    print "Hello,", name

@logged("post")
def hello2(name):
    print "Hello2,", name

hello("World!")
print '*' * 30
hello2("World!")
```

输出：

    Called:
    function: <function hello at 0xb733da04>
    args: ('World!',)
    kargs: {}
    Hello, World!
    ******************************
    Hello2, World!
    Called:
    function: <function hello2 at 0xb733dca4>
    args: ('World!',)
    kargs: {}
    time delta: 2.00271606445e-05

logged()函数其职责就是获得关于何时函数调用应该被写入日志的用户请求。logged()有3 个在它的定义体之内的助手内部函数：log(),pre_logged()以及post_logged()。log()是实际上做日志写入的函数。它仅仅是显示标准输出函数的名字和参数。pre_logged()和post_logged()都会包装目标函数然后根据它的名字写入日志，比如，当目标函数已经执行之后，post_loggeed()会将函数调用写入日志，而pre_logged()则是在执行之前。

两个*logged()函数都包括了一个名为wrapper()的闭包。当合适将其写入日志的时候，它便会调用目标函数。这个函数返回了包裹好的函数对象，该对象随后将被重新赋值给原始的目标函数标识符。

装饰器用特殊的装饰将原始函数对象进行了包裹并返回这个包裹后的hello()和hello2()版本。
