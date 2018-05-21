# Collection Data Types
## SEQUENCE TYPES
### Tuples
Figure 3.1 Tuple index positions

![1](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/03fig01.jpg)

Tuples provide just two methods, `t.count(x)`, which returns the number of times object x occurs in tuple t, and `t.index(x)`, which returns the index position of the leftmost occurrence of object x in tuple t—or raises a ValueError exception if there is no x in the tuple.

In addition, tuples can be used with the `operators + (concatenation)`, `* (replication)`, and `[] (slice)`, and with `in` and `not in` to test for membership.

Tuples can be compared using `the standard comparison operators (<, <=, ==, !=, >=, >)`, with the comparisons being applied item by item (and recursively for nested items such as tuples inside tuples).

![2](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/109fig01.jpg)

```py
>>> hair[:2], "gray", hair[2:]
(('black', 'brown'), 'gray', ('blonde', 'red'))

>>> hair[:2] + ("gray",) + hair[2:]
('black', 'brown', 'gray', 'blonde', 'red')
```

### Named Tuples
![3](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/112fig02.jpg)

When it comes to extracting named tuple items for use in strings there are three main approaches we can take.

```py
>>> print("{0} {1}".format(aircraft.manufacturer, aircraft.model))
Airbus A320-200

"{0.manufacturer} {0.model}".format(aircraft)

# Private methods such as namedtuple._asdict() are not guaranteed to be available in all Python 3.x versions
"{manufacturer} {model}".format(**aircraft._asdict())
```

## Lists
Figure 3.2 List index positions

![4](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/03fig02.jpg)

And given this list, L, we can use the slice operator—repeatedly if necessary—to access items in the list:

![5](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/113fig01.jpg)

Table 3.1 List Methods

![6](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/115tab01.jpg)

Any iterable (lists, tuples, etc.) can be unpacked using the `sequence unpacking operator`, an asterisk or star (*).

![7](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/114fig01.jpg)

When the sequence unpacking operator is used like this, the expression *rest, and similar expressions, are called `starred expressions`.

Python also has a related concept called `starred arguments`. For example, if we have the following function that requires three arguments:

```py
def product(a, b, c):
       return a * b * c # here, * is the multiplication operator
```

we can call it with three arguments, or by using starred arguments:

![8](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/114fig02.jpg)

### List Comprehensions
The simplest form of a list comprehension is this:

```py
[item for item in iterable]
```

two general syntaxes for list comprehensions:

```py
[expression for item in iterable]
[expression for item in iterable if condition]

leaps = [y for y in range(1900, 1940)
               if (y % 4 == 0 and y % 100 != 0) or (y % 400 == 0]
```

## SET TYPES
### Sets
Figure 3.4 The standard set operators

![9](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/03fig04.jpg)

Table 3.2 Set Methods and Operators

![10](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/123tab01.jpg)

### Set Comprehensions
Like list comprehensions, two syntaxes are supported:

```py
{expression for item in iterable}
{expression for item in iterable if condition}
```

### Frozen Sets
Frozen sets can only be created using the frozenset data type called as a function. With no arguments, `frozenset()` returns an empty frozen set, with a frozenset argument it returns a shallow copy of the argument, and with any other argument it attempts to convert the given object to a frozenset. 

## MAPPING TYPES
### Dictionaries
![11](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/125fig01.jpg)

Table 3.3 Dictionary Methods

![12](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/129tab01.jpg)

The `dict.items()`, `dict.keys()`, and `dict.values()` methods all return dictionary `views`. Given dictionary view v and set or dictionary view x, the supported operations are:

![13](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/129fig01.jpg)

We can use the `membership operator, in`, to see whether a particular key is in a dictionary, for example, x in d. And we can use the `intersection operator` to see which keys from a given set are in a dictionary. 

![14](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/130fig01.jpg)

We call `dict.get()` with a default value of 0. If the word is already in the dictionary, dict.get() will return the associated number, and this value plus 1 will be set as the item’s new value. If the word is not in the dictionary, dict.get() will return the supplied default of 0.

![15](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/132fig01.jpg)

To make the `dict.setdefault()` method’s functionality clear, here are two equivalent code snippets:

![16](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/133fig01.jpg)

### Dictionary Comprehensions
Like list and set comprehensions, two syntaxes are supported:

```py
{keyexpression: valueexpression for key, value in iterable}
{keyexpression: valueexpression for key, value in iterable if condition}

inverted_d = {v: k for k, v in d.items()}
```

### Default Dictionaries
Here is how the default dictionary is created:

```py
words = collections.defaultdict(int)
words[word] += 1 # 0->1
```

### Ordered Dictionaries
Ordered dictionaries can be used as drop-in replacements for unordered dicts because they provide the same API. The difference between the two is that ordered dictionaries store their items in the order in which they were inserted—a feature that can be very convenient.

We can also call `popitem()` to remove and return the last key–value item in the ordered dictionary; or we can call `popitem(last=False)`, in which case the first item will be removed and returned.

## ITERATING AND COPYING COLLECTIONS
### Iterators and Iterable Operations and Functions
An iterable data type is one that can return each of its items one at a time. Any object that has an `__iter__()` method, or any sequence (i.e., an object that has a `__getitem__()` method taking integer arguments starting from 0) is an iterable and can provide an iterator. An iterator is an object that provides a `__next__()` method which returns each successive item in turn, and raises a `StopIteration` exception when there are no more items.

Table 3.4 Common Iterable Operators and Functions

![17](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/140tab01.jpg)

Here are a couple of usage examples that show `all()`, `any()`, `len()`, `min()`, `max()`, and `sum()`:

![18](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/139fig02.jpg)

If we need a list or tuple of integers, we can convert the iterator returned by `range()` by using the appropriate conversion function.

```py
>>> list(range(5)), list(range(9, 14)), tuple(range(10, -11, -5))
([0, 1, 2, 3, 4], [9, 10, 11, 12, 13], (10, 5, 0, -5, -10))
```

The `zip()` function takes one or more iterables and returns an iterator that returns tuples.

![19](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/143fig01.jpg)

The `sorted()` function returns a list with the items sorted, and the `reversed()` function simply returns an iterator that iterates in the reverse order to the iterator it is given as its argument.

![20](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/144fig01.jpg)

![21](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/144fig02.jpg)

![22](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/145fig02.jpg)

```py
def swap(t):
       return t[1], t[0]

>>> sorted(x, key=swap)
[(3, 'canoe'), (3, 'dorie'), (1, 'kayak'), (1, 'pram')]

sorted(["1.3", -7.5, "5", 4, "-2.4", 1], key=float)
```

### Copying Collections
Since Python uses object references, when we use the assignment `operator (=)`, no copying takes place. If the right-hand operand is a literal such as a string or a number, the left-hand operand is set to be an object reference that refers to the in-memory object that holds the literal’s value. If the right-hand operand is an object reference, the left-hand operand is set to be an object reference that refers to the same object as the right-hand operand.

![23](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/147fig01.jpg)
![23-1](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/147fig02.jpg)

However, in some situations, we really do want a separate copy of the collection (or other mutable object). 

![24](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/147fig03.jpg)

For dictionaries and sets, copying can be achieved using `dict.copy()` and `set.copy()`. In addition, the copy module provides the `copy.copy()` function that returns a copy of the object it is given. Another way to copy the `built-in collection types` is to use the type as a function with the collection to be copied as its argument.

![25](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/147fig04.jpg)

Note, though, that all of these copying techniques are shallow(`shallow-copy`)—that is, only object references are copied and not the objects themselves.

![26](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/148fig01.jpg)

We can `deep-copy`:

![27](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/148fig02.jpg)