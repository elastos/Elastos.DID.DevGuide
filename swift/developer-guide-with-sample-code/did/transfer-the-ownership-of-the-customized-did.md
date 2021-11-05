# Transfer the ownership of the customized DID

转让自定义DID文档：

```
// Create normal DID first
let controller = try identity.newDid("YOUR-STORE-PASSWORD")
// publish
try controller.publish(using: "YOUR-STORE-PASSWORD")

// Create customized DID
let did = try DID("did:elastos:helloworld")
var doc = try controller.newCustomizedDid(withId: did, "YOUR-STORE-PASSWORD")
try doc.publish(using: "YOUR-STORE-PASSWORD")

// create new controller
let newController = try identity.newDid(storePassword)
try newController.publish(using: storePassword)

// create the transfer ticket
try doc.setEffectiveController(controller.subject)
let ticket = try doc.createTransferTicket(to: newController.subject, using: storePassword)

```

转让多签文档：

```
// ***  创建多签文档 ***
// Create normal DID first
let ctrl1 = try identity.newDid("YOUR-STORE-PASSWORD")
try ctrl1.publish(using: "YOUR STORE-PASSWORD")

let ctrl2 = try identity.newDid("YOUR-STORE-PASSWORD")
try ctrl2.publish(using: "YOUR-STORE-PASSWORD")

let ctrl3 = try identity.newDid("YOUR-STORE-PASSWORD")
try ctrl3.publish(using: "YOUR-STORE-PASSWORD")

// Create multi-signed customized DID
let did = try DID("did:elastos:helloworld")
// 
var doc = try ctrl1.newCustomizedDid(withId: did, [ctrl2.subject, ctrl3.subject],
                    2, storePassword)
doc = try ctrl2.sign(with: doc, using: storePassword)

// 设置controller
try doc.setEffectiveController(ctrl1.subject)
try doc.publish(using: "YOUR-STORE-PASSWORD")

// *** 转让多签文档 ***
// new controllers for the did
let documentUser1 = ...
let documentUser2 = ...
let documentUser3 = ...
let documentUser4 = ...

// transfer ticket
var ticket = try ctrl1.createTransferTicket(withId: did, to: documentUser1.subject, using: "YOUR-STORE-PASSWORD")
ticket = try ctrl2.sign(with: ticket, using: "YOUR-STORE-PASSWORD")
try doc = documentUser1.newCustomizedDid(withId: did, [documentUser2.subject, documentUser3.subject, documentUser4.subject],
                        3, true, "YOUR-STORE-PASSWORD")
try doc = documentUser2.sign(with: doc, using: "YOUR-STORE-PASSWORD")
try doc = documentUser3.sign(with: doc, using: "YOUR-STORE-PASSWORD")

// transfer
try doc.publish(with: ticket, using: storePassword)
```

参数：

The document should have an effective controller, otherwise this method will fail.

* did: the target customized DID to be transfer
* to: who the did will transfer to
* storePassword: the password for the DIDStore
