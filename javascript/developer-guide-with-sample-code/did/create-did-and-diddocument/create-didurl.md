# Create DIDURL

DIDURL提供方法构造DIDURL对象。DIDURL大致分为四个部分：DID，path，query和fragment，其中DID规则查看DID内容，path以“/”开头， query以“？”开头，fragment以“#”开头。比如did:elastos:foobar/path/to/resource?test=true&key=value&name=foobar#helloworld。

## Example

```typescript
let did = new DID("did:elastos:foobar");
let id1 = new DIDURL("did:elastos:foobar#helloworld");
let id2 = new DIDURL(“did:elastos:foobar/path/to/resource?test=true&key=value&name=foobar#helloworld”);
let id3 = new DIDURL("did:elastos:foobar#helloworld", did);
```

## Usage

```typescript
public constructor(
	url?: DIDURL | string,
	context?: DID
)；
```

`url`为代表DIDURL字符串内容；`context`为DIDURL里的DID内容。两者不可以同时为null或undefined。
