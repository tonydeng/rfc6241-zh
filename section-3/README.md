# 3. XML注意事项(XML Considerations)

`XML`作为`NETCONF`的编码格式，允许以文本格式表示复杂的分层数据，可以使用传统的文本工具和特定于`XML`的工具来读取，保存和操作文本格式。

所有`NETCONF`消息必须是格式良好的`XML`，用`UTF-8` [[RFC3629 - UTF-8, a transformation format of ISO 10646]](https://tools.ietf.org/html/rfc3629)编码。 如果一个`peer`接收到一个`<rpc>`消息，这个消息不是格式良好的`XML`或者没有用`UTF-8`编码，它应该回复一个“格式错误的消息()”错误。 如果回复不能以任何理由发送，服务器必须终止会话。

`NETCONF`消息可以以`XML`声明开始（参见[W3C.REC-xml-20001006](https://tools.ietf.org/html/rfc6241#ref-W3C.REC-xml-20001006)的[第2.8节](https://www.w3.org/TR/2000/REC-xml-20001006#sec-prolog-dtd)）。

本节讨论与`NETCONF`有关的少量`XML`相关考虑。
