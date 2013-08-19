title: java socket编程-基本概念
date: 2013-08-19 16:21:47
tags: java_socket
---
##### 基本概念及套接字
- 基本概念
- TCP Socket
- UDP Socket 
<!--more--> 

在学习套接字细节之前，有必要了解计算机网络和通信协议的整体框架，这样更便于了解，套接字是使用在什么地方的。
程序是如何通过网络进行通信的? 
Java语言从一开始就是为了人们使用互联网而设计的，它为实现程序间相互通信提供了许多有用的抽象程序接口（Application Programming Interface）,这类应用程序接口称为套接字(Socket)。
#####基本概念 
<strong>信息</strong>（Information）
``` 
信息是指由程序创建和解释的字节序列，在计算机网络环境中，这些字节序列被称为分组报文（packets）.
一组报文包括了网络用来完成工作的控制信息，有时还包括一些用户数据。
```
<strong>协议</strong>（protocol） 
```
相当于是相互通信的程序间达成的一种约定，它规定了分组报文的交换方式合它们包含的意义。
一组协议规定了分组报文的结构以及怎样对报文中所包含的信息进行解析。
设计一组协议，通常是为了在一定约束条件下解决某一特定的问题。
```
<strong>TCP/IP协议族</strong> 主要协议有IP协议、TCP协议、UDP协议。 
在TCP/IP协议族中，底层由基础的通信信道构成，这些信道由网络层使用。而网络层则完成将分组报文传输到它们目的地的工作。 

<strong>IP协议</strong>提供了一种数据报服务：每组分组报文都由网络独立处理和分发，为实现这个功能，每个IP报文必须包含一个保存其目的地址的字段。 
但IP协议只是一个“尽力而为”（best-effort）的协议：它试图分发每一个分组报文，但在网络传输过程中，偶尔也会发生丢失报文，是报文顺序被打乱，或重复发送报文的情况。 

IP协议层之上为传输层，它提供了两种选择的协议<strong>TCP协议</strong>和<strong>UDP协议</strong>。
两种协议都建立在IP层所提供的服务基础之上，但根据应用程序协议的不同需求，它们使用不同的方法来实现不同方式的传输。

TCP协议和IP协议有一个共同的功能，即寻址。 

IP协议只是将分组报文分发到不同的主机，还需要更细粒度的寻址将报文发送到主机中指定的应用程序，因为同一主机上可能有多个应用程序在使用网络。  
TCP协议和IP协议使用的地址叫做端口号（port number）,用来区分同一主机中的不同应用程序。 

TCP协议能够检测和恢复IP层提供的主机到主机的信道中可能发生的报文丢失、重复及其他错误。 

TCP协议提供了一个可信赖的字节流（reliable byte-stream）信道，这样应用程序就不需要再处理上述问题。 

在TCP/IP协议中，由两部分信息来定位一个指定的程序：互联网地址（Internet address)和端口号（port number）.
其中互联网地址由IP协议使用，而附加的端口地址信息由传输协议对其进行解析。  

<strong>回环地址</strong>（loopback address），该地址总是被分配一个特殊的回环接口，回环接口是一种虚拟设备，它的功能只是简单的将发送给它的报文直接回发给发送者。  

<strong>基本套接字</strong> 
getNetworkInterface():获取一个接口列表
<strong>注：</strong> 
（1）这个列表包含主机的所有接口，包括不能向网络中其他主机发送或接收消息的虚拟回环接口。 
（2）同样，列表中可能还包括外部不可到达本地的链接地址。 
（3）这些列表是无序的，所以不能简单的认为第一个接口的第一个地址就可以访问网络，而需要通过InetAddress类的属性检查方法，来判断一个地址是不是回环地址等。 
getInetAddress():获取每个接口的所有地址 
<strong>TCP套接字</strong>
Java为TCP协议提供了两个类：Socket类和ServerSocket类。
一个Socket实例代表了TCP连接的一端，一个TCP连接时一条抽象的双向信道，两端分别由IP地址和端口号确定。
<strong>Socket创建</strong>
```
Socket(InetAddress remoteAddr, int remotePort);
Socket(String remoteHost, int remotePort);
Socket(InetAddress remoteAddr, int remotePort,InetAddress localAddr, int localPort);
Socket(String remoteHost,int remotePort, InetAddress localAddr, int localPort);
Socket();
```
<strong>Socket操作</strong>
```
void connect(SocketAddress destination);
void connect(SocketAddress destination, int timeout);
InputStream getInputStreas();
OutputStream getOutputStream();
void close();
void shutdownInput();
void shutdownOutput();
Socket 获取/检测属性
InetAddress getInetAddress();
int getPort();
InetAddress getLocalAddress();
int getLocalPort();
SocketAddress getRemoteSocket();
SocketAddress getLocalSocketAddress();
InetSocketAddress:创建与访问
InetSocketAddress(InetAddress addr, int port);
InetSocketAddress(int port);
InetSocketAddress(String hostname,int port);
static InetSocketAddress createUnresolved(String host,int port);
boolean isUnresolved();
InetAddress getAddress();
int getPort();
String getHostName();
String toString();
```

<strong>TCP服务器端</strong>
1. 创建一个ServerSocket实例并指定本地端口，此套接字的功能是侦听该指定端口收到的连接。
2. 重复执行：
调用ServerSocket的accept()方法以获取下一个客户端连接。基于新建立的客户端连接，创建一个Socket实例，并由accept()方法返回。
使用所返回的Socket实例的InputStream和OutputStream与客户端进行通信。
通信完成后，使用Socket类

<strong>UDP套接字</strong>
```
DatagramSocket 
public DatagramSocket(int aPort)；
public DatagramSocket(int aPort, InetAddress addr) ；
DatagramPacket
public DatagramPacket(byte[] data, int length)；
public DatagramPacket(byte[] data, int offset, int length)；
public DatagramPacket(byte[] data, int offset, int length, InetAddress host, int aPort)；
public DatagramPacket(byte[] data, int length, InetAddress host, int port)；
send(DatagramPacket packet);
receive(DatagramPacket packet);
```
<strong>Reference</strong>
1.Java TCP/IP Socket编程