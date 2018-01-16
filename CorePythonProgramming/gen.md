# 生成器
从句法上讲，生成器是一个带yield 语句的函数。一个函数或者子程序只返回一次，但一个生成器能暂停执行并返回一个中间的结果——那就是yield 语句的功能， 返回一个值给调用者并暂停执行。当生成器的next()方法被调用的时候，它会准确地从离开地方继续。当到达一个真正的返回或者函数结束没有更多的值返回（当调用next())，一个StopIteration 异常就会抛出。简单的生成器：

```python
def simple_gen():
        yield 1
        yield '2 --> punch!'
```

调用：

    >>> myG = simple_gen()
    >>> myG.next()
    1
    >>> myG.next()
    '2 --> punch!'
    >>> myG.next()
    Traceback (most recent call last):
    File "", line 1, in ?
    myG.next() StopIteration

由于python 的for 循环有next()调用和对StopIteration 的处理，使用一个for 循环而不是手动迭代穿过一个生成器（或者那种事物的迭代器）总是要简洁漂亮得多:

    >>> for eachItem in simple_gen():
    ... print eachItem
    ...
    1
    '2 --> punch!'

下面的例子，将要创建一个带序列并从那个序列中返回一个随机元素的随机迭代器，每个返回的元素将从那个队列中消失，像一个list.pop()和random.choice()的结合的归类：

```python
from random import randint
def rand_gen(aList):
    while len(aList) > 0:
        yield aList.pop(randint(0, len(aList)))
```

调用：

    >>> for item in rand_gen(['rock', 'paper', 'scissors']):
    ... print item
    ...
    scissors
    rock
    paper

除了next()来获得下个生成的值，用户可以将值回送给生成器[send()]，在生成器中抛出异常，以及要求生成器退出[close()]

```python
def counter(start_at=0):
        count = start_at
        while True:
            val = (yield count)
            if val is not None:
                print 'reset here'
                count = val
            else:
                count += 1
                if count == 4:
                    raise Exception('aha')
```

send调用：

```python
count = counter(5)
print count.next()
print count.next()
count.send(9)
print count.next()
```

输出：

    5
    6
    reset here
    10

close调用：

```python
count = counter(5)
print count.next()
print count.next()
count.close()
count.next()  # raise StopIteration here
```

输出：

    5
    6
    Traceback (most recent call last):
      File "/home/zhangjun/workspace/try/src/try.py", line 16, in <module>
        count.next()  # raise StopIteration here
    StopIteration

抛出异常调用：

```python
count = counter(3)
print count.next()
print count.next()# raise Exception here
```

输出：

    3
    Traceback (most recent call last):
      File "/home/zhangjun/workspace/try/src/try.py", line 14, in <module>
        print count.next()# raise Exception here
      File "/home/zhangjun/workspace/try/src/try.py", line 11, in counter
        raise Exception('aha')
    Exception: aha
