# verify the integrity of credential

## Verify the integrity of credential

为了安全，需要检查凭证内容本身是否有效，其是否过期，是否被撤销，以检查凭证的可用性。

For security, it is necessary to check whether the content of the credential itself is valid, and whether it is expired or revoked, so as to check the availability of the credential.

## Usage

```c
int Credential_IsGenuine(Credential *credential);
```

该方法检查凭证是否完整，凭证各元素是否符合要求，凭证是否可验签，未被篡改的方法。

This method checks whether the credential is complete, whether each element of it meets the requirements, whether it can be verified and signed, and whether it has been tampered with.

```c
int Credential_WasDeclared(DIDURL *id);
```

该方法用于检查Credential是否被declare过。

This method is used to check whether Credential has been declared.

```c
int Credential_IsRevoked(Credential *credential);
```

该方法检查凭证是否被撤销。

This method checks whether the credential has been revoked.

```c
int Credential_IsValid(Credential *credential);
```

该方法全面检查凭证是否有效。

This method comprehensively checks whether the credential is valid.
