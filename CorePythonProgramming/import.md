# import
## 1.四种方式可以import module

* `import X`：在当前的namespace中创建X的reference。可以通过X.name来调用X中的实体
* `from X import *`：将X中所有public对象的reference到当前namespace。直接通过name调用，X.name是无效的。
* `from X import a, b, c`：与上面方式的区别是仅仅拿到a,b,c三个对象的reference。
* `X = __import__(‘X’)`：与import X效果一致。区别是可以通过string来import，这个用在运行期知道需要import的module。

## 2.循环import

```python
# module X

import Y

def spam():
    print "function in module x"
```

当在main函数中import X，此时会将X挂到sys.modules下，紧接着发现import Y，所以又会将Y挂到sys.modules下。注意此时spam()还没有被执行到。如果module Y：
```python
# module Y

from X import spam # doesn't work: spam isn't defined yet!
```

或者

```python
# module Y

import X

X.spam() # doesn't work either: spam isn't defined yet!
```

这样都会失败。解决的办法是：

1.  在module X中将import Y放到def spam()之后
2.  定义一个module Z，将module Y中涉及对module X的调用放在其中
