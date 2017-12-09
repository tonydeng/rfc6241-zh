# 7.5.  `<lock>`

## 说明：

`<lock>`操作允许客户端锁定设备的整个配置数据存储系统。这样的锁旨在是短暂的，并且允许客户进行改变，而不用担心与其他`NETCONF`客户端，非`NETCONF`客户端（例如，`SNMP`和命令行接口（`CLI`）脚本）和人类用户的交互。

如果现有会话或其他实体在锁定目标的任何部分持有锁定，则尝试锁定配置数据存储务必失败。

当获得锁定时，服务器必须禁止除了这个会话所请求的锁定资源的任何改变。 `SNMP`和`CLI`请求修改资源必须失败，并出现适当的错误。

锁定的持续时间被定义为当获得锁定时开始，并持续到锁定被释放或`NETCONF`会话关闭。会话闭包可以由客户端显式地执行，或者由服务器根据诸如底层传输失败，简单的不活动超时或检测客户端的滥用行为之类的标准隐含地执行。这些标准取决于实施和底层运输。

 `<lock>`操作采用强制参数`<target>`。 `<target>`参数命名将被锁定的配置数据存储。锁定处于活动状态时，在锁定的配置数据存储上使用`<edit-config>`操作并将锁定的配置用作`<copy-config>`操作的目标将被任何其他`NETCONF`会话禁止。另外，系统将确保这些锁定的配置资源不会被其他非`NETCONF`管理操作（如`SNMP`和`CLI`）修改。可以使用`<kill-session>`操作强制释放另一个`NETCONF`会话拥有的锁。定义如何打破其他实体的锁定超出了本文档的范围。

如果满足以下任一条件，则不得授予锁定：

- 任何`NETCONF`会话或其他实体已经拥有一个锁。

- 目标配置是`<candidate>`，它已被修改，并且这些更改没有被提交或回滚。

- 目标配置是`<running>`，另一个`NETCONF`会话有一个正在进行的确认提交（见[第8.4节](https://tools.ietf.org/html/rfc6241#section-8.4)）。

服务器必须响应一个`<ok>`元素或一个`<rpc-error>`。

如果持有锁的会话因任何原因终止，系统将释放一个锁。


## 参数：

- `target`：要锁定的配置数据存储的名称。

## 正面响应：

如果设备能够满足请求，则发送包含`<ok>`元素的`<rpc-reply>`。

## 负面响应：

如果由于任何原因无法完成请求，`<rpc-error>`元素将包含在`<rpc-reply>`中。

如果锁已被占用，那么`<error-tag>`元素将被“锁定拒绝(`lock-denied`)”，并且`<error-info>`元素将包括锁拥有者的`<session-id>`。 如果锁由非`NETCONF`实体持有，则包含`0`（`zero`）的`<session-id>`。 请注意，任何其他实体甚至在部分目标上执行锁定都将阻止在该目标上获得`NETCONF`锁（这是全局的）。

## 示例：

- 以下示例显示了一个锁的成功获取。

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <lock>
        <target>
            <running/>
        </target>
    </lock>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/> <!-- lock succeeded -->
</rpc-reply>
```

- 以下示例显示锁已在使用中时尝试获取锁的失败。

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <lock>
        <target>
            <running/>
        </target>
    </lock>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <rpc-error> <!-- lock failed -->
        <error-type>protocol</error-type>
        <error-tag>lock-denied</error-tag>
        <error-severity>error</error-severity>
        <error-message>
            Lock failed, lock is already held
        </error-message>
        <error-info>
            <session-id>454</session-id>
            <!-- lock is held by NETCONF session 454 -->
        </error-info>
    </rpc-error>
</rpc-reply>
```
