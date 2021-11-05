# Create multi-signed customized DID

创建多签文档：

```
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
```

参数：

* did: the new customized identifier
* controllers: the other controllers for the new customized DID
* multisig: how many signatures are required
* storePassword: the password for the DIDStore
