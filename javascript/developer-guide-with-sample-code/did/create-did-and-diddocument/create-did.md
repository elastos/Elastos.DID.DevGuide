# Create DID

DID provides three methods to create DID objects based on DID strings. The DID string is roughly divided into three parts: schema, method, and methodSpecificId, which are separated by “:”, e.g. did:elastos:icJ4z2DULrHEzYSvjKNJpKyhqFDxvYV7pN。

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
