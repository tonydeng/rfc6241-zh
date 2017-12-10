# 8. 能力

本节定义了客户端或服务器可以实现的一组能力。每个对等点(`peer`)通过在初始能力交换期间发送它们来告知它的能力。每个对等体(`peer`)只需要了解它可能使用的那些能力，而且必须忽略从另一个对等体(`peer`)接收到的任何不需要或不理解的能力。

可以使用[附录D](https://tools.ietf.org/html/rfc6241#appendix-D)中的模板来定义其他功能。将来的功能定义可以作为标准发布为标准，或作为专有扩展发布。

一个`NETCONF`能力用一个`URI`来标识。使用`URN`按照[RFC3553 - An IETF URN Sub-namespace for Registered Protocol Parameters](https://tools.ietf.org/html/rfc3553)中描述的方法定义基本功能。本文档中定义的功能具有以下格式：

> urn:ietf:params:netconf:capability:{name}:1.x

其中`{name}`是能力的名称。如果能力存在于多个版本中，则通常会在讨论和电子邮件中使用简写`:{name}`或`:{name}:{version}`引用功能。例如，`foo`能力将具有正式名称"`urn:ietf:params:netconf:capability:foo:1.0`"并被称为“`:foo`”。协议内不得使用简写形式。
