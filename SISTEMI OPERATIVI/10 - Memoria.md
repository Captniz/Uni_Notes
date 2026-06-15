---
Date created: 08-06-26 • 17:17
tags:
  - Sistemi-Operativi
Related PDF/DOC:
  - "[[10-Memoria.pdf]]"
Related Pages:
---
>La condivisione della memoria da parte di più processi è essenziale per l’efficienza del sistema.

Si hanno diverse problematiche da risolvere ...
- Allocazione della memoria ai singoli job.
- Protezione dello spazio di indrizzamento.
- Condivisione dello spazio di indirizzamento.
- Gestione della memoria virtuale (*swap*).

Nei sistemi moderni la gestione della memoria è <mark class="hltr-orange">inseparabile dal concetto di memoria virtuale</mark>.

## Inserimento di processi in memoria
> Ogni programma deve essere portato in memoria e trasformato in processo per essere eseguito.


Questi sono i passi dell'esecuzione di un programma :
1. La **CPU preleva le istruzioni** da eseguire dalla memoria in base al valore del PC.
2. L’ **istruzione viene codificata** e può prevedere il prelievo di operandi dalla memoria.
3. Al termine dell’esecuzione dell’istruzione, il **risultato può essere scritto in memoria**.
4. Quando il processo termina, la sua **memoria viene rilasciata**.

### Traduzione di programma in processo
>La trasformazione da programma a processo avviene attraverso varie fasi *precedenti all’esecuzione*.

In ogni fase si ha una diversa semantica degli indirizzi, che rappresenta la differenza tra <mark class="hltr-orange">spazio logico vs. spazio fisico</mark>.

Gli indirizzi del programma sorgente sono **simbolici** e devono essere tradotti in  fisici ( *indirizzi reali della memoria* ).

Il compilatore associa a questi degli indirizzi **rilocabili**; in seguito il linker o il loader associano agli indirizzi rilocabili indirizzi **assoluti**.


> [!info] Tipi di indirizzi
> Lo spazio di indirizzamento logico è legato a uno spazio di indirizzamento fisico :
>  - Indirizzo <mark class="hltr-purple">logico</mark> ( *indirizzo virtuale* ) : Generato dalla CPU.
>  - Indirizzo <mark class="hltr-orange">fisico</mark>: Indirizzo reale della memoria. 


> [!info] MMU - Memory Management Unit
> Dispositivo **hardware** che mappa indirizzi virtuali (*logici*) in indirizzi fisici.
> Il valore del *registro di rilocazione* è aggiunto ad ogni indirizzo generato da un processo, e inviato alla memoria.
> 
> ![[EMBED/10-Memoria 3.png]]
>
[[10-Memoria.pdf#page=13&rect=125,75,582,236|10-Memoria, p.13]]


> [!example] Schema della traduzione
> ![[EMBED/10-Memoria.png]]
>
[[10-Memoria.pdf#page=6&rect=50,225,680,453|10-Memoria, p.6]]

#### Binding
> Il processo di trasformazione di indirizzi *simbolici* a *fisici* è detto **binding**.

Il binding di dati e istruzioni a indirizzi di memoria può avvenire in tre momenti distinti, suddivisi in due sottocategorie:
- <mark class="hltr-orange">Binding statico</mark> : <mark class="hltr-red">indirizzo fisico e logico coincidono</mark>
	- **Compile time** : Se è noto a priori in quale parte della memoria risiederà il processo, è possibile generare <mark class="hltr-orange">codice assoluto</mark>; se la locazione di partenza cambia è necessario compilare nuovamente.
	- **Load time** : Il codice deve essere *rilocabile*, con indirizzi relativi ( *Eg. 128 byte dall’inizio del programma* ). Se cambia l’indirizzo di riferimento, devo caricare nuovamente il programma.
- <mark class="hltr-purple">Binding dinamico</mark> : <mark class="hltr-red">indirizzo fisico e logico diversi</mark>
	- **Run time** : Binding posticipato se il processo può essere spostato durante l’esecuzione in posizioni diverse della memoria. E'Richiesto supporto hardware per efficienza.

> [!example]- Binding statico vs dinamico
> ![[EMBED/10-Memoria 2.png]]
>
[[10-Memoria.pdf#page=12&rect=16,81,718,433|10-Memoria, p.12]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


> [!error] Caveat binding
> In un sistema multiprogrammato non è possibile conoscere in anticipo dove un processo può essere posizionato in memoria.
> > <mark class="hltr-red">Binding a compile time non possibile.</mark>
> ---
> L’esigenza di avere lo swap impedisce di poter utilizzare indirizzi rilocati in modo statico.
> > <mark class="hltr-red">Binding a load time non possibile.</mark>

Dati i limiti imposto so ha che la rilocazione <mark class="hltr-purple">dinamica</mark> è limitata a sistemi complessi e alla gestione della memoria nei SO.

La rilocazione <mark class="hltr-orange">statica</mark> invece è usata in sistemi per applicazioni specifiche.

> [!example] Schema del binding
> ![[EMBED/10-Memoria 1.png]]
>
[[10-Memoria.pdf#page=8&rect=34,72,707,444|10-Memoria, p.8]]


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Linking

Si hanno due tipi di linking :
- <mark class="hltr-orange">Statico</mark>
- <mark class="hltr-purple">Dinamico</mark>

Nel linking <mark class="hltr-orange">statico</mark> tutti i riferimenti sono **definiti prima dell’esecuzione**; perciò l’immagine del processo contiene una copia delle librerie usate.

Nel linking <mark class="hltr-purple">dinamico</mark> il link viene posticipato **al tempo di esecuzione**. Il codice del programma non contiene il codice delle librerie ma solo un riferimento ( *stub* ) per poterle recuperare ( *Eg. Windows DLL* ).


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Loading

Come per il linking si ha ...
- <mark class="hltr-orange">Statico</mark>
- <mark class="hltr-purple">Dinamico</mark>

Nel loading <mark class="hltr-orange">statico</mark> tutto il codice è caricato in memoria al tempo dell’esecuzione.

Nel loading <mark class="hltr-purple">Dinamico</mark> il caricamento è posticipato; **i moduli vengono caricati in corrispondenza del loro primo utilizzo**. Il codice non utilizzato non viene caricato; questo lo rende utile per programmi con molti casi *“speciali”*.

---

## Schemi di gestione della memoria

> [!warning] Soluzioni realistiche
> Le soluzioni realistiche utilizzano memoria virtuale

### Allocazione contigua
> Processi allocati in memoria in posizioni contigue all’interno di una partizione.

In questo schema la memoria è suddivisa in diverse partizioni **fisse** o **variabili**.


> [!warning] Protezione dei limiti del MMU
> L' MMU consiste di registri di rilocazione per proteggere lo spazio dei vari processi.
> 
> I registri sono :
> - Registro **base** ( *di rilocazione* ) : Indirizzo più basso.
> - Registro **limite** : Indirizzo del limite superiore dello spazio logico.
> 
> <mark class="hltr-red">Ogni indirizzo logico deve risultare minore del limite.</mark>

#### Partizioni fisse
> Memoria suddivisa in partizioni con dimensioni diverse.

Pur essendo semplice, prevede un grado di multiprogrammazione limitato dal numero di partizioni e introduce la possibile <mark class="hltr-orange">frammentazione</mark>  ( *spreco di memoria* ).


> [!error] Tipi di frammentazione
> La frammentazione può essere **interna o esterna** relativa alle partizioni ...
> 
> La frammentazione <mark class="hltr-purple">interna</mark> implica che la dimensione della partizione sia più grande della dimensione del job.
> 
> La frammentazione <mark class="hltr-blue">esterna</mark> implica che vi siano partizioni non utilizzate che non soddisfano le esigenze dei processi in attesa.


L'assegnazione della memoria viene effettuata dallo scheduler a lungo termine secondo due possibili code ...
- **Coda per partizione** : Il processo viene assegnato alla partizione più piccola in grado di contenerlo.
- **Coda singola**

La **coda per partizione** risulta poco flessibile, dato che possono esserci partizioni vuote e job nelle altre code.

La coda singola ( *FIFO* ) è semplice ma utilizza poco la memoria. I processi vengono assegnati o per *best-fit* con la partizione o per *first-fit*.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Partizioni variabili
> Spazio utente diviso in partizioni di dimensioni variabili.
> 
> Questo schema mira a eliminare la frammentazione *interna*, facendo si che le partizioni siano di **dimensioni identiche alla dimensione dei processi**.

La memoria viene vista come un insieme di *holes*, cioè <mark class="hltr-orange">blocchi liberi di memoria disponibile</mark>, quando arriva un processo, gli viene allocata memoria usando la buca che lo può contenere.

Il SO mantiene informazioni su ...
- Partizioni di memoria allocate
- Holes 

Anche qua le strategie per l'assegnazione delle buche sono :
- *first-fit*
- *best-fit*
- *worst-fit*


> [!warning] Riduzione della frammentazione esterna
> Ovviamente la frammentazione esterna persiste: con first fit, dati $N$ blocchi allocati, $N/2$ blocchi vanno persi.
> 
> Il miglioramento più intuitivo è la **compattazione**, dove il <mark class="hltr-orange">contenuto della memoria viene spostato</mark> in modo da rendere contigue le partizioni.
> 
> Questa tecnica è possibile solo se la <mark class="hltr-red">rilocazione è dinamica e richiede la modifica del registro base</mark>; inoltre risulta molto costosa.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Buddy system
> Compromesso tra partizioni fisse e variabili, dove la memoria è vista come una *serie di liste di blocchi*.

I blocchi sono di dimensione $2^K$, con $L<k<U$, dove
- $2^L$ : Più piccolo blocco allocato ( *Eg. 4K* ).
- $2^U$ : Più grande blocco allocato ( *Eg. tutta la memoria* ).

All’inizio tutta la memoria è disponibile, quindi la lista di <mark class="hltr-orange">blocchi di dimensione $2^U$ contiene un solo blocco</mark> ( *che rappresenta tutta la memoria* ) e le <mark class="hltr-orange">altre liste sono vuote</mark>.

Quando arriva una richiesta di dimensione $s$, si cerca un blocco libero con dimensione *“adatta”* <mark class="hltr-red">purché sia pari a una potenza del 2</mark>.

Se ...

$$2^{U-1} \lt s \le 2^U$$

... allora l’intero blocco di dimensione $2^U$ viene allocato.

Altrimenti, il blocco $2^U$ è diviso in due blocchi di dimensione $2^{U-1}$ e si ripete il processo con quest'ultimi (*controllo se $2^{U-2} < s <= 2^{U-1}$* ). Si procede fino al blocco di dimensione $2^L$.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Quando un processo rilascia la memoria, <mark class="hltr-orange">il suo blocco torna a far parte della lista dei blocchi di dimensione corrispondente</mark>.


> [!info] Compattazione
Se si formano 2 blocchi adiacenti di dimensione $2^K$, è possibile **compattarli** ottenendo un unico blocco libero di dimensione $2^{K+1}$.
>
La compattazione, in questo modo, richiede solo di scorrere la lista dei blocchi di dimensione $2^K$, quindi è veloce. 
>
>Tuttavia si ha frammentazione interna dovuta solo ai blocchi di dimensione $2^L$.

---

## Paginazione
> Tecnica per eliminare la frammentazione esterna che si basa sul permettere che <mark class="hltr-orange">lo spazio di indirizzamento fisico di un processo sia non-contiguo</mark>.
>
> Si alloca memoria fisica dove essa è *disponibile*. 

La memoria fisica viene divisa in blocchi di dimensione fissa detti **frame** ( *solitamente 512B ... 8KB* ); lo stesso viene applicato alla memoria logica, ma le suddivisioni vengono dette **pagine**.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Per eseguire un programma avente dimensione $n$ pagine, bisogna trovare <mark class="hltr-purple">$n$ frame liberi</mark>.

Si utilizza quindi una **page table** per mantenere traccia di quale frame corrisponde a quale pagina ( *traduce indirizzi logici in fisici* ); ogni processo ha una page table.


> [!warning] Frammentazione inevitabile
> Sia ...
> - Programma : $2.3\ KB$
> - Dim. Pagine : $1\ KB$
> 
> <mark class="hltr-red">E' inevitabile che dell'ultima pagina vengano usati solo $0.3\ KB$.</mark>
> 
> Tuttavia la frammentazione interna viene <mark class="hltr-orange">limitata all'ultima pagina</mark>.

L’indirizzo generato dalla CPU viene diviso in due parti: 
- Pagina $p$ : Usato come indice nella tabella delle pagine che contiene l’indirizzo di base di ogni frame.
- Offset $d$ : Combinato con l’indirizzo base definisce l’indirizzo fisico che viene inviato alla memoria.


> [!example] Esempio di indirizzo
> Se la dimensione della memoria è $2^m$ e quella di una pagina è $2^n$ ( *parole/byte* ), l’indirizzo è suddiviso così.
>
![[EMBED/10-Memoria 4.png]]
>
[[10-Memoria.pdf#page=41&rect=222,53,537,126|10-Memoria, p.41]]

> [!example]- Schema fisico dell'architettura per la traduzione degli indirizzi
> ![[EMBED/10-Memoria 5.png]]
>
[[10-Memoria.pdf#page=42&rect=6,60,650,477|10-Memoria, p.42]]


> [!example]- Esempio pratico di traduzione di un indirizzo
> ![[EMBED/10-Memoria 6.png]]
>
[[10-Memoria.pdf#page=43&rect=28,81,432,448|10-Memoria, p.43]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

L’implementazione della tabella delle pagine può avere due soluzioni :
- **Implementazione tramite registri**
- **Implementazione in memoria**
	- Tabella delle pagine multi-livello
	- Tabella delle pagine invertita

### Implementazione tramite registri
> Le entry ( *righe* ) della tabella delle pagine sono mantenute nei registri.

Molto efficiente ma <mark class="hltr-orange">fattibile se il numero di entry è limitato</mark> ( *pochi registri* ); inoltre allunga i tempi di *contex switch* perché richiede salvataggio dei registri.

### Implementazione tramite memoria
>La tabella risiede in memoria

Vengono utilizzati due registri :
- **Page-table base register** ( *PTBR* ) : Punta alla tabella delle pagine.
- **Page-table length register** ( *PTLR* ) : <mark class="hltr-purple">Opzionale</mark>, contiene la dimensione della tabella delle pagine. 

Il context switch è più breve perché richiede solo modifica del PTBR e PTLR, tuttavia ogni accesso a dati/istruzioni richiede <mark class="hltr-orange">due accessi in memoria</mark> ( *tabella delle pagine + dato/istruzione* ).

#### TLB - translation look-aside buffers

Il problema del doppio accesso può essere risolto tramite una *cache veloce* detta **translation look-aside buffers** ( *TLB* ); la cache confronta l’elemento fornito con il campo chiave di tutte le entry ( *contemporaneamente* ).

Il TLB è *molto costoso*, quindi viene memorizzato solo un piccolo sottoinsieme delle entry della tabella delle pagine; inoltre ad ogni context switch il TLB viene ripulito per evitare mapping di indirizzi errati.

> [!example] Schema della tabella delle chiavi
>  <mark class="hltr-purple">Chiave</mark> : n° di pagina
>  <mark class="hltr-blue">Valore</mark> : n° di frame
>  
> ![[EMBED/10-Memoria 8.png]]
>
[[10-Memoria.pdf#page=47&rect=344,64,693,211|10-Memoria, p.47]]

Durante un accesso alla memoria ...
- Se la pagina cercata è nel TLB, viene restituito il numero di frame <mark class="hltr-orange">con un singolo accesso</mark>.
- Altrimenti è necessario accedere alla tabella delle pagine in memoria.

Il tempo richiesto per l'accesso è inferiore al 10% del tempo in assenza di TLB.


> [!info]- EAT - Effective Access Time
>  Il tempo di accesso effettivo viene definito come ...
> $$EAT = (T_{MEM} + T_{TLB})*α + (2*T_{MEM} + T_{TLB})*(1-α)$$
> Dove:
> - $T_{TLB}$ : Tempo di accesso a TLB
> - $T_{MEM}$ : Tempo di accesso a memoria
> - $\alpha$  : Hit rate


> [!example]- Architettura per la traduzione con TLB
> ![[EMBED/10-Memoria 9.png]]
>
[[10-Memoria.pdf#page=48&rect=75,57,638,473|10-Memoria, p.48]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Protezione delle pagine
> Realizzata associando bit di protezione ad ogni frame.

I bit sono di diversa tipologia :
- **Bit di validità** : Per ogni entry della tabella delle pagine si può avere tre stati ...
	- *valid* : La pagina associata è nello spazio di indirizzamento logico del processo.
	- *invalid*: La pagina associata <mark class="hltr-red">NON è</mark> nello spazio di indirizzamento logico del processo.
- **Bit di accesso** : Per marcare una pagina modificabile o meno ( *read-only* ) o per marcare una pagina eseguibile o meno.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Pagine condivise
> Se si ha del codice condiviso, vi è un’ *unica copia fisica*, ma più copie logiche.

Il codice read-only ( *rientrante* ) può essere condiviso tra processi ( *Eg : text editor, compilatori, window manager* ), mentre i dati in generale saranno diversi da processo a processo.


> [!example] Schema di pagine condivise
> ![[EMBED/10-Memoria 10.png]]
>
[[10-Memoria.pdf#page=54&rect=151,6,661,472|10-Memoria, p.54]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Spazio di indirizzamento
Nelle architetture moderne ( *Indirizzo 32/64 bit* ), lo spazio di indirizzamento virtuale è 232/264 bit, sono quindi necessari meccanismi per gestire la dimensione della tabella delle pagine ...

- Tabella delle pagine multi-livello
- Tabella delle pagine invertita

##### Tabella delle pagine multi-livello
>Equivalente a *“paginare”* la tabella delle pagine. Solo alcune parti della tabella delle pagine sono memorizzate esplicitamente in memoria, le altre sono su disco.

La paginazione può essere anche su molteplici livelli ( *2,3,4* ).

Ogni livello è memorizzato come una tabella separata in memoria, la conversione dell’indirizzo logico in quello fisico richiede $1+n$ accessi a memoria ( *$n$ numero di livelli* ); il TLB mantiene comunque le prestazioni a livelli ragionevoli.

> [!example] Schema della paginazione multilivello
> ![[EMBED/10-Memoria 12.png]]
>
[[10-Memoria.pdf#page=56&rect=169,2,634,471|10-Memoria, p.56]]


> [!example]- Esempio pratico della paginazione multilivello
> ![[EMBED/10-Memoria 13.png]]
>
[[10-Memoria.pdf#page=58&rect=38,67,715,472|10-Memoria, p.58]]
> 
> ---
> 
>![[EMBED/10-Memoria 14.png]]
>
[[10-Memoria.pdf#page=59&rect=24,53,686,461|10-Memoria, p.59]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

##### Tabella delle pagine invertita
Tabella <mark class="hltr-purple">unica</mark> ( *solo una* ) nel sistema  avente una entry per ogni frame, contenente la coppia `<process-id, page-number>` dove:
- `process-id` : Identificativo del processo che possiede la pagina.
- `page-number` : Indirizzo logico della pagina contenuta nel frame corrispondente a quella entry.

Ogni indirizzo logico generato dalla CPU e’ una tripla `<process-id, page-number, offset>`, segue che si debba <mark class="hltr-blue">cercare il valore desiderato</mark>, vi quindi un aumento del tempo necessario per cercare il riferimento alla pagina.

La ricerca avviene tramite l’equivalente di una tabella hash ( *complessità `O(1)`* ), è necessario quindi un meccanismo per gestire le collisioni quando <mark class="hltr-orange">diversi indirizzi virtuali corrispondono allo stesso frame</mark>.

> [!example]- Schema della traduzione con tabella invertita
> ![[EMBED/10-Memoria 15.png]]
>
[[10-Memoria.pdf#page=62&rect=84,53,638,430|10-Memoria, p.62]]

---

## Segmentazione
> Schema di gestione della memoria che supporta la vista che l’utente ha della memoria. Nella segmentazione un programma è definito come una collezione di segmenti.

Un **segmento** è un unità logica quale, che può essere ...
- Main
- Procedure
- Funzioni
- Variabili locali e globali
- Stack
- Symbol table
- Vettori

Si ha quindi una *segment table*, cioè una mappa di <mark class="hltr-purple">indirizzi logici bidimensionali in indirizzi fisici unidimensionali</mark>; in questa tecnica un indirizzo logico è rappresentato da `<numero di segmento, offset>`.

Ogni entry contiene:
- **Base** : Indirizzo fisico di partenza del segmento in memoria.
- **Limite** : Lunghezza del segmento.


> [!example]- Schema della segmentazione
> ![[EMBED/10-Memoria 16.png]]
>
[[10-Memoria.pdf#page=66&rect=110,53,572,462|10-Memoria, p.66]]

### Memorizzazione

La rappresentazione della tabella in memoria è simile a quella della pagine ed è composta da ...
- **Segment-table base register** ( *STBR* ) : Punta alla locazione della tabella dei segmenti in memoria.
- **Segment-table length register** ( *STLR* ) : Indica il numero di segmenti usati dal processo; un indirizzo logico `<s,d>` è *valido* se $s < STLR$.

$STBR + s$ rappresenta l'indirizzo dell’elemento della tabella dei segmenti da recuperare.

Il TLB viene usato per memorizzare le entry maggiormente usate.

#### Frammentazione
Il SO deve allocare spazio in memoria per i segmenti di un programma; dato che i segmenti hanno lunghezza variabile, l'allocazione dinamica di questi avviene con first-fit o best-fit.

Si ha quindi la possibilità di frammentazione esterna ( *specie per segmenti di dimensione significativa* ).

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Protezione
> La segmentazione supporta per natura la protezione e  condivisione di porzioni di codice.

Ad ogni segmento ( *entità con semantica ben definita* ) sono associati ...
- Bit di modalità ( *read/write/execute* )
- Valid bit ( *0 cioè segmento non legale* )

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Condivisione
A livello di segmento, se si vuole condividere qualcosa basta inserirlo in un segmento; in questo modo si ha la possibilità di condividere anche parti di un programma ( *Eg. funzioni di libreria* ).


> [!example]- Esempio di condivisione
> ![[EMBED/10-Memoria 17.png]]
>
[[10-Memoria.pdf#page=73&rect=13,63,673,470|10-Memoria, p.73]]

---

## Paginazione vs Segmentazione


|               | Paginazione                                                                                                    | Segmentazione                                                                                                           |
| ------------- | -------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Vantaggi**  | Non esiste frammentazione ( *minima interna* ).<br><br>Allocazione dei frame non richiede algoritmi specifici. | Consistenza tra vista utente e vista fisica della memoria.<br><br>Associazione di protezioni/ condivisione di segmenti. |
| **Svantaggi** | Separazione tra vista utente e vista fisica della memoria.                                                     | Richiesta allocazione ( *dinamica* ) dei segmenti.<br><br>Potenziale frammentazione esterna.                            |

---

## Segmentazione paginata
> Consiste nel *paginare* i segmenti.

Ogni segmento è suddiviso in pagine e possiede la sua tabella delle pagine. 

La tabella dei segmenti non contiene l’indirizzo base di ogni segmento, ma l’indirizzo base delle tabelle delle pagine per ogni segmento; questo elimina il problema dell’allocazione dei segmenti e della frammentazione esterna.


> [!example]- Vari esempi pratici
> [[10-Memoria.pdf#page=78|10-Memoria, p.78]]
