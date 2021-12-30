# Transfer the ownership of the customized DID

自定义 DID 因为没有和验证密钥的强关联，是可以变更持有人的。对自定义 DID 而言，修改多签规则、修改持有人，都是属于变更 DID 所有权。

The customized DID is not strongly associated with the authentication key, so the controller of the customized DID can be changed. Modifying the multi-signature rule and the controller of the customized DID are equivalent to a change of the ownership of DID.

变更 DID 所有权需要原 DID 的持有人按照当前 DID 文档的多签规则，签署一个转让凭证（TransferTicket）给新的持有人之一。新的持有人就可以重新创建该 DID 的文档，声明新的 controllers 以及多签规则，并使用原持有人提供的转让凭证发布新的 DID 到 ID 侧链，从而实现 DID 所有权的转移。

To change the ownership of DID, the original controller of DID should sign a TransferTicket and give it to one of the new controllers per the multi-signature rule of the current DID document. The new controller can transfer the ownership of the customized DID by re-creating the DID document, clarifying the new controllers and multi-signature rule, and publishing a new DID to the ID side chain with the transfer ticket provided by the original controller.

比如 `did:elastos:example` 目前由 Alice、Bob 和 Carol 持有3签2的 DID，他们三个的 DID 分别是：

For instance, did:elastos:example is currently owned by Alice, Bob and Carol, which requires 2 signatures of the 3 controllers. The DIDs of Alice, Bob and Carol are as follows:

* Alice - did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz
* Bob - did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw
* Carol - did:elastos:ignLDwpCumFLA2GrXptr5zNugtMcfvoM4d

需要将 `did:elastos:example` 的所有权转让给 Dan、Erin，他们的 DID 分别是：

The ownership of did:elastos:example should be transferred to Dan and Erin whose DIDs are as follows:

* Dan - did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p
* Erin - did:elastos:iTsF892QDbDu3JnHXzPbE6udK9b5TZQdTo

## 1. Alice 创建一个 TransferTicket

1.1.Alice creates a TransferTicket

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let alice = try DID("did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz")
let dan = try DID("did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p")
let example = try DID("did:elastos:example")

// Alice get her document
let aliceDoc = try store.loadDid(alice)

// Create the TransferTicket for example to Dan
let ticket = try aliceDoc.createTransferTicket(withId: example, to: dan, using: storePasswd)

// Serialize the ticket to the string
let serializedTicket = ticket.serialize()
```

主要参数：

Main parameters:

* withId: the customized DID to be transfer
* to: one of the new controllers, Dan or Erin both are ok

在 DID 的所有权转移凭证中，只要声明新的持有人中的任何一个即可，本示例声明转让给 Dan。

The TransferTicket of DID only needs to declare any one of the new controllers. This example declares that the ownership is transferred to Dan.

Alice 创建的 TransferTicket 只有 Alice 的签名，还没有达到 2 个签名的要求。所以 Alice 需要将这个文档序列化，并通过应用提供的途径交给 Bob 或者 Carol 对 DIDDocument 继续进行签名。本示例假定由 Bob 进行签名。

The TransferTicket created by Alice only contains Alice’s signature, which fails to reach the requirement of 2 signatures. Hence, Alice needs to serialize this document and give it to Bob or Carol to continue signing DIDDocument through the way provided by the application. In this example, this document is assumed to be signed by Bob.

## 2. Bob 对 TransferTicket 进行签名

2.Bob signs the TransferTicket

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let bob = try DID("did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw")
let serializedTicket: String = ... // the serialized ticket got from Alice

// Bob get his document
let bobDoc = try store.loadDid(bob)
let ticket = try TransferTicket.deserialize(serializedTicket)

// Bob sign the ticket
let signedTicket = trybobDoc.sign(with: ticket, using:storePasswd)

// Serialize the ticket to the string
let finalTicket = ticket.serialize()
```

Bob 签署好 TransferTicket 后，TransferTicket 中已经包含了 Alice 和 Bob 两个签名，符合了 example DID 声明的多签约定，是一个有效的转让凭证。

Once Bob signs the TransferTicket, the TransferTicket will be a valid transfer ticket, for it contains Alice’s and Bob’s signatures, which accords with the multi-signature rules stipulated by example DID.

example DID 的现在持有人只需将这个签署好有效的转让凭证转交给 Dan，Dan 就可以使用这个转让凭证发布新的 DID 文档。

The current controller of example DID only needs to provide this signed valid transfer ticket to Dan, so that Dan can publish a new DID document with this transfer ticket.

## 3. Dan 和 Erin 重新创建 DID 文档

3.Dan and Erin re-create the DID document

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let dan = try DID("did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p")
let erin = try DID("did:elastos:iTsF892QDbDu3JnHXzPbE6udK9b5TZQdTo")
let example = try DID("did:elastos:example")

// Alice get her document
let danDoc = try store.loadDid(dan)

// Create example's DID
DIDDocument exampleDoc = danDoc.newCustomizedDid(withId: example, [dan, erin], 2, true, storePassword)

// Serialize the document to the string
let serializedDoc = try exampleDoc.serialize()
```

然后将待签署的文档交由 Erin 签署：

Then submit the document to be signed to Erin for signing:

```
let store: DIDStore = ... // an opened DIDStore instance
let storePasswd = "secret"
let erin = try DID("did:elastos:iTsF892QDbDu3JnHXzPbE6udK9b5TZQdTo")
let serializedDoc: String = ... // the serialized document got from Alice

// Erin get his document
let erinDoc = store.loadDid(erin)
let exampleDoc = try DIDDocument.convertToDIDDocument(fromJson: serializedDoc)

// Erin sign the example's DID document
let signedExampleDoc = erinDoc.sign(with: exampleDoc, using: storePasswd)
```

> 因为 TransferTicket 是给 Dan 的，所以 Dan 必须是新的 DID 的 controller 之一，并且在第一次发布的 DID 文档中必须要包含 Dan 的签名。
>
> Since TransferTicket is for Dan, Dan must be one of the controllers of the new DID, and his signature must be included in the DID document published for the first time.

## 4. 使用 TransferTicket 发布新的 DID 文档完成所有权转移

4.Complete ownership transfer by publishing the new DID document using TransferTicket

在 Dan 和 Erin 创建并签署好有有效的 DID 文档后，任何一个人都可以凭之前得到的 example 的转让凭证来发布新的 DID 来声明对 example 的所有权。假设由 Dan 来发布 example 的新的 DID 文档：

After Dan and Erin create and sign the valid DID document, any controller can publish a new DID with the obtained transfer ticket of the example to declare the ownership of the example. Suppose Dan publishes the new DID document of example:

```
let store: DIDStore = ...  // an opened DIDStore instance
let storePasswd = "secret"
let dan = try DID("did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p")
let serializedNewExampleDoc: String = ... // the serialized DIDDocument for example
let serializedTicket: String = ... // the serialized TransferTicket

// Get the new DIDDocument for example DID
let exampleDoc = try DIDDocument.convertToDIDDocument(fromJson: serializedNewExampleDoc)
// Parse the TransferTicket
let ticket = TransferTicket.deserialize(serializedTicket)

try exampleDoc.setEffectiveController(dan)
try exampleDoc.publish(with: ticket, using: storePasswd)
```

参数：

Parameters:

The document should have an effective controller, otherwise this method will fail.

* did: the target customized DID to be transfer
* to: who the did will transfer to
* storePassword: the password for the DIDStore

> 在转移 DID 所有权时，TranferTicket 中包含有目标 DID 的最后链上状态，所以一旦创建了 TransferTicket，那么原始的持有人就不应该对改 DID 进行更新操作了。如果发生了更新操作，那么之前创建的 TransferTicket 就会失效，使用失效的 ticket 发布新的文档时，ID 侧链会拒绝这个交易。
>
> When transferring the ownership of DID, the TranferTicket contains the final chain state of the target DID. Therefore, once the TransferTicket is created, the original controller should not update this DID. If there is an update operation, the previously created TransferTicket will become invalid. If the invalid ticket is used to publish a new document, this transaction will be rejected by the ID side chain.
