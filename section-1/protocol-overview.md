# 1.2 协议概述

`NETCONF`使用简单的基于`RPC`的机制来促进客户端和服务器之间的通信。 客户端可以是通常作为网络管理器的一部分运行的脚本或应用程序。 服务器通常是一个网络设备。 术语“设备(`device`)”和“服务器(`server`)”在本文档中可互换使用，“客户端(`client`)”和“应用(`application`)”也是如此。

`NETCONF`会话是网络管理员或网络配置应用程序与网络设备之间的逻辑连接。 设备必须至少支持一个`NETCONF`会话，并且应该支持多个会话。 全局配置属性可以在任何授权的会话中更改，并且效果在所有会话中都可见。 会话特定的属性只影响它们被改变的会话。

NETCONF可以从概念上分为四层，如图1所示。


```
            Layer                 Example
       +-------------+      +-----------------+      +----------------+
   (4) |   Content   |      |  Configuration  |      |  Notification  |
       |             |      |      data       |      |      data      |
       +-------------+      +-----------------+      +----------------+
              |                       |                      |
       +-------------+      +-----------------+              |
   (3) | Operations  |      |  <edit-config>  |              |
       |             |      |                 |              |
       +-------------+      +-----------------+              |
              |                       |                      |
       +-------------+      +-----------------+      +----------------+
   (2) |  Messages   |      |     <rpc>,      |      | <notification> |
       |             |      |   <rpc-reply>   |      |                |
       +-------------+      +-----------------+      +----------------+
              |                       |                      |
       +-------------+      +-----------------------------------------+
   (1) |   Secure    |      |  SSH, TLS, BEEP/TLS, SOAP/HTTP/TLS, ... |
       |  Transport  |      |                                         |
       +-------------+      +-----------------------------------------+
```
> 图1: `NETCONF`协议分层(`NETCONF Protocol Layers`)

1. 安全传输(`Secure Transport`)层提供客户端和服务器之间的通信路径。 NETCONF可以通过任何提供一组基本要求的传输协议进行分层。 [第2节](https://tools.ietf.org/html/rfc6241#section-2)讨论这些要求。

2. 消息(`Messages`)层为编码`RPC`和通知提供了一个简单的，与传输无关的成帧机制。 [第4节](https://tools.ietf.org/html/rfc6241#section-4)记录`RPC`消息，[RFC5717 - Partial Lock Remote Procedure Call (RPC) for NETCONF](https://tools.ietf.org/html/rfc5717)记录通知。

3. 操作(`Operations`)层定义了一组使用`XML`编码参数作为`RPC`方法调用的基本协议操作。 [第7节](https://tools.ietf.org/html/rfc6241#section-7)详细列出了基本协议操作。

4. 内容(`Content`)层不在本文的范围之内。 预计将分别开展标准化NETCONF数据模型的工作。

`YANG`数据建模语言[RFC6020 - YANG - A Data Modeling Language for the Network Configuration Protocol (NETCONF)](https://tools.ietf.org/html/rfc6020)是为指定`NETCONF`数据模型和协议操作而开发的，涵盖了图1的操作和内容层。
