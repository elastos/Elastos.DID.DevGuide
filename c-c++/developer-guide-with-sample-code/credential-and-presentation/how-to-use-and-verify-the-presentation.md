# How to use and verify the presentation

Presentation提供多种get方法来获取内部各元素，比如Id，holder，Credential个数，各Credential内容等，这些方法详细可参考API文档。

Presentation provides a variety of get methods to get internal elements, such as Id, holder, number of credentials, contents of credentials, etc. Refer to API documentation for these methods.

Presentation同时还提供完整性和有效性检查的方法，不仅对Presentation自身各元素的检查正确性，还对内部元素比如holder和Credential进行有效性检查。

Presentation also provides a method of completeness and validity check, which not only checks the correctness of each element of the Presentation itself, but also checks the validity of internal elements such as holder and Credential.

## Usage

```c
int Presentation_IsGenuine(Presentation *presentation)；
```

Presentation提供方法用于检查Presentation以及各元素的完整性，比如Presentation holder的完整性，封装的各Credential的完整性等。

Presentation provides methods to check the integrity of Presentation and each element, such as the integrity of Presentation holder and the integrity of each encapsulated Credential.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, presentation isn't genuine;
  ```
* ```
   return value = 1, presentation is genuine.
  ```

```c
int Presentation_IsValid(Presentation *presentation)；
```

Presentation提供方法用于检查Presentation以及各元素的有效性，比如Presentation holder的有效性，封装的各Credential的有效性等。有效性包含完整性。

Presentation provides methods to check the Presentation and the validity of each element, such as the validity of Presentation holder and the validity of each encapsulated Credential. Validity includes completeness.

* ```
   return value = -1, if error occurs;
  ```
* ```
   return value = 0, presentation isn't valid;
  ```
* ```
   return value = 1, presentation is valid.
  ```
