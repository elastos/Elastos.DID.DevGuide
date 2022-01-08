# Sign and Verify Data by DID Document

The DID document can sign the data on behalf of DID, so the DID document offers methods for signing and verifying data.

## Usage

```typescript
public signWithId(
	id: DIDURL | string | null,
	storepass: string,
	...data: Buffer[]
): Promise<string>；
```

This method signs the data with the private key of the specified key, and then return signature.

```typescript
public signWithStorePass(
	storepass: string,
	...data: Buffer[]
): Promise<string>；
```

This method uses the default key of the DID document to sign the data, and then return the signature. If the DID document is a multi-signed customized DID document, an error is returned.

```typescript
public async signWithTicket(
	ticket: TransferTicket,
	storepass: string
): Promise<TransferTicket>；
```

This method applies to the multi-signing of TransferTicket. According to the multi-signature rule of the customized DID document, this method is used for the second and later controller DID document signature. See “Transfer DID” for specific examples.

```typescript
public async signWithDocument(
	doc: DIDDocument,
	storepass: string
): Promise<DIDDocument>；
```

The method is used for the multi-signing of the customized DID Document. According to the multi-signature rule of the customized DID document, it is used for the second and later controller DID document signature. See “Create multi-signed customized DID” for specific examples.

```typescript
public verify(
	id: DIDURL | string | null,
	signature: string,
	...data: Buffer[]
): boolean;
```

This method verifies the signature. If the key, signature, and data don't match, the verification fails, which mainly prevents data from being tampered.

id is the ID of the key; the signature denotes the string of the results containing a signature; data refers to the original signed data.
