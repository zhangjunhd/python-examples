# Object-Oriented Programming
## THE OBJECT-ORIENTED APPROACH
### Object-Oriented Concepts and Terminology
In C++ terminology, all Python methods are `virtual`.

## CUSTOM CLASSES
### Attributes and Methods
![1](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/239fig01.jpg)

![2](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/239fig02.jpg)

Figure 6.2 The Point class’s inheritance hierarchy

![3](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/06fig02.jpg)

Table 6.1 Comparison Special Methods

![4](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/242tab01.jpg)

### Inheritance and Polymorphism
![5](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/243fig02.jpg)
![5-1](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/244fig01.jpg)

Figure 6.3 The Circle class’s inheritance hierarchy

![6](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/06fig03.jpg)

Inside the `__init__()` method we use `super()` to call the base class’s `__init__()` method.

### Using Properties to Control Attribute Access
![7](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/246fig02.jpg)

If we provide only `getters` as we have done here, the properties are `read-only`.

The `property()` decorator function is built-in and takes up to four arguments: a getter function, a setter function, a deleter function, and a docstring. The effect of using `@property` is the same as calling the property() function with just one argument, the getter function. 

![8](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/246fig03.jpg)

To turn an attribute into a readable/writable property we must create a private attribute where the data is actually held and supply getter and setter methods. Here is the radius’s `getter`, `setter`, and `docstring` in full:

![9](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/247fig01.jpg)

Every property that is created has a `getter`, `setter`, and `deleter` attribute, so once the radius property is created using `@property`, the radius.getter, radius.setter, and radius.deleter attributes become available. The radius.getter is set to the getter method by the @property decorator. The other two are set up by Python so that they do nothing (so the attribute cannot be written to or deleted), unless they are used as decorators, in which case they in effect replace themselves with the method they are used to decorate.

### Creating Complete Fully Integrated Data Types
Table 6.2 Fundamental Special Methods

![10](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/250tab01.jpg)

Table 6.3 Numeric and Bitwise Special Methods

![11](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/253tab01.jpg)

The `__del__(self)` special method is called when an object is destroyed—at least in theory. In practice, `__del__()` may never be called, even at program termination. Furthermore, when we write del x, all that happens is that the object reference x is deleted and the count of how many object references refer to the object that was referred to by x is decreased by 1. Only when this count reaches 0 is `__del__()` likely to be called, but Python offers no guarantee that it will ever be called. In view of this, `__del__()` is very rarely reimplemented.

## CUSTOM COLLECTION CLASSES
![12](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/262fig01.jpg)

Figure 6.6 The square_eye.xpm image

![13](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/06fig06.jpg)

![14](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/263fig01.jpg)

![15](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/264fig01.jpg)

![16](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/264fig02.jpg)

![17](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/265fig01.jpg)

![18](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/265fig02.jpg)

Table 6.4 Collection Special Methods

![19](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/265tab01.jpg)

## FURTHER OBJECT-ORIENTED PROGRAMMING
![20](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/363fig01.jpg)

When a class is created without the use of `__slots__`, behind the scenes Python creates a private dictionary called `__dict__` for each instance, and this dictionary holds the instance’s data attributes. This is why we can add or remove attributes from objects.

If we only need objects where we access the original attributes and don’t need to add or remove attributes, we can create classes that don’t have a `__dict__`. This is achieved simply by defining a class attribute called `__slots__` whose value is a tuple of attribute names. 

### Controlling Attribute Access
![21](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/364fig01.jpg)

![22](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/364fig02.jpg)

The class works because we are using the object’s `__dict__` which is what the base class `__getattr__()`, `__setattr__()`, and `__delattr__()` methods use, although here we have used only the base class’s `__getattr__()` method. 

Table 8.2 Attribute Access Special Methods

![23](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/365tab01.jpg)

There is another way of getting constants: We can use `named tuples`. Here are a couple of examples:

![24](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/365fig01.jpg)

We provided access to them using `read-only properties`. For example, we had:

![25](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/365fig02.jpg)

Here is a different solution that handles all the Image class’s read-only properties in a single method:

![26](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/366fig01.jpg)

If we attempt to access an object’s attribute and the attribute is not found, Python will call the `__getattr__()` method (providing it is implemented, and that we have not reimplemented `__getattribute__()`), with the name of the attribute as a parameter. Implementations of `__getattr__()` must raise an AttributeError exception if they do not handle the given attribute.

But instead we have chosen a more compact approach. Since we know that under the hood all of an object’s nonspecial attributes are held in `self.__dict__`, we have chosen to access them directly. For private attributes (those whose name begins with two leading underscores), the name is mangled to have the form `_className__attributeName`, so we must account for this when retrieving the attribute’s value from the object’s private dictionary.

Every object has a `__class__` special attribute, so `self.__class__` is always available inside methods and can safely be accessed by `__getattr__()` without risking unwanted recursion.

Note that there is a subtle difference in that using `__getattr__()` and `self.__class__` provides access to the attribute in the instance’s class (which may be a subclass), but accessing the attribute directly uses the class the attribute is defined in.

One special method that we have not covered is `__getattribute__()`. Whereas the `__getattr__()` method is called last when looking for (nonspecial) attributes, the `__getattribute__()` method is called first for every attribute access. Although it can be useful or even essential in some cases to call `__getattribute__()`, reimplementing the `__getattribute__()` method can be tricky. Reimplementations must be very careful not to call themselves recursively—using `super().__getattribute__()` or `object.__getattribute__()` is often done in such cases. Also, since `__getattribute__()` is called for every attribute access, reimplementing it can easily end up degrading performance compared with direct attribute access or properties. None of the classes presented in this book reimplements `__getattribute__()`.

### Functors
In computer science a functor is an object that can be called as though it were a function, so in Python terms a functor is just another kind of function object. Any class that has a `__call__()` special method is a `functor`.

![27](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/367fig01.jpg)

![28](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/367pro01.jpg)

A `closure` is a function or method that captures some external state.

![29](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/368fig01.jpg)

The classic use case for functors is to provide key functions for sort routines. Here is a generic SortKey functor class (from file SortKey.py):

![30](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/368fig02.jpg)

![31](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/368fig03.jpg)

Suppose we have a list of Person objects in the people list. We can sort the list by surnames like this: `people.sort(key=SortKey("surname"))`. If there are a lot of people there are bound to be some surname clashes, so we can sort by surname, and then by forename within surname, like this: `people.sort(key=SortKey("surname", "forename"))`.

Another way of achieving the same thing, but without needing to create a functor at all, is to use the operator module’s `operator.attrgetter()` function. For example, to sort by surname we could write: `people.sort(key=operator.attrgetter("surname"))`. And similarly, to sort by surname and forename: `people.sort(key=operator.attrgetter("surname", "forename"))`. The `operator.attrgetter()` function returns a function (a closure) that, when called on an object, returns those attributes of the object that were specified when the closure was created.

### Context Managers
Context managers allow us to simplify code by ensuring that certain operations are performed before and after a particular block of code is executed. The behavior is achieved because context managers define two special methods, `__enter__()` and `__exit__()`, that Python treats specially in the scope of a with statement. When a context manager is created in a with statement its `__enter__()` method is automatically called, and when the context manager goes out of scope after its with statement its `__exit__()` method is automatically called.

```py
with expression as variable:
    suite
```

Using nested with statements can quickly lead to a lot of indentation. Fortunately, the standard library’s contextlib module provides some additional support for context managers, including the contextlib.nested() function which allows two or more context managers to be handled in the same with statement rather than having to nest with statements.

![32](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/370fig03.jpg)

### Descriptors
Descriptors are classes which provide access control for the attributes of other classes. Any class that implements one or more of the descriptor special methods, `__get__()`, `__set__()`, and `__delete__()`, is called (and can be used as) a descriptor.

![33](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/373fig01.jpg)

![34](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/373fig02.jpg)

![35](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/374fig01.jpg)

![36](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/374fig02.jpg)

Here’s the start of a modified version of the Point class that makes use of the descriptor (from the ExternalStorage.py file):

![37](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/375fig01.jpg)

![38](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/375fig02.jpg)

To complete our coverage of descriptors we will create the Property descriptor that mimics the behavior of the built-in property() function, at least for setters and getters. 

![39](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/376fig01.jpg)

![40](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/376fig02.jpg)

![41](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/377fig01.jpg)

![42](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/377fig02.jpg)

### Class Decorators
Class decorators take a class object (the result of the class statement), and should return a class—normally a modified version of the class they decorate.

![43](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/378fig02.jpg)

![44](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/378fig03.jpg)

We must use `nonlocal` so that the nested function uses the attribute_name from the outer scope rather than attempting to use one from its own scope. 

Given a class that defines only < (or < and ==), the decorator produces the missing comparison operators by using the following logical equivalences:

![45](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/379fig01.jpg)

```py
@Util.complete_comparisons
class FuzzyBool:
```

![46](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/380pro01.jpg)

### Abstract Base Classes
An abstract base class (ABC) is a class that cannot be used to create objects. Instead, the purpose of such classes is to define interfaces.

Table 8.3 The Numbers Module’s Abstract Base Classes

![47](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/380tab01.jpg)

Table 8.4 The Collections Module’s Main Abstract Base Classes

![48](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/381tab01.jpg)

Here’s the ABC:

![49](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/384fig01.jpg)

We have set the class’s metaclass to be abc.ABCMeta since this is a requirement for ABCs.

- We have made `__init__()` an abstract method to ensure that it is reimplemented.
- The price `property is abstract` (so we cannot use the @property decorator), and is readable/writable.
- We set the private __model data once in the `__init__()` method, and provide read access via the read–only model property.

![50](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/385fig01.jpg)

The Cooker class must reimplement the `__init__()` method and the price property. 

![51](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/385fig02.jpg)

![52](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/385fig03.jpg)

```py
vowel_counter = CharCounter()
vowel_counter("dog fish and cat fish", "aeiou")           # returns: 5
```

![53](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/386fig03.jpg)

![53-1](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/387fig01.jpg)

![54](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/387fig02.jpg)

### Multiple Inheritance
![55](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/388fig01.jpg)

![55-1](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/389fig01.jpg)

![56](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/389fig02.jpg)

### Metaclasses
![57](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/391fig02.jpg)

![58](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/392pro01.jpg)

![59](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/392pro02.jpg)

As an alternative to using the @property and @name.setter decorators, we could create classes where we use a simple naming convention to identify properties. For example, if a class has methods of the form `get_name()` and `set_name()`, we would expect the class to have a private __name property accessed using instance.name for getting and setting. 

![60](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/393fig01.jpg)

![61](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/393fig02.jpg)

![62](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/393fig03.jpg)

![62-1](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/394fig01.jpg)

A metaclass’s `__new__()` class method is called with the metaclass, and the class name, base classes, and dictionary of the class that is to be created. We must use a reimplementation of `__new__()` rather than `__init__()` because we want to change the dictionary before the class is created.