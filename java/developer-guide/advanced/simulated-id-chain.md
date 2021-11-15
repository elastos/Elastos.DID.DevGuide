# Simulated ID chain

因为开发的需要，Java SDK 内置了一个仿真的 ID 侧链。仿真 ID 侧链的后端逻辑和 Elastos ID side chain 的实现完全一致，并基于 HTTP 提供了一组 REST API，特别方便开发期间的测试和验证。

另外 Simulated ID chain 和 Elastos ID side chain 有很重要的区别

- Simulated ID chain 是基于本地内存结构实现，没有 ID side chain 的确认时间，ID 交易立即生效
- Simulated ID chain 使用 HTTP API 对外提供服务，不需要钱包、gas 费等
- Simulated ID chain 没有数据持久化，仅面向开发时

## 在程序中启动和使用 Simulated ID chain

```java
SimulatedIDChain simChain = new SimulatedIDChain();
simChain.start();
DIDAdapter adapter = simChain.getAdapter();

DIDBackend.initialize(adapter);
```

按照上述示例启动 Simulated ID chain 并初始化 DIDBackend，那么 DID SDK 将会使用仿真 ID 侧链对 DID 进行解析和发布。Simulated ID chain 默认工作再 localhost 的 9123 端口，应用可以通过 `SimulatedIDChain(String host, int port)` 构造对象，并指定需要的侦听接口和端口。

## Simulated ID chain 的 API

### Create ID transaction

- Endpoint: /idtx
- Method: POST
- Content-Type: application/json
- Request Body: Any ID Chain request payload, include DID and VerifiableCredential operations.
- Response Body: None
- Success Status: 202

### Resolve

- Endpoint: /resolve
- Method: POST
- Content-Type: application/json
- Request Body: Any DID or VC resolve/list request.
- Response Body: Resolved result.
- Success Status: 200

### Reset data

- Endpoint: /reset
- Parameters(optional): idtxsonly, vctxsonly
- Method: POST
- Request Body: None
- Response Body: None
- Success Status: 200

### Shutdown

- Endpoint: /shutdown
- Method: POST
- Request Body: None
- Response Body: None
- Success Status: 202