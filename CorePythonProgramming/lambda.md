## 1.lambda定义
python 允许用lambda 关键字创造匿名函数。参数是可选的，如果使用的参数话，参数通常也是表达式的一部分。下匿名函数的语法:

    lambda [arg1[, arg2, ... argN]]: expression

    def true(): return True
    #相当于
    lambda :True

在上面的例子中，我们简单地用lambda 创建了一个函数（对象），但是既没有在任何地方保存它,也没有调用它。这个函数对象的引用计数在函数创建时被设置为True，但是因为没有引用保存下来， 计数又回到零，然后被垃圾回收掉。为了保留住这个对象，我们将它保存到一个变量中，以后可以随时调用：

    >>> true = lambda :True
    >>> true() True

几个例子：

    >>> a = lambda x, y=2: x + y
    >>> a(3)
    5
    >>> a(3,5)
    8
    >>> a(0)
    2
    >>> a(0,9)
    9
    >>>
    >>> b = lambda *z: z //可变长参数
    >>> b(23, 'zyx')
    (23, 'zyx')
    >>> b(42)
    (42,)

## 2.内建函数filter()、map()、reduce()与lambda配合使用

- filter(func, seq):调用一个布尔函数func 来迭代遍历每个seq 中的元素； 返回一个使func 返回值为ture 的元素的序列。
- map(func, seq1[,seq2...]):将函数func 作用于给定序列（s)的每个元素，并用一个列表来提供返回值；如果func 为None，func 表现为一个身份函数，返回一个含有每个序列中元素集合的n个元组的列表。
- reduce(func, seq[, init]):将二元函数作用于seq 序列的元素，每次携带一对（先前的结果以及下一个序列元素），连续的将现有的结果和下一个值作用在获得的随后的结果上，最后减少我们的序列为一个单一的返 回值；如果初始值init 给定，第一个比较会是init 和第一个序列元素而不是序列的头两个元素。

如果我们想要用纯python 编写filter()，它或许就像这样：
```python
def filter(bool_func, seq):
    filtered_seq = []
    for eachItem in seq:
        if bool_func(eachItem):
            filtered_seq.append(eachItem)
    return filtered_seq
```

下面展示在一个使用了filer()来获得任意奇数的简短列表的脚本。该脚本产生一个较大的随机数集合，然后过滤出所有的的偶数，留给我们一个需要的数据集。当一开始编写这个例子的时候，oddnogen.py 如下所示：
```python
from random import randint

def odd(n):
    return n % 2

allNums = []
for eachNum in range(9):
    allNums.append(randint(1, 99))

print filter(odd, allNums)
```

用一个lambda 表达式替换:
```python
from random import randint

allNums = []
for eachNum in range(9):
    allNums.append(randint(1, 99))

print filter(lambda n: n%2, allNums)
```

使用列表解析替换：
```python
from random import randint

allNums = []
for eachNum in range(9):
    allNums.append(randint(1, 99))

print [n for n in allNums if n%2]
```

通过整合另外的列表解析将我们最后的列表放在一起，来进一步简化我们的代码：
```python
from random import randint as ri
print [n for n in [ri(1,99) for i in range(9)] if n%2]
```

如果我们要用python 编写这个简单形式的map()如何运作的， 它可能如下代码：
```python
def map(func, seq):
    mapped_seq = []
    for eachItem in seq:
        mapped_seq.append(func(eachItem))
    return mapped_seq
```

列举一些简短的lambda 函数来展示如何用map()处理实际数据(列表解析具有同样功能)：

    >>> map((lambda x: x+2), [0, 1, 2, 3, 4, 5])
    [2, 3, 4, 5, 6, 7]
    >>> map(lambda x: x**2, range(6))
    [0, 1, 4, 9, 16, 25]
    >>> [x+2 for x in range(6)]
    [2, 3, 4, 5, 6, 7]
    >>>[x**2 for x in range(6)]
    [0, 1, 4, 9, 16, 25]

这里是使用带多个序列的map()的例子：

    >>> map(lambda x, y: x + y, [1,3,5], [2,4,6])
    [3, 7, 11]
    >>> map(lambda x, y: (x+y, x-y), [1,3,5], [2,4,6])
    [(3, -1), (7, -1), (11, -1)]
    >>> map(None, [1,3,5], [2,4,6])
    [(1, 2), (3, 4), (5, 6)]

上面最后的例子使用了map()和一个为None 的函数对象来将不相关的序列归并在一起。这种思想在一个新的内建函数，zip，被加进来之前的python2.0 是很普遍的。而zip 是这样做的：

    >>> zip([1,3,5], [2,4,6])
    [(1, 2), (3, 4), (5, 6)]

如果我们想要试着用纯python 实现reduce()， 它可能会是这样：
```python
def reduce(bin_func, seq, init=None):
    lseq = list(seq) # convert to list
    if init is None: # initializer?
        res = lseq.pop(0) # no
    else:
        res = init # yes
    for item in lseq: # reduce sequence
        res = bin_func(res, item) # apply function
    return res # return result
```

这里给出一个示例：

    >>> def mySum(x,y): return x+y
    >>> for eachNum in allNums:
    ... total = mySum(total, eachNum)
    ...
    >>> print 'the total is:', total
    the total is: 10


使用lambda 和reduce(),我们可以以一行代码做出相同的事情。

    >>> print 'the total is:', reduce((lambda x,y: x+y), range(5))
    the total is: 10

给出了上面的输入，reduce()函数运行了如下的算术操作。

    ((((0 + 1) + 2) + 3) + 4) => 10

## 3.使用lambda的排序代码
```python
# -*- coding: utf-8 -*-

a = 'awk#39'
b = 'sed#1'
c = 'java#21'
d = 'python#100'

data = [a, b, c, d]

print data  # ['awk#39', 'sed#1', 'java#21', 'python#100']
data.sort(key=lambda x: int(x.split('#')[1]))
print data  # ['sed#1', 'java#21', 'awk#39', 'python#100']
```
