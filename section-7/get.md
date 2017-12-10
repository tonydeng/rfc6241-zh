# 7.7  `<get>`

## 说明：

检索运行配置和设备状态信息。

## 参数：

- `filter`：此参数指定要检索的系统配置和状态数据的部分。 如果此参数不存在，则返回所有设备配置和状态信息。

    `<filter>`元素可以包含一个“`type`”属性。 该属性指示`<filter>`元素中使用的过滤语法的类型。 `NETCONF`中的默认过滤机制称为子树过滤，在[第6节](https://tools.ietf.org/html/rfc6241#section-6)中进行了描述。值“`subtree`”明确地标识了这种类型的过滤。

    如果`NETCONF`节点(`peer`)支持：`xpath`能力（见[第8.9节](https://tools.ietf.org/html/rfc6241#section-8.9)），则可以使用值“`xpath`”来指示`<filter>`元素的“`select`”属性包含一个`XPath`表达式。

## 正面响应：

如果设备能够满足请求，则发送`<rpc-reply>`。` <data`>部分包含适当的子集。

## 负面响应：

如果由于任何原因无法完成请求，`<rpc-error>`元素将包含在`<rpc-reply>`中。


## 示例

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <get>
        <filter type="subtree">
            <top xmlns="http://example.com/schema/1.2/stats">
                <interfaces>
                    <interface>
                        <ifName>eth0</ifName>
                    </interface>
                </interfaces>
            </top>
        </filter>
    </get>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <data>
        <top xmlns="http://example.com/schema/1.2/stats">
            <interfaces>
                <interface>
                    <ifName>eth0</ifName>
                    <ifInOctets>45621</ifInOctets>
                    <ifOutOctets>774344</ifOutOctets>
                </interface>
            </interfaces>
        </top>
    </data>
</rpc-reply>
```
