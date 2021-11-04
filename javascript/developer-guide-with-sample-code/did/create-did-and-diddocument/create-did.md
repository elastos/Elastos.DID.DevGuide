# Create DID

DID提供三种方法根据DID字符串来构造DID对象。DID字串大致分为三个部分：schema，method和methodSpecificId，每个部分用 ：隔开，比如did:elastos:icJ4z2DULrHEzYSvjKNJpKyhqFDxvYV7pN。

## Example

```typescript
let did1 = new DID("did:elastos:icJ4z2DULrHEzYSvjKNJpKyhqFDxvYV7pN");
let did2 = new DID("elastos", "abc");
let did3 = DID.createFrom("did:elastos:littlefish", 0, 22);
let did4 = DID.From("did:elastos:icJ4z2DULrHEzYSvjKNJpKyhqFDxvYV7pN");
let did5 = DID.From(did2);
```

## Usage

```typescript
public constructor(
    methodOrDID: string,
    methodSpecificId: string | null = null,
    start?: number, limit?: number
)；
```

```typescript
public static createFrom(
    methodOrDID: string,
    start: number,
    limit: number
): DID；
```

```typescript
public static from(
    did: DID | string | null
): DID | null；
```
