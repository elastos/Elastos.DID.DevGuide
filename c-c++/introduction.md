---
description: >-
  DID Native SDK is the SDK version provided by Elastos DID for C language
  developers.
---

# Introduction

## Features

The implementation of DID Native SDK conforms to [W3C’s DID](https://www.w3.org/TR/vc-data-model/) specification and verifiable credential specification - the main function set is as follows:

* DID and DIDDocument
* Verifiable credential
* Verifiable presentation
* Hierarchical Deterministic DIDs (similar with [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki))
* Secure local DID objects store
* DID and credential publishing mechanism base on Elastos ID side chain

In addition, because [JSON Web Token (JWT)](https://jwt.io/) is widely used, the key of DID can be used for signing and verifying JWT. Therefore, the Elastos DID SDK integrates the support of JWT, which provides a safe and convenient way to use JWT for applications, and there is no risk of key leakage. The JWT integrated by Native DID SDK provides the following support:

* JWT
* JWS (algorithm: ECDSA using P-256 and SHA-256)
* Claims assertions
* Claim POJO marshaling and unmarshaling

## Repository and download

C/C++ code is in GitHub repository.

Download link: [https://github.com/elastos/Elastos.DID.Native.SDK.git](https://github.com/elastos/Elastos.DID.Native.SDK.git)

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
