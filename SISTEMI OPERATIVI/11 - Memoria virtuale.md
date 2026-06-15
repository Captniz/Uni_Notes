---
Date created: 13-06-26 • 14:47
tags:
  - Sistemi-Operativi
Related PDF/DOC:
  - "[[11-Memoria Virtuale.pdf]]"
Related Pages:
---
> La memoria virtuale, composta da **ram + disco**, permette la separazione della memoria logica ( *utente* ) dalla memoria fisica.

Tutti gli schemi descritti in [[10 - Memoria]] hanno in comune il fatto che <mark class="hltr-orange">l’intero programma debba essere caricato in memoria per essere eseguito</mark>.

In generale, questo non è strettamente necessario ... 

Solo una parte del programma può essere in memoria portando a : 
- **Spazio degli indirizzi logici più grande** dello spazio di indirizzi fisici.
- **Più processi** possono essere mantenuti in memoria.

Per fare ciò, si deve poter *"swappare"* pagine da e verso la memoria e non l’intero processo.

Questa funzione può essere implementata in due modi :
- Paginazione su domanda ( *demand paging* )
- Segmentazione su domanda ( *demand segmentation* )

## Paginazione su domanda
> Una pagina viene caricata in memoria solo quando necessario.

Si hanno quindi meno richieste di IO durante uno swap e meno memoria utilizzata.


> [!example]- Schema paginazione su domanda 
> ![[EMBED/11-Memoria Virtuale.png]]
> [[11-Memoria Virtuale.pdf#page=7&rect=138,49,681,469|11-Memoria Virtuale, p.7]]


<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Lo stato di una pagina viene memorizzato attraverso un **bit di validità** ( *per pagina* ) nella page table e rappresenta se la pagina è presente o no in memoria.

Se durante la traduzione di un indirizzo logico a fisico una entry ha bit non valido ( *pagina non in memoria* ), si ha un <mark class="hltr-orange">page fault</mark>.


Un page-fault causa un interrupt al SO dove ...
1. Il SO verifica una tabella ( *associata al processo* ).
	- Riferimento non valido : `abort`
	- Riferimento valido : carica la pagina
2. Cerca un frame vuoto.
3. Swap della pagina nel frame ( *da disco* )
4. Modifica le tabelle
	- Page table: valid bit = 1
	- Tabella interna del processo: pagina in memoria
5. Ripristina l’istruzione che ha causato il page fault


> [!warning] Primo page fault
> Il primo accesso in memoria di un programma<mark class="hltr-red"> risulta essere sempre un page fault</mark>.

> [!info]- Impatto sul tempo
> 
 La paginazione su domanda influenza l'EAT con la formula :
$$EAT = (1 – p) * t_{mem} + p * t_\text{page-fault}$$
<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Nel caso in cui non ci siano frame liberi, si cerca di rimpiazzarne uno, minimizzando i page fault causati dall'operazione.

In assenza frame liberi, sono necessari due accessi alla memoria ...
- Scrittura su disco della *“vittima”*.
- Scrittura in memoria del frame da caricare.

<mark class="hltr-orange">Raddoppiando quindi tempo di page fault</mark>.

Per ottimizzare questo caso si usa un <mark class="hltr-purple">bit di modifica</mark> ( *dirty bit* ), che ci dice se la pagina è stata modificata ( *scrittura* ) dal momento in cui viene caricata in memoria.

<mark class="hltr-orange">Solo le pagine che risultano modificate</mark> vengono scritte su disco quando diventano vittime.


> [!example] Gestione del page fault
> ![[EMBED/11-Memoria Virtuale 1.png]]
>
[[11-Memoria Virtuale.pdf#page=10&rect=58,12,671,468|11-Memoria Virtuale, p.10]]

### Algoritmi di rimpiazzo
> L'obbiettivo è selezionare una vittima che causi il minor numero di page fault.

La selezione della vittima può essere di due tipi :
- **Locale** :  vittime solo tra i frame del processo stesso.
- **Globale** : Il SO sceglie un frame dall’insieme di tutti i frame.

Il rimpiazzo <mark class="hltr-blue">globale</mark> migliora throughput ed è quindi più usato del locale.


<hr style="width: 70%; margin-left: auto;margin-right: auto;">


L'algoritmo ideale, dovrebbe rimpiazzare le pagine che non saranno usate per il periodo più lungo a venire.

Questo è <mark class="hltr-orange">impossibile</mark> tuttavia, dato che richiederebbe una conoscenza anticipata dei riferimenti, ma possiamo approssimare.
#### FIFO
> La  prima pagina introdotta è la prima ad essere rimossa.

Algoritmo che non valuta l’importanza della pagina rimossa ( *frequenza di riferimento* ), pertanto tende ad aumentare il tasso di page fault.

Inoltre soffre dell’<mark class="hltr-orange">anomalia di Belady</mark>.


> [!info]- Anomalia di Belady
> Come regola generale della memoria virtuale si ha che, all'aumentare dei frame disponibili, diminuiscano i page fault.
> 
> L'anomalia di Belady invece nega questo fatto :
> Per alcuni algoritmi, <mark class="hltr-red">il numero di page fault può non decrescere all’aumentare del numero di frame</mark>.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### LRU - Least Recently Used
> Rimpiazza la pagina che non viene usata da più tempo.

Questo algoritmo usa <mark class="hltr-blue">il passato recente come previsione del futuro</mark>.

La difficoltà di questo algoritmo sta nell'implementazione non banale e nella rappresentazione della *pagina meno utilizzata*.

Possiamo implementare questo algoritmo tramite ...
- **Timestamp** : Ad ogni pagina è associato un timestamp.
	- Costoso a causa della ricerca della pagina
- **Stack** : Viene mantenuto uno stack di pagine; al  riferimento, la pagina viene messa in cima allo stack.
	- Più veloce dato che la pagina in fondo allo stack è la vittima.
	- Pone problemi di memorizzazione e di mantenimento della struttura a basso livello.

Si opta quindi per delle approssimazioni ...

##### Bit di reference
A ogni pagina vengono assegnati $n$ bit di *reference*, che fanno da **registro di scorrimento** per i riferimenti.

Periodicamente ( *solitamente ogni 100ms* ) avvengono questi passi :
1. Se nel periodo la pagina è stata referenziata si setta il bit.
2. Si esegue un right-shift dei bit.
3. Si imposta il nuovo bit a 0.

La vittima sarà quindi la pagina col valore <mark class="hltr-purple">numerico</mark> minore nel proprio registro a scorrimento.

##### LFU - Least Frequently Used
> Mantiene un conteggio del numero di riferimenti fatti ad ogni pagina e rimpiazza la pagina con il conteggio più basso.

Spesso, può non corrispondere alla pagina scelta da un algoritmo LRU.

##### MFU - Most Frequently Used
> Ragionamento inverso rispetto al LFU.

Questa variante è basata sull'ipotesi che la pagina con il conteggio più basso è probabilmente stata appena caricata, e verrà quindi usata ancora.

##### FIFO Second chance
Basato su FIFO, ma con struttura circolare.

Inoltre si ha l'aggiunta di un bit di *seconda chance* che permette alla pagina di essere ignorata una volta. 

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Allocazione dei frame
> Data una memoria con $N$ frame e $M$ processi è importante scegliere bene quanti frame allocare ad ogni processo.


> [!warning] Numero minimo di frame
> 
Ogni processo necessita di un <mark class="hltr-orange">minimo numero di pagine</mark> per poter essere eseguito.
>
Il minimo è dettato dal fatto che l’**istruzione interrotta da un page fault deve essere fatta ripartire**, quindi si ha che ...
$$min_{pag} = \text{massimo numero di indirizzi specificabile in una istruzione}$$

Si hanno due schemi per l'allocazione dei frame :
- **Fisso** : Un processo ha sempre lo stesso numero di frame.
- **Variabile** : Il numero di frame allocati a un processo può variare durante l’esecuzione.

#### Allocazione fissa
> Il numero di frame di un processo non varia nel tempo.

Possiamo inoltre avere due approcci per l'allocazione fissa :
- **In parti uguali** : Dati $m$ frame e $n$ processi, alloca ad ogni processo $m/n$ frame.
- **Proporzionale** : Alloca secondo la dimensione del processo ( *Può non essere un parametro significativo o meno significativo della sua priorità* ).

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


#### Allocazione variabile
> Permette di modificare dinamicamente le allocazioni ai vari processi.

La modifica può avvenire in base a due propietà :
- **Working set**
- **PFF - Page Fault Frequency**

##### Working Set
> Rimodula l’allocazione dei frame in base alle richieste effettive di ogni processo.

Un processo passa da una località ( *di indirizzi* ) all’altra durante la sua esecuzione ( *array, procedure, moduli* ).

Il **working set** è il set di indirizzi con cui il programma sta lavorando; idealmente un processo necessità di un numero di frame pari alla sua località.

Definiamo quindi ...

<mark class="hltr-purple">$WS_i(t,\Delta)$</mark> come *finestra del working set*, cioè il <mark class="hltr-purple">numero di pagine referenziate nell’intervallo di tempo $[t-\Delta, t]$ più recente</mark>.

<mark class="hltr-blue">$WSS_i(t,\Delta)$</mark> come dimensione di <mark class="hltr-blue">$WS_i(t,\Delta)$ in funzione del tempo</mark>.

> [!info]- Selezione del periodo Δ
> - Δ troppo piccolo : poco significativo
> - Δ troppo grande : può coprire varie località
> - Δ = ∞ : copre tutto il programma


> [!info]- Misurazione pratica del working set
> Le pagine di $WS_i$ vengono contate attraverso un timer periodico e dei bit di reference.

La richiesta reale di frame sarà quindi :
$$F = \sum^i{WSS_i}$$

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Nel caso in cui $F$ sia maggiore del numero totale di frame avviene un fenomeno detto <mark class="hltr-orange">thrashing</mark>, dove la CPU perde tempo continuando a eseguire swap.

Le conseguenze sono un basso numero di frame e un *circolo vizioso* di swap.


> [!info] Esplicitazione del circolo vizioso
> ![[EMBED/11-Memoria Virtuale 2.png]]
>
[[11-Memoria Virtuale.pdf#page=49&rect=14,102,700,447|11-Memoria Virtuale, p.49]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

##### PFF - Page Fault Frequency
> Stabilire un range di page-fault *“accettabile”* e variare i frame in base ad esso.

Se il processo provoca pochi page fault, vengono rilasciati dei frame ( *ne ha troppi* ). 

Al contrario se ne provoca troppi il processo ottiene più frame.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Considerazioni aggiuntive
#### Dimensioni delle pagine


| Problema              | Dimensione delle pagine preferita | Motivo                                                         |
| --------------------- | --------------------------------- | -------------------------------------------------------------- |
| Frammentazione        | Piccole                           | Pagine grandi creano frammentazione interna significativa.     |
| Dimensione Page Table | Grandi                            | Page più piccole portano a più entry.                          |
| IO Overhead           | Grandi                            | Pagine piccole non ammortizzano il costo di lettura/scrittura. |
| Località              | Piccole                           | Pagine più grandi rischiano di includere dati non necessari.   |
<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Struttura dei programmi
> Struttura dei programmi influisce sul numero di page fault.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Frame Locking
> Esistono frame che non devono essere mai rimpiazzati.

Questi frame non rimpiazzabili corrispondono a pagine del kernel o pagine usate per trasferire dati in IO.
