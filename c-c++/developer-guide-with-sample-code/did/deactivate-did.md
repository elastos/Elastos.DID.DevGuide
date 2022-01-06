# Deactivate DID

Publish DID方法和Transfer DID方法都是有效上链，Deactivate DID即为失效上链，停用DID。

The Publish DID method and the Transfer DID method are both effective cochain, and deactivated DID is the invalid cochain, so the DID is deactivated.

DID可以由自己或者委托者来停用DID，普通DID有Authentication Key和Authorization Key完成停用，自定义DID有Authentication Key和Controller的Default Key完成停用。

The DID can be deactivated by itself or the principal. Primitive DID can be deactivated by Authentication Key and Authorization Key. Customized DID can be deactivated by Authentication Key and Controller’s Default Key.

未上链过的DID不可执行Deactivate操作。

The deactivation cannot be performed by a DID which is not on the chain.

## Usage

```c
int DIDDocument_DeactivateDID(
        DIDDocument *document,
        DIDURL *signkey,
        const char *storepass);
```

该方法是由被停用DID自己发起，用自己的Authentication Key完成停用。

The method is initiated by the deactivated DID itself, and the deactivating is completed with its own Authentication Key.

`signKey`是被指定签名Deactivate交易的Authentication Key，`signKey`为null，则默认使用Default Key。

_signKey_ is the Authentication Key of the designated signature deactivation transaction. If signKey is null, the Default Key will be used by default.

其他参数同Publish DID方法。

Other parameters are the same as the Publish DID method.

```c
int DIDDocument_DeactivateDIDByAuthorizor(
        DIDDocument *document,
        DID *target,
        DIDURL *signkey,
        const char *storepass);
```

该方法是由委托者发起的停用操作。

This method is a deactivating operation initiated by the principal.

`target`就是等待被停用的DID。

_target_ is the DID waiting to be deactivated.

`signkey`：普通DID的停用就由Autherizor DID Document发起，提供Autherizor Authentication Key，该Key也必须在Target的DID Document作为Authorization Key存在；自定义DID就是用Controller DID Document发起，提供Controller Default Key作为Sign Key。

_signkey_: the deactivation of primitive DID is initiated by Autherizor DID Document, which provides Autherizor Authentication Key, which must also exist as Authorization Key in DID Document of Target; Customized DID is initiated by Controller DID Document, and Controller Default Key is provided as Sign Key.

其他参数同Publish DID。

Other parameters are the same as Publish DID.
