# Verify the integrity of credential

为了安全，需要检查凭证内容本身是否有效，其是否过期，是否被撤销，以检查凭证的可用性。

# Usage

```c
int Credential_IsGenuine(Credential *credential);
```
该方法检查凭证是否完整，凭证各元素是否符合要求，凭证是否可验签，未被篡改的方法。

```c
int Credential_WasDeclared(DIDURL *id);
```
该方法用于检查Credential是否被declare过。

```c
int Credential_IsRevoked(Credential *credential);
```
该方法检查凭证是否被撤销。

```c
int Credential_IsValid(Credential *credential);
```
该方法全面检查凭证是否有效。

