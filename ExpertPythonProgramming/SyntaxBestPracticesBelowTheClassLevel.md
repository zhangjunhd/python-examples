# 语法最佳实践——低于类级
## 2.1 列表推导
list_comprehension.py
```py
# list comprehension
print [i for i in range(10) if i % 2 == 0]  # [0, 2, 4, 6, 8]


# enumerate
seq = ["one", "two", "three"]
for i, element in enumerate(seq):
    seq[i] = '%d: %s' % (i, seq[i])
print seq  # ['0: one', '1: two', '2: three']


#  refactored in a list comprehension like this:
def _treatment(pos, elem):
    return '%d: %s' % (pos, elem)

seq = ["one", "two", "three"]
print [_treatment(i, el) for i, el in enumerate(seq)]  # ['0: one', '1: two', '2: three']
```

**每当要对序列中的内容进行循环处理时，就应当尝试用List comprehensions来代替它。**

## 2.2 迭代器和生成器
### 2.2.1 迭代器
- next 返回容器的下一个项目
- \_\_iter\_\_ 返回迭代器本身

iter.py
```py
i = iter('abc')
i.next()  # a
i.next()  # b
i.next()  # c
i.next()  # StopIteration
```

编写自己的迭代器类：my_iterator.py

```py
class MyIterator(object):
    def __init__(self, step):
        self.step = step

    def next(self):
        if self.step == 0:
            raise StopIteration
        self.step -= 1
        return self.step

    def __iter__(self):
        return self

'''
3
2
1
0
'''
for el in MyIterator(4):
    print el
```

### 2.2.2 生成器
yield：

- 基于yield指令，可以暂停一个函数并返回中间结果
- 该函数将返回一个特殊的迭代器，也就是generator对象，它知道如何保存执行环境。对它的调用是不确定的，每次都将产生序列中的下一个元素。

fibonacci.py
```py
def fibonacci():
    a, b = 0, 1
    while True:
        yield b
        a, b = b, a + b

fib = fibonacci()
fib.next()  # 1
fib.next()  # 1
fib.next()  # 2

fib = fibonacci()
print [fib.next() for i in range(10)]  # [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```

标准库的tokenize：针对每个处理过的行返回一个迭代器

my_tokenize.py
```py
import tokenize

reader = open('iter.py').next
tokens = tokenize.generate_tokens(reader)

tokens.next()  # (1, '__author__', (1, 0), (1, 10), "__author__ = 'zhangjun'\n")
tokens.next()  # (51, '=', (1, 11), (1, 12), "__author__ = 'zhangjun'\n")
tokens.next()  # (3, "'zhangjun'", (1, 13), (1, 23), "__author__ = 'zhangjun'\n")
tokens.next()  # (4, '\n', (1, 23), (1, 24), "__author__ = 'zhangjun'\n")
```

下面的例子中，每个函数用来在序列上定义一个转换。然后它们被链接起来应用。每次调用将处理一个元素并将返回其结果：iter_convert.py

```py
def power(values):
    for value in values:
        print 'powering %s' % value
        yield value


def adder(values):
    for value in values:
        print 'adding to %s' % value
        if value % 2 == 0:
            yield value + 3
        else:
            yield value + 2

elements = [1, 4, 7, 9, 12, 19]
res = adder(power(elements))
res.next()
# powering 1
# adding to 1
# 3

res.next()
# powering 4
# adding to 4
# 7
```

send的工作机制和next一样，但是yield将变成能够返回传入的值。talk.py

```py
def psychologist():
    print 'Please tell me your problems'
    while True:
        answer = (yield)
        if answer is not None:
            if answer.endswith('?'):
                print ("Don't ask yourself too much questions")
            elif 'good' in answer:
                print "A that's good, go on"
            elif 'bad' in answer:
                print "Don't be so negative"

free = psychologist()
free.next()
# Please tell me your problems

free.send('I feel bad')
# Don't be so negative

free.send("Why I shouldn't ?")
# Don't ask yourself too much questions

free.send("ok then i should find what is good for me")
# A that's good, go on
```

- throw允许客户端代码传入任意类型的异常；
- colse会抛出一个特定的异常GeneratorEixt；

talk_throw.py
```py
def my_generator():
    try:
        yield 'something'
    except ValueError:
        yield 'dealing with the exception'
    finally:
        print "ok let's clean"


gen = my_generator()
gen.next()  # something
gen.throw(ValueError('mean mean mean'))  # dealing with the exception
gen.close()  # ok let's clean
gen.next()   # StopIteration
```

### 2.2.3 生成器表达式
genexp.py //区别于List comprehensions的是它用圆括号

```py
iter = (x**2 for x in range(10) if x % 2 == 0)
for el in iter:
    print el
```

### 2.2.4 itertools模块
#### islice
返回一个运行在序列的子分组之上的迭代器starting_at_five.py:
```py
import itertools


def starting_at_five():
    value = raw_input().strip()
    while value != '':
        for el in itertools.islice(value.split(), 4, None):
            yield el
        value = raw_input().strip()
```

#### tee
提供了在一个序列上运行多个迭代器的模式with_head.py
```py
import itertools


def with_head(iterable, headsize=1):
    a, b = itertools.tee(iterable)
    return list(itertools.islice(a, headsize)), b
```

### RLE(run-length encoding)groupby实现
rle.py
```py
from itertools import groupby


def compress(data):
    return ((len(list(group)), name) for name, group in groupby(data))


def decompress(data):
    return (car * size for size, car in data)

list(compress('get uuuuuuuuuuuuuuuuuup'))  # [(1, 'g'), (1, 'e'), (1, 't'), (1, ' '), (18, 'u'), (1, 'p')]

compressed = compress('get uuuuuuuuuuuuuuuuuup')

''.join(decompress(compressed))  # get uuuuuuuuuuuuuuuuuup
```

## 2.3 装饰器
编写一个函数，返回封装原始函数调用的一个子函数；当装饰器需要参数时，必须使用第二级封装。mydecorator.py

```py
def mydecorator(function):
    def _mydecorator(*args, **kw):
        # do some stuff before the real function gets called
        res = function(*args, **kw)
        # do some stuff after
        return res
    # returns the sub-function
    return _mydecorator


def mydecorator(arg1, arg2):
    def _mydecorator(function):
        def __mydecorator(*args, **kw):
            # do some stuff before the real
            # function gets called
            res = function(*args, **kw)
            # do some stuff after
            return res
        # returns the sub-function
        return __mydecorator
    return _mydecorator
```

一些应用：
### 参数检查xmlrpc_check.py
```py
from itertools import izip

rpc_info = {}


def xmlrpc(in_=(), out=(type(None),)):
    def _xmlrpc(function):
        # registering the signature
        func_name = function.func_name
        rpc_info[func_name] = (in_, out)

        def _check_types(elements, types):
            """Subfunction that checks the types."""
            if len(elements) != len(types):
                raise TypeError('argument count is wrong')
            typed = enumerate(izip(elements, types))
            for index, couple in typed:
                arg, of_the_right_type = couple
                if isinstance(arg, of_the_right_type):
                    continue
                raise TypeError('arg #%d should be %s' % (index, of_the_right_type))

        # wrapped function
        def __xmlrpc(*args):   # no keywords allowed
            # checking what goes in
            checkable_args = args[1:]   # removing self
            _check_types(checkable_args, in_)
            # running the function
            res = function(*args)

            # checking what goes out
            if not type(res) in (tuple, list):
                checkable_res = (res,)
            else:
                checkable_res = res
            _check_types(checkable_res, out)

            # the function and the type checking succeeded
            return res
        return __xmlrpc
    return _xmlrpc


# A usage example would be:
class RPCView(object):
    @xmlrpc((int, int))     # two int -> None
    def meth1(self, int1, int2):
        print 'received %d and %d' % (int1, int2)

    @xmlrpc((str,), (int,))     # string -> int
    def meth2(self, phrase):
        print 'received %s' % phrase
        return 12

my = RPCView()
my.meth1(1, 2)  # received 1 and 2
my.meth2(2)  # TypeError: arg #0 should be <type 'str'>
```

### 缓存cache.py
```py
import time
import hashlib
import pickle

cache = {}


def is_obsolete(entry, duration):
    return time.time() - entry['time'] > duration


def compute_key(function, args, kw):
    key = pickle.dumps((function.func_name, args, kw))
    return hashlib.sha1(key).hexdigest()


def memoize(duration=10):
    def _memoize(function):
        def __memoize(*args, **kw):
            key = compute_key(function, args, kw)

            # do we have it already ?
            if key in cache and not is_obsolete(cache[key], duration):
                print 'we got a winner'
                return cache[key]['value']

            # computing
            result = function(*args, **kw)

            # storing the result
            cache[key] = {'value': result, 'time': time.time()}
            return result
        return __memoize
    return _memoize


# an example of usage:
@memoize()  # Notice that here uses empty parenthesis because of the two-level wrapping.
def very_very_very_complex_stuff(a, b):
    # if your computer gets too hot on this calculation consider stopping it
    return a + b

very_very_very_complex_stuff(2, 2)
# 4

very_very_very_complex_stuff(2, 2)
# we got a winner
# 4


@memoize(1)  # invalidates the cache after 1 second
def very_very_very_complex_stuff2(a, b):
    return a + b

very_very_very_complex_stuff2(2, 2)
# 4

very_very_very_complex_stuff2(2, 2)
# we got a winner
# 4

time.sleep(2)

very_very_very_complex_stuff2(2, 2)
# 4
```
### 代理proxy.py
```py
class User(object):
    def __init__(self, roles):
        self.roles = roles


class Unauthorized(Exception):
    pass


def protect(role):
    def _protect(function):
        def __protect(*args, **kw):
            user = globals().get('user')
            if user is None or role not in user.roles:
                raise Unauthorized("I won't tell you")
            return function(*args, **kw)
        return __protect
    return _protect


tarek = User(('admin', 'user'))
bill = User(('user',))


class MySecrets(object):
    @protect('admin')
    def waffle_recipe(self):
        print 'use tons of buffer!'

these_are = MySecrets()
user = tarek
these_are.waffle_recipe()
# use tons of buffer!

user = bill
these_are.waffle_recipe()
# __main__.Unauthorized: I won't tell you
```
### 上下文提供者context_provider.py
```py
from threading import RLock

lock = RLock()


def synchronized(function):
    def _synchronized(*args, **kw):
        lock.acquire()
        try:
            return function(*args, **kw)
        finally:
            lock.release()
    return _synchronized

@synchronized
def thread_safe(): # make sure it locks the resource
    pass
```

## 2.4 with和contextlib
确保文件的close：my_with.py,可以使用with的模块：
- thread.LockType
- threading.Lock
- threading.RLock
- threading.Condition
- threading.Semaphore
- threading.BoundedSemaphore

```py
'''
hosts = file('/etc/hosts')
try:
    for line in hosts:
        if line.startswith('#'):
            continue
        print line
finally:
    hosts.close()
'''

with file('/etc/hosts') as hosts:
    for line in hosts:
        if line.startswith('#'):
            continue
        print line
```

这些类都实现了两个方法——\_\_enter\_\_和\_\_exit\_\_，这都来自于with协议。换句话说，任何类都可以实现为如下所示。with_context.py

```py
class Context(object):
    def __enter__(self):
        print 'entering the zone'

    def __exit__(self, exception_type, exception_value, exception_traceback):
        print 'leaving the zone'
        if exception_type is None:
            print 'with no error'
        else:
            print 'with an error (%s)' % exception_value

with Context():
    print 'i am the zone'
'''
entering the zone
i am the zone
leaving the zone
with no error
'''
```

contextlib模块中最有用的辅助类是contextmanager，这是一个装饰器，它增强了包含以yield语句分开的\_\_enter\_\_和\_\_exit\_\_两部分的生成器。

my_contextmanager.py，如果发生任何异常，该函数需要重新抛出这个异常，以便传递它。

```py
from contextlib import contextmanager
from __future__ import with_statement

@contextmanager
def context():
    print 'entering the zone'
    try:
        yield
    except Exception, e:
        print 'with an error (%s)' % e
        # we need to re-raise here
        raise e
    else:
        print 'with no error'
```

上下文实例 log.py

```py
from contextlib import contextmanager

@contextmanager
def logger(klass, logger):
    # logger
    def _log(f):
        def __log(*args, **kw):
            logger(f, args, kw)
            return f(*args, **kw)
        return __log

    # let's equip the class
    for attribute in dir(klass):
        if attribute.startswith('_'):
            continue
        element = getattr(klass, attribute)
        setattr(klass, '__logged_%s' % attribute, element)
        setattr(klass, attribute, _log(element))

    # let's work
    yield klass

    # let's remove the logging
    for attribute in dir(klass):
        if not attribute.startswith('__logged_'):
            continue
        element = getattr(klass, attribute)
        setattr(klass, attribute[len('__logged_'):], element)
        delattr(klass, attribute)


class One(object):
    def _private(self):
        pass

    def one(self, other):
        self.two()
        other.thing(self)
        self._private()

    def two(self):
        pass


class Two(object):
    def thing(self, other):
        other.two()


calls = []


def called(meth, args, kw):
    calls.append(meth.im_func.func_name)

with logger(One, called):
    one = One()
    two = Two()
    one.one(two)

print calls  # ['one', 'two', 'two']
```
