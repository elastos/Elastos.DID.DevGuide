# Create RootIdentity

RootIdentity是DID体系的基础，提供三种生成方法：一、根据mnemonic和passphrase生成RootIdentity object；二、根据扩展根私钥生成RootIdentity object；三、根据衍生根公钥生成RootIdentity object。

## Example

```typescript
let rootPath = "root/store";
let store = await DIDStore.open(rootPath);
... ... ... ...
let mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
let rootKey = "xprv9s21ZrQH143K4biiQbUq8369meTb1R8KnstYFAKtfwk3vF8uvFd1EC2s49bMQsbdbmdJxUWRkuC48CXPutFfynYFVGnoeq8LJZhfd9QjvUt";
let derivedPubKey = "xpub6BmohzsffkuPQHqRNqksqvnef6c3wKarsRAmBjRHZgkLrT91xzH3HnkkJv48oursb6CxdzwuDecozwCXF5t9ropBqpPVz4hw2foivZxsmVs";
let passphrase = "helloworld";

//the first method
let identity = RootIdentity.createFromMnemonic(mnemonic, passphrase, store, "pwd");
if (identity) {
    let id1 = identity.getId();
    identity = RootIdentity.createFromPrivateKey(rootKey, store, "pwd");
    if (identity) {
        let id2 = identity.getId();
        identity = createFromPreDerivedPublicKey(derivedPubKey, 0);
        if (identity) {
            let id3 = identity.getId();
            if (id1 === id2 && id2 === id3) {
                console.log("three ids are same.");
            else
                console.log("three ids are same.");
        }
    }
}
... ... ... ...
store.close();
```

## Usage

```typescript
public static createFromMnemonic(
    mnemonic: string,
    passphrase: string,
    store: DIDStore,
    storepass: string,
    overwrite = false
): RootIdentity;
```

```typescript
public static createFromPrivateKey(
    extentedPrivateKey: string,
    store: DIDStore,
    storepass: string,
    overwrite = false
): RootIdentity;
```

```typescript
public static createFromPreDerivedPublicKey(
    preDerivedPublicKey: string,
    index: number
): RootIdentity;
```

mnemonic目前支持九种语言的助记词，分别是简体中文，繁体中文，英文，法文，意大利文，韩文，日文，西班牙文，捷克文。

extendedPrivateKey是82位扩展根私钥的base58字符串，这样便于处理也不需要暴露裸私钥。

preDerivedPublicKey是82为衍生根公钥的base58字符串。

overwrite表示是否需要覆盖本地已经存在的RootIdentity。overwrite为true时，覆盖已有RootIdentity，返回新生成RootIdentity object；overwrite为false时，若已存在RootIdentity，抛异常，返回NULL。
