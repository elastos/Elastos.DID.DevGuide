# Customized DID

Customized DID就是自定义DID，DID字串是用户提供的短字符串，也具有唯一性。

Customized DID means customized DID, and the DID string is a short string provided by the user, which is also unique.

Customized DID和普通DID一样，详细的内容都集中在DID Document中表达。Customized DID是由至少一个普通DID作为controller生成和管理的。多个controller之间通过多签规则对DID内容进行认可和签名。controller也可以代替customized DID行使Customized DID作为DID需要参与的操作，比如签名。

Like primitive DID, the details of the customized DID are expressed in DID Document.

Customized DID is generated and managed by at least one primitive DID as a controller. The DID content is recognized and signed by multiple controllers through multi-signature rules. Controller can also take the place of customized DID to perform the operations that customized DID needs to participate as DID, such as signing.

Customiezed DID Document可以不包含任何形式的key，通过持有者的key来完成授权和委托。

Customized DID Document can not contain any form of key, and authorization and entrustment can be completed by the holder’s key.

自定义DID支持添加，删除持有者（自定义DID必须包含一个持有者）以及更改多重签名规则，当自定义DID文档发生持有者更改或者更改多重签名规则，就需要通过所有权转移交易来完成文档上链操作。

Customized DID supports adding and deleting holders (customized DID must contain one holder), as well as changing multi-signature rules. When the customized DID Document changes the holder or the multi-signature rules, it is necessary to complete the document chain operation through the ownership transfer transaction.

更改持有者是较为谨慎和严谨的操作，鉴于此特性，该交易必须同时基于修改后的文档和转移凭证（ticket），使用DID原持有者主密钥对来完成该交易。其中转移凭证的内容为DID主题，转移凭证的接收者和前一个DID文档操作的交易ID，转移凭证需要原持有者根据多重签名规则共同签名完成，以表示原持有者对更换持有者的认可。

Changing the holder is a cautious and rigorous operation. In view of this, the transaction must be completed by using the main key of DID original holder based on the modified document and transfer ticket at the same time. Among them, the contents of the transfer ticket are DID topic, the recipient of the transfer ticket and the transaction ID of the previous DID Document operation, and the transfer ticket needs to be signed by the original holder according to the multi-signature rule to show the original holder’s approval of the replacement holder.
