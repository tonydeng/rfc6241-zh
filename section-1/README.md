# 1. 介绍(Introduction)

`NETCONF`协议​​定义了一个简单的机制，通过它可以管理网络设备，可以检索配置数据信息，并且可以上传和操作新的配置数据。该协议允许设备公开一个完整的，正式的应用程序编程接口（`API`）。应用程序可以使用这个直接的`API`来发送和接收完整的和部分的配置数据集。

   `NETCONF`协议​​使用远程过程调用（`RPC`）范例。客户端使用XML编码RPC [[W3C.REC-xml-20001006]](https://tools.ietf.org/html/rfc6241#ref-W3C.REC-xml-20001006)，并使用安全的面向连接的会话将其发送到服务器。服务器以`XML`编码回复。请求和响应的内容在`XML DTD`或`XML`模式或两者中都有充分的描述，使双方都能识别强加在交换上的语法约束。

   `NETCONF`的一个关键方面是它允许管理协议的功能密切地反映设备的本地功能。这降低了实施成本，并且允许及时访问新功能。另外，应用程序可以访问设备本地用户界面的语法和语义内容。

   NETCONF允许客户端发现服务器支持的一组协议扩展。这些“功能”允许客户调整其行为以利用设备暴露的特征。能力定义可以非中心化的方式轻松扩展。标准和非标准功能可以用语义和句法严谨来定义。能力在[第8节](https://tools.ietf.org/html/rfc6241#section-8)讨论。

   NETCONF协议​​是自动配置系统中的一个构建模块。 `XML`是交换的通用语言，为分层内容提供了灵活但完全指定的编码机制。 `NETCONF`可以与基于`XML`的转换技术（如`XSLT` [[W3C.REC-xslt-19991116]](https://tools.ietf.org/html/rfc6241#ref-W3C.REC-xslt-19991116)）配合使用，以提供用于自动生成完整和部分配置的系统。系统可以查询一个或多个数据库以获取有关网络拓扑，链接，策略，客户和服务的数据。这些数据可以使用一个或多个`XSLT`脚本从面向任务的，独立于供应商的数据模式转换成特定于供应商，产品，操作系统和软件版本的形式。结果数据可以通过`NETCONF`协议​​传递给设备。

本文件中的 "`MUST`", "`MUST NOT`", "`REQUIRED`", "`SHALL`", "`SHALL NOT`",   "`SHOULD`", "`SHOULD NOT`", "`RECOMMENDED`", "`MAY`"和"`OPTIONAL`" 按[RFC2119](https://tools.ietf.org/html/rfc2119)中的描述进行解释。
