# 8.9. XPath功能(`XPath Capability`)

## 描述

`XPath`能力表明`NETCONF`对等体(`peer`)支持在`<filter>`元素中使用`XPath`表达式。 `XPath`在[[W3C.REC-xpath-19991116]](http://www.w3.org/TR/1999/REC-xpath-19991116)中描述。

`XPath`表达式中使用的数据模型与`XPath 1.0` [[W3C.REC-xpath-19991116]](http://www.w3.org/TR/1999/REC-xpath-19991116)中使用的数据模型相同，具有与`XSLT 1.0`（[[W3C.REC-xslt-19991116]](http://www.w3.org/TR/1999/REC-xslt-19991116)）相同的扩展名， ，[第3.1节](/section-3/namespace.md)）。具体来说，这意味着根节点可以有任意数量的元素节点作为子节点。

`XPath`表达式在以下上下文中进行评估：

- 一组名称空间声明是`<filter>`元素上的作用域。

- 变量绑定的集合由数据模型定义。如果没有定义这样的变量绑定，则该集合是空的。

- 函数库是核心函数库，加上数据模型定义的任何函数。

上下文节点是根节点。

`XPath`表达式必须返回一个节点集。如果不返回节点集，则操作失败，并显示“无效值”错误。

响应消息包含由过滤器表达式选择的子树。对于每个这样的子树，响应消息中都包含从数据模型根节点到子树的路径，包括唯一标识子树所需的任何元素或属性。特定的数据实例不会在响应中重复。

## 依赖

没有。

## 能力标识符

`:xpath`能力由以下能力字符串标识：

> urn:ietf:params:netconf:capability:xpath:1.0

## 新操作

没有。

## 对现有操作的修改

### `<get-config>`和`<get>`

`:xpath`能力修改`<get>`和`<get-config>`操作以接受`<filter>`元素的“`type`”属性中的值“`xpath`”。当“`type`”属性设置为“`xpath`”时，必须在`<filter>`元素上存在一个“`select`”属性。 “`select`”属性将被视为`XPath`表达式，并用于过滤返回的数据。在这种情况下，`<filter>`元素必须是空的。

选择表达式的`XPath`结果必须是一个节点集。节点集中的每个节点必须对应于基础数据模型中的一个节点。为了正确识别每个节点，定义了以下编码规则：

- 结果节点的所有祖先节点必须首先被编码，所以根据底层的数据模型，应答中返回的`<data>`元素只包含完全指定的子树。

- 如果需要结果节点的任何兄弟节点或祖先节点来标识概念数据结构中的特定实例，那么这些节点也必须在响应中被编码。

例如：

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <get-config>
        <source>
            <running/>
        </source>
        <!-- get the user named fred -->
        <filter xmlns:t="http://example.com/schema/1.2/config"
            type="xpath"
            select="/t:top/t:users/t:user[t:name='fred']"/>
    </get-config>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <data>
        <top xmlns="http://example.com/schema/1.2/config">
        <users>
            <user>
                <name>fred</name>
                <company-info>
                    <id>2</id>
                </company-info>
            </user>
        </users>
    </top>
    </data>
</rpc-reply>
```
