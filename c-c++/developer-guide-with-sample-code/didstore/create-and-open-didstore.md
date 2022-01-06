# Create and open DIDStore

DID文档，凭证，私钥等内容都需要保存或者读取，DID Store是整个DID体系里使用较为频繁的实例。相关操作前需要打开DIDStore，获取文件存储句柄，从而才能完成DID内容的保存或者读取。当然，用完DID Store需将其关闭。

DID documents, credentials, private keys, and other contents need to be saved or read. DID Store is a frequently used example in the whole DID system. Before operation, you need to open the DID Store and get the file storage handle, so that the DID content can be saved or read. Certainly, the DID Store needs to be closed after it is used.

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

`root`是需要打开的文件夹路径，若该文件夹路径不存在，SDK会自动创建。

_root_ is the folder path that needs to be opened. If the folder path does not exist, SDK will create one automatically.

```c
void DIDStore_Close(DIDStore *store);
```

SDK会清除掉缓存等内容。

SDK will clear the cache and other contents.
