# didutils command line tool

在 DID 的源代码仓库中除了 SDK 和示例代码，还包含一个 DID 相关的命令行工具，这个工具提供了日常 DID 使用中常用的命令集。

DID SDK 的 release 中已经包含了预编译好的 jar 包，可以从[最新的 release](https://github.com/elastos/Elastos.DID.Java.SDK/releases/latest) 页面的 Assets 中下载 didutils.jar 使用。

下载后可以通过以下命令使用该工具：

```bash
java -jar didutils.jar [COMMAND] <ARGS>
```

可以通过以下命令获得支持的命令列表：

```bash
java -jar didutils.jar --help
```

## 支持的命令列表

| 命令 | 描述 |
| ----------- | ----------- |
| resolvedid | Resolve DID from the ID side chain. |
| verifydoc | Verify the DID document. |
| verifyvc | Verify the verifiable credential. |
| verifyvp | Verify the verifiable presentation. |
| verifyjwt | Verify the JTW/JWS token. |
| createidentity | Create a RootIdentity. |
| listidentity | List the RootIdentities. |
| deleteidentity | Delete the root identity. |
| createdid | Create a DID. |
| listdid | List the DIDs. |
| deletedid | Delete the DID. |
| displaydid | Display the DID document and metadata. |
| publishdid | Publish DID to ID transaction payload. |
| recover | Recover the wrong DIDs. |
| simchain | Simulated ID Chain for testing. |
| jsonld | JSON-LD tools. |
| shell | Interactive shell. |

## 获取具体命令的帮助

上述每个命令都可以支持 `--help` 命令行参数获取命令的帮助，比如获取 resolvedid 命令的帮助：

```bash
java -jar didutils.jar resolvedid --help
```

