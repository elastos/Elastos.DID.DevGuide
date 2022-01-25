# Create DID

DID provides three methods to construct the DID object according to the DID string and destroy the DID object after use. The DID string is roughly divided into three parts: schema, method, and methodSpecificId. Each part is separated by “:”, for example: did:elastos:ICJ4Z2Dulrhezysvjknjpkyhqfdxyv7pn.

## Example

```c
//create DID -- did:elastos:icJ4z2DULrHEzYSvjKNJpKyhqFDxvYV7pN
DID *did = DID_FromString("did:elastos:icJ4z2DULrHEzYSvjKNJpKyhqFDxvYV7pN");
DID_Destroy(did);

//create DID -- did:elastos:icJ4z2DULrHEzYSvjKNJpKyhqFDxvYV7pN
did = DID_New("icJ4z2DULrHEzYSvjKNJpKyhqFDxvYV7pN");
DID_Destroy(did);

//create DID -- did:elastos:icJ4z2DULrHEzYSvjKNJpKyhqFDxvYV7pN
did = DID_NewWithMethod("elastos", "icJ4z2DULrHEzYSvjKNJpKyhqFDxvYV7pN");
DID_Destroy(did);
```

## Usage

```c
DID *DID_FromString(const char *idstring)；
```

The method generates DID object according to idstring, where it's a string containing id information, such as did:elastos:ixxxxxxx.

```c
DID *DID_New(const char *method_specific_string);
```

The method generates a new DID object according to the provided method specific string and default “method” string, such as ixxxxxxx.

```c
DID *DID_NewWithMethod(
    const char *method,
    const char *method_specific_string);
```

The method generates a new DID object according to the provided method specific string and method string. For example, method specific is ixxxxxxx and method string is method.

```c
void DID_Destroy(DID *did);
```

DID object obtained by the above three methods needs to be destroyed after use.
