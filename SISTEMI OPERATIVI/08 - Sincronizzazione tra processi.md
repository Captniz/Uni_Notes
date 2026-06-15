---
Date created: 08-05-26 • 08:54
tags:
  - Sistemi-Operativi
Related PDF/DOC:
  - "[[08-Sincronizzazione.pdf]]"
Related Pages:
---
## Modello Produttore-Consumatore

> Il modello base della comunicazione tra processi è il *produttore-consumatore* : un processo manda messaggi e l'altro li riceve.

L'esecuzione concorrente dei due processi è permessa da un buffer (*queue*) che contiene i messaggi.

Ovviamente però, per una corretta comunicazione, si devono <mark class="hltr-orange">sincronizzare le tempistiche</mark> di invio e ricevimento dei messaggi.

### Buffer di tipo P/C
> Il buffer nel modello P/C ha una struttura *circolare*.


> [!example] Implementazione del codice del buffer (C++)
> ![[EMBED/08-Sincronizzazione.png]]
>
[[08-Sincronizzazione.pdf#page=5&rect=34,77,693,424|08-Sincronizzazione, p.5]]
>
> ---
>
> Il counter per semplicità di rappresentazione indica gli elementi nel buffer :
> - `ctr = 0` : Vuoto
> - `ctr = N` : Pieno

Il codice del buffer introduce un problema fondamentale della sincronizzazione tra processi : <mark class="hltr-red">Le rush conditions causate dall'accesso simultaneo agli stessi dati</mark>.


> [!example] Rush condition tra P/C sul counter
> ![[EMBED/08-Sincronizzazione 1.png]]
>
[[08-Sincronizzazione.pdf#page=7&rect=28,229,688,400|08-Sincronizzazione, p.7]]

Dobbiamo quindi introdurre il concetto di <mark class="hltr-orange">sezione critica del codice</mark> ...

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Sezione critica
> Porzione di codice in cui si accede ad una risorsa condivisa.

Questa sezione rispetta tre criteri fondamentali :
- **Mutua esclusione** : Solo un processo alla volta può accedere alla sezione critica.
- **Progress** : Solo i processi che *stanno per entrare* nella sezione critica possono decidere chi entra.
- **Bounded Waiting** : Esiste un massimo di volte per cui un processo può aspettare (*consecutive*).


> [!example] Struttura generica di sezione critica
> ![[EMBED/08-Sincronizzazione 2.png]]
>
[[08-Sincronizzazione.pdf#page=10&rect=38,74,700,331|08-Sincronizzazione, p.10]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

La sezione critica e la condivisione di variabili implica un metodo di sincronizzare i processi in ambiente globale, e quindi di <mark class="hltr-orange">condividere celle di memoria</mark>.

Si possono avere diversi approcci per implementare la sincronizzazione ...

<mark class="hltr-blue">Soluzioni software</mark> : 
- Aggiunta di codice alle applicazioni-
- Nessun supporto hardware o del SO.

<mark class="hltr-purple">Soluzioni “Hardware”</mark> : 
- Aggiunta di codice alle applicazioni. %%?????????%%
- Necessario supporto hardware.

---
## Condivisione di memoria tra processi
### Soluzioni software
#### Algoritmo 1
> Algoritmo alternante

> [!example] Implementazione algoritmo 1
> ![[EMBED/08-Sincronizzazione 3.png]]
>
[[08-Sincronizzazione.pdf#page=14&rect=25,133,624,376|08-Sincronizzazione, p.14]]

Problemi dell'algoritmo :
- Richiede <mark class="hltr-orange">stretta alternanza tra i processi</mark>
- Non rispetta il <mark class="hltr-purple">criterio del progresso</mark> : Non c’è nessuna nozione di *“stato”*.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Algoritmo 2
> Algoritmo con accesso su richiesta


> [!example] Implementazione algoritmo 2
> ![[EMBED/08-Sincronizzazione 4.png]]
>
[[08-Sincronizzazione.pdf#page=18&rect=5,161,716,353|08-Sincronizzazione, p.18]]

Questo algoritmo risolve l'alternanza forzata dell'algoritmo 1  ...

Tuttavia questo algoritmo introduce una situazione di **[[09-Deadlock.pdf|deadlock]]** nel caso in cui i due processi eseguano in sequenza dell’istruzione `flag[]=true`.
(*essenzialmente serve una sezione critica per la sezione critica*)


##### Algoritmo 2 - VARIANTE
> Algoritmo 2 con inversione delle istruzioni



> [!example] Implementazione variante alg. 2
> ![[EMBED/08-Sincronizzazione 5.png]]
>
[[08-Sincronizzazione.pdf#page=22&rect=1,136,710,335|08-Sincronizzazione, p.22]]

Qui viene risolto il deadlock, tuttavia violiamo il criterio di <mark class="hltr-orange">mutua esclusione</mark>.


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Algoritmo 3
>Algoritmo combinato di 1 & 2


> [!example] Implementazione algoritmo 3
> ![[EMBED/08-Sincronizzazione 6.png]]
>
[[08-Sincronizzazione.pdf#page=25&rect=33,223,685,456|08-Sincronizzazione, p.25]]

Combinando i due algoritmi precedenti si giunge a una soluzione corretta.


> [!info]- Dimostrazione di correttezza
> [[08-Sincronizzazione.pdf#page=27|08-Sincronizzazione, p.27-28]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Algoritmo del Fornaio
> Algoritmo funzionante per N processi

E' una modifica dell'algoritmo 3 studiato per funzionare per N processi.

L'idea è che ...
- Ogni processo sceglie un numero (`choosing[i]=1`).
- Il numero più basso verrà servito per primo. 
- Per situazioni di numero identico, si usa un confronto a due livelli (`numero, i`).


> [!example] Implementazione dell'algoritmo del fornaio - V1.0 
> ![[EMBED/08-Sincronizzazione 7.png]]
>
[[08-Sincronizzazione.pdf#page=34&rect=12,58,709,452|08-Sincronizzazione, p.34]]

---
### Soluzioni Hardware
Il modo *“hardware”* per risolvere il problema è quello di <mark class="hltr-orange">disabilitare gli interrupt</mark> mentre una variabile condivisa viene modificata.

Il problema di questo metodo è che se il test per l’accesso è troppo lungo, gli interrupt devono essere disabilitati per troppo tempo.

Dobbiamo quindi costringere l’operazione per l’accesso alla risorsa ad <mark class="hltr-red">un unico ciclo di istruzione</mark> (*non interrompibile*).

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Si hanno due approcci ...
- <mark class="hltr-orange">Test-and-set</mark> : Funzione che ritorna il valore di una variabile per poi settarla a `true`.
- <mark class="hltr-purple">Swap</mark> : Funzione che scambia il valore di due variabili.

> [!example] Esempio di utilizzo di test and set
> ![[EMBED/08-Sincronizzazione 8.png]]
>
[[08-Sincronizzazione.pdf#page=38&rect=24,59,698,425|08-Sincronizzazione, p.38]]

> [!example] Esempio di utilizzo di swap
> ![[EMBED/08-Sincronizzazione 9.png]]
>
[[08-Sincronizzazione.pdf#page=40&rect=18,152,694,445|08-Sincronizzazione, p.40]]


Tuttavia entrambi gli algoritmi non concedono una *attesa limitata*.


> [!example]- Variazione di test-and-set con attesa limitata
> ![[EMBED/08-Sincronizzazione 10.png]]
>
[[08-Sincronizzazione.pdf#page=43&rect=7,51,667,464|08-Sincronizzazione, p.43]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

L'approccio HW permette una migliore **scalabilità** (*indipendenti dal numero di processi coinvolti*) e l' estensione a N sezioni critiche risulta immediato.

Tuttavia si ha una **maggiore complessità** per il programmatore rispetto alle soluzioni SW (*Eg. come impongo l’attesa limitata alle soluzioni precedenti?*); l'implementazione corretta richiede del *busy waiting*.


> [!warning] Busy Waiting
> > Il **busy waiting** è una tecnica in cui un thread <mark class="hltr-orange">controlla continuamente una condizione</mark> invece di sospendersi, <mark class="hltr-red">sprecando tempo</mark> della CPU per altri processi.
>
> Un esempio tipico con Test-and-Set:
>```
>while (TestAndSet(&lock))    ;   // attesa attiva
>```
>
>Se il lock è occupato, il thread continua a eseguire il ciclo senza fare lavoro utile, consumando tempo di CPU.


---

### Semafori
> Alternativa generica alle soluzioni HW senza l'implementazione di *busy waiting*.

Un semaforo, è una variabile intera `S` a cui si accede attraverso due primitive (*atomiche*) :
- <mark class="hltr-orange">Signal `V`</mark> :  `S = S+1`.
- <mark class="hltr-purple">Wait `P`</mark> : `S = S-1` <mark class="hltr-red">solo se `S > 0`</mark>, altrimenti si attende.

Un semaforo inoltre può essere ...
- Binario : `S = 0 | 1`
- Generico : `S>=0`

#### Implementazione
L'implementazione reale è diversa dalla concettuale (*Eg. `S` può essere `< 0` per semafori interi*) e `S` rappresenta quanti processi sono in attesa.


La lista dei processi in attesa spesso è FIFO (*strong semaphore*), garantendo attesa limitata.

Nell'implementazione dobbiamo inoltre garantire **atomicità** e assicuraci di evitare una **race condition**.


> [!example] Implementazione semaforo generico
> ![[EMBED/08-Sincronizzazione 12.png]]
>
[[08-Sincronizzazione.pdf#page=53&rect=26,66,714,409|08-Sincronizzazione, p.53]]
>
> ---
> 
> ![[EMBED/08-Sincronizzazione 13.png]]
>
[[08-Sincronizzazione.pdf#page=54&rect=27,41,700,458|08-Sincronizzazione, p.54]]

I semafori possono essere implementati con ...

<mark class="hltr-blue">Busy-waiting</mark> (*spinlock*) : La CPU controlla attivamente il verificarsi della condizione di accesso alla sezione critica. Quest'approccio è scalabile e veloce, ma CPU-intensive, quindi adatto per attese brevi (*accesso a memoria*).

<mark class="hltr-purple">Sleep</mark> (*mutex, semaforo*) : Il processo viene messo in attesa (*sleep*) che si verifichi la condizione di accesso. Più lento rispetto al precedente, e quindi adatto per attese lunghe (*IO*), ma meno impattante sulla CPU. 


> [!warning]- Mutex
> Un semaforo binario **mutex** implica la <mark class="hltr-orange">mutua esclusione</mark> tra processi.


Un ultima alternativa al busy-waiting sarebbe <mark class="hltr-red">disabilitare interrupt durante P e V</mark> (*istruzioni di processi diversi non possono essere eseguite*).


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Applicazioni
I semafori hanno diverse applicazioni ...
- Semaforo binario con valore iniziale 1 (*mutex*) : Protezione di sezione critica per `n` processi.
- Semaforo binario con valore iniziale  0 : Sincronizzazione (*attesa di evento*) tra processi.


> [!example] Esempio di sincronizzazione di evento
> ![[EMBED/08-Sincronizzazione 14.png]]
>
[[08-Sincronizzazione.pdf#page=63&rect=15,42,711,350|08-Sincronizzazione, p.63]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Limitazioni

Anche nel caso dei semafori si parla di **deadlock** e **starvation**. 


> [!example]- Esempio di deadlock per evento
> Processo bloccato in attesa di un evento che solo lui può generare.
> 
> ![[EMBED/08-Sincronizzazione 15.png]]
>
[[08-Sincronizzazione.pdf#page=66&rect=223,174,472,344|08-Sincronizzazione, p.66]]

Inoltre l’utilizzo dei semafori comporta difficoltà nella scrittura dei programmi e poca chiarezza degli algoritmi.

In alternativa, vengono utilizzati specifici <mark class="hltr-purple">costrutti forniti da linguaggi di programmazione ad alto livello</mark>.

##### Costrutti sostitutivi
###### Monitor
> Costrutti per la condivisione sicura ed efficiente di dati tra processi, simile al concetto di _classe_.

Le variabili del monitor sono visibili <mark class="hltr-orange">solo al suo interno</mark> e le sue procedure accedono solo alle variabili del monitor stesso.

**Solo un processo alla volta è attivo** in un monitor: il programmatore non deve codificare esplicitamente la mutua esclusione.

I monitor <mark class="hltr-red">RICHIEDONO</mark> la presenza di memoria condivisa.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Per permettere ad un processo di attendere all’interno del monitor sono necessari opportuni tipi di <mark class="hltr-blue">sincronizzazione</mark>, le variabili di tipo *condition* :
- Dichiarate all’interno del monitor ( *Eg: `condition x, y;`*).
- Accessibili solo tramite due primitive ( *come per i semafori, ma intrinsecamente diverse* ).
	- `wait()`  
	- `signal()`

La `wait()` <mark class="hltr-red">blocca sempre il processo che la chiama</mark>.

Il `signal()` <mark class="hltr-orange">sveglia solo un processo</mark>, nel caso ce ne siano multipli in attesa lo scheduler decide quale processo può entrare.
Un processo che invoca signal ha due possibilità :
- Il processo si blocca e l’esecuzione passa al processo sbloccato.
- Il processo esce dal monitor (*signal deve essere ultima istruzione di una procedura*).


> [!example]- Esempio di uso di monitor
> ![[08-Sincronizzazione.pdf#page=95|08-Sincronizzazione, p.95]]
> [[08-Sincronizzazione.pdf#page=95|08-Sincronizzazione, p.95]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

###### Sincronizzazione (Java)
> Gestita dalla keyword *synchronized*, permette a sezioni critiche o metodi di essere eseguiti da un solo thread alla volta.

Per la sincronizzazione java usa un *lock* per oggetto, detto <mark class="hltr-orange">monitor</mark> (*diverso da quello precedente*) :
- **Metodi** `synchronized static` : Un lock per classe.
- **Blocchi** `synchronized` : Possibile mettere lock su un qualsiasi oggetto per definire una sezione critica.
- Sincronizzazioni addizionali : `wait()`, `notify()`, `notifyAll()`, ereditati da tutti gli oggetti.

Java affronta la sezione critica come astrazione della *concorrenza* tra processi e fornisce soluzioni con diversi compromessi complessità/difficoltà di utilizzo.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


###### Scrittori lettori
> Sia un area di dati condivisa, bisogna gestire gli accessi di *lettori e scrittori*.


Per assicurare la correttezza dei dati con accessi multipli di lettori e scrittori si usano queste regole :
1. Più lettori possono leggere il file contemporaneamente.
2. Solo uno scrittore alla volta può scrivere nel file.
3. Se uno scrittore sta scrivendo nel file, nessun lettore può leggerlo.
4. Gli scrittori non possono leggere.
5. I lettori non possono essere anche scrittori

---

##### Problemi Classici

Si hanno quindi dei problemi classici dei semafori ...
- Problema del *produttore–consumatore*.
- Problema dei *dining philosophers*.
- Problema dello *sleeping barber*.

###### Problema del produttore-consumatore
Siano due processi, uno produttore ed uno consumatore, che condividono un buffer di dimensione fissata. Il produttore genera dati e li deposita nel buffer di continuo; contemporaneamente, il consumatore consuma i dati prelevandoli  dal buffer. 

Il problema è assicurare che il produttore non elabori nuovi dati se il buffer è pieno, e che il consumatore non cerchi dati se il buffer è vuoto.


La soluzione è utilizzare **3 semafori** :
- `mutex`  (*binario inizializzato a TRUE*) : Mutua esclusione per il buffer.
- `empty` (*intero inizializzato a N*) : Blocca P se buffer è pieno. 
- `full` (*intero inizializzato a 0*) : Blocca C se buffer è vuoto.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

###### Problema dei Dining-Philosophers
`N` filosofi passano la vita mangiando e pensando su una tavola con `N` bacchette e `1` ciotola di riso. 

Se un filosofo pensa non interagisce con gli altri, mentre se un filosofo ha fame prende `2` bacchette e inizia a mangiare; quando un filosofo termina di mangiare rilascia le bacchette.

Inoltre sono disposte le condizioni :
- Il filosofo può prendere solo le bacchette che sono alla sua destra e alla sua sinistra.
- Il filosofo può prendere una bacchetta alla volta (*prima una poi l'altra*).
- Se non ci sono 2 bacchette libere il filosofo non può mangiare.


> [!example] Soluzione corretta
>  ![[08-Sincronizzazione.pdf#page=81|08-Sincronizzazione, p.81]]
>  [[08-Sincronizzazione.pdf#page=81|08-Sincronizzazione, p.81]]


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

###### Problema dello Sleeping Barber

...

