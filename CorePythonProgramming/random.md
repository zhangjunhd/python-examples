## random.randint()
可以通过参数限定生成随机整数的范围。下面的程序随机生成一个1~9的整数。

```python
# -*- coding: utf-8 -*-
import random
print 'generate a random integer (1~9):%d' % random.randint(1, 9) #generate a random integer (1~9):3
```

## random.randrange()
通过改变第三个参数便可以更加灵活地生成想要的随机数。如当第三个参数为10时，生成的随机数便是10的倍数。下面的例子会生成指定范围内的偶数。

```python
# -*- coding: utf-8 -*-
import random
print 'generate a random even number  (20~200):%d' % random.randrange(20, 201, 2) #generate a random even number  (20~200):44
```

## random.random()和random.uniform()
这两个函数可以生成随机浮点数。

```python
# -*- coding: utf-8 -*-
import random
print 'generate a float (0~1):%f' % random.random() #generate a float (0~1):0.941344
print'generate a float (1~20):%f' % random.uniform(1, 20) #generate a float (1~20):15.957946
```

## random.choice()和random.sample()
这两个函数用于生成随机字符和字符串。

```python
# -*- coding: utf-8 -*-
import random
import string

print 'generate a random character (a~z):%c' % random.choice('abcdefghijklmopqrstuvwxyz') #generate a random character (a~z):k
print 'generate a random string (spring,summer,fall,winter):%s' % random.choice(['spring', 'summer', 'fall', 'winter']) #generate a random string (spring,summer,fall,winter):fall
print 'generate a random string:%s' % string.join(random.sample('abcdefghijklmopqrstuvwxyz', 4), '') #generate a random string:oegu
```

## random.shuffle()
此函数用于乱序操作。

```python
# -*- coding: utf-8 -*-
import random
items = [1,2,3,4,5,6,7,8,9,0]
print '%s' % items #[1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
random.shuffle(items)
print '%s' % items #[5, 6, 4, 1, 3, 2, 9, 7, 8, 0]
```
