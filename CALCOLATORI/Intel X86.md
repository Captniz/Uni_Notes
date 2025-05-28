---
Date created: 27-05-25 • 13:42
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250327_L11-INTELx86.pdf]]"
Related Pages:
---
## Descrizione
L'architettura **Intel X86** è di tipo *[[Assembly#^7b8b5b|CISC]]* ( *Gran numero di istruzioni e modalità di indirizzamento* ).

Inoltre questo assembly fornisce varie **modalità di funzionamento** per una migliore *compatibilità*; per questo esistono diversi [[Assembly#^2e347f|assembler]], ognuno con sintassi differente.

## Caratteristiche
### Registri
Intel X86 fornisce due tipi di registri, ***General Purpose*** e ***Specializzati***.
#### Registri General Purpose
Questi registi vengono preceduti dal prefisso `%`.

Sono 16 registri a <mark class="hltr-orange">64 bit</mark> e hanno nomi che riflettono gli strascichi di compatibilità con le versioni precedenti: 
- `%rax` / `%rbx` / `%rcx` / `%rdx`.
- `%rsi` / `%rdi`.
	- Derivano dai registri `%si` e `%di` della *CPU 8086* per la copia di array ( *`%si` → source index, `%di` → destination index* ).
- `%rbp` : Base Pointer ( *stack frame* ).
- `%rsp` : Stack Pointer.
- `%r8` ... `%r15`.

I registri a <mark class="hltr-orange">64 bit</mark> `%rax` .. `%rsp` estendono i registri a <mark class="hltr-purple">32 bit</mark> `%eax` .. `%esp`.  
I registri a <mark class="hltr-purple">32 bit</mark> `%eax` .. `%esp` estendono i registri a <mark class="hltr-blue">16 bit</mark> `%ax` ... `%sp`.  

I registri a <mark class="hltr-blue">16 bit</mark> `%ax`, `%bx`, `%cx`, `%dx` sono divisi in due registri a <mark class="hltr-green">8 bit</mark> indicati sostituendo `h` ( *per il primo registro* ) o `l` ( *per il secondo registro* ) alla `x`.

>[!example]- Esempio grafico
>![[intel_reg.png]]

Similmente i registri `r8` .. `r15` estendono seguendo questo schema:
- `r8` .... `r15`  <mark class="hltr-orange">64 bit</mark>
- `r8d` .. `r15d` <mark class="hltr-purple">32 bit</mark>
- `r8w` .. `r15w` <mark class="hltr-blue">16 bit</mark>
-  `r8b` .. `r15b` <mark class="hltr-green">8 bit</mark>
	- Solo 1 : **Byte meno significativo** ( *Il corrispettivo sarebbe* `l` ).


#### Registri Specializzati
I registri specializzati sono:
- `%rip` -> **[[Assembly#^99422c|Instruction Pointer]]** *visibile*
- `%rflags` -> Registro delle **[[#^d9f687|Flag]]**
	- `%rflags` <mark class="hltr-orange">64 bit</mark> -> `%eflags` <mark class="hltr-purple">32 bit</mark> -> `%flags` <mark class="hltr-blue">16 bit</mark>
	- Strutturato come set di flag, settati da istruzioni logico aritmetiche :
		- **`CF` : Carry Flag** → `1` se risultato è andato in *unsigned overflow* o se c’è *carry-out*.
		- **`ZF` : Zero Flag** → `1` se risultato è zero. 
		- **`SF` : Sign Flag** → `1` se risultato è negativo. 
		- **`OF` : 2’s Overflow Flag** →`1` se risultato è andato in *overflow*.
		- ...

> [!info]- Flag
> Valore *booleano* che funziona come un interruttore, segnalando un qualche tipo di evento. 

^d9f687

Il registro `%rflags`in particolare viene usato dalle [[Operazioni in RISC-V#Operazioni di controllo del flusso|istruzioni di salto condizionale]] o per controllare e contenere informazioni riguardo la CPU ( *Flag `IF`* ). 

### Convenzioni di chiamata
Queste *non fanno propriamente parte dell’architettura*: data una architettura, si possono usare molte diverse convenzioni di chiamata, infatti vengono specificate dall'**[[Assembly#ABI|ABI]]** non dall'**[[Assembly#ISA|ISA]]**.

Queste servono per **rendere compatibili diversi compilatori o librerie** ed altre parti del sistema operativo. 

In particolare specificano:
- Come/Dove passare i parametri ( *Stack o Registri* ).
- Quali registri preservare dopo una chiamata a una *sub-routine*. 

Nel caso dell'Intel X86 si ha che:
- **Primi 6 argomenti** di una funzione/sub-routine : 
	- `%rdi` / `%rsi` / `%rdx` / `%rcx` / `%r8` / `%r9`.
	- Nel caso ne siano necessari altri ( $7 \to n$ ) vengono messi nello stack.
- **Valori di ritorno** : `%rax` / `rdx`.
- **Registri preservati** : 
	- `%rbp` / `%rbx` / `%r12` / `%r13` / `%r14` / `%r15`.
- **Registri NON preservati** :
	- Ogni registro degli argomenti.
	- `%rax` / `%r10` / `%r11`.

### Modalità di indirizzamento
Le istruzioni in Intel X86 sono prevalentemente composte da **2 operandi**, di cui il secondo è assegnato come **destinazione implicita** ( *La destinazione del risultato dell'operazione è il secondo registro* ).

Questo comporta che, a differenza del <mark class="hltr-orange">RISC-V</mark>, non è possibile specificare una destinazione al di fuori dei due operandi dell'istruzione.

Il primo operando è detto <mark class="hltr-blue">Sorgente</mark> e può essere dei seguenti tipi :
- **Operando Immediato** ( *Una costante con `$` come prefisso* ). 
- **Operando in Registro** ( *Valore di un registro* ).
- **Operando in Memoria** ( *Valore in locazione di memoria determinato da un indirizzo. Es : `0x0100A8`* ).

Il secondo operando è detto <mark class="hltr-purple">Destinazione</mark> e può essere dei seguenti tipi :
- **Operando in Registro**.
- **Operando in Memoria**.

> [!attention]- Operazioni con destinazione in memoria
> A differenza di RISC-V si possono avere operazioni con **destinazione in memoria** diverse da `load` & `store`.
> 
> Unico vincolo è che <mark class="hltr-red">si può avere UN SINGOLO operando in memoria in un istruzione</mark>, mai 2 o più. 

#### Metodi di indirizzamento alla memoria
In Intel X86 si hanno due principali metodi di indirizzamento per gli operandi in memoria:
- <mark class="hltr-orange">Accesso Diretto</mark> : L'indirizzo in memoria viene dato come *"costante"*.

> [!example] Esempio
> ```c
mov eax, [0x00401000]
> ```

- <mark class="hltr-blue">Accesso Indiretto</mark> : L'indirizzo è ricavato da un registro e altri valori.

> [!example] Esempio
> ```c
mov eax, [8(ebx, esi, 4)]
> ```

La sintassi di quest'ultimo è : 
```c
<displacement> ( <base reg>, <index reg>, <scale> )
```

Di cui le componenti sono : 

| **Nome**         | *Tipo*                            | Descrizione / Appunti                                                    |
| ---------------- | --------------------------------- | ------------------------------------------------------------------------ |
| **Displacement** | *Costante a 8, 16 o 32 bit.*      | RISC-V permette offset/displacement solo a 12bit.                        |
| **Base**         | *Valore in registro*              | Uguale a RISC-V                                                          |
| **Indice**       | *Valore in registro*              | Semplifica iterazione su array                                           |
| **Scala**        | *Costante : <br>`[ 1, 2, 4, 8 ]`* | Semplifica accesso ad array con elementi di dimensione 1, 2, 4 o 8 byte. |
Possono venire omessi i campi **displacement**, **indice** e **scala** a seconda dell'utilizzo necessario.

L'indirizzo è poi calcolato con la formula:
$$displ+base+(indice*scala)$$


> [!example]- Tabella comprensiva delle possibilità di indirizzamento
> ![[indirizzamento_intel.png | center]]

### Istruzioni
La sintassi generale di un istruzione in Intel X86 è : 
```c
<opcode> <source>, <destination>
```

All'**opcode**, che definisce l'operazione da svolgere ( *Nome dell'istruzione* ), viene appeso un carattere che definisce la *"larghezza"* in bit degli operandi. 
In particolare uno tra questi :
- `b` : 8bit
- `w` : 16bit
- `l` : 32bit
- `q` : 64bit

Inoltre la sintassi dei vari elementi è la seguente :
- I nomi dei registri iniziano con `%`.
- Le costanti ( *Valori immediati* ) iniziano con `$`.
- Gli indirizzi diretti ( *Costanti* ) non hanno nessun carattere speciale iniziale, sono semplici numeri.

In Intel X86 le istruzioni vengono codificate con un **numero variabile di bit**. 
#### Istruzioni comuni

##### Aritmetiche
Le istruzioni aritmetico-logiche **modificano i flag** ( `carry`, `zero`, `sign`, ...).

| **Istruzione**          | Descrizione                           |
| ----------------------- | ------------------------------------- |
| `mov`                   | Copia dati da sorgente a destinazione |
| `movsx`                 | Copia dati con estensione del segno   |
| `movzx`                 | Copia dati con estesione con 0        |
| `add`                   | Somma                                 |
| `adc`                   | Somma con carry                       |
| `sub`                   | Sottrazione                           |
| `sbc`                   | Sottrazione con carry                 |
| `mul`                   | Moltiplicazione                       |
| `imul`                  | Moltiplicazione unisgned              |
| `div`                   | Divisione                             |
| `idiv`                  | Divisione unigned                     |
| `inc`                   | Incremento ( Somma 1 )                |
| `dec`                   | Decremento ( Sottrae 1 )              |
| `neg`                   | Negazione ( Complemento 2 )           |
| `rcl / rcr / rol / ror` | Diverse forme di rotazione            |
| `sal / sar / shl / shr` | Shift aritmetici e logici             |
| `and / or / xor / not`  | Operazioni booleane bit a bit         |


> [!info]- Istruzioni apparentemente inutili
> Operazioni come `inc / dec` risultano inutili all'apparenza, dato che possono essere sostituite da altre come `add $1 .. / sub $1 ..`.
> 
> L'efficacia di queste operazioni sta nella codifica: 
> 
> Dato che Intel usa una codifica delle istruzioni con un **numero variabile di bit**, operazioni come `inc / dec`, che richiedono solo un registro come parametro, ci risparmiano la codifica della costante `$1`.
> 
> Vale a dire <mark class="hltr-orange">risparmiare un minimo di 16 bit</mark>. 


##### Accesso alla memoria o ai dati

| **Istruzione**          | Descrizione                                   |
| ----------------------- | --------------------------------------------- |
| `nop`                   | Nessuna operazione<br>Spreca un giro di clock |
| `clc / stc / cld / cmc` | Modifica dei flag                             |
| `push`                  | Inserisce dati sullo stack                    |
| `pop`                   | Rimuove dati dallo stack                      |
| `lea`                   | [[#LEA\|Spiegazione]]                         |
###### LEA
L'istruzione **LEA** ( *Load Effective Address* ) è un'istruzione particolare nata per **calcolare un indirizzo indiretto in memoria**, senza fare accessi ad essa.

In particolare, opera con la stessa sintassi e usa lo <mark class="hltr-orange">stesso hardware</mark> necessario per calcolare un [[#Metodi di indirizzamento alla memoria|indirizzo indiretto]] in memoria; dopo averlo ottenuto tuttavia, lo memorizza sul secondo operando, senza far alcun accesso in memoria.

> [!example]- Esempio
> L'istruzione ...
> ```c
> lea 80(%rdx, %rcx, 2), %rax
>```
>... avrà come risultato :
> $$rax = rdx+(2*rcx)+80$$
> E nessun accesso in memoria.

Viene spesso utilizzata *"impropriamente"* come istruzione aritmetica che **effettua contemporaneamente due somme**, shiftando uno degli addendi ( *Un valore immediato e due registri* ) o per memorizzare una somma su un registro diverso dai due operandi. 

> [!example]- Esempio di uso improprio
> Vogliamo sommare due registri, salvando il risultato su un terzo.
> 
> Normalmente non sarebbe possibile in Intel X86 attraverso l'operazione di `add`...
>  
> Tuttavia in questo caso possiamo usare l'istruzione `lea`, considerando un accesso a memoria con base `%rbx` ed indice `%rcx` :
> ```c
> lea (%rbx, %rcx), %rax
>``` 


##### Comparazione
Le istruzioni di comparazione **settano i [[#^d9f687|flag]]**.

| **Istruzione** | Descrizione                         |
| -------------- | ----------------------------------- |
| `cmp`          | Comparazione attraverso sottrazione |
| `test`         | Comparazione attraverso `&`         |

##### Salto
La condizione di salto è stabilita **in base ai valori nel [[#Registri Specializzati|Flag Register]]** ( *jump if equal, jump if not zero, ...* ).

In generale la sintassi per i <mark class="hltr-purple">salti condizionati</mark> è `j<condition>`.

| **Istruzione**        | Descrizione          |
| --------------------- | -------------------- |
| `jmp`                 | Salto incondizionato |
| `je / jnz / jc / ...` | Salti condizionati   |

> [!example]- Tabella estesa dei salti condizionati
> ![[jumps_intel.png | center]]


> [!info]- Istruzioni condizionate
> 
Esistono anche istruzioni condizionate non di salto ( *conditional move, set on condition, ...* ).


##### Relative alle Procedure
Queste istruzioni sono la composizione delle istruzioni *"base"* necessarie per chiamare una sub-routine ( *Salvare la prossima istruzione, modificare PC, ...* ).

| **Istruzione** | Descrizione             |
| -------------- | ----------------------- |
| `call <label>` | Chiamata alla procedura |
| `ret`          | Ritorno dalla procedura |
