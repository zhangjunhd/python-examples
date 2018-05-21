# 元类编程
元类让你来定义某些类是如何被创建的，从根本上说，赋予你如何创建类的控制权。示例1，它在用元类创建一个类时，显示时间标签。

```python
#!/usr/bin/env python
from time import ctime

class MetaC(type):
    def __init__(cls, name, bases, attrd):
        super(MetaC, cls).__init__(name, bases, attrd)
        print '*** Created class %r at: %s' % (name, ctime())

class Foo(object):
    __metaclass__ = MetaC

    def __init__(self):
        print '*** Instantiated class %r at: %s' % (self.__class__.__name__, ctime())

f = Foo()
```

输出：

    *** Created class 'Foo' at: Wed Feb  9 11:50:37 2011
    *** Instantiated class 'Foo' at: Wed Feb  9 11:50:37 2011

示例2，将创建一个元类，要求程序员在他们写的类中提供一个__str__()方法的实现。

```python
#!/usr/bin/env python
from warnings import warn

class ReqStrSugRepr(type):
    def __init__(cls, name, bases, attrd):
        super(ReqStrSugRepr, cls).__init__(name, bases, attrd)

        if '__str__' not in attrd:
            raise TypeError("Class requires overriding of __str__()")

        if '__repr__' not in attrd:
            warn('Class suggests overriding of __repr__()\n', stacklevel=3)

class Foo(object):
    __metaclass__ = ReqStrSugRepr

    def __str__(self):
        return 'Instance of class:', self.__class__.__name__

    def __repr__(self):
        return self.__class__.__name__

class Bar(object):
    __metaclass__ = ReqStrSugRepr

    def __str__(self):
        return 'Instance of class:', self.__class__.__name__

class FooBar(object):
    __metaclass__ = ReqStrSugRepr
```

输出：

    sys:1: UserWarning: Class suggests overriding of __repr__()

    Traceback (most recent call last):
      File "/home/zhangjun/workspace/try/src/try.py", line 29, in <module>
        class FooBar(object):
      File "/home/zhangjun/workspace/try/src/try.py", line 9, in __init__
        raise TypeError("Class requires overriding of __str__()")
    TypeError: Class requires overriding of __str__()

简单说明一下，定义元类ReqStrSugRepr，如果没有`__str__()`方法的实现，则会抛出一个异常，而如果没有`__repr__()`方法的实现，则会打印出一个UserWarning。Foo 定义成功的；定义Bar 时，提示警告`__repr__()`未实现；FooBar 的创建没有通过安全检查，以致程序最后没有打印出关于FooBar 的Traceback。
