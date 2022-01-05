# Issue a credential

Issuer通过owner提供的subject数据生成并颁发凭证。根据Issuer和owner的关系，凭证可以分为自声明凭证和第三方凭证，前者的Issuer就是owner，就是自己给自己颁发凭证，主要封装有个人提供的信息；后者的Issuer是除owner以外的其他DID，就是他人（如可信机构）根据owner提供给的数据封装的凭证。

The issuer generates and issues credentials based on subject data provided by the owner. According to the relationship between the issuer and the owner, credentials can be divided into self-proclaimed and third-party credentials. The issuer of a self-proclaimed credential is its owner, that is, individuals issue credentials encapsulated with individual information for themselves; third-party credential is issued by a DID other than the owner, that is, the credential is encapsulated by a third party, such as a trusted institution, according to the data provided by the owner.

## Example

自声明凭证示例：

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

第三方凭证示例：

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

通过构造函数生成Issuer，其中doc作为Issuer的DID Document，signKey为该DID用来签名凭证的Authentication Key，若未指定，则默认使用Default Key。

The constructor generates the issuer, with doc as the DID document of the issuer and signKey as the authentication key used by the DID to sign the credential. If not specified, the default key is used by default.

```typescript
public issueFor(did: DID | string): VerifiableCredential.Builder;
```

Issuer提供方法来指定为谁颁发凭证，得到VerifiableCredential.Builder object后根据VerifiableCredential.Builder提供的各种方法来设置更多凭证的属性。

The issuer specifies for whom the credential should be issued. After the VerifiableCredential.Builder object is obtained, more attributes of the credential are set according to various methods provided by VerifiableCredential.Builder.

did为需要颁发的凭证的owner。

DID is the owner of the credential to be issued.

```typescript
public async seal(storepass: string): Promise<VerifiableCredential>;
```

VerifiableCredential.Builder提供方法用来封装凭证内容，storepass是Issuer所在DID Store的storepass。

VerifiableCredential.Builder provides a method to encapsulate the content of the credential, and storepass refers to the storepass in the DID store of the issuer.
