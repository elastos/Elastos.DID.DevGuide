# Integrity and expiration

为了安全起见，Elastos DID Document，Verifialbe credential都设有有效期。DID Document最长有效期可以设定为5年，用户也可以根据自身需求设定为更短的有效期。超出有效期的DID主题则会被DID解析器识别为无效DID主题。Verifialbe credential最长有效期为其持有者的有效期，超过也是为无效凭证。当然我们可以修改有效期来延长有效时间范围。

For safety reasons, Elastos DID Document and verifiable credential all have effective period. The longest effective period of DID Document can be set to 5 years, or users can set it to a shorter effective period according to their own needs. A DID topic that exceeds the effective period will be recognized as an invalid DID topic by the DID parser. The longest effective period of verifiable credential is the effective period of its holder, and if it exceeds the effective period, it is also an invalid credential. Certainly, the effective period can be modified to prolong the time range.

Elastos DID是个去中心化的个人身份系统，如何在实现去中心化的前提下保证安全是重中之重。

Elastos DID is a decentralized personal identity system, and the most important thing is to ensure security under the premise of decentralization.

Elastos DID Document，Verifiable credential，Verfiable presentation都需要符合一定有效性条件。

Elastos DID Document, verifiable credential and verifiable presentation all need to meet certain validity conditions.
