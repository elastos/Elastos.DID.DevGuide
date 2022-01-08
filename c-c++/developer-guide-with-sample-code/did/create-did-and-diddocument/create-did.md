# Create DID

DID提供三种方法根据DID字符串来构造DID对象，使用完后销毁DID对象。DID字串大致分为三个部分：schema，method和methodSpecificId，每个部分用 ：隔开，比如did:elastos:icJ4z2DULrHEzYSvjKNJpKyhqFDxvYV7pN。

DID provides three methods to construct the DID object according to the DID string and destroy the DID object after use. The DID string is roughly divided into three parts: schema, method and methodSpecificId. Each part is separated by “:”, for example, did:elastos:ICJ4Z2Dulrhezysvjknjpkyhqfdxyv7pn.

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

该方法根据idstring生成DID object。其中`idstring`是包含id信息的字符串，比如did:elastos:ixxxxxxx。

The method generates DID object according to idstring. In which _idstring_ is a string containing id information, such as did:elastos:ixxxxxxx.

```c
DID *DID_New(const char *method_specific_string);
```

该方法根据提供的method specific string和默认的‘method’ 字符串生成新的DID object，比如ixxxxxxx。

The method generates a new DID object according to the provided method specific string and the default “method” string, such as ixxxxxxx.

```c
DID *DID_NewWithMethod(
    const char *method,
    const char *method_specific_string);
```

该方法根据提供的method specific string和method字符串生成新的DID object，比如method specific为 ixxxxxxx，method字符串为method。

The method generates a new DID object according to the provided method specific string and method string, for example, method specific is ixxxxxxx and method string is method.

```c
void DID_Destroy(DID *did);
```

用以上三种方法得到的DID object，用完后需要销毁对象。

DID object obtained by the above three methods needs to be destroyed after use.
