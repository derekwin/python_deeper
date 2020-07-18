

### python - HTTP - Requests

---

Request库

**https://requests.readthedocs.io/en/master/**







### python - socket编程

---

> Python 提供了两个级别访问的网络服务。：
>
> - 低级别的网络服务支持基本的 Socket，它提供了标准的 BSD Sockets API，可以访问底层操作系统Socket接口的全部方法。
> - 高级别的网络服务模块 SocketServer， 它提供了服务器中心类，可以简化网络服务器的开发。

#### AF_UNIX域socket通信

本地IPC，类似于管道，依赖路径名标识发送方和接收方。即发送数据时，指定接收方绑定的路径名，操作系统根据该路径名可以直接找到对应的接收方，并将原始数据直接拷贝到接收方的内核缓冲区中，并上报给接收方进程进行处理。同样的接收方可以从收到的数据包中获取到发送方的路径名，并通过此路径名向其发送数据。

![](C:\Users\seclee\Desktop\python_deeper\python_deeper\chapter_9.assets\20141105212926515.png)



####  AF_INET域socket通信

典型的TCP/IP四层模型的通信过程。

发送方、接收方依赖IP:Port来标识，即将本地的socket绑定到对应的IP端口上，发送数据时，指定对方的IP端口，经过Internet，可以根据此IP端口最终找到接收方；接收数据时，可以从数据包中获取到发送方的IP端口。

发送方通过系统调用send()将原始数据发送到操作系统内核缓冲区中。内核缓冲区从上到下依次经过TCP层、IP层、链路层的编码，分别添加对应的头部信息，经过网卡将一个数据包发送到网络中。经过网络路由到接收方的网卡。网卡通过系统中断将数据包通知到接收方的操作系统，再沿着发送方编码的反方向进行解码，即依次经过链路层、IP层、TCP层去除头部、检查校验等，最终将原始数据上报到接收方进程。
![](C:\Users\seclee\Desktop\python_deeper\python_deeper\chapter_9.assets\20141105212647791.png)



#### socket库

```python
import socket
```

###### 初始化连接对象

```python
s = socket.socket(socket.AF_INET,socket.SOCK_STREAM，protocol=0)
```

> 协议 : AF_INET (ipv4) , AF_INET6, AF_UNIX , AF_UNSPEC(未指定)
>
> type : socket.SOCK_STREAM(面向连接) , socket.SOCK_DGRAM(非连接),socket.SOCK_RAW(IP协议的数据报接口) , socket.SOCK_SEQPACKET(固定长度的，有序的，可靠的，面向连接的报文传递)
>
> protocol用于指定协议簇，默认是0.

socket.SOCK_STREAM 面向连接的默认协议(0) 是TCP协议，socket.SOCK_DGRAM => UDP

###### 函数

> https://blog.csdn.net/rebelqsp/article/details/22109925

服务端函数

- s.bind(address) 将套接字绑定到地址,AF_INET=> (host,port)
- s.listen(backlog) 最大连接数
- s.accept()  接受TCP连接，返回（conn，address），conn 套接字对象，收发数据，address 客户端地址

客户端函数

- s.connect(address)  连接到address的套接字，一般用元组（hostname，port），错误返回socket.error
- s.connect_ex(address) 类似与connect(address),但是当遇到c语言层的异常时，并不会抛出异常，而是返回一个错误指示器。但其他异常如host not found还是会抛出异常，操作成功时，错误指示器的值是0，否则是不确定的值。

公共函数

[TCP]

- s.recv(bufsize[,flag])  接受TCP套接字的数据。数据以字符串形式返回，bufsize指定要接收的最大数据量。flag提供有关消息的其他信息，通常可以忽略。
- s.send(string[,flag])   发送TCP数据。将string中的数据发送到连接的套接字。返回值是要发送的字节数量，该数量可能小于string的字节大小。
- s.sendall(string[,flag])  完整发送TCP数据。将string中的数据发送到连接的套接字，但在返回之前会尝试发送所有数据。成功返回None，失败则抛出异常。

[UDP]

- s.recvfrom(bufsize[.flag])  接受UDP套接字的数据。与recv()类似，但返回值是（data,address）。其中data是包含接收数据的字符串，address是发送数据的套接字地址。
- s.sendto(string[,flag],address)  发送UDP数据。将数据发送到套接字，address是形式为（ipaddr，port）的元组，指定远程地址。返回值是发送的字节数。



- s.close()

- s.getpeername()  返回远端地址 元组
- s.getsockname()  返回套接字的地址 元组
- s.setsockopt(level,optname,value)  设置给定套接字选项的值。
- s.getsockopt(level,optname[.buflen])  返回套接字选项的值。
- s.settimeout(timeout)  设置套接字操作的超时期，timeout是一个浮点数，单位是秒。值为None表示没有超时期。一般，超时期应该在刚创建套接字时设置，因为它们可能用于连接的操作（如connect()）
- s.gettimeout()  返回当前超时期的值，单位是秒，如果没有设置超时期，则返回None。

- s.fileno()  返回套接字的文件描述符。
- s.setblocking(flag)  如果flag为0，则将套接字设为非阻塞模式，否则将套接字设为阻塞模式（默认值）。非阻塞模式下，如果调用recv()没有发现任何数据，或send()调用无法立即发送数据，那么将引起socket.error异常。
- s.gethostname() 本机的hostname
- s.getbyhostname(hostname) 返回ip



### socketserver

```python
class socketserver.TCPServer(server_address, RequestHandlerClass, bind_and_activate=True)
class socketserver.UDPServer(server_address, RequestHandlerClass, bind_and_activate=True)
class socketserver.UnixStreamServer(server_address, RequestHandlerClass, bind_and_activate=True)
class socketserver.UnixDatagramServer(server_address, RequestHandlerClass,bind_and_activate=True)
```





### python中的字节流处理

```python
s.send(bytes)
```

string to bytes

```python
b'string'
'string'.encode()
```

num to bytes

```python
'{}'.format(num).encode()
```











### javascript - websocket









### python 原生实现 rest 接口





### python 原生实现 RPC



### 异步网络编程

async

await