title: TCP/IP协议（1）-概要
date: 2013-08-20 00:28:45
tags: tcp/ip
---
####协议、标准
<strong>协议</strong>：一组控制数据通信的规则. 

协议的三要素：语法 syntax、语义 semantics、时序 Timing 

<strong>标准</strong>： 一致同意的规则

标准的种类：  

事实上的标准de-factor   --- 实际或习惯

合法标准 de-jury        --- 法律、法规

####OSI模型和TCP/IP协议族
- OSI参考模型
- OSI模型的层次功能
- TCP/IP协议族
- 编址
- TCP/IP的版本
<!--more-->

#####ISO标准
Open System Interconnection,开放系统互连 
Reference Model,<strong>参考模型</strong>

目的：使两个不同的系统能够通信，不需要改变底层的硬件或软件逻辑。  

ISO是一个组织，OSI是一个模型。 
OSI不是协议，是网络体系结构的<strong>概念模型</strong>。 

#####OSI层次体系结构(7层)
应用支持层(软件): Application、Presentation、Session、 
Transport、 
网络支持层（软/硬件）Network、Data Link、Physical；

#####对等层通信（Peer-to-Peer Communication）
<strong>服务</strong>：下层（提供者）提供给上层（使用者）的操作(功能)。
注：服务只能存在于相邻层之间。

<strong>接口</strong>：相邻层之间通过接口进行交换操作。 

<strong>对等层（peer-layer）</strong>

<strong>对等实体</strong>：在不同系统上，同一层的实体. 

<strong>对等层协议（Peer-to-Peer Protocol）</strong>: 对等层之间通信的规则。

每一层可以同时存在多个实体。
-->  存在通信关系的对等层实体才是对等实体。

每一层可以同时存在多个协议。 
--> 协议是对等实体间的通信规则。 

#####数据通信-封装
<strong>PDU</strong>：Protocol Data Unit,协议数据单元

数据链路层之上，下层数据总是将上层数据+文件头，而数据链路层，不光增加了头部，还增加了尾部，因为物理层传输可能存在差错，所以需要进行校验。

对等层的PDU的专有名词： 

<strong>帧（Frame）</strong>      -  数据链路层的PDU 

<strong>分组（Packet）</strong>   -  网络层的PDU 

<strong>段（Segement）</strong>   - 传输层的PDU 

<strong>数据（Data）</strong>     - 应用层的PDU 

<strong>比特流</strong>           - 物理层


#####OSI模型的层次功能
```
Application: Network processes to applications. 
Presentation: Data representation. 
Session: Inter-host communication. 
Transport: End-to-end connection. 
Network: Addressing and best path. 
Data Link: Access to media. 
Physical: Binary transmission. 
```



