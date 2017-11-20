## 1. 检测与处理异常
### 1.1 try-except 语句

```python
try: 
    try_suite # watch for exceptions here 
except Exception[, reason]: 
    except_suite # exception-handling code 
```

解释器尝试执行 try 块里的所有代码, 如果代码块完成后没有异常发生, 执行流就会忽略 except 语句继续执行. 而当 except 语句所指定的异常发生后, 我们保存了错误的原因, 控制流立即跳转到对应的处理器( try 子句的剩余语句将被忽略)。实例：

```python
def safe_float(obj):
    try:
        retval = float(obj)
    except ValueError:
        retval = None
    return retval
```

### 1.2 带有多个 except 的 try 语句

```python
try:
    try_suite # watch for exceptions here
except Exception1[, reason1]:
    suite_for_exception_Exception1
except Exception2[, reason2]:
    suite_for_exception_Exception2
```

首先尝试执行 try 子句, 如果没有错误, 忽略所有的 except 从句继续执行. 如果发生异常, 解释器将在这一串处理器(except 子句)中查找匹配的异常. 如果找到对应的处理器,执行流将跳转到这里。实例：

```python
def safe_float(obj):
    try:
        retval = float(obj)
    except ValueError:
        retval = 'could not convert non-number to float'
    except TypeError:
        retval = 'object type cannot be converted to float'
    return retval
```

### 1.3 处理多个异常的 except 语句

```python
try:
    try_suite # watch for exceptions here
except (Exception1, Exception2)[, reason]: 
    suite_for_Exception1_and_Exception2
```

except 语句可以处理任意多个异常,前提只是它们被放入一个元组里。实例：

```python
def safe_float(obj):
    try:
        retval = float(obj)
    except (ValueError, TypeError):
        retval = 'argument must be a number or numeric string'
    return retval
```

### 1.4 捕获所有异常

```python
try:
    try_suite # watch for exceptions here
except Exception, e:#Exception 是在最顶层的
    # error occurred, log 'e', etc. 
```

关于捕获所有异常, 你应当知道有些异常不是由于错误条件引起的. 它们是 `SystemExit` 和 `KeyboardInterupt` . SystemExit 是由于当前 Python 应用程序需要退出, KeyboardInterupt 代表用户按下了 CTRL-C (^C) , 想要关闭 Python。 在真正需要的时候, 这些异常却会被异常处理捕获.一个典型的迂回工作法代码框架可能会是这样:

```python
try:
    try_suite # watch for exceptions here
except (KeyboardInterupt, SystemExit):
    # user wants to quit
    raise # reraise back to caller
except Exception:
    # handle real errors
```

KeyboardInterrupt 和 SystemExit 和 Exception 平级:
    
    - BaseException
    |- KeyboardInterrupt
    |- SystemExit
    |- Exception
       |- (all other current built-in exceptions) 所有当前内建异常

如果你确实需要捕获所有异常, 那么你就得BaseException :

```python
try:
    try_suite # watch for exceptions here
except BaseException, e:
    # handle all errors
```

### 1.5 异常参数

except 语句的这个语法可以被扩展为:

```python
# single exception
except Exception[, reason]: 
    suite_for_Exception_with_Argument 
# multiple exceptions
except (Exception1, Exception2, ..., ExceptionN)[, reason]: 
    suite_for_Exception1_to_ExceptionN_with_Argument 
```

无论 reason 只包含一个字符串或是由错误编号和字符串组成的元组, 调用 str(reason) 总会返回一个良好可读的错误原因. 不要忘记 reason 是一个类实例。实例：

```python
def safe_float(object):
    try:
        retval = float(object)
    except (ValueError, TypeError), diag:
        retval = str(diag)
    return retval
```

### 1.6 else 子句

对于try-except 语句段:在try 范围中没有异常被检测到时,执行else 子句.在else 范围中的任何代码运行前,try 范围中的所有代码必须完全成功(也就是,结束前没有引发异常)。实例：

```python
log = open('logfile.txt', 'w')
try:
    3rd_party_module.function()
except:
    log.write("*** caught exception in module\n")
else:
    log.write("*** no exceptions caught\n")
log.close()
```

### 1.7 finally 子句

finally 子句是无论异常是否发生,是否捕捉都会执行的一段代码.下面是try-except-else-finally 语法的示例:

```python
try:
    A
except MyException:
    B
else:
    C
finally:
    D
```

可能的顺序是A-C-D[`正常`]或A-B-D[`异常`]。另一种使用finally 的方式是finally 单独和try 连用.这个try-finally 语句和try-except区别在于它不是用来捕捉异常的.作为替代,它常常用来维持一致的行为而无论异常是否发生.

```python
try:
    try_suite
finally:
    finally_suite #无论如何都执行
```

当在try 范围中产生一个异常时,(这里)会立即跳转到finally 语句段.**当finally 中的所有代码都执行完毕后,会继续向上一层引发异常.**。实例：

```python
try:
    ccfile = open('carddata.txt', 'r')
    txns = ccfile.readlines()
    ccfile.close()
except IOError:
    log.write('no txns this month\n') 
```

注意如果按照这样的顺序发生错误:打开成功,但是出于一些原因readlines()调用失败,异常处理会去继续执行except 中的子句,而不去尝试关闭文件.我们可以通过try-finally 来实现:

```python
ccfile = None
try:
    try:
        ccfile = open('carddata.txt', 'r')
        txns = ccfile.readlines()
    except IOError:
        log.write('no txns this month\n')
finally:
    if ccfile:
        ccfile.close() 
```

另一种可选的实现切换了try-except 和try-finally 包含的方式:

```python
ccfile = None
try:
    try:
        ccfile = open('carddata.txt', 'r')
        txns = ccfile.readlines()
    finally:
        if ccfile:
            ccfile.close()
except IOError:
    log.write('no txns this month\n') 
```

一个这样写的理由是如果在finally 的语句块内发生了一个异常,你可以创建一个同现有的异常处理器在同一个(外)层次的异常处理器来处理它.这样,从本质上来说,就可以同时处理在原始的 try语句块和finally 语句块中发生的错误.这种方法唯一的问题是,当finally 语句块中的确发生异常时,你会丢失原来异常的上下文信息,除非你在某个地方保存了它.

## 2. 上下文管理与with 语句
with 语句的目的在于从流程图中把 try,except 和finally 关键字和资源分配释放相关代码统统去掉. with 语法的基本用法看上去如下:

```python
with context_expr [as var]:
    with_suite
```

with 语句仅能工作于支持上下文管理协议(context management protocol)的对象.目前已经有了一些支持该协议的对象.下面是第一批成员的简短列表:

    -file
    -decimal.Context
    -thread.LockType
    -threading.Lock
    -threading.RLock
    -threading.Condition
    -threading.Semaphore
    -threading.BoundedSemaphore

示例：

```python
with open('/etc/passwd', 'r') as f:
    for eachLine in f:
        # ...do stuff with eachLine or f...
```

它会完成准备工作,比如试图打开一个文件,如果一切正常,把文件对象赋值给f.然后用迭代器遍历文件中的每一行,当完成时,关闭文件.无论的在这一段代码的开始,中间,还是结束时发生异常,会执行清理的代码,此外文件仍会被自动的关闭.

## 3. 触发异常

rasie 一般的用法是:

```python
raise [SomeException [, args [, traceback]]] 
```

不含任何参数的raise 语句结构会引发当前代码块(code block)最近触发的一个异常.如果之前没有异常触发,会因为没有重新触发的异常而生成一个TypeError 异常.raise 语句的用法：

* raise exclass 触发一个异常,从exclass 生成一个实例(不含任何异常参数)
* raise exclass() 同上,除了现在不是类;通过函数调用操作符(function calloperator:"()")作用于类名生成一个新的exclass 实例,同样也没有异常参数
* raise exclass, args 同上,但同时提供的异常参数args,可以是一个参数也可以元组
* raise exclass(args) 同上
* raise exclass,args, tb 同上,但提供一个追踪(traceback)对象tb 供使用
* raise exclass,instance 通过实例触发异常(通常是exclass 的实例);如果实例是exclass的子类实例,那么这个新异常的类型会是子类的类型(而不是exclass);如果实例既不是exclass 的实例也不是exclass 子类的实例,那么会复制此实例为异常参数去生成一个新的exclass 实例.
* raise instance 通过实例触发异常: 异常类型是实例的类型; 等价于raise
* instance.__class__, instance (同上).
* raise (1.5 新增)重新触发前一个异常,如果之前没有异常,触发TypeError.

## 4. 断言
断言语句等价于这样的Python 表达式,如果断言成功不采取任何措施(类似语句),否则触发AssertionError(断言错误)的异常.assert 的语法如下:

```python
assert expression[, arguments] 
```

一些演示assert 用法的语句:

```python
assert 1 == 1
assert 2 + 2 == 2 * 2
assert len(['my list', 12]) < 10
assert range(3) == [0, 1, 2] 
```

AssertionError 异常和其他的异常一样可以用try-except 语句块捕捉,但是如果没有捕捉,它将终止程序运行而且提供一个如下的traceback:

    >>> assert 1 == 0
    Traceback (innermost last): File "<stdin>", line 1, in ?
    AssertionError

如同先前章节我们研究的raise 语句,我们可以提供一个异常参数给我们的assert 命令:

    >>> assert 1 == 0, 'One does not equal zero silly!' 
    Traceback (innermost last):
    File "<stdin>", line 1, in ? 
    AssertionError: One does not equal zero silly! 

下面是我们如何用try-except 语句捕获AssertionError 异常:

```python
try:
    assert 1 == 0, 'One does not equal zero silly!'
except AssertionError, args:
    print '%s: %s' % (args.__class__.__name__, args)
```

从命令行执行上面的代码会导致如下的输出:

    AssertionError: One does not equal zero silly!

assert 断言语句在Python 中函数实现类似这样:

```python
def assert(expr, args=None):
    if __debug__ and not expr:
        raise AssertionError, args
```

内建的变量__debug__在通常情况下为True,如果开启优化后为False(命令行选项-O)

## 5. 异常与sys模块
另一种获取异常信息的途径是通过sys 模块中exc_info()函数. 此功能提供了一个3 元组信息 :

```python
try:
    float('abc123')
except:
    import sys
    exc_tuple = sys.exc_info()

print exc_tuple
for eachItem in exc_tuple:
    print eachItem 
```

输出：

    (<type 'exceptions.ValueError'>, ValueError('invalid literal for float(): abc123',), <traceback object at 0xb73aaf54>)
    <type 'exceptions.ValueError'>
    invalid literal for float(): abc123
    <traceback object at 0xb73aaf54>

我们从sys.exc_info()得到的元组中是:

* exc_type: 异常类
* exc_value: 异常类的实例
* exc_traceback: 追踪(traceback)对象

## 6. traceback
traceback模块被用来跟踪异常返回信息.直接打印错误栈信息使用traceback.print_exc()，需要返回一个string对象则使用traceback.format_exc().

```python
import traceback
try:
    float('abc123')
except:
    traceback.print_exc()

try:
    float('abc123')
except:
    err_string = traceback.format_exc()
    # log error string 
```
