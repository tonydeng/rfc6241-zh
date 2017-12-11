# 附录A. NETCONF错误列表

本部分是规范性的。

对于每个错误标签，都会列出有效的错误类型和错误严重性值以及任何必需的错误信息（如果有）。


| error-tag     | error-type     |error-severity|error-info|Description|
| :------------- | :------------- |:------------- |:------------- |:------------- |
| in-use      | protocol, application       |error|none|The request requires a resource that already is in use.|
|invalid-value|protocol, application|error|none|The request specifies an unacceptable value for one or more parameters.|
|too-big|transport, rpc, protocol, application|error|none|The request or response (that would be generated) is too large for the implementation to handle.|
|missing-attribute|rpc, protocol, application|error|<bad-attribute> : name of the missing attribute <bad-element> : name of the element that is supposed to contain the missing attribute|An expected attribute is missing.|
|bad-attribute|rpc, protocol, application|error|<bad-attribute> : name of the attribute w/ bad value <bad-element> : name of the element that contains the attribute with the bad value|An attribute value is not correct; e.g., wrong type,  out of range, pattern mismatch|
|unknown-attribute|rpc, protocol, application|error|<bad-attribute> : name of the unexpected attribute <bad-element> : name of the element that contains the unexpected attribute|An unexpected attribute is present.|
|missing-element|protocol, application|error|<bad-element> : name of the missing element|An expected element is missing.|
|bad-element|protocol, application|error|<bad-element> : name of the element w/ bad value|An element value is not correct; e.g., wrong type, out of range, pattern mismatch.|
| unknown-element|protocol, application|error|<bad-element> : name of the unexpected element|An unexpected element is present|
| unknown-namespace|protocol, application|error|<bad-element> : name of the element that contains the unexpected namespace <bad-namespace> : name of the unexpected namespace|An unexpected namespace is present|
|access-denied|protocol, application|error|none|Access to the requested protocol operation or data model is denied because authorization failed.|
|lock-denied| protocol|error|<session-id> : session ID of session holding the requested lock, or zero to indicate a non-NETCONF entity holds the lock|Access to the requested lock is denied because the lock is currently held by another entity.|
|resource-denied| transport, rpc, protocol, application|error|none|Request could not be completed because of insufficient resources.|
|rollback-failed|protocol, application|error|none|Request to roll back some configuration change (via rollback-on-error or <discard-changes> operations) was not completed for some reason.|
|data-exists|application|error|none|Request could not be completed because the relevant ata model content already exists.  For example, a "create" operation was attempted on data that already exists.|
|data-missing|application|error|none|Request could not be completed because the relevant data model content does not exist.  For example, a "delete" operation was attempted on data that does not exist.|
|operation-not-supported|protocol, application|error|none|Request could not be completed because the requested operation is not supported by this implementation.|
| operation-failed|rpc, protocol, application|error|none|Request could not be completed because the requested operation failed for some reason not covered by |
|partial-operation|application|error|<ok-element> : identifies an element in the data model for which the requested operation has been completed for that node and all its child nodes. This element can appear zero or more times in the  <error-info> container.  <br/><br/>  <err-element> : identifies an element in the data model for which the requested operation has failed for that node and all its child nodes. This element can appear zero or more times in the <error-info> container. <br/><br/>  <noop-element> : identifies an element in the data model for which the requested operation was not attempted for that node and all its child nodes. This element can appear zero or more times in the <error-info> container.|This error-tag is obsolete, and SHOULD NOT be sent by servers conforming to this document. <br/><br/> Some part of the requested operation failed or was not attempted for some reason.  Full cleanup has not been performed (e.g., rollback not supported) by the server.  The error-info container is used to identify which portions of the application data model content for which the requested operation has succeeded (<ok-element>), failed (<bad-element>), or not been attempted (<noop-element>).|
|malformed-message|rpc|error|none|A message could not be handled because it failed to be parsed correctly.  For example, the message is not  well-formed XML or it uses an invalid character set. <br/><br/> This error-tag is new in :base:1.1 and MUST NOT be sent to old clients.|
