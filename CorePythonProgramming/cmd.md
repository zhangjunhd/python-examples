cmd模块为命令行接口(command-line interfaces, CLI)提供了一个简单的框架，可以在自己的程序中使用它来创建自己的命令行程序。
```python
# -*- coding: utf-8 -*-
import cmd
import sys

class CLI(cmd.Cmd):

    def __init__(self):
        cmd.Cmd.__init__(self)
        self.prompt = '> '  # 定义命令行操作符

    def do_hello(self, arg):    # 定义hello命令所执行的操作
        print "hello again", arg, "!"

    def help_hello(self):   # 定义hello命令的帮助输出
        print "syntax: hello [message]",
        print "-- prints a hello message"

    def do_quit(self, arg): # 定义quit命令所执行的操作
        sys.exit(1)

    def help_quit(self):    # 定义quit命令的帮助输出
        print "syntax: quit",
        print "-- terminates the application"

    # 定义quit的快捷方式
    do_q = do_quit

# 创建CLI实例并运行
cli = CLI()
cli.cmdloop()
```

终端执行：

    > help hello
    syntax: hello [message] -- prints a hello message
    > hello python
    hello again python !
    > haha
    *** Unknown syntax: haha
    > q
