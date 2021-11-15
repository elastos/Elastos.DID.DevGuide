# How to use and verify the presentation

Presentation提供多种get方法来获取内部各元素，比如Id，holder，Credential个数，各Credential内容等，这些方法详细可参考API文档。

Presentation同时还提供完整性和有效性检查的方法，不仅对Presentation自身各元素的检查正确性，还对内部元素比如holder和Credential进行有效性检查。

## Usage

```c
int Presentation_IsGenuine(Presentation *presentation)；
```
Presentation提供方法用于检查Presentation以及各元素的完整性，比如Presentation holder的完整性，封装的各Credential的完整性等。

 *      return value = -1, if error occurs;
 *      return value = 0, presentation isn't genuine;
 *      return value = 1, presentation is genuine.

```c
int Presentation_IsValid(Presentation *presentation)；
```
Presentation提供方法用于检查Presentation以及各元素的有效性，比如Presentation holder的有效性，封装的各Credential的有效性等。有效性包含完整性。

 *      return value = -1, if error occurs;
 *      return value = 0, presentation isn't valid;
 *      return value = 1, presentation is valid.
