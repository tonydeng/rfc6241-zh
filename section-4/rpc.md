# 4.1 < rpc >元素

`<rpc>`元素用于封装从客户端发送到服务器的`NETCONF`请求。

`<rpc>`元素有一个强制属性“`message-id`”，它是由`RPC`的发送者选择的一个字符串，它通常编码一个单调递增的整数。 `RPC`的接收者不解码或解释这个字符串，只是简单地保存它在任何生成的`<rpc-reply>`消息中用作“`message-id`”属性。 发送者必须确保“`message-id`”值根据[W3C.REC-xml-20001006](https://tools.ietf.org/html/rfc6241#ref-W3C.REC-xml-20001006)中定义的`XML`属性值规范化规则进行规范化，如果发送者希望字符串不被修改返回的话。 例如：

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <some-method>
      <!-- method parameters here... -->
    </some-method>
</rpc>
```

如果在`<rpc>`元素中存在额外的属性，`NETCONF` `peer`必须在`<rpc-reply>`元素中不加修改地返回它们。 这包括任何“`xmlns`”属性。

`RPC`的名称和参数被编码为`<rpc>`元素的内容。 `RPC`的名字是直接在`<rpc>`元素中的元素，任何参数都在这个元素内编码。

以下示例调用一个名为`<my-own-method>`的方法，该方法有两个参数：`<my-first-parameter>`，其值为“`14`”，另一个参数的值为“`fred`”：

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <my-own-method xmlns="http://example.net/me/my-own/1.0">
    <my-first-parameter>14</my-first-parameter>
    <another-parameter>fred</another-parameter>
  </my-own-method>
</rpc>
```

以下示例调用`<zip-code>`参数为“`27606-0100`”的`<rock-the-house>`方法：

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <rock-the-house xmlns="http://example.net/rock/1.0">
    <zip-code>27606-0100</zip-code>
  </rock-the-house>
</rpc>
```

以下示例调用不带参数的`NETCONF` `<get>`方法：

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <get/>
</rpc>
```
