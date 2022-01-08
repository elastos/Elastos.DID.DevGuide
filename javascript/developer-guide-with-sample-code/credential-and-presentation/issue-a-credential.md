# Issue a Credential

The issuer generates and issues credentials based on subject data provided by the owner. According to the relationship between the issuer and the owner, credentials can be divided into self-proclaimed and third-party credentials. The issuer of a self-proclaimed credential is its owner, that is, individuals issue credentials encapsulated with individual information for themselves; a third-party credential is issued by a DID other than the owner - that is, the credential is encapsulated by a third party, such as a trusted institution, according to the data provided by the owner.

## Example

An example of a self-proclaimed credential:

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
let storePass = "pwd";
... ... ... ...
let mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
let passphrase = "helloworld";
let identity = RootIdentity.createFromMnemonic(mnemonic, passphrase, store, "pwd");
let doc = await identity.newDid("pwd");

let props = {
	name: "Testing Issuer",
	nation: "Singapore",
	language: "English",
	email: "issuer@example.com"
};
//create Issuer
let issuer = new Issuer(doc);
let cb = issuer.issueFor(doc.getSubject());
//create self-claimed credential
let vc = await cb.id("#myCredential")
	.type("BasicProfileCredential",
	"SelfProclaimedCredential")
	.properties(props)
	.seal(storePass);
... ... ... ...
store.close();
```

An example of a third-party credential:

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
let storePass = "pwd";
... ... ... ...
let mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
let passphrase = "helloworld";
let identity = RootIdentity.createFromMnemonic(mnemonic, passphrase, store, "pwd");
let doc = await identity.newDid("pwd");
let issuerDoc = await identity.newDid("pwd");

let props = {
	name: "John",
	gender: "Male",
	nation: "Singapore",
	language: "English",
	email: "john@example.com",
	twitter: "@john"
};

//create Issuer
let issuer = new Issuer(issuerDoc);
let cb = issuer.issueFor(doc.getSubject());
//create kyc credential
let vc =  await cb.id("#testCredential")
	.type("BasicProfileCredential", "InternetAccountCredential")
	.properties(props)
	.seal(storePass);
	
... ... ... ...
store.close();
```

## Usage

```typescript
export class Issuer {
	... ... ... ...
	constructor(doc: DIDDocument, signKey?: DIDURL);
	... ... ... ...
}
```

The constructor generates the issuer, with doc as the DID document of the issuer and signKey as the authentication key used by the DID to sign the credential. If not specified, the default key is used by default.

```typescript
public issueFor(did: DID | string): VerifiableCredential.Builder;
```

The issuer specifies for whom the credential should be issued. After the VerifiableCredential.Builder object is obtained, more attributes of the credential are set according to various methods provided by VerifiableCredential.Builder.

DID is the owner of the credential to be issued.

```typescript
public async seal(storepass: string): Promise<VerifiableCredential>;
```

VerifiableCredential.Builder provides a method to encapsulate the content of the credential, and storepass refers to the storepass in the DID store of the issuer.
