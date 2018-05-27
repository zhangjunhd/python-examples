## 单例模式
singleton.py

```py
class Singleton(object):
    def __new__(cls, *args, **kw):
        if not hasattr(cls, '_instance'):
            orig = super(Singleton, cls)
            cls._instance = orig.__new__(cls, *args, **kw)
        return cls._instance


class MyClass(Singleton):
    a = 1

one = MyClass()
two = MyClass()
two.a = 3
print one.a  # 3


# Although the problem with this pattern is subclassing;
# all instances will be instances of MyClass no matter what the method resolution order (__mro__) says:
class MyOtherClass(MyClass):
    b = 2

three = MyOtherClass()
print three.b  # AttributeError: 'MyClass' object has no attribute 'b'
```

上述方法的限制是不可再继承得到新的子类，为了避免这种现在，提出一种基于共享状态的替代实现，即Brog。brog.py

```py
class Borg(object):
    _state = {}

    def __new__(cls, *args, **kw):
        ob = super(Borg, cls).__new__(cls, *args, **kw)
        ob.__dict__ = cls._state
        return ob


class MyClass(Borg):
    a = 1

one = MyClass()
two = MyClass()
two.a = 3
print one.a  # 3


class MyOtherClass(MyClass):
    b = 2

three = MyOtherClass()
print three.b  # 2
print three.a  # 3
three.a = 2
print one.a  # 2
```

不推荐单例的类有多级的继承。Python模块也是一个单例，可以考虑使用带有函数的模块来代替。

## 外观模式
一般不需要类来提供外观模式，\_\_init\_\_.py模块中的简单函数就足够了。我们在导入一个包时，实际上是导入了它的\_\_init\_\_.py文件。这样我们可以在\_\_init\_\_.py文件中批量导入我们所需要的模块，而不再需要一个一个的导入。

```py
# package
# __init__.py
import re
import urllib
import sys
import os

# a.py
import package 
print(package.re, package.urllib, package.sys, package.os)
```

注意这里访问__init__.py文件中的引用文件，需要加上包名。

\_\_init\_\_.py中还有一个重要的变量，\_\_all\_\_, 它用来将模块全部导入。

```py
# __init__.py
__all__ = ['os', 'sys', 're', 'urllib']

# a.py
from package import *
```

这时就会把注册在\_\_init\_\_.py文件中\_\_all\_\_列表中的模块和包导入到当前文件中来。

## 观察者
observer.py

```py
class Event(object):
    _observers = []

    def __init__(self, subject):
        self.subject = subject

    @classmethod
    def register(cls, observer):
        if observer not in cls._observers:
            cls._observers.append(observer)

    @classmethod
    def unregister(cls, observer):
        if observer in cls._observers:
            cls._observers.remove(observer)

    @classmethod
    def notify(cls, subject):
        event = cls(subject)
        for observer in cls._observers:
            observer(event)


class WriteEvent(Event):
    def __repr__(self):
        return 'WriteEvent'


def log(event):
    print '%s was written' % event.subject

WriteEvent.register(log)


class AnotherObserver(object):
    def __call__(self, event):
        print 'Yeah %s told me !' % event

WriteEvent.register(AnotherObserver())
WriteEvent.notify('a given file')
# a given file was written
# Yeah WriteEvent told me !
```

## 访问者
visitor.py

```py
class Vlist(list):
    def accept(self, visitor):
        visitor.visit_list(self)


class Vdict(dict):
    def accept(self, visitor):
        visitor.visit_dict(self)


class Printer(object):
    def visit_list(self, ob):
        print 'list content :'
        print str(ob)

    def visit_dict(self, ob):
        print 'dict keys: %s' % ','.join(ob.keys())

a_list = Vlist([1, 2, 5])
a_list.accept(Printer())
# list content :
# [1, 2, 5]

a_dict = Vdict({'one': 1, 'two': 2, 'three': 3})
a_dict.accept(Printer())
# dict keys: one,three,two


# Since Python allows code introspection,
# a better idea is to automatically link visitors and visited class:
def visit(visited, visitor):
    cls = visited.__class__.__name__
    meth = 'visit_%s' % cls
    method = getattr(visitor, meth, None)
    if method is not None:
        method(visited)

visit([1, 2, 5], Printer())
# list content :
# [1, 2, 5]

visit({'one': 1, 'two': 2, 'three': 3}, Printer())
# dict keys: one,three,two
```

## 模板
indexer.py

```py
from itertools import groupby

class Indexer(object):
    def process(self, text):
        text = self._normalize_text(text)
        words = self._split_text(text)
        words = self._remove_stop_words(words)
        stemmed_words = self._stem_words(words)
        return self._frequency(stemmed_words)

    def _normalize_text(self, text):
        return text.lower().strip()

    def _split_text(self, text):
        return text.split()

    def _remove_stop_words(self, words):
        raise NotImplementedError

    def _stem_words(self, words):
        raise NotImplementedError

    def _frequency(self, words):
        counts = {}
        for word in words:
            counts[word] = counts.get(word, 0) + 1
        return counts


class BasicIndexer(Indexer):
    _stop_words = ('he', 'she', 'is', 'and', 'or')

    def _remove_stop_words(self, words):
        return (word for word in words if word not in self._stop_words)

    def _stem_words(self, words):
        return ((len(word) > 2 and word.rstrip('aeiouy') or word) for word in words)

indexer = BasicIndexer()
print indexer.process('My Tailor is rich and he is also my friend')
# {'friend': 1, 'tailor': 1, 'my': 2, 'als': 1, 'rich': 1}
```
