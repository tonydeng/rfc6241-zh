# 8.6. 验证能力(`Validate Capability`)

## 描述

验证包括在将配置应用于设备之前检查语法和语义错误的完整配置。

如果通告了这个功能，那么该设备支持`<validate>`协议操作并至少检查语法错误。此外，此功能还支持`<edit-config>`操作的`<test-option>`参数，并在提供时至少检查语法错误。

这个功能的版本`1.0`是在[RFC4741 - NETCONF Configuration Protocol](https://tools.ietf.org/html/rfc4741)中定义的。 `1.1`版本在本文档中定义，并通过在`<edit-config>`操作的`<test-option>`参数中添加一个新值“`test-only`”来扩展1.0版本。为了与旧客户端向后兼容，符合此规范的服务器可以在版本`1.1`以外的版本中宣告版本`1.0`。

## 依赖

没有。

## 能力标识符

`:validate:1.1`能力由以下能力字符串标识：

> urn:ietf:params:netconf:capability:validate:1.1

## 新操作

### `<validate>`

#### 描述：

此协议操作验证指定配置的内容。

#### 参数：

- `source`：

要验证的配置数据存储的名称，例如`<candidate>`或包含要验证的完整配置的`<config>`元素。

#### 正面回应：

如果设备能够满足请求，则发送包含`<ok>`元素的`<rpc-reply>`。

#### 负面回应：

如果由于任何原因无法完成请求，`<rpc-error>`元素将包含在`<rpc-reply>`中。

由于许多原因（如语法错误，缺少参数，对未定义的配置数据的引用或对基础数据模型建立的规则的任何其他违规），`<validate>`操作可能会失败。

#### 示例

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <validate>
        <source>
            <candidate/>
        </source>
    </validate>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <ok/>
</rpc-reply>
```

### 对现有操作的修改

#### `<edit-config>`

`:validate:1.1`能力修改`<edit-config>`操作来接受`<test-option>`参数。
