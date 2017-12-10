# 8.5. 回滚错误能力(`Rollback-on-Error Capability`)

## 描述

此功能表示服务器将支持`<edit-config>`操作的`<error-option>`参数中的“`roll-on-error`”值。

对于共享配置，除非使用了配置锁定功能，否则此功能可能会导致其他配置更改（例如，通过其他`NETCONF`会话）被无意中更改或删除（换句话说，在`<edit-config>` 操作开始）。 因此，强烈建议为了在共享配置数据存储中使用此功能，还可以使用配置锁定。

## 依赖

没有。

## 能力标识符

`:rollback-on-error`能力由以下能力字符串标识：

> urn:ietf:params:netconf:capability:rollback-on-error:1.0

## 新操作

没有。

## 对现有操作的修改

### `<edit-config>`

`:rollback-on-error`能力允许在`<edit-config>`操作中的`<error-option>`参数的“`rollback-on-error`”值。

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <edit-config>
        <target>
            <running/>
        </target>
        <error-option>rollback-on-error</error-option>
        <config>
            <top xmlns="http://example.com/schema/1.2/config">
                <interface>
                    <name>Ethernet0/0</name>
                    <mtu>100000</mtu>
                </interface>
            </top>
        </config>
    </edit-config>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>
```
