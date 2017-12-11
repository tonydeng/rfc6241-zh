# 附录F. RFC 4741的变化

本节列出了本文档和[RFC4741 - NETCONF Configuration Protocol](https://tools.ietf.org/html/rfc4741)之间的主要变化。

- 添加了“格式错误的消息”(`malformed-message`)错误标记。

- 将“删除”(`remove`)枚举值添加到“操作”(`operation`)属性。

- 过时了“部分操作”错误标记枚举值。

- 为`<commit>`操作添加了`<persist>`和`<persist-id>`参数。

- 更新了基本协议`URI`并澄清了`<hello>`消息交换，以选择和标识用于特定会话的基本协议版本。

- 添加了一个`YANG`模块来模拟操作，并从`XSD`中删除操作层。

- 澄清候选数据存储的锁定行为。

- 阐明了“操作”(`operation`)属性的“删除”(`delete`)枚举值的错误响应服务器要求。

- 为子树过滤添加了一个名称空间通配符机制。

- 为`<edit-option>`参数添加了一个“`test-only`”值给`<edit-config>`操作。

- 添加了一个`<cancel-commit>`操作。

- 介绍了`NETCONF`用户名和传输协议的要求，以解释如何派生用户名。
