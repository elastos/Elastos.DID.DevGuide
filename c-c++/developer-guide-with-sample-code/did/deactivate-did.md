# Deactivate DID

Publish DID方法和Transfer DID方法都是有效上链，Deactivate DID即为失效上链，停用DID。

DID可以由自己或者委托者来停用DID，普通DID有Authentication Key和Authorization Key完成停用，自定义DID有Authentication Key和Controller的Default Key完成停用。

未上链过的DID不可执行Deactivate操作。

## Usage

```c
int DIDDocument_DeactivateDID(DIDDocument *document, DIDURL *signkey,
        const char *storepass);
```

该方法是由被停用DID自己发起，用自己的Authentication Key完成停用。

`signKey`是被指定签名Deactivate交易的Authentication Key，`signKey`为null，则默认使用Default Key。

其他参数同Publish DID方法。

```c
int DIDDocument_DeactivateDIDByAuthorizor(DIDDocument *document, DID *target,
        DIDURL *signkey, const char *storepass);
```

该方法是由委托者发起的停用操作。

`target`就是等待被停用的DID。

`signkey`：普通DID的停用就由Autherizor DID Document发起，提供Autherizor Authentication Key，该Key也必须在Target的DID Document作为Authorization Key存在；自定义DID就是用Controller DID Document发起，提供Controller Default Key作为Sign Key。

其他参数同Publish DID。

