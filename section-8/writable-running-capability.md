# 8.2. 可写入运行能力(`Writable-Running Capability`)

## 描述

可写可运行能力(`Writable-Running Capability`)表示设备支持直接写入`<running>`配置数据存储。 换句话说，该设备支持以`<running>`配置为目标的`<edit-config>`和`<copy-config>`操作。

## 依赖

没有。

## 能力标识符

可写可运行能力由以下能力字符串标识：

> urn:ietf:params:netconf:capability:writable-running:1.0

## 新操作

没有。

## 对现有操作的修改

### `<edit-config>`

可写可运行能力修改`<edit-config>`操作以接受`<running>`元素作为`<target>`。

### `<copy-config>`

可写可运行能力修改`<copy-config>`操作来接受`<running>`元素作为`<target>`。
