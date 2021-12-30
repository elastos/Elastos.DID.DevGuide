# DID Store

The DID Store provides a locally secure DID object storage container for applications to save application or DID objects, which are user-related locally. Currently, the following objects are supported by DID Store:

* RootIdentity
* DIDDocument
* VerifiableCredential
* PrivateKey

Similar to the Key Store of PKI, the Private Key in DID Store is encrypted and saved. At the same time, the Private Key can’t be read directly after writing, and the one saved in the DID Store can only be used through the signature or encryption method related to the DID Document. The DID Store provides a standard reading and writing interface for other objects.

In addition to the standard reading and writing interface, the DID Store also provides privileged import and export methods, which can realize the data export and import of the DID or this store for safe data backup or migration. The exported data will contain the Private Key related to the exported object. To ensure security, the Private Key in the exported data is also encrypted.

The default implementation of the DID Store is based on the file system directory. It is not recommended for developers or methods beyond the DID Store to modify the data within storage directory, which may lead to the abnormity of the DID Store or the loss of data.
