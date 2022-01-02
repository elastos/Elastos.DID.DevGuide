# Create DIDs from the Root Identity

In the Elastos DID SDK, all primitive DIDs are created from Root Identity. Each created DID corresponds to an integer index value, and the index value space is 0 \~ 2,147,483,647. There is a unique correspondence between each index value and the DID it created, that is, the DID created by the index is always consistent.

When creating a DID from Root Identity, DID document and Private Key corresponding to the newly created DID will be saved to the DID Store where Root Identity is located by default. The newly created DID will not be published automatically, but only a local DID object. If it needs to be published, it needs to be published explicitly - see [Publishing DID](../did/publish-did.md).



## Create DID with Default Index

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
RootIdentity identity; // a RootIdentity instance

DIDDocument doc = identity.newDid(storePasswd);
```

When you create DID with the default index, you don’t need to specify an index value. The next available index of the current record will be used by Root Identity and then updated automatically - this method is recommended during the creation of a new DID.

## Specify Index to Create DID

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
RootIdentity identity; // a RootIdentity instance
int index = 8; // the specified index

DIDDocument doc = identity.newDid(index, storePasswd);
```

When the DID is created by specified index, its value isn't managed by Root Identity, so developers or applications are needed to manage the available indexes and avoid conflicts caused by repeated use of indexes.

## Recreate DID Objects and their Documents

If you need to recreate the developed DIDs, this process will fail by default because the DIDs already exist. In this case, you can use the method with the overwrite parameter to create one. If overwrite is true, the DID SDK will ignore the already existing DID object, recreate the corresponding DID document object and regenerate its corresponding Private Key.

```java
DIDStore store; // an opened DIDStore instance
String storePasswd = "secret";
RootIdentity identity; // a RootIdentity instance
int index = 8; // the specified index
boolean overwrite = true;

DIDDocument doc = identity.newDid(index, overwrite, storePasswd);
```
