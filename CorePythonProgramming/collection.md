# collection小技巧
## 1.序列连接操作符
连接操作符( + )这个操作符允许我们把一个序列和另一个相同类型的序列做连接。

    sequence1 + sequence2
对字符串来说，这个操作不如把所有的子字符串放到一个列表或可迭代对象中，然后调用一个join方法来把所有的内容连接在一起节约内存;对列表来说，推荐用列表类型的extend()方法来把两个或者多个列表对象合并.

## 2.序列切片

    >>> s = 'abcdefgh'
    >>> s[::-1] # 可以视作"翻转"操作
    'hgfedcba'
    >>> s[::2] # 隔一个取一个的操作
    'aceg

切片索引的开始和结束素引值可以超过字符串的长度。换句话说，起始索引可以小于0,而对于结束索引，即使索引值为100 的元素并不存在也不会报错.

    >>> ('Faye', 'Leanna', 'Daylen')[-100:100]
    ('Faye', 'Leanna', 'Daylen')

有这么一个问题：有一个字符串，我们想通过一个循环按照这样的形式显示它：每次都把位于最后的一个字符砍掉，下面是实现这个要求的一种方法：

    >>> s = 'abcde'
    >>> i = -1
    >>> for i in range(-1, -len(s), -1):
    ...     print s[:i]
    ...
    abcd
    abc
    ab
    a

因为-1 已经是“最小”的索引了.我们不可能用0 来作为索引值，因为这会切片到第一个元素之前而什么都不会显示:

    >>> s[:0]
    ''

我们的方案是使用另一个小技巧:用None 作为索引值

    >>> s = 'abcde'
    >>> for i in [None] + range(-1, -len(s), -1):
    ...     print s[:i]
    ...
    abcde
    abcd
    abc
    ab
    a

似乎还可以先创建一个只包含None 的列表,然后用extend()函数把range()的输出添加到这个列表,或者先建立range()输出组成的列表然后再把None 插入到这个列表的最前面，然后对这个列表进行遍历:

    >>> for i in [None].extend(range(-1, -len(s), -1)):
    ...     print s[:i]
    ...
    Traceback (most recent call last):
    File "<stdin>", line 1, in ?
    TypeError: iteration over non-sequence

这个错误发生的原因是[None].extend(...)函数返回None , None 既不是序列类型也不是可迭代对象.

## 3.序列的可变对象与不可变对象
字符串是一种不可变数据类型。就是说它的值是不能被改变或修改的。这就意味着如果你想修改一个字符串，或者截取一个子串，或者在字符串的末尾连接另一个字符串等等，你必须新建一个字符串。

    >> s = 'abc'
    >>>
    >>> id(s)
    135060856
    >>>
    >>> s += 'def'
    >>> id(s)
    135057968

对字符串的一个字符或者一片字符的改动都是不被允许的:

    >>> s
    'abcdef'
    >>>
    >>> s[2] = 'C'
    Traceback (innermost last):
    File "<stdin>", line 1, in ? AttributeError: __setitem__
    >>>
    >>> s[3:6] = 'DEF'
    Traceback (innermost last):
    File "<stdin>", line 1, in ?
    AttributeError: __setslice__

那些可以改变对象值的可变对象的方法是没有返回值的:

    >>> music_media.sort()# 没有输出?
    >>>

在使用可变对象的方法如sort(),extend()和reverse()的时候要注意,这些操作会在列表中原地执行操作,也就是说现有的列表内容会被改变,但是没有返回值!是的,与之相反,字符串方法确实有返回值:

    >>> 'leanna, silly girl!'.upper()
    'LEANNA, SILLY GIRL!'

虽然元组对象本身是不可变的，但这并不意味着元组包含的可变对象也不可变了:

    >>> t = (['xyz', 123], 23, -103.4)
    >>> t
    (['xyz', 123], 23, -103.4)
    >>> t[0][1]
    123
    >>> t[0][1] = ['abc', 'def']
    >>> t
    (['xyz', ['abc', 'def']], 23, -103.4)

## 4.序列类型对象的拷贝
序列类型对象的浅拷贝是默认类型拷贝,并可以以下几种方式实施:(1)完全切片操作[:],(2)利用工厂函数,比如list(),dict()等,(3)使用copy 模块的copy 函数.

    >>> person = ['name', ['savings', 100.00]]
    >>> hubby = person[:] # slice copy
    >>> wifey = list(person) # fac func copy
    >>> [id(x) for x in person, hubby, wifey]
    [11826320, 12223552, 11850936]

当进行列表复制时，第一个对象是不可变的(是个字符串类型),而第二个是可变的(一个列表).正因为如此,当进行浅拷贝时,字符串被显式的拷贝,并新创建了一个字符串对象,而列表元素只是把它的引用复制了一下。

    >>> hubby[0] = 'joe'
    >>> wifey[0] = 'jane'
    >>> hubby, wifey
    (['joe', ['savings', 100.0]], ['jane', ['savings', 100.0]])
    >>> hubby[1][1] = 50.00
    >>> hubby, wifey
    (['joe', ['savings', 50.0]], ['jane', ['savings', 50.0]])

要得到一个完全拷贝或者说深拷贝--创建一个新的容器对象,包含原有对象元素（引用）全新拷贝的引用--需要copy.deepcopy()函数。

    >>> person = ['name', ['savings', 100.00]]
    >>> hubby = person
    >>> import copy
    >>> wifey = copy.deepcopy(person)
    >>> [id(x) for x in person, hubby, wifey]
    [12242056, 12242056, 12224232]
    >>> hubby[0] = 'joe'
    >>> wifey[0] = 'jane'
    >>> hubby, wifey
    (['joe', ['savings', 100.0]], ['jane', ['savings', 100.0]])
    >>> hubby[1][1] = 50.00
    >>> hubby, wifey
    (['joe', ['savings', 50.0]], ['jane', ['savings', 100.0]])

非容器类型(比如数字,字符串和其他"原子"类型的对象,像代码,类型和xrange 对象等)没有被拷贝一说。

如果元组变量只包含原子类型对象,对它的深拷贝将不会进行。

    >>> person = ['name', ('savings', 100.00)]
    >>> newPerson = copy.deepcopy(person)
    >>> [id(x) for x in person, newPerson]
    [12225352, 12226112]
    >>> [id(x) for x in person]
    [9919616, 11800088]
    >>> [id(x) for x in newPerson]
    [9919616, 11800088]

## 5.访问字典中的值
如果我们想访问该字典中的一个数据元素，而它在这个字典中没有对应的键，将会产生一个错误：

    >>> dict2['server'] Traceback (innermost last):
    File "<stdin>", line 1, in ?
    KeyError: server

检查一个字典中是否有某个键的方法使用in 或 not in 操作符：

    >>> 'server' in dict
    False
    >>> 'name' in dict
    True

## 6.字典排序
```python
mydict = {'carl':40, 'alan':2, 'bob':1, 'danny':3}
```

基于key排序：
```python
for key in sorted(mydict.iterkeys()):
    print "%s: %s" % (key, mydict[key])
```

Results:

    alan: 2
    bob: 1
    carl: 40
    danny: 3

基于value排序：
```python
for key, value in sorted(mydict.iteritems(), key=lambda (k,v): (v,k)):
    print "%s: %s" % (key, value)
```

Results:

    bob: 1
    alan: 2
    danny: 3
    carl: 40

取出最大和最小key：
```python
min_key = min(mydict.keys())
max_key = max(mydict.keys())
```

使用iterator遍历，dict有几种迭代子，它们分别是：iteritems, iterkeys, itervalues。下面就iteritems给出一个使用的例子：
```python
for k,v in myDict.iteritems():
    print k,v
```

## 7.判断一个 list 是否为空
传统的方式：
```python
if len(mylist):
    # Do something with my list
else:
    # The list is empty
```

由于一个空 list 本身等同于 False，所以可以直接：
```python
if mylist:
    # Do something with my list
else:
    # The list is empty
```

## 8.遍历 list 的同时获取索引
传统的方式：
```python
i = 0
for element in mylist:
    # Do something with i and element
    i += 1
```

这样更简洁些：
```python
for i, element in enumerate(mylist):
    # Do something with i and element
    pass
```

## 9.list 排序
在包含某元素的列表中依据某个属性排序是一个很常见的操作。例如这里我们先创建一个包含 person的list：
```python
class Person(object):
    def __init__(self, age):
        self.age = age

persons = [Person(age) for age in (14, 78, 42)]
```

传统的方式是：
```python
def get_sort_key(element):
    return element.age

for element in sorted(persons, key=get_sort_key):
    print "Age:", element.age
```

更加简洁、可读性更好的方法是使用 Python 标准库中的 operator 模块。attrgetter 方法优先返回读取的属性值作为参数传递给 sorted 方法。operator 模块还包括 itemgetter 和 methodcaller 方法，作用如其字面含义。
```python
from operator import attrgetter

for element in sorted(persons, key=attrgetter('age')):
    print "Age:", element.age
```

## 10.在 Dictionary 中元素分组
```python
class Person(object):
    def __init__(self, age):
        self.age = ageperson

s = [Person(age) for age in (78, 14, 78, 42, 14)]
```

如果现在我们要按照年龄分组的话，一种方法是使用 in 操作符：
```python
persons_by_age = {}

for person in persons:
    age = person.age
    if age in persons_by_age:
        persons_by_age[age].append(person)
    else:
        persons_by_age[age] = [person]

assert len(persons_by_age[78]) == 2
```

相比较之下，使用 collections 模块中 defaultdict 方法的途径可读性更好。defaultdict 将会利用接受的参数为每个不存在的 key 创建对应的值，这里我们传递的是 list，所以它将为每个 key 创建一个 list 类型的值：
```python
from collections import defaultdict

persons_by_age = defaultdict(list)
for person in persons:
    persons_by_age[person.age].append(person)
```
