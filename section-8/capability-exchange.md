# 8.1. 能力交换(`Capabilities Exchange`)

功能在会话建立期间由每个对等体发送的消息中公布。当`NETCONF`会话打开时，每个对等体(`peer`)（客户机和服务器）都必须发送一个包含该对等体能力列表的`<hello>`元素。每个对等体必须至少发送基本的`NETCONF`能力，“`urn:ietf:params:netconf:base:1.1`”。一个对等(`peer`)可能包含以前的`NETCONF`版本的能力，表明它支持多个协议版本。

两个`NETCONF`对等体(`peer`)都必须验证另一个对等体(`peer`)已经公布了一个公共协议版本。在比较协议版本能力`URI`时，只有基本部分被使用，如果任何参数在`URI`字符串的末尾被编码。如果没有找到共同的协议版本能力，`NETCONF`对等体不能继续会话。如果存在多于一个的协议版本`URI`，则最高编号（最新）的协议版本必须由两个对等体使用。

发送`<hello>`元素的服务器必须包含一个包含此`NETCONF`会话会话ID的`<session-id>`元素。发送`<hello>`元素的客户端绝不能包含`<session-id>`元素。

使用`<session-id>`元素接收`<hello>`消息的服务器必须终止`NETCONF`会话。同样的，在服务器的`<hello>`消息中没有收到`<session-id>`元素的客户端必须终止`NETCONF`会话（不先发送`<close-session>`）。

在以下示例中，服务器通告基本`NETCONF`功能，在基本`NETCONF`文档中定义的一个`NETCONF`功能，以及一个实现特定功能。

```xml
<hello xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <capabilities>
        <capability>
            urn:ietf:params:netconf:base:1.1
        </capability>
        <capability>
            urn:ietf:params:netconf:capability:startup:1.0
        </capability>
        <capability>
            http://example.net/router/2.3/myfeature
        </capability>
    </capabilities>
    <session-id>4</session-id>
</hello>
```

一旦连接打开，每个对等体(`peer`)都会同时发送它的`<hello>`元素。 对等体(`peer`)在发送自己的集合之前不得等待从对方接收到的能力集合。
