---
Date created: 06-03-25 • 10:56
tags:
  - Funzionale
Related PDF/DOC:
  - "[[L2 - Expressions and Types.pdf]]"
  - "[[L3 - Type errors, variables and functions.pdf]]"
Related Pages:
---
## Proprieties
- <mark class="hltr-red">Functional</mark>
	-  Basic mode of computation: **definition and application of functions**.
	-  Functions can be considered as <mark class="hltr-purple">code</mark> or as <mark class="hltr-orange">values</mark> ( *i.e.* as parameters of other functions).
- <mark class="hltr-red">Rule-based programming</mark>
	- `If-then-else` rules implemented via **pattern matching**.
- <mark class="hltr-red">No side effects</mark>
	- Computation is by *evaluation of expressions*, **NOT** by *assigning values to variables*.
		- *i.e.* in C, `a=b+c` modifies the value of `a` ( **[[#^128b6c|side effect]]** ).
		  In ML, `b+c` creates a *new element associated with the result*.
	- If needed, side effects are allowed ( *printing output* ), but are not the main means of computation.
- <mark class="hltr-red">Strong typing</mark> 
	- All values and variables have types that can be determined at **compile-time**. 
	- Variable declarations is usually *not needed*.
- <mark class="hltr-red">Polymorphism</mark> 
	- A function can have arguments of different types
- <mark class="hltr-red">ABS ( Abstract Data Types )</mark> 
	- Ability to construct new types
- <mark class="hltr-red">Eager evaluation</mark>
	- Except ( *Lazy evaluation* ) in:
		- `andalso`
		- `orelse`
		- `if-then-else`


> [!info]- Side effects
> Any change in the system **state** that occurs outside the immediate context of the computation.
> 
>> [!example]
> > In C we can consider assigning a variable a **side effect**:
> > `a=b+c` 

^128b6c

## Syntactic entities
### Expressions
Syntactic entities ( *"phrases"* ) whose evaluation either **produces a value** or **does not terminate**.

Usually composed of: 
- **A single entity** ( constant, variable ).
- An **operator applied to a number of arguments** ( which are also expressions ).

> [!example] Title
>  Single: `a`
>  Operator: `a+b`

**Everything** in ML is an *Expression*, except **commands**.

### Commands
Syntactic entity whose evaluation **does not necessarily return a value** but can **have a [[#^128b6c|side effect]]**.

## Types
### Control proprieties
#### Equivalence
Two types `T` and `S` are <mark class="hltr-orange">equivalent</mark> if **every object** of type `T` **is also of type** `S`, and vice versa.

There are two types of equivalence:
- **Equivalence by name** : The definition of a type is *opaque*.
- **Structural equivalence** : The definition is *transparent*.

ML uses **structural equivalence** <mark class="hltr-red">except</mark> for **types defined with** `datatype`.

##### Equivalence by name
Two types are equivalent if they have the **same name**.

##### Structural equivalence
Two types are structurally equivalent if they have the **same structure**: substituting names for the relevant definitions, identical types are obtained.

Two types constructed using the **same type constructor** applied to equivalent types, are equivalent.
#### Compatibility
`T` is compatible with `S` **if objects of type** `T` **can be used in contexts where objects of type** `S` **are expected**.

If `T` is compatible with `S`, there is some type **conversion** mechanism. The main ones are: 
-  **Implicit conversion**, also called *coercion* : The language implementation does the conversion, with no mention at the language level.
- **Explicit conversion**, or *cast* : The conversion is mentioned in the program.

#### Inference
Types of operands and results of **arithmetic** expressions must agree.

> [!example]
> `(a+b) * 2.0` 
> 
> The right operand is a `real`, therefore `a+b` must be a `real`, therefore, so are `a` and `b`

In a [[#Comparison operators|comparison]], both arguments **have the same type**.

Type inference follows this rules:
- If an **expression** used *as an argument of a function* **is of a known type**, the <mark class="hltr-orange">parameter must be of that type</mark>.
- If the **expression** *defining the result of a function* **is of a known type**, the <mark class="hltr-orange">function returns that type</mark>.
- If **there is no way to determine the types of the arguments** *of an overloaded function* ( *such as +* ), the <mark class="hltr-orange">type is the default ( usually integer )</mark>.
- If **there is no way to determine the types of the arguments and operators are not used**, we can <mark class="hltr-orange">use the generic type ‘a</mark>.
- **If there is no way to determine the type of two arguments and there is no relation among them**, we can <mark class="hltr-orange">use the generic types ‘a and ‘b</mark>. 
### Basic types

| Type      | Definition                                                                                       |
| :-------- | :----------------------------------------------------------------------------------------------- |
| `int`     | **Positive int:** `3`<br>**Negative int:** `-3` is written as `~3`<br>**Hexadecimal int:** `0x3` |
| `real`    | `123.0` -> `123.0`<br>`3E~3` -> `0.003`<br>`3.14e12` -> `3.14e12`                                |
| `bool`    | `true`/`false`                                                                                   |
| `char`    | `#"a"`                                                                                           |
| `string`  | `"foo"`                                                                                          |
| `unit`    |                                                                                                  |
| `generic` | `'a` defined on use                                                                              |
#### Special chars

| Char   | Description                |
| ------ | -------------------------- |
| `\n`   | Newline                    |
| `\t`   | Tabspace                   |
| `\\`   | Back-slash                 |
| `\"`   | Apices                     |
| `\xyz` | ASCII char with code `xyz` |
### Complex types

#### Tuples

> [!info] Definition:
>```ml
> > val tup = ( 1 , "two", 3.0, ( 4, [ 5, 6 ] ) );
>```

> [!info] Access:
>```ml
>> val tup = ( 1 , "two", 3.0, ( 4, [ 5, 6 ] ) );
>>
>> #1 (tup); 
>
>--------------------------------------------------------------------
>
>> val it = 1: int
>```
>

#### Lists

> [!info] Definition:
>```ml
> > val lst = [ 1 , 2 , 3 ];
>```

> [!info] Access:
>```ml
>> val lst = [ 1 , 2 , 3 ];
>>
>> hd (lst); 
>> tl (lst)
>
>--------------------------------------------------------------------
>
>> val it = 1: int
>> val it = [ 2, 3 ] : int list
>```
>

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
|  Hexadecimal   |   `0x`   |     Read the next integer as an **Hex**      |

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
| ...  |       `<foo> andalso <bar>`       |                                             **AND** ( `&&` ) with lazy evaluation                                              |
| ...  |       `<foo> orelse <bar>`        |                                             **OR** ( `\|\|` ) with lazy evaluation                                             |
| ...  | `if <cond> then <foo> else <bar>` | `cond` : Condition<br>`foo` : Result if **TRUE**<br>`bar` : Result if **FALSE**<br><br>`foo`&`bar` **MUST** have the same type |

#### General operators

|    Name     | Operator |           Description           |
| :---------: | :------: | :-----------------------------: |
|   Concat    |   `^`    |      String concatenation       |
| List Concat |   `@`    |       List concatenation        |
|    Cons     |   `::`   | `'a` to `'a list` concatenation |

## Functions
In ML functions are **just another type of value**, represented by a **parametrized expression**.

### Function definition
#### $\lambda$ Lambda Functions

> [!info] Definition:
> ```ml
> > val increment = fn n => n+1
> ```


> [!info] Call:
> ```ml
> > increment( 1 );
> 
>--------------------------------------------------------------------
>
>> val it = 2: int
> ```

#### Normal Functions
As **Lambda Functions** are the *standard*, in ML is present a *syntactic sugar notation* for **functions with names**:

> [!info] Definition:
> ```ml
> > fun add ( a : int , b : int ) : int = a + b;
> ```

### Useful Functions

| Function  | Input -> Output        | Description                                       |
| --------- | ---------------------- | ------------------------------------------------- |
| `floor`   | `real -> int`          | Rounds a real to the lower integer                |
| `ceil`    | `real -> int`          | Rounds a real to the upper integer                |
| `ord`     | `char -> int`          | Returns the ASCII value for the char              |
| `chr`     | `int -> char`          | Returns the char corresponding to the ASCII value |
| `real`    | `int -> real`          | Returns a real from an int                        |
| `str`     | `char -> string`       | Returns a string from a char                      |
| `explode` | `string -> list(char)` | Returns a list of chars of the given string       |
| `implode` | `list(char) -> string` | Returns a string from the given list of chars     |

## Variables 
### Environments
An **Environment** is a set of pairs of <mark class="hltr-purple">identifiers</mark> and <mark class="hltr-orange">values</mark>. In other words is the container for all the *named* values.

It can be modified by **[[#Assignment|val-declarations]]**.

### Assignment
Variables <mark class="hltr-red">CANNOT</mark> be **modified** only **reassigned**.

```ml
val <name> = <value>;
val pi = 3.14

val <name> : <type> = <value>;
val pi : real = 3.14 

val <name> = <expression>;
```

%%TODO L4%%