# Sign and verify data by DID Document

DID Document可以代表DID对数据签名，因此DID Document提供方法用于签名和验签。

The DID document can sign the data on behalf of DID, so the DID document offers methods for signing and verifying data.

## Usage

```typescript
public signWithId(
	id: DIDURL | string | null,
	storepass: string,
	...data: Buffer[]
): Promise<string>；
```

该方法使用指定Key的私钥对数据data签名，返回Signature。

This method signs the data with the private key of the specified key, and then return signature.

```typescript
public signWithStorePass(
	storepass: string,
	...data: Buffer[]
): Promise<string>；
```

该方法使用DID Document 的Default Key对数据data签名，返回Signature。若DID Document为多签Customized DID Document，则报错。

This method uses the default key of the DID document to sign the data, and then return the signature. If the DID document is a multi-signed customized DID document, an error is returned.

```typescript
public async signWithTicket(
	ticket: TransferTicket,
	storepass: string
): Promise<TransferTicket>；
```

该方法是用于TransferTicket多签。按照Customized DID Document多签规则，用于第二个以后的Controller DID Document签名。具体示例应用可见Transfer DID。

This method applies to the multi-signing of TransferTicket. According to the multi-signature rule of the customized DID document, this method is used for the second and later controller DID document signature. See “Transfer DID” for specific examples.

```typescript
public async signWithDocument(
	doc: DIDDocument,
	storepass: string
): Promise<DIDDocument>；
```

该方法是用于Customised DID Document多签。按照Customized DID Document多签规则，用于第二个以后的Controller DID Document签名。具体示例应用可见Create Multi-signed Customized DID。

The method is used for the multi-signing of the customized DID Document. According to the multi-signature rule of the customized DID document, it is used for the second and later controller DID document signature. See “Create multi-signed customized DID” for specific examples.

```typescript
public verify(
	id: DIDURL | string | null,
	signature: string,
	...data: Buffer[]
): boolean;
```

该方法用于验证签名，key，signature和data三者其一不匹配就失败，主要防止数据被篡改。

This method verifies the signature. If the key, signature and data do not match, the verification fails, which mainly prevents data from being tampered.

id为签名的key id；signature为签名结果字符串；data为被签名的原始数据。

Id is the ID of the key; signature denotes the string of the results containing a signature; data refers to the original signed data.
