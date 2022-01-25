# How to Use and Verify Presentation

Presentation offers various methods to get internal elements, e.g. id, holder, number of credentials, and different credentials. See “API document” for specifics.

Presentation also provides a method of checking integrity and validity, which not only checks the correctness of each element of the presentation itself but also verifies the validity of internal elements such as holder and credential.

## Usage

```typescript
public isGenuine(
	listener: VerificationEventListener = null
): boolean；
```

Presentation offers a method to check the integrity of the presentation and its various elements, e.g. the integrity of the presentation holder and the encapsulated credentials.

Listener obtains the error information set in the verification process, and locates the points of failure of specific deep calls.

```typescript
public isValid(
	listener: VerificationEventListener = null
): boolean ；
```

Presentation offers a method to verify the validity of the presentation and its various elements, e.g. the validity of the presentation holder and the encapsulated credentials. Validity includes integrity.
