## DIDBackend

整个DID系统都需要本地与链进行交互，所以DID体系启动初期就需要先初始化DID Backend，设定好链的rpc服务端，合约地址，还有链上数据缓冲区等内容，为DID设定好与链交互接口。

## Example

```c
static bool createIdTransaction(const char *payload, const char *memo)
{
    if (!payload)
        return false;

    ... ... ... ...
    return true;
}

int rc = DIDBackend_InitializeDefault(DIDAdapter_CreateIdTransaction, “mainnet”, cachedir);
if (rc < 0)
	printf("initialize DID Backend failed.\n");
else
	printf("initialize DID Backend successfully.\n");
```

## Usage
```c
typedef bool CreateIdTransaction_Callback(
	const char *payload,
	const char *memo);
	
int DIDBackend_InitializeDefault(
	CreateIdTransaction_Callback *createtransaction,
    const char *url,
    const char *cachedir);
```
```c
typedef const char* Resolve_Callback(
	const char *request);
	
int DIDBackend_Initialize(
	CreateIdTransaction_Callback *createtransaction,
    Resolve_Callback *resolve,
    const char *cachedir);
```
SDK提供两种初始化DID Backend的方法。

`createtransaction`是用户提供的上链回调函数，用于Publish  DID或者Declare Credential。

`url`支持链的地址，也可以为"mainnet" 或者 "testnet"，SDK会根据相应参数设置resolve地址。

`resolve`是用户提供resolve回调函数。

`cachedir`为保存resolve下来的缓存路径。
