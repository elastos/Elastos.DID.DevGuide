# How to Use and Verify Presentation

Presentation provides a variety of get methods to get internal elements, such as ID, holder, number of credentials, contents of credentials, etc. Refer to the API documentation for these methods.

Presentation also provides a method of completeness and validity check, which not only checks the correctness of each element of the Presentation itself, but also checks the validity of internal elements such as holder and Credential.

## Usage

```c
int Presentation_IsGenuine(Presentation *presentation)；
```

Presentation provides methods to check its integrity and each element, such as the integrity of Presentation holder and the integrity of each encapsulated Credential.

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
