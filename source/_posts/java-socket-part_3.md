title: java socket编程-多任务处理
date: 2013-08-22 21:35:56
tags: java_socket
---
####多任务处理
- 一客户一线程 （thread-per-client）-
- 线程池（thread-pool）
- 多接收者（广播、多播）
<!--more-->
#####Java多线程 
1) 继承 Thread 类，只适用于没有继承其他类的类； 

2) 实现Runnable接口的类;

注：新线程创建后并不立即执行，而是等到其start()方法被调用。
	当Thread对象的start()方法被调用时，Java虚拟机将一个新的线程中执行对象的run()方法，从而实现与其他任务并行执行。 

#####一客户一线程 

#####线程池
每个新线程都会消耗系统资源：创建一个线程将占用CPU周期，而且每个线程都有自己的数据结构，也要消耗系统内存。
 
使用线程池，通过限制总线程数并重复使用线程可以避免上面的问题。与每个连接创建一个新的线程不同，服务器在启动时创建一个由固定数量组成的线程池。 

系统管理调用：Executor接口 

ExecutorService接口

```
Executor service = Executors.newFixedThreadPool(threadPoolSize);
Executor service = Executors.newSingleThreadExecutor();
```

#####阻塞和超时
Socket的I/O调用可能会因为多种原因而阻塞。
 
调用一个已经阻塞的方法将使应用程序停止（并使运行它的线程无效）。
1.accetp(), read() 和receiver()
  对于这些方法，可以使用Socket类、ServerSocket类和DatagramSocket类的 setSoTimeout()方法，设置器阻塞的最长时间（毫秒）。 
  对于Socket实例，在调用read()方法前，还可以使用该套接字的InputStream的avaiable()方法来检测是否有可读的数据。 

2.连接和写数据

Socket类的构造函数会尝试根据参数中指定的主机和端口来建立连接，并阻塞等待，直到连接成功建立或发生系统定义的超时。 
  而系统定义的超时时间很长，Java又没有提供任何缩短它的方法。
  
 <strong>解决办法</strong>：
 
使用Socket类的无参数构造函数，它返回的是一个没有建立连接的Socket实例，需要建立连接时，调用该实例的connect()方法，并指定一个远程终端盒超时时间（毫秒）.

  write()方法 调用也会阻塞等待，直到最后一个字节成功写入到TCP实现的本地缓存中。 

#####多接收者
以单播（unicast）的方式发送信息给多个接收者，效率可能非常低，因为同样的数据发送了多次，在一个网络连接上单播同一数据的多个副本非常浪费带宽。 
实际上，如果以固定频率发送数据，网络连接带宽就已经限制了其锁支持的接收者数量。
例如：如果视频服务器以1Mbps的速率发送数据流，而其网络连接速率是3Mbps（良好的连接速率），那么该连接最多只能同时支持3个用户。

网络本身提供了一个更有效使用带宽的方法，将复制数据包的工作交个网络来做，而不是发送者负责。
 
<strong>广播（broadcast）</strong>、<strong>多播(multicast)</strong>。

#####多播
IPv4多播地址范围：224.0.0.0 ~ 239.255.255.255 

Java中多播应用程序主要是通过MulticastSock实例进行通信，它是一个DatagramSocket的一个子类。 
重点需要理解的是，一个MulticastSocket实例实际上就是一个UDP套接字（DatagramSocket），其包含了一些额外的可以控制的多播特定属性。 

多播数据报文实际上可以通过DatagramSocket中发送，只需要简单地指定一个多播地址。 

不过MuiltcastSocket还有一些DatagramSocket没有的能力： 

（1）允许指定数据报文的TTL

（2）允许指定和改变通过哪个接口将数据报文发送到组（接口由其互联网地址确定）。

注：一个多播消息接收者必须使用MulticastSocket来接收数据，因为它需要用到MulticastSocket加入组的功能。 

MulticastSocket 创建：
```
MulticastSocket()
MulticastSocket(int localPort)
MulticastSocket(SocketAddress bindaddr)
```
MulitcastSocket 组管理：
```
void joinGroup(InetAddress groupAddress);
void joinGroup(SocketAddress mcastaddr, NetworkInterface netIf);
void leaveGroup(InetAddress groupAddress);
void leaveGroup(SocketAddress mcastaddr,NetworkInterface netIf);
```
MulticastSocket 设置/获取多播选项
```
int getTimeToLive()
void setTimeToLive(int ttl)
boolean getLoopbackMode()
void setLoopbackMode(boolean disable)
InetAddress getInterface()
NetworkInterface getNetworkInterface()
void setInterface(InetAddress inf)
void setNetworkInterface(NetworkInterface netIf)
```
#####控制默认行为 
TCP/IP协议的开发者用了大量的时间来考虑协议的默认行为，以满足大部分应用程序的需要。对于大多数应用程序来说，这些设计都非常适合。
如默认情况下，DatagramSocket类的receive()方法将无限期地阻塞等待一个数据报文，某些情况下，并不希望这样，所以，可同通过UDP套接字提供的接口改变协议的默认行为，如设置超时时间:setSoTimeout();

######Keep-Alive 
默认情况下，keep-alive机制是关闭的，通过setKeepAlive()方法将其设置为true来开启keep-alive机制。
```
boolean getKeepAlive();
void setKeepAlive(boolean on);
```
#####发送和接收缓存区的大小 
一旦创建了一个Socket或DatagramSocket实例，操作系统就必须为其分配缓存区以存放接收的和要发送的数据。 
Socket,DatagramSocket: 设置和获取发送接收缓存区大小
```
int getReceiveBufferSize();
void setReceiveBufferSize(int size);
int getSendBufferSize();
void setSendBufferSize(int size);
```
Socket,ServerSocket,DatagramSocket： 设置/获取I/O超时时间
```
int getSoTimeout();
void setSoTimeout(int timeout);
```
Socket,ServerSocket,DatagramSocket: 设置/获取地址重用
```
boolean getReuseAddress()
void setReuseAddress(boolean on);
```
Socket: 设置/获取TCP缓冲延迟
```
boolean getTCPNoDelay();
void setTcpNoDelay(boolean on);
```
Socket:在close()方法停留
```
int getSoLinger();
void setSoLinger(boolean on, int linger);
```

#####Reference
1.Java TCP/IP Socket编程
