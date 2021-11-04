---
description: 把文档发布到链上，生成的did文档发布到链上后真正的生效。文档发布到链上分为同步和异步两种机制。
---

# Publish DID

1. 例子: 发布普通的document，对document编辑签名之后，发布上链。

```
try documet.publish(using: "YOUR-STORE-PASSWORD")
```

2\. 转让document并发布到链上，首先创建TransferTicket，参数did： 为需要转让的DID，to：接受转让的did（转让个给谁），storePassword：本地私钥加密密码。

```
// create new controller
let newController = try identity.newDid(storePassword)
let ticket = try controller.createTransferTicket(withId: did, to: newController.subject, using: storePassword)
// create new document for customized DID
let newDoccument = try newController.newCustomizedDid(withId: did, true, storePassword)
```



