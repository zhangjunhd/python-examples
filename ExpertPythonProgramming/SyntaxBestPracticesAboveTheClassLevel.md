# 语法最佳实践——类级
## 3.1 子类化内建函数
distinctdict.py:python中dict的子类。这个类与dict不同的是它不允许为一个key多次赋值。

```py
class DistinctError(Exception):
    pass


class DistinctDict(dict):
    def __setitem__(self, key, value):
        try:
            value_index = self.values().index(value)
            # keys() and values() will return
            # corresponding lists
            # as long as the dict is not changed
            # between the two calls
            # otherwise the dict type does not guaranteen the ordering.
            existing_key = self.keys()[value_index]
            if existing_key != key:
                raise DistinctError("This value already exists for '%s'" % str(self[existing_key]))
        except ValueError:
            pass
        super(DistinctDict, self).__setitem__(key, value)
```

## 3.2 访问超类中的方法
super是一个内建类型，用来访问某个对象的超类中的特性（attribute）。my_super.py

```py
class Mama(object):
    def says(self):
        print 'do your homework'


class Sister(Mama):
    def says(self):
        super(Sister, self).says()
        print 'and clean your bedroom'
```

Method Resolution Order (MRO): my_mro.py

- L[MyClass(Base1,Base2)]=MyClass+merge(L[Base1],L[Base2],Base1,Base2)
- L[MyClass]是MyClass类的线性化，而merge是合并多个线性化结果的具体算法。
- C的线性化是C加上父类的线性化和父类列表的合并的总和。
- merge算法负责删除重复项并保持正确的顺序。
- 类的__mro__特性（只读）用来存储线性化计算的结果

```py
class BaseBase(object):
    pass


class Base1(BaseBase):
    pass


class Base2(BaseBase):
    pass


class MyClass(Base1, Base2):
    pass


def L(klass):
    return [k.__name__ for k in klass.__mro__]

print L(MyClass)  # ['MyClass', 'Base1', 'Base2', 'BaseBase', 'object']
```

## 3.4 描述符和属性
描述符是Python中复杂特性访问的基础。它们在内部使用，以实现属性、类、静态方法和super类等。它们是定义另一个类特性可能的访问方式的类。换句话说，一个类可以委托另一个类来管理其特性。

### 描述符类
描述符类基于三个必须实现的特殊方法：

- \_\_set\_\_ 在任何特性被设置的时候调用（setter）
- \_\_get\_\_ 在任何特性被读取时调用（getter）
- \_\_delete\_\_在特性上请求del时调用

这些方法将在\_\_dict\_\_特性之前被调用。一个使用数据描述符的实例：upper_string.py

```py
class UpperString(object):
    def __init__(self):
        self._value = ''

    def __get__(self, instance, klass):
        return self._value

    def __set__(self, instance, value):
        self._value = value.upper()


class MyClass(object):
    attribute = UpperString()

instance_of = MyClass()
instance_of.attribute = 'my value'
print instance_of.attribute  # MY VALUE

# Now if we add a new attribute in the instance, it will be stored in its __dict__ mapping:
instance_of.new_att = 1
print instance_of.__dict__  # {'new_att': 1}

# But if a new data descriptor is added in the class, it will take precedence over the instance __dict__:
MyClass.new_att = 2
print instance_of.__dict__  # {'new_att': 1}

instance_of.new_att = 'other value'
print instance_of.new_att   # other value
print instance_of.__dict__  # {'new_att': 'other value'}

class Whatever(object):
    def __get__(self, instance, klass):
        return 'whatever'

MyClass.whatever = Whatever()
print instance_of.__dict__  # {'new_att': 'other value'}
print instance_of.whatever  # whatever
instance_of.whatever = 1
print instance_of.__dict__  # {'new_att': 'other value', 'whatever': 1}
```

### 内省描述符（introspection descriptor）
这种描述符将检查宿主类签名，以计算一些信息。api.py

```py
class API(object):
    def _print_values(self, obj):
        def _print_value(key):
            if key.startswith('_'):
                return ''
            value = getattr(obj, key)
            if not hasattr(value, 'im_func'):
                doc = type(value).__name__
            else:
                if value.__doc__ is None:
                    doc = 'no docstring'
                else:
                    doc = value.__doc__
            return '    %s : %s' % (key, doc)
        res = [_print_value(el) for el in dir(obj)]
        return '\n'.join([el for el in res if el != ''])

    def __get__(self, instance, klass):
        if instance is not None:
            return self._print_values(instance)
        else:
            return self._print_values(klass)


class MyClass(object):
    __doc__ = API()

    def __init__(self):
        self.a = 2

    def meth(self):
        """my method"""
        return 1

print MyClass.__doc__  # meth : my method
instance = MyClass()
print instance.__doc__
# a : int
# meth : my method
```

### 元描述符（meta descriptor）
这种描述符使用类方法本身来完成值计算。chainer.py

```py
class Chainer(object):
    def __init__(self, methods, callback=None):
        self._methods = methods
        self._callback = callback

    def __get__(self, instance, klass):
        if instance is None:
            # only for instances
            return self
        results = []
        for method in self._methods:
            results.append(method(instance))
            if self._callback is not None:
                if not self._callback(instance, method, results):
                    break
        return results


class TextProcessor(object):
    def __init__(self, text):
        self.text = text

    def normalize(self):
        if isinstance(self.text, list):
            self.text = [t.lower() for t in self.text]
        else:
            self.text = self.text.lower()

    def split(self):
        if not isinstance(self.text, list):
            self.text = self.text.split()

    def treshold(self):
        if not isinstance(self.text, list):
            if len(self.text) < 2:
                self.text = ''
        self.text = [w for w in self.text if len(w) > 2]


def logger(instance, method, results):
    print 'calling %s' % method.__name__
    return True


def add_sequence(name, sequence):
    setattr(TextProcessor, name, Chainer([getattr(TextProcessor, n) for n in sequence], logger))


add_sequence('simple_clean', ('split', 'treshold'))
my = TextProcessor(' My Taylor is Rich ')
my.simple_clean
# calling split
# calling treshold

print my.text  # ['Taylor', 'Rich']

# let's perform another sequence
add_sequence('full_work', ('normalize', 'split', 'treshold'))
my.full_work
# calling normalize
# calling split
# calling treshold

print my.text  # ['taylor', 'rich']
```

### 属性（property）
知道如何将一个特性链接到一组方法上。属性采用fget参数和3个可选的参数——fset，fdel和doc。

my_property.py

```py
class MyClass(object):
    def __init__(self):
        self._my_secret_thing = 1

    def _i_get(self):
        return self._my_secret_thing

    def _i_set(self, value):
        self._my_secret_thing = value

    def _i_delete(self):
        print 'neh!'

    my_thing = property(_i_get, _i_set, _i_delete, 'the thing')


instance_of = MyClass()
print instance_of.my_thing  # 1
instance_of.my_thing = 3
print instance_of.my_thing  # 3
del instance_of.my_thing  # neh!
help(instance_of)
'''
Help on MyClass in module __main__ object:
class MyClass(__builtin__.object)
 |  Methods defined here:
 |
 |  __init__(self)
 |
 |  ----------------------------------------------------------------------
 |  Data descriptors defined here:
 |
 |  __dict__
 |      dictionary for instance variables (if defined)
 |
 |  __weakref__
 |      list of weak references to the object (if defined)
 |
 |  my_thing
 |      the thing
'''
```

overload_property.py

```py
class FirstClass(object):
    def get_price(self):
        return '$ 500'
    price = property(get_price)


class SecondClass(FirstClass):
    def get_price(self):
        return '$ 20'

plane_ticket = SecondClass()
print plane_ticket.get_price()  # $ 20
```

## 3.5 槽
它允许使用\_\_slot\_\_特性为指定类设置一个静态特性列表，并且跳过每个类实例中\_\_dict\_\_列表的创建工作。它们用来为特性很少的类节省存储空间，因为将不在每个实例中创建\_\_dict\_\_。除此之外，它们有助于设计签名必须被冻结的类。frozen.py

```py
class Frozen(object):
    __slots__ = ['ice', 'cream']


print '__dict__' in dir(Frozen)  # False
print 'ice' in dir(Frozen)  # True

glagla = Frozen()
glagla.ice = 1
glagla.cream = 1
glagla.aha = 1  # AttributeError: 'Frozen' object has no attribute 'aha'
```

## 3.6 元编程
### 3.6.1 __new__方法
\_\_init\_\_在子类中不会被隐式调用。所以\_\_new\_\_用来确定已经在整个类层次中完成了初始化工作。my_new.py

```py
class MyClass(object):
    def __new__(cls):
        print '__new__ called'
        return object.__new__(cls)  # default factory

    def __init__(self):
        print '__init__ called'
        self.a = 1

instance = MyClass()
# __new__ called
# __init__ called


class MyOtherClassWithoutAConstructor(MyClass):
    pass

instance = MyOtherClassWithoutAConstructor()
# __new__ called
# __init__ called


class MyOtherClass(MyClass):
    def __init__(self):
        print 'MyOther class __init__ called'
        super(MyOtherClass, self).__init__()
        self.b = 1

instance = MyOtherClass()
# __new__ called
# MyOther class __init__ called
# __init__ called
```

### 3.6.2 __metaclass__方法
内建类型type是内建的基本工厂，它用来生成指定名称、基类以及包含其特性的映射的任何类。my_type.py

```py
def my_method(self):
    return 1

klass = type('MyClass', (object,), {'method': my_method})
instance = klass()
print instance.method()  # 1


# This is similar to an explicit definition of the class:
class MyClass2(object):
    def my_method(self):
        return 1
```

元类：my_meta_cls.py

```py
from ch3.api import API


def equip(classname, base_types, dict):
    if '__doc__' not in dict:
        dict['__doc__'] = API()
    return type(classname, base_types, dict)


class MyClass2(object):
    __metaclass__ = equip

    def alright(self):
        """the ok method"""
        return 'okay'

ma = MyClass2()
ma.y = 6
print ma.__doc__
# alright : the ok method
# y : int
```

动态增强：enhance.py

```py
def enhancer_1(klass):
    c = [l for l in klass.__name__ if l.isupper()]
    klass.contracted_name = ''.join(c)


def enhancer_2(klass):
    def logger(function):
        def wrap(*args, **kw):
            print 'I log everything !'
            return function(*args, **kw)
        return wrap
    for el in dir(klass):
        if el.startswith('_'):
            continue
        value = getattr(klass, el)
        if not hasattr(value, 'im_func'):
            continue
        setattr(klass, el, logger(value))


def enhance(klass, *enhancers):
    for enhancer in enhancers:
        enhancer(klass)


class MySimpleClass(object):
    def ok(self):
        """I return ok"""
        return 'I lied'


enhance(MySimpleClass, enhancer_1, enhancer_2)
thats = MySimpleClass()
print thats.ok()
# I log everything !
# I lied

print thats.contracted_name  # MSC
```

元编程可以动态地在已经被实例化的类定义上创建许多不同变化，但是元类或动态增强只是一种“补丁”，它可能会很快地使精心定义的、清晰的类层次变得一团糟。
