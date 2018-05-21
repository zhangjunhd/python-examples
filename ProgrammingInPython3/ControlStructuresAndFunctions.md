# Control Structures and Functions
## CONTROL STRUCTURES
### Conditional Branching
![1](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/160fig01.jpg)

In some cases, we can reduce an if ... else statement down to a `single conditional expression`. The syntax for a conditional expression is:

```py
expression1 if boolean_expression else expression2
```

### Looping
#### while Loops
![2](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/161fig01.jpg)

#### for Loops
![3](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/162fig02.jpg)

## EXCEPTION HANDLING
#### Catching and Raising Exceptions
![4](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/163fig02.jpg)

Figure 4.1 Some of Python’s exception hierarchy

![5](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/04fig01.jpg)

Figure 4.2 Try ... except ... finally control flows

![6](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/04fig02.jpg)

#### Raising Exceptions
![7](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/167fig02.jpg)

#### Custom Exceptions
```py
class exceptionName (baseException): pass
```

![8](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/170fig01.jpg)

## CUSTOM FUNCTIONS
### Argument and Parameter Unpacking
![9](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/180fig01.jpg)

### Accessing Variables in the Global Scope
![10](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/180fig02.jpg)
![10-1](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/180fig03.jpg)

What stands out here is the use of the `global` statement. This statement is used to tell Python that a variable exists at the global (file) scope, and that assignments to the variable should be applied to the global variable, rather than cause a local variable of the same name to be created.

![11](https://www.safaribooksonline.com/library/view/programming-in-python/9780321699909/graphics/181fig01.jpg)

If we did not use the global statement the program would run, but when Python encountered the Language variable in the if statement it would look for it in the local (function) scope, and not finding it would create a new local variable called Language, leaving the global Language unchanged.

### Lambda Functions
```py
lambda parameters: expression

elements.sort(key=lambda e: (e[1], e[2]))
elements.sort(key=lambda e: e[1:3])
elements.sort(key=lambda e: (e[2].lower(), e[1]))
```

### Assertions
```py
assert boolean_expression, optional_expression
```