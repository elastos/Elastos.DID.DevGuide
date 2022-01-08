# Create RootIdentity

Root Identity is the foundation of the DID system, which provides three generation methods. Firstly, generate Root Identity object according to mnemonic and passphrase. Secondly, generate Root Identity object according to the extended root private key/ Thirdly, generate a Root Identity object according to that derive root public key.

## Example

```c
const char *rootPath = "root/store";
DIDStore *store = await DIDStore.open(rootPath);
... ... ... ...
const char *mnemonic = "pact reject sick voyage foster fence warm luggage cabbage any subject carbon";
const char *rootKey = "xprv9s21ZrQH143K4biiQbUq8369meTb1R8KnstYFAKtfwk3vF8uvFd1EC2s49bMQsbdbmdJxUWRkuC48CXPutFfynYFVGnoeq8LJZhfd9QjvUt";
const char *derivedPubKey = "xpub6BmohzsffkuPQHqRNqksqvnef6c3wKarsRAmBjRHZgkLrT91xzH3HnkkJv48oursb6CxdzwuDecozwCXF5t9ropBqpPVz4hw2foivZxsmVs";
const char *passphrase = "helloworld";

//the first method
RootIdentity *identity = RootIdentity_Create(mnemonic, passphrase, true, store, "pwd");
if (!identity)
		//error operation
  
const char *id = RootIdentity_CreateId(mnemonic, passphrase);
if (!id)
		//error operation 
if (strcmp(id, RootIdentity_GetId(identity)))
		//error operation
free((void*)id);
RootIdentity_Destroy(identity);

RootIdentity *ri = RootIdentity_CreateFromRootKey(rootKey, true, store, "pwd");
if (!ri)
		//error operation
  
id = RootIdentity_CreateIdFromRootKey(rootKey);
if (!id)
		//error operation 
if (strcmp(id, RootIdentity_GetId(ri)))
		//error operation
free((void*)id);
RootIdentity_Destroy(ri);
... ... ... ...
DIDStore_Close(store);
```

## Usage

```c
RootIdentity *RootIdentity_Create(
        const char *mnemonic,
        const char *passphrase,
        bool overwrite,
        DIDStore *store,
        const char *storepass
);
```

```c
RootIdentity *RootIdentity_CreateFromRootKey(
        const char *extendedprvkey,
        bool overwrite,
        DIDStore *store,
        const char *storepass
);
```

The SDK provides two methods to generate Root Identity - the first is to obtain it through mnemonic and passphrase, and the second is to get it by expanding the root private key.

mnemonic currently supports mnemonics in nine languages: Simplified Chinese, Traditional Chinese, English, French, Italian, Korean, Japanese, Spanish, and Czech.

passphrase may not be provided, or it can be an empty string.

extendedprvkey is the base58 string of the 82-bit extended root private key, which is convenient to handle and does not need to expose the naked private key.

overwrite indicates whether it is necessary to overwrite the existing Root Identity locally. When overwrite is true, the existing Root Identity needs to be overwritten and the newly generated Root Identity object is returned. When overwrite is false, if Root Identity already exists, an exception will be thrown, and NULL will be returned.

```c
void RootIdentity_Destroy(RootIdentity *rootidentity);
```

After using the Root Identity object, it needs to be destroyed with this method.

```c
const char *RootIdentity_CreateId(
    const char *mnemonic,
    const char *passphrase);
```

```c
const char *RootIdentity_CreateIdFromRootKey(const char *extendedprvkey);
```

The SDK provides two methods to obtain the Root Identity ID, but it's not to generate and save a new Root Identity.

**Attention:** When the return value is used up, it needs to be released.
