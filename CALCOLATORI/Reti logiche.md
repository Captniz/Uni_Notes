---
Date created: 02-04-25 • 09:12
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250311_L5-RetiLogiche.pdf]]"
Related Pages:
---
## Valori logici
I computer sono realizzati tramite circuiti elettronici.

Trattandosi di elementi digitali avremo due livelli fondamentali: 
- **Alto** - **Asserito** `1`: associato alla tensione di alimentazione VDD 
- **Basso** - **Negato** `0`: associato alla massa

## Tipologie di reti logiche
Le reti logiche sono dei circuiti che trasformano alcuni valori logici in ingresso in altri valori logici in uscita.

Si hanno di due tipi:
- **Combinatorie**
	- Relazione funzionale tra I/O
	- Non hanno memoria
	- Output dipende SOLO dall' Input
- **Sequenziali**
	- Output dipende ANCHE dalla storia degli Input passati
	- Hanno memoria ( *stato* ) 

## Algebra di Boole e operatori logici
Una maniera compatta di specificare le funzioni logiche combinatorie è tramite **espressioni algebriche definite con l’algebra di Boole**.

### Operatori logici di base
#### AND
Simbolo : •

| Input 1 | Input 2 | Output |
| :-----: | :-----: | :----: |
|    `1`    |    `1`    |   `1`    |
|    `1`    |    `0`    |   `0`    |
|    `0`    |    `1`    |   `0`    |
|    `0`    |    `0`    |   `0`    |
#### OR
Simbolo : +

| Input 1 | Input 2 | Output |
| :-----: | :-----: | :----: |
|   `1`   |   `1`   |  `1`   |
|   `1`   |   `0`   |  `1`   |
|   `0`   |   `1`   |  `1`   |
|   `0`   |   `0`   |  `0`   |
#### NOT
Simbolo : $\overline{X}$

| Input 2 | Output |
| :-----: | :----: |
|   `1`   |  `0`   |
|   `0`   |  `1`   |

### Algebra di Boole
#### Regole fondamentali
L'algebra di Boole ha alcune fondamentali proprietà e regole:
- **Identità :** $(A+0),(A\cdot1=A)$
- **Regola "zero e uno" :** $(A+1=1),(A\cdot0=0)$
- **Regola dell'inversa :** $(A+\overline{A}=1)(A\cdot\overline{A}=0)$
- **Propietà commutativa :** $(A+B=B+A),(A\cdot{B}=B\cdot{A})$ 
- **Propietà associativa :** $(A+[B+C]=[A+B]+C),(A\cdot[B\cdot{C}]=[A\cdot{B}]\cdot{C})$
- **Propietà distributiva :** $(A\cdot[B+C]=[A\cdot{B}]+[A\cdot{C}])$

#### Regole di De Morgan
In più esistono due regole dette di **De Morgan**:

1. $$\overline{A\cdot{B}}=\overline{A}+\overline{B}$$
2. $$\overline{A+B}=\overline{A}\cdot\overline{B}$$

Queste due regole ci dicono che se abbiamo un elemento logico **NAND** ( *NOT AND* ), oppure **NOR** ( *NOT OR* ), **tutti gli altri operatori logici si possono ricavare da questo operatore**.

##### Esempio
>Algebra di Boole standard :
>$$E = (A · B · \overline{C}) + (A · C · \overline{B}) + (B · C · \overline{A})$$

>De Morgan : 
>$$E = \overline{(\overline{A} + \overline{B} + C) \cdot (\overline{A} + \overline{C} + B) \cdot (\overline{B} + C + A)}$$

%%PP 12 lez 5%%