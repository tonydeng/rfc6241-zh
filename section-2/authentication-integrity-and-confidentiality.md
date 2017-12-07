# 2.2 身份验证，完整性和机密性

`NETCONF`连接必须提供身份验证，数据完整性，机密性和重播保护。 `NETCONF`依赖于此功能的传输协议。 `NETCONF`对等方认为独立于本文档提供适当级别的安全性和机密性。例如，根据底层协议，可以使用`TLS`[[RFC5246 - The Transport Layer Security (TLS) Protocol              Version 1.2]](https://tools.ietf.org/html/rfc5246)或`SSH`[[RFC4251 - The Secure Shell (SSH) Protocol Architecture]](https://tools.ietf.org/html/rfc4251)对连接进行加密。

`NETCONF`连接必须经过认证。传输协议负责将服务器认证到客户端，并将客户端认证到服务器。 `NETCONF`对等体假定连接的认证信息已经被底层传输协议使用足够可信的机制进行了验证，并且对等体的身份已经被充分验证。

`NETCONF`的一个目标是为设备提供一个编程接口，紧密跟踪设备本地接口的功能。因此，预计底层协议使用设备上可用的现有认证机制。例如，支持`RADIUS` [[RFC2865 - Remote Authentication Dial In User Service (RADIUS)]](https://tools.ietf.org/html/rfc2865)的设备上的`NETCONF`服务器可能允许使用`RADIUS`来验证`NETCONF`会话。

认证过程必须产生一个认证的客户端标识，其权限是服务器已知的。客户端的身份验证通常被称为`NETCONF`用户名。用户名是一个字符串，与[W3C.REC-xml-20001006](https://tools.ietf.org/html/rfc6241#ref-W3C.REC-xml-20001006)的[第2.2节](https://www.w3.org/TR/2000/REC-xml-20001006#sec-documents)的“`Char`”生产相匹配。用于导出用户名的算法是传输协议特定的，另外还特定于传输协议使用的验证机制。传输协议必须提供一个由其他`NETCONF`层使用的用户名。

由`NETCONF`用户名标识的给定客户端的访问权限是`NETCONF`服务器配置的一部分。这些权限必须在`NETCONF`会话的其余部分执行。如何配置访问控制的细节超出了本文档的范围。
