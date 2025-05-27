---
Date created: 06-03-25 • 10:56
tags:
  - Funzionale
Related PDF/DOC:
  - "[[L2 - Expressions and Types.pdf]]"
  - "[[L3 - Type errors, variables and functions.pdf]]"
  - "[[L4 - Abstraction of control.pdf]]"
  - "[[L5 - Patterns and local environment.pdf]]"
  - "[[L6 - Input-output, exceptions and polymorphic functions.pdf]]"
  - "[[L7 - Higher order and curried functions.pdf]]"
  - "[[L8 - Curried functions and data types.pdf]]"
  - "[[L9 - Abstract_data_types.pdf]]"
  - "[[L10.1 - Lambda Calculus.pdf]]"
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

| Type      | Definition                                                                                         |
| :-------- | :------------------------------------------------------------------------------------------------- |
| `int`     | **Positive int:** `3`<br>**Negative int:** `-3` is written as `~3`<br>**Hexadecimal int:** `0x3`   |
| `real`    | `123.0` -> `123.0`<br>`3E~3` -> `0.003`<br>`3.14e12` -> `3.14e12`                                  |
| `bool`    | `true`/`false`                                                                                     |
| `char`    | `#"a"`                                                                                             |
| `string`  | `"foo"`                                                                                            |
| `unit`    | Used for expressions and functions that **do not return a value**. <br>It has a unique value: `()` |
| `generic` | `'a` defined on use                                                                                |
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
An **empty list** is indicated by the keyword `nil`.

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
##### Equality types
> *Types that allow the use of equality tests (`=` / `<>`)*

**Type variables**, whose values are restricted to be an equality type, are indicated with a double quote `''a`.

Types that allow equality:
- Integers
- Booleans
- Characters
- Tuples
- Lists

Types that <mark class="hltr-red">DO NOT</mark> allow equality:
- **Reals**
- **Functions**



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

#### Patterns
Patterns have *simple* types but can be combined freely to extend them to **nested patterns**.

|      Name       |                                                                     Pattern                                                                     |                                       Description                                        |
| :-------------: | :---------------------------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------: |
|    WIldcard     |                                                                       `_`                                                                       |                                     Matches anything                                     |
|      Value      |                                               `42    (int)`<br>`true   (bool)`<br>`#"a"   (char)`                                               |                                 Matches a specific value                                 |
|    Variable     |                                                                       `x`                                                                       |                           Matches anything and binds it to `x`                           |
|      Tuple      |                                                                    `(x, y)`                                                                     |                         Matches a pair and binds each component                          |
| List structrure | `[]   (Empty list)`<br>`[x]      (Singleton list)`<br>`[x, y]   (List with two elements)`<br>`x :: xs      (Matches a head and tail of a list)` |                               Matches lists by structure.                                |
| Record pattern  |                                                                `{a = x, b = y}`                                                                 |                            Matches record with fields a and b                            |
|       Or        |                                              `("yes" \| "no")     (Matches either "yes" or "no")`                                               |                          Matches if any of the patterns matches                          |
|       As        |                                  `lst as x :: xs       (* Binds lst to the full list and matches head/tail *)`                                  | Binds the whole matched value to a name while also matching and assigning subcomponents. |


## Functions
In ML functions are **just another type of value**, represented by a **parameterized expression**.

### Polymorphism
> ***Polymorphic functions**: functions that permit multiple types.*

ML uses `‘a` for denoting generic polymorphic type; this is where **[[#Inference]]** comes into play.

>[!example]-
>```ml
>> fun identity (x) = x; 
> val identity = fn: 'a -> 'a 
> 
> > identity (2); 
> val it = 2: int 
> 
> > identity (2.0); 
> val it = 2.0: real
> 
> > identity (2) + floor (identity (3.5));
> val it = 5: int
>```

#### Polymorphism restrictions
Some operators restrict polymorphism ...
- **Arithmetic operators**: `+` / `-` / `*` / `~` 
- **Inequality comparison operators**
( *Both this operators use **default types*** )

- **Division-related operators** : `/` / `div` / `mod`
- **Boolean connectives** : `andalso` / `orelse` / `not`
- **String concatenation operators**
- **Type conversion operators** : `ord` / `chr` / `real` / `str` / `floor` / `ceiling` / `round` / `truncate`

... While others allow it :
- **Tuples operators** : `( .. , .. )` / `#1`
- **List operators** : `::` / `@` / `hd` / `tl` / `nil` / `[]`
- **Equality operators** : `=` / `<>`

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

#### Pattern matching
Similar to *switch-case* statements trough the use of [[#Patterns|patterns]]:

> [!info] Definition:
> With *Normal* Functions :
> ```ml
>  fun [name] ( [first pattern] ) = [first expression]  
>  | [name] ( [second pattern] ) = [second expression];
> ```
>
> With *Lambda* Functions :
> ```ml
> fn x => case x of
> [pattern 1] => [expression 1]
> | [pattern 2] => [expression 2]
>```

### Useful Functions

| Function                                                   | Input -> Output                                         | Description                                                                                                                                                                           |
| ---------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `floor`                                                    | `real -> int`                                           | Rounds a real to the lower integer                                                                                                                                                    |
| `ceil`                                                     | `real -> int`                                           | Rounds a real to the upper integer                                                                                                                                                    |
| `ord`                                                      | `char -> int`                                           | Returns the ASCII value for the char                                                                                                                                                  |
| `chr`                                                      | `int -> char`                                           | Returns the char corresponding to the ASCII value                                                                                                                                     |
| `real`                                                     | `int -> real`                                           | Returns a real from an int                                                                                                                                                            |
| `str`                                                      | `char -> string`                                        | Returns a string from a char                                                                                                                                                          |
| `explode`                                                  | `string -> list(char)`                                  | Returns a list of chars of the given string                                                                                                                                           |
| `implode`                                                  | `list(char) -> string`                                  | Returns a string from the given list of chars                                                                                                                                         |
| `print`                                                    | `string -> unit`                                        | Prints the given string                                                                                                                                                               |
| `Real.toString()`<br>`Bool.toString()`<br>`Int.toString()` | `real -> string`<br>`bool -> string`<br>`int -> string` | Various conversions from the given value to string                                                                                                                                    |
| `ValOf()`                                                  | `a' option -> 'a`                                       | Returns the value associated with the value option variable.<br><br>Best use `case` when possible:<br>`fun getOptionValue opt =`<br>`case opt of`<br>`SOME x => x`<br>`\| NONE => 0 ` |
#### File Functions

| Function                               | Input -> Output            | Description                                                                                                                                                                                                                                                                              |
| -------------------------------------- | -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `TextIO.openIn( [filename] )`          | `string -> file`           | Opens the file in input mode and binds it to the variable.                                                                                                                                                                                                                               |
| `TextIO.endOfStream ( [file] )`        | `file -> bool`             | Checks if the EOF has been reached.                                                                                                                                                                                                                                                      |
| `TextIO.inputN( [file] , [ch_num] )`   | `(file,int) -> string`     | Reads the given number of chars from the file, returns them as a string and moves the file cursor forward.                                                                                                                                                                               |
| `TextIO.inputLine( [file] )`           | `file -> string option`    | Reads a whole line from the file, returns it and moves the cursor forwards.<br><br>The option is `SOME` if it returns a string and `NONE` if there are no more lines to read.                                                                                                            |
| `TextIO.input( [file] )`               | `file -> string`           | Reads the whole file.                                                                                                                                                                                                                                                                    |
| `TextIO.input1( [file] )`              | `file -> char option`      | Reads a single char.<br><br>The option is `SOME` if it returns a char and `NONE` if there are no more chars to read.                                                                                                                                                                     |
| `TextIO.closeIn( [file] )`             | `file -> unit`             | Closes the file.                                                                                                                                                                                                                                                                         |
| `TextIO.lookahead( [file] )`           | `void -> char option`      | Reads a single char.<br>**DOES NOT** move the cursor forward.<br><br>The option is `SOME` if it returns a char and `NONE` if there are no more chars to read.                                                                                                                            |
| `TextIO.canInput( [file] , [ch_num] )` | `(file,int) -> int option` | Returns the number of readable characters ahead.<br><br>If the number of readable character is more than the int specified it returns the integer.<br>( `return m<ch_num \|\| ch_num` ).<br><br>The option is `SOME` if it returns an int and `NONE` if there are no more chars to read. |

## Variables 
### Environments
An **Environment** is a set of pairs of <mark class="hltr-purple">identifiers</mark> and <mark class="hltr-orange">values</mark>. In other words it's a collection of associations between **names** and **denotable objects** that exist at <mark class="hltr-green">runtime</mark>.

It can be modified by **[[#Assignment|Val Declarations]]**.

An environment is defined by : 
- Visibility rules ( *block structure* ).
- Scope rules.
- Rules for the parameter passing.
- Binding policy.
#### Blocks
Blocks are a key concept for the environment organization.

A block is a **section of the program identified by opening and closing signs** that **contains declarations** local to that region.

>[!example]-
>ML : `let .. in .. end`
>C : `{ .. }`
>LISP : `( .. )`

The environment in a block can be subdivided in: 
- **Local environment** : Contains the associations *inside the block* :
	- Local variables
	- Formal parameters
- **Non-local environment** : Associations inherited from other blocks external to the current one.
- **Global environment** : Part of the non-local environment that contains associations common **to all blocks** o Explicit declarations of **global variables**.

In ML the block structure is denoted trough the keywords `let .. in .. end`.

##### Local environment declaration
To to this we use the structure `let .. in .. end` :

> [!example] 
> ```ml
> fun foo( bar ) = 
> let 
> 	val [first variable] = [first expression]; 
> 	val bar = bar + bar;
> 	 … 
> in 
> 	[func body] 
> end;
>```

#### Scope rules
The **scope** is subdivided in two types which specify the **visibility** ( *blocks* ) of the variables :

A **non-local** reference in a block can be resolved by :
- <mark class="hltr-orange">Static scoping</mark> : In the block that **syntactically includes the block** ( *Is written outside* ).
- <mark class="hltr-purple">Dynamic scoping</mark> : In the block that is **executed immediately before the block**.

> [!example]- 
> <mark class="hltr-orange">Static scoping</mark> :
> ```ml
> {
> 	int x=0; 
> 	void fie (int n){ 
> 		x = n+1; 
> 	} 
> 	fie(3); 
> 	write (x); 
> 	{
> 		int x =0; 
> 		fie(3); 
> 		write (x);
> 	} 
> 	write (x); 
> }
>```

%%TODO l4%%
TODO Non si capisce un cazzo dello scoping e non serve a una sega

### Declaration
Variables <mark class="hltr-red">CANNOT</mark> be **modified**, only **reassigned**.

```ml
val <name> = <value>;
val pi = 3.14

val <name> : <type> = <value>;
val pi : real = 3.14 

val <name> = <expression>;
```

## Exceptions
### Default Exceptions

| Exception               | Example                |
| ----------------------- | ---------------------- |
| Exception- Div raised   | `5 div 0;`             |
| Exception- Empty raised | `hd (nil: int list);`  |
| Exception- Empty raised | `tl (nil: real list);` |
| Exception- Chr raised   | `chr (500);`           |

### User defined Exceptions
#### Normal Exceptions

>[!info] Define an Exception
>```ml
> > exception Foo;
> > Foo;
> ----------------------
> > val it = Foo: exn
>```
>`exn` is the **Exception** type.


>[!info] Raise an Exception
>```ml
> > raise Foo;
> ----------------------
> > Exception- Foo raised
>```

#### Parametrized Exceptions

>[!info] Define a parametrized Exception
>```ml
> > exception Foo of string;
> > Foo;
> ----------------------
> > val it = fn: string -> exn
>```
>`exn` is the **Exception** type.


>[!info] Raise a parametrized Exception
>```ml
> > raise Foo ("bar");
> ----------------------
> > Exception- Foo "bar" raised
>```

### Exception handling
We have the exception ...
```ml
exception OutOfRange of int * int; 
```

... and the function `comb_helper` which raises the exception `OutOfRange`:
```
fun comb_helper(n,m)= 
	if n <= 0 then raise OutOfRange (n,m) 
	else if m<0 orelse m>n then raise OutOfRange (n,m) 
	else if m=0 orelse m=n then 1 
	else comb_helper (n-1,m) + comb_helper (n-1,m-1); 
```

>[!info] Handling the exception with another function
> The syntax to handle exceptions is `[expression] handle [match]` 
>```ml
>fun comb (n,m) = comb1 (n,m) handle 
>	OutOfRange (0,0) => 1 
>	| OutOfRange (n,m) => ( 
>		print ("out of range: n="); 
>		print (Int.toString(n)); 
>		print (" m="); 
>		print (Int.toString(m)); 
>		print ("\n"); 
>	0 
>	);
>```

%%TODO L7%%