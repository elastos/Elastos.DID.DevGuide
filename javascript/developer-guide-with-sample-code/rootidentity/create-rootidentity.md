# Create RootIdentity

As the foundation of the DID system, RootIdentity provides three generation methods:

First, generate the RootIdentity object according to mnemonic and passphrase. Second, generate the RootIdentity object per the extended root private key. Third, generate the RootIdentity object according to that derived root public key.

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

Mnemonic currently supports mnemonics in nine languages, namely Simplified Chinese, Traditional Chinese, English, French, Italian, Korean, Japanese, Spanish, and Czech.

ExtendedPrivateKey is the base58 string of 82-bit extended root private key, which is convenient for processing without having to expose the private key.

PreDerivedPublicKey is a base58 string of 82-bit derived root public key.

Overwrite indicates whether it's necessary to overwrite the RootIdentity that is already on the local machine. When overwrite is true, then overwrite the existing RootIdentity and return the newly generated RootIdentity object; when overwrite is false, if RootIdentity already exists, throw an exception and return NULL.
