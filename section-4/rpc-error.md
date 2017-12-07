# 4.3. < rpc-error > 元素

如果在处理`<rpc>`请求期间发生错误，`<rpc-error>`元素将在`<rpc-reply>`消息中发送。

如果服务器在处理`<rpc>`请求期间遇到多个错误，`<rpc-reply>`可能包含多个`<rpc-error>`元素。但是，如果请求包含多个错误，则服务器不需要检测或报告多个`<rpc-error>`元素。

服务器不需要按照特定的顺序检查特定的错误条件。如果在处理期间出现任何错误情况，服务器务必返回一个`<rpc-error>`元素，并且如果在处理期间出现任何警告条件，应该返回一个`<rpc-error>`元素。

服务器绝对不能在客户端没有足够访问权限的`<rpc-error>`元素中返回应用级或数据模型特定的错误信息。

`<rpc-error>`元素包含以下信息：

- `error-type`：定义错误发生的概念层，下列类型之一。

    1. `transport` (`layer: Secure Transport`)
    1. `rpc` (`layer: Messages`)
    1. `protocol` (`layer: Operations`)
    1. `application` (`layer: Content`)

- `error-tag`：包含识别错误条件的字符串。 相关的值，请参阅[附录A](https://tools.ietf.org/html/rfc6241#appendix-A)。

- `error-severity`：包含标识错误严重性的字符串，由设备确定。 下列之一：

    1. `error`
    1. `warning`
        > 请注意，本文档中没有使用`warning`枚举定义的<error-tag>值。 这是保留供将来使用。

- `error-app-tag`：包含标识数据模型特定或特定于实现的错误条件（如果存在）的字符串。 如果没有适当的应用程序错误标签可以与特定的错误条件相关联，则这个元素将不存在。 如果数据模型特定的和特定于实现的`error-app-tag`都存在，那么服务器必须使用数据模型特定的值。
- `error-path`：包含绝对[XPath](https://tools.ietf.org/html/rfc6241#ref-W3C.REC-xpath-19991116)表达式，用于标识`<rpc-error>`元素中报告的错误关联的节点的元素路径。 如果没有适当的有效载荷元素或数据存储节点可以与特定的错误条件相关联，则该元素将不存在。

    - `XPath`表达式在以下上下文中进行解释。
        - 名称空间声明的集合是`<rpc-error>`元素上的范围声明。
        - 变量绑定的集合是空的。
        - 函数库是核心函数库。
    - 上下文节点取决于与所报告的错误相关联的节点：
        - 如果`payload`元素可以与错误关联，则上下文节点是`rpc`请求的文档节点（即`<rpc>`元素）。
        - 否则，上下文节点是所有数据模型的根，即所有数据模型中具有作为子节点的顶级节点的节点。
- `error-message`：包含一个适合人类阅读的字符串，用于描述错误情况。 如果没有为特定错误条件提供适当的消息，则该元素将不存在。 这个元素应该包含一个在[[W3C.REC-xml-20001006]](https://tools.ietf.org/html/rfc6241#ref-W3C.REC-xml-20001006)中定义并在[[RFC3470 - Guidelines for the Use of Extensible Markup Language (XML) within IETF Protocols]](https://tools.ietf.org/html/rfc3470)中讨论的`"xml：lang"`属性。
- `error-info`：包含协议或数据模型特定的错误内容。 如果没有为特定的错误条件提供这样的错误内容，则该元素将不存在。 [附录A](https://tools.ietf.org/html/rfc6241#appendix-A)中的列表定义了每个错误的任何强制错误信息内容。 在任何协议规定的内容之后，数据模型定义可能会要求某些应用层错误信息包含在错误信息容器中。 一个实现可以包含额外的元素来提供扩展和/或实现特定的调试信息。

例子：

```xml
<rpc-reply xmlns="URN" xmlns:junos="URL">
    <rpc-error>
        <error-severity>error-severity</error-severity>
        <error-path>error-path</error-path>
        <error-message>error-message</error-message>
        <error-info>...</error-info>
    </rpc-error>
</rpc-reply>
```

如果接收到<rpc>元素而没有`message-id`属性，则返回错误。
> 请注意，只有在这种情况下，`NETCONF`对等方可以省略`<rpc-reply>`元素中的`message-id`属性。

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <get-config>
    <source>
      <running/>
    </source>
  </get-config>
</rpc>

<rpc-reply xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <rpc-error>
    <error-type>rpc</error-type>
    <error-tag>missing-attribute</error-tag>
    <error-severity>error</error-severity>
    <error-info>
      <bad-attribute>message-id</bad-attribute>
      <bad-element>rpc</bad-element>
    </error-info>
  </rpc-error>
</rpc-reply>
```

以下`<rpc-reply>`说明了返回多个`<rpc-error>`元素的情况。

> 请注意，本节中的示例中使用的数据模型使用<name>元素来区分<interface>元素的多个实例。

```xml
<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0"
    xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
    <rpc-error>
        <error-type>application</error-type>
        <error-tag>invalid-value</error-tag>
        <error-severity>error</error-severity>
        <error-message xml:lang="en">
            MTU value 25000 is not within range 256..9192
        </error-message>
        <error-info>
            <top xmlns="http://example.com/schema/1.2/config">
                <interface>
                    <name>Ethernet0/0</name>
                    <mtu>25000</mtu>
                </interface>
            </top>
        </error-info>
    </rpc-error>
    <rpc-error>
        <error-type>application</error-type>
        <error-tag>invalid-value</error-tag>
        <error-severity>error</error-severity>
        <error-message xml:lang="en">
            Invalid IP address for interface Ethernet1/0
        </error-message>
        <error-info>
            <top xmlns="http://example.com/schema/1.2/config">
            <interface xc:operation="replace">
                <name>Ethernet1/0</name>
                <address>
                    <name>1.4</name>
                    <prefix-length>24</prefix-length>
                </address>
            </interface>
            </top>
        </error-info>
    </rpc-error>
</rpc-reply>
```
