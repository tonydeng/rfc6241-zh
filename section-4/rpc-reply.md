# 4.2. < rpc-reply > 元素

响应`<rpc>`操作发送`<rpc-reply>`消息。

`<rpc-reply>`元素有一个强制属性“`message-id`”，它等于这是一个响应的`<rpc>`的“`message-id`”属性。

`NETCONF`对等体也必须返回在`<rpc-reply>`元素中未修改的`<rpc>`元素中包含的任何附加属性。

响应名称和响应数据被编码为`<rpc-reply>`元素的内容。 回复的名字是直接在`<rpc-reply>`元素中的一个元素，任何数据都在这个元素内编码。

例如：

以下`<rpc>`元素调用`NETCONF` `<get>`方法，并包含一个名为“`user-id`”的附加属性。 注意“`user-id`”属性不在`NETCONF`命名空间中。 返回的`<rpc-reply>`元素返回“`user-id`”属性以及请求的内容。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0"
     xmlns:ex="http://example.net/content/1.0"
     ex:user-id="fred">
  <get/>
</rpc>

<rpc-reply message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0"
     xmlns:ex="http://example.net/content/1.0"
     ex:user-id="fred">
  <data>
    <!-- contents here... -->
  </data>
</rpc-reply>
```
