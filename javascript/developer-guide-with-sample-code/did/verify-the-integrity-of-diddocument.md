# Verify the Integrity of DIDDocument

For security and the unity of documents on the chain, it's necessary to verify the usability of the DID document by checking whether the DID document itself is valid, expired, or invalid.

## Usage

```typescript
public isGenuine(
	listener: VerificationEventListener = null
): boolean；
```

The DID document provides a method to check whether the document is complete, each element of meets the requirements, whether can be verified and signed, or it's been tampered with. For instance, whether the number of signatures of the customized DID document meets the multi-signature rule is important to understand.

The listener obtains the error information set in the verification process and locates the points of failure of specific deep calls.

```typescript
public isExpired(): boolean；
```

The DID document offers a method for checking if it expires.

```typescript
public isDeactivated(): boolean；
```

The DID document also provides a method of checking if it's invalid. Invalidity is not equivalent to expiration. In the case of invalidity, the invalid document should be operated by the DID itself or the client. Whether the document expires is determined by whether it is beyond its validity period, and the document that expires can be reset and restored.

```typescript
public isValid(
	listener: VerificationEventListener = null
): boolean ；
```

The DID document offers a method of comprehensively verifying whether it's valid, which includes the above three rules.
