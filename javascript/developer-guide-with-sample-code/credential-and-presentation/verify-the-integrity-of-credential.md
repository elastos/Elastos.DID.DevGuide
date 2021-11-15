# Verify the integrity of credential

为了安全，需要检查凭证内容本身是否有效，其是否过期，是否被撤销，以检查凭证的可用性。

## Usage

```typescript
public async isGenuine(
	listener: VerificationEventListener = null
): Promise<boolean>；
```

Credential提供检查凭证是否完整，凭证各元素是否符合要求，凭证是否可验签，未被篡改的方法。

listener是用于获取验证过程中的错误信息集，找到具体深层调用的错误点，便于定位。

```typescript
public async isExpired(): Promise<boolean>；
```

Credential提供检查Credential是否过期的方法。Credential的有效期不可以超过所属DID Document的有效期。

```typescript
public async isRevoked(): Promise<boolean>；
```

Credential提供检查凭证是否被撤销的方法。

```typescript
public async isValid(
	listener: VerificationEventListener = null
): Promise<boolean>；
```

Credential提供全面检查凭证是否有效的方法，其包含了以上三条检查。
