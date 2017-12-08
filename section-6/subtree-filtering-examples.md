# 6.4 子树过滤示例

## 6.4.1 没有过滤器

在`<get>`操作中省略过滤器将返回整个数据模型。

```xml
 <rpc message-id="101"
      xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
   <get/>
 </rpc>

 <rpc-reply message-id="101"
      xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
   <data>
     <!-- ... entire set of data returned ... -->
   </data>
 </rpc-reply>
```

## 6.4.2 空过滤器

空的过滤器将不会选择任何内容，因为没有内容匹配或选择节点存在。 这不是一个错误。 这些例子中使用的`<filter>`元素的“`type`”属性将在[7.1节](https://tools.ietf.org/html/rfc6241#section-7.1)进一步讨论。

```xml
 <rpc message-id="101"
      xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
   <get>
     <filter type="subtree">
     </filter>
   </get>
 </rpc>

<rpc-reply message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <data>
    </data>
</rpc-reply>
```

## 6.4.3 选择整个`<users>`子树

这个例子中的过滤器包含一个选择节点（`<users>`），所以只有该子树被过滤器选中。 这个例子代表了下面大部分过滤器例子中的完全填充的`<users>`数据模型。 在真实的数据模型中，`<company-info>`不可能与特定主机或网络的用户列表一起返回。

> 注：本文档中使用的过滤和配置示例出现在命名空间“`http://example.com/schema/1.2/config`”中。 这个命名空间的根元素是`<top>`。 `<top>`元素及其后代仅表示一个示例配置数据模型。

```xml
<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <get-config>
        <source>
            <running/>
        </source>
        <filter type="subtree">
            <top xmlns="http://example.com/schema/1.2/config">
                <users/>
            </top>
        </filter>
    </get-config>
</rpc>


<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <data>
        <top xmlns="http://example.com/schema/1.2/config">
            <users>
                <user>
                    <name>root</name>
                    <type>superuser</type>
                    <full-name>Charlie Root</full-name>
                    <company-info>
                        <dept>1</dept>
                        <id>1</id>
                    </company-info>
                </user>
                <user>
                    <name>fred</name>
                    <type>admin</type>
                    <full-name>Fred Flintstone</full-name>
                    <company-info>
                        <dept>2</dept>
                        <id>2</id>
                    </company-info>
                </user>
                <user>
                    <name>barney</name>
                    <type>admin</type>
                    <full-name>Barney Rubble</full-name>
                    <company-info>
                        <dept>2</dept>
                        <id>3</id>
                    </company-info>
                </user>
            </users>
        </top>
    </data>
</rpc-reply>
```

下面的过滤器请求会产生相同的结果，但只是因为容器`<users>`定义了一个子元素（`<user>`）。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <get-config>
    <source>
        <running/>
    </source>
    <filter type="subtree">
        <top xmlns="http://example.com/schema/1.2/config">
            <users>
                <user/>
            </users>
        </top>
    </filter>
    </get-config>
</rpc>
```


## 6.4.4. 选择`<users>`子树内的所有`<name>`元素

该过滤器包含两个容器节点（`<users>`，`<user>`）和一个选择节点（`<name>`）。 在过滤器输出中选择同一兄弟集合中`<name>`元素的所有实例。 客户端可能需要知道`<name>`在这个特定的数据结构中被用作实例标识符，但是服务器不需要知道该元数据来处理该请求。


```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <get-config>
    <source>
      <running/>
    </source>
    <filter type="subtree">
      <top xmlns="http://example.com/schema/1.2/config">
        <users>
          <user>
            <name/>
          </user>
        </users>
      </top>
    </filter>
  </get-config>
</rpc>

<rpc-reply message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <data>
    <top xmlns="http://example.com/schema/1.2/config">
      <users>
        <user>
          <name>root</name>
        </user>
        <user>
          <name>fred</name>
        </user>
        <user>
          <name>barney</name>
        </user>
      </users>
    </top>
  </data>
</rpc-reply>
```
## 6.4.5 一个特定的`<user>`条目

此过滤器包含两个容器节点（`<users>`，`<user>`）和一个内容匹配节点（`<name>`）。 在过滤器输出中选择包含`<name>`（其名称等于“`fred`”的值的兄弟集合的所有实例）。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <get-config>
    <source>
      <running/>
    </source>
    <filter type="subtree">
      <top xmlns="http://example.com/schema/1.2/config">
        <users>
          <user>
            <name>fred</name>
          </user>
        </users>
      </top>
    </filter>
  </get-config>
</rpc>

<rpc-reply message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <data>
    <top xmlns="http://example.com/schema/1.2/config">
      <users>
        <user>
          <name>fred</name>
          <type>admin</type>
          <full-name>Fred Flintstone</full-name>
          <company-info>
            <dept>2</dept>
            <id>2</id>
          </company-info>
        </user>
      </users>
    </top>
  </data>
</rpc-reply>
```

## 6.4.6 来自特定`<user>`条目的特定元素

该过滤器包含两个容器节点（`<users>`，`<user>`），一个内容匹配节点（`<name>`）和两个选择节点（`<type>`，`<full-type>`）。 在过滤器输出中选择包含`<name>`的相同同级组中的`<type>`和`<full-name>`元素的所有实例，其中`<name>`的值等于“`fred`”。 不包含`<company-info>`元素，因为兄弟集合包含选择节点。

```xml


<rpc message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <get-config>
        <source>
        <running/>
        </source>
        <filter type="subtree">
            <top xmlns="http://example.com/schema/1.2/config">
                <users>
                    <user>
                        <name>fred</name>
                        <type/>
                        <full-name/>
                    </user>
                </users>
            </top>
        </filter>
    </get-config>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <data>
        <top xmlns="http://example.com/schema/1.2/config">
        <users>
            <user>
                <name>fred</name>
                <type>admin</type>
                <full-name>Fred Flintstone</full-name>
            </user>
        </users>
        </top>
    </data>
</rpc-reply>
```

## 6.4.7 多个子树

这个过滤器包含三个子树（`name = root`，`fred`，`barney`）。

“`root`”子树筛选器包含两个包含节点（`<users>`，`<user>`），一个内容匹配节点（`<name>`）和一个选择节点（`<company-info>`）。满足子树的选择条件，在过滤器输出中只选择“`root`”的`company-info`子树。

“`fred`”子树筛选器包含三个包含节点（`<users>`，`<user>`，`<company-info>`），一个内容匹配节点（`<name>`）和一个选择节点（`<id>`）。满足子树的选择条件，在过滤器输出中只选择“`fred`”的`company-info`子树内的`<id>`元素。

“`barney`”子树筛选器包含三个包含节点（`<users>`，`<user>`，`<company-info>`），两个内容匹配节点（`<name>`，`<type>`）和一个选择节点（`<dept>`）。由于用户“`barney`”不是“`superuser`”，并且“`barney`”（包括其父`<user>`条目）的整个子树被从过滤器输出中排除，所以不满足子树选择条件。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <get-config>
        <source>
            <running/>
        </source>
        <filter type="subtree">
            <top xmlns="http://example.com/schema/1.2/config">
                <users>
                    <user>
                        <name>root</name>
                        <company-info/>
                    </user>
                    <user>
                        <name>fred</name>
                        <company-info>
                            <id/>
                        </company-info>
                    </user>
                    <user>
                        <name>barney</name>
                        <type>superuser</type>
                        <company-info>
                            <dept/>
                        </company-info>
                    </user>
                </users>
            </top>
        </filter>
    </get-config>
</rpc>

<rpc-reply message-id="101"
    xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <data>
        <top xmlns="http://example.com/schema/1.2/config">
            <users>
                <user>
                    <name>root</name>
                    <company-info>
                        <dept>1</dept>
                        <id>1</id>
                    </company-info>
                </user>
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

## 6.4.8 具有属性命名的元素

在此示例中，筛选器包含一个包含节点（`<interfaces>`），一个属性匹配表达式（“`ifName`”）和一个选择节点（`<interface>`）。 在过滤器输出中选择具有“`ifName`”属性等于“`eth0`”的`<interface>`子树的所有实例。 过滤器数据元素和属性是合格的，因为如果命名空间属性不合格，“`ifName`”属性将不被视为“`schema/1.2`”命名空间的一部分。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <get>
    <filter type="subtree">
      <t:top xmlns:t="http://example.com/schema/1.2/stats">
        <t:interfaces>
          <t:interface t:ifName="eth0"/>
        </t:interfaces>
      </t:top>
    </filter>
  </get>
</rpc>

<rpc-reply message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <data>
    <t:top xmlns:t="http://example.com/schema/1.2/stats">
      <t:interfaces>
        <t:interface t:ifName="eth0">
          <t:ifInOctets>45621</t:ifInOctets>
          <t:ifOutOctets>774344</t:ifOutOctets>
        </t:interface>
      </t:interfaces>
    </t:top>
  </data>
</rpc-reply>
```

如果“`ifName`”是一个子节点而不是一个属性，那么下面的请求会产生类似的结果。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <get>
    <filter type="subtree">
      <top xmlns="http://example.com/schema/1.2/stats">
        <interfaces>
          <interface>
            <ifName>eth0</ifName>
          </interface>
        </interfaces>
      </top>
    </filter>
  </get>
</rpc>
```
