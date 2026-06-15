---
Date created: 04-06-26 • 16:20
tags:
  - Sistemi-Operativi
Related PDF/DOC:
  - "[[09-Deadlock.pdf]]"
Related Pages:
---
>Un insieme di processi è in *deadlock* quando ogni processo è in attesa di un evento che può essere causato solo da un processo dello stesso insieme.

Il deadlock dipende da <mark class="hltr-orange">ordine e tempistiche</mark> di esecuzione.

Queste sono le condizioni necessarie ( *non sufficienti* ) per cui ci possa essere deadlock ...
- **Mutua esclusione** : Almeno una risorsa deve essere *non condivisibile*.
- **Hold and Wait** : Deve esistere un processo che detiene una risorsa e che attende di acquisirne un’altra, detenuta da un altro.
- **No preemption** : Le risorse non possono essere rilasciate se non *“volontariamente”* dal processo che le usa.
- **Attesa circolare** : Deve esistere un insieme di processi che attendono ciclicamente il liberarsi di una risorsa.

Queste condizioni <mark class="hltr-orange">devono essere vere contemporaneamente</mark> perchè si possa verificare il deadlock.

## Modello RAG - Resource Allocation Graph
> Modello astratto utilizzato per rappresentare la concorrenza tra processi e l'allocazione di risorse.

Il RAG è un grafo semplice *G(V,E)*.

I vertici *(V)* sono detti <mark class="hltr-orange">nodi</mark> e rappresentano cose diverse a seconda della forma ...
- **Cerchi** : Processi $P$.
- **Rettangoli** : Risorse $R$ (*CPU, I/O, memoria*).
	- Nei rettangoli vi sono tanti *“•”* quante sono le istanze della  risorsa.

Gli archi *(E)* cambiano significato a seconda del verso ...
- $P \to R$  : Il processo <mark class="hltr-orange">richiede</mark> la risorsa.
- $R \to P$ : Il processo <mark class="hltr-purple">detiene</mark> risorsa.


> [!example]- Esempio di RAG
> ![[EMBED/09-Deadlock.png]]
>
[[09-Deadlock.pdf#page=8&rect=67,52,699,430|09-Deadlock, p.8]]

Il RAG ci permette di identificare i casi di deadlock verificando se il grafo contiene dei **cicli**.

Una volta identificato un ciclo si hanno due casi, dipendenti dal numero di istanze della risorsa :
- **Una sola istanza** : Si ha deadlock.
- **Più istanze** : Deadlock dipende dallo schema di allocazione delle risorse.

---

## Prevenzione del deadlock

Si hanno diverse opzioni per la prevenzione di situazioni di deadlock :
- **Prevenzione statica** : Evitare che si possa verificare una delle quattro condizioni.
- **Prevenzione dinamica** ( *avoidance* ) basata su allocazione delle risorse ( *irrealistica poiché richiede troppa conoscenza sulle richieste di risorse* ).
- **Rilevamento e ripristino** (*detection &  recovery*) : Permettere che si verifichino deadlock e implementare metodi per riportare il sistema al funzionamento normale.
- **Algoritmo dello struzzo** : Non fare nulla, i deadlock sono rari e gestirli costa troppo.

### Prevenzione statica
> Le tecniche di prevenzione statica possono portare a un *basso utilizzo delle risorse* perché mettono vincoli sul modo in cui i processi possono accedere alle risorse.


La **mutua esclusione** è l'unico principio non rimovibile e irrinunciabile per determinate risorse.

Possiamo prendere diversi approcci per la prevenzione statica ...

#### Hold and wait
Princìpi :
- Un processo alloca all’inizio tutte le risorse che deve utilizzare.
- Un processo può ottenere una risorsa solo se non ne ha altre.

Problemi :
 - Basso utilizzo delle risorse.
 - Possibilità di starvation (*risorse “popolari”*).
 - Magari non si è a conoscenza di tutte le risorse richieste.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### No preemption

Princìpi :
- Un processo che richiede una risorsa non disponibile deve cedere tutte le altre risorse che detiene.
- In alternativa, può cedere risorse che detiene su richiesta di un altro processo.

Problemi :
- Fattibile solo per risorse il cui stato può essere facilmente *“ristabilito”* (*CPU, registri, semafori, file*).


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Attesa circolare

- Assegnare una priorità (*ordinamento globale*) ad ogni risorsa ...
	- $F: R → N$
	-  $F(R_0) < F(R_1) < … < F(R_n)$
- Un processo può richiedere risorse solo in ordine crescente di priorità.
- Priorità deve seguire il normale ordine di richiesta (*Eg. disco prima di stampante*).

L’attesa circolare diventa impossibile poiché se ...
$$P_0 → R_0 →P_1→…→ R_{n-1} → P_n → R_n→P_0$$ 
Allora ... 
$$F(R_0) < F(R_1) < … < F(R_n) < F(R_0)$$

Che è impossibile.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Prevenzione dinamica
> La prevenzione dinamica esegue l'*analisi dinamica del RAG* per evitare situazioni cicliche.

La prevenzione dinamica <mark class="hltr-red">richiede la conoscenza del caso peggiore</mark> ( *conoscere il massimo numero di istanze di una risorsa richieste per processo* ).


Per l'analisi si usano diversi **stati**; lo stato di assegnazione delle risorse viene calcolato come: 
-  Numero di istanze disponibili e allocate
- Richieste massime dei processi

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Il sistema si trova in uno stato safe se esiste una *sequenza safe*. Ovvero se usando le risorse disponibili, può allocare risorse ad ogni processo, in qualche ordine, in modo che ciascun di essi possa terminare la sua esecuzione.

Una sequenza di processi ($P_1,...,P_N$) è safe se, per ogni $P_i$, le risorse che Pi può richiedere possono essere esaudite usando : 
- Le risorse disponibili.
- Le risorse detenute da $P_j, j < i$ ( *attendendo che Pj termini* ). 

Se non esiste tale sequenza, siamo in uno <mark class="hltr-red">stato unsafe</mark>. Uno stato unsafe <mark class="hltr-orange">non è necessariamente uno stato di deadlock</mark>, ma può condurre ad esso.

Ogni volta che $P$ richiede $R$, $R$ viene assegnata solo se si rimane in uno stato safe.


Lo svantaggio della prevenzione dinamica è che l’utilizzo delle risorse è minore rispetto al caso in cui non si usino tecniche di questo tipo.


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Una volta a conoscenza dello stato la prevenzione dinamica mira a **utilizzare algoritmi di prevenzione per mantenere il sistema in uno sato safe**.

Si hanno due algoritmi principali per la prevenzione dinamica ...

#### Algoritmo con RAG
Funziona solo se ho un’istanza per ogni risorsa.

Il RAG viene esteso con *archi di rivendicazione* indicati con freccia tratteggiata : $P_i → R_j$ se $P_i$ può richiedere $R_j$ in futuro.


All’inizio, ogni processo deve dire quali risorse vorrebbe usare durante la sua esecuzione.

Una richiesta viene soddisfatta se l’allocazione della risorsa non crea un ciclo nel RAG.

L'algoritmo per la rilevazione cicli ha complessità $O(n^2)$.


> [!example]- Esempio di verifica dello stato
> ![[EMBED/09-Deadlock 1.png]]
>
[[09-Deadlock.pdf#page=25&rect=32,49,688,420|09-Deadlock, p.25]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


#### Algoritmo del banchiere
> Meno efficiente dell'algoritmo RAG ma funziona con più istanze delle risorse.

I processi sono visti come dei <mark class="hltr-purple">clienti</mark> che possono richiedere credito presso la <mark class="hltr-orange">banca</mark> ( *fino ad un limite individuale* ) e le risorse allocabili sono viste come il <mark class="hltr-blue">denaro</mark>. 

La banca, non può permettere a tutti i clienti di raggiungere il loro limite di credito contemporaneamente, poiché in tal caso la banca fallirebbe ( *Il sistema non potrebbe allocare risorse, causando un deadlock* ).

I princìpi di questo algoritmo sono :
- Ogni processo dichiara la sua massima richiesta
- Ogni volta che un processo richiede una risorsa, si determina se soddisfarla ci lascia in uno stato safe ...
	- Algoritmo di allocazione.
	- Algoritmo di verifica dello stato. 


> [!example]- Algoritmo di allocazione
> ![[EMBED/09-Deadlock 2.png]]
>
[[09-Deadlock.pdf#page=28&rect=51,61,697,415|09-Deadlock, p.28]]


> [!example]- Algoritmo di verifica dello stato
> ![[EMBED/09-Deadlock 3.png]]
>
[[09-Deadlock.pdf#page=29&rect=34,60,705,457|09-Deadlock, p.29]]

### Rilevamento e ripristino
Prevenzione statica e dinamica sono conservativi e riducono eccessivamente l’utilizzo delle risorse.

Si hanno quindi due approcci di rilevamento e ripristino :
- Tramite il Grafo di Attesa calcolato tramite il RAG
- Algoritmo di rilevazione

#### R&R tramite RAG
>Analizza periodicamente il Grafo di Attesa, e verifica se esistono deadlock (*detection*) ed inizia il ripristino (*recovery*).


Questo metodo funziona solo con una risorsa per tipo.


Vantaggi :
- Conoscenza anticipata delle richieste non necessaria.
- Utilizzo migliore delle risorse.

Svantaggio :
• Alto costo del recovery.



> [!example] Esempio di rilevamento tramite RAG
> ![[EMBED/09-Deadlock 4.png]]
>
[[09-Deadlock.pdf#page=41&rect=71,34,703,460|09-Deadlock, p.41]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Algoritmo di rilevamento
> Basato sull’esplorazione di ogni possibile sequenza di allocazione per i processi che non hanno ancora terminato.
> Se la sequenza va a buon fine (*safe*), non c’è deadlock.

Quanto spesso questo algoritmo viene chiamato dipende dai casi d'uso ...
- Dopo ogni richiesta
- Ogni N secondi
- Quando utilizzo della CPU scende sotto una soglia T

Quando si rileva un deadlock si possono avere due approcci :
- Uccisione processi coinvolti.
- Prelazione delle risorse dai processi bloccati nel deadlock.

##### Uccisione dei processi
Si può ...
- Uccidere <mark class="hltr-orange">tutti i processi</mark> ( *costoso in qualunque termine* ).
- Uccidere <mark class="hltr-purple"> selettivamente</mark> i processi  fino alla scomparsa del deadlock ( *costoso perchè richiama sempre l'algoritmo di rilevamento* ).

Per l'uccisione selettiva si può andare in ordine di ...
- Priorità 
- Tipi di risorse allocate
- Quante risorse mancavano
- Quanto tempo mancava alla fine

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

##### Prelazione delle risorse
Una volta scelto un processo da prelare bisogna apportare delle modifiche in modo da evitare un deadlock.

Per questo si effettua un *rollback* in uno stato safe; questo potrebbe anche voler dire ripartire da zero.

Questo può portare alla <mark class="hltr-orange">starvation</mark > se si toglie le risorse sempre agli stessi processi.

Quindi per evitare ciò si imposta un *cap* di rollback per processo.

---

## Conclusioni


> [!quote] Schema riassuntivo della prevenzione del 
> ![[EMBED/09-Deadlock 5.png]]
>
[[09-Deadlock.pdf#page=51&rect=37,45,695,444|09-Deadlock, p.51]]

Ognuno dei tre approcci visti ha vantaggi e svantaggi, nessuno è sempre superiore agli altri.

La soluzione *"corretta"* è una combinazione delle precedenti :
1. Partizionare le risorse in classi.
2. Usare una strategia di ordinamento tra classi di risorse (*priorità*).
3. All’interno di una classe, usare l’algoritmo più appropriato per quella classe.

Solitamente le risorse vengono partizionate come :
- Risorse interne ( *Risorse del  sistema. Eg. PCB, I/O* )
- Memoria
- Risorse di processo ( *Eg. File* )
- Spazio di swap ( *blocchi su disco* )

