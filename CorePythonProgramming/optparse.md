```python
import optparse
import xmlrpclib

parser = optparse.OptionParser()

parser.add_option('-r', '--address',
            dest="server_address",
            default="http://localhost:8080",
            help="Remote rpc server http address,default is http://localhost:8311",
            )

parser.add_option('-m', '--method',
            dest="method",
            default="list",
            help="rpc method name,default is list,which to list all remote service",
            )

parser.add_option('-p', '--param',
            dest="param",
            default="",
            help="rpc param content,default is null",
            )

options, remainder = parser.parse_args()
if len(remainder) != 0: #means param had not set by parser.add_option
    print '[warning] unknown args : %s' % str(remainder)

proxy = xmlrpclib.ServerProxy(options.server_address)
if options.method == "list":
    print proxy.system.listMethods() # return a list
else:
    print getattr(proxy, options.method)(options.param) #call function by reflection

```

commond line:

    zhangjun@zhangjun:~/workspace/try/src$ python try.py --help
    Usage: try.py [options]

    Options:
      -h, --help            show this help message and exit
      -r SERVER_ADDRESS, --address=SERVER_ADDRESS
                            Remote rpc server http address,default is
                            http://localhost:8311
      -m METHOD, --method=METHOD
                            rpc method name,default is list,which to list all
                            remote service
      -p PARAM, --param=PARAM
                            rpc param content,default is null
