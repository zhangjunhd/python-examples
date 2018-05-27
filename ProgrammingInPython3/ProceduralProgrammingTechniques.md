# Procedural Programming Techniques
## FURTHER PROCEDURAL PROGRAMMING
### Branching Using Dictionaries
![1](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/341fig01.jpg)

![2](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/341fig02.jpg)

### Generator Expressions and Functions
```py
(expression for item in iterable)
(expression for item in iterable if condition)
```

![3](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/342fig01.jpg)

### Dynamic Code Execution and Dynamic Imports
#### Dynamic Code Execution
The easiest way to execute an expression is to use the built-in `eval()` function.

```py
x = eval("(2 ** 31) - 1")         # x == 2147483647
```

This is fine for user-entered expressions, but what if we need to create a function dynamically? For that we can use the built-in `exec()` function.

![4](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/345fig01.jpg)

In some cases it is convenient to provide the entire global context to exec(). This can be done by passing the dictionary returned by the `globals()` function. One disadvantage of this approach is that any objects created in the exec() call would be added to the global dictionary. A solution is to copy the global context into a dictionary, for example, context = globals().copy().

We can also pass the local context, for example, by passing `locals()` as a third argument—this makes objects in the local scope accessible to the code executed by exec().

#### Dynamically Importing Modules
Table 8.1 Dynamic Programming and Introspection Functions

![5](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/349tab01.jpg)

### Function and Method Decorators
![6](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/357fig01.jpg)

![7](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/357fig02.jpg)

Here is a slightly cleaner version of the @positive_result decorator. The wrapper itself is wrapped using the functools module’s `@functools.wraps` decorator, which ensures that the wrapper() function has the name and docstring of the original function.

![8](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/357fig03.jpg)

We can call a function with the parameters we want and that returns a decorator which can then decorate the function that follows it.

![9](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/358fig01.jpg)

![10](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/358fig02.jpg)

It is a logging function that records the name, arguments, and result of any function it is used to decorate. For example:

![11](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/358fig03.jpg)

![12](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/359fig01.jpg)

![13](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/359fig02.jpg)

### Function Annotations
Here’s the general syntax:

```py
def functionName(par1 : exp1, par2 : exp2, ..., parN : expN) -> rexp:
      suite
```

If annotations are present they are added to the function’s `__annotations__` dictionary; if they are not present this dictionary is empty. 

Here is an example of an annotated function that is in the Util module:

![14](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/361fig01.jpg)

![15](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/361fig02.jpg)

If we want to give meaning to annotations, for example, to provide type checking, one approach is to decorate the functions we want the meaning to apply to with a suitable decorator. Here is a very basic `type-checking decorator`:

![16](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/361fig03.jpg)