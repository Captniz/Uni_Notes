---
Date created: 28-05-25 • 17:35
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250328_L12-ARM.pdf]]"
Related Pages:
---
## Descrizione
L'architettura **ARM** nasce a 32 bit, ma recentemente si è sviluppata anche a 64 bit.

ARM è di tipo [[Assembly#^7b8b5b|RISC]] *"pragmatico"*, cioè prende come base l'architettura RISC e ci aggiunge le caratteristiche migliori dell'architettura CISC ( *In particolare di [[Intel X86]]* ).

Gli obbiettivi alla base del suo sviluppo sono :
- Trovare soluzioni ai **problemi delle architetture RISC**, come la scarsità delle modalità di indirizzamento.
- Massimo risparmio energetico.

> [!info]- ISA differenti
> In realtà ARM è una **famiglia di CPU**, con ISA leggermente differenti.

## Caratteristiche
### Registri
ARM fornisce 16 registri ***General Purpose*** e 2 ***Specializzati***, tutti di **32 bit**, l'uso di alcuni registri per funzioni specifiche/specializzate <mark class="hltr-red">è una linea guida più che un vincolo</mark>.

#### Registri General Purpose
Questi registri sono **16** e vengono nominati `r0` ... `r15`. 

> [!error] R13 / R14 / R15
> Anche se categorizziamo `r13` / `r14` / `r15` come *general purpose*, tecnicamente sono <mark class="hltr-red">Specializzati</mark>.
> 
> Questo in quanto : 
> - `r13` : Spesso usato come **Stack Pointer**.
> - `r14` : Spesso usato come **Link Register** ( *Indirizzo di ritorno da subroutine* ).
> - `r15` : Contiene il [[Assembly#^99422c|PC]] e alcuni flag ( *bit da `31` a `28`* ).
>   
>
> > [!warning] Particolarità di R15
> >  Pur ricoprendo tutti e tre un ruolo simile, lo scopo `r15` è l'<mark class="hltr-red">UNICO</mark> definito dall'[[Assembly#ISA|ISA]].
> >  
> >  Lo scopo degli altri due registri ( `r13` / `r14` ) viene definito invece dall'[[Assembly#ISA|ABI]]. Quindi **il loro scopo può variare**.
>
>   
> Questi tre registri sono accessibili tramite un *nome simbolico* che ne identifica l’utilizzo:
> 
> - `r13` :LiArrowRight: `sp` : Stack Pointer.
> - `r14` :LiArrowRight: `lr` : Link Register.
> - `r15` :LiArrowRight: `pc` : Program Counter.

#### Registri Specializzati
I due registri specializzati di ARM sono `apsr` e `cpsr`.

> [!info]- Numero dei registri specializzati
> Il numero, nomi e scopi dei registri specializzati varia a seconda della versione di ARM in uso.
> 
> Per semplicità studieremo questi due, essendo i più comuni.

Entrambi questi registri ricoprono il ruolo di **Flag Register**.
La differenza tra i due sta nel loro uso:


| CPSR                                                                                                                                    | APSR                                                                                                                                                                                                                                                                                                                          |
| --------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| È il **registro di stato principale**.<br><br>Contiene **flag di stato**, **informazioni di controllo**, e **modalità del processore**. | È una **vista parziale del CPSR**.<br>    <br>Contiene **solo i flag di stato** usati dall'applicazione:<br>    <br>    - **N (Negative)**<br>        <br>    - **Z (Zero)**<br>        <br>    - **C (Carry)**<br>        <br>    - **V (Overflow)**<br>        <br>    - (In alcune varianti anche **Q**, il saturate flag) |

##### Flag di comparazione
Le seguenti comparazioni e corrispettivi [[Intel X86#^d9f687|flag]] sono contenuti nel registro `apsr` e vengono aggiornati dalle operazioni di comparazione e aritmetiche :


| **Comparazione** | Descrizione                    | *Flag*                                             |
| ---------------- | ------------------------------ | -------------------------------------------------- |
| `eq`             | Equal                          | $Z=1$                                              |
| `ne`             | Not Equal                      | $Z=0$                                              |
| `hs`             | Higher Same / Carry Set        | $C=1$                                              |
| `lo`             | Lower / Carry Clear            | $C=0$                                              |
| `mi`             | Minus : *Risultato è negativo* | $N=1$                                              |
| `pl`             | Plus : *Risultato è positivo*  | $N=0$                                              |
| `vs`             | Overflow                       | $V=1$                                              |
| `vc`             | Overflow Clear                 | $V=0$                                              |
| `hi`             | Higher                         | $C=1,Z=0$                                          |
| `ls`             | Lower Same                     | $C=0,Z=1$                                          |
| `ge`             | Greater Equal                  | $(N=1 \land V=1) \lor (N=0 \land V=0)$             |
| `lt`             | Less Than                      | $(N=1 \land V=0) \lor (N=0 \land V=1)$             |
| `gt`             | Greater Than                   | $((N=1 \land V=1) \lor (N=0 \land V=0)) \land Z=0$ |
| `le`             | Less Equal                     | $((N=1 \land V=0) \lor (N=0 \land V=1)) \lor Z=1$  |

### Convenzioni di chiamata
Come abbiamo [[Intel X86#Convenzioni di chiamata|detto in precedenza]] **non fanno propriamente parte dell’architettura** e vengono definite dall'[[Assembly#ABI|ABI]]. 

> [!info]- ABI di ARM
> Per ARM, esistono **molte** ABI diverse ( <mark class="hltr-red">anche molto diverse!</mark> ).

Per ARM si ha che :
- **Primi 4 argomenti di una funzione/subroutine** : `r0` ... `r3`.
	- Questi non vengono preservati al ritorno.
	- Altri argomenti ( $4 \to n$ ) vengono memorizzati sullo stack.
- **Registri preservati al ritorno** : `r4` ... `r11`.
	- In alcune ABI `r9` non è preservato.
- **Valori di ritorno** : `r0` / `r1`.

### Modalità di indirizzamento
Le istruzioni in ARM sono prevalentemente composte da **3 argomenti**.

Il primo argomento è sempre **il registro di destinazione**, mentre gli altri due sono gli **operandi**.

Il tipo degli operandi è il seguente:
- <mark class="hltr-orange">Operando Sinistro</mark> : Registro.
- <mark class="hltr-purple">Operando Destro</mark> : Valore Immediato / Registro.

Gli operandi <mark class="hltr-red">NON</mark> possono essere in memoria.

#### Metodi di indirizzamento alla memoria
Le **uniche operazioni** che accedono alla memoria in ARM sono `load` e `store` e le loro varianti :
- `ldr` / `str` : Load/Store register.
- `ldm` / `stm` : Load/Store multiple.

L'indirizzamento alla memoria si ha in varie modalità:
- **Base** + **Offset**
- **Base** + **Indice**

| **Nome**                  | *Tipo*               | Descrizione / Appunti          |
| ------------------------- | -------------------- | ------------------------------ |
| **Base**                  | *Valore in registro* | Uguale a RISC-V                |
| **Offset**                | *Costante a 12 bit*  | Uguale a RISC-V                |
| **Indice** ( *Shiftato* ) | *Valore in registro* | Semplifica iterazione su array |
Inoltre in ARM si ha la possibilità di **aggiornare il registro base** <mark class="hltr-orange">prima dell’accesso</mark> ( *Pre-indexed* ) o <mark class="hltr-purple">dopo l’accesso</mark> ( *Post-indexed* ), questo semplifica l'implementazione dei cicli.

> [!warning]- Shift
> Lo shift dell'indice viene espresso attraverso una delle **operazioni di shift** con questa sintassi:
> ```c
> [r0, r1, LSL #2]
>```
>Dove :
>- `r0` : Registro Base
>- `r1` : Registro Indice
>- `LSL` : Operazione di Shift ( *Left Shift Logical* )
>- `#2` : Parametro di `LSL`
>
> Le operazioni di shift valide sono:
> - `LSL` : Left Shift Logical
> - `RSL` : Right Shift Logical
> - `ASR` : Arithmetic Shift Right
> - `ROR` : Rotate Right
> - ...

^bf9ab8

> [!warning]- Aggiornamento dell'array base
>
>
>In particolare il <mark class="hltr-orange">Pre-Indexing</mark> aggiorna il registro base attraverso l'offset o l'indice e **poi** fornisce l'indirizzo all'operazione di accesso alla memoria ...
>
>... invece il <mark class="hltr-purple">Post-Indexing</mark> **prima** fornisce l'indirizzo all'operazione di accesso alla memoria e solo dopo aggiorna il registro base.


> [!example]- Esempi
>
> > [!example] Caso Base : Accesso standard alla memoria
> > Base + Offset : 
> >```c
> >LDR r0, [r1, #1]
> >```
> > Base + Registro : 
> >```c
> >LDR r0, [r1, {+-}r2]
> >```
> > Base + Registro Shiftato : 
> >```c
> >LDR r0, [r1, {+-}r2, LSL #2]
> >```
>
> > [!example] Accesso con Pre-Indexing
> > Base + Offset : 
> >```c
> >LDR r0, [r1, #1]!
> >```
> > Base + Registro : 
> >```c
> >LDR r0, [r1, {+-}r2]!
> >```
> > Base + Offset : 
> >```c
> >LDR r0, [r1, {+-}r2, LSL #2]!
> >```
>
> > [!example] Accesso con Post-Indexing
> > Base + Offset : 
> >```c
> >LDR r0, [r1], #1
> >```
> > Base + Registro : 
> >```c
> >LDR r0, [r1], {+-}r2
> >```
> > Base + Offset : 
> >```c
> >LDR r0, [r1], {+-}r2, LSL #2
> >```

### Istruzioni

A discapito dell'architettura RISC, ARM fornisce **molte** istruzioni.

Tutte le istruzioni in ARM permettono l'esecuzione condizionale attraverso i [[#Flag di comparazione|suffissi delle operazioni di comparazione]].


> [!example]- Esempio
> ```c
> CMP     r0, r1       ; Confronta r0 e r1
MOVEQ   r2, #1       ; Se r0 == r1, allora r2 = 1
MOVNE   r2, #0       ; Se r0 != r1, allora r2 = 0
> ```


#### Sintassi
La maggior parte delle istruzioni in ARM ha come argomenti un <mark class="hltr-orange">registro di destinazione</mark> e due *operandi*.

Il <mark class="hltr-purple">primo</mark> di questi operandi è **sempre un registro**...
mentre il <mark class="hltr-blue">secondo</mark> può essere o un **valore immediato** o un **registro** ( *Anche **shiftato** con la [[#^bf9ab8|sintassi]] vista in precedenza*  ).

In ARM i registri vengono scritti senza alcun carattere speciale, mentre le costanti ( *Valore immediato* ) vengono scritti con `#` come prefisso.

#### Istruzioni aritmetiche
Le istruzioni aritmetiche in ARM possono avare il suffisso `s` ( *Dopo eventuali suffissi di comparazione* ). 
Lo scopo di questo suffisso è che, **se presente**, aggiorna i flag di [[#Registri Specializzati|CPSR]] con il risultato dell'operazione.


| **Istruzione** | Descrizione                                                                                                                                                                   |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `add` / `adc`  | Somma / Somma con carry<br>`r0 = r1 + r2` / `r0 = r1 + r2 + c`                                                                                                                |
| `sub` / `sbc`  | Sottrazione / Sottrazione con carry<br>`r0 = r1 - r2` / `r0 = r1 - r2 + c -1`                                                                                                 |
| `rsb` / `rsc`  | Reverse sub. / Reverse sub. con carry<br>`r0 = r2 - r1` / `r0 = r2 - r1 + c -1`<br><br>Lo scopo di queste istruzioni è permettere anche al primo operando di essere shiftato. |
| `mul` / `mla`  | Moltiplicazione / Moltiplicazione + Somma<br>`r0 = r1 ** r2` / `r0 = r1 ** r2 + r3`                                                                                           |
| `and`          | AND booleano bit a bit                                                                                                                                                        |
| `orr`          | OR booleano bit a bit                                                                                                                                                         |
| `eor`          | XOR booleano bit a bit                                                                                                                                                        |
| `bic`          | Bit Clear ( $r0 = r1\ and\ not\ r2$ )<br><br>Cioè, ogni 1 in r2 mette a 0 il bit corrispondente in r1.                                                                        |

> [!info]- Rotazione
> L'operazione di rotazione si può ottenere attraverso lo shift del secondo operatore della somma:
> ```c
> add r0, r1, r2, lsl #4 ; r0 = r1 + r2 << 4 
> add r0, r1, r2, lsl r4 ; r0 = r1 + r2 << r4
>```


> [!info]- Divisione
> Nella maggior parte di architetture ARM l'operazione di **Divisione** <mark class="hltr-red">NON</mark> viene fornita.
> 
> Quest'operazione si ottiene attraverso una funzione simile a questa:
> ```c
> int divide ( int A , int B) { 
> 	int Q = 0; 
> 	int R = A ; 
> 	while ( R >= B ) { 
> 		Q = Q + 1 ; 
> 		R = R − B ; 
> 	} 
> 	return Q;
>  }
>```


#### Istruzioni di movimento


| **Istruzione** | Descrizione                                                                                                                                    |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `mov`          | Sposta / Assegna valore a registro<br>`r0 = r1` / `r0 = 10`                                                                                    |
| `mvn`          | Sposta / Assegna il complemento 1 del valore a registro<br>`r0 = not r1`                                                                       |
| `b`            | Branch / Salto incondizionato ( *Permette le condizioni attraverso suffissi* )<br>`bne <label>`                                                |
| `bl`           | Branch and Link / Salta e salva l'indirizzo di ritorno nel **link register** `r14` ( *Usato per l'invocazione di subroutine* )<br>`bl <label>` |
| `bx`           | Salta a un indirizzo in un registro ( *Equivalente a mov r15, r* )<br>`bx r0`                                                                  |

#### Istruzioni di comparazione


| **Istruzione** | Descrizione                                                                              |
| -------------- | ---------------------------------------------------------------------------------------- |
| `cmp`          | Setta i flag di comparazione attraverso un operazione di **sottrazione**<br>`cmp r0, r1` |
| `cmn`          | Setta i flag di comparazione attraverso un operazione di **somma**<br>`cmn r0, r1`       |
| `tst`          | Setta i flag di comparazione attraverso un operazione di **and**<br>`tst r0, r1`         |
| `teq`          | Setta i flag di comparazione attraverso un operazione di **xor**<br>`teq r0, r1`         |

#### Istruzioni di accesso alla memoria

| **Istruzione** | Descrizione                                                                       |
| -------------- | --------------------------------------------------------------------------------- |
| `ldr`          | Load registro in memoria<br>[[#Metodi di indirizzamento alla memoria\|sintassi]]  |
| `str`          | Store registro in memoria<br>[[#Metodi di indirizzamento alla memoria\|sintassi]] |
| `ldm`          | Load multipli registri in memoria<br>`ldm r0, {r1,r2,r3}`                         |
| `stm`          | Store multipli registri in memoria<br>`stm r0, {r1,r2,r3}`                        |

> [!info]- Versioni per diverse lunghezze
> Nessun suffisso -> <mark class="hltr-orange">32 bit</mark> :
> - `ldr`
> - `str`
>   
>   
> ---
> 
> Suffisso `h` -> <mark class="hltr-purple">16 bit</mark> :
> - `ldrh`
>  - `strh`
> 
> ---
> 
> Suffisso `b` -> <mark class="hltr-blue">8 bit</mark> : 
> - `ldrb`
> - `strb`

##### Store/Load Multiple
Queste operazioni consentono di trasferire grandi quantità di dati in modo efficiente.

Vengono utilizzate nel *prologo* e nel *epilogo* di funzioni per salvare e ripristinare i **registri** e **l’indirizzo di ritorno**.

La sintassi di questa istruzione è :
```c
ldm<mode>[b] r [!], <register list>
stm<mode>[b] r , <register list>
```


| **Campo**       | Opzioni              | Descrizione                                                                                                                                                                                                                                                                                        |
| --------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `mode`          | `ia`                 | *Increment After*<br>Registri salvati/recuperati dalle locazioni di memoria `r`, `r+4`, `r+8`, ...                                                                                                                                                                                                 |
| ..              | `ib`                 | *Increment Before*<br>Registri salvati/recuperati dalle locazioni di memoria `r+4`, `r+8`, `r+12`, ...                                                                                                                                                                                             |
| ..              | `da`                 | *Decrement After*<br>Registri salvati/recuperati dalle locazioni di memoria `r`, `r-4`, `r-8`, ...                                                                                                                                                                                                 |
| ..              | `db`                 | *Decrement Before*<br>Registri salvati/recuperati dalle locazioni di memoria `r-4`, `r-8`, `r-12`, ...                                                                                                                                                                                             |
| `[!]`           | `!/''`               | Se specificato `!` aggiorna `r` ( *Registro base* ) al termine dell' operazione puntando **alla fine** del blocco usato.<br><br>Es.<br>```stm r0!, {r1,r2}```<br><br>- Salva `r1` in `[r0] = 0x1000`<br>    <br>- Salva `r2` in `[r0+4] = 0x1004`<br>    <br>- Poi aggiorna `r0 = r0 + 8 = 0x1008` |
| `register-list` | `{r1,r2,r3}/{r1-r3}` | Lista dei registri su cui operari                                                                                                                                                                                                                                                                  |

> [!info]- Alias per lo stack
> Le opzioni di `mode`, per essere più chiare quando usate quando si opera con lo stack, hanno degli `alias` :
> 
> 
| Suffisso “generico” | Suffisso “semantico” | Contesto tipico                |
| ------------------- | -------------------- | ------------------------------ |
| `STMDB`             | `STMFD`              | Push su stack full descending  |
| `LDMIA`             | `LDMFD`              | Pop da stack full descending   |
| `STMIA`             | `STMEA`              | Push su stack empty ascending  |
| `LDMDB`             | `LDMEA`              | Pop da stack empty ascending   |
| `STMDA`             | `STMED`              | Push su stack empty descending |
| `STMIB`             | `STMFA`              | Push su stack full ascending   |
>
> **`IA`, `IB`, `DA`, `DB`**: sono tecnici e indicano **direzione** e **quando** aggiornare l’indirizzo.    
> 
**`FD`, `FA`, `ED`, `EA`**: sono **mnemonici**, usati per indicare chiaramente il tipo di stack o struttura dati.

