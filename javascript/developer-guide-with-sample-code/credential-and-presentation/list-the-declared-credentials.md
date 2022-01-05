# List the declared credentials

Credential List 是用来获取指定DID所有声明过的凭证的方法。

Credential list is a method to obtain all the credentials declared by the specified DID.

## Example

```typescript
//if did has 6 credentials, ids.length == 6
let ids = await VerifiableCredential.list(did);

//if did has 200 declared credentials, ids1.length == 128, ids2.length == 72
let ids1 = await VerifiableCredential.list(did);
let ids2 = await VerifiableCredential.list(did, ids1.length);

//if did has 1028 declared credentials, ids1.length == 512, ids2.length == 512, ids3.length == 4
ids1 = await VerifiableCredential.list(did, 0, 1028);
ids2 = await VerifiableCredential.list(did, 512, 516);
let ids3 = await VerifiableCredential.list(did, 1024, 4);
```

## Usage

```typescript
public static list(
	did: DID,
	skip = 0,
	limit = 0
): Promise<DIDURL[]>;
```

did为需要枚举所有declared凭证的所有者；skip为忽略前skip个credentials；limit表明该次list最多可接受多少个credentials。

Did is the owner who needs to list all the declared credentials; skip is to ignore the first skip-th credentials; limit indicates the maximum number of credentials that can be listed.

如果 limit 为默认值， 且链上实际declared凭证数量大于128，则只返回128个credential，若想获取剩下的credential，可以通过设置skip来实现；若limit > 512，且链上实际declared凭证大于512，则只返回512个credential，若想获取剩下的credential，可以通过设置skip来实现。

If limit is the default value and the actual number of declared credentials in the chain surpasses 128, only 128 credentials will be returned. Set skip to get the remaining credentials. If limit > 512 and there are more than 512 declared credentials in the chain, only 512 credentials will be returned. Set skip to get the remaining credentials.
