---
Date created: 03-06-25 • 15:03
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250505_L15-ToolChain.pdf]]"
Related Pages:
---
## Le basi della Toolchain
Anche se esiste l'[[Assembly|Assembly]], in genere non si programma direttamente con quest'ultimo, ma con linguaggi di programmazioni di **più alto livello**.
La conversione poi avviene attraverso un **compilatore**.

Gli step di creazione di un *eseguibile* partendo da un codice in C sono :
1. **Preprocessore** : Gestisce le direttive `#....` ( *Generalmente sostituzione del codice* ).
2. **Compilatore** : Da C ad Assembly ( *File* `.s` ).
3. **Assembler** : Da Assembly a linguaggio macchina ( *File* `.o` ).
4. **Linker** : Mette assieme codice in linguaggio macchina e librerie per generare un *eseguibile*.

>[!info]- Schema processo di compilazione
> ![[processo_compilazione.png]]

Normalmente, un *[[#^9f15fa|Driver]]* gestisce tutto questo in automatico.

Il file eseguibile può ora essere caricato in memoria con un’apposita system call ( *In Unix, la famiglia* `exec()` ).

>[!info]- Driver
>Nel processo di compilazione, il driver è un **programma di controllo** che **coordina le fasi del processo di compilazione**: preprocessing, compilazione, assemblaggio e linking.
>
>L'esempio più comune a noi è il comando `gcc`, che è un **driver**.

^9f15fa

### GCC - Gnu Compiler Collection
Suite di compilatori di vari linguaggi di alto livello e programmi di controllo ( *[[#^9f15fa|Driver]]* ), che può generare assembly par varie architetture di CPU.

Per la creazione di un eseguibile GCC usa vari passaggi ad opera di diversi programmi: `cpp`, `cc`, `as`, `ld` . Questi vengono invocati da GCC usando i giusti parametri.

Ovviamente GCC può anche invocare solo alcuni dei sotto-processi attraverso queste flag :
•`gcc -S` : Si ferma dopo aver invocato `cc` ( *Genera file assembly `.s`* ).
•`gcc -c` : Si ferma dopo aver invocato `as` ( *Genera file oggetto `.o`* ).

## Creazione di un eseguibile nel dettaglio
Vediamo ora i vari passaggi di cui si occupa GCC prendendo come esempio un programma C, `file.c` :

### Compilatore
Dato `file.c`, il comando `gcc -S` invoca `cc` per generare un
file Assembly `file.s` ( *Il flag opzionale `-o` specifica un nome per il file generato* ) :

```bash
gcc -S file.c [-o <nomefile>]
```

`cc` è il **Compilatore** propriamente detto e conosce l'architettura *target* meglio di qualunque programmatore umano.


> [!info]- Ottimizzazioni
> Il comando `gcc` permette i flag `-o<N>`, dove `N` specifica il livello di ottimizzazione con cui il compilatore deve operare.
> 
> Più alta l'**ottimizzazione**, <mark class="hltr-orange">più veloce il programma</mark> ma <mark class="hltr-purple">più lenta la compilazione</mark>.
> 
> Ogni livello `N` ha delle diverse ottimizzazioni abilitate e non sempre un livello maggiore è più adatto al programma.
> 
> Il `-o0` è il default senza ottimizzazioni.

### Assembler
Dato un `file.c` o `file.s`, `gcc -c` invoca `cc` e `as` o solo
`as` per generare un file oggetto `file.o` ( *Anche qui è applicabile `-o`* ) :

```sh
gcc -c file.{s|c} [-o <nomefile>]
```

`as` è l'**Assembler**.

L'Assembler fa spesso qualcosa in più rispetto alla semplice sostituzione di codici mnemonici con sequenze di bit :

- Converte le *[[#^fbbff4|pseudo-istruzioni]]* in istruzioni riconosciute dalla CPU.
- Converte numeri da decimale/esadecimale a binario.
- Gestisce le *label*.
- Genera meta-dati.
- Gestisce i salti.
	- Se la destinazione è troppo lontana, `j <DEST>` va convertita in `[caricamento di registro] + jr`.

>[!info]- Pseudo-istruzioni
>Le **pseudo-istruzioni** sono istruzioni *"finte"* che sembrano parte del linguaggio assembly, ma **non esistono realmente nell’architettura della CPU**. 
>
>Sono **alias** pensate per rendere la scrittura del codice assembly più facile e leggibile.
>
>Es :
>```c
> li a0, 10                ; Carica 10 in a0  
>```
>Viene tradotto in :
>```
>addi a0, x0, 10   
>```

^fbbff4

#### File oggetto
Il file generato dall'Assembler è un **File oggetto** che è composto da segmenti distinti :
1. **Header** : Specifica dimensione e posizione degli altri segmenti del file oggetto.
2. **Segmento di testo/Text segment** : Contiene il codice in linguaggio macchina.
3. **Segmento dati /Data segment** : Contiene tutti i dati ( *Sia statici che dinamici* ) allocati per la durata del programma.
4. **Tabella dei simboli/Symbol table** : 
	1. Associa simboli ad indirizzi ( *Relativi* ).
	2. Enumera simboli non definiti ( *Sono in altri moduli* ).
5. **Tabella di rilocazione/Relocation table** : Enumera istruzioni che fanno riferimento a istruzioni e dati che dipendono da indirizzi assoluti ( *Da “patchare”* ) nel momento in cui il programma viene caricato in memoria.
6. Altre info ...

### Linker
Dato un file `file.c` o `file.s` o `file.o`, `gcc` senza opzioni
`-S` e `-c` invoca anche il **linker** ( `ld` ) per generare un eseguibile ( *Senza `-o` il nome default del file creato è `a.<ext>`* ) :

```sh
gcc file.{s|c|o} [-o <nomefile>]
```

Su i sistemi Unix l'output è `a.out` mentre su Windows è `a.exe`.

`ld` è il **Linker** mette assieme uno o più [[#File oggetto|file oggetto]], eseguendo le necessarie *rilocazioni* :

- Decide come codice e dati sono disposti in memoria.
- Associa indirizzi assoluti a tutti i simboli.
- Risolve simboli che erano lasciati indefiniti in alcuni file `.o`.
- `“Patcha”` le istruzioni macchina citate nella tabella di rilocazione ( *In base agli indirizzi assegnati* ).

Lo scopo di `ld` è quindi di eliminare le <mark class="hltr-orange">tabelle dei simboli</mark> e <mark class="hltr-purple">tabelle di rilocazione</mark>, generando codice macchina con i giusti riferimenti.


Il Linker può gestire vari tipi di simboli :
- **Simboli definiti / Defined** : Associati ad un indirizzo ( *Relativo* ) nella tabella dei simboli.
- **Simboli non definiti / Undefined** : Usati in un file ( *e quindi presenti nella tabella dei simboli* ) ma <mark class="hltr-blue">definiti in un file diverso</mark>.
	- Se non vengono trovati negli altri file si ha un <mark class="hltr-red">Errore di linking</mark>.
- **Simboli locali**: Definiti ed usati in un file ( *Simili a simboli definiti* ), ma non usabili in altri file.

In tutti i casi, viene associato un **indirizzo assoluto** ad ogni simbolo.

>[!attention]- Riassunto della fase di linking
>Riassumendo, il Linker ...
>1. Dispone in memoria i vari segmenti ( *`.text`, `.data`, etc...* ) dei file `.o`.
>	- I segmenti testo uno dopo l’altro, *idem* per i segmenti dati, etc...
> 2. In base al passo precedente, assegna un indirizzo assoluto ad ogni simbolo contenuto nelle varie **tabelle dei simboli**.
> 3. In base alle **tabelle di rilocazione**, correggere le varie istruzioni con gli indirizzi calcolati.
> ---
>    
> Il file risultante viene poi *“incapsulato”* in un file eseguibile :
> - Segmenti ( *testo, dati, etc...*).
> - Informazioni per il caricamento in memoria ( *Indirizzo di caricamento dei segmenti, indirizzo entry point, etc.* ).
> - Altre informazioni...

#### Librerie
Esistono funzioni *“predefinite”* fornite dal compilatore / sistema, queste vengono definite in file `.o` e inclusi in **ogni** eseguibile prodotto dal Linker.

Data l'impraticità di linkare molti file `.o` si sono venute a creare le **Librerie**, cioè collezioni di file `.o`, linkabili tutte d'un colpo.

Esistono due tipi di librerie :
- **[[#Librerie statiche|Statiche]]** ( `.a` ) :  Semplici collezioni di file oggetto ( *Il Linker fa tutto il lavoro* ).
- **[[#Librerie dinamiche|Dinamiche]]** ( *.so* ) : Librerie linkate a tempo di esecuzione ( *Caricamento dinamico* ), sollevando il Linker dall'incarico.

##### Librerie statiche
Il linker inserisce nell’eseguibile **tutto** il codice della libreria utilizzato dal programma ( _La libreria non serve più, viene usata solo in questa fase : **Codice autocontenuto**_ ). 

Questo rende il <mark class="hltr-orange">caricamento del programma da parte del SO molto semplice</mark>, tuttavia le <mark class="hltr-purple">dimensioni dell’eseguibile aumentano</mark> notevolmente.

##### Librerie dinamiche
Il linker inserisce nell’eseguibile **riferimenti** alle librerie usate ed alle
funzioni invocate <mark class="hltr-red">ma non le include nell’eseguibile</mark>.

Ogni eseguibile creato in questo modo contiene un riferimento ad un **Linker dinamico** ( `/lib/ld-linux.so` ), che verrà caricato ed eseguito **all’esecuzione del programma**, passandogli il programma stesso come argomento.

Il Linker dinamico quindi caricherà l’eseguibile e le librerie ( `.so` ) da cui
dipende, e si occuperà di fare il linking al momento ( *La libreria serve anche in fase di esecuzione* ).

Rispetto alle [[#Librerie statiche|Librerie statiche]] si hanno i seguenti <mark class="hltr-green">pro</mark> e <mark class="hltr-orange">con</mark> : 
- <mark class="hltr-green">\+ Piccole dimensioni delle eseguibile</mark>.
- <mark class="hltr-green">\+ Possibile aggiornare le librerie senza ricompilare.</mark>
- <mark class="hltr-orange">\- Il programma non è auto-contenuto.</mark>
- <mark class="hltr-orange">\- Il Caricamento del programma da parte dell'SO è complesso.</mark>

###### Lazy linking
Un ulteriore complicazione del Linking dinamico è il **Lazy Linking**.

Invece di fare le operazioni di linking a **tempo di caricamento**, le si pospongono il più possibile, a **quando vengono chiamate effettivamente le funzioni**.  

Questo perchè se un eseguibile è linkato ad una libreria, ma non ne invoca mai i servizi <mark class="hltr-red">a runtime</mark>, forse si può evitare di linkarla.

Quando nel programma viene chiamata una funzione di una libreria, si chiama uno **stub** al suo posto. Questo stub esegue caricamento, rilocazione e linking sul momento.

La seconda volta che la procedura verrà chiamata, **il processo sarà più semplice**, poichè la procedura ora è già stata caricata.

![[lazy_linking.png| center]]

