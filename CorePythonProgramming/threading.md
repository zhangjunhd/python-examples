使用线程有两种方法，一种是创建线程要执行的函数，把这个函数传递进Thread对象里，让它来执行。

```python
# -*- coding: utf-8 -*-
import threading
import time
import random

count = 0
mutex = threading.Lock()


def thread_main(i):
    global count, mutex
    thread_name = threading.currentThread().getName()

    for x in xrange(0, int(i)):
        mutex.acquire()
        count += 1
        print thread_name, x, count
        mutex.release()
        time.sleep(random.randint(1, 3))


def main(num):
    global count, mutex
    threads = []

    for x in xrange(0, num):
        threads.append(threading.Thread(target=thread_main, args=(3, ))) # loop 3 rounds
    for t in threads:
        t.start()
    for t in threads:
        t.join()

if __name__ == '__main__':
    main(4) # create 4 threads
```

输出：

    Thread-1 0 1
    Thread-2 0 2
    Thread-3 0 3
    Thread-4 0 4
    Thread-1 1 5
    Thread-2 1 6
    Thread-3 1 7
    Thread-4 1 8
    Thread-1 2 9
    Thread-4 2 10
    Thread-3 2 11
    Thread-2 2 12

第二种方法是直接从Thread继承，创建一个新的类，把线程执行的代码放到这个新类里。

```python
# -*- coding: utf-8 -*-
import threading
import time
import random


class Test(threading.Thread):
    def __init__(self, num):
        threading.Thread.__init__(self)
        self.run_num = num

    def run(self):
        global count, mutex
        thread_name = threading.currentThread().getName()

        for x in xrange(0, int(self.run_num)):
            mutex.acquire()
            count += 1
            print thread_name, x, count
            mutex.release()
            time.sleep(random.randint(1, 3))

if __name__ == '__main__':
    global count, mutex
    threads = []
    count = 0
    mutex = threading.Lock()
    for t in xrange(0, 4):  # create 4 threads
        threads.append(Test(3))  # loop 3 rounds
    for t in threads:
        t.start()
    for t in threads:
        t.join()
```

输出：

    Thread-1 0 1
    Thread-2 0 2
    Thread-3 0 3
    Thread-4 0 4
    Thread-1 1 5
    Thread-2 1 6
    Thread-4 1 7
    Thread-3 1 8
    Thread-1 2 9
    Thread-2 2 10
    Thread-3 2 11
    Thread-4 2 12
