# 全局变量与global语句
如果将全局变量的名字声明在一个函数体内的时候，全局变量的名字能被局部变量给覆盖掉。

code1:

    bar = 100

    def foo():
        print "\ncalling foo()..."
        bar = 200
        print "in foo(), bar is", bar

    print "in __main__, bar is", bar
    foo()
    print "\nin __main__, bar is (still)", bar
输出：

    in __main__, bar is 100
    calling foo()...
    in foo(), bar is 200
    in __main__, bar is (still) 100

在foo()函数中局部的bar将全局的bar"覆盖掉了"。

code2:

    bar = 100

    def foo():
        print "\ncalling foo()..."
        #bar = 200
        print "in foo(), bar is", bar

    if __name__ == '__main__' :
        print "in __main__, bar is", bar
        foo()
        print "\nin __main__, bar is (still)", bar

输出：

    in __main__, bar is 100

    calling foo()...
    in foo(), bar is 100

    in __main__, bar is (still) 100
这里将foo()函数内对bar赋值语句注释掉，发现函数内部可以访问全局的bar。

code 3:

    bar = 100

    def foo():
        print "\ncalling foo()..."
        bar += 1
        print "in foo(), bar is", bar

    if __name__ == '__main__' :
        print "in __main__, bar is", bar
        foo()
        print "\nin __main__, bar is (still)", bar
输出：

    in __main__, bar is 100

    calling foo()...
    Traceback (most recent call last):
      File "/home/zhangjun/workspace/try/src/try.py", line 10, in <module>
        foo()
      File "/home/zhangjun/workspace/try/src/try.py", line 5, in foo
        bar += 1
    UnboundLocalError: local variable 'bar' referenced before assignment
这里在foo()函数内部，对bar变量值进行修改，发现修改的其实被视为一个局部变量。

code 4:

    bar = 100

    def foo():
        print "\ncalling foo()..."
        global bar
        bar += 1
        print "in foo(), bar is", bar

    if __name__ == '__main__' :
        print "in __main__, bar is", bar
        foo()
        print "\nin __main__, bar is (still)", bar
输出：

    in __main__, bar is 100

    calling foo()...
    in foo(), bar is 101

    in __main__, bar is (still) 101
此时，在foo()函数内，我们首先对变量bar`进行global声明`，这样就可以顺利修改这个全局变量了。
