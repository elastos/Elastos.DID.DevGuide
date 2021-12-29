# DID

DID 对象用户身份的代表，每个用户可以有很多 DIDs，在不同的场景中使用不同的 DID。DID 只是一个字符串的标识符，每个 DID 都有一个 DIDDocument 对应，DID 和 DIDDocument 是一组共同的存在，类似于一个硬币的两面。Elastos DID 支持两种 DID，一种是普通的 DID，DID 的标识符是由 DID 默认的公钥通过 Hash 运算衍生出来的一个 base58 的字符串，用户不能直接指定；另外一种是自定义 DID，即 DID 的标识符是用户自己指定的。

A DID object is the representative of the identity of user. Each user can have many DIDs, which can be used in different scenarios. DID is just an identifier of a string, and each of it has a corresponding DID Document. DID and DID Document are a group of common existence, like two sides of one coin. Two kinds of DIDs are supported by Elastos DID. One is primitive DID, and its identifier is a base58 string derived from DID’s default public key through Hash operation, which can't be directly specified by users. The other is customized DID, that is, the identifier of DID is specified by the user.

通常的情况下，在 DID 所有者使用 DID 前，需要将 DID 发布到 ID 侧链上。给定一个 DID 标识符，都可以通过 ID 侧链解析出其对应的 DIDDocument。所以在日常使用中，只要提供 DID 的标识符即可表达用户的身份，应用可以通过该标识符获取 DIDDocument，并进行必要的身份验证的相关的操作。

Usually, before the usage of DID, it is necessary to publish DID to the ID side chain. The corresponding DID Document can be parsed through the ID side chain, when the DID identifier is given. Therefore, in daily use, as long as the identifier of DID is provided, the user’s identity can be expressed, and the application can obtain DID Document through the identifier as well as carry out necessary operations related to authentication.

## 普通 DID

Primitive DID

因为普通 DID 的 DID 标识符和默认密钥对的关联关系，从而形成了对该 DID 进行验证的可信根，并且据此形成完整的 DID 验证链，保证DID 身份的安全可信。

Because of the relationship between the DID identifier of the primitive DID and the default key, a trusted root for verifying the DID is formed, and a complete DID verification chain is formed according to this, which ensures the security and credibility of the DID identity.

因为普通 DID 标识符和密钥的对应关系，原则上不能进行所有权转移，因为不能保证转移的有效性。

Owing to the correspondence between primitive DID identifier and key, in principle, ownership transfer cannot be carried out, for the validity of the transfer cannot be guaranteed.

## 自定义 DID

Customized DID

自定义 DID 的标识符是可以用户自己指定，没有类似普通 DID 的可信根，所以自定义 DID 不能独立存在，必须由普通 DID 创建出来，并且由普通 DID 所拥有并控制，间接的建立可信根，从而形成对 DID 的可信验证链。自定义 DID 对外可以独立使用，和普通 DID 没有差别。

The identifier of the customized DID can be specified by the user, and there is no trusted root similar to the primitive DID, thus the customized DID cannot exist independently, but must be created, owned and controlled by the primitive DID, and the trusted root can be indirectly established, to form a trusted verification chain for the DID. Customized DID can be used independently, which is no different from primitive one.

自定义 DID 可以进行所有权转移，即变更 controller，详细说明参见[自定义 DID 变更所有权](transfer-the-ownership-of-the-customized-did.md)。

Customized DID can transfer ownership, that is, change the controller. See the changing of ownership of customized DID for details.

自定义 DID 可以由1个或者多个 controller 所持有，对于单一 controller 的情况，该 controller 拥有自定义 DID 的全部所有权，对自定义 DID 的文档的任何修改和签名仅能由该 controller 完成。对于多个 controller 持有的情况，可以定义一个多签的规则，即指定自定义 DID 文档的签名需要几个 controller 的签名才能生效。比如一个自定义 DID 为3个 controller 持有，任何两个 controller 签名都可以作为 DID 文档的签名，那么可以在文档创建的时候指定3签2（2 of 3）的多签规则。这个多签规则作用于文档的任何修改。

Customized DID can be held by one or more controllers. In the case of a single controller, this controller owns all the ownership of customized DID, and any modification and signature of the document of the customized DID can only be completed by this controller. In the case of multiple controllers, a rule of multiple signatures can be defined, that is, you can specify that the signatures of several controllers are required for the customized DID document to take effect. For example, if a customized DID is held by three controllers, and the signatures of any two controllers can be used as the signatures of the DID document, the multi-signature rule of 3 of 2 can be specified when the document is created. This multi-signature rule applies to any modification of the document.
