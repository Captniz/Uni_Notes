---
Date created: 27-05-25 • 13:42
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250327_L11-INTELx86.pdf]]"
Related Pages:
---
## Descrizione
L'assembly **Intel X86** possiede un architettura di tipo *CISC* ( *gran numero di istruzioni e modalità di indirizzamento* ).

Inoltre questo assembly fornisce varie **modalità di funzionamento** per una migliore *compatibilità*; per questo esistono diversi [[Assembly#^2e347f|assembler]], ognuno con sintassi differente.

## Caratteristiche
### Registri
Intel X86 fornisce due tipi di registri, ***General Purpose*** e ***Specializzati***.
#### Registri General Purpose
Questi registi vengono preceduti dal prefisso `%`.

Sono 16 registri a <mark class="hltr-orange">64 bit</mark> e hanno nomi che riflettono gli strascichi di compatibilità con le versioni precedenti: 
- `%rax`, `%rbx`, `%rcx`, `%rdx`.
- `%rsi`, `%rdi` -> Derivano dai registri `%si` e `%di` della *CPU 8086* per la copia di array ( *`%si` → source index, `%di` → destination index* ).
- `%rbp` -> Base Pointer ( *stack frame* ).
- `%rsp` -> Stack Pointer.
- `%r8` ... `%r15`.

I registri a <mark class="hltr-orange">64 bit</mark> `%rax` .. `%rsp` estendono i registri a <mark class="hltr-purple">32 bit</mark> `%eax` .. `%esp`.
... I registri a <mark class="hltr-purple">32 bit</mark> `%eax` .. `%esp` estendono i registri a <mark class="hltr-blue">16 bit</mark> `%ax` ... `%sp`.
... I registri a <mark class="hltr-blue">16 bit</mark> `%ax`, `%bx`, `%cx`, `%dx` sono divisi in due registri a <mark class="hltr-green">8 bit</mark> indicati sostituendo `h` ( *per il primo registro* ) o `l` ( *per il secondo registro* ) alla `x`.

>[!example]- Esempio grafico
>![[intel_reg.png]]

Similmente i registri `r8` .. `r15` estendono seguendo questo schema:
- `r8` .. `r15` <mark class="hltr-orange">64 bit</mark>
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
	- `%rdi`, `%rsi`, `%rdx`, `%rcx`, `%r8` ed `%r9`.
	- Nel caso ne siano necessari altri ( $7 \to n$ ) vengono messi nello stack.
- **Valori di ritorno** : `%rax`, `rdx`.
- **Registri preservati** : 
	- `%rbp`, `%rbx`, `%r12`, `%r13`, `%r14` ed `%r15`
- **Registri NON preservati** :
	- Ogni registro degli argomenti.
	- `%rax`, `%r10`, `%r11`.

### Modalità di indirizzamento
%%TODO p9 L11%%