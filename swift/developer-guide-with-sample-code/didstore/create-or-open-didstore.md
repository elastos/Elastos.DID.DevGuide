---
description: 初始化或者打开DIDStore，需要指定ElastosDIDSDK持久化的路径
---

# Create or open DIDStore

请指定 "YOUR-ElastosDIDSDK-INFO-PRESISTENT-PATH"

```
let rootPath = "YOUR-ElastosDIDSDK-INFO-PRESISTENT-PATH"
let store = try DIDStore.open(atPath: rootPath)
```

通过store实例可以对本地存储的DIDDocument、RootIdentity、VerifiableCredential和PrivateKey进行增删改查、导出为zip、同步链上的DIDDocument到本地，也可以把DIDDocument zip等文档导入到给定的YOUR-ElastosDIDSDK-INFO-PRESISTENT-PATH路径下。具体使用详见DOC说明文档。

