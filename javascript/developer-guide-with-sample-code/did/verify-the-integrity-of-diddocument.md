# Verify the integrity of DIDDocument

为了安全和链上文档的统一，需要检查DID Document内容本身是否有效，其是否过期，是否失效，以检查文档的可用性。

# Usage

```typescript
public isGenuine(
	listener: VerificationEventListener = null
): boolean；
```
该方法检查文档是否完整，文档各元素是否符合要求，文档是否可验签，未被篡改。比如Customized DID Dodument的signature个数是否符合多签规则。

listener是用于获取验证过程中的错误信息集，找到具体深层调用的错误点，便于定位。

```typescript
public isExpired(): boolean；
```
该方法检查DID Document是否过期。

```typescript
public isDeactivated(): boolean；
```
该方法检查DID Document是否已经失效。失效非过期，失效需要DID自己或者委托者来操作失效文档，而过期是根据有效期判断是否超出有效期范围，可再重置恢复。

```typescript
public isValid(
	listener: VerificationEventListener = null
): boolean ；
```
该方法是全面检查DID Document是否有效，其包含了以上三条检查。
