# 8.8. URL能力(URL Capability)

## 描述

`NETCONF`对等体(`peer`)有能力接受`<source>`和`<target>`参数中的`<url>`元素。该功能通过指示所支持的`URL`方案的`URL`参数进一步标识。

## 依赖

没有。

## 能力标识符

`URL`能力由以下能力字符串标识：

> urn:ietf:params:netconf:capability:url:1.0?scheme={name,...}

`URL`能力`URI`必须包含一个“`scheme`”参数，该参数被赋予一个以逗号分隔的方案名称列表，表明`NETCONF`对等体(`peer`)支持哪些方案。例如：

> urn:ietf:params:netconf:capability:url:1.0?scheme=http,ftp,file

## 新操作

没有。

## 对现有操作的修改

### `<edit-config>`

`:url`能力修改`<edit-config>`操作来接受`<url>`元素作为`<config>`参数的替代方法。 `url`引用的文件包含要修改的配置数据层次结构，在`XML`中的“`urn:ietf:params:xml:ns:netconf:base:1.0`”命名空间中的元素`<config>`下进行编码。

### `<copy-config>`

`:url`能力修改`<copy-config>`操作来接受`<url>`元素作为`<source>`和`<target>`参数的值。

`url`引用的文件包含完整的数据存储区，以`XML`格式编码在“`urn:ietf:params:xml:ns:netconf:base:1.0`”命名空间中的`<config>`元素下。

### `<delete-config>`

`:url`能力修改`<delete-config>`操作来接受`<url>`元素作为`<target>`参数的值。

### `<validate>`

`:url`能力修改`<validate>`操作来接受`<url>`元素作为`<source>`参数的值。
