# Create DIDs from the RootIdentity

## Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ...
let mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
let passphrase = "helloworld";
let identity = RootIdentity.createFromMnemonic(mnemonic, passphrase, store, "pwd");
let doc = await identity.newDid("pwd");
let did = identity.getDid(0);
if (doc && did) {
    console.log("new did {} successfully.", doc.getSubject());
    loadDoc = store.loadDid(doc.getSubject());
    if (loadDoc)
        console.log("new did {} is in the DID store.", doc.getSubject());
    else
        console.log("new did {} isn't in the DID store.", doc.getSubject());
    if (did.equals(doc.getSubject()))
        console.log("two dids are same.");
    else
        console.log("two dids aren't same");
} else if (doc == null) {
    console.log("new did failed.");
} else {
    console.log("get did failed.");
}
... ... ... ...
store.close();
```

## Usage

```typescript
public async newDid(
    storepass: string,
    index: number = undefined,
    overwrite = false
): Promise<DIDDocument>;
```

RootIdentity生成新的DID，并且将其DID Document保存在DID store，并返回DID Document。

RootIdentity generates a new DID, saves its DID document in the DID store, and returns DID document.&#x20;

值得注意的是：该功能不需要提供DID store，DID store是由RootIdentity提供（生成RootIdentity是记录下了其相关联的DID store。）

It should be noted that this function does not need to provide a DID store, which is provided by RootIdentity (generating RootIdentity is equivalent to recording its associated DID store).

overwrite表示是否需要覆盖已经存在的DID。overwrite为true时，覆盖已有DID，返回新生成DID object；overwrite为false时，若已存在DID，抛异常，返回NULL。

Overwrite indicates whether the existing DID needs to be overwritten. When overwrite is true, overwrite the existing DID and return the newly generated DID object; when overwrite is false, if DID already exists, throw an exception and return NULL.

```typescript
public getDid(
    index: number
): DID；
```

getDid用于获取DID对象，过程中不生成也不保存DID Document。主要用于用户需要获取DID对象去执行其他的DID操作。

GetDid is used to get DID objects, which will neither generate nor save DID document in the process. It is mainly used for users to obtain DID objects to perform other DID operations.
