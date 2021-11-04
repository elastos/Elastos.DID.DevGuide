---
description: sdk支持自定义DID
---

# Create customized DID

例子：使用当前controller作为新的自定义的DID的Controller

```
// Create customized DID
let did = try DID("did:elastos:helloworld")
let document = try controller.newCustomizedDid(withId: did, storePassword)
```
