---
Date created: 06-03-25 • 11:39
tags:
  - Funzionale
Related PDF/DOC:
  - "[[L2 - Expressions and Types.pdf]]"
Related Pages:
---
## Expressions & commands
### Expressions
> **Syntactic entities** whose evaluation either produces a **value** or does **not terminate** (`undefined expression`).
#### Composizione
An expression is usually formed either by:
- **A single entity** (constant, variable).
- **An operator applied to a number of arguments** (which are also expressions).

Three notational systems:
- **Infix**: a + b
- **Prefix** (Polish): + a b
- **Suffix** (reverse Polish): a b +
#### Valutazione
Two evaluation strategies: 
- **Eager evaluation**: first evaluating all the operands and then applying the operator to the values.
- **Lazy evaluation**: operands are evaluated only when needed.c
### Commands
> Syntactic entity whose evaluation does **not necessarily return a value** but can have a **side effect**.

The purpose of a command is the modification of the **state**.

#### Side effects
> Any **change in the system** state that occurs outside the immediate (local) context of the computation.

%%TODO: pp.24 lez.2%%