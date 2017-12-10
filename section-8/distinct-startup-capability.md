# 8.7. 独特的启动能力(`Distinct Startup Capability`)

## 描述

该设备支持单独的运行和启动配置数据存储。 启动配置在启动时由设备加载。 影响运行配置的操作不会自动复制到启动配置。 从`<running>`到`<startup>`的显式`<copy-config>`操作用于将启动配置更新为正在运行的配置的当前内容。 `NETCONF`协议操作使用`<startup>`元素引用启动数据存储。

## 依赖

没有。

## 能力标识符

启动功能由以下功能字符串标识：

> urn:ietf:params:netconf:capability:startup:1.0

## 新操作

没有。

## 对现有操作的修改

### `General`

启动功能将`<startup/>`配置数据存储添加到多个`NETCONF`操作的参数。 服务器必须支持以下附加值：

```
+--------------------+--------------------------+-------------------+
| Operation          | Parameters               | Notes             |
+--------------------+--------------------------+-------------------+
| <get-config>       | <source>                 |                   |
|                    |                          |                   |
| <copy-config>      | <source> <target>        |                   |
|                    |                          |                   |
| <lock>             | <target>                 |                   |
|                    |                          |                   |
| <unlock>           | <target>                 |                   |
|                    |                          |                   |
| <validate>         | <source>                 | If :validate:1.1  |
|                    |                          | is advertised     |
|                    |                          |                   |
| <delete-config>    | <target>                 | Resets the device |
|                    |                          | to its factory    |
|                    |                          | defaults          |
+--------------------+--------------------------+-------------------+
```

要保存启动配置，请使用`<copy-config>`操作将`<running>`配置数据存储复制到`<startup>`配置数据存储。

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <copy-config>
        <target>
            <startup/>
        </target>
        <source>
            <running/>
        </source>
    </copy-config>
</rpc>
```
