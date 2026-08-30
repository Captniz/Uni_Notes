---
Date created: 30-08-26 • 23:14
tags:
  - Algoritmi
Related PDF/DOC:
  - "[[13_1-p_dinamica.pdf]]"
  - "[[13_2-p_dinamica.pdf]]"
  - "[[13_3-p_dinamica.pdf]]"
Related Pages:
---
# Teoria
> Metodo di programmazione basato sulla memorizzazione di sottoproblemi precedenti che portano alla soluzione completa.


> [!info] Schema programmazione dinamica
>![[EMBED/13_1-p_dinamica.png]]
>
> [[13_1-p_dinamica.pdf#page=5&rect=5,13,359,230|13_1-p_dinamica, p.3]]

Nei problemi di dinamica si inizia definendo un qualche tipo di *memoria* (Array, Matrice, ...), chiamato solitamente $DP$. 

La metodologia standard di risoluzione di un problema di dinamica è detto **bottom-up**, cioè si parte dal risolvere il minore dei sottoproblemi e si risale la piramide fino a risolvere il problema originale.

Avendo $DP$ la norma è risolvere l'algoritmo in maniera iterativa (*si evita la ricorsione*).

## Fasi della programmazione dinamica
Possiamo riassumere la soluzione di un problema attraverso p.dinamica con queste fasi :
1. Caratterizzare la **struttura** di una soluzione ottima
2. Dimostrare che la soluzione gode di **sottostruttura ottima**.
3. Definire **ricorsivamente il valore di una soluzione** ottima. 
4. **Calcolare il valore** di una soluzione ottima attraverso ...
	- <mark class="hltr-orange">Programmazione dinamica</mark> : Metodo *"bottom-up"*
	- <mark class="hltr-purple">Memoization</mark> : Metodo *"top-down"* 
5. Ricostruzione di una soluzione ottima ,(*Opzionale - Necessario se il valore richiesto dal problema non è "in chiaro" nella tabella $DP$* ).

---

# Problemi noti
## PROBLEMA - Domino
> l gioco del domino è basato su tessere di dimensione $2 × 1$. Scrivere un algoritmo efficiente che prenda in input un intero $n$ e restituisca il numero di possibili disposizioni di n tessere in un rettangolo $2 × n$.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Definiamo una formula ricorsiva `DP[n]` che permetta di calcolare il numero di disposizioni possibili quando si hanno `n` tessere.

Come per altri algoritmi ricorsivi si parte dal passo base...

$$
DP[n]=\begin{cases}
1 & n\le1 \\
? & n>1
\end{cases}
$$

Pensiamo ora alle *mosse o possibilità* che abbiamo a disposizione con $n\ge2$ ...
- <mark class="hltr-orange">Tessera in verticale</mark> : Occupa $1$ spazio orizzontale del rettangolo, lasciando $n-1$ spazi aperti a possibilità.
- <mark class="hltr-purple">Tessera in orizzontale</mark> : Occupa $2$ spazi orizzontali, lasciando $n-2$ spazi aperti.

Vedendo il problema attraverso queste 2 *possibilità* possiamo dire che per ogni spazio $n$ che aggiungiamo dobbiamo considerare la somma delle disposizioni di $n-1$ e $n-2$.

In sintesi :
- Se metto una tessera in verticale, risolverò il problema di dimensione $n − 1$.
- Se metto una tessera in orizzontale, ne devo mettere due; risolverò il problema di dimensione $n − 2$

Arriviamo quindi alla formula ricorsiva ...
$$
DP[n]=\begin{cases}
1 & n\le1 \\
DP[n-2] + DP[n-1] & n>1
\end{cases}
$$

Partendo ora dal passo base possiamo trovare la soluzione per qualsiasi $n$.

---
## PROBLEMA - HateVille
> Hateville è un villaggio composto da $n$ case.
> Ogni abitante $i$ ha intenzione di donare una quantità $D[i]$, ma non intende partecipare ad una raccolta fondi a cui partecipano uno o entrambi i propri vicini $i − 1$ e $i + 1$.
> 
> Scrivere un algoritmo che dato il vettore $D$, restituisca la quantità massima di fondi che può essere raccolta.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Ri-definiamo il problema ...

>Sia $HV (i)$ uno dei possibili **insiemi di indici** da selezionare per ottenere una donazione ottimale dalle prime $i$ case di Hateville, numerate $1 . . . n$.
>
>$HV (n)$ è la **soluzione del problema originale**.


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Ora consideriamo il vicino $i$-esimo, quali sono le *possibilità*...
- <mark class="hltr-orange">Accetto la donazione di $i$</mark> :  $HV (i) = {i} ∪ HV (i − 2)$
- <mark class="hltr-purple">Rifiuto la donazione di $i$</mark> : $HV (i) = HV (i − 1)$

Ora che abbiamo in mente le possibilità come facciamo a decidere quale assegnare a $DP[i]$? 

In questo caso cerchiamo il caso migliore tra i due cioè quello che ci porta ad una donazione maggiore :

$$
HV (i) = highest(HV (i − 1), {i} ∪ HV (i − 2))
$$

Completiamo quindi l'equazione di ricorsione :
$$\begin{cases}
\emptyset & i=0 \\
\{1\} & i=1 \\
highest(HV (i − 1), {i} ∪ HV (i − 2)) & i\ge2
\end{cases}$$

Riconvertiamo ora a $DP$...
>Sia $DP[i]$ il valore della massima quantità di donazioni che possiamo ottenere dalle prime $i$ case di Hateville.
>
>$DP[n]$ è la soluzione ottima per $n$ case.

Facciamo questo poichè memorizzare degli insiemi $HV$ è uno spreco inutile di risorse.

Infine avremo quindi :
$$\begin{cases}
0 & i=0 \\
DP[1] & i=1 \\
max(DP (i − 1), DP[i] + DP[i − 2]) & i\ge2
\end{cases}$$

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Teorema della sottostruttura ottima
La soluzione di questo algoritmo è data dal teorema della sottostruttura ottima:

> Sia $P_i$ il problema dato dalle prime $i$ case
>  Sia $S_i$ una soluzione *ottima* per il problema $P_i$
>  
> Ne consegue: 
> - Se $i \notin S_i$ : Allora $S_i = S_i−1$ 
> - Se $i ∈ S_i$ : Allora $S_i = S_i−2 ∪ \{i\}$



> [!example]-  Dimostrazione del teorema
> [[13_1-p_dinamica.pdf#page=28|13_1-p_dinamica, p.26]]

---

## PROBLEMA - Knapsack o Zaino

>Sia un insieme di $n$ oggetti, ognuno caratterizzato da un *peso* $w$ e un *profitto* $v$, e uno "zaino" con un *limite di capacità* $C$.
>
> Individua un sottoinsieme di oggetti il cui **peso sia inferiore alla capacità dello zaino** e il **valore totale degli oggetti sia massimale**.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

I ***sottoproblemi*** in questa situazione si riferiscono al fatto che selezionando un oggetto $i$ riduciamo il problema in due modi :
- La capacità dello zaino passa da <mark class="hltr-orange">$C$ a $C-w[i]$</mark>
- La "pool" degli oggetti passa da <mark class="hltr-purple">$n$ a $n-1$</mark> ( *Viene ovviamente escluso l'oggetto appena preso*).

Definiamo quindi DP ...
>  Definiamo $DP[i][c]$ come il massimo profitto che può essere ottenuto dai primi $i ≤ n$ oggetti contenuti in uno zaino di capacità $c ≤ C$.
> 
> Il massimo profitto ottenibile dal problema originale è rappresentato da $DP[n][C]$.

Ora che le ***scelte*** che abbiamo a disposizione sono ...
- <mark class="hltr-orange">Prendere l'oggetto $i$</mark> : $DP [i][c] = DP [i − 1][(c − w[i])] + p[i]$
- <mark class="hltr-purple">Lasciare l'oggetto $i$</mark> : $DP [i][c] = DP [i − 1][c]$

Prendiamo la scelta secondo questa formula...

$$
DP [i][c] = max( \overbrace{DP [i − 1][(c − w[i])] + p[i]}^{\text{Prendere}}, \overbrace{DP [i − 1][c]}^{\text{Lasciare}})$$

che significa o <mark class="hltr-orange">prendo e diminuisco la pool e la capacità</mark> massima o <mark class="hltr-purple">lascio e tengo il punteggio di prima</mark>.

Scriviamo infine la formula con i casi base :
$$
DP[i][c]= \begin{cases}
0 & i=0 \text{ or } c=0 \\
-\infty & c<0 \\
max( DP [i − 1][(c − w[i])] + p[i], DP [i − 1][c]) & \text{else}
\end{cases}
$$

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Complessità di un algoritmo rispetto ad un valore numerico in input

Possiamo dire che questo algoritmo ha complessità <mark class="hltr-blue">polinomiale</mark> ...
$$T(n)=O(nC)$$

Tuttavia questo, se pur corretto, rappresenta la realtà solo parzialmente.

Infatti possiamo anche vedere il valore $C$ non come *numero* ma come **rappresentazione binaria di un valore**, quindi in bit.

In questo caso allora si ha che il numero di bit necessari per rappresentare $C$ sia $bit = \lceil\log C\rceil$.  A questo punto la complessità diventa ...

$$
T(n)=O(n 2^{bit} )
$$

Possiamo vedere quindi che la complessità non risulta più polinomiale.

Algoritmi di questo tipo vengono chiamati <mark class="hltr-red">pseudo-polinomiali</mark>.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Memoization
In questo algoritmo, visualizzando la tabella $DP$ possiamo vedere come non tutte le celle siano necessarie al raggiungimento della soluzione :


> [!example] Tabella DP per un problema knapsack
> ![[EMBED/13_2-p_dinamica.png]]
>
> [[13_2-p_dinamica.pdf#page=6&rect=8,22,255,169|13_2-p_dinamica, p.4]]

In questo caso la soluzione $DP[n][C]$ deriva da $DP[n-1][C]$ e $DP[n-1][C-4]$, saltando le altre celle (*Ovviamente questo vale anche per tutte le altre sotto-soluzioni*).

E' quindi apparente che in questo caso converrebbe un approccio <mark class="hltr-orange">top-down</mark> opposto allo standard <mark class="hltr-purple">bottom-up</mark> della dinamica.

La soluzione è questa :
- Si crea una tabella $DP$, con le celle inizializzate con un valore speciale che indica che il sottoproblema corrispondente<mark class="hltr-red"> non è ancora stato risolto</mark>.
- La risoluzione del problema parte dal risultato finale, nel nostro caso $DP[n][C]$.
- Quando il sottoproblema viene *richiesto* da un problema superiore, si controlla se è già stato risolto ... 
	- **SI** : Si usa il risultato della tabella.
	- **NO** : Si calcola il risultato e lo si memorizza.

Questa tecnica è detta **Memoization** .


> [!example]- Esempio di memoization
> Come detto prima, partendo da $DP[n][C]$, questo andrà a richiedere la risoluzione dei sotto-problemi 
> - $DP[n-1][C]$ 
> - $DP[n-1][C-4]$
> 
> Che a loro volta richiederanno la soluzione di altri sottoproblemi, fino a risalire l'albero di ricorsione e trovare la soluzione completa.

La memoization spesso viene implementata attraverso *hash-table* rispetto a rabelle poichè ha diversi vantaggi :
- Miglior complessità spaziale.
- Non è necessario fare inizializzazione (*non si ha costo di inizializzazione*).
- Il costo di esecuzione è pari a $O(min(2^n, nC))$.

---

## PROBLEMA - Sottoseq. Comune Massimale
> Date due sequenze $T$ e $U$ di lunghezza $n$ e $m$, scrivere una formula ricorsiva $DP [i][j]$ che restituisca la lunghezza della LCS dei prefissi $T (i)$ e $U (j)$.

> [!QUOTE]- Definizione : Sottosequenza
> > Una sequenza $P$ è una sottosequenza di $T$ se $P$ è ottenuto da $T$ rimuovendo uno o più dei suoi elementi.
> > 
> > Alternativamente, $P$ è definito come il sottoinsieme degli indici $\{1, . . . , n\}$ degli elementi di $T$ che compaiono anche in $P$.
> > 
> >Gli elementi di $P$ non sono necessariamente contigui all'interno di $T$. 
>
> PDF : [[13_2-p_dinamica.pdf#page=27&selection=6,0,45,31|13_2-p_dinamica, p.25]]


> [!QUOTE]- Definizione : Sottosequenza comune
> > Una sequenza $X$ è una sottosequenza comune (*common subsequence*) di due sequenze $T$ , $U$ , se è sottosequenza sia di $T$ che di $U$.
> > $$ X ∈ cs(T, U )$$
>
> PDF : [[13_2-p_dinamica.pdf#page=28&selection=6,0,35,1|13_2-p_dinamica, p.26]]


> [!QUOTE]- Definizione : Sottosequenza comune massimale
> > Una sequenza $X ∈ cs(T, U )$ è una sottosequenza comune massimale (*longest common subsequence*) di due sequenze $T$ , $U$ , se non esiste altra sottosequenza comune $Y ∈ cs(T, U )$ tale che $Y$ sia più lunga di $X$ ($|Y | > |X|$). 
> > $$X ∈ lcs(T, U )$$
>
> PDF : [[13_2-p_dinamica.pdf#page=28&selection=39,0,99,1|13_2-p_dinamica, p.26]]


Anche in questo caso si deve ridurre in sottoproblemi il problema originale.


> [!info] Definizione : Prefisso
> Sottosequenza composta dai primi $n$ caratteri di una sequenza.
> $$ \begin{array} \\\text{Sia } T :\text{ABCDEF} \\T(0)=\text{""} \\T(3)=\text{ABC} \\\end{array}$$
^2bcb19

Ora possiamo fare anche la differenza tra due serie di sottoproblemi tra prefissi :
- $lcs(T(0\to n),U(0\to m))$
- $lcs(U(0\to m),T(0\to n))$

Veniamo quindi ai passi della formula ricorsiva e alle scelte da fare; si hanno due casi da prendere in esame :
- <mark class="hltr-purple">Gli ultimi caratteri di $T (i)$ e di $U (j)$ coincidono ( $t_{i}= u_{j}$ )</mark> : $$LCS(T (i), U (j)) = LCS(T (i − 1), U (j − 1)) ⊕ t_i$$
- <mark class="hltr-orange">I caratteri non coincidono ($t_{i}\neq u_{j}$)</mark> : $$LCS(T (i), U (j)) = longest(LCS(T (i − 1), U (j)), LCS(T (i), U (j − 1))$$

Andiamo quindi a scrivere la formula ricorsiva, tradotta per $DP$ e aggiungiamo il caso base:
$$
\begin{cases}
0 & i=0 \text{ or } j=0 \\
DP[i-1][j-1]+1 & i>0\text{ and } j>0\text{ and } t_{i} = u_{j} \\
max\{DP[i][j-1],DP[i-1][j]\} & i>0\text{ and } j>0\text{ and } t_{i} \neq u_{j}
\end{cases}
$$

$DP [n][m]$ contiene la lunghezza della LCS del problema originale.

---

## PROBLEMA - String Matching Approssimato
> Trovare un’occorrenza $k$-approssimata di $P$ in $T$ con $k$ minimo ($0 ≤ k ≤ m$).


> [!QUOTE] Occorenza $k$-approssimato
> > Siano: 
> > - $P = p_1 . . . p_m$ una stringa detta *pattern*.
> > - $T = t_1 . . . t_n$ una stringa detta testo, con $m ≤ n$, 
> >   
> > Un’occorrenza $k$-approssimata di $P$ in $T$ è una copia di $P$ in $T$ in cui sono ammessi $k$ "errori" tra $P$ e $T$ , del seguente tipo: 
> > 1. I corrispondenti caratteri in $P$, $T$ sono diversi (**sostituzione**).
> > 2. Un carattere in $P$ non è incluso in $T$ (**inserimento**).
> > 3. Un carattere in $T$ non è incluso in $P$ (**cancellazione**).
>
> PDF : [[13_3-p_dinamica.pdf#page=3&selection=6,0,102,15|13_3-p_dinamica, p.1]]

Simile ai problemi in precedenza vediamo di fare i controlli su [[#^2bcb19|prefissi]] delle due sequenze.

> Definiamo $DP[0 . . . m][0 . . . n]$ tale che $DP[i][j]$ sia il minimo valore $k$ per cui esiste un’occorrenza $k$-approssimata di $P(i)$ in $T (j)$ <mark class="hltr-orange">che termina nella posizione $j$</mark>.
> 
> Questo sta a significare che, anche trovando un riscontro $0$-approssimato, se vi fossero caratteri aggiuntivi dopo il pattern questi verrebbero contati come errori.

Definiamo le possibilità :
1. <mark class="hltr-orange">$P(i)=T(j)$</mark> $\to$ Avanzo su entrambi i caratteri (*ugualgianza*) : 
   $DP [i][j] = DP [i − 1][j − 1]$
2. <mark class="hltr-purple">$P(i)\neq T(j)$</mark> ...
	1. Avanzo su entrambi i caratteri (*sostituzione*) : 
	   $DP [i][j] = DP [i − 1][j − 1]+1$
	2. Avanzo sul pattern $P$ (*inserimento*) : 
	   $DP [i][j] = DP [i − 1][j ]+1$
	3. Avanzo sul testo $T$ (*cancellazione*) : 
	   $DP [i][j] = DP [i ][j − 1]+1$

La decisione in questo caso sta nel prendere l'operazione più adatta nel caso in cui $P(i)\neq T(j)$. Possiamo semplicemente decidere prendendo quella con lo score pi basso.

Da qui definiamo la formula ricorsiva :

$$
DP[i][j]=\begin{cases}
0 & i=0 \\
i & j=0 &  \\
min(& \text{else}\\DP[i-1][j-1]+\sigma, \\ DP[i-1][j]+1, \\ DP[i][j-1]+1\\) 
\end{cases}
$$

La soluzione del problema è data dal più piccolo valore $DP[m][j]$, per $0 ≤ j ≤ n$ (*ultima riga, qualunque colonna*).

Come vediamo da questo problema <mark class="hltr-red">la soluzione non è sempre all' interno di $DP[n][m]$</mark>, ma può essere dovunque all'interno della tabella.

---

## PROBLEMA - Parentisizzazione ottima
>Data una sequenza di $n$ matrici $A_1, A_2, A_3, . . . , A_n$ compatibili due a due al prodotto, vogliamo calcolare il loro prodotto impiegando il più basso numero possibile di moltiplicazioni scalari. 
>
>Trovare la parentesizzazione ottima del prodotto tra le matrici.

> [!QUOTE] Definizione : Parentesizzazione
> > Una parentesizzazione $P(i,j)$ del prodotto $A_i · A_i+1 · · · A_j$ consiste  ... 
> > - Nella matrice $A_i$, se $i = j$
> > - Nel prodotto di due parentesizzazioni $(P(i,k) · P(k+1,j) )$, altrimenti.
> > 
> >![[EMBED/13_3-p_dinamica.png]]
>
> PDF : [[13_3-p_dinamica.pdf#page=14&selection=6,0,53,14|13_3-p_dinamica, p.12]]



> [!quote] Definizione : Ultimo prodotto
> > La parentesizzazione radice dell'albero delle parentesizzazioni di un prodotto. Ultimo prodotto che viene eseguito durante il calcolo matematico.
> > 
> > ![[EMBED/13_3-p_dinamica 1.png]]
>
> 
> PDF : [[13_3-p_dinamica.pdf#page=14&selection=6,0,53,14|13_3-p_dinamica, p.12]]

^f9d842


> [!QUOTE] Definizione : Parentesizzazione ottima
> > La parentesizzazione che richiede il minor numero di moltiplicazioni scalari per essere completata, fra tutte le parentesizzazioni possibili.
>
> PDF : [[13_3-p_dinamica.pdf#page=15&selection=6,0,7,72|13_3-p_dinamica, p.13]]
> 

Dalla definizione di [[#^f9d842|ultimo prodotto]] ( *Abbreviato u.pr.* ) possiamo dedurre che ...
- Per $n$ matrici si hanno $n-1$ possibili u.pr.
- Ogni possibile u.pr. $P(k)$ divide il prodotto originale in due :
	- $A_1\cdot\dots{}\cdot A_k$
	- $A_{k+1}\cdot\dots{}\cdot A_{n}$ 

A questo punto è facile vedere la divisione in sottoproblemi.

Una partentesizzazione ottima, con ultimo prodotto $P(k)$, sarà composta da due parentesizzazioni ottime per le sotto-sequenze $A_1\dots A_k$ e $A_{k+1}...A_n$.

Definiamo quindi $DP$ e i casi dell'equazione di ricorsione :

>Sia $DP[i][j]$ il minimo numero di moltiplicazioni scalari necessarie per calcolare il prodotto $A[i . . . j]$.

- <mark class="hltr-orange">Caso base $i = j$</mark> : $DP [i][j] = 0$
- <mark class="hltr-purple">Passo ricorsivo per $i\lt j$</mark> : $DP [i][j] = DP [i][k] + DP[k + 1][j] + c_{i−1} · c_k · c_j$


> [!info] Dove ...
> - $k$ posizione dell ultimo prodotto.
> - $c_{i−1} · c_k · c_j$ è il costo per moltiplicare le matrici ...
> 	- $A_i\dots A_k$ : $c_{i−1}$ righe, $c_k$ colonne
> 	- $A_{k+1}\dots A_j$ : $c_k$ righe, $c_j$ colonne
>   


La scelta sta nel selezionare il $k$ che porti al risultato migliore, e possiamo trovarlo semplicemente provando tutti i possibili valori (*$i\le k \le j-1$*) e prendendo il minore dei risultati.

Veniamo quindi alla formula completa ...

$$
\begin{cases}
0 & i=j \\
min_{i\le k \le j-1}(DP [i][j] = DP [i][k] + DP[k + 1][j] + c_{i−1} · c_k · c_j) & i<j
\end{cases}
$$

Il costo della parentesizzazione ottima si trova nella posizione $DP[1][n]$

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Numero di Catalan
I numeri di Catalan sono utili ogni volta che si devono <mark class="hltr-orange">contare strutture ricorsive</mark>, cioè oggetti che possono essere costruite dividendole in una parte sinistra e una destra (*o in due sottoproblemi indipendenti*).

$$C_n=\frac{1}{n+1}\binom{2n}{n} = \frac{1}{n+1} \cdot\frac{2n!}{n!\cdot n!}$$

#### Esempi

I casi più comuni sono:

1. **Alberi binari pieni**
    - Con $n$ nodi interni (oppure $2n+1$ nodi totali), il numero di forme possibili è $C_n$.
2. **Parentesizzazioni**
    - Quanti modi ci sono di mettere le parentesi in un prodotto di $n+1$ fattori?
3. **Sequenze di parentesi corrette**
    - Quante sequenze corrette si possono formare con $n$ coppie di parentesi?
4. **Cammini su una griglia**
    - Numero di percorsi che non superano mai la diagonale.
5. **Triangolazioni di un poligono**
    - Un poligono con $n+2$ lati può essere triangolato in $C_n$ modi.


---


## PROBLEMA - Insieme indipendente di intervalli pesati
> Siano dati $n$ intervalli distinti (*aperti a destra*) $[a_1, b_1) \dots [a_n, b_n)$ della retta reale, dove all’intervallo $i$ è associato un profitto $w_i$.
> 
> Trovare un insieme indipendente di peso massimo (*profitto massimo*).

> [!QUOTE] Intervalli disgiunti
> > Due intervalli $i$ e $j$ si dicono disgiunti se ...
> > $$b_j \le a_i \text{ oppure } b_i \le a_j$$
>
> PDF : [[13_3-p_dinamica.pdf#page=41&selection=51,0,77,1|13_3-p_dinamica, p.39]]


In questo caso per applicare dinamica è necessario (*o quantomeno estremamente più efficente*) ordinare gli intervalli per **estremi finali non decrescenti** : $b_1 \le b_2 \le \dots \le b_n$.

Potremmo fermarci quà, ma è anche utile pre-calcolare il *predecessore* di un intervallo. Cioè il primo intervallo "compatibile" con l'intervallo in esame.

Il predecessore di $i$, $pred_i = j$, quindi ha che ...
- $j < i$
- $j$ è il massimo indice tale che $b_j \le a_{i}$

Se non esiste nessun predecessore si ha che $pred_i=0$.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Possiamo gia quindi definire la formula ricorsiva :

$$
\begin{cases}
0 & i=0 \\
max(DP [i − 1], DP[pred_i] + w_i) & i\gt 0
\end{cases}
$$