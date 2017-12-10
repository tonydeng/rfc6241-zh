# 7.9  `<kill-session>`

## 说明：

强制终止`NETCONF`会话。

当一个`NETCONF`实体收到一个打开会话的`<kill-session>`请求时，它将中止当前正在进行的任何操作，释放与该会话关联的任何锁和资源，并关闭任何关联的连接。

如果一个`NETCONF`服务器在处理一个确认的提交时收到一个`<kill-session>`请求（[8.4节](https://tools.ietf.org/html/rfc6241#section-8.4)），它必须在确认的提交被发出之前把配置恢复到它的状态。

否则，`<kill-session>`操作不会回滚持有该锁的实体所做的配置或其他设备状态修改。

## 参数：

- `session-id`：终止的`NETCONF`会话的会话标识。如果该值等于当前会话`ID`，则返回“无效值”(`invalid-value`)错误。

## 正面响应：

如果设备能够满足请求，则发送包含`<ok>`元素的`<rpc-reply>`。

## 负面响应：

如果由于任何原因无法完成请求，`<rpc-error>`元素将包含在`<rpc-reply>`中。

## 示例

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <kill-session>
        <session-id>4</session-id>
    </kill-session>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>
```
