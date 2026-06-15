---
Date created: 07-05-26 • 11:05
tags:
  - Sistemi-Operativi
Related PDF/DOC:
  - "[[06-Processi e Thread.pdf]]"
Related Pages:
---
## I processi
>  Istanza di programma in esecuzione.

Un processo esegue le proprie istruzioni in maniera *sequenziale*...

tuttavia  in un sistema multiprogrammato i processi evolvono in modo <mark class="hltr-orange">concorrente</mark>.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


In un ambiente multiprogrammato, a causa delle limitate risorse fisiche e logiche, la gestione di quest'ultime è fondamentale.

### Immagine di un processo in memoria
> I dati in memoria di un processo vengono detti **immagine**.


Un processo in memoria consiste di:
- Istruzioni (*sec. Codice o Testo*) : Il codice. 
- Dati (*sec. Dati*) : <mark class="hltr-red">SOLO VARIABILI GLOBALI</mark>.
- Stack : Chiamate a procedure e parametri & variabili locali *statiche*.
- Heap : Memoria allocata *dinamica*.
- Attributi : (*id, stato, controllo*).



> [!example] Schema di immagine di processo
> ![[EMBED/06-Processi e Thread.png]]
>
[[06-Processi e Thread.pdf#page=6&rect=68,53,658,415|06-Processi e Thread, p.6]]

#### PCB - Process Control Block - Attributi di un processo
>All’interno del S.O. ogni processo è rappresentato dal **process control block**.

Il PCB contiene i seguenti dati : 

- Stato del processo
- PC - Program Counter
- Valori dei registri
- Informazioni sulla memoria (*eg. registri limite, tabella pagine*)
- Informazioni sullo stato dell’I/O (*eg. richieste pendenti, file*)
- Informazioni sull’utilizzo del sistema (*CPU*)
- Informazioni di scheduling (*eg. priorità*)


<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Stati di un processo
> Durante la sua esecuzione, un processo evolve attraverso diversi stati.

Gli stati si susseguono in linea teorica e sono :
- NEW
- READY
- RUNNING
- WAITING
- FINISHED

Tra un processo può tornare indietro solo tra `running` e `ready` tramite un meccanismo chiamato **_prelazione_** ( *il contrario di  dispatch : `ready` $\to$ `running`*).


> [!example] Schema completo degli stati di un processo
> ![[EMBED/06-Processi e Thread 1.png]]
>
[[06-Processi e Thread.pdf#page=11&rect=56,51,667,367|06-Processi e Thread, p.11]]

---
## Operazioni sui processi
### Processi figli
> Un processo può creare un *figlio*.

Un processo figlio ottiene risorse dal SO o dal padre (*spartizione, condivisione*).

Il padre rispetto ai figli può continuare la sua esecuzioni in diversi modi :
- <mark class="hltr-purple">Sincrona</mark> : Padre attende la terminazione dei figli.
- <mark class="hltr-blue">Asincrona</mark> : Evoluzione *“parallela”* (*concorrente*) di padre e figlio.


> [!example]- Esempi di sys-call per la creazione di un figlio in UNIX
> UNIX utilizza queste sys-call per i processi figli :
> - `fork` : Crea un figlio che è un duplicato esatto del padre.
> - `exec` : Carica sul figlio un programma diverso da quello del padre.
> - `wait` : Per esecuzione sincrona tra padre e figlio.
>   
>   ---
>   
>
> > [!example] Codice per la creazione di figli in UNIX
> >   ![[EMBED/06-Processi e Thread 4.png]]
> > [[06-Processi e Thread.pdf#page=25&rect=32,53,703,439|06-Processi e Thread, p.25]]


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Terminazione di un processo

Ci sono diverse condizioni per cui un processo viene terminato : 

- Il processo **finisce l'esecuzione**.
- Il processo viene **terminato forzatamente** dal padre.
	-  Per eccessivo uso di risorse.
	- Il compito richiesto al figlio non è più necessario.
	- Il padre termina e il SO non permette ai figli di sopravvivere al padre.
-  Il processo viene terminato forzatamente dal SO.
	- L'Utente chiude l'applicazione.
	- Errori (*Eg. aritmetici, di protezione, di memoria, …*).

---

## Gestione dei processi del SO
> Il SO è un programma a tutti gli effetti, come viene gestita la sua esecuzione quindi?

Per la gestione dei processi del SO si hanno diverse opzioni :
- Kernel eseguito **separatamente**.
- Kernel eseguito all’interno di un **processo utente**.
- Kernel eseguito come **processo**.

### Kernel separato
> Kernel esegue *“al di fuori”* di ogni processo.

Con questo modello ...
- Il SO possiede uno spazio riservato in memoria.
- Il SO prende il controllo del sistema. %%wtf ???????????????%%
- Il SO è sempre in esecuzione in modo privilegiato.

Queste caratteristiche implicano che il  concetto di processo, e le sue caratteristiche, vengano applicate solo a processi utente.

Questo modello è tipico dei primi SO.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Kernel in processi utente
> I servizi del SO sono procedure chiamabili da programmi utente

Questi servizi sono accessibili solo in modalità protetta (*kernel mode*).

Con questo modello si altera leggermente l'immagine dei processi; vengono aggiunte due sezioni :
- <mark class="hltr-orange">Kernel stack</mark> : gestisce il funzionamento di un processo in modalità protetta (*chiamate a funzioni del SO*).
- <mark class="hltr-purple">Spazio di indirizzamento condiviso</mark> : Codice/dati del SO condivisi tra i processi.


> [!example] Schema dell'immagine di un processo con dati del SO
> ![[EMBED/06-Processi e Thread 5.png]]
>
[[06-Processi e Thread.pdf#page=31&rect=557,67,700,361|06-Processi e Thread, p.31]]


Questo modello ha due vantaggi principali :
- In occasione di interrupt o trap durante l’esecuzione serve solo effettuare un *mode switch*
- Dopo il completamento del suo lavoro, il SO può decidere di riattivare lo stesso processo utente (*mode switch*) o un altro (*context switch*).


> [!warning]- Mode switch
> Il sistema passa da user mode a kernel mode e viene eseguita la parte di codice relativa al SO senza *context switch*.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Kernel come processo
> I servizi del SO sono processi individuali.

I diversi processi del SO vengono eseguiti in modalità protetta.

Una minima parte del SO <mark class="hltr-red">deve comunque eseguire al di fuori</mark> di tutti i processi : lo **scheduler**.

Questo modello risulta vantaggioso per sistemi multiprocessore, dove i processi del SO possono essere eseguiti su processore separato.

---

## Threads
> Un thread è l'unità minima di utilizzo della CPU di un processo.

Un processo unisce due concetti ...
- Il possesso delle risorse (*Eg. spazio di memoria, file, IO, …*).
- L’utilizzo della CPU, cioè l'esecuzione (*Eg. priorità, stato, registri, …* ).

Il **thread** è l'utilizzo della CPU, mentre il processo è il possesso delle risorse. 

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Ad un processo vengono associati :
- Spazio di indirizzamento
- Risorse del sistema

Invece, ad un singolo thread sono associati : 
- Stato di esecuzione 
- PC (*program counter*) 
- Insieme di registri 
- Stack

Le thread condividono tra loro :
- Spazio di indirizzamento 
- Risorse e stato del processo

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Un thread ha vari attributi, ad esempio ...
- La sua **priorità** (*influenza la frequenza con cui verrà schedulato*).
- La **dimensione del suo stack** (*che specifica la quantità massima di argomenti che gli si possono passare, la profondità delle chiamate ricorsive, ...*).

### Multi-Threading
> Possibilità di supportare più thread per un singolo processo.

In un SO classico, un processo ha solo un thread, ma con il multi-threading questo cambia.

Questa tecnica implica che <mark class="hltr-orange">più flussi di operazioni vengano associati ad un singolo spazio di indirizzamento</mark>.

I vantaggi, invece sono :
- **Riduzione tempo di risposta** : Un thread può continuare a operare se gli altri sono bloccati (*operazioni lunghe o accessi IO*).
- **Condivisione delle risorse** : I thread di un processo condividono la memoria senza dover introdurre tecniche di condivisione; a differenza di processi diversi (*sincronizzazione o comunicazione*).
- **Maggior velocità** : La gestione di thread e contex switch tra essi è più veloce che non tra processi.
- **Scalabilità** :  Il multithreading aumenta il *parallelismo* se l’esecuzione avviene su <mark class="hltr-purple">multiprocessore</mark> (*un thread per processore*).

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Modelli di thread
> Gli stati di un thread sono simili a quelli di un processo, tuttavia lo stato del processo può, *non coincidere con lo stato della thread*.

I thread creano problemi nel percorso di esecuzione di un processo; per esempio : *"un thread in attesa deve bloccare l’intero processo?"*

Dipende dall'implementazione, che può essere di diversi tipi ...


> [!example] Schema delle implementazioni dei thread
> ![[EMBED/06-Processi e Thread 6.png]]
>
[[06-Processi e Thread.pdf#page=45&rect=51,87,689,398|06-Processi e Thread, p.45]]


#### User Level Thread
> Negli <mark class="hltr-orange">User level Thread</mark> la gestione è affidata alle applicazioni.

In questo modello il kernel ignora l’esistenza delle thread. Le funzionalità di threading sono disponibili tramite una libreria di programmazione.

I vantaggi di questo modello sono :
- **Più efficienti** : Non è necessario passare in modalità kernel per utilizzare thread, quindi previene due mode switch.
- Meccanismo di scheduling variabile da applicazione ad applicazione
- **Portabilità** : girano su qualunque SO senza bisogno di modificare il kernel.

Mentre gli svantaggi sono :
- Il blocco di una thread <mark class="hltr-orange">blocca l’intero processo</mark> (*Ovviabile con accorgimenti*).
- Non è possibile sfruttare un multiprocessore : Lo scheduling di un thread avviene sempre sullo stesso processore, quindi è ammesso un solo thread in esecuzione per ogni processo.


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Kernel Level Thread
> Nei <mark class="hltr-purple">Kernel level Thread</mark>, invece la gestione è affidata al kernel.

In questo modello le applicazioni usano i thread tramite sys-call.

I vantaggi sono ...
- Scheduling a livello di thread, quindi <mark class="hltr-orange">il blocco di un thread NON blocca il processo</mark>.
- Più thread dello stesso processo possono essere in esecuzione contemporanea su CPU diverse (*o in esecuzione concorrente con una CPU*).
- Le funzioni del SO stesso possono essere multithreaded.

Gli svantaggi sono essenzialmente gli opposti dei vantaggi degli user-level thread :
 - **Scarsa efficienza** : il passaggio tra thread implica un passaggio attraverso il kernel.
- **Non portabilità**

---

## Esempio di thread in POSIX

[[06-Processi e Thread.pdf#page=54|06-Processi e Thread, p.54 - 69]]