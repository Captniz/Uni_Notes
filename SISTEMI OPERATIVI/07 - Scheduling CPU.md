---
Date created: 02-05-26 • 19:03
tags:
Related PDF/DOC:
  - "[[07-Scheduling_CPU v0.pdf]]"
Related Pages:
---
> Lo scheduling è definito come l'assegnazione di attività nel tempo (*riferito ai processi*).

La **multiprogrammazione** impone l’esistenza di una strategia per regolamentare:
1. Ammissione dei processi nella <mark class="hltr-orange">memoria</mark> 
2. Ammissione dei processi all’esecuzione (<mark class="hltr-purple">CPU</mark> - *time sharing*)


> [!example] Schema del ciclo di esecuzione di un processo
> ![[EMBED/06-Scheduling_CPU v0 1.png]]
>
[[06-Scheduling_CPU v0.pdf#page=5&rect=51,59,659,422|06-Scheduling_CPU v0, p.5]]

---


## TIpi di scheduler


> [!example] Schema della differenza tra long-term e short-term scheduler
> ![[EMBED/06-Scheduling_CPU v0 2.png]]
>
[[06-Scheduling_CPU v0.pdf#page=8&rect=140,23,661,189|06-Scheduling_CPU v0, p.8]]


### Scheduler a breve termine (CPU Scheduler)
> Seleziona dalla memoria quale processo deve essere eseguito dalla *CPU*.

Dato che viene invocato spesso esegue le sue operazioni in tempo $O(ms)$.
> [!example]- Calcolo del tempo utilizzato dallo scheduler CPU
>
> *Es.* su 100ms concessi a ogni processo, 10ms sono necessari per lo scheduling.
> 
> $10/(110) = 9\%$ del tempo di CPU sprecato per lo scheduling.

#### Code IO
> Oltre alla *ready queue* sono presenti diverse **IO queues** per i diversi dispositivi IO.

Essenzialmente sono code dei processi in attesa che il dispositivo si liberi.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Dispatcher
> Modulo del SO che passa il controllo della CPU al processo scelto dallo scheduler. 

Il dispatcher esegue il suo compito in tre parti :
1. Scambio del [[06 - Processi e Thread#PCB - Process Control Block - Attributi di un processo |PCB]] dei due processi (*Context switch*).
2. Passaggio alla modalità *user* (*mode switch*).
3. Salto alla opportuna locazione nel programma per farlo continuare.

> [!warning]- Context switching
> Il context switch consiste in ... 
> 1. Registrazione dello stato del processo vecchio.
> 2. Caricamento dello stato (*precedentemente registrato*) del nuovo processo.
> 
> Il tempo necessario al cambio di contesto è puro overhead e la sua durata dipende dall’architettura.
> 
> ---
> 
>
> > [!example] Schema di un context switch
> > ![[EMBED/06-Processi e Thread 3.png]]
> >
[[06-Processi e Thread.pdf#page=21&rect=68,39,667,408|06-Processi e Thread, p.21]]

La *latenza di dispatch* è il tempo necessario al dispatcher per fermare un processo e farne ripartire un altro.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Prelazione ( Preemption )
> Con *prelazione* si definisce il rilascio forzato dalla CPU.

Lo short-term scheduler può essere con o senza prelazione ...
- <mark class="hltr-orange">Preemptive</mark> : Il processo può essere forzato a rilasciare la CPU prima del termine del burst.
- <mark class="hltr-purple">Non-preemptive</mark> : Il processo che detiene la CPU non la rilascia fino al termine del burst.

> [!example] Schema della differenza tra preemptive e non
> ![[EMBED/06-Scheduling_CPU v0 6.png]]
>
[[06-Scheduling_CPU v0.pdf#page=17&rect=74,71,665,224|06-Scheduling_CPU v0, p.17]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Alterazioni del percorso di esecuzione
I processi rimangono nella ready queue finchè il dispatcher non li ammette all'esecuzione.

Durante l’esecuzione, si possono tuttavia avere diverse situazioni che alterano il percorso di esecuzione del processo : 
- Il processo necessita di IO e viene inserito in una IO queue.
- Il processo termina il quanto di tempo accordato, viene rimosso forzatamente dalla CPU e re-inserito nella ready queue (*preemption*).
-  Il processo crea un figlio e ne attende la terminazione.
- Il processo si mette in attesa di un evento.


> [!example] Schema delle code di attesa
> ![[EMBED/06-Processi e Thread 2.png]]
>
[[06-Processi e Thread.pdf#page=17&rect=43,53,678,416|06-Processi e Thread, p.17]]


---
### Scheduler a lungo termine (JOB Scheduler)

>Seleziona quali processi devono essere portati dalla memoria alla *ready queue*. 

Determina inoltre il grado di multiprogrammazione del SO.

A differenza dello short-term scheduler può essere più lento (*$O(s)$*).

Controlla il mix di processi entranti nell'esecuzione; diversi processi richiedono diverse risorse e pertanto devono essere spartiti correttamente :
- <mark class="hltr-orange">IO Bound</mark> : Molte operazioni IO, molti CPU burst brevi.
- <mark class="hltr-purple">CPU Bound</mark> : Molti calcoli, pochi CPU burst lunghi.

> [!example] Schema di esecuzione di un processo
> ![[EMBED/06-Scheduling_CPU v0 4.png]]
>
[[06-Scheduling_CPU v0.pdf#page=15&rect=385,68,704,422|06-Scheduling_CPU v0, p.15]]


Il long-term scheduler <mark class="hltr-red">PUO' ANCHE ESSERE ASSENTE</mark> (*presente solitamente in sistemi con risorse limitate*). 

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Scheduler a medio termine
> Lo scheduler a medio termine è necessario per la momentanea rimozione forzata (*swapping*) di un processo dalla RAM

<mark class="hltr-red">SOLO</mark> i SO con *memoria virtuale* prevedono un livello intermedio di scheduling, che è necessario per ridurre il grado di multiprogrammazione.


> [!example] Schema dello scheduler a medio termine
> ![[EMBED/06-Scheduling_CPU v0 3.png]]
>
[[06-Scheduling_CPU v0.pdf#page=11&rect=38,74,690,395|06-Scheduling_CPU v0, p.11]]

---
## Algoritmi di scheduling

> [!info]- Metriche di scheduling
> > Utilizzo della CPU 
> 
> L’obiettivo è tenere CPU occupata più possibile.
> > Throughput 
> 
> Numero di processi completati per unità di tempo.
> > Tempo di attesa (*waiting time*)
> 
> Quantità totale di tempo spesa da un processo nella coda di attesa; influenzato dall’algoritmo di scheduling.
> 
> > Tempo di completamento (*turnaround*)
> 
> Tempo necessario ad eseguire un particolare processo dal momento della sottomissione al momento del completamento.
> >Tempo di risposta (*response time*)
>
> Tempo trascorso da quando una richiesta è stata sottoposta al sistema fino alla prima risposta del sistema stesso.

### FCFS - First come, first served
> Implementato attraverso una coda FIFO. La più semplice delle implementazioni.

Lo svantaggio di quest'algoritmo è l'*effetto convoglio*, cioè quando processi brevi si accodano ai processi lunghi precedentemente arrivati.

Questo crea problemi in contesti interattivi e programm che richiedono spesso piccoli CPU burst.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### SJF - Shortest Job First
> Il processo con il prossimo burst di CPU più breve viene selezionato per l’esecuzione.

Il calcolo del prossimo burst <mark class="hltr-red">E' SOLO UNA STIMA</mark> basata sui burst precedenti. Si utilizza una media esponenziale basata sulla formula :

$$
τ_{n+1} = α * t_n + (1 - α) * τ_n
$$
- $t_n$ : Lunghezza reale n-esimo burst
- $τ_n$ : Lunghezza stimata n-esimo burst
- $α$ : Coefficiente ($0<α<1$)

Il coefficiente specifica in che quantità considerare le stime e i valori reali precedenti.


> [!example]- Esempio coefficiente
> > $α = 0 \implies τ_{n+1} = τ_n$
> 
> Storia recente non viene usata.
> > $α = 1 \implies τ_{n+1} = t_n$
> 
> Conta solo l’ultimo burst reale.




<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Anche qui si può parlare di variante **preemptive e non** ...

#### SRTF - Shortest Remaining Time First 
> SJF con schema **preemptive**.

In questo algoritmo, se arriva un nuovo processo con un burst di CPU più breve del tempo rimanente all'esecuzione del processo corrente, quest’ultimo viene rimosso dalla CPU per fare spazio a quello appena arrivato.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Scheduling a priorità
> Viene associata una priorità a ogni processo; la CPU viene allocata al processo con priorità più alta.


> [!warning]- Specifica SJF 
> SJF <mark class="hltr-red">E' ANCHE</mark> uno scheduling a priorità:  $$\text{priorita}=1/\text{next burst length}$$


La *priorità* può essere assegnata attraverso politiche interne al SO ...

- Limiti di tempo
- Requisiti di memoria
- File aperti
- ...

... o esterne al SO :

- Importanza del processo
- Motivi politici
- ...

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Il problema più noto di questo scheduling è la <mark class="hltr-orange">starvation</mark>.

La *starvation* implica che processi a bassa priorità possono non venire mai eseguiti.

La soluzione è l'<mark class="hltr-purple">aging</mark>, cioè l'aumento della priorità col passare del tempo.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Anche in questo caso si può parlare di preemptive ...

#### HRRN - Higher Response Ratio Next
> Algoritmo a priorità non-preemptive

La priorità, il *response ratio* ...
$$
R = (t_\text{wait}+t_\text{burst})/t_\text{burst}
$$

va ricalcolata al termine di un processo (*solo se nel frattempo ne sono arrivati altri*) oppure, al termine di un processo.

Con questa formula sono favoriti i processi che:
- Completano in poco tempo (*come SJF*)
- Hanno atteso molto

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### RR - Round Robin
> Scheduling basato su time-out

A ogni processo viene assegnata una piccola parte (*quanto*) del tempo di CPU (*10-100 millisecondi*); al termine del quanto, il processo è messo in coda alla ready queue.

La scelta della dimensione del quanto è fondamentale :

Un $q$ grande essenzialmente emula la **FCFS**, mentre per $q$ piccolo bisogna far attenzione all'overhead introdotto dal *context switch*. Un valore di $q$ ragionevole fa si che sia maggiore dell'80% dei burst di CPU. 

Rispetto a SJF, RR ha *turnaround* maggiore/uguale e *response time* minore/uguale.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Scheduling a code multi-livello
> Classe di algoritmi in cui la ready queue è partizionata in più code.

Le code sono divisi per tipologia di processo e <mark class="hltr-orange">ogni coda ha un suo algoritmo di scheduling</mark>.

Inoltre è necessario un algoritmo di scheduling <mark class="hltr-purple">tra le code</mark>, che può essere ...



> [!example] Schema di una coda multi-livello
> ![[EMBED/06-Scheduling_CPU v0 7.png]]
>
[[06-Scheduling_CPU v0.pdf#page=48&rect=52,59,658,472|06-Scheduling_CPU v0, p.48]]


#### Code multi-livello  a priorità fissa
> Si parte dalla coda con maggior priorità e si va a scendere.

In pratica si serve prima tutti i processi di sistema, poi quelli in foreground, poi quelli in background, ...

Il problema di questo algoritmo è la <mark class="hltr-orange">starvation</mark> delle ultime code.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Code multi-livello basate su time slice
> Ogni coda ottiene un quanto del tempo di CPU che può usare per schedulare i suoi processi.

I quanti <mark class="hltr-red">NON DEVONO PER FORZA AVERE LA STESSA DIMENSIONE</mark>.


> [!example]- Esempio di time slices per le code 
> **80%** per job di *foreground* con RR
> **20%** per job di *background* con FCFS

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Code multi-livello con feedback ( Adattive )
> A differenza della coda  multi-livello classica, un processo può spostarsi as altre code a seconda delle sue caratteristiche.

Questo tipo di coda è utilizzato per implementare l'<mark class="hltr-purple">aging</mark>.

L'algoritmo di scheduling tra code a questo punto deve avere questi parametri :
- Numero di code.
- Algoritmo associati alle code.
- Criteri per promozione/degradazione dei processi.
- Criteri per l'assegnazione iniziale di un processo a una coda.


> [!example]- Esempio di funzionamento di coda multi-livello adattiva
> ![[EMBED/06-Scheduling_CPU v0 8.png]]
>
[[06-Scheduling_CPU v0.pdf#page=50&rect=28,79,691,447|06-Scheduling_CPU v0, p.50]]

---
### Scheduling fair-share
> Algoritmo che si concentra sulle applicazioni (e quindi agli utenti) e non ai singoli processi.

A differenza degli altri algoritmi, fair-share cerca di dividere le risorse per applicazione (*formate da diversi processi*) anzi che dividerle per job.

In sintesi **le risorse vengono divise tra gruppi di processi.**


> [!example] Schema della divisione di risorse del fair-share
> ![[EMBED/06-Scheduling_CPU v0 9.png]]
>
[[06-Scheduling_CPU v0.pdf#page=52&rect=134,44,624,205|06-Scheduling_CPU v0, p.52]]

---

## Scheduling nella realtà
Gli algoritmi reali usano la prelazione e sono spesso basati su **RR**.


> [!example]- Esempio reale di algoritmo di scheduling - Unix Solaris
> Basato su priorità con aging.
> - Priorità = priorità base + priorità corrente
> - Priorità base = [-20 … +20]
> - Priorità corrente = $0.1 \cdot CPU(5\cdot n)$ 
> 	- $CPU(t)$ : utilizzo della CPU negli ultimi $t$ secondi
> 	- $n$ : numero medio di processi pronti all’esecuzione nell’ultimo secondo
> 
> *Concetto*: Lo scheduler “dimentica” il 90% dell’utilizzo di CPU degli ultimi $5n$ secondi.
> 
> *Idea*: favorire processi che hanno usato “poco” la CPU.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Gli algoritmi vengono valutati in tre passi :
- Modello deterministico
- Modello a reti di code
- Simulazione

### Modello deterministico ( Analitico )
> Definisce le prestazioni di ogni algoritmo per uno specifico carico. Di solito usato per illustrate gli algoritmi.

Essenzialmente un esempio con schema di un determinato caso. Non definisce in modo generale le prestazioni dell'algoritmo.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Modello a reti di code
> Modello basato sulla distribuzione prevista di CPU burst e I/O burst.

Il sistema di calcolo è descritto come una *rete di server ognuno con la propria coda*.

Per determinare il carico del modello si utilizzano formule matematiche che determinano: 
- La probabilità che si verifichi un certo CPU burst. 
- La distribuzione dei tempi di arrivo dei processi.

Da questo modello è possibile ricavare : 
- utilizzo
- throughput medio 
- tempi di attesa
- ...

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Simulazione
> Modello programmato del sistema.

Abbastanza precisa ma costosa, vengono poi utilizzati dati statistici o reali sui processi.
