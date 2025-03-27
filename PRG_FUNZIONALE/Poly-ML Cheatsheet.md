---
Date created: 06-03-25 • 10:56
tags:
  - Funzionale
Related PDF/DOC:
  - "[[L2 - Expressions and Types.pdf]]"
  - "[[L3 - Type errors, variables and functions.pdf]]"
Related Pages:
---
## Expressions
Example: `1+2*3;`

**Everything** in ML is an *Expression*
## Types
#### Basic types

| Type     | Definition                                                                                       |
| :------- | :----------------------------------------------------------------------------------------------- |
| `int`    | **Positive int:** `3`<br>**Negative int:** `-3` is written as `~3`<br>**Hexadecimal int:** `0x3` |
| `real`   | `123.0` -> `123.0`<br>`3E~3` -> `0.003`<br>`3.14e12` -> `3.14e12`                                |
| `bool`   | `true`/`false`                                                                                   |
| `char`   | `#"a"`                                                                                           |
| `string` | `"foo"`                                                                                          |
| `unit`   |                                                                                                  |
##### Special chars

| Char   | Description                |
| ------ | -------------------------- |
| `\n`   | Newline                    |
| `\t`   | Tabspace                   |
| `\\`   | Back-slash                 |
| `\"`   | Apices                     |
| `\xyz` | ASCII char with code `xyz` |
#### Complex types

| Type    | Definition  |
| :------ | :---------- |
| `tuple` | `( 1 , 2 )` |
## Operators
#### Arithmetic operators

|      Name      | Operator |                 Description                  |
| :------------: | :------: | :------------------------------------------: |
|    Addition    |   `+`    |                     ...                      |
|  Subtraction   |   `-`    |                     ...                      |
| Multiplication |   `*`    |                     ...                      |
|    Division    |   `/`    |            Division of **reals**             |
|    Divison     |  `div`   | Division of **integers** ( *rounding down* ) |
|    Modulus     |  `mod`   |        Remainder of integer division         |
|  Unary minus   |   `~`    |      Make the next integer **negative**      |

#### Comparison operators

|    Name    | Operator |                                  Description                                  |
| :--------: | :------: | :---------------------------------------------------------------------------: |
|  Equality  |   `=`    | Equal to ...<br><br>*!ATTENTION!*<br>Equality of **reals** is **NOT ALLOWED** |
|    Less    |   `<`    |                                Lesser than ...                                |
|    More    |   `>`    |                                 More than ...                                 |
| Less equal |   `<=`   |                              Lesser or equal ...                              |
| More equal |   `>=`   |                              More or equal ....                               |
|  Modulus   |   `<>`   |                         Remainder of integer division                         |

#### Logical operators

| Name |             Operator              |                                                          Description                                                           |
| :--: | :-------------------------------: | :----------------------------------------------------------------------------------------------------------------------------: |
| ...  |               `not`               |                                                  Negates the next expression                                                   |
| ...  |             `andalso`             |                                             **AND** ( *&&* ) with lazy evaluation                                              |
| ...  |             `orelse`              |                                             **OR** ( *\|\|* ) with lazy evaluation                                             |
| ...  | `if <cond> then <foo> else <bar>` | `cond` : Condition<br>`foo` : Result if **TRUE**<br>`bar` : Result if **FALSE**<br><br>`foo`&`bar` **MUST** have the same type |

#### General operators

|  Name  | Operator |     Description      |
| :----: | :------: | :------------------: |
| Concat |   `^`    | String concatenation |

## Function

| Function  | Input -> Output        | Description                                         |
| --------- | ---------------------- | --------------------------------------------------- |
| `floor`   | `real -> int`          | Rounds a real to the lower integer                  |
| `ceil`    | `real -> int`          | Rounds a real to the upper integer                  |
| `ord`     | `char -> int`          | Returns the ASCII value for the char                |
| `chr`     | `int -> char`          | Returns the char corresponding to the ASCII value   |
| `real`    | `int -> real`          | Returns a real from an int                          |
| `str`     | `char -> string`       | Returns a string from a char                        |
| `explode` | `string -> list(char)` | Returns a list of chars of the given string         |
| `implode` | `list(char) -> string` | Returns a string from the given list of characterse |


%%TODO: PP. 74 PDF. 1%%