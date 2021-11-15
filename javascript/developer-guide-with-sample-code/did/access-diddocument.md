# Access the DID Document

DID Document提供get方法获取内部对象的数量和个体：Subject，Controller，Multisig，Public Key，Authentication Key，Authorization Key，Verifiable Credential，Service。这些API理解和使用都相对简单，可直接参考API文档即可。

这小节提一下DID Document提供的获取DID Metadata和Default Key方法。

## Usage

```typescript
public getDefaultPublicKeyId(): DIDURL；
```

该方法是获取DID Document的主Key。

```typescript
public getMetadata(): DIDMetadata；
```

该方法获取DIDMetadata，DIDMetadata里包含了DID无法放入DID Document的信息内容。


