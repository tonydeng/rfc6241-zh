# 附录E.使用NETCONF配置多个设备

本部分是非规范性的。

## E.1 个别设备上的操作

考虑在单个设备上执行配置更新所涉及的工作。在对配置进行更改时，应用程序需要建立对其更改已正确完成的信任，并且不会影响设备的操作。应用程序（和应用程序用户）应该确信自己的更改没有损坏网络。

保护每个单独的设备由多个步骤组成：

- 获取配置锁定。

- 检查正在运行的配置。

- 加载并验证传入的配置。

- 更改运行配置。

- 测试新的配置。

- 使更改永久（如果需要）。

- 释放配置锁定。

我们来看看每一步的细节。

### E.1.1 获取配置锁定

应该获取锁定以防止同时从多个来源更新。如果有多个源影响设备，则应用程序在测试其配置更改以及恢复失败时都会受到阻碍。获得短暂的锁定是防止其他方面引入不相关变化的简单防御措施。

锁可以使用`<lock>`操作获取。

```xml
 <rpc message-id="101"
      xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
   <lock>
     <target>
       <running/>
     </target>
   </lock>
 </rpc>

If the :candidate capability is supported, the candidate
configuration should be locked.

 <rpc message-id="101"
      xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
   <lock>
     <target>
       <candidate/>
     </target>
   </lock>
 </rpc>
```

### E.1.2 检查运行配置

加载新配置之前，正在运行的配置可以作为检查点保存到本地文件中。 如果更新失败，则可以通过重新加载检查点文件来恢复配置。

检查点文件可以使用`<copy-config>`操作创建。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <copy-config>
    <target>
      <url>file://checkpoint.conf</url>
    </target>
    <source>
      <running/>
    </source>
  </copy-config>
</rpc>
```

> 要恢复检查点文件，请颠倒`<source>`和`<target>`参数。

### E.1.3 加载和验证传入配置

如果支持：候选能力，可以将配置加载到设备上，而不会影响正在运行的系统。

```xml
<rpc message-id="101"
      xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
   <edit-config>
     <target>
       <candidate/>
     </target>
     <config>
       <!-- place incoming configuration changes here -->
     </config>
   </edit-config>
</rpc>
```

如果设备支持`:validate:1.1`功能，那么默认情况下会在将入站配置加载到候选中时对其进行验证。 为了避免这种验证，传递值为“`set`”的`<test-option>`参数。 可以使用`<validate>`操作请求完整验证。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <validate>
    <source>
      <candidate/>
    </source>
  </validate>
</rpc>
```

### E.1.4 更改运行配置

当传入配置安全地加载到设备上并进行验证后，就可以对运行中的系统产生影响。

如果设备支持：候选能力，请使用`<commit>`操作将运行配置设置为候选配置。 如果与设备的连接失败，请使用`<confirmed>`参数来自动恢复到原始配置。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <commit>
    <confirmed/>
    <confirm-timeout>120</confirm-timeout>
  </commit>
</rpc>
```

如果设备不支持候选，则传入的配置更改会直接加载到运行中。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <edit-config>
    <target>
      <running/>
    </target>
    <config>
      <!-- place incoming configuration changes here -->
    </config>
  </edit-config>
</rpc>
```

### E.1.5 测试新的配置

现在传入的配置已经集成到运行配置中，应用程序需要相信这种更改会以预期的方式影响设备，而不会对其造成负面影响。

为了获得这种信心，应用程序可以运行设备运行状态的测试。 测试的性质取决于变更的性质，超出了本文档的范围。 这些测试可能包括运行应用程序的系统（使用`ping`）的可达性，与网络其余部分的可达性变化（通过比较设备的路由表），或者检查特定的变化（寻找`BGP`对等体(`peer`)的操作证据 刚刚添加）。

### E.1.6 使变更永久

当配置更改到位并且应用程序对此更改的适当功能有足够的信心时，应用程序应该使更改永久。

如果设备支持：启动功能，则可以使用启动配置作为`<copy-config>`操作的目标，将当前配置保存到启动配置中。

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

如果设备支持：候选功能，并且请求了已确认的提交，则确认提交必须在超时到期之前发送。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <commit/>
</rpc>
```

### E.1.7。 释放配置锁

配置更新完成后，必须释放锁定，以允许其他应用程序访问配置。

使用`<unlock>`操作释放配置锁定。

```xml
<rpc message-id="101"
     xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <unlock>
    <target>
      <running/>
    </target>
  </unlock>
</rpc>
```

如果：候选能力被支持，候选配置应该被解锁。

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

## E.2 在多个设备上运行

当配置更改需要更新多个设备时，需要注意提供所需的事务语义。 NETCONF协议​​包含足够的原语，可以建立面向事务的操作。跨多个设备提供完整的事务语义是非常昂贵的，但可以减少故障场景的大小和窗口数量。

有两类多设备操作。第一类允许操作在单个设备上失败，而不需要所有设备恢复到其原始状态。该操作可以在稍后重试，或者其失败简单地报告给用户。这个类的一个例子可能是添加一个NTP服务器。对于这一类操作，故障避免和恢复集中在单个设备上。这意味着恢复设备，报告故障，并可能安排另一次尝试。

第二类更有意思，要求操作应在所有设备上完成或完全颠倒。网络要么转变成新的状态，要么被重置为原来的状态。例如，对VPN的更改可能需要更新多个设备。另一个例子可能是添加一个服务类定义。将网络保留在只有一部分设备已经用新定义进行更新的状态下，当定义被引用时将导致将来的失败。

为了提供事务性语义，使用上面列出的单设备操作中使用的相同步骤，但在所有设备上并行执行。应该在所有目标设备上获取配置锁定，并保存，直到所有设备都更新并且所做的更改都是永久的。应该上传配置更改并在所有设备上执行验证。检查点应在每个设备上进行。然后，运行配置可以改变，测试，并永久。如果这些步骤中的任何一个都失败了，以前的配置可以在任何被更改的设备上恢复。完成更改或完全废弃后，可以释放每个设备上的锁。
