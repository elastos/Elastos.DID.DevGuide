# Package the Credentials as Presentation for the Selective Disclosure Request

Verifiable presentation refers to a data set containing a subset of verifiable credentials and countersign of an entity, which is used to show the identity to a third party.

Verifiable presentation can also be an empty presentation entity rather than include any verifiable credential.

## Example

```typescript
//DID has two 'TwitterCredential' and 'PassportCredential' credentials to package
let pb = await VerifiablePresentation.createFor(doc.getSubject(), null, store);
let vp = await pb
	.credentials(doc.getCredential("#profile"))
	.credentials(doc.getCredential("#email"))
	.credentials(TwitterCredential)
	.credentials(PassportCredential)
	.realm("https://example.com/")
	.nonce("873172f58701a9ee686f0630204fee59")
	.seal(TestConfig.storePass);
// to use presentation
... ... ... ...
```

## Usage

```typescript
public static async createFor(
	did: DID | string,
	signKey: DIDURL | string | null,
	store: DIDStore
): Promise<VerifiablePresentation.Builder>;
```

This method gets the builder object to generate the presentation of the specified DID.

did is the owner of the presentation; signKey is the authentication key for the owner of the presentation to sign and encapsulate the presentation.

```typescript
public credentials(...credentials: VerifiableCredential[]): Builder;
```

This method applies to adding VerifiableCredential.

```typescript
public realm(realm: string): Builder;
```

VerifiablePresentation.Builder provides a method to set realm, which indicates the applicable domain and address of this presentation.

```typescript
public nonce(nonce: string): Builder;
```

VerifiablePresentation.Builder provides a method to set nonce, which is a random value used by the sign action.

```typescript
public async seal(storepass: string): Promise<VerifiablePresentation>;
```

VerifiablePresentation.Builder provides the encapsulation method, which finally obtains the VerifiablePresentation.
