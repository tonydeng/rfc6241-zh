# 8.3. 候选配置能力(Candidate Configuration Capability)

## 描述

候选配置功能：候选(`candidate`)，表示设备支持候选配置数据存储，用于保存可以在不影响设备当前配置的情况下操作的配置数据。候选配置是一个完整的配置数据集，用作创建和操作配置数据的工作场所。可以对此数据进行添加，删除和更改以构建所需的配置数据。可以随时执行`<commit>`操作，使设备的运行配置设置为候选配置的值。

  `<commit>`操作有效地将运行配置设置为候选配置的当前内容。虽然它可以被模仿为一个简单的副本，但是由于许多原因，它是作为一个独特的操作完成的。

在把高级概念保持为一流的操作的同时，我们允许开发人员更清楚地了解客户端请求的内容以及服务器必须执行的操作。这样可以使意图更加明显，特殊情况更简单，操作之间的交互更加直接。

例如，`confirm-commit：1.1` 功能（[第8.4节 - 确认提交能力(Confirmed Commit Capability)](onfirmed-commit-capability.md)）与“复制确认(`copy confirmed`)”操作无关。

候选配置可以在多个会话中共享。除非客户端具有不共享候选配置的特定信息，否则它必须假定其他会话能够同时修改候选配置。因此，谨慎的做法是在修改候选配置之前锁定候选配置。

客户端可以通过执行`<discard-changes>`操作来放弃对候选配置的任何未提交的更改。该操作将候选配置的内容恢复为正在运行的配置的内容。

## 依赖

无

## 功能标识符

候选能力由以下能力字符串标识：

> urn:ietf:params:netconf:capability:candidate:1.0

## 新操作

### `<commit>`

#### 描述:

当候选配置的内容完成时，可以提交配置数据，将数据集发布到设备的其余部分，并请求设备符合新配置中描述的行为。

要将候选配置作为设备的新当前配置提交，请使用`<commit>`操作。

`<commit>`操作指示设备实现候选配置中包含的配置数据。如果设备无法提交候选配置数据存储中的所有更改，则正在运行的配置必须保持不变。如果设备成功提交，则正在运行的配置必须更新为候选配置的内容。

如果运行或候选配置当前被不同的会话锁定，则`<commit>`操作必须失败，并且`<error-tag>`值为“正在使用”。

如果系统没有`:canability`能力，则`<commit>`操作不可用。

#### 正面的回应:

如果设备能够满足请求，则发送包含`<ok>`元素的`<rpc-reply>`。

#### 负面的回应:

如果由于任何原因无法完成请求，`<rpc-error>`元素将包含在`<rpc-reply>`中。

#### 例子：

```xml
<rpc message-id="101"
         xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <commit/>
</rpc>

<rpc-reply message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <ok/>
</rpc-reply>
```

### `<discard-changes>`

如果客户端决定不提交候选配置，则可以使用`<discard-changes>`操作将候选配置恢复为当前运行配置。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <discard-changes/>
</rpc>
```

此操作通过使用正在运行的配置的内容重置候选配置来丢弃任何未提交的更改。

## 对现有操作的修改

### `<get-config>`, `<edit-config>`, `<copy-config>`, 和`<validate>`

候选配置可用作任何`<get-config>`，`<edit-config>`，`<copy-config>`或`<validate>`操作的源或目标作为`<source>`或`<target>`参数。 `<candidate>`元素用于指示候选配置：

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <get-config>
        <source>
            <candidate/>
        </source>
    </get-config>
</rpc>
```

### `<lock>`和`<unlock>`

可以使用带有`<candidate>`元素的`<lock>`操作作为`<target>`参数来锁定候选配置：

```xml
<rpc message-id="101"
      xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <lock>
        <target>
            <candidate/>
        </target>
    </lock>
</rpc>
```

同样，使用`<candidate>`元素作为`<target>`参数解锁候选配置：

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <unlock>
    <target>
      <candidate/>
    </target>
  </unlock>
</rpc>
```

当客户端发生故障并对候选配置进行了重大更改时，恢复可能会很困难。 为了便于恢复，当释放锁时，无论是显式地使用`<unlock>`操作还是隐含地来自会话失败，都会丢弃任何未完成的更改。
