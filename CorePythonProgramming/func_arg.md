convert()函数传入一个内建函数int(),long(), 或者float()来执行转换:

```python
#!/usr/bin/env python

def convert(func, seq):
    'conv. sequence of numbers to same type'
    return [func(eachNum) for eachNum in seq]

myseq = (123, 45.67, -6.2e8, 999999999L)
print convert(int, myseq)
print convert(long, myseq)
print convert(float, myseq)
```

输出：
    
    [123, 45, -620000000, 999999999]
    [123L, 45L, -620000000L, 999999999L]
    [123.0, 45.670000000000002, -620000000.0, 999999999.0]
