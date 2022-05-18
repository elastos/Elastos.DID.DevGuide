# Create DIDDocument

DID documents are divided into ordinary and customized DIDs, and everything explained here is applicable to ordinary ones. The customized DID document will be described in detail in the following sections.

Generate the DID document by newDid. This DID document is the most basic, containing only the default key. Users can modify the content of DID document via DID document Builder, and can terminate the modification by the seal method to get a new DID document.

The DID document contains five elements: public key, authentication key, authorization key, verifiable credential, and service. The document can be correspondingly modified by adding or removing methods. Please refer to API doc for specific methods.

## Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
let storePass = "pwd";
... ... ... ...
let rootidentity = await store.loadRootIdentity();
//create did
let doc = await rootidentity.newDid(storePass);
//edit builder
let db = DIDDocument.Builder.newFromDocument(doc).edit();

//add authentication key
let id = DIDURL.from("#test1", db.getSubject());
let key = HDKey.deriveWithPath(HDKey.DERIVE_PATH_PREFIX + 5);
db.addAuthenticationKey(id, doc.getSubject(), key.getPublicKeyBase58());//
//to add/delete other elem
... ... ... ...
//seal DID Document
let doc = await db.seal(storePass);
doc.publish(storePass);
... ... ... ...
store.close();
```

## Usage

The newDid method has been introduced in the chapter of RootIdentity, so it's unnecessary to go into details here.

```typescript
public static newFromDocument(
    doc: DIDDocument,
    controller?: DIDDocument
): Builder;

public edit(
    controller?: DIDDocument
): Builder;
```

newFromDocument is used together with the edit method, as shown in the example.

NewFromDocument copies the original DID document to prevent subsequent modification from affecting the original DID document, and edit checks whether the document signed by DID document subject meets the requirements.

Controller parameter is modified for the customized DID document, which will be explained in detail in the following sections. For ordinary DID document, there is no need to provide parameters.

```typescript
public async seal(
    storepass: string
): Promise<DIDDocument>;
```

The seal method encapsulates and gets a new DID document. Storepass is the password of the DID Store, which is used to sign the main content of the modified DID document via the private key of the master key.

```typescript
public createAndAddPublicKey(
    id: DIDURL | string,
    pk: string,
    controller?: DID | string,
    type = "ECDSAsecp256r1"
): Builder;
```

If this method adds the public key that already exists (with id or pk being the same), an error is returned.

```typescript
public removePublicKey(
    id: DIDURL | string,
    force = false
): Builder；
```

It should be noted that the default key cannot be removed; when you want to remove the authentication or authorization key, decide whether to remove it according to the force parameter. If force is true and removal is supported, an error is returned.

```typescript
public addExistingAuthenticationKey(
    id: DIDURL | string
): Builder；
```

The method is to add the public key that already exists but meets the authentication key as the authentication key. If it doesn't exist or its type does not meet the requirements, errors are returned.

```typescript
public addAuthenticationKey(
    id: DIDURL | string,
    pk: string
): Builder；
```

This method is to add the new authentication key. If it already exists, an error is returned.

Pk is the base58 string of the public key.

```typescript
public removeAuthenticationKey(
    id: DIDURL | string
): Builder；
```

This method removes the authentication key that already exists. If no authentication key exists, an error is returned.

```typescript
public addExistingAuthorizationKey(
    id: DIDURL | string
): Builder；
```

This method adds the public key that already exists as the authorization key. The following conditions should be met:

First, the public key already exists. Second, it's neither an authentication key nor an authorization key. Third, the controller is not a DID document subject. If these conditions are not met, an error is returned.

```typescript
public addAuthorizationKey(
	id: DIDURL | string,
	controller: DID | string,
	pk: string
): Builder；
```

This method adds a new authorization key. If it already exists, an error is returned.

Pk is the base58 string of the public key; the controller refers to the owner of the key.

```typescript
public async authorizeDid(
	id: DIDURL,
	controller: DID,
	key: DIDURL
): Promise<Builder>；
```

This method is to add the specified key to be an Authorization key - this specified key is the key of specified controller. Authentication is the mechanism by which the controller(s) of a DID can cryptographically prove that they are associated with that DID. A DID Document must include an authentication key.

```typescript
public removeAuthorizationKey(
	inputId: DIDURL | string
): Builder；
```

This method removes the specified authorization key. If this key does not exist or it is not an authorization key, an error is returned.

```typescript
public addCredential(
	vc: VerifiableCredential
): Builder；
```

This method adds the verifiable credential provided by users. If the ID of this credential already exists in the DID document, an error is returned.

```typescript
public async createAndAddCredential(
	storepass: string,
	id: DIDURL | string,
	subject: JSONObject | string = null,
	types: string[] = null,
	expirationDate: Date = null
): Promise<Builder>；
```

This method is used to directly add the self-proclaimed credential, without having to creating the credential and then adding it to the DID document.

Subject refers to the theme of the credential, that is, the primary content of the credential, which is provided in the jason format.

Types refer to the types of credentials. The SDK temporarily supports the following five types: SelfProclaimedCredential, EmailCredential, ProfileCredential, SocialCredential, and WalletCredential, which will be properly adjusted as needed. Users can choose the type according to their needs, or add customized types using corresponding methods.

expirationDate is the validity period, which cannot be greater than that of the document of the DID.

```typescript
public removeCredential(
	id: DIDURL | string
): Builder；
```

This method removes the specified credential. If it does not exist, an error is returned.

```typescript
public addService(
	id: DIDURL | string,
	type: string,
	endpoint: string,
	properties?: JSONObject
): Builder；
```

This method adds the service. If the specified ID already exists, an error is returned.

Endpoint is the address of the service point; properties is the content customized by users.

```typescript
public removeService(
	id: DIDURL | string
):  Builder；
```

This method removes specified service. If no specified service exists, an error is returned.

```typescript
public setExpires(
	expires: Date
): Builder；
```

The validity period of DID Document is five years after its creation by default. If the validity period is customized by users, it's set using this method cannot exceed five years, otherwise an error will be returned.
