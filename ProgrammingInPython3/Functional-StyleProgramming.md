# Functional-Style Programming
Reducing involves taking a function and an iterable and producing a single result value.The functools module’s functools.reduce() function supports this. 

![1](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/396pro01.jpg)

If we only wanted to count the .py file sizes we can filter out non-Python files. Here are three ways to do this:

![2](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/396fig01.jpg)

When we want to sort we can specify a key function. This function can be any function, for example, a lambda function, a built-in function or method (such as str.lower()), or a function returned by `operator.attrgetter()`.

```py
L.sort(key=operator.attrgetter("priority")).
```

For example, although it is possible to iterate over two or more lists by concatenating them, an alternative is to use `itertools.chain()` like this:

```py
for value in itertools.chain(data_list1, data_list2, data_list3):
    total += value
```

## Partial Function Application
Partial function application is the creation of a function from an existing function and some arguments to produce a new function that does what the original function did, but with some arguments fixed so that callers don’t have to pass them.

![3](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/398fig01.jpg)

```py
reader = functools.partial(open, mode="rt", encoding="utf8")
writer = functools.partial(open, mode="wt", encoding="utf8")
```

### Coroutines
In Python, a coroutine is a function that takes its input from a `yield` expression. 

### Performing Independent Actions on Data
Coroutines are useful when we want to apply multiple functions to the same pieces of data, or when we want to create data processing pipelines, or when we want to have a master function with slave functions. Coroutines can also be used to provide simpler and lower-overhead alternatives to threading.

![4](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/400fig02.jpg)

![5](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/401fig01.jpg)

![6](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/401fig02.jpg)

![7](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/401fig03.jpg)

![8](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/402fig01.jpg)

![9](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/402fig02.jpg)

### Composing Pipelines
Figure 8.3 A three-step coroutine pipeline processing six items of data

![10](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/403fig01.jpg)