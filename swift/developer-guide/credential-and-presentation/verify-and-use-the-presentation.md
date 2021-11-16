# Verify and use the presentation

VerifiablePresentation 是一个 sealed 对象，创建后为创建者的签名所保护，不能进行任何修改，所有的修改都会破坏其完整性。在使用 VerifiablePresentation 前需要先验证其有效性，以及包含的凭证的有效性，在有效的前提下才能采信并使用凭证中包含的信息。

## 验证 presentation

对 presentation 的验证需要包含对 presentation 本身的签名，以及内部包含的所有凭证进行验证。VerifiablePresentation 的 isValid() 方法，包含了对所有上述项目的验证，使用示例：

```
let serializedVp: String = ... // a serialized presentation
VerifiablePresentation vp = try VerifiablePresentation.fromJson(serializedVp);
let valid = try vp.isValid()
```

## 读取 VerifiablePresentation 的信息

VerifiablePresentation 提供了方法可以读取所包含的凭证，以及验证信息，示例：

```
let serializedVp: String = ... // a serialized presentation
let vp = try VerifiablePresentation.fromJson(serializedVp)

// get the holder
let holder = vp.holder
// get the embedded credentials
let vcs = vp.credentials
// get the Proof
let proof = vp.proof
let realm = proof.realm
let nonce = proof.nonce
```

 
