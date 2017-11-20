## server.py

```python
# -*- coding: utf-8 -*-
import socket

host = '127.0.0.1'
port = 5678

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # tcp
s.bind((host, port))
s.listen(1)

while True:
    clientSocket, clientAddress = s.accept()
    print 'Client connected from', clientAddress  # Client connected from ('127.0.0.1', 51927)
    clientSocket.sendall('Welcome to Python World!')
    clientSocket.close()
```

## client.py

```python
# -*- coding: utf-8 -*-
import socket

host = '127.0.0.1'
port = 5678

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # tcp
s.connect((host, port))

while True:
    buf = s.recv(2048)
    if not len(buf):
        break
    print buf  # Welcome to Python World!
```
