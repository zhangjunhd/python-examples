# 实例属性与类属性
## 1.类属性为不变对象时的‘隐藏’效果
```python
class Foo(object):
    version = 1.0

f = Foo()
print Foo.version  # 1.0
print f.version   # 1.0

Foo.version += 0.1
print Foo.version # 1.1
print f.version # 1.1

f.version += 0.1
print Foo.version # not be modified, 1.1
print f.version # 1.2

del f.version
print Foo.version # 1.1
print f.version # recover back, 1.1
```

类属性version，可以通过类名和实例名访问之，都得到1.0。此时，通过类名修改version，都得到1.1。而如果通过实例名修改属性，发现此时创建了一个新的实例属性，其值为1.2，而类属性依旧为1.1。最后删除实例属性，发现通过实例名调用再次得到类属性值1.1。

## 2.类属性为不变对象
```python
class Foo(object):
    version = {'a':1}

f = Foo()
print Foo.version['a']  # 1
print f.version['a']   # 1

Foo.version['a'] = 2
print Foo.version['a'] # 2
print f.version['a'] # 2

f.version['a'] = 3
print Foo.version['a'] # be modified, 3
print f.version['a'] # 3

del f.version # raise exception here:AttributeError: 'Foo' object attribute 'version' is read-only
```

类属性可变的情况下，即使使用实例名修改属性值，此时它还是类属性。所以试图通过实例名删除时，会得到异常。

## 3.类属性持久性
当一个实例在类属性被修改后才创建，那么更新的值就将生效。类属性的修改会影响到所有的实例。
```python
class Foo(object):
    version = 1.0

f1 = Foo()
print f1.version # 1.0

Foo.version += 0.1
print Foo.version  # 1.1
print f1.version   # 1.1

f2 = Foo()
print f2.version # 1.1

Foo.version += 0.1
print f1.version # 1.2
print f2.version # 1.2

f1.version += 0.1
f2.version += 0.2
print Foo.version # 1.2
print f1.version # 1.3
print f2.version # 1.4

Foo.version += 1
print Foo.version # 2.2
print f1.version # 1.3
print f2.version # 1.4
```
最后两段代码演示了，当通过实例名修改属性，相当于此时创建了一个新的实例属性。所以其后再对类属性进行修改(使用类名修改属性值)，将不会影响到实例属性。
