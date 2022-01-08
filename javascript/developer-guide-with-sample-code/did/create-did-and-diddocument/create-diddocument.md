# Create DIDDocument

DID Document 分为普通DID Document和自定义DID Document，这里讲的内容都是针对普通DID Document。自定义DID Document将在后续章节详细介绍。

DID documents are divided into ordinary and customized DID documents, and everything explained here is applicable to ordinary ones. Customized DID document will be described in detail in the following chapters.

通过newDid来生成DID Document，该DID Document是只含有default key的最基础DID Document。用户可以用过DID Document Builder来修改DID Document 内容，完成修改，通过seal方法终止修改，得到新DID Document。

Generate DID document by newDid. This DID document is the most basic DID document containing only the default key. Users can modify the content of DID document via DID document Builder, and terminate the modification by the seal method to get a new DID document.

DID Document 有public key，authentication key，authorization key，verifiable credential和service五种元素，通过相应的添加或者移除方法来修改文档。具体的方法请参考API doc。

The DID document contains five elements, namely public key, authentication key, authorization key, verifiable credential and service. The document can be correspondingly modified by the adding or removing methods. Please refer to API doc for specific methods.

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

newDid方法在RootIdentity章节已经介绍过了，这里不在多加说明了。

The newDid method has been introduced in the chapter of RootIdentity, so it is unnecessary to go into details here.

```typescript
public static newFromDocument(
    doc: DIDDocument,
    controller?: DIDDocument
): Builder;

public edit(
    controller?: DIDDocument
): Builder;
```

newFromDocument和edit方法搭配使用，如example里面一样。

newFromDocument is used together with the edit method, as shown in the example.

newFromDocument拷贝了原始DID Document，防止后续修改影响原始DID Document，edit检查对DID Document主体签名的doc是否符合要求等。

NewFromDocument copies the original DID document to prevent subsequent modification from affecting the original DID document, and edit checks whether the doc signed by DID document subject meets the requirements.

controller参数是针对自定义 DID Document修改，在后续章节里会详细说明。对于普通DID Document不提供参数即可。

Controller parameter is modified for the customized DID document, which will be explained in detail in the following chapters. For ordinary DID document, there is no need to provide parameters.

```typescript
public async seal(
    storepass: string
): Promise<DIDDocument>;
```

seal方法封装且得到新的DID Document。其中storepass为DID Store密码，用于对修改后DID Document主体内容通过主key私钥进行签名。

The seal method encapsulates and gets a new DID document. Storepass is the password of the DID Store, which is used to sign the main content of the modified DID document via the private key of the master key.

```typescript
public createAndAddPublicKey(
    id: DIDURL | string,
    pk: string,
    controller?: DID | string,
    type = "ECDSAsecp256r1"
): Builder;
```

该方法添加已经存在的public key（id或者pk相同）则报错。

If this method adds the public key that already exists (with id or pk being the same), an error is returned.

```typescript
public removePublicKey(
    id: DIDURL | string,
    force = false
): Builder；
```

该方法需要注意：default key无法被移除；当希望被移除的是authentication key或者authorization key时，根据force参数决定是否移除，若force为true，支持移除，则报错。

It should be noted that the default key cannot be removed; when you want to remove the authentication key or authorization key, decide whether to remove it according to the force parameter. If force is true and removal is supported, an error is returned.

```typescript
public addExistingAuthenticationKey(
    id: DIDURL | string
): Builder；
```

该方法为添加已经存在但符合authentication key的public key为authentication key。若不存在或者类型不符合要求，均报错。

The method is to add the public key that already exists but meets the authentication key as the authentication key. If it does not exist or its type does not meet the requirements, errors are returned.

```typescript
public addAuthenticationKey(
    id: DIDURL | string,
    pk: string
): Builder；
```

该方法为添加新authentication key，若已经存在，则报错。

This method is to add the new authentication key. If it already exists, an error is returned.

pk为公钥的base58字符串。

Pk is the base58 string of the public key.

```typescript
public removeAuthenticationKey(
    id: DIDURL | string
): Builder；
```

该方法移除已经存在的authentication key，若不存在，则报错。

This method removes the authentication key that already exists. If no authentication key exists, an error is returned.

```typescript
public addExistingAuthorizationKey(
    id: DIDURL | string
): Builder；
```

该方法添加已经存public key为authorization key。符合如下条件：一、已存在；二、非authentication key 和authorization key；三、controller非DID Document subject。不符合要求则报错。

This method adds the public key that already exists as the authorization key. The following conditions should be met: First, the public key already exists; second, it is neither an authentication key nor an authorization key; third, the controller is not a DID document subject. If the above conditions are not met, an error is returned.

```typescript
public addAuthorizationKey(
	id: DIDURL | string,
	controller: DID | string,
	pk: string
): Builder；
```

该方法为添加新authorization key，若已经存在，则报错。

This method adds a new authorization key. If it already exists, an error is returned.

pk为公钥的base58字符串；controller为该key的持有者。

Pk is the base58 string of the public key; the controller refers to the owner of the key.

```typescript
public async authorizeDid(
	id: DIDURL,
	controller: DID,
	key: DIDURL
): Promise<Builder>；
```

This method is to add the specified key to be an Authorization key. This specified key is the key of specified controller. Authentication is the mechanism by which the controller(s) of a DID can cryptographically prove that they are associated with that DID. A DID Document must include authentication key.

```typescript
public removeAuthorizationKey(
	inputId: DIDURL | string
): Builder；
```

该方法移除指定的authorization key。若该key不存在或者非authorization key，则报错。

This method removes the specified authorization key. If this key does not exist or it is not an authorization key, an error is returned.

```typescript
public addCredential(
	vc: VerifiableCredential
): Builder；
```

该方法用于添加用户提供的Verifiable Credential。若该Credential的Id在DID Document 中已经存在，则报错。

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

该方法用于直接添加自声明凭证，无需先创建credential在添加到DID Document中。

This method is used to directly add the self-proclaimed credential, without having to creating the credential and then adding it to the DID document.

subject为凭证的主题，是凭证最主要的内容，以json形式提供。

Subject refers to the theme of the credential, that is, the primary content of the credential, which is provided in the jason format.

types是Credential的类型，SDK暂支持五种type：SelfProclaimedCredential，EmailCredential，ProfileCredential，SocialCredential和WalletCredential，后续根据需求做适当调整。用户可以根据需要选择type，也可以通过相应方法添加自定义的type。

Types refer to the types of credentials. SDK temporarily supports the following five types: SelfProclaimedCredential, EmailCredential, ProfileCredential, SocialCredential and WalletCredential, which will be properly adjusted as needed. Users can choose the type according to their needs, or add customized types using corresponding methods.

expirationDate是有效期，不可以大于所属DID的Document有效期。

expirationDate is the validity period, which cannot be greater than that of the document of the DID.

```typescript
public removeCredential(
	id: DIDURL | string
): Builder；
```

该方法移除指定的Credential。若不存在则报错。

This method removes the specified credential. If it does not exist, an error is returned.

```typescript
public addService(
	id: DIDURL | string,
	type: string,
	endpoint: string,
	properties?: JSONObject
): Builder；
```

该方法为添加Service。若指定id已经存在，则报错。

This method adds the service. If the specified ID already exists, an error is returned.

endpoint为Service服务点地址；properties是用户可自定义添加的内容。

Endpoint is the address of the service point; properties is the content customized by users.

```typescript
public removeService(
	id: DIDURL | string
):  Builder；
```

该方法移除指定的Service，若不存在则报错。

This method removes specified service. If no specified service exists, an error is returned.

```typescript
public setExpires(
	expires: Date
): Builder；
```

DID Document的有效期默认为创建之时起后推五年。如果用户也可以通过该方法自行定义有效期。该方法设置的有效期不可超过五年，否则报错。

The validity period of DID document is five years after its creation by default. If the validity period is customized by users, the validity period set using this method cannot exceed five years, otherwise an error will be returned.
