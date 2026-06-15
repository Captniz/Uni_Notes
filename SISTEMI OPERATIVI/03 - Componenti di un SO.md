---
Date created: 23-04-26 • 10:00
tags:
  - Sistemi-Operativi
Related PDF/DOC:
  - "[[03-Componenti.pdf]]"
Related Pages:
---
## Compiti di un SO

Un SO esegue diversi compiti all'interno di un calcolatore:
- Gestione dei processi 
- Gestione della memoria primaria
- Gestione della memoria secondaria 
- Gestione dell’I/O 
- Gestione dei file 
- Protezione 
- Rete 
- Interprete dei comandi
- ...


> [!info]- Compiti di un SO in Sistemi distribuiti
> > Un Sistema distribuito è una collezione di elementi di calcolo che non condividono né la memoria né un clock connessi da una rete.
> 
>  Il SO in questo caso è responsabile della gestione *in rete* delle varie componenti: 
>  - Processi distribuiti. 
>  - Memoria distribuita. 
>  - File system distribuito


### Gestione dei processi
> Un processo è definito come un *programma in esecuzione*.

Un processo necessità oltre all'esecuzione sequenziale delle proprie istruzioni di **risorse** ( *File, Dati, ...* ).

Inoltre esistono processi di due tipi, differenziati dal loro livello di permessi, detti <mark class="hltr-orange">processi del SO</mark> e <mark class="hltr-purple">processi utente</mark>.



Il sistema operativo, per quanto riguarda i processi gestisce...
- **Creazione e distruzione** di processi. 
- **Sospensione e riesumazione** di processi.
- Fornitura di canali per la **sincronizzazione e la comunicazione** tra processi.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Gestione della memoria primaria
>La memoria primaria conserva dati condivisi dalla CPU e dai dispositivi di I/O.

Un programma deve essere caricato in memoria per poter essere eseguito.

Riguardo la memoria primaria il SO è responsabile di...
- **Gestione dello spazio** di memoria (*quali parti e da chi sono usate*).
- **Decisione sul processo da caricare** quando esiste spazio disponibile.
- **Allocazione e rilascio** dello spazio di memoria.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Gestione della memoria secondaria
>Al contrario della memoria primaria, la memoria secondaria memorizza grandi quantità di dati in modo permanente.

L' SO è responsabile di...
- **Gestire lo spazio** libero su disco.
- **Allocare lo spazio** su disco.
- **Scheduling degli accessi** su disco.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Gestione dell I/O
> Uno dei compiti principali dell'SO è l'astrazione delle specifiche caratteristiche dei dispositivi di I/O.

L' SO mette a disposizione...
- Un sistema per accumulare gli accessi ai dispositivi (*buffering*).
- Una generica interfaccia verso le periferiche (*driver*).
- Driver specifici per alcuni dispositivi.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Gestione dei file
> Un *File* è un astrazione logica, di una raccolta di informazioni correlate, con lo scopo rendere conveniente l’uso della memoria non volatile.

I dati possono essere su supporti fisici diversi (*dischi, USB, …*) controllati da driver con caratteristiche diverse; il compito dell'SO è di standardizzarne le operazioni.

Il SO è responsabile della...
- **Creazione e cancellazione di file e directory**.
- **Supporto di primitive per la gestione** di file e directory (*copia, sposta, modifica, …*).
- **Corrispondenza tra file e spazio fisico** su disco.
- **Salvataggio delle informazioni** a scopo di backup.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Protezione degli accessi
> Meccanismo per controllare l’accesso alle risorse da parte di utenti e processi.

Il SO ha il compito di...
- **Definire accessi autorizzati** e non.
- **Definire dei controlli** da imporre.
- **Fornitura di strumenti per verificare** le politica di accesso.

---
## System calls
>Il S.O. implementa le sue funzionalità attraverso dei servizi detti **system calls**.

> [!example]- Esempio di eventi di una syscall
> ![[EMBED/03-Componenti.png]]
> 
[[03-Componenti.pdf#page=12&rect=19,44,704,443|03-Componenti, p.12]]


> [!info]- strace
> `strace` è un comando utilizzato per *tracciare* le syscall.
> 
>
> > [!example] Output di strace
> > 
![[EMBED/03-Componenti 1.png]]
> > 
[[03-Componenti.pdf#page=13&rect=102,88,594,462|03-Componenti, p.13]]


---

## Interprete dei comandi e Shell
### L'interprete

La maggior parte dei comandi vengono forniti dall’utente al SO tramite *istruzioni di controllo* (*Comunemente detti comandi*) che permettono di: 
- Creare e gestire processi.
- Gestire l’I/O.
- Gestire il disco, la memoria, il file system.
- Gestire le protezioni.
- Gestire la rete.

Il programma che legge ed interpreta questi comandi dell'utente è **l’interprete dei comandi**.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

I comandi possono essere **memorizzati** dall'interprete come in due modi:
- Codice <mark class="hltr-orange">integrato</mark> nell'interprete
- Codice <mark class="hltr-purple">separato</mark> in programmi esterni da referenziare

Il secondo metodo è spesso il migliore perchè permette la modifica dei comandi (*o della loro sintassi*) separata dalla shell.

> [!example]- Esempio dello stack di un interprete comandi
> ![[EMBED/03-Componenti 2.png]]
>
[[03-Componenti.pdf#page=16&rect=19,68,708,423|03-Componenti, p.16]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### La shell

L'interprete può fornire diversi tipi di interfaccia:
- CLI (Shell)
- GUI
- Batch
- ...


> [!error]- Shell (CLI) != Interprete 
> La **shell** è l’ambiente/interfaccia (CLI) con cui l'utente interagisce.
> 
> L’**interprete dei comandi** è il componente che legge ed esegue i comandi.
> 
> Tuttavia è vero che in molte implementazioni (*Bash*), <mark class="hltr-red">shell e interprete sono lo stesso programma</mark>.

L'esempio migliore di GUI è il *desktop*:
- Le icone rapppresentano i file, programmi, azioni.
- Le azioni del cursore e dei tasti possono avere diverse funzioni (*informazioni, opzioni, eseguire funzioni,...*).

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Comandi dei programmi tramite API
> La shell è legata all'utente, i programmi invece inviano syscalls **tramite un interfaccia (*API*) offerta dal SO**, spesso scritte in *C* e *Assembly*.




L'API è solitamente una funzione di medio livello che maschera i dettagli implementativi della SysCall (Eg. `read(fd, buffer, size)`).

All'interno della funzione sono presenti la vera chiamata della SysCall al kernel (Eg. `ssize_t sys_read(unsigned int fd, char __user *buf, size_t count);`) e il codice in Assembly necessario; è qui che avviene lo scambio tra <mark class="hltr-purple">user-mode</mark> e <mark class="hltr-orange">kernel-mode</mark>.

> [!example] Esempio di API (Livello basso) : WIN32 ReadFile
> ![[EMBED/03-Componenti 3.png]]
>
>[[03-Componenti.pdf#page=21&rect=91,106,637,289|03-Componenti, p.21]]
>
> ---
> 
>  - `HANDLE` : il file da leggere
>  - `LPVOID` : un buffer di interposizione tra letture/scrittue
>  - `DWORD` : il numero di bytes da leggere dal buffer
>  - `LPDWORD` : il numero di bytes letti nell’ultima read
>  - `LPOVERLAPPED` : indica se è stato usato spooling


> [!example] Processo di chiamata di una SysCall
> ![[EMBED/03-Componenti 4.png]]
>
[[03-Componenti.pdf#page=27&rect=62,55,667,465|03-Componenti, p.27]]


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Le API piú comuni sono... 
- `Win32/Win64` per Windows.
- `POSIX` per sistemi POSIX (*Portable Operating-System Interface for Unix*) che includono di fatto tutte le versioni di UNIX, Linux, e Mac OS X.

API <mark class="hltr-red">DELLO STESSO TIPO</mark> gerantiscono (*in teoria*) la portabilità delle applicazioni tra loro.


> [!info]- Compatibility layers e VM
> I **Compatibility Layers** lavorano sul tradurre le SysCall da un API a un altra mentre un programma è in esecuzione. 
> 
> Un esempio può essere *WIne* che traduce le SysCall da `POSIX` a `WIN@` con l'obiettivo di rendere programmi Windows eseguibili su Linux (*e fratelli*).
> 
> I compatibility layer <mark class="hltr-red">NON IMPLEMENTANO UN KERNEL WIN</mark>.
> 
> ---
>  Una **Virtual Machine** al contrario simula un intero hardware e quindi anche un <mark class="hltr-orange">kernel</mark> del sistema virtualizzato.
>  
>  ---
>  
> In sintesi:
> - **Compat. Layer** : Veloce all'avvio e leggero. Lento nella traduzione e non assicura compatibilità totale.
> - **VM** : Lenta all avvio e pesante sulle risorse dell'host. Compatibilità totale e impatto sulla performance dei programmi simulati minore.  
> 

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Processo di una SysCall
Ogni SysCall ha un numero associato, e il SO mantiene una tabella indicizzata secondo questi numeri per poterle chiamare. 

Dopo l'invocazione della SysCall, è necessario **passarne i parametri** al SO (Kernel); questo può avvenire in tre modi... 

- Passa i parametri tramite **registri**.
- Passa i parametri tramite lo **stack** del processo.
- Memorizzare i parametri in una tabella in **memoria**.



> [!example] Esecuzione di una SysCall con passaggio tramite Stack
> ![[EMBED/03-Componenti 7.png]]
>
[[03-Componenti.pdf#page=31&rect=126,4,705,471|03-Componenti, p.31]]

> [!example] Esecuzione di una SysCall con passaggio tramite tabella
> ![[EMBED/03-Componenti 8.png]]
>
[[03-Componenti.pdf#page=36&rect=10,47,722,465|03-Componenti, p.36]]


