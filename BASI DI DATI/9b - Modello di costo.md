---
Date created: 12-12-25 • 14:42
tags:
  - Databases
Related PDF/DOC:
  - "[[09 Modello di costo.pdf]]"
  - "[[10 Indici.pdf]]"
Related Pages:
  - "[[9a - Tecniche di storage]]"
---
## Modello di costo
> Con costo ci riferiamo al dispendio di risorse necessario a completare un operazione su una base di dati.

Nel nostro caso utilizzeremo un <mark class="hltr-orange">modello di costo basato sul numero di pagine che il DBMS deve utilizzare</mark> per eseguire un' operazione.


> [!danger] Specifica di UTILIZZO
> Con *utilizzare una pagina*, si intende tutte le volte che una pagina viene <mark class="hltr-red">letta o scritta</mark> dalla memoria secondaria.
> 
> Questo vuol dire che operazioni di modifica/inserimento dovranno calcolare il costo di ...
>1. Leggere la pagina dalla memoria.
>2. Scrivere la pagina in memoria dopo la modifica.


Essendo le operazioni in memoria centrale (*RAM*) **MOLTO** più veloci, solitamente vengono trascurate nel modello di costo; Vengono invece enfatizzate le operazioni in memoria secondaria.

---

## Costo delle operazioni per tipo di memorizzazione
### Heap File
> [[9a - Tecniche di storage#Heap File|Definizione di Heap File]]

#### Scan - (SELECT \*)
Avendo la query ...
```sql
SELECT * FROM R;
```

Se $P_R$ è il numero delle pagine che contengono le tuple di $R$ allora ...
$$\text{Costo}_{\text{scan}} = P_R$$
<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Equality Search - (SELECT \* WHERE X = Y)
Avendo la query ...
```sql
SELECT * FROM R WHERE city = ‘Trento’;
```

Se $P_R$ è il numero delle pagine che contengono le tuple di $R$ allora ...
$$\text{Costo}_{\text{eq-search}} = P_R$$
<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Insert
In questo caso il valore del costo è immediato :
$$\text{Costo}_{\text{insert}} = 2$$

Ovvero il costo di ... 
1. Leggere l’ultima pagina del file (*o eventualmente crearne una nuova*).
2. Scrivere la pagina modificata sul disco.


> [!important]- Come raggiungere l'ultima pagina
> Per trovare l’ultima pagina basta calcolare il numero totale di pagine, dividendo la dimensione $S$ del file (*nota*) per la dimensione $P$ delle singole pagine (*fissa*). 
 $$P_R = S/P$$

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Delete
Avendo la query ...
```sql
DELETE FROM R WHERE city=‘Trento’;
```

Se $P_R$ è il numero delle pagine che contengono le tuple di $R$ e $|R_{A=c}|$ è il numero di tuple che corrispondono alla condizione allora ...
$$\text{Costo}_{\text{delete}} = P_R+|R_{A=c}|$$

Solitamente non si conosce il valore esatto di $|R_{A=c}|$, lo si può tuttavia *approssimare* :

Sia $|R|$ il numero di tuple in $R$ e $|R.A|$ il numero di valori diversi dell’attributo $A$ in $R$; possiamo quindi assumere che in media ...

$$|R_{A=c}|= \left\lceil\frac{|R|}{|R.A|}\right\rceil$$

Quindi possiamo infine approssimare la formula del costo di una `DELETE` come :

$$\text{Costo}_{\text{delete}} = P_R+\left\lceil\frac{|R|}{|R.A|}\right\rceil$$


##### Selectivity factor
> [!quote] Selectivity factor
> > Author O'Connell defines **join selection factor** as "the percentage of records in one file that will be joined with records of another file". [...] It is primarily concerned with [query optimization](https://en.wikipedia.org/wiki/Query_optimization "Query optimization").
> > > ~Wikipedia~ ~:~ ~"Join~ ~selectivity~ ~factor"~
> 
> Article : [Link](https://en.wikipedia.org/wiki/Join_selection_factor)


Il selectivity factor **$f$** per un dato attributo (*nel nostro caso $A$*) viene calcolato come...
$$\frac{1}{|R.A|}$$

Di conseguenza un altro modo per scrivere il costo della `DELETE` può essere :


$$\text{Costo}_{\text{delete}} = P_R+\left\lceil f\cdot |R|\right\rceil$$

---

### Sorted file
> [[9a - Tecniche di storage#Sorted file|Definizione di Sorted File]]

#### Scan - (SELECT \*)

Uguale a [[#Heap File#Scan - (SELECT )]].
<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Equality Search - (SELECT \* WHERE X = Y)

Avendo la query ...
```sql
SELECT * FROM R WHERE city = ‘Trento’;
```

In questo caso, dato che le pagine sono ordinate, possiamo utilizzare <mark class="hltr-orange">ricerca dicotomica</mark>.


%%non serve che spiegi come funziona%%

Usando questa tecnica il costo della query diventa :
$$\text{Costo}_{\text{eq-search}}=\left\lceil \log_2(P_R)\right\rceil+\left\lceil  \frac{|R_{A=c}|}{\left\lfloor\frac{P}{t_R}\right\rfloor}\right\rceil $$

Dove gli addendi corrispondono a ... 
1. **Il numero di pagine da leggere** per trovare l'inizio delle tuple col valore cercato.
2. **Il numero di pagine** che contengono tuple che **corrispondono alla condizione**.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Search a intervallo - (SELECT \* WHERE X < Y AND  > Z)

Avendo la query ...
```sql
SELECT * FROM R WHERE age >= 18 AND <=30;
```

Anche in questo caso si utilizza la ricerca binaria; di conseguenza la formula del costo è uguale alla Equality Search :
$$\text{Costo}_{\text{span-search}}=\left\lceil \log_2(P_R)\right\rceil+\left\lceil  \frac{|R_{A=c}|}{\left\lfloor\frac{P}{t_R}\right\rfloor}\right\rceil $$
<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Delete

Anche in questo caso si utilizza la ricerca binaria e la stessa formula scritta in precedenza, tuttavia <mark class="hltr-orange">si aggiunge una moltiplicazione per $2$</mark>. Questo per tenere conto anche delle <mark class="hltr-red">scritture</mark> in memoria che seguono la cancellazione.

$$\text{Costo}_{\text{delete}}=\left\lceil \log_2(P_R)\right\rceil+2\cdot \left\lceil  \frac{|R_{A=c}|}{\left\lfloor\frac{P}{t_R}\right\rfloor}\right\rceil $$

Nel calcolo trascuriamo il costo del riempimento dello spazio liberato dalle tuple eliminate.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


#### Insert
Per inserire nuove tuple in un file ordinato dobbiamo per prima cosa **trovare la posizione corretta** dove inserire la nuova tupla.

Quindi al costo dell'inserimento (*$1$*) si aggiunge quello della ricerca...

$$\text{Costo}_{\text{insert}}=\left\lceil \log_2(P_R)\right\rceil+1 $$

Tuttavia, dobbiamo anche considerare il caso in cui nella pagina non ci sia spazio per aggiungere la nuova tupla. In questo caso (*worst case*) il costo equivalerebbe a $P_R+1$.

E' chiaro che un inserimento in un Sorted File è <mark class="hltr-red">molto più costoso rispetto a un Heap File</mark>.

### Indexed File 
> [[9a - Tecniche di storage#Indexed file|Definizione di Indexed File]]

Per gli indexed file dobbiamo pensare a che rappresentazione usare ...

#### B+ Tree

Un B+ Tree è in generale più efficiente di un *sorted file* perché ...
1. Non ci sono i costi per mantenere ordinato il file dei dati.
2. La ricerca è logaritmica in base $B$ (*e non in base 2*).

Con $B$ numero massimo d
###### Index Matchingi figli di un nodo interno.


##### Costo del lookup
I B+ Tree hanno un costo intrinseco, quello del *lookup*; cioè il <mark class="hltr-purple">numero di pagine che dobbiamo aprire per trovare il nodo foglia che contiene i puntatori ai dati</mark> che stiamo cercando.

Sia $B$ il numero massimo di figli che può avere un nodo interno di un B+ Tree, $n$ il numero di nodi foglia e $A$ la chiave di ricerca, il costo di lookup viene calcolato come ...

$$\left\lceil\log_B(|R.A|)\right\rceil$$

Dato che a ogni passo divido per B il numero di nodi rimanenti.

Se non è noto il valore di $B$ e di $|R.A|$, in genere si assume che il costo di lookup sia 3.
<hr style="width: 70%; margin-left: auto;margin-right: auto;">

##### Equality Search
###### File non-ordinato
Sia la relazione $R$ memorizzata su un file <mark class="hltr-orange">non ordinato</mark> (*unclustered*) e sia $B$ il numero massimo di figli che può avere un nodo, il costo della ricerca sarà :

$$\text{Costo}_{\text{eq-search}} = \text{lookup} + |R_{A=c}|$$

###### File ordinato
Sia la relazione $R$ memorizzata su un file <mark class="hltr-orange">ordinato</mark> (*clustered*) sulla chiave di ricerca e sia $B$ il numero massimo di figli che può avere un nodo, il costo della ricerca sarà :

$$\text{Costo}_{\text{eq-search}} = \text{lookup} + \left\lceil\frac{|R_{A=c}|}{\frac{P}{t_R}}\right\rceil$$

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

##### Search a intervalli
Date le propietà delle foglie di un B+ Tree, in particolare il *[[9a - Tecniche di storage#^5c4a69|sequence set]]*, una volta trovata la foglia di partenza, possiamo usare i puntatori *”orizzontali”* per trovare le restanti tuple.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


##### Insert
L’inserimento ha lo stesso costo dell’Equality Search, più l'eventuale costo di creare una nuova pagina e di salvare la pagina modificata su disco:


$$\text{Costo}_{\text{insert}} = \text{lookup} + 1 +1 $$
Anche qui, non stiamo considerando il costo di aggiornare l’indice.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

##### Scan
Lo scan ha il solito costo di aprire tutte le pagine in cui è memorizzata la relazione R :

$$\text{Costo}_{\text{scan}} = P_R$$

---


#### Hash

##### Equality search
Data la query :
```sql
SELECT * FROM R WHERE age = 30;
```

Sia $L_h$ il costo di lookup, definito come la somma tra ...
1. Il costo del della funzione di hash sul valore cercato(*Trascurato perchè eseguito in RAM, valore 0*).
2. Il costo della lettura della pagina contenente il bucket corrispondente.
3. Tutte le eventuali pagine di overflow (*in genere si assume un valore tra 1 e 2*).

a questo punto possiamo definire il costo dell'operazione come :

$$\text{Costo}_{\text{eq-search}} = L_h+|R_{A=c}|$$

E' apparente che l'hash index è <mark class="hltr-orange">il metodo più efficiente</mark> per gli equality search.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

##### Search su intervalli
Al contrario dell'equality search, l'hash index è <mark class="hltr-purple">poco efficiente</mark> su questo tipo di ricerca; questo perchè <mark class="hltr-red">valori vicini non hanno necessariamente bucket vicini</mark>.  

Per svolgere questa operazione quindi il costo è simile a quello dell'equality search, con la differenza che dobbiamo ripetere il lookup per ogni valore nel range.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

##### Insert

L'insert con gli hash richiede tre operazioni :
1. Aprire l’ultima pagina del **file di dati**.
2. Appendere (*scrivere*) la nuova tupla in fondo.
3. Eseguire il lookup per trovare il bucket in cui inserire il puntatore al nuovo valore.

Quindi da questo possiamo dire che il costo è :
$$\text{Costo}_{\text{insert}}=L_h+1+1$$

Anche in questo caso ignoreremo il costo di aggiornare l’indice.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

##### Delete
Similmente alle operazioni precedenti, la delete richiede ...
1. Eseguire il lookup per trovare il bucket con i puntatori alle tuple che corrispondono alla condizione.
2. Aprire le pagine del **file di dati** che contengono tuple.
3. Rimuovere le tuple e riscrivere la pagina modificata.

Abbiamo quindi che il costo corrisponde a :
$$\text{Costo}_{\text{delete}} = L_h + 2\cdot|R_{A=c}|$$

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

##### Scan
Lo scan ha il solito costo di ...

$$\text{Costo}_{\text{scan}}=P_R$$

Poichè possiamo ignorare l'index file e leggere tutti i dati.

---
#### Index matching

Finora, abbiamo considerato solo selezioni costituite da un **singolo attributo** (*es. city = ‘Trento’*). Per capire come vanno usati gli indici nel caso in cui ci siamo **molteplici condizioni di selezione** dobbiamo introdurre l'index-matching.

Diciamo che un indice <mark class="hltr-purple">soddisfa un predicato</mark> di selezione se l’indice può essere utilizzato per valutare il predicato.


> [!example] Esempio di predicato soddisfatto
> Assumiamo di avere una relazione `R(A,B,C,D)` e un indice hash multi-colonna sulla chiave composta `(A,B)`.
> ```sql
> select * from R where A=10 AND B=5 // MATCH!
select * from R where A=5          // NO MATCH!
> ```
> 
> <hr style="width: 70%; margin-left: auto;margin-right: auto;">
> L'index matching su un <mark class="hltr-purple">hash-index</mark> soddisfa la condizione di selezione <mark class="hltr-red">se e solo se tutti gli attributi nella chiave di ricerca dell’indice compaiono in un predicato di identità.</mark>
> 
> Assumendo sempre una relazione `R(A,B,C,D)` ...
> 
![[EMBED/10 Indici 1.png]]
>
> [[10 Indici.pdf#page=29&rect=18,26,699,270|10 Indici, p.29]]
> 
> <hr style="width: 70%; margin-left: auto;margin-right: auto;">
>
> Invece l'index-matching un un <mark class="hltr-purple">B+Tree-index</mark> soddisfa la condizione di selezione <mark class="hltr-red">se e solo se gli attributi nei predicati costituiscono un prefisso della chiave di ricerca del B+ tree</mark>.
>
 > Assumendo sempre una relazione `R(A,B,C,D)` ...
 > 
 >
![[EMBED/10 Indici 2.png]]
[[10 Indici.pdf#page=31&rect=34,19,690,273|10 Indici, p.31]]


In alcuni casi, <mark class="hltr-orange">una selezione può matchare più indici</mark>.

> [!example]- Esempio di selezione corrispondente a più indici
> Assumiamo :
> - Hash index su `A`
> - B+ tree index su `(B,C)`
>   
La Selezione `A=7 AND B=5 AND C=4` corrisponde a entrambi.

In questi casi è necessario usare più indici allo stesso tempo; esistono diversi modi per fare ciò :

- Usare l’hash index su `A` e poi verificare le condizioni `B=5 e C=4` solo sulle tuple recuperate.
- Usare il B+ tree index su `(B,C)` e poi verificare la condizione `A=7` solo sulle tuple recuperate.
- Usare entrambi gli indici, fare l’intersezione dei *RID* e solo successivamente recuperare le tuple corrispondenti.

