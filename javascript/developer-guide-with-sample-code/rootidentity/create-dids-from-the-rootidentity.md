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

值得注意的是：该功能不需要提供DID store，DID store是由RootIdentity提供（生成RootIdentity是记录下了其相关联的DID store。）

overwrite表示是否需要覆盖已经存在的DID。overwrite为true时，覆盖已有DID，返回新生成DID object；overwrite为false时，若已存在DID，抛异常，返回NULL。

```typescript
public getDid(
    index: number
): DID；
```

getDid用于获取DID对象，过程中不生成也不保存DID Document。主要用于用户需要获取DID对象去执行其他的DID操作。
