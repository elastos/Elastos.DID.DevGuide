# DIDStore

DIDStore 提供了一个本地安全的 DID 对象存储容器，用于应用在本地保存应用或者用户相关的 DID 对象，目前 DIDStore 支持以下对象的保存：

DID Store provides a locally secure DID object storage container for applications to save application or DID objects which are user-related locally. Currently, the following objects are supported by DID Store:

* RootIdentity
* DIDDocument
* VerifiableCredential
* PrivateKey

类似于 PKI 的 KeyStore，DIDStore 中的 PrivateKey 是加密保存的，同时写入后不能直接读取 PrivateKey，仅能通过 DIDDocument 相关的签名或者加密方法使用保存在 DIDStore 中的 PrivateKey；DIDStore 为其他的对象都提供了标准的读写界面。

Similar to the Key Store of PKI, the Private Key in DID Store is encrypted and saved. At the same time, the Private Key can’t be read directly after writing, and the one saved in DID Store can only be used through the signature or encryption method related to DID Document. DID Store provides a standard reading and writing interface for other objects.

除了标准的读写界面外，DIDStore 还提供了特权的导入导出方法，可以实现 DID 或者这个 store 的数据导出和导入，用于进行安全的数据备份或者迁移。导出的数据中将包含导出对象相关的 PrivateKey，为了保证安全，在导出数据中 PrivateKey 同样是采用加密处理的。

In addition to the standard reading and writing interface, DID Store also provides privileged import and export methods, which can realize the data export and import of DID or this store for safe data backup or migration. The exported data will contain the Private Key related to the exported object. To ensure security, the Private Key in the exported data is also encrypted.

DIDStore 的默认实现是基于文件系统目录进行存储的，不建议开发者或者应用非 DIDStore 的方法修改 DIDStore 存储目录的数据，可能会导致 DIDStore 的异常或者数据的丢失。

The default implementation of DID Store is based on the file system directory. It is not recommended for developers or methods beyond the DID Store to modify the data in the DID Store storage directory, which may lead to the abnormity of DID Store or the loss of data.
