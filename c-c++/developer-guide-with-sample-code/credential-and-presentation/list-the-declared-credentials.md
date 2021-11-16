# List the declared credentials

Credential List 是用来获取指定DID所有声明过的凭证的方法。

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

如果 limit 为默认值， 且链上实际declared凭证数量大于128，则只返回128个credential，若想获取剩下的credential，可以通过设置skip来实现；若limit > 512，且链上实际declared凭证大于512，则只返回512个credential，若想获取剩下的credential，可以通过设置skip来实现。
