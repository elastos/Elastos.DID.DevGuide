# Verify the integrity of DIDDocument

## Verify the integrity of DIDDocument

为了安全和链上文档的统一，需要检查DID Document内容本身是否有效，其是否过期，是否失效，以检查文档的可用性。

For security and the unity of documents on the chain, it is necessary to verify the usability of the DID document by checking whether the DID document itself is valid, whether it is expired or invalid.

## Usage

```typescript
public isGenuine(
	listener: VerificationEventListener = null
): boolean；
```

DID Document提供检查文档是否完整，文档各元素是否符合要求，文档是否可验签，未被篡改的方法。比如Customized DID Dodument的signature个数是否符合多签规则。

The DID document provides a method to check whether the document is complete, whether each element of the document meets the requirements, whether the document can be verified and signed, and whether the document has been tampered with. For instance, whether the number of signatures of the customized DID document meets the multi-signature rule.

listener是用于获取验证过程中的错误信息集，找到具体深层调用的错误点，便于定位。

Listener obtains the error information set in the verification process, and locates the points of failure of specific deep calls.

```typescript
public isExpired(): boolean；
```

DID Document提供检查DID Document是否过期的方法。

The DID document offers a method of checking if the DID document expires.

```typescript
public isDeactivated(): boolean；
```

DID Document提供检查DID Document是否已经失效的方法。失效非过期，失效需要DID自己或者委托者来操作失效文档，而过期是根据有效期判断是否超出有效期范围，可再重置恢复。

The DID document offers a method of checking if the DID document is invalid. Invalidity is not equivalent to expiration. In the case of invalidity, the invalid document should be operated by the DID itself or the client. Whether the document expires is determined by whether it is beyond its validity period, and the document that expires can be reset and restored.

```typescript
public isValid(
	listener: VerificationEventListener = null
): boolean ；
```

DID Document提供全面检查DID Document是否有效的方法，其包含了以上三条检查。

The DID document offers a method of comprehensively verifying whether the DID document is valid, which includes the above three rules.
