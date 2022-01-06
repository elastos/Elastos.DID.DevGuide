---
description: DID Native SDK 是 Elastos DID 针对 C 语言开发者提供的 SDK 版本。
---

# Introduction

DID Native SDK is the SDK version provided by Elastos DID for C language developers.

## Features

DID Native SDK 的实现符合 W3C 的 [DID 规范](https://www.w3.org/TR/did-core/)，以及[可验证凭证规范](https://www.w3.org/TR/vc-data-model/)，主要实现的功能集如下：

The implementation of DID Native SDK conforms to W3C’s DID specification and verifiable credential specification, and the main function set is as follows:

* DID and DIDDocument
* Verifable credential
* Verifable presentation
* Hierarchical Deterministic DIDs (similar with [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki))
* Secure local DID objects store
* DID and credential publishing mechanism base on Elastos ID side chain

另外，因为 [JSON Web Token(JWT)](https://jwt.io) 被应用广泛的采用，而 DID 的密钥对可以用于 JWT 的签名和验证。所以 Elastos DID SDK 集成了 JWT 的支持，为应用提供了安全便捷的使用 JWT 的方法，而且没有密钥泄露的风险。Native DID SDK 集成的 JWT 提供了如下的支持：

In addition, because JSON Web Token (JWT) is widely used, the key of DID can be used for signing and verifying JWT. Therefore, Elastos DID SDK integrates the support of JWT, which provides a safe and convenient way to use JWT for applications, and there is no risk of key leakage. The JWT integrated by Native DID SDK provides the following support:

* JWT
* JWS (algorithm: ECDSA using P-256 and SHA-256)
* Claims assertions
* Claim POJO marshaling and unmarshaling

## Repository and download

c/c++代码位于github仓库。

具体下载链接：https://github.com/elastos/Elastos.DID.Native.SDK.git

c/c++ code is in github repository.

Download link: https://github.com/elastos/Elastos.DID.Native.SDK.git

## License

elastos/Elastos.DID.Native.SDK is licensed under the MIT License.

```
Copyright (c) 2019 - 2021 Elastos Foundation

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"),to deal in the Software without restriction, including without limitationthe rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```
