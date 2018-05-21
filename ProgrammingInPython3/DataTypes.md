# Data Types
## IDENTIFIERS AND KEYWORDS
Python has a built-in function called `dir()` that returns a list of an object’s attributes. If it is called with no arguments it returns the list of Python’s built-in attributes. For example:

```py
>>> dir()
['__builtins__', '__doc__', '__loader__', '__name__', '__package__', '__spec__']
>>> dir(__builtins__)
['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException', 'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning', 'ChildProcessError', 'ConnectionAbortedError', 'ConnectionError', 'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning', 'EOFError', 'Ellipsis', 'EnvironmentError', 'Exception', 'False', 'FileExistsError', 'FileNotFoundError', 'FloatingPointError', 'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError', 'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError', 'IsADirectoryError', 'KeyError', 'KeyboardInterrupt', 'LookupError', 'MemoryError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented', 'NotImplementedError', 'OSError', 'OverflowError', 'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError', 'RecursionError', 'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning', 'StopAsyncIteration', 'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError', 'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError', 'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeError', 'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning', 'ValueError', 'Warning', 'ZeroDivisionError', '_', '__build_class__', '__debug__', '__doc__', '__import__', '__loader__', '__name__', '__package__', '__spec__', 'abs', 'all', 'any', 'ascii', 'bin', 'bool', 'bytearray', 'bytes', 'callable', 'chr', 'classmethod', 'compile', 'complex', 'copyright', 'credits', 'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'exit', 'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr', 'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass', 'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview', 'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property', 'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice', 'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars', 'zip']
```

The `__builtins__` attribute is, in effect, a module that holds all of Python’s built-in attributes.

A `single underscore` on its own can be used as an identifier, and inside an interactive interpreter or Python Shell, _ holds the result of the last expression that was evaluated. Some programmers like to use _ in for ... in loops when they don’t care about the items being looped over. For example

```py
>>> for _ in (0, 1, 2, 3, 4, 5):
...     print("Hello")
... 
Hello
Hello
Hello
Hello
Hello
Hello
```

Be aware, however, that those who write programs that are internationalized often use _ as the name of their translation function. They do this so that instead of writing gettext.gettext("Translate me"), they can write _("Translate me"). (For this code to work we must have first imported the get-text module so that we can access the module’s `gettext()` function.)

## INTEGRAL TYPES
Python provides two built-in integral types, int and bool. Both integers and Booleans are immutable.

### Integers
Integer literals are written using base 10 (decimal) by default, but other number bases can be used when this is convenient:

![1](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/056fig01.jpg)

Binary numbers are written with a leading 0b, octal numbers with a leading 0o, and hexadecimal numbers with a leading 0x. Uppercase letters can also be used.

Table 2.2 Numeric Operators and Functions

![2](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/055tab01.jpg)

**For ints**, using a positive **rounding value** has no effect and results in the same number being returned, since there are no decimal digits to work on. But when a negative rounding value is used a subtle and useful behavior is achieved—for example, round(13579, -3) produces 14000, and round(34.8, -1) produces 30.0.

**The first use case is when a data type is called with no arguments.** In this case an object with a default value is created—for example, x = int() creates an integer of value 0. All the built-in types can be called with no arguments.

The second use case is **when the data type is called with a single argument**. If an argument of the same type is given, a new object which is a shallow copy of the original object is created.
- If an argument of a different type is given, a conversion is attempted.
- If the argument is of a type that supports conversions to the given type and the conversion fails, a ValueError exception is raised; otherwise, the resultant object of the given type is returned. 
- If the argument’s data type does not support conversion to the given type a TypeError exception is raised. 

Table 2.3 Integer Conversion Functions

![3](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/055tab02.jpg)

Table 2.4 Integer Bitwise Operators

![4](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/057tab01.jpg)

#### 3.1
From Python 3.1, the `int.bit_length()` method is available. This returns the number of bits required to represent the int it is called on. For example, (2145).bit_length() returns 12.

## FLOATING-POINT TYPES
Python provides three kinds of floating-point values: the built-in float and complex types, and the decimal.Decimal type from the standard library. All three are immutable. 

### Floating-Point Numbers
Here is a simple function for **comparing floats for equality** to the limit of the machine’s accuracy:

```py
def equal_float(a, b):
       return abs(a - b) <= sys.float_info.epsilon
```

**Floating-point numbers can also be represented as strings** in hexadecimal format using the float.hex() method. Such strings can be converted back to floating-point numbers using the float.fromhex() method. For example:

![5](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/061fig01.jpg)

#### 3.x
In addition to the built-in floating-point functionality, the math module provides many more functions that operate on floats, as shown in Tables 2.5 and 2.6. 

Table 2.5 The Math Module’s Functions and Constants #1

![6](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/060tab01.jpg)

Table 2.6 The Math Module’s Functions and Constants #2

![7](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/061tab01.jpg)

### Complex Numbers
The separate parts of a complex are available as attributes real and imag. For example:

![8](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/062fig02.jpg)

complex numbers have a method, `conjugate()`, which changes the sign of the imaginary part. For example:

![9](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/063fig01.jpg)

### Decimal Numbers
To create a Decimal we must import the decimal module. For example:

![10](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/064fig01.jpg)

#### 3.1
In some situations the **difference in accuracy between floats and decimal.Decimals** becomes obvious:

![11](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/064fig02.jpg)

## STRINGS
Strings are represented by the immutable str data type which holds a sequence of Unicode characters.

Table 2.7 Python’s String Escapes

![12](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/066tab01.jpg)

If we want to know the **Unicode code point** (the integer assigned to the character in the Unicode encoding) for a particular character in a string, we can use the built-in `ord()` function. For example:

![13](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/067fig03.jpg)

Similarly, we can convert any integer that represents a valid code point into the corresponding Unicode character using the built-in `chr()` function:

![14](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/068fig01.jpg)

### Comparing Strings
The first problem is that some Unicode characters can be represented by two or more different byte sequences. For example, the character Å (Unicode code point 0x00C5) can be represented in UTF-8 encoded bytes in three different ways: [0xE2, 0x84, 0xAB], [0xC3, 0x85], and [0x41, 0xCC, 0x8A]. Fortunately, we can solve this problem. If we import the unicodedata module and call `unicodedata.normalize()` with "NFKC" as the first argument (this is a normalization method—three others are also available, "NFC", "NFD", and "NFKD"), and a string containing the Å character using any of its valid byte sequences, the function will return a string that when represented as UTF-8 encoded bytes will always be the byte sequence [0xC3, 0x85].

The second problem is that the sorting of some characters is language-specific. One example is that in Swedish ä is sorted after z, whereas in German, ä is sorted as if though were spelled ae. Another example is that although in English we sort ø as if it were o, in Danish and Norwegian it is sorted after z.

In the case of string comparisons, it compares using the strings’ in-memory byte representation. This gives a sort order based on Unicode code points which gives ASCII sorting for English. Lower- or uppercasing all the strings compared produces a more natural English language ordering. Normalizing is unlikely to be needed unless the strings are from external sources like files or network sockets, but even in these cases it probably shouldn’t be done unless there is evidence that it is needed. 

### Slicing and Striding Strings
Figure 2.1 String index positions

![15](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/02fig01.jpg)

The slice operator has three syntaxes:

![16](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/069fig01.jpg)

Figure 2.2 Sequence slicing

![17](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/02fig02.jpg)

Figure 2.3 Sequence striding

![18](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/02fig03.jpg)

Figure 2.4 Sequence slicing and striding

![19](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/02fig04.jpg)

### String Operators and Methods
Table 2.8 String Methods #1

![20](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/073tab01.jpg)

Table 2.9 String Methods #2

![21](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/074tab01.jpg)

Table 2.10 String Methods #3

![22](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/075tab01.jpg)

We have seen that the + operator is overloaded to provide string concatenation. In cases where we want to concatenate lots of strings the `str.join()` method offers a better solution.

![23](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/072fig01.jpg)

The * operator provides string replication:

![24](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/072fig02.jpg)

Using the `str.index()` method often produces cleaner code:

![25](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/075fig01.jpg)

The methods `str.count()`, `str.endswith()`, `str.find()`, `str.rfind()`, `str.index()`, `str.rindex()`, and `str.startswith()` all accept up to two optional arguments: a start position and an end position.

```py
s.count("m", 6) == s[6:].count("m")
s.count("m", 5, -3) == s[5:-3].count("m")
```

Now we will look at another equivalence, this time to help clarify the behavior of `str.partition()`—although we’ll actually use a `str.rpartition()` example:

![26](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/076fig01.jpg)

The is*() methods work on the basis of Unicode character classifications, so for example, calling `str.isdigit()` on the strings "\N{circled digit two}03" and "![27](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/076inline01.jpg)03" returns True for both of them. 

```py
>>> "917.5".isdigit(), "".isdigit(), "-2".isdigit(), "203".isdigit()
(False, False, False, True)
```

The `str.translate()` method takes a translation table as an argument and returns a copy of its string with the characters translated according to the translation table.

![28](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/078fig01.jpg)

### String Formatting with the str.format() Method
The `str.format()` method returns a new string with the replacement fields in its string replaced with its arguments suitably formatted.

```py
>>> "The novel '{0}' was published in {1}".format("Hard Times", 1854)
"The novel 'Hard Times' was published in 1854"
```

If we try to concatenate a string and a number, Python will quite rightly raise a TypeError. But we can easily achieve what we want using `str.format()`:

```py
>>> "{0}{1}".format("The amount due is $", 200)
'The amount due is $200'
```

#### Field Names
![29](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/080fig01.jpg)

Field names may refer to collection data types:

![30](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/080fig02.jpg)

These store key–value items, and since they can be used with str.format(), we’ll just show a quick example here.

![31](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/080fig03.jpg)

We can also access named attributes. Assuming we have imported the math and sys modules, we can do this:

```py
>>> "math.pi=={0.pi} sys.maxunicode=={1.maxunicode}".format(math, sys)
'math.pi==3.14159265359 sys.maxunicode==65535'
```

##### 3.1
From Python 3.1 it is possible to omit field names, in which case Python will in effect put them in for us, using numbers starting from 0. For example:

```py
>>> "{} {} {}".format("Python", "can", "count")
'Python can count'
```

The local variables that are currently in scope are available from the built-in locals() function. 

![32](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/081fig01.jpg)

#### Conversions
Currently there are three such specifiers: s to force string form, r to force representational form, and a to force representational form but only using ASCII characters. Here is an example:

```py
>>> "{0}   {0!s}     {0!r}   {0!a}".format(decimal.Decimal("93.4"))
"93.4     93.4    Decimal('93.4')    Decimal('93.4')"
```

#### Format Specifications
Figure 2.6 The general form of a format specification

![33](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/084tab01.jpg)

![34](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/084fig01.jpg)

![35](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/084fig02.jpg)

##### 3.x
![36](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/085fig01.jpg)

![37](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/085fig02.jpg)

![38](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/086fig01.jpg)

![39](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/086fig02.jpg)

##### 3.1
```py
>>> "{0:,}   {0:*>13,}".format(int(2.39432185e6))
'2,394,321    ****2,394,321'
```

The default locale is called the C `locale`, and for this the decimal and grouping characters are a period and an empty string.

![40](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/087fig01.jpg)

##### 3.x
![41](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/087fig02.jpg)

##### 3.1
```py
>>> "{:,.6f}".format(decimal.Decimal("1234567890.1234567890"))
'1,234,567,890.123457'
```

![42](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/088fig01.jpg)

##### 3.1
Python 3.1 supports formatting complex numbers using the same syntax as for floats:

```py
>>> "{:,.4f}".format(3.59284e6-8.984327843e6j)
'3,592,840.0000-8,984,327.8430j'
```

### Character Encodings
The `str.encode()` method returns a sequence of bytes—actually a bytes object.

![43](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/091fig01.jpg)

The complement of `str.encode()` is bytes.decode() (and bytearray.decode()) which returns a string with the bytes decoded using the given encoding.

```py
>>> print(b"Tage \xc3\x85s\xc3\xa9n".decode("utf8"))
Tage Åsén
>>> print(b"Tage \xc5s\xe9n".decode("latin1"))
Tage Åsén
```