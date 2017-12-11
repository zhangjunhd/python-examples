# 序列化
一旦数据被序列化，就可以把它写入到文件、socket、管道等中。之后你可以读取这个文件，读取这些数据结构具有相同值的新对象。

```python
# -*- coding: utf-8 -*-

try:
    import cPickle as pickle
except:
    import pickle
import pprint

data1 = [{'a': 'A', 'b': 2, 'c': 3.0}]
print 'BEFORE:',
pprint.pprint(data1)

data1_string = pickle.dumps(data1)
data2 = pickle.loads(data1_string)

print 'AFTER:',
pprint.pprint(data2)

print 'SAME?:', (data1 is data2)
print 'EQUAL?:', (data1 == data2)
```

输出：

    BEFORE:[{'a': 'A', 'b': 2, 'c': 3.0}]
    AFTER:[{'a': 'A', 'b': 2, 'c': 3.0}]
    SAME?: False
    EQUAL?: True

在处理自定义类时，应该保证这些被序列化的类会在进程名空间出现。只要数据实例才能被序列化，而不能是定义的类。在反序列化时，类的名字被用于寻找构造器以便创建新对象。dump_obj.py将一个类实例写入到文件中，load_obj.py将刚才序列化的对象装载进来。

dump_obj.py
```python
# -*- coding: utf-8 -*-
try:
    import cPickle as pickle
except:
    import pickle

class SimpleObject(object):

    def __init__(self, name):
        self.name = name
        l = list(name)
        l.reverse()
        self.name_backwards = ''.join(l)

if __name__ == '__main__':
    data = []
    data.append(SimpleObject('pickle'))
    data.append(SimpleObject('cPickle'))
    data.append(SimpleObject('last'))

    out_s = open('testPickle', 'wb')
    try:
        for o in data:
            print 'WRITING: %s (%s)' % (o.name, o.name_backwards)
            pickle.dump(o, out_s)
    finally:
        out_s.close()
```

输出：

    WRITING: pickle (elkcip)
    WRITING: cPickle (elkciPc)
    WRITING: last (tsal)

load_obj.py
```python
# -*- coding: utf-8 -*-
try:
    import cPickle as pickle
except:
    import pickle
from dump_obj import SimpleObject


in_s = open('testPickle', 'rb')
try:
    while True:
        try:
            o = pickle.load(in_s)
        except EOFError:
            break
        else:
            print 'READ: %s (%s)' % (o.name, o.name_backwards)
finally:
    in_s.close()
```

输出：

    READ: pickle (elkcip)
    READ: cPickle (elkciPc)
    READ: last (tsal)
