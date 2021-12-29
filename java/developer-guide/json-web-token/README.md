# Json Web Token

JSON Web Token(JWT) 目前的使用非常广泛，而 DID 本身的密钥可以用来给 JWT 进行 token 的签名和验证。如果采用外部的 JWT 实现，势必需要将 DID 的密钥对从 DID 中导出，并提供给第三方的 JWT 库，这个过程需要开发者干预，存在比较多的安全隐患。

At present, JSON Web Token (JWT) is widely used, and the key of DID itself can be used to sign and verify the token for JWT. If external JWT implementation is adopted, it is necessary to export the key from DID and provide it to the JWT library of a third party. This process requires the intervention of developers, and there are many security risks.

据此，Elastos DID 内置了 JWT 的支持。可以直接使用 DID SDK 提供的接口和方法创建、验证 JWT。

Therefore, Elastos DID has built-in JWT support. JWT can be created and verified directly by using the interfaces and methods provided by DID SDK.
