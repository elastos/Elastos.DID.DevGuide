# Introduction

DID Java SDK 是 Elastos DID 针对 Java 语言开发者提供的 SDK 版本，兼容的最低 Java 语言版本是 Java 1.8(aka Java 8)。可以在 desktop 或者 server 端的标准 Java 环境，以及 Android Java 环境中使用。

Java SDK 发布了两个主流的版本 1.x 和 2.x，其中 1.x 已经废弃不再维护并演进，2.x 是目前开发和维护的版本。这两个版本的 API 不互相兼容，但是 2.x 版本的 SDK 可以兼容处理 1.x 生成的 DID 对象。当前最新的 SDK 版本是 2.1.x 系列，如果在应用开发中新导入 Elastos DID 支持，建议使用 2.x 最新的 release 版本。

## Features

DID Java SDK 的实现符合 W3C 的 [DID 规范](https://www.w3.org/TR/did-core/)，以及[可验证凭证规范](https://www.w3.org/TR/vc-data-model/)，主要实现的功能集如下：

- DID and DIDDocument
- Verifable credential
- Verifable presentation
- Hierarchical Deterministic DIDs (similar with [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki))
- Secure local DID objects store
- DID and credential publishing mechanism base on Elastos ID side chain

另外，因为 [JSON Web Token(JWT)](https://jwt.io/) 被应用广泛的采用，而 DID 的密钥对可以用于 JWT 的签名和验证。所以 Elastos DID SDK 集成了 JWT 的支持，为应用提供了安全便捷的使用 JWT 的方法，而且没有密钥泄露的风险。Java DID SDK 集成的 JWT 提供了如下的支持：

- JWT
- JWS (algorithm: ECDSA using P-256 and SHA-256)
- Claims assertions
- Claim POJO marshaling and unmarshaling

## Repository and download

DID Java SDK 是 open source 的实现，代码仓库托管在 GitHub，仓库的地址是：

  https://github.com/elastos/Elastos.DID.Java.SDK

## Support

Elastos DID Java SDKs are supported by Trinity Tech and the community developers through GitHub issues. Also, we would love to get your contributions, whether they are in form of bug reports, Requests for Enhancement (RFE), documentation, or code patches.

## License

```
MIT License

Copyright (c) 2019 - 2021 elastos.org

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

