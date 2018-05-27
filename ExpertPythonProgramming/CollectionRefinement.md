## 在一个有序list中进行二分查找
find.py

```py
from bisect import bisect

def find(seq, el):
    pos = bisect(seq, el)
    if pos == 0 or (pos == len(seq) and seq[-1] != el):
        return -1
    return pos - 1

seq = [2, 3, 7, 8, 9]
find(seq, 9)  # 4
find(seq, 10)  # -1
find(seq, 0)  # -1
find(seq, 7)  # 2
```

## 使用set保存无重复key的list 
my_set.py

```py
seq = ['a', 'a', 'b', 'c', 'c', 'd']
res = set(seq)
print res  # set(['a', 'c', 'b', 'd'])
```

## deque
my_deque.py

```py
from collections import deque

d = deque()
d.append('1')
d.append('2')
d.append('3')

print d  # deque(['1', '2', '3'])
print len(d)   # 3
print d[0]  # 1
print d[-1]  # 3

d.pop()
d.popleft()
print d  # deque(['2'])
d.extendleft(['1', '0'])
d.extend(['3', '4', '5'])
print d  # deque(['0', '1', '2', '3', '4', '5'])
```

## defaultdict
my_defaultdict.py

```py
from collections import defaultdict


class Person(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return self.name

    def __repr__(self):
        return self.name


persons = [Person('bob', 20), Person('jack', 20), Person('mary', 22)]

persons_by_age = defaultdict(list)

for person in persons:
    persons_by_age[person.age].append(person)


for k, v in persons_by_age.items():
    print ':'.join([str(k), str(v)])

# 20:[bob, jack]
# 22:[mary]
```

## namedtuple
my_namedtuple.py

```py
Bob = ('bob', 30, 'male')
print 'Representation:', Bob  # Representation: ('bob', 30, 'male')

Jane = ('Jane', 29, 'female')
print 'Field by index:', Jane[0]  # Field by index: Jane

for people in [Bob, Jane]:
    print "%s is %d years old %s" % people
# bob is 30 years old male
# Jane is 29 years old female

from collections import namedtuple
Person = namedtuple('Person', 'name age gender')
print 'Type of Person:', type(Person)  # Type of Person: <type 'type'>

Bob = Person(name='Bob', age=30, gender='male')
print 'Representation:', Bob  # Representation: Person(name='Bob', age=30, gender='male')

Jane = Person(name='Jane', age=29, gender='female')
print 'Field by Name:', Jane.name  # Field by Name: Jane

for people in [Bob, Jane]:
    print "%s is %d years old %s" % people
# Bob is 30 years old male
# Jane is 29 years old female
```