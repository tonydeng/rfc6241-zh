# 7.8  `<close-session>`

## 说明：

请求正常终止一个`NETCONF`会话。

当一个`NETCONF`服务器收到一个`<close-session>`请求时，它会优雅地关闭会话。 服务器将释放与会话相关的任何锁和资源，并正常关闭任何关联的连接。 在`<close-session>`请求之后收到的任何`NETCONF`请求将被忽略。

## 正面响应：

如果设备能够满足请求，则发送包含`<ok>`元素的`<rpc-reply>`。

## 负面响应：

如果由于任何原因无法完成请求，`<rpc-error>`元素将包含在`<rpc-reply>`中。

## 示例

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <close-session/>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>
```
