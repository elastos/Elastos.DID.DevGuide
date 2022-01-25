# Create and Open DIDStore

DID documents, credentials, private keys, and other contents need to be saved or read. DID Store is a frequently used example in the whole DID system. Before operation, you need to open the DID Store and get the file storage handle, so that the DID content can be saved or read. The DID Store needs to be closed after it's used.

## Example

```c
const char *rootPath = "root/store";
DIDStore *store = DIDStore_Open(rootPath);
... ... ... ... ... ... ...
DIDStore_Close(store);
```

## Usage

```c
DIDStore* DIDStore_Open(const char *root);
```

_root_ is the folder path that needs to be opened. If the folder path does not exist, the SDK will create one automatically.

```c
void DIDStore_Close(DIDStore *store);
```

The SDK will clear the cache and other contents.
