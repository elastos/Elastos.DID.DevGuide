# DIDBackend

The whole DID system needs to interact with the chain locally, so it's necessary to initialize the DID Backend and set the rpc server of the chain, the contract address, the data buffer on the chain, as well as the interactive interface with the chain for DID at the initial stage of the DID system.

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

SDK provides two methods to initialize DID Backend.

_createtransaction_ is a user-provided uplink callback function, which is used for Publish DID or Declare Credential.

The URL support chain address can also be “mainnet” or “testnet”, and the SDK will set the resolve address according to the corresponding parameters.

_resolve_ is a user-provided resolve callback function.

_cachedir_ is the cache path where resolve is saved.
