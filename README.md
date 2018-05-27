# python-examples
1. Python核心编程（第二版）
    - [cover][0]
    - [异常处理][1]
        - 检测与处理异常
        - 上下文管理与with 语句
        - 触发异常
        - 断言
        - 异常与sys模块
        - traceback
    - [处理时间][2]
        - time.time()
        - time.ctime()
        - time.clock()
        - struct_time
        - 解析与格式化时间
        - 使用Time Zone
    - [产生随机数][3]
        - random.randint()
        - random.randrange()
        - random.random()和random.uniform()
        - random.choice()和random.sample()
        - random.shuffle()
    - [import][24]
    - [socket编程][4]
    - [字符串模板][5]
    - [命令行参数解析optparse][6]
    - [cmd模块][7]
    - [对XML进行操作][8]
    - [参数形式传递函数][9]
    - [多线程编程][10]
    - [base64编码][11]
        - base64编码
        - base64解码
        - URL_Safe变体
    - [重载操作符][12]
    - [格式化输出][13]
    - [序列化][14]
    - [使用chardet检测字符编码][15]
    - [logging][16]
    - [全局变量与global语句][17]
    - [collection小技巧][18]
    - [生成器][19]
    - [可变长参数][20]
    - [闭包编程][23]
    - [实例属性与类属性][21]
    - [元类编程][22]
1. [Python高级编程][30]
    - [语法最佳实践——低于类级][31]
        - 列表推导
        - 迭代器和生成器
        - 装饰器
        - with和contextlib
    - [语法最佳实践——类级][32]
        - 子类化内建函数
        - 访问超类中的方法
        - 描述符和属性
        - 槽
        - 元编程
1. [Programming in Python 3][40]
    - [Data Types][41]
        - IDENTIFIERS AND KEYWORDS
            - dir()
            - `__builtins__`
            - single underscore
            - gettext()
        - INTEGRAL TYPES
            - Integers
                - Numeric Operators and Functions
                - Integer Conversion Functions
                - Integer Bitwise Operators
                - int.bit_length()
        - FLOATING-POINT TYPES
            - Floating-Point Numbers
            - The Math Module’s Functions and Constants
            - Complex Numbers
                - conjugate()
            - Decimal Numbers
        - STRINGS
            - Python’s String Escapes
            - ord()
            - chr()
            - Comparing Strings
                - unicodedata.normalize()
            - Slicing and Striding Strings
            - String Operators and Methods
                - String Methods
                - str.join()
                - * operator
                - str.index()
                - str.count(), str.endswith(), str.find(), str.rfind(), str.index(), str.rindex(), and str.startswith()
                - str.partition(), str.rpartition()
                - str.isdigit()
                - str.translate()
            - String Formatting with the str.format() Method
                - str.format()
                - Field Names
                - Conversions
                - Format Specifications
                - locale
            - Character Encodings
                - str.encode()
                - str.encode()
    - [Collection Data Types][42]
        - SEQUENCE TYPES
            - Tuples
                - Tuple index positions
                - t.count(x)
                - operators + (concatenation), * (replication), and [] (slice), and with in and not in
                - the standard comparison operators (<, <=, ==, !=, >=, >)
            - Named Tuples
            - Lists
                - List index positions
                - List Methods
                - sequence unpacking operator, an asterisk or star (*)
                - starred expressions
                - starred arguments
            - List Comprehensions
        - SET TYPES
            - Sets
                - The standard set operators
                - Set Methods and Operators
            - Set Comprehensions
            - Frozen Sets
                - frozenset()
        - MAPPING TYPES
            - Dictionaries
                - Dictionary Methods
                - dict.items(), dict.keys(), and dict.values() methods all return dictionary views
                - membership operator, in
                - intersection operator
                - dict.get()
            - Dictionary Comprehensions
            - Default Dictionaries
            - Ordered Dictionaries
                - popitem()
                - popitem(last=False)
        - ITERATING AND COPYING COLLECTIONS
            - Iterators and Iterable Operations and Functions
                - `__iter__()`,`__getitem__()`,`__next__()`,StopIteration
                - Common Iterable Operators and Functions
                - all(), any(), len(), min(), max(), and sum()
                - range()
                - zip()
                - sorted(),reversed()
            - Copying Collections
                - operator (=)
                - dict.copy() and set.copy()
                - copy.copy()
                - built-in collection types
                - shallow-copy
                - deep-copy
    - [Control Structures and Functions][43]
        - CONTROL STRUCTURES
            - Conditional Branching
            - Looping
                - while Loops
                - for Loops
        - EXCEPTION HANDLING
            - Catching and Raising Exceptions
                - Some of Python’s exception hierarchy
                - Try ... except ... finally control flows
            - Raising Exceptions
            - Custom Exceptions
        - CUSTOM FUNCTIONS
            - Argument and Parameter Unpacking
            - Accessing Variables in the Global Scope
                - global
            - Lambda Functions
            - Assertions
    - [Object-Oriented Programming][44]
        - THE OBJECT-ORIENTED APPROACH
            - Object-Oriented Concepts and Terminology
                - virtual
        - CUSTOM CLASSES
            - Attributes and Methods
                - Comparison Special Methods
            - Inheritance and Polymorphism
                - `__init__()`
                - super()
            - Using Properties to Control Attribute Access
                - property()
                - @property
                - getter, setter, and docstring
                - getter, setter, and deleter
            - Creating Complete Fully Integrated Data Types
                - Fundamental Special Methods
                - Numeric and Bitwise Special Methods
                - `__del__(self)`
        - CUSTOM COLLECTION CLASSES
            - Collection Special Methods
        - FURTHER OBJECT-ORIENTED PROGRAMMING
            - `__slots__`,`__dict__`
            - Controlling Attribute Access
                - `__getattr__()`, `__setattr__()`, and 
                - Attribute Access Special Methods
                - named tuples
                - read-only properties
                - `__getattr__()`,`__getattribute__()`
                - `__class__`
            - Functors
                - `__call__()`
                - closure
                - people.sort(key=SortKey("surname")),people.sort(key=SortKey("surname", "forename"))
                - operator.attrgetter(),people.sort(key=operator.attrgetter("surname", "forename"))
            - Context Managers
                - `__enter__()` and `__exit__()`
                - with
            - Descriptors
                - `__get__()`, `__set__()`, and `__delete__()`
            - Class Decorators
                - nonlocal
            - Abstract Base 
                - The Numbers Module’s Abstract Base Classes
                - The Collections Module’s Main Abstract Base Classes
                - `__init__()`
                - property is abstract
            - Multiple Inheritance
            - Metaclasses
                - `__new__()`
                - `__init__()`

## Procedural Programming Techniques
- FURTHER PROCEDURAL PROGRAMMING
    - Branching Using Dictionaries
    - Generator Expressions and Functions
    - Dynamic Code Execution and Dynamic Imports
        - Dynamic Code Execution
            - eval()
            - exec()
            - globals()
        - Dynamically Importing Modules
            - Dynamic Programming and Introspection Functions
        - Function and Method Decorators
            - @functools.wraps
        - Function Annotations
            - `__annotations__`
            - type-checking decorator

## Functional-Style Programming
- functools.reduce()
- operator.attrgetter()
- itertools.chain()
- Partial Function Application
    - Coroutines
        - yield
    - Performing Independent Actions on Data
    - Composing Pipelines


[0]: CorePythonProgramming/
[1]: CorePythonProgramming/exception-handling.md
[2]: CorePythonProgramming/time.md
[3]: CorePythonProgramming/random.md
[4]: CorePythonProgramming/socket.md
[5]: CorePythonProgramming/string-template.md
[6]: CorePythonProgramming/optparse.md
[7]: CorePythonProgramming/cmd.md
[8]: CorePythonProgramming/xml.md
[9]: CorePythonProgramming/func_arg.md
[10]: CorePythonProgramming/threading.md
[11]: CorePythonProgramming/base64.md
[12]: CorePythonProgramming/override-op.md
[13]: CorePythonProgramming/print.md
[14]: CorePythonProgramming/pickle.md
[15]: CorePythonProgramming/chardet.md
[16]: CorePythonProgramming/logging.md
[17]: CorePythonProgramming/global.md
[18]: CorePythonProgramming/collection.md
[19]: CorePythonProgramming/gen.md
[20]: CorePythonProgramming/var_arg.md
[21]: CorePythonProgramming/property.md
[22]: CorePythonProgramming/meta-class_programming.md
[23]: CorePythonProgramming/closure.md
[24]: CorePythonProgramming/import.md

[30]: ExpertPythonProgramming/
[31]: ExpertPythonProgramming/SyntaxBestPracticesBelowTheClassLevel.md
[32]: ExpertPythonProgramming/SyntaxBestPracticesAboveTheClassLevel.md


[40]: ProgrammingInPython3/
[41]: ProgrammingInPython3/DataTypes.md
[42]: ProgrammingInPython3/CollectionDataTypes.md
[43]: ProgrammingInPython3/ControlStructuresAndFunctions.md
[44]: ProgrammingInPython3/Object-OrientedProgramming.md