# 7.2.  `<edit-config>`

## 描述：

`<edit-config>`操作将全部或部分指定配置加载到指定的目标配置数据存储。此操作允许以多种方式表示新配置，例如使用本地文件，远程文件或内联。如果目标配置数据存储不存在，它将被创建。

如果一个`NETCONF`节点(`peer`)支持：`url`能力（[8.8节](https://tools.ietf.org/html/rfc6241#section-8.8)），那么`<url>`元素可以出现而不是`<config>`参数。

设备分析源和目标配置并执行所请求的更改。目标配置不一定被替换，就像`<copy-config>`消息一样。而是根据源数据和请求的操作来更改目标配置。

如果`<edit-config>`操作包含应用于基础数据模型中相同概念节点的多个子操作，则操作的结果是未定义的（即，不在`NETCONF协`议​​的范围之内）。

## 属性：

- `operation`：`<config>`子树中的元素可以包含一个“`operation`”属性，它属于[3.1](https://tools.ietf.org/html/rfc6241#section-3.1)节定义的`NETCONF`名称空间。该属性标识配置中的要执行该操作的点，并可以在整个`<config>`子树中的多个元素上出现。如果未指定“`operation`”属性，则配置将合并到配置数据存储中。 “`operation`”属性具有以下值之一：

    - `merge`：由包含此属性的元素标识的配置数据与由`<target>`参数标识的配置数据存储中对应级别的配置合并。这是默认行为。

    - `replace`：由包含此属性的元素标识的配置数据将替换由`<target>`参数标识的配置数据存储区中的任何相关配置。如果配置数据存储中不存在此类配置数据，则会创建它。与替换整个目标配置的`<copy-config>`操作不同，只有实际存在于`<config>`参数中的配置受到影响。

    - `create`：当且仅当配置数据存在于配置数据存储中时，才将包含此属性的元素标识的配置数据添加到配置中。如果配置数据存在，则返回一个`<error-tag>`值为“`data-exists`”的`<rpc-error>`元素。

    - `delete`：当且仅当配置数据当前存在于配置数据存储中时，才从配置中删除由包含此属性的元素标识的配置数据。如果配置数据不存在，则返回一个`<error-tag>`值为“`data-missing`”的`<rpc-error>`元素。

    - `remove`：如果配置数据当前存在于配置数据存储中，则从配置中删除由包含此属性的元素标识的配置数据。如果配置数据不存在，服务器会自动忽略“`remove`”操作。

## 参数：

- `target`：正在编辑的配置数据存储的名称，例如`<running/>`或`<candidate />`。

- `default-operation`：选择此`<edit-config>`请求的默认操作（如“`operation`”属性中所述）。 `<default-operation>`参数的默认值是`“merge`”。`<default-operation>`参数是可选的，但是如果提供，它具有以下值之一：

    - `merge`：`<config>`参数中的配置数据与目标数据存储中相应级别的配置合并。这是默认行为。

    - `replace`：`<config>`参数中的配置数据完全替换了目标数据存储中的配置。这对加载以前保存的配置数据很有用。

    - `none`：目标数据存储不受`<config>`参数中的配置影响，除非和直到传入的配置数据使用“`operation`”属性请求不同的操作。如果`<config>`参数中的配置包含目标数据存储中没有相应级别的数据，则返回`<rpc-error>`，并带有`<error-tag>`缺少数据的值。使用“`none`”允许像“`delete`”这样的操作避免无意中创建要删除的元素的父层次结构。

- `test-option`：只有当设备公布支持`:validate:1.1`能力时才能指定`<test-option>`元素（[8.6节](https://tools.ietf.org/html/rfc6241#section-8.6)）。`<test-option>`元素具有以下值之一：

    - `test-then-set`：在尝试设置之前执行验证测试。 如果发生验证错误，请不要执行`<edit-config>`操作。 这是默认的测试选项。

    - `set`：先执行一个没有验证测试的设置。

    - `test-only`：仅执行验证测试，而不尝试设置。

- `error-option`：`<error-option>`元素具有以下值之一：

    - `stop-on-error`：停止第一个错误的`<edit-config>`操作。这是默认的错误选项。

    - `continue-on-error`：继续处理错误的配置数据;记录错误，如果发生任何错误，则产生否定响应。

    - `rollback-on-error`：如果发生错误情况，从而生成错误严重性`<rpc-error>`元素，则服务器将停止处理`<edit-config>`操作，并在指定的开始时将指定的配置恢复到完整状态这个`<edit-config>`操作。该选项要求服务器支持[8.5节](https://tools.ietf.org/html/rfc6241#section-8.5)中描述的错误回滚功能。

- `config`：配置数据的层次结构，由设备的一个数据模型定义。内容必须放置在适当的命名空间中，以允许设备检测适当的数据模型，并且内容必须遵循该数据模型的约束，如其能力定义所定义。能力在[第8节](https://tools.ietf.org/html/rfc6241#section-8)讨论。

## 正面响应：

如果设备能够满足请求，则发送包含`<ok>`元素的`<rpc-reply>`。

## 负面响应：

如果因任何原因无法完成请求，则会发送`<rpc-error>`答复。

## 示例：

本节中的`<edit-config>`示例使用一个简单的数据模型，其中存在多个实例的`<interface>`元素，每个`<interface>`元素中的`<name>`元素用来区分一个`<interface>`实例。

- 在正在运行的配置中名为“`Ethernet0/0`”的`interface`上将`MTU`设置为`1500`：

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <edit-config>
        <target>
            <running/>
        </target>
        <config>
            <top xmlns="http://example.com/schema/1.2/config">
                <interface>
                    <name>Ethernet0/0</name>
                    <mtu>1500</mtu>
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

- 在正在运行的配置中添加一个名为“`Ethernet0/0`”的`interface`，用这个名称替换以前的`interface`：

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <edit-config>
        <target>
            <running/>
        </target>
        <config xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
            <top xmlns="http://example.com/schema/1.2/config">
                <interface xc:operation="replace">
                    <name>Ethernet0/0</name>
                    <mtu>1500</mtu>
                    <address>
                        <name>192.0.2.4</name>
                        <prefix-length>24</prefix-length>
                    </address>
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

- 从正在运行的配置中删除名为“`Ethernet0/0`”的`interface`的配置：

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <edit-config>
        <target>
            <running/>
        </target>
        <default-operation>none</default-operation>
        <config xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
            <top xmlns="http://example.com/schema/1.2/config">
                <interface xc:operation="delete">
                    <name>Ethernet0/0</name>
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

- 从`OSPF`区域删除接口`192.0.2.4`（在同一区域配置的其他接口不受影响）：

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <edit-config>
        <target>
            <running/>
        </target>
        <default-operation>none</default-operation>
        <config xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
        <top xmlns="http://example.com/schema/1.2/config">
            <protocols>
                <ospf>
                    <area>
                        <name>0.0.0.0</name>
                        <interfaces>
                            <interface xc:operation="delete">
                                <name>192.0.2.4</name>
                            </interface>
                        </interfaces>
                    </area>
                </ospf>
            </protocols>
        </top>
        </config>
    </edit-config>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>
```
