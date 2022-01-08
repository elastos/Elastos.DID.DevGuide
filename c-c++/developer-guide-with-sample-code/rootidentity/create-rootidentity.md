# Create RootIdentity

RootIdentity是DID体系的基础，提供三种生成方法：一、根据mnemonic和passphrase生成RootIdentity object；二、根据扩展根私钥生成RootIdentity object；三、根据衍生根公钥生成RootIdentity object。

Root Identity is the foundation of DID system, which provides three generation methods: firstly, generate Root Identity object according to mnemonic and passphrase; Secondly, generate Root Identity object according to the extended root private key; Thirdly, generate a Root Identity object according to that derive root public key.

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

SDK提供两种方法生成RootIdentity，第一种是通过`mnemonic`和`passphrase`来获取；第二种是通过扩展根私钥来获取。

SDK provides two methods to generate Root Identity, the first is to obtain it through mnemonic and passphrase; The second is to get it by expanding the root private key.

`mnemonic`目前支持九种语言的助记词，分别是简体中文，繁体中文，英文，法文，意大利文，韩文，日文，西班牙文，捷克文。

_mnemonic_ currently supports mnemonics in nine languages, namely Simplified Chinese, Traditional Chinese, English, French, Italian, Korean, Japanese, Spanish and Czech.

`passphrase`可以不提供，也可以为空字符串。

_passphrase_ may not be provided, or it can be an empty string.

`extendedprvkey`是82位扩展根私钥的base58字符串，这样便于处理也不需要暴露裸私钥。

_extendedprvkey_ is the base58 string of the 82-bit extended root private key, which is convenient to handle and does not need to expose the naked private key.

`overwrite`表示是否需要覆盖本地已经存在的RootIdentity。`overwrite`为true时，覆盖已有RootIdentity，返回新生成RootIdentity object；`overwrite`为false时，若已存在RootIdentity，抛异常，返回NULL。

_overwrite_ indicates whether it is necessary to overwrite the existing Root Identity locally. When overwrite is true, the existing Root Identity needs to be overwritten and the newly generated Root Identity object is returned; When overwrite is false, if Root Identity already exists, an exception will be thrown, and NULL will be returned.

```c
void RootIdentity_Destroy(RootIdentity *rootidentity);
```

使用完RootIdentity object，该方法销毁RootIdentity。

After using the Root Identity object, it needs to be destroyed with this method.

```c
const char *RootIdentity_CreateId(
    const char *mnemonic,
    const char *passphrase);
```

```c
const char *RootIdentity_CreateIdFromRootKey(const char *extendedprvkey);
```

SDK提供两种方法用来获取RootIdentity标识ID，但是并不是生成且保存新RootIdentity。

SDK provides two methods to obtain the Root Identity ID, but it is not to generate and save a new Root Identity.

注意：用完返回值，需要释放。

Attention: When the return value is used up, it needs to be released.
