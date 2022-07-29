# ℹ Introduction

DID Javascript SDK refers to the SDK version of Javascript provided by Elastos DID for Javascript language developers.

## Features

The implementation of DID Javascript SDK accords with W3C’s [DID specification](https://www.w3.org/TR/did-core/) and the specification of verifiable credentials, and it mainly implements the following function sets:

* DID and DIDDocument
* Verifiable credential
* Verifiable presentation
* Hierarchical Deterministic DIDs (similar with [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki))
* Secure local DID objects store
* DID and credential publishing mechanism based on Elastos ID side chain

In addition (since [JSON Web Token (JWT)](https://jwt.io/) is widely used), while the key pair of DID can be used for signing and verifying JWT, Elastos DID SDK integrates the support of JWT, which provides applications with a safe and convenient way to use JWT, without any risk of secret key leakage. JWT integrated with JavaScript SDK offers the following support:

* JWT
* JWS (algorithm: ECDSA using P-256 and SHA-256)
* Claims assertions
* Claim POJO marshaling and unmarshaling

## Repository and Download

The code of Javascript is located in the GitHub repository.

The download link is here for reference: [https://github.com/elastos/Elastos.DID.JS.SDK.git](https://github.com/elastos/Elastos.DID.JS.SDK.git)&#x20;

## License

elastos/Elastos.DID.JS.SDK is licensed under the MIT License.

```
Copyright (c) 2021 Elastos Foundation
Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"),to deal in the Software without restriction, including without limitationthe rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```
