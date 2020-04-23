#	Socket TCP 溝通

server

```python
import socket
import threading
import time
def dealClient(sock, addr):
    print('Accept new connection from %s:%s...' % addr)
    sock.send(b'Hello,I am server!')
    while True:
        data = sock.recv(1024)
        time.sleep(1)
        if not data or data.decode('utf-8') == 'exit':
            break
        # print '-->>%s!' % data.decode('utf-8')
        sock.send(('Loop_Msg: %s!' % data.decode('utf-8')).encode('utf-8'))
    sock.close()
    print('Connection from %s:%s closed.' % addr)
if __name__=="__main__":
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.bind(('127.0.0.1', 9999))
    s.listen(5)
    print('Waiting for connection...')
    while True:
        sock, addr = s.accept()
        t = threading.Thread(target=dealClient, args=(sock, addr))
        t.start()
```

客戶端

```python
import socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(('127.0.0.1', 9999))
print('-->>'+s.recv(1024).decode('utf-8'))
s.send(b'Hello,I am a client')
print('-->>'+s.recv(1024).decode('utf-8'))
s.send(b'exit')
s.close()
```

### 	輸出
server:
>	Waiting for connection...
>
>	Accept new connection from 127.0.0.1:49767...
>
>	Connection from 127.0.0.1:49767 closed.

客戶端
>	-->>Hello,I am server!
>	
>	-->>Loop_Msg: Hello,I am a client!
