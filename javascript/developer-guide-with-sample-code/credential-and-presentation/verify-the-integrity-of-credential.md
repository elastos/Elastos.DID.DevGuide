# Verify the integrity of credential

为了安全，需要检查凭证内容本身是否有效，其是否过期，是否被撤销，以检查凭证的可用性。

For security, it is necessary to verify the availability of the credential by checking whether the content of the credential is valid, expired or revoked.

## Usage

```typescript
public async isGenuine(
	listener: VerificationEventListener = null
): Promise<boolean>；
```

Credential提供检查凭证是否完整，凭证各元素是否符合要求，凭证是否可验签，未被篡改的方法。

Credential provides a method to check whether the credential is complete, whether each element of the credential is qualified, whether the credential is verifiable, and whether it has not been tampered with.

listener是用于获取验证过程中的错误信息集，找到具体深层调用的错误点，便于定位。

Listener obtains the error information set in the verification process, and locates the points of failure of specific deep calls.

```typescript
public async isExpired(): Promise<boolean>；
```

Credential提供检查Credential是否过期的方法。Credential的有效期不可以超过所属DID Document的有效期。

Credential offers a method of checking if the credential expires. The validity period of the credential cannot be greater than that of the document of the DID.

```typescript
public async isRevoked(): Promise<boolean>；
```

Credential提供检查凭证是否被撤销的方法。

Credential offers a method of checking whether the credential is revoked.

```typescript
public async isValid(
	listener: VerificationEventListener = null
): Promise<boolean>；
```

Credential提供全面检查凭证是否有效的方法，其包含了以上三条检查。

Credential offers a method of comprehensively verifying whether the credential is valid, which includes the above three rules.
