# 7.4. `<delete-config>`

## 说明：

删除配置数据存储。 `<running>`配置数据存储不能被删除。

如果一个`NETCONF`节点(`peer`)支持：`url`功能（[第8.8节](https://tools.ietf.org/html/rfc6241#section-8.8)），`<url>`元素可以作为`<target>`参数出现。

## 参数：

- target：要删除的配置数据存储的名称。

## 正面响应：

如果设备能够满足请求，则发送包含`<ok>`元素的`<rpc-reply>`。

## 负面响应：

如果由于任何原因无法完成请求，则`<rpc-reply>`元素将包含`<rpc-error>`元素。

## 示例：

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <delete-config>
        <target>
            <startup/>
        </target>
    </delete-config>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>
```
