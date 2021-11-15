# Sign and verify data by DID Document

DID Document可以代表DID对数据签名，因此DID Document提供方法用于签名和验签。

## Usage

```typescript
public signWithId(
	id: DIDURL | string | null,
	storepass: string,
	...data: Buffer[]
): Promise<string>；
```

该方法使用指定Key的私钥对数据data签名，返回Signature。

```typescript
public signWithStorePass(
	storepass: string,
	...data: Buffer[]
): Promise<string>；
```

该方法使用DID Document 的Default Key对数据data签名，返回Signature。若DID Document为多签Customized DID Document，则报错。

```typescript
public async signWithTicket(
	ticket: TransferTicket,
	storepass: string
): Promise<TransferTicket>；
```

该方法是用于TransferTicket多签。按照Customized DID Document多签规则，用于第二个以后的Controller DID Document签名。具体示例应用可见Transfer DID。

```typescript
public async signWithDocument(
	doc: DIDDocument,
	storepass: string
): Promise<DIDDocument>；
```

该方法是用于Customised DID Document多签。按照Customized DID Document多签规则，用于第二个以后的Controller DID Document签名。具体示例应用可见Create Multi-signed Customized DID。

```typescript
public verify(
	id: DIDURL | string | null,
	signature: string,
	...data: Buffer[]
): boolean;
```

该方法用于验证签名，key，signature和data三者其一不匹配就失败，主要防止数据被篡改。

id为签名的key id；signature为签名结果字符串；data为被签名的原始数据。
