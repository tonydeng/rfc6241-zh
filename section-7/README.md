# 7. 协议操作(Protocol Operations)

`NETCONF`协议​​提供一组底层操作来管理设备配置`(configurations)`和检索设备状态`(state)`信息。基本协议提供了检索`(retrieve)`，配置`(configure)`，复制`(copy)`和删除`(delete)`配置数据存储的操作。可以根据设备宣告的功能，提供了额外的操作。

基本协议包括以下协议操作：

- [get](get.md)

- [get-config](get-config.md)

- [edit-config](eidt-config.md)

- [copy-config](copy-config.md)

- [delete-config](delete-config.md)

- [lock](lock.md)

- [unlock](unlock.md)

- [close-session](close-session.md)

- [kill-session](kill-session.md)

协议操作可能由于各种原因而失败，包括“不支持操作`(operation not supported)`”。发起者不应该认为任何操作总会成功。任何RPC回复中的返回值应该检查错误响应。

协议操作的语法和`XML`编码在[附录C](https://tools.ietf.org/html/rfc6241#appendix-C)中的`YANG`模块中正式定义。以下部分描述了每个协议操作的语义。
