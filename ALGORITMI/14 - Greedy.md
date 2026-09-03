---
Date created: 30-08-26 • 23:16
tags:
  - Algoritmi
Related PDF/DOC:
  - "[[14-greedy.pdf]]"
Related Pages:
---
# Teoria
La filosofia degli algoritmi greedy consiste nel <mark class="hltr-orange">selezionare l'unica decisione che appare ottima</mark> (*localmente ottima*) ogni ciclo. Tuttavia è necessario **dimostrare** che seguendo questo metodo si ottiene un ottimo globale.

La tecnica greedy si può solitamente applicare quando ...
- E' possibile dimostrare che esiste una scelta greedy :
  > Fra le molte scelte possibili, ne può essere facilmente individuata una che porta sicuramente alla soluzione locale ottima.
- Il problema ha sottostruttura ottima :
  > Fatta tale scelta, resta un sottoproblema con la stessa struttura del problema principale.

Il processo di risoluzione attraverso greedy inizia come in programmazione dinamica :
1. Individuiamo una sottostruttura ottima
2. Scriviamo la definizione ricorsiva per la dimensione della soluzione ottima
3. Scriviamo una versione iterativa dell'algoritmo

Per poi passare alla tecnica greedy :
1. Cerchiamo una possibile scelta greedy 
2. Dimostriamo che la scelta greedy porta alla soluzione ottima 
3. Scriviamo un algoritmo ricorsivo o iterativo che effettua sempre la scelta greedy

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


I passi per lo sviluppo di un algoritmo greedy sono i seguenti:

1. Evidenziare i **"passi di decisione"**.
   > Trasformare il problema di ottimizzazione in un problema di "scelte" successive 
2. **Evidenziare una possibile scelta** greedy. 
   >Dimostrare che tale scelta rispetto il "principio della scelta ingorda" 
3. Evidenziare la **sottostruttura ottima**. 
   > Dimostrare che la soluzione ottima del problema "residuo" dopo la scelta ingorda può essere unito a tale scelta 
4. **Scrittura codice**: top-down, anche in maniera iterativa ( *Può essere necessario pre-processare l’input*).

---
# Problemi noti
## PROBLEMA - Insieme indipendente massimale di intervalli 
> Siano dati $n$ intervalli distinti (*aperti a destra*) $[a_1, b_1) \dots [a_n, b_n)$ della retta reale, dove all’intervallo $i$ è associato un profitto $w_i$.
> 
> Trovare un insieme indipendente massimale, cioè un sottoinsieme di massima cardinalità formato da intervalli disgiunti tra loro.

[[13 - Programmazione Dinamica#PROBLEMA - Insieme indipendente di intervalli pesati|Soluzione di problema simile attraverso programmazione dinamica]]

Assumiamo sempre che gli intervalli siano ordinati per tempo di fine in maniera crescente.

Definiamo il sottoproblema $S[i . . . j]$ come l’insieme di intervalli che iniziano dopo $i$ e finiscono prima di $j$.

$$
S[i\dots j] = \{k|b_{i}\le a_k\lt b_{k} \le a_{j} \} 
$$

Per far si che la definizione valga aggiungiamo due intervalli fittizi: 
- Intervallo $0$ : $b_0 = −∞$ 
- Intervallo $n + 1$ : $a_{n+1} = +∞$

In questo modo il problema iniziale corrisponde a $S[0,n+1]$.

Sia inoltre $A[i . . . j]$ una soluzione ottimale di $S[i...j]$ e $k$ un intervallo appartenente ad $A[i\dots j]$.

> [!example] TEOREMA - Sottostruttura ottima
> [[14-greedy.pdf#page=10|14-greedy, p.7]]

Definiamo la soluzione come :
$$
A[i . . . j] = A[i . . . k] ∪ \{k\} ∪ A[k . . . j]
$$

Possiamo determinare $k$ scorrendo tutte le possibilità e trovando la migliore. 

Definiamo quindi la formula ricorsiva e il valore di DP ...
> Sia $DP [i][j]$ la dimensione del più grande sottoinsieme $A[i . . . j] ⊆ S[i . . . j]$ di intervalli indipendenti.

$$
DP[i][j] = \begin{cases}
0 & S[i\dots j] = \emptyset  \\
\text{max}_{k\in S[i\dots j]}(DP[i][k]+DP[k][j]+1) & else
\end{cases}
$$

A questo punto abbiamo terminato la soluzione in <mark class="hltr-purple">p.dinamica</mark> e possiamo passare alla tecnica <mark class="hltr-orange">greedy</mark>.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Dal problema possiamo dire che ...
>Sia $S[i . . . j]$ un sottoproblema *non vuoto*, e $m$ l’intervallo di $S[i . . . j]$ con il minor tempo di fine, allora: 
>1. Il sottoproblema $S[i . . . m]$ è vuoto.
>2. $m$ è compreso in qualche soluzione ottima di $S[i . . . j]$.



> [!example] TEOREMA - Dimostrazione scelta geedy
> [[14-greedy.pdf#page=15|14-greedy, p.11]]

Sapendo questo possiamo semplicemente eseguire la scelta greedy e analizzare il singolo sottoproblema $S[m . . . j]$.

---

## PROBLEMA - Money Change (Resto)
> Sia $t[1 . . . n]$ un insieme di "tagli" di monete (*interi positivi*) e $R$ il resto che dobbiamo restituire.
> 
> Trovare il più piccolo numero intero di pezzi necessari per dare un resto $R$  utilizzando i tagli disponibili, assumendo di avere un numero illimitato di monete per ogni taglio.

Definiamo la sottostruttura ottima ...

Sia $S(i)$ il problema di dare un resto pari ad $i$ e sia $A(i)$ una soluzione ottima del problema $S(i)$, rappresentata da un multi-insieme (*insieme in cui lo stesso elemento può comparire più volte*). 
Sia infine $j ∈ A(i)$ (*$j$ indice di un taglio in $t[n]$*).

Allora, $S(i − t[j])$ è un sottoproblema di $S(i)$, la cui soluzione ottima è data da $A(i) − \{j\}$.

Veniamo subito alla formula ricorsiva e alla definizone di $DP$ :

> $DP [i]$: minimo numero di monete per risolvere il problema $S(i)$.

$$
DP[i] = \begin{cases}
0 & i=0 \\
\text{min}_{1\le j\le n \wedge t[j]\le i}(DP[i-t[j]])+1  & i>0 
\end{cases}
$$

E' facile però vedere una scelta greedy migliore.
Basta selezionare la moneta $j$ più grande tale per cui $t[j] ≤ R$, e poi risolvere il problema $S(R − t[j])$.


> [!example] TEOREMA - Dimostrazione scelta greedy
> [[14-greedy.pdf#page=27|14-greedy, p.22]]


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

## PROBLEMA - Scheduling
> Sia un processore e $n$ job da eseguire su di esso, ognuno caratterizzato da un tempo di esecuzione $t[i]$.
> 
> Trovare una sequenza di esecuzione (permutazione) che minimizzi il tempo di completamento medio.

> [!QUOTE] Tempo di completamento
> > Dato un vettore $A[1 . . . n]$ contenente una permutazione di $\{1, . . . n\}$, il tempo di completamento dell’$h$-esimo job nella permutazione è:
> > $$T_{A}(h)=\sum^h_{i=1}t[A[i]] $$
> PDF : [[14-greedy.pdf#page=30&selection=27,0,44,32|14-greedy, p.25]]

Definiamo la scelta greedy come ...
Esiste una soluzione ottima $A$ in cui il job con *minor tempo di fine* $m$ si trova in prima posizione ($A[0] = m$).

Definiamo invece la sottostruttura ottima ...
Sia $A$ una soluzione ottima di un problema con $n$ job, con $A[0]=m$ (*$m$ come definito in precedenza*). La permutazione dei seguenti $n − 1$ job in $A$ è una soluzione ottima al sottoproblema in cui il job $m$ non viene considerato.


> [!example] TEOREMA - Dimostrazione della scelta greedy
> [[14-greedy.pdf#page=33|14-greedy, p.28]]


---

## PROBLEMA - Knapsack reale o frazionario
> Sia un intero positivo $C$ la capacità dello zaino e siano $n$ oggetti, tali che l’oggetto $i$-esimo è caratterizzato da un profitto $p[i] ∈ \mathbb{Z^+}$ e da un peso $w[i] ∈ \mathbb{Z^+}.$
>
>Trovare un sottoinsieme $S$ di ${1, . . . , n}$ di oggetti tale che il loro peso totale non superi la capacità massima e il loro profitto totale sia massimo.

[[13 - Programmazione Dinamica#PROBLEMA - Knapsack o Zaino|Soluzione di zaino intero con p.d.]]


In questa variante è possibile prendere *frazioni degli oggetti*.

A questo punto possiamo subito vedere la scelta greedy di ordinare gli oggetti per $p[i]/w[i]$ decrescente e prendere sempre il primo. 

Questo approccio ovviamente non è valido per lo zaino intero.


> [!example] TEOREMA - Dimostrazione informale della correttezza
> [[14-greedy.pdf#page=39|14-greedy, p.33]]


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

## PROBLEMA - Compressione di Huffman
> Sia un file $F$ composto da caratteri nell’alfabeto $Σ$
> 
> Quanti bit sono richiesti per codificare il file?


> [!QUOTE] Definizione : Codice a prefisso
> > In un codice a prefisso, nessun codice usato per la rappresentazione è prefisso di un altro codice (condizione necessaria per la decodifica).
>
> PDF : [[14-greedy.pdf#page=43&selection=6,0,8,12|14-greedy, p.36]]


> [!QUOTE] Albero binario di decodifica
> > Albero caratterizzato da ... 
> > - Figlio sinistro/destro: 0 / 1 
> > - Caratteri dell’alfabeto sulle foglie
>
> PDF : [[14-greedy.pdf#page=45&selection=6,0,15,6|14-greedy, p.38]]
> 


Iniziamo subito definendo gli elementi del problema :
Sia $T$ un albero di codifica. Per ogni $c ∈ Σ$, sia $d_T (c)$ la profondità della foglia che rappresenta $c$ (*il codice che rappresenta $c$ richiederà  $dT (c)$ bit*).

Se $f [c]$ è il numero di occorrenze di $c$ in $F$ , allora la dimensione della codifica sarà...

$$C(F, T ) = \sum_{c∈Σ} f [c] · d_T (c)$$

Il principio del codice di Huffman da seguire in questo caso è il seguente :
- Minimizzare la lunghezza dei caratteri che compaiono più frequentemente.
- Assegnare ai caratteri con la frequenza minore i codici corrispondenti ai percorsi più lunghi all’interno dell’albero.

Per fare ciò l'algoritmo da sviluppare segue 4 passi da ripetere per ogni ciclo:
1. Rimuovere i due nodi con frequenze minori $f_x$, $f_y$. 
2. Creare un nodo padre con etichetta "`-`" e frequenza $f_x + f_y$.
3. Collegare i due nodi rimossi con il nuovo nodo (*come figli*). 
4. Aggiungere il nodo padre all’insieme.


> [!example] Schema dell'algoritmo
> ![[EMBED/14-greedy.png]]
>[[14-greedy.pdf#page=56&rect=11,19,231,194|14-greedy, p.49]]

La scelta greedy viene definita come ...
> Scegliere i due elementi con la frequenza più bassa conduce sempre ad una soluzione ottimale.

Basato sulla tesi che esiste un codice prefisso ottimo per $Σ$ in cui $x, y$ (*caratteri con la freq. minore*) hanno la stessa profondità massima e i loro codici differiscono solo per l’ultimo bit ( *foglie sorelle* ).

Mentre la sottostruttura ...
> Dato un problema sull’alfabeto $Σ$, è possibile costruire un sottoproblema con un alfabeto più piccolo.


> [!example] TEOREMA - Dimostrazione della scelta greedy
> [[14-greedy.pdf#page=59|14-greedy, p.52]]

---

## PROBLEMA - Albero di copertura di peso minimo
> Sia ...
> - $G = (V, E)$ un grafo non orientato e connesso 
> - $w : V × V → R$ una funzione di peso (*costo di connessione*). 
> 	- Se $(u, v) ∈ E$, allora $w(u, v)$ è il peso dell’arco $(u, v)$. 
> 	- Se $(u, v) \not\in E$, allora $w(u, v) = +∞$
> 
> Dato il grafo pesato $G$, determinare come interconnettere tutti i suoi nodi minimizzando il costo del peso associato ai suoi archi.

> [!QUOTE] Definizione : Albero di copertura
> > Dato un grafo $G = (V, E)$ non orientato e connesso, un albero di copertura di $G$ è un sottografo $T = (V, E_T )$ tale che ...
> > - $T$ è un albero
> > - $E_T ⊆ E$
> > - $T$ contiene tutti i vertici di $G$
>
> PDF : [[14-greedy.pdf#page=64&selection=6,0,46,1|14-greedy, p.57]]


Definiamo inoltre $A$ come sottoinsieme di archi di qualche albero di connessione minimo.

> [!QUOTE] Definizione : Arco sicuro
> > Un arco $(u, v)$ è detto sicuro per $A$ se $A ∪ \{(u, v)\}$ è ancora un sottoinsieme di qualche albero di connessione minimo.
>
> PDF : [[14-greedy.pdf#page=68&selection=6,0,27,53|14-greedy, p.61]]

> [!QUOTE] Definizone : Taglio
> > Un taglio $(S, V − S)$ di un grafo non orientato $G = (V, E)$ è una partizione di $V$ in due sottoinsiemi disgiunti.
> > 
> > Un arco $(u, v)$ attraversa il taglio se $u ∈ S$ e $v ∈ V − S$. 
> > Un arco che attraversa un taglio è *leggero* nel taglio se il suo peso è minimo fra i pesi degli archi che attraversano un taglio.
> > 
> > Un taglio *rispetta* un insieme di archi $A$ se nessun arco di $A$ attraversa il taglio. 
>
> PDF : [[14-greedy.pdf#page=69&selection=4,0,66,56|14-greedy, p.62]]
> 

A questo punto possiamo sviluppare il teorema sulla scelta greedy come ...
Sia $A ⊆ E$ un sottoinsieme contenuto in un qualche albero di copertura minimo per $G$. 

Avendo un qualunque taglio $(S, V − S)$ che rispetta $A$ e un arco leggero $(u, v)$ che attraversa il taglio, <mark class="hltr-orange">allora l’arco $(u, v)$ è sicuro per $A$</mark>.


> [!example] TEOREMA - Dimostrazione della scelta greedy
> [[14-greedy.pdf#page=73|14-greedy, p.66]]


### Algoritmo di Kruskal
Questo algoritmo per la risoluzione si basa sulla scelta greedy di ordinare gli archi per peso crescente e inserire nell'albero solo quelli che uniscono nuovi vertici all'insieme.

Formalmente ...
L'algoritmo ingrandisce sottoinsiemi disgiunti di un albero di copertura minimo (*insiemi disgiunti tra loro di vertici connessi da archi sicuri*) connettendoli fra di loro fino ad avere l’albero completo.

Per ogni ciclo, si individua un arco sicuro in questo modo :
- Si sceglie l'arco $(u, v)$ di peso minimo tra tutti gli archi. 
- Si verifica che l'arco connetta due distinti alberi (*componenti connesse*) della foresta, basta controllare che gli insiemi di vertici collegati siano diversi.

Per l'implementazione viene utilizzato un **Merge-find set**.


> [!info]- Merge-FInd set
> Un **Merge-Find Set** è una struttura dati usata per gestire un insieme di *insiemi disgiunti*.
>
>Le due operazioni fondamentali sono:
>- $\text{Find}(x)$ → determina a quale insieme appartiene $x$.
>- $\text{Union}(x, y)$ → unisce gli insiemi che contengono $x$ e $y$.

Il tempo di esecuzione per l’algoritmo di Kruskal dipende dalla struttura dati per Merge-Find Set, solitamente si ha $O(m \log n)$.


### Algoritmo di Prim
L’algoritmo di Prim procede mantenendo in $A$ un singolo
albero. L’albero parte da un vertice arbitrario $r$ (*radice*) e cresce
fino a quando non ricopre tutti i vertici.

Ad ogni passo viene aggiunto un arco leggero che collega un
vertice in $V_A$ con un vertice in $V −V_A$, dove $V_A$ è l’insieme di
nodi raggiunti da archi in $A$.

$(V_A, V −V_A)$ è un taglio che rispetta A (*per definizione*) e sappiamo che gli archi leggeri che attraversano il taglio sono sicuri per $A$.

Informalmente l'algoritmo aggiunge ad $A$ il vertice di peso minore che aggiunge un nuovo vertice a $V_A$.

Come per l'algoritmo di Kruskal la complessità è $O(m \log n)$ ma dipende dalla struttura dati usata.