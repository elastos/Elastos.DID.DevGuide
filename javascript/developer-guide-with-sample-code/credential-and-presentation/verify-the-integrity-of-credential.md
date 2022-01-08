# Verify the Integrity of Credential

For security, it's necessary to verify the availability of the credential by checking whether the content of the credential is valid, expired, or revoked.

## Usage

```typescript
public async isGenuine(
	listener: VerificationEventListener = null
): Promise<boolean>；
```

Credential provides a method to check whether the credential is complete, each element of the credential is qualified, it's verifiable, and has not been tampered with.

Listener obtains the error information set in the verification process and locates the points of failure of specific deep calls.

```typescript
public async isExpired(): Promise<boolean>；
```

Credential offers a method of checking if the credential expires. The validity period of the credential cannot be greater than that of the document of the DID.

```typescript
public async isRevoked(): Promise<boolean>；
```

Credential offers a method of checking whether the credential is revoked.

```typescript
public async isValid(
	listener: VerificationEventListener = null
): Promise<boolean>；
```

Credential offers a method of comprehensively verifying whether the credential is valid, which includes the above three rules.
