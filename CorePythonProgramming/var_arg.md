# 可变长参数
## 1.非关键字可变长参数（元组）
可变长的参数元组必须在位置和默认参数之后，带元组（或者非关键字可变长参数）的函数普遍的语法如下：
```python
def function_name([formal_args,] *nkwargs):
    "function_documentation_string"
    function_body_suite
```

星号操作符之后的形参将作为元组传递给函数,元组保存了所有传递给函数的"额外"的参数(匹配了所有位置和具名参数后剩余的)。如果没有给出额外的参数，元组为空。示例：
```python
def tupleVarArgs(arg1, arg2='defaultB', *theRest):
    'display regular args and non-keyword variable args'
    print 'formal arg 1:', arg1
    print 'formal arg 2:', arg2
    for eachXtrArg in theRest:
        print 'another arg:', eachXtrArg
        print '*' * 20

tupleVarArgs('abc')
tupleVarArgs(23, 4.56)
tupleVarArgs('abc', 123, 'xyz', 456.789)
```

输出：

    formal arg 1: abc
    formal arg 2: defaultB
    ********************
    formal arg 1: 23
    formal arg 2: 4.56
    ********************
    formal arg 1: abc
    formal arg 2: 123
    another arg: xyz
    another arg: 456.789
    ********************
## 2.关键字变量参数（字典）
使用变量参数字典来应对额外关键字参数的函数定义的语法：
```python
def function_name([formal_args,][*nkwargs,] **kwargs):
    "function_documentation_string"
    function_body_suite
```

关键字变量参数应该为函数定义的最后一个参数，带**。示例：
```python
def dictVarArgs(arg1, arg2='defaultB', **theRest):
    'display 2 regular args and keyword variable args'
    print 'formal arg1:', arg1
    print 'formal arg2:', arg2
    for eachXtrArg in theRest.keys():
        print 'Xtra arg %s: %s' % (eachXtrArg, str(theRest[eachXtrArg]))
        print '*' * 20

dictVarArgs(1220, 740.0, c='grail')
dictVarArgs(arg2='tales', c=123, d='poe', arg1='mystery')
dictVarArgs('one', d=10, e='zoo', men=('freud', 'gaudi'))
```

输出：

    formal arg1: 1220
    formal arg2: 740.0
    Xtra arg c: grail
    ********************
    formal arg1: mystery
    formal arg2: tales
    Xtra arg c: 123
    Xtra arg d: poe
    ********************
    formal arg1: one
    formal arg2: defaultB
    Xtra arg men: ('freud', 'gaudi')
    Xtra arg e: zoo
    Xtra arg d: 10
    ********************

## 3.示例
关键字和非关键字可变长参数都有可能用在同一个函数中，只要关键字字典是最后一个参数并且非关键字元组先于它之前出现，正如在如下例子中的一样：
```python
def newfoo(arg1, arg2, *nkwargs, **kwargs):
    'display regular args and all variable args'
    print 'arg1 is:', arg1
    print 'arg2 is:', arg2
    for eachNKW in nkwargs:
        print 'additional non-keyword arg:', eachNKW
    for eachKW in kwargs.keys():
        print "additional keyword arg '%s': %s" % (eachKW, kwargs[eachKW])
    print '*' * 20

#example 1
newfoo(10, 20, 30, 40, foo=50, bar=60)

#example 2
newfoo(2, 4, *(6, 8), **{'foo': 10, 'bar': 12})

#example 3
aTuple = (6, 7, 8)
aDict = {'z': 9}
newfoo(1, 2, 3, x=4, y=5, *aTuple, **aDict)
```

输出：

    arg1 is: 10
    arg2 is: 20
    additional non-keyword arg: 30
    additional non-keyword arg: 40
    additional keyword arg 'foo': 50
    additional keyword arg 'bar': 60
    ********************
    arg1 is: 2
    arg2 is: 4
    additional non-keyword arg: 6
    additional non-keyword arg: 8
    additional keyword arg 'foo': 10
    additional keyword arg 'bar': 12
    ********************
    arg1 is: 1
    arg2 is: 2
    additional non-keyword arg: 3
    additional non-keyword arg: 6
    additional non-keyword arg: 7
    additional non-keyword arg: 8
    additional keyword arg 'y': 5
    additional keyword arg 'x': 4
    additional keyword arg 'z': 9
    ********************
说明：

* example 1：使用旧风格的方式来分别列出所有的参数。
* example 2：将非关键字参数放在元组中将关键字参数放在字典中，而不是逐个列出变量参数。
* example 3：在函数调用之外来创建我们的元组和字典。注意，额外的非关键字值‘3’以及‘x’和‘y'关键字对也被包含在最终的参数列表中，而它们不是`*`和`**`的可变参数中的元素。
