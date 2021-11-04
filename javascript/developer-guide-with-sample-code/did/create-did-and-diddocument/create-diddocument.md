# Create DIDDocument

DID Document 分为普通DID Document和自定义DID Document，这里讲的内容都是针对普通DID Document。自定义DID Document将在后续章节详细介绍。

通过newDid来生成DID Document，该DID Document是只含有default key的最基础DID Document。用户可以用过DID Document Builder来修改DID Document 内容，完成修改，通过seal方法终止修改，得到新DID Document。

DID Document 有public key，authentication key，authorization key，verifiable credential和service五种元素，通过相应的添加或者移除方法来修改文档。具体的方法请参考API doc。

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

newFromDocument拷贝了原始DID Document，防止后续修改影响原始DID Document，edit检查对DID Document主体签名的doc是否符合要求等。

controller参数是针对自定义 DID Document修改，在后续章节里会详细说明。对于普通DID Document不提供参数即可。

```typescript
public async seal(
    storepass: string
): Promise<DIDDocument>;
```

seal方法封装且得到新的DID Document。其中storepass为DID Store密码，用于对修改后DID Document主体内容通过主key私钥进行签名。

```typescript
public createAndAddPublicKey(
    id: DIDURL | string,
    pk: string,
    controller?: DID | string,
    type = "ECDSAsecp256r1"
): Builder;
```

该方法添加已经存在的public key（id或者pk相同）则报错。

```typescript
public removePublicKey(
    id: DIDURL | string,
    force = false
): Builder；
```

该方法需要注意：default key无法被移除；当希望被移除的是authentication key或者authorization key时，根据force参数决定是否移除，若force为true，支持移除，则报错。

```typescript
public addExistingAuthenticationKey(
    id: DIDURL | string
): Builder；
```

该方法为添加已经存在但符合authentication key的public key为authentication key。若不存在或者类型不符合要求，均报错。

```typescript
public addAuthenticationKey(
    id: DIDURL | string,
    pk: string
): Builder；
```

该方法为添加新authentication key，若已经存在，则报错。

pk为公钥的base58字符串。

```typescript
public removeAuthenticationKey(
    id: DIDURL | string
): Builder；
```

该方法移除已经存在的authentication key，若不存在，则报错。

```typescript
public addExistingAuthorizationKey(
    id: DIDURL | string
): Builder；
```

该方法添加已经存public key为authorization key。符合如下条件：一、已存在；二、非authentication key 和authorization key；三、controller非DID Document subject。不符合要求则报错。
