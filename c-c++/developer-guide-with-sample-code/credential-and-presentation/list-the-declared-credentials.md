# List the declared credentials

Credential List 是用来获取指定DID所有声明过的凭证的方法。

_Credential List_ is a method used to obtain all declared credentials of the specified DID.

## Example

```c
DIDURL *buffer[512] = {0};
//if did has 2 credentials, size == 2
ssize_t size = Credential_List(&did, buffer, sizeof(buffer), 0, 2);

//if did has 200 declared credentials, size == 128, size2 == 72
size = Credential_List(&did, buffer, sizeof(buffer), 0, 0);
ssize_t size2 = Credential_List(&did, buffer, sizeof(buffer), 128, 0);

//if did has 1028 declared credentials, size == 512, size2 == 512, size3 == 4
size = Credential_List(&did, buffer, sizeof(buffer), 0, 520);
size2 = Credential_List(&did, buffer, sizeof(buffer), 512, 516);
ssize_t size3 = Credential_List(&did, buffer, sizeof(buffer), 1024, 4);
```

## Usage

```c
ssize_t Credential_List(
    DID *did,
    DIDURL **buffer,
    size_t size,
    int skip, int limit);
```

`did`为需要枚举所有declared凭证的所有者；`skip`为忽略前skip个credentials；`limit`表明该次list最多可接受多少个credentials。

_did_ is the owner who needs to enumerate all declared credentials; _skip_ is to ignore the first skip credentials; _limit_ indicates how many credentials the list can accept at most.

如果 limit 为默认值， 且链上实际declared凭证数量大于128，则只返回128个credential，若想获取剩下的credential，可以通过设置skip来实现；若limit > 512，且链上实际declared凭证大于512，则只返回512个credential，若想获取剩下的credential，可以通过设置skip来实现。

If limit is the default value and the actual number of declared credentials in the chain is greater than 128, only 128 credentials will be returned. If you want to get the remaining credentials, you can set skip. If the limit is greater than 512 and the actual declared credential on the chain is greater than 512, only 512 credentials will be returned. If you want to get the remaining credentials, you can set skip.
