# 8.4. 确认提交能力(`Confirmed Commit Capability`)

## 描述

`:confirmed-commit:1.1`能力表示服务器将支持`<commit>`操作的`<cancel-commit>`操作和`<confirmed>`，`<confirm-timeout>`，`<persist>`和`<persist-id>`参数。有关`<commit>`操作的更多细节，请参见[第8.3节 - 候选配置能力(Candidate Configuration Capability)](candidate-configuration-capability.md)。

如果在超时期限内没有发出确认提交，默认的`<commit>`操作必须被恢复（默认600秒= 10分钟）。确认提交是一个`<commit>`操作，没有`<confirmed>`参数。超时时间可以通过`<confirm-timeout>`参数进行调整。如果在定时器超时之前发出了一个后续确认的`<commit>`操作，定时器将重置为新值（默认为600秒）。确认提交和后续确认的`<commit>`操作都可以引入对配置的其他更改。

如果在确认的提交操作中没有给出`<persist>`元素，则任何后续提交和确认提交必须在发出确认提交的同一会话上发出。如果在确认的`<commit>`操作中给出了`<persist>`元素，那么可以在任何会话中提供后续提交和确认提交，并且它们必须包含`<persist-id>`元素，其值等于给定的`<persist>`元素的值。

如果服务器还通告：启动功能，则从运行到启动的`<copy-config>`也可以保存更改以启动。

如果发布确认提交的会话在确认超时到期之前因任何原因被终止，则服务器务必将配置恢复到确认提交发出之前的状态，除非确认提交还包含`<persist>`元素。

如果设备在确认超时到期之前因任何原因而重新启动，则服务器务必将配置恢复到确认提交发出之前的状态。

如果未发出确认提交，则设备将其配置恢复到发出确认提交之前的状态。要取消已确认的提交并恢复更改而不等待确认超时过期，客户端可以使用`<cancel-commit>`操作显式地将配置恢复到确认的提交发出之前的状态。

对于共享配置，除非使用了配置锁定功能，否则此功能可能会导致其他配置更改（例如，通过其他`NETCONF`会话）被无意中更改或删除（换句话说，在`<edit-config>`操作开始）。因此，强烈建议为了在共享配置数据存储中使用此功能，还应该使用配置锁定。

这个功能的版本`1.0`是在[RFC4741 - NETCONF Configuration Protocol](https://tools.ietf.org/html/rfc4741)中定义的。 `1.1`版本在本文档中定义，并通过添加一个新操作`<cancel-commit>`和两个新的可选参数`<persist>`和`<persist-id>`来扩展版本`1.0`。为了与旧客户端向后兼容，符合此规范的服务器可以在版本`1.1`以外的版本中宣告对版本`1.0`的支持。

## 依赖

只有在`:candidate`能力也被支持的情况下，` :confirmed-commit:1.1`才具有相关性。

## 能力标识符

`:confirmed-commit:1.1`能力由以下能力字符串标识：

> urn:ietf:params:netconf:capability:confirmed-commit:1.1

## 新操作

### `<cancel-commit>`

#### 描述：

取消正在进行的确认提交。 如果没有给出`<persist-id>`参数，那么`<cancel-commit>`操作必须在发出确认提交的同一个会话上发出。

#### 参数：

- `persist-id`:

取消持续确认的提交。 该值必须等于`<commit>`操作的`<persist>`参数中给出的值。 如果值不匹配，则操作失败，出现“无效值”错误。

#### 正面回应：

如果设备能够满足请求，则发送包含`<ok>`元素的`<rpc-reply>`。

#### 负面回应：

如果由于任何原因无法完成请求，`<rpc-error>`元素将包含在`<rpc-reply>`中。

#### 示例

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <commit>
        <confirmed/>
    </commit>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>

<rpc message-id="102"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <cancel-commit/>
</rpc>

<rpc-reply message-id="102"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>
```

## 对现有操作的修改

### `<commit>`

`:confirmed-commit:1.1`功能允许4个额外的参数给`<commit>`操作。

#### 参数：

- `confirmed`：

执行一个确认的`<commit>`操作。

- `confirm-timeout`：

确认提交的超时时间，以秒为单位。 如果未指定，则确认超时默认为600秒。

- `persist`：

使已确认的提交在会话终止之后生效，并在持续的已确认提交上设置令牌。

- `persist-id`：

用于从任何会话发出后续确认的提交或确认提交，以及来自前一个`<commit>`操作的令牌。

#### 示例：

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <commit>
        <confirmed/>
        <confirm-timeout>120</confirm-timeout>
    </commit>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>
```

#### 示例：

```xml
<!-- start a persistent confirmed-commit -->
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <commit>
        <confirmed/>
        <persist>IQ,d4668</persist>
    </commit>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>

<!-- confirm the persistent confirmed-commit,
possibly from another session -->
<rpc message-id="102"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <commit>
        <persist-id>IQ,d4668</persist-id>
    </commit>
</rpc>

<rpc-reply message-id="102"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>
```
