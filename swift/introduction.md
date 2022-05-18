# Introduction

The DID Swit SDK is the SDK version of Elastos DID provided for Swift language developers, which is compatible with Xcode 10.0 and IOS 11.0 at the minimum.

## Features

The implementation of DID Swift SDK conforms to the DID specification of [W3C](https://www.w3.org/TR/did-core/) and verifiable credentials, which mainly realizes the following sets of functions:

* DID and DID Document
* Verifiable credential
* Verifiable presentation
* Hierarchical Deterministic DIDs (similar with [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki))
* Secure local DID objects store
* DID and credential publishing mechanism base on Elastos ID side chain

In addition, [JSON Web Token](https://jwt.io) (JWT) is widely used in applications, while the key pair of DID can be used for signing and verifying JWT. Therefore, supported by JWT, the Elastos DID SDK provides applications with a safe and convenient way to use JWT, without the risk of exposing the keys.

## Repository and Download

The DID Swift SDK is the implementation of open source, where the source code repository is hosted in GitHub. The address of the repository is: [https://github.com/elastos/Elastos.DID.Swift.SDK.git](https://github.com/elastos/Elastos.DID.Swift.SDK.git)

## Support

Elastos DID Swift SDKs are supported by Trinity Tech and the community developers through GitHub issues. Also, we would love to receive your contributions, whether they're in form of bug reports, Requests for Enhancement (RFE), documentation, or code patches.

## License

MIT License

Copyright (c) 2018 Elastos Foundation

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
