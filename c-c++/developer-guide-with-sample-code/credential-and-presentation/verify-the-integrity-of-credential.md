# Verify the Integrity of Credential

For security, it's necessary to check whether the content of the credential itself is valid, expired, or revoked so as to check the availability of the credential.

## Usage

```c
int Credential_IsGenuine(Credential *credential);
```

This method checks whether the credential is complete, each element meets the requirements, can be verified and signed, and has been tampered with.

```c
int Credential_WasDeclared(DIDURL *id);
```

This method is used to check whether Credential has been declared.

```c
int Credential_IsRevoked(Credential *credential);
```

This method checks whether the credential has been revoked.

```c
int Credential_IsValid(Credential *credential);
```

This method comprehensively checks whether the credential is valid.
