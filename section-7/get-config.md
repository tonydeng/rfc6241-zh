# 7.1.  `<get-config>`

## 说明：

检索全部或部分指定的配置数据存储。

## 参数：

- `source`：被查询的配置数据存储的名称，例如`<running/>`。
- `filter`：此参数标识要检索的设备配置数据存储的各个部分。 如果此参数不存在，则返回整个配置。
            `<filter>`元素可以包含一个“`type`”属性。 该属性指示`<filter>`元素中使用的过滤语法的类型。 `NETCONF`中的默认过滤机制称为子树过滤，在[第6节](https://tools.ietf.org/html/rfc6241#section-6)中进行了描述。值“`subtree`”明确地标识了这种类型的过滤。
            如果`NETCONF`对等体`(peer)`支持：`xpath`能力（[8.9节](https://tools.ietf.org/html/rfc6241#section-8.9)），则可以使用值“`xpath`”来指示`<filter>`元素上的“`select`”属性包含一个`XPath`表达式。

## 正面响应：

如果设备能够满足请求，服务器将发送一个包含`<data>`元素的`<rpc-reply>`元素以及查询结果。

## 负面响应：
如果由于任何原因无法完成请求，`<rpc-error>`元素将包含在`<rpc-reply>`中。

## 示例：

### 检索整个<users>子树：

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <get-config>
        <source>
            <running/>
        </source>
        <filter type="subtree">
            <top xmlns="http://example.com/schema/1.2/config">
                <users/>
            </top>
        </filter>
    </get-config>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <data>
        <top xmlns="http://example.com/schema/1.2/config">
            <users>
                <user>
                    <name>root</name>
                    <type>superuser</type>
                    <full-name>Charlie Root</full-name>
                    <company-info>
                        <dept>1</dept>
                        <id>1</id>
                    </company-info>
                </user>
                <!-- additional <user> elements appear here... -->
            </users>
        </top>
    </data>
</rpc-reply>
```

[第6节](https://tools.ietf.org/html/rfc6241#section-6)包含子树过滤的其他示例。
