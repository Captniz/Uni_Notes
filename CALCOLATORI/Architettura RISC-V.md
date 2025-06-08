---
Date created: 13-04-25 • 22:29
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250319_L7-Operazioni_Assembly.pdf]]"
Related Pages:
  - "[[Procedure in RISC-V]]"
  - "[[Operazioni in RISC-V]]"
---
## Principi di progettazione del RISC-V
Vedremo la struttura/progettazione delle istruzioni che portano alla creazione di un programma in assembly con **IS** ( *Instruction Set, legato ovviamente all'[[Assembly#ISA|ISA]]* ) **RISC-V**.

Il RISC-V è l'IS adottata da **Intel** ed è un esempio di architettura **RISC** pragmatica ( *vicina al CISC* ) e open-source. 

Una volta ultimato il programma verrà caricato in **RAM** per essere eseguito.

### Principio 1 - La semplicità favorisce la regolarità  
#### Istruzioni aritmetiche
Il modo più semplice di immaginare una istruzione aritmetica è a tre operandi:
$$A = B + C$$

Proprio per questo RISC-V prevede <mark class="hltr-red">SOLO</mark> operazioni **aritmetiche** a tre operandi:

```asm
add a, b, c
```

Se si necessita un istruzione **più complessa** la si può ottenere dalla **combinazione** di più operazioni più semplici.

>[!example]- Esempio 1
$$a=b+c\ ,\ d=a-e$$
>```asm
add a, b, c
sub d, a ,e
>```


>[!example]- Esempio 2
$$a=b+c+d+e$$
>```asm
add a, b, c
add a, a ,d
>add a, a, e
>```

#### Operandi
In RISC-V gli operandi di operazioni aritmetiche sono <mark class="hltr-red">vincolati</mark> ad essere contenuti **nei [[Reti logiche#Registri|registri]]**.

>[!important]- Registri in RISC-V
> In RISC-V si hanno 32 registri a **64 bit**, anche detti a *parola doppia*.
> 
> $32$ bit :LiArrowRight: *Word*
> $64$ bit :LiArrowRight: *Double Word* 

Il vincolo di operare solo tra registri **semplifica di molto** la progettazione dell’hardware.

Esistono delle <mark class="hltr-orange">eccezioni</mark> tuttavia: essere vincolari a caricare in un registro un dato, anche se questo è una **costante** è molto **inefficiente**.

Dovendo svolgere un operazione come:
```cpp
f = f + 4
```

Dovremmo caricare `4` in un registro; per rendere **più veloci** queste operazioni si hanno **istruzioni apposite**:
```asm
addi x22, x22, 4
```
^immediate

Come questa `addi` ( *Add-Immediate* ).
Queste operazioni rendono anche possibile alcune operazioni logiche come <mark class="hltr-green">spostare un registro in un altro</mark>, che si traduce nel memorizzare nel registro di destinazioni la **somma con 0** dell'altro registro. ^e2e611

Per questo RISC-V dedica un registro alla costante **0** : **`x0`**.

### Principio 2 - Minori dimensioni, maggiore velocità
#### Dati memorizzati
##### Registri
Avere **molti registri** obbligherebbe i segnali a *«spostarsi»* su distanze più lunghe all’interno del processore.

Quindi per effettuare uno spostamento all’interno di un ciclo di clock saremmo **costretti a rallentare il clock**.

A questo punto però lo spazio non basta, necessitiamo quindi di **istruzioni di trasferimento**:
- `load` : Preleva i dati da locazioni di memoria, e li carica nei registri.
- `store` : Salva il contenuto dei registri in memoria.

##### Memoria
La memoria è una **sequenza di byte** ( *Ogni indirizzo corrisponde a 1 byte* ). 
Per quanto sia possibile accedere a ogni byte spesso si preferisce utilizzare **multipli di $4$ o $8$ byte** ( *Word /  Double Word * ).

Per caricare un dato dalla memoria ai registri è necessario specificarne **l’indirizzo**. In RISC-V l’indirizzo si specifica tramite una <mark class="hltr-green">base</mark> ( *in un registro* ) e uno <mark class="hltr-purple">spiazzamento</mark> ( *costante* ).

Quindi per caricare dalla memoria scriveremo:
```asm
ld x9, 8 (x22)
```
- `x9` : Registro di destinazione.
- `8` : Offset/spiazzamento ( *Lunghezza della double-word* ). 
- `x22` : Registro contenente l'indirizzo di memoria di base.

> [!example]- Esempio di programma con accesso alla memoria
> Vogliamo tradurre questa istruzione ( *C* ):
> ```c
> A[12] = h + A[8]
>```
>  In RISC-V sarà:
 > ```asm
 > ld x9, 64(x22)
 > 
 > add x9, x21, x9
 > 
 > sd x9, 96(x22)
>``` 
>- `x22` : Registro contenente l'indirizzo di `A`.
>- `x21` : Registro contenente `h`.
>  - `64` : Offset di `A[8]` ( *Ottava Double-word :LiArrowRight: $8\cdot{8}$* ). 
>- `96` : Offset di `A[12]` ( *... :LiArrowRight: $8\cdot{12}$* ).

> [!important]- Uso efficente della memoria ( *Register spilling* ) 
Solitamente i programmi contengono più variabili da memorizzare che registri.
>
La soluzione a questo problema è <mark class="hltr-blue">caricare le variabili utili</mark> in un dato momento nei registri e si <mark class="hltr-orange">scaricare quelle che non si usano più</mark>.
>
 Questa operazione è chiamata **Register Spilling** ed è eseguita dal [[Assembly#^2e347f|compilatore]], che stima il *working set* ( *Insieme delle variabili in uso* ) ed inserisce nel codice  le operazioni di `load`/`store` appropriate.

#### Codifica delle istruzioni
Per codificare le istruzioni è importante tenere a mente le [[Aritmetica dei calcolatori#Base del trattamento dei dati su un calcolatore|basi della codifica]] all'interno di un calcolatore.

Inoltre è importante sapere come leggere un **codice esadecimale** correttamente: nel dato `0xEA01BD1c`, `EA` è il <mark class="hltr-green">bit più significativo</mark> mentre `1C` il <mark class="hltr-orange">meno significativo</mark>.


>[!info]- Little-Endian & Big-Endian
>Questi due termini servono a descrivere la modalità di **codifica/lettura/scrittura** di un dato binario o esadecimale.
>
> In particolare si riferiscono al valore del **Bit finale** ( *Endian* ). Prendendo come esempio `EA01BD1C` si ha che ...
>
In <mark class="hltr-blue">Big-Endian</mark> :
>
| ADDR | DATA |
| :--: | :--: |
|  3   | `1C` |
|  2   | `BD` |
|  1   | `01` |
|  0   | `EA` |
>
>  Invece in <mark class="hltr-purple">Little-Endian</mark>:
>   
| ADDR | DATA |
| :--: | :--: |
|  3   | `EA` |
|  2   | `01` |
|  1   | `BD` |
|  0   | `1C` |
>
>   RISC-V utilizza **Little-Endian.**

Come i dati, anche le istruzioni devono essere memorizzate in formato binario  (*codice macchina* ).

Per rappresentare un istruzione tramite un codice numerico **univoco** occorre:
- Un codice che ci dica che ci dia il <mark class="hltr-blue">tipo dell'istruzione</mark>.
- $n$ codici che ci dicano gli <mark class="hltr-purple">operandi</mark>.

In RISC-V, ogni istruzione viene codificata in una **word** ( *32 bit* ), secondo questo schema:


| FUNCT 7 | REG SOURCE 2 | REG SOURCE 1 | FUNCT 3 | REG DESTINATION | CODOP |
| ------- | ------------ | ------------ | ------- | --------------- | ----- |
| 7 bit   | 5 bit        | 5 bit        | 3 bit   | 5 bit           | 7 bit |

- **CODOP** : Codice operativo dell'istruzione.
- **FUNCT {3/7}** : Codici operativi aggiuntivi.
- **REG SOURCE {1/2}** : Registro contenente gli operandi sorgente.
- **REG DESTINATION** : Registro destinatario del risultato.

I registri essendo **32** ( *0  ... 31* ) possono essere codificati con **5 bit** ( *$2^5=32$ Combinazioni* ).

>[!example]- Esempio di codifica
> Codifichiamo l'istruzione ...
> ```
> add x9, x20, x21
> ```
> Come:
> 
| FUNCT 7 | RS 2 | RS 1 | FUNCT 3 | RD | CODOP |
| ----------- | ---------------- | ---------------- | ----------- | ----------- | ------------ |
| 0000000      | 10101 | 10100 | 000       | 01001       | 0110011        |
> | 0 | 21 | 20 | 0 | 9 | 51 |
> 

Come  codifichiamo quindi le operazioni **immediate** ? 
Per rispondere passeremo al [[#Principio 3 - Un buon progetto richiede compromessi|prossimo principio]] di progettazione del RISC... 
### Principio 3 - Un buon progetto richiede compromessi
#### Codifica delle istruzioni in 32 bit
La [[#Codifica delle istruzioni|codifica delle istruzioni]] con 32 bit comporta varie limitazioni...
- *Poche* istruzioni.
- *Pochi* registri.
- *Poche* modalità di indirizzamento.

... tuttavia comporta un grande **guadagno in efficienza**.

Inoltre come abbiamo visto per ovviare alla codifica forzata degli operatori come registri ( _formato **R**_ ) esistono **[[#^immediate|operazioni  immediate]]** ( _formato **I**_ ) utilizzate nei casi di indirizzamento immediato e di istruzioni con costanti.

Queste istruzioni immediate vengono codificate in questo modo:

| COSTANT | RS 1  | FUNCT 3 | RD    | CODOP |
| ------- | ----- | ------- | ----- | ----- |
| 12 bit  | 5 bit | 3 bit   | 5 bit | 7 bit |

>[!example]- Esempio di codifica
> Codifichiamo l'istruzione ...
> ```
> add x9, 64(x22)
> ```
> Come:
> 
| COSTANT | RS 1 | FUNCT 3 | RD | CODOP |
| ------------- | ---------------- | ----------- | ----------- | ------------ |
| 0000 01000 0000      | 10110 | 001 | 000       | 01001       | 0000011        |
> | 64 |  22 | 3 | 9 | 3 |
> 

