---
Date created: 29-04-26 • 09:08
tags:
  - Sistemi-Operativi
Related PDF/DOC:
  - "[[04-Architettura.pdf]]"
Related Pages:
---
## Princìpi fondamentali di progettazione

Tradizionalmente i SO venivano scritti in Assembly, tuttavia i SO moderni vengono scritti in linguaggi ad alto livello (*C/C++*) per ottenere :
- Implementazione più rapida
- Miglior compattezza
- Miglior capacità di mantenimento
- Miglior Portabilità

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Durante la progettazione di un SO dobbiamo pensare a...
- <mark class="hltr-orange">Policy</mark> : Cosa fare? (*obeittivo*)
- <mark class="hltr-purple">Meccanismo</mark> : Come fare? (*processo*)

La separazione tra le due è fondamentale, perché permette la maggior flessibilitá se le policy devono essere modificate.

Esistono inoltre delle *"filosofie"* di progettazione... 

- **KISS** : Keep It Small and Simple / Keep It Simple Stupid.
- **POLP** : Principle of Least Privileges.
	- Ogni componente ha solo i privilegi necessari ad eseguire la sua funzione.
	- Garantisce affidabilita e sicurezza.


---

## Architetture di SO
Esistono diverse architetture ...

### Sistemi monoblocco
I sistemi monoblocco <mark class="hltr-red">non hanno una gerarchia</mark>, questo comporta :
- Presente un unico strato SW tra utente e HW.
- I componenti sono tutti allo stesso livello.
- Le procedure possono chiamarsi a vicenda.

Gli svantaggi sono :
- I pezzi di codice dipendenti dall’HW sono sparsi su tutto il S.O.
- Test e debug difficile.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Sistemi a struttura semplice
In questi SO si ha una <mark class="hltr-orange">organizzazione gerarchica minima</mark> ...
- I livelli della gerarchia sono flessibili.
- La struttura mira a semplificare sviluppo e manutenzione. 


#### MS-DOS
> SO Pensato per fornire il maggior numero di funzionalità nel minimo spazio.

MSDOS data la sua filosofia ... 
- <mark class="hltr-orange">Non è suddiviso</mark> in moduli.
- Possiede un struttura minimale, ma  interfacce e livelli di funzionalità non sono ben definiti.
- Non prevede dual mode (*perché Intel 8088 non lo forniva*).



> [!example] Schema della struttura di MS-DOS
> 
![[EMBED/04-Architettura.png]]
>
[[04-Architettura.pdf#page=7&rect=61,9,665,248|04-Architettura, p.7]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### UNIX (originale)
> UNIX introduce il Kernel

Il <mark class="hltr-orange">Kernel</mark>, cioè *tutto ciò che sta tra il livello dell’interfaccia delle sys-call e l’HW*, fornisce diverse funzioni :
- File system
- Scheduling della CPU
- Gestione della memoria
- ...

Inoltre in Unix i programmi di sistema fanno parte dello user-space e operano anch'essi attraverso il kernel.


> [!example] Schema della struttura di UNIX
![[EMBED/04-Architettura 1.png]]
>
[[04-Architettura.pdf#page=9&rect=68,52,649,420|04-Architettura, p.9]]

---

### Sistema a livelli
> I sistemi a livelli offrono servizi organizzati per livelli gerarchici.

L'intero sistema è diviso a livelli, partendo dall'interfaccia utente (*Livello più alto*) e arrivando all'HW (*Livello basso*).

La separazione tra livelli comporta che ogni layer ...
- Possa usare <mark class="hltr-purple">solo funzioni fornite da livelli inferiori</mark>.
- Definisca precisamente il tipo di servizio e l’interfaccia verso il livello superiore nascondendone l’implementazione.

Il vantaggio di questa architettura è la **modularità**, che facilita manutenzione e sviluppo. 

Mentre gli svantaggi sono :
- Difficile definizione degli strati.
- Minor efficienza delle sys-call (*molto overhead*).
- Minor portabilità a causa di funzionalità dipendenti dall’architettura sparse tra livelli.

#### THE ( Dijkstra 1968 )
> Sistema operativo accademico per sistemi batch.

E' il primo sistema a livelli e coordina i programmi attraverso dei **semafori**.



> [!example] Schema della struttura di THE
> ![[EMBED/04-Architettura 2.png]]
>
[[04-Architettura.pdf#page=12&rect=61,45,661,278|04-Architettura, p.12]]

---

### Sistemi kernel based
Questi sistemi implementano **due** soli livelli :
1. <mark class="hltr-orange">Servizi kernel </mark> 
2. <mark class="hltr-purple">Servizi non-kernel</mark>


> [!info]- Eccezioni
> In alcuni sistemi, determinate funzionalità esistono fuori dal kernel come il *File system*. 
> 
> (*Es. Implementazioni “moderne” di UNIX*)

Questi sistemi hanno **gli stessi vantaggi dei sistemi a livelli**, meno che gli svantaggi causati da un numero eccessivo di livelli.

Invece gli svantaggi sono :
- Meno generalità rispetto a un sistema a livelli classico.
- Nessuna regola organizzativa per parti del S.O. fuori dal kernel.
- Kernel complesso che tende al <mark class="hltr-orange">monolitico</mark>.


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### SO con Micro-Kernel
> Questi SO adottano la filosofia di mettere nel kernel solo cio che è **strettamente necessario**.

Quindi sono semplicemente un miglioramento organizzativo rispetto a i sistemi con kernel normali.


> [!example]- Schema della struttura di un SO con micro-kernel 
> ![[EMBED/04-Architettura 5.png]]
>
[[04-Architettura.pdf#page=14&rect=0,68,702,350|04-Architettura, p.14]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Virtual Machine
> La virtual machine è un estremizzazione dell'approccio a livelli...

Una VM viene trattata come *hardware* durante l'esecuzione; in particolare il SO gira sopra la VM.

Durante l'esecuzione la VM illude i processi di essere inividuali (*non uniti sotto una VM*) e di eseguire su un proprio HW.

Il concetto chiave di una VM è la separazione della <mark class="hltr-orange">multiprogrammazione</mark> (*VM*) e della <mark class="hltr-purple">presentazione</mark> (*SO*).


> [!example] Schema della struttura di una VM 
> ![[EMBED/04-Architettura 6.png]]
>
[[04-Architettura.pdf#page=17&rect=38,172,675,466|04-Architettura, p.17]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Una VM offre ...
- Più SO sulla stessa macchina. 
- Isolazione completa tra i  SO.
- Ottimizzazione delle risorse HW.
- Buona portabilità.

Tuttavia hanno compromessi su ...
- Prestazioni.
- Facilità della gestione di una dual mode (*kernel/user*) virtual.
- Isolazione pressochè totale tra SO.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Una VM è composta da due parti :
1. <mark class="hltr-orange">Monitor/Hyprevisor</mark>
2. <mark class="hltr-blue">Runtime Env</mark> (*SO della VM + User Programs*)

In particolare esistono 2 tipi di hypervisor, e questi definiscono come funziona la VM ...


> [!example] Schema delle tipologie di hypervisor
> ![[EMBED/04-Architettura 7.png]]
>
[[04-Architettura.pdf#page=18&rect=18,50,716,395|04-Architettura, p.18]]


#### Hypervisor tipo 1
> Separa runtime-env e HW

L'<mark class="hltr-orange">hypervisor sostituisce il sistema operativo host</mark>, pertanto è in kernel mode e si intra-pone tra l'HW e il runtime-env.

Lo user-space è composto da uno o più runtime enviroments.

##### Design dell hypervisor di tipo 1: Monolitico o MicroKernel
Possiamo anche dividere gli hypervisor di tipo 1 per filosofia di design...

- <mark class="hltr-green">Monolitico</mark> : L'**hypervisor** contiene il *"virtualization stack"* e i driver. 
- <mark class="hltr-blue">MicroKernel</mark> : L'**SO** contiene il *"virtualization stack"* e i driver.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Hypervisor tipo 2
> Separa runtime-env e SO Host

L'<mark class="hltr-purple">hypervisor di tipo 2 viene trattato come un programma utente del sistema host</mark>, pertanto viene eseguito in user mode, e assicura la separazione tra i due SO.

Quindi in questo caso, l'user-space è costituito da un singolo hypervisor e uno o multipli runtime-env, oltre che i processi del SO Host. 

---

### SO Client-Server
>Si basa sull'idea di portare il codice/processi di sistema a livelli superiori, lasciando un kernel più piccolo.

L'approccio è quello di implementare la maggior parte delle **funzioni di sistema operativo nei processi utente**, mentre il kernel si occupa solo della <mark class="hltr-orange">gestione della comunicazione tra client e server</mark>.

Questo modello si presta bene per SO distribuiti.



> [!example] Schema della struttura di un SO Client-Server
> ![[EMBED/04-Architettura 8.png]]
>
[[04-Architettura.pdf#page=25&rect=109,9,638,237|04-Architettura, p.25]]
