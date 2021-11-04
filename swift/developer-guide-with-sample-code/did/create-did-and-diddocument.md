---
description: >-
  DID确定DID主体，并解析为对应的DID文档，DIDDocument不是可以和DID主体分开的资源，DIDDocument是由DIDController控制的DID解读信息，用于描述DID主体。DID文档中有一个必须属性ID：用于定义
  DID 主体的标识符。
---

# Create DID and DIDDocument

1：首次生成DID：生成DID需要通过RootIdentity

```
// STORE-PASSWORD用于本地一些数据加密的密码
let document = try identity.newDid("STORE-PASSWORD")
// 获取subject
let subject = document.subject
// 获取contollers
let contollers = document.contollers()
```

2： 从外部导入DIDDocument：SDK对外提供了5种方法，可以从url路径、json字符串、Dictionary、Data生成DIDDocument。

```
// 从路径加载document
let contollers = DIDDocument.convertToDIDDocument(fromFileAtPath: "YOUR-FILE-PATH-STRING")
// 从路径URL获取
let contollers = DIDDocument.convertToDIDDocument(fromUrl: URLPath)
```



DID subject 和 DID Controller 有2种情况：1： DID Subject 是DID Controller ，如：当个人建立DID并进行自我识别时。2： DID Subject 不是DID Controller，如：当父母为孩子创建DID并保持控制时。DIDDocument可以有多个DIDController。

DIDDocument中的每个信息都是关于DID控制人的：

* 字符串，用于定义 DID 主体的标识符
* 如何与 DID 主体进行交互
