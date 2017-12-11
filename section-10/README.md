# 10. IANA考虑事项

## NETCONF XML命名空间

本文档在`IETF XML`注册表[RFC3688 - The IETF XML Registry](https://tools.ietf.org/html/rfc3688)中为`NETCONF XML`名称空间注册了一个`URI`。

`IANA`更新了以下`URI`以引用此文档。

> URI: urn:ietf:params:xml:ns:netconf:base:1.0

注册人联系人：`IESG`。

`XML`：`N/A`，请求的`URI`是一个`XML`命名空间。

## NETCONF XML Schema

该文档在`IETF XML`注册表[RFC3688 - The IETF XML Registry](https://tools.ietf.org/html/rfc3688)中注册`了NETCONF XML schema`的`URI`。

`IANA`更新了以下`URI`以引用此文档。

> URI: urn:ietf:params:xml:schema:netconf

注册人联系人：IESG。

XML：本文件的[附录B](/appendix/README.md).

## NETCONF YANG Module

该文档在`YANG`模块名称注册表[RFC6020 - YANG - A Data Modeling Language for the Network Configuration Protocol (NETCONF)](https://tools.ietf.org/html/rfc6020)中注册了一个`YANG`模块。

- `name`:        `ietf-netconf`
- `namespace`:   `urn:ietf:params:xml:ns:netconf:base:1.0`
- `prefix`:      `nc`
- `reference`:   [RFC6241 - Network Configuration Protocol (NETCONF)](https://tools.ietf.org/html/rfc6241)

## NETCONF能力URNs

`IANA`已经创建并维护了一个分配`NETCONF`能力标识符的注册“网络配置协议（`NETCONF`）能力`URN`”。 添加注册表需要IETF标准行动。

`IANA`更新了以下能力的分配以引用此文档。

能力标识符索引

- :writable-running
    >   urn:ietf:params:netconf:capability:writable-running:1.0

- :candidate
    >   urn:ietf:params:netconf:capability:candidate:1.0

- :rollback-on-error
    >   urn:ietf:params:netconf:capability:rollback-on-error:1.0

-:startup
    >   urn:ietf:params:netconf:capability:startup:1.0

- :url
    > urn:ietf:params:netconf:capability:url:1.0

- :xpath
    >   urn:ietf:params:netconf:capability:xpath:1.0


`IANA`已将以下能力添加到注册表中：

能力标识符索引

- :base:1.1
   > urn:ietf:params:netconf:base:1.1

- :confirmed-commit:1.1
  > urn:ietf:params:netconf:capability:confirmed-commit:1.1

- :validate:1.1
  > urn:ietf:params:netconf:capability:validate:1.1
