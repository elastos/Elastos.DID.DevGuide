# List the published credentials

特定的 DID 可以公开多个凭证，ID 侧链也提供了检索特定 DID 的所有公开凭证的特性，DID SDK 提供了 list 方法。示例如下：

```java
DID did = new DID("did:elastos:ihnk95mguTQzRrRrxFmc72z3r3y5NwKSJX");

// list all declared credentials for did
List<DIDURL> vcIds = VerifiableCredential.list(did);
```

如果已经公开的凭证被撤销了，那么将不会别包含在结果列表中。

如果一个 DID 公开的凭证很多，那么在默认 list 的结果中最多只包含最近的 128 个凭证。如果需要获取所有的凭证，需要使用分页支持。分页列表的示例如下：

```java
DID did = new DID("did:elastos:ihnk95mguTQzRrRrxFmc72z3r3y5NwKSJX");

int skip = 0;
int limit = 256;
List<DIDURL> ids;
while (true) {
    ids = VerifiableCredential.list(did, skip, limit);
    if (ids == null)
        break;

     // process the list result...

     skip += ids.size();
}
```

其中 skip 和 limit 是两个用于分页的参数，分别代表跳过之前多少个 id 和结果中包含多少个 id。Java SDK 的实现 limit 最大是 512 个。

