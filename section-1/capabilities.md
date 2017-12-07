# 1.3 能力

`NETCONF`功能是一组补充基本`NETCONF`规范的功能。该能力由统一资源标识符（`URI`）[RFC3986 -  Uniform Resource Identifier (URI): Generic Syntax](https://tools.ietf.org/html/rfc3986)标识。

功能增强了设备的基本操作，描述了额外的操作和内部操作允许的内容。客户端可以发现服务器的功能，并使用这些功能定义的任何附加操作，参数和内容。

能力定义可能会命名一个或多个相关功能。为了支持一个功能，服务器必须支持它所依赖的任何功能。

[第8节](https://tools.ietf.org/html/rfc6241#section-8)定义了允许客户端发现服务器功能的功能交换。[第8节](https://tools.ietf.org/html/rfc6241#section-8)还列出了本文档中定义的一组功能。

可以随时在外部文档中定义额外的功能，从而使这组功能随着时间的推移而扩展。标准组织可以定义标准化的功能，实现可以定义专有的功能。能力`URI`必须充分区分命名权限以避免命名冲突。
