# How to use and verify the presentation

Presentation提供多种get方法来获取内部各元素，比如Id，holder，Credential个数，各Credential等，这些方法详细可参考API文档。

Presentation同时还提供完整性和有效性检查的方法，不仅对Presentation自身各元素的检查正确性，还对内部元素比如holder和Credential进行了有效性检查。

## Usage

```typescript
public isGenuine(
	listener: VerificationEventListener = null
): boolean；
```

Presentation提供方法用于检查Presentation以及各元素的完整性，比如Presentation holder的完整性，封装的各Credential的完整性等。

listener是用于获取验证过程中的错误信息集，找到具体深层调用的错误点，便于定位。

```typescript
public isValid(
	listener: VerificationEventListener = null
): boolean ；
```

Presentation提供方法用于检查Presentation以及各元素的有效性，比如Presentation holder的有效性，封装的各Credential的有效性等。有效性包含完整性。
