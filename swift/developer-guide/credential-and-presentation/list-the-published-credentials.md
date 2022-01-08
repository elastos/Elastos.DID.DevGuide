# List the published credentials

特定的 DID 可以公开多个凭证，ID 侧链也提供了检索特定 DID 的所有公开凭证的特性，DID SDK 提供了 list 方法。示例如下：

A specific DID can declare multiple credentials, and the ID side chain also provides the features of retrieving all declared credentials of a specific DID. The DID SDK provides the listing method. Example:

```
let did = try DID("did:elastos:ihnk95mguTQzRrRrxFmc72z3r3y5NwKSJX")

// list all declared credentials for did
let vcIds = try VerifiableCredential.list(did)
```

如果已经公开的凭证被撤销了，那么将不会别包含在结果列表中。

If the declared credential is revoked, it will not be included in the resulting list.

如果一个 DID 公开的凭证很多，那么在默认 list 的结果中最多只包含最近的 128 个凭证。如果需要获取所有的凭证，需要使用分页支持。分页列表的示例如下：

If a DID has declared many credentials, only the most recent 128 credentials will be included in the default resulting list. To get all the credentials, paging support will be needed. Here is an example of the paginated list:

```
let did = try DID("did:elastos:ihnk95mguTQzRrRrxFmc72z3r3y5NwKSJX")

var skip = 0
let limit = 256
var ids: [DIDURL] = [ ]
while (true) {
    ids = try VerifiableCredential.list(did, skip, limit)
    if ids.count == 0 {
        break
		}
     // process the list result...

     skip += ids.count
}
```

其中 skip 和 limit 是两个用于分页的参数，分别代表跳过之前多少个 id 和结果中包含多少个 id。Java SDK 的实现 limit 最大是 512 个。

Skip and limit are two parameters for paging, which respectively represent how many previous IDs are skipped and how many IDs are included in the result. The maximum implementation limit of Java SDK is 512.
