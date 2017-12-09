# 7.6.  `<unlock>`

## 说明：

`<unlock>`操作用于释放以前通过`<lock>`操作获得的配置锁定。

如果满足以下任一条件，`<unlock>`操作将不会成功：

- 指定的锁不是当前激活的。

- 发出`<unlock>`操作的会话与获得锁的会话不同。

服务器必须响应一个`<ok>`元素或一个`<rpc-error>`。

## 参数：

- `target`：要解锁的配置数据存储的名称。`NETCONF`客户端不允许解锁未锁定的配置数据存储。

## 肯定回应：

如果设备能够满足请求，则发送包含`<ok>`元素的`<rpc-reply>`。

## 否定响应：

如果由于任何原因无法完成请求，`<rpc-error>`元素将包含在`<rpc-reply>`中。

## 示例：

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <unlock>
        <target>
            <running/>
        </target>
    </unlock>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>
```
