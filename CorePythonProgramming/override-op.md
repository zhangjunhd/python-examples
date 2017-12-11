# 重载操作符

```python
class Pair:
    def __init__(self, a, b):
        self.a = a
        self.b = b

    def __str__(self):
        return 'Pair (%d, %d)' % (self.a, self.b)

    def __add__(self,other):
        return Pair(self.a + other.a, self.b + other.b)

v1 = Pair(2, 10)
v2 = Pair(5, -2)
print v1 + v2  #Pair (7, 8)
```
