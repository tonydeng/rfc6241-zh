# 4.4. < ok > 元素

如果在处理`<rpc>`请求期间没有发生错误或警告，`<ok>`元素将在`<rpc-reply>`消息中发送。 例如：

```xml
<rpc-reply message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <ok/>
</rpc-reply>
```
