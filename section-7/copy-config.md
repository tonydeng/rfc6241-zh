# 7.3.  `<copy-config>`

## 说明：

使用另一个完整配置数据存储的内容创建或替换整个配置数据存储。如果目标数据存储存在，则会被覆盖。否则，如果允许的话，创建一个新的。

如果`NETCONF`节点(`peer`)支持：`url`功能（见[第8.8节](https://tools.ietf.org/html/rfc6241#section-8.8)），那么`<url>`元素可以作为`<source>`或`<target>`参数出现。

即使它通告：可写可运行(`writable-running`)能力，设备也可以选择不支持`<running/>`配置数据存储作为`<copy-config>`操作的`<target>`参数。设备可以选择不支持远程到远程复制操作，其中`<source>`和`<target>`参数都使用`<url>`元素。如果`<source>`和`<target>`参数标识相同的URL或配置数据存储，则必须返回一个包含“`invalid-value`”的错误标签的错误。

## 参数：

- `target`：用作`<copy-config>`操作的目标的配置数据存储的名称。

- `source`：用作`<copy-config>`操作源的配置数据存储的名称，或包含要复制的完整配置的`<config>`元素。

## 正面响应：

如果设备能够满足请求，则发送包含`<ok>`元素的`<rpc-reply>`。

## 负面响应：

如果由于任何原因无法完成请求，则`<rpc-reply>`元素将包含`<rpc-error>`元素。

## 示例：

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <copy-config>
        <target>
            <running/>
        </target>
        <source>
            <url>https://user:password@example.com/cfg/new.txt</url>
        </source>
    </copy-config>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>
```
