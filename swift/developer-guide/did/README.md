# DID

DID 对象用户身份的代表，每个用户可以有很多 DIDs，在不同的场景中使用不同的 DID。DID 只是一个字符串的标识符，每个 DID 都有一个 DIDDocument 对应，DID 和 DIDDocument 是一组共同的存在，类似于一个硬币的两面。Elastos DID 支持两种 DID，一种是普通的 DID，DID 的标识符是由 DID 默认的公钥通过 Hash 运算衍生出来的一个 base58 的字符串，用户不能直接指定；另外一种是自定义 DID，即 DID 的标识符是用户自己指定的。

通常的情况下，在 DID 所有者使用 DID 前，需要将 DID 发布到 ID 侧链上。给定一个 DID 标识符，都可以通过 ID 侧链解析出其对应的 DIDDocument。所以在日常使用中，只要提供 DID 的标识符即可表达用户的身份，应用可以通过该标识符获取 DIDDocument，并进行必要的身份验证的相关的操作。

## 普通 DID

因为普通 DID 的 DID 标识符和默认密钥对的关联关系，从而形成了对该 DID 进行验证的可信根，并且据此形成完整的 DID 验证链，保证DID 身份的安全可信。

因为普通 DID 标识符和密钥的对应关系，原则上不能进行所有权转移，因为不能保证转移的有效性。

## 自定义 DID

自定义 DID 的标识符是可以用户自己指定，没有类似普通 DID 的可信根，所以自定义 DID 不能独立存在，必须由普通 DID 创建出来，并且由普通 DID 所拥有并控制，间接的建立可信根，从而形成对 DID 的可信验证链。自定义 DID 对外可以独立使用，和普通 DID 没有差别。

自定义 DID 可以进行所有权转移，即变更 controller，详细说明参见[自定义 DID 变更所有权](transfer-the-ownership-of-the-customized-did.md)。

自定义 DID 可以由1个或者多个 controller 所持有，对于单一 controller 的情况，该 controller 拥有自定义 DID 的全部所有权，对自定义 DID 的文档的任何修改和签名仅能由该 controller 完成。对于多个 controller 持有的情况，可以定义一个多签的规则，即指定自定义 DID 文档的签名需要几个 controller 的签名才能生效。比如一个自定义 DID 为3个 controller 持有，任何两个 controller 签名都可以作为 DID 文档的签名，那么可以在文档创建的时候指定3签2（2 of 3）的多签规则。这个多签规则作用于文档的任何修改。







