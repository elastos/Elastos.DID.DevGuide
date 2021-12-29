# Transfer the ownership of the customized DID

自定义 DID 因为没有和验证密钥的强关联，是可以变更持有人的。对自定义 DID 而言，修改多签规则、修改持有人，都是属于变更 DID 所有权。

Customized DID can change the controller because it is not strongly associated with the authentication key. For the customized DID, modifying the multi-signature rule and changing the controller are all changes to the ownership of DID.

变更 DID 所有权需要原 DID 的持有人按照当前 DID 文档的多签规则，签署一个转让凭证（TransferTicket）给新的持有人之一。新的持有人就可以重新创建该 DID 的文档，声明新的 controllers 以及多签规则，并使用原持有人提供的转让凭证发布新的 DID 到 ID 侧链，从而实现 DID 所有权的转移。

Changing the ownership of DID requires the controller of the original DID to sign a Transfer Ticket to one of the new controllers according to the multi-signature rule of the current DID document. The new controller can re-create the DID document, declare new controllers and multi-signature rule, and issue a new DID-to-ID side chain using the Transfer Ticket provided by the original controller, thus realizing the transfer of DID ownership.

比如 `did:elastos:example` 目前由 Alice、Bob 和 Carol 持有3签2的 DID，他们三个的 DID 分别是：

For example, presently, the 2 of 3 DID did:elastos:example is held by Alice, Bob and Carol, and their DIDs are:

* Alice - did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz
* Bob - did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw
* Carol - did:elastos:ignLDwpCumFLA2GrXptr5zNugtMcfvoM4d

需要将 `did:elastos:example` 的所有权转让给 Dan、Erin，他们的 DID 分别是：

Now, it is needed to transfer the ownership of did:elastos:example to Dan and Erin, whose DIDs are:

* Dan - did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p
* Erin - did:elastos:iTsF892QDbDu3JnHXzPbE6udK9b5TZQdTo

## 1. Alice 创建一个 TransferTicket

1.Alice created a Transfer Ticket.

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID alice = new DID("did:elastos:iXkgy7RruwjJDP2o3pfPaxpVR65bEzzjtz");
DID dan = new DID("did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p");
DID example = new DID("did:elastos:example");

// Alice get her document
DIDDocument aliceDoc = store.loadDid(alice);

// Create the TransferTicket for example to Dan
TransferTicket ticket = aliceDoc.createTransferTicket(
    example,   // the customized DID to be transfer
    dan,       // one of the new controllers, Dan or Erin both are ok
    storePasswd);

// Serialize the ticket to the string
String serializedTicket = ticket.serialize();
```

在 DID 的所有权转移凭证中，只要声明新的持有人中的任何一个即可，本示例声明转让给 Dan。

In DID’s ownership Transfer Ticket, we needed to declare one of the new controllers. The DID example was declared to transfer to Dan.

Alice 创建的 TransferTicket 只有 Alice 的签名，还没有达到 2 个签名的要求。所以 Alice 需要将这个文档序列化，并通过应用提供的途径交给 Bob 或者 Carol 对 DIDDocument 继续进行签名。本示例假定由 Bob 进行签名。

The Transfer Ticket created by Alice only got the signature of Alice, which didn’t reach the requirement of two. So, Alice needed to serialize this document and give it to Bob or Carol to continue signing through the way provided by the application. This example assumes that Bob would sign it.

## 2. Bob 对 TransferTicket 进行签名

2.Bob signed the Transfer Ticket.

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID bob = new DID("did:elastos:iUCXpvpNZ9B9vwWf6PMFMJ6duRPPiLSWSw");
String serializedTicket; // the serialized ticket got from Alice

// Bob get his document
DIDDocument bobDoc = store.loadDid(bob);
TransferTicket ticket = TransferTicket.parse(serializedTicket);

// Bob sign the ticket
TransferTicket signedTicket = bobDoc.sign(ticket, storePasswd);

// Serialize the ticket to the string
String finalTicket = ticket.serialize();
```

Bob 签署好 TransferTicket 后，TransferTicket 中已经包含了 Alice 和 Bob 两个签名，符合了 example DID 声明的多签约定，是一个有效的转让凭证。

After Bob signed the Transfer Ticket, it already contained signatures of Alice and Bob, which met the requirement of the multi-signature rule.

example DID 的现在持有人只需将这个签署好有效的转让凭证转交给 Dan，Dan 就可以使用这个转让凭证发布新的 DID 文档。

The current controller of example DID only needed to hand over this signed and valid Transfer Ticket to Dan, and Dan could use this ticket to publish a new DID document.

## 3. Dan 和 Erin 重新创建 DID 文档

3.Dan and Erin recreated the DID document.

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID dan = new DID("did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p");
DID erin = new DID("did:elastos:iTsF892QDbDu3JnHXzPbE6udK9b5TZQdTo");
DID example = new DID("did:elastos:example");

// Alice get her document
DIDDocument danDoc = store.loadDid(dan);

// Create example's DID
DIDDocument exampleDoc = danDoc.newCustomizedDid(
    example,                         // the new customized DID
    new DID[] { dan, erin },         // the 3 controllers
    2,                               // needs 2 signatures
    storePasswd);

// Serialize the document to the string
String serializedDoc = exampleDoc.serialize();
```

然后将待签署的文档交由 Erin 签署：

Then Erin would sign the document:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID erin = new DID("did:elastos:iTsF892QDbDu3JnHXzPbE6udK9b5TZQdTo");
String serializedDoc; // the serialized document got from Alice

// Erin get his document
DIDDocument erinDoc = store.loadDid(erin);
DIDDocument exampleDoc = DIDDocument.parse(serializedDoc);

// Erin sign the example's DID document
DIDDocument signedExampleDoc = erinDoc.sign(exampleDoc, storePasswd);
```

> 因为 TransferTicket 是给 Dan 的，所以 Dan 必须是新的 DID 的 controller 之一，并且在第一次发布的 DID 文档中必须要包含 Dan 的签名。
>
> Because this Transfer Ticket is for Dan, Dan must be one of the controllers of the new DID, and the first published DID document must contain Dan’s signature.
>
> &#x20;

## 4. 使用 TransferTicket 发布新的 DID 文档完成所有权转移

4.Use Transfer Ticket to publish a new DID document to complete the ownership transfer

在 Dan 和 Erin 创建并签署好有有效的 DID 文档后，任何一个人都可以凭之前得到的 example 的转让凭证来发布新的 DID 来声明对 example 的所有权。假设由 Dan 来发布 example 的新的 DID 文档：

After Dan and Erin created and signed a valid DID document, anyone could issue a new DID with the ticket of example obtained before to declare the ownership of example. Suppose Dan published the new DID document of example:

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
DID dan = new DID("did:elastos:ice6yo4NuazaJP9hwgXxhgmNVGssusEz7p");
String serializedNewExampleDoc; // the serialized DIDDocument for example
String serializedTicket; // the serialized TransferTicket

// Get the new DIDDocument for example DID
DIDDocument exampleDoc = DIDDocument.parse(serializedNewExampleDoc);
// Parse the TransferTicket
TransferTicket ticket = TransferTicket.parse(serializedTicket);

exampleDoc.setEffectiveController(dan);
exampleDoc.publish(ticket, storePasswd);
```

> 在转移 DID 所有权时，TranferTicket 中包含有目标 DID 的最后链上状态，所以一旦创建了 TransferTicket，那么原始的持有人就不应该对改 DID 进行更新操作了。如果发生了更新操作，那么之前创建的 TransferTicket 就会失效，使用失效的 ticket 发布新的文档时，ID 侧链会拒绝这个交易。
>
> When transferring the ownership of DID, the Tranfer Ticket contains the last chain state of the target DID, so once the Transfer Ticket is created, the original controllers should not update it. If there is an update operation, the previously created Transfer Ticket will become invalid. When a new document is published using the invalid ticket, the ID side chain will reject this transaction.
