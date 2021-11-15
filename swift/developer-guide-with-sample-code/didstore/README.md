# DIDStore

DIDStore 提供了一个本地安全的 DID 对象存储容器，用于应用在本地保存应用或者用户相关的 DID 对象，目前 DIDStore 支持以下对象的保存：

- RootIdentity
- DIDDocument
- VerifiableCredential
- PrivateKey

类似于 PKI 的 KeyStore，DIDStore 中的 PrivateKey 是加密保存的，同时写入后不能直接读取 PrivateKey，仅能通过 DIDDocument 相关的签名或者加密方法使用保存在 DIDStore 中的 PrivateKey；DIDStore 为其他的对象都提供了标准的读写界面。

除了标准的读写界面外，DIDStore 还提供了特权的导入导出方法，可以实现 DID 或者这个 store 的数据导出和导入，用于进行安全的数据备份或者迁移。导出的数据中将包含导出对象相关的 PrivateKey，为了保证安全，在导出数据中 PrivateKey 同样是采用加密处理的。

DIDStore 的默认实现是基于文件系统目录进行存储的，不建议开发者或者应用非 DIDStore 的方法修改 DIDStore 存储目录的数据，可能会导致 DIDStore 的异常或者数据的丢失。
