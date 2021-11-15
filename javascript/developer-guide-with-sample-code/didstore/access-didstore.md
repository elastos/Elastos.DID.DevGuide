# Access DIDStore

## 读取DID store中的对象

DID store提供了一系列的load API用于store内的对象的读取，可以支持读取store中的RootIdentity、DIDDocument、VerifiableCredential。

但是处于安全的考虑，存储在store中的私钥是不能被直接读取的，只能通过签名或者加密的API透明的使用。

* #### Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);

// Load RootIdentity
let rootidentity = await store.loadRootIdentity();
if (rootidentity) {
    ... ... ...
}

let did = new DID("did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y");
// Load DIDDocuemnt
let doc = await store.loadDid(did);
... ... ... ...

let id = DIDURL.from("#cred-1", did);
// Load VerifiableCredential
let vc = await store.loadCredential(id);
... ... ... ...

store.close();
```

* #### Usage

```typescript
public async loadRootIdentity(
    id: string = undefined
): Promise<RootIdentity>;
```

id是RootIdentity的标识字符串。如果该DIDStore就一个RootIdentity，那么在使用该api时可以不给id标识字符串，如上面的example。否则，获取失败。

获取结果是RootIdentity object。

```typescript
public async loadDid(
    didOrString: DID | string
): Promise<DIDDocument>;
```

参数可以是DID object也可以是DID字符串，其与在DIDStore保存中的文件名一致。如果该DID内容不存在或者DID Document保存格式有误，则获取失败。声明：有多种方式可以获取DID object，example里只是其中一种，用户根据自己需求来使用api。具体内容详见DID/Create DID and DIDDocument章节。

获取结果是DID Document object（也是DIDStore保存的DID内容）。

```typescript
public async loadCredential(
    id: DIDURL | string
): Promise<VerifiableCredential>；
```

id是credential的唯一标识符，因此需要通过id去获取。id参数可以是DIDURL object也可以是ID字符串。如果该凭证不存在或者格式有误，则获取失败。声明：有多种方式可以获取DIDURL object，example里只是其中一种，用户根据自己需求来使用api。具体内容详见DID/Create DID and DIDDocument章节。

获取结果是Credential object。

## 保存DID信息到DID store

DID store提供一系列api用于保存DID信息到DID store，主要有DID Document，Verifiable credential和Private key。

其中，保存私钥主要用于DIDDocument添加key时，密钥对中的公钥添加到Document，私钥加密保存到DID store，用于DID的授权和委托。

* #### Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ...
let did = new DID("did:elastos:iXcRhYB38gMt1phi5JXJMjeXL2TL8cg58y");
let doc = await did.resolve();
if (doc)
    // store DIDDocument
    await store.storeDid(doc);
... ... ... ...
let storePass = "pwd";
let id = DIDURL.from("#key-1", did);
let sk = HDKey.deserializeBase58("xprv9s21ZrQH143K4biiQbUq8369meTb1R8KnstYFAKtfwk3vF8uvFd1EC2s49bMQsbdbmdJxUWRkuC48CXPutFfynYFVGnoeq8LJZhfd9QjvUt").serialize();
//store PrivateKey
store.storePrivateKey(id, sk, storePass);
... ... ... ...
let issuer = new Issuer(doc);
let vc = issuer.issueFor(did)
                    .id("#test")
                    .type("SelfProclaimedCredential")
                    .properties({"name": "littlefish"})
                    .seal(storePass);
if (vc)
    //store Credential
    await store.storeCredential(vc);
... ... ... ...
store.close();
```

* #### Usage

```typescript
public async storeDid(
    doc: DIDDocument
): Promise<void>;
```

SDK实现将DID内容保存在DID store。

参数doc：DIDDocument有多种获取方式，example里只是其中一种。

```typescript
public async storeCredential(
    credential: VerifiableCredential
): Promise<void>;
```

SDK实现将Credential object保存在DID store。

```typescript
public storePrivateKey(
    idOrString: DIDURL | string,
    privateKey: Buffer,
    storepass: string
):void;
```

参数privateKey：82位扩展私钥

参数storepass：用于私钥加密。

## 列举DID store中的对象

DID store是后台存储，用户需要知道DID store中保存的所有DID信息就可以通过列举方式来获取。DID store提供RootIdentity，DID和Verifiable Credential的例举方法。

为了安全起见，私钥不可列举。

* #### Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ...

//list RootIdentities
let ids = await store.listRootIdentities();
if (ids.length > 0) 
    console.log("there are {} root identities in the DID store.", ids.length);
else
    console.log("there are no root identity in the DID store.");
... ... ... ...   

//list DIDs
let remains = await store.listDids();
if (remains.length > 0) {
    console.log("there are {} dids in the DID store.", remains.length);
    //list Credentials
    let vcs = await store.listCredentials(remains[0]);
    if (vcs.length > 0)
        console.log("did: {} has {} credentials.", remains[0], vcs.length);
    else
        console.log("did: {} has no credential.", remains[0]); 
} else {
    console.log("there are no did in the DID store");
}
... ... ... ...

store.close();
```

* #### Usage

```typescript
public async listRootIdentities(): Promise<RootIdentity[]>;
```

列举DID store中保存的RootIdentity，并将RootIdentity object放在数组中返回。

```typescript
public async listDids(): Promise<DID[]>;
```

列举DID store中保存的DID，并将DID放在数组中返回。如需获取更详细的DID内容，可以通过loadDid得到DID Document。

```typescript
public async listCredentials(didOrString: DID | string): Promise<DIDURL[]>;
```

列举指定DID的所有凭证，并将凭证ID放在数组中返回。如需获取更详细的credential object，可以通过loadCredential得到。

## 挑选DID store中的对象

List功能可以列举DID store中所有同类型对象，select功能可以列举符合指定条件的DID和Verifiable Credential对象。

* #### Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ...
//select DIDs
await store.selectDids(new class implements DIDFilter {
    public select(d: DID): boolean {
        return d.resolve() ? true : false;     
    }
});
... ... ... ... 
//select credentials
await store.selectCredentials(did, new class implements DIDFilter {
    public select(id: DIDURL): boolean {
        return true;     
    }
});
... ... ... ...
store.close();
```

* #### Usage

```typescript
export interface DIDFilter {
    select(did: DID): boolean;
}

public async selectDids(filter: DIDStore.DIDFilter): Promise<DID[]>;
```

用户实现DIDFilter以提供选择DID的方法。具体使用可见sample。

```typescript
export interface CredentialFilter {
    select(id: DIDURL): boolean;
}

public async selectCredentials(
    didOrString: DID | string,
    filter: DIDStore.CredentialFilter
): Promise<DIDURL[]>;
```

用户实现CredentialFilter以提供选择Credential的方法，返回符合要求的Credential Id标识。具体使用可见sample。

## 删除DID store中的对象

DID store提供方法保存DID对象，自然也提供删除保存的DID对象的方法，先课删除RootIdentity，DID，Verifiable Credential和Private key。

* #### Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);

... ... ... ...
let ids = await store.listRootIdentities();
if (ids.length > 0) {
    //delete RootIdentity
    if (store.deleteRootIdentity(ids[0].getId()))
        console.log("delete root identity {} sucessfully.", ids[0].getId());
    else
        console.log("delete root identity {} failed.", ids[0].getId());
}      
... ... ... ...

let dids = await store.listDids();
if (dids.length > 0) {
    console.log("there are {} dids in the DID store.", dids.length);
    let vcs = await store.listCredentials(dids[0]);
    if (vcs.length > 0) {
        console.log("did: {} has {} credentials.", dids[0], vcs.length);
        //list Verifiable Credential
        if (store.deleteCredential(vcs[0]))
            console.log("delete credential {} successfully.", vcs[0]);
        else
            console.log("delete credential {} failed.", vcs[0]);
    } else {
        console.log("did: {} has no credential.", dids[0]); 
    }
    
    //delete DID
    if (store.deleteDid(dids[0]))
        console.log("delete did {} successfully.", dids[0]);
    else
        console.log("delete did {} failed.", dids[0]);
} else {
    console.log("there are no did in the DID store");
}
... ... ... ...

store.close();
```

* #### Usage

```typescript
public deleteRootIdentity(id: string): boolean;
```

根据id字串标识，删除RootIdentity。若无该RootIdentity，或者该RootIdentity是DID store中唯一的对象，不可删除，则返回false。

```typescript
public deleteDid(didOrString: DID | string): boolean;
```

根据DID标识，删除保存在DIDStore内所有和didOrString相关的内容，如文档，凭证和私钥。若无该DID信息或者发生错误，则返回false。

```typescript
public deleteCredential(idOrString: DIDURL | string): boolean;
```

根据id标识，删除凭证。若无该DID信息或者发生错误，则返回false。

```typescript
public deletePrivateKey(idOrString: DIDURL | string): boolean;
```

根据id标识，删除对应私钥。若无该私钥信息或者发生错误，则返回false。

## 检查DID store中是否包含对象

有些时候用户只需要知道DID store中是否保存某对象，而不需要该对象实例。因此DID store提供了方法检查RootIdentity，Mnemonic，DID，Verifiable Credential和Private key的存在与否。

* #### Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);

//contains RootIdentities
if (store.containsRootIdentities()) {
    console.log("there are root identities in the DID store.");
    let ids = await store.listRootIdentities();
    let id = ids[0].getId();
    //contains RootIdentity
    if (store.containsRootIdentity(id)) {
        console.log("root identity {} is in the DID store.", id);
        //contains RootIdentity Mnemonic
        if (store.containsRootIdentityMnemonic(id)
             console.log("root identity {} has mnemonic.", id);
         else
             console.log("root identity {} doesn't have mnemonic.", id);
    } else {
        console.log("root identity {} isn't in the DID store.", id);
    }
} else {
    console.log("there are no root identity in the DID store.");
}

//contains DIDs
if (store.containsDids()) {
    console.log("there are DIDs in the DID store.");
    let dids = await store.listDIDs();
    let did = dids[0];
    //contains DID
    if (store.containsDid(id)) 
        console.log("DID {} is in the DID store.", did);
    else
        console.log("DID {} isn't in the DID store.", did);
} else {
    console.log("there are no DID in the DID store.");
}

//contains Credentials
if (store.containsCredentials(did)) {
    console.log("there are some credentials in the DID store.");
    let vcs = await store.listCredentials();
    let vc = vcs[0];
    //contains Credential
    if (store.containsCredential(vc))
        console.log("Credential {} is in the DID store.", vc);
    else
        console.log("Credential {} isn't in the DID store.", vc);
} else {
    console.log("there are no credential in the DID store.");
}
... ... ... ...

store.close();
```

* #### Usage

```typescript
public containsRootIdentity(id: string): boolean;
```

id是RootIdentity的标识字符串，DID store若没有该RootIdentity或者发生其他错误，返回false。

```typescript
public containsRootIdentities(): boolean;
```

检查DID store是否含有RootIdentity。若含有任何一个RootIdentity，返回true；否则，返回false。

```typescript
public containsRootIdentityMnemonic(id: string): boolean;
```

检查指定RootIdentity是否含有mnemonic。

```typescript
public containsDid(did: DID | string): boolean;
```

检查DID Store是否含有指定DID。

```typescript
public containsDids(): boolean;
```

检查DID Store是否含有DID。若含有任何一个DID，返回true；否则，返回false。

```typescript
public async containsCredential(id: DIDURL | string): Promise<boolean>;
```

检查DID store是否含有指定id的Credential。

```typescript
public containsCredentials(did : DID) : boolean;
```

检查DID store是否含有指定DID的Credential。若含有任何一个Credential，返回true；否则，返回false。

```typescript
public containsPrivateKey(id: DIDURL | string): boolean;
```

检查DID store是否含有指定id的Private key。

```typescript
public containsPrivateKeys(did: DID | string): boolean;
```

检查DID store是否含有指定did的私钥。若含有任何一个Private key，返回true；否则，返回false。
