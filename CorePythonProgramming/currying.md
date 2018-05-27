`currying` 的概念将函数式编程的概念和默认参数以及可变参数结合在一起。一个带n 个参数，curried 的函数固化第一个参数为固定参数，并返回另一个带n-1 个参数函数对象，分别类似于LISP的原始函数car 和cdr 的行为。Currying 能泛化成为偏函数应用（PFA）， 这种函数将任意数量（顺序）的参数的函数转化成另一个带剩余参数的函数对象。在某种程度上，这似乎和不提供参数，就会使用默认参数情形相似。这个特征是在python2.5 的时候被引入的，通过functools 模块能很好的给用户调用。

    >>> from operator import add, mul
    >>> from functools import partial
    >>> add1 = partial(add, 1) # add1(x) == add(1, x)
    >>> mul100 = partial(mul, 100) # mul100(x) == mul(100, x)
    >>>
    >>> add1(10)
    11
    >>> add1(1)
    2
    >>> mul100(10)
    1000
    >>> mul100(500)
    50000

下面的一个例子来自python 文档中关于在应用程序中使用，在这些程序中需要经常将二进制（作为字符串）转换成为整数。这个例子使用了int()内建函数并将base 固定为2 来指定二进制字符串转化。

    >>> baseTwo = partial(int, base=2)
    >>> baseTwo.__doc__ = 'Convert base 2 string to an int.'
    >>> baseTwo('10010')
    18

如果你创建了不带base 关键字的偏函数，比如， baseTwoBAD = partial(int, 2)，这可能会让参数以错误的顺序传入int()，因为固定参数总是放在运行时刻参数的左边， 比如baseTwoBAD(x) == int(2, x)。如果你调用它， 它会将2 作为需要转化的数字，base 作为'10010'来传入，接着产生一个异常：

    >>> baseTwoBAD = partial(int, 2)
    >>> baseTwoBAD('10010')
    Traceback (most recent call last): File "<stdin>", line 1, in <module>
    TypeError: an integer is required

由于关键字放置在恰当的位置， 顺序就得固定下来，因为，如你所知，关键字参数总是出现在形参之后， 所以baseTwo(x) == int(x, base=2).
