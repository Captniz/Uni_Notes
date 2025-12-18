---
Date created: 17-12-25 • 14:18
tags:
  - Databases
Related PDF/DOC:
  - "[[Updated_11 Algoritmi JOIN e costi.pdf]]"
Related Pages:
  - "[[9b - Modello di costo]]"
---
## Costo del JOIN

E' importante considerare i costi per gli **algoritmi di JOIN**, dato che queste sono le query al contempo più frequenti e più complesse da eseguire.



> [!info] Specifica  
> In questi appunti faremo riferimento al `JOIN` tra due tabelle `R(A,B)` e `S(C,D)` con condizione `B = C`.
> 
> <hr style="width: 70%; margin-left: auto;margin-right: auto;">
>
>
![[EMBED/Updated_11 Algoritmi JOIN e costi.png]]
[[Updated_11 Algoritmi JOIN e costi.pdf#page=2&rect=13,10,683,224|Updated_11 Algoritmi JOIN e costi, p.2]]

Per calcolare il costo di un `JOIN` si devono fare alcune specifiche : 
1. **ll costo del JOIN** è il numero di pagine che l’algoritmo dovrà leggere o scrivere per calcolare il risultato del JOIN.
2. Le tuple prodotte dall'algoritmo vengono mostrate a video all’utente e **non riscritte su disco** (*quindi* <mark class="hltr-orange">non avremo costi legati alla scrittura</mark> *del risultato su disco*).

---
### NLJ - Nested Loop Join
>L’algoritmo più semplice e immediato per eseguire un JOIN.

L'NLJ si basa su due cicli : 

```py
foreach tuple t in R do: 
	foreach tuple t1 in S do: 
		if t.B = t1.C 
			output (t,t1)
```
 
 Questo algoritmo dovrà leggere per $R$ volte $1+|S|$ tuple, ovvero $|R| + |R|\cdot|S|$ tuple.

Tuttavia il <mark class="hltr-red">DBMS non legge singole tuple</mark>, ma intere pagine che contengono un certo numero di tuple.

Quindi la versione più realistica del NLJ è :
```py
foreach page P in R do: 
	foreach page P1 in S do: 
		 per ogni coppia di tuple t ϵ P, t’ ϵ P’
			 if t.B = t1.C 
				 output (t,t1)
```

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Il costo di questo algoritmo è quindi ...
$$\text{Costo}_{\text{NLJ}}=P_R+P_R\cdot P_S$$

Che possiamo notare essere estremamente alto.

> [!error] Ordine delle tabelle
> 
Da notare che l’ordine delle tabelle conta : 
<mark class="hltr-red">Conviene sempre usare la tabella con cardinalità minore come outer table</mark>.

#### Indexed NLJ
> E’ una variante del NLJ in cui si assume che sia presente un indice sulla **inner** table.

Questo algoritmo infatti legge tutte le pagine della outer table $P_R$ e poi cerca tramite indice le tuple di $S$ che soddisfano la condizione del JOIN.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Il costo quindi si riduce a...

$$
\text{Costo}_{\text{INLJ}} = P_R + |R| \cdot \text{Costo}_{\text{eq-search}}(S)
$$

Il costo dell'Eq. search dipende dall'indice

---

### Sort-Merge JOIN
>Questo algoritmo assume che le $R$ e $S$ siano ordinate rispetto ai valori degli attributi su cui intendiamo fare il JOIN.

Al contrario del precedente questo algoritmo è molto più efficiente : 
1. Prendiamo il primo valore di $B$ in $R$. 
2. Iniziamo a scorrere i valori di $C$ in $S$ : 
    - Se troviamo lo stesso valore (*anche più volte*), stampiamo la nuova tupla (*o tuple*).
    - Se troviamo un valore più alto, ripetiamo il processo invertendo il ruolo di $R$ e $S$.



> [!example]- Esempio di svolgimento di Merge JOIN
> [[Updated_11 Algoritmi JOIN e costi.pdf#page=9|Updated_11 Algoritmi JOIN e costi, p.9]]

In questo algoritmo dobbiamo considerare solo $|R|+|S|$, dato che non abbiamo mai dovuto leggere due volte la stessa tupla.

In termini di pagine, il merge JOIN richiede solo di leggere $P_R + P_S$ pagine. Tuttavia, il prezzo da pagare è quello di <mark class="hltr-orange">ordinare le tuple di $R$ e $S$</mark> che può risultare piuttosto costoso.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Tutto considerato il costo di questo algoritmo equivale a ...

$$
\text{Costo}_{\text{sort-merge}}=\text{Costo}_{\text{sort R}} + \text{Costo}_{\text{sort S}} + P_R + P_S
$$

Sebbene quest'algoritmo sia generalmente più veloce, talvolta è comunque utile considerare l'NLJ, con le dovute ottimizzazione.

---
### Hash JOIN
> Utilizza hash tables per velocizzare il JOIN di due tabelle.

L’idea è di usare una funzione di hashing sui valori degli attributi di $R$ e $S$ usati nel JOIN per dividere le tuple delle due tabelle in *bucket*.

Se $t$ e $t’$ possono essere messe in JOIN, allora $t.B = t.C$, dato che hanno lo stesso hash.


> [!error] Eccezioni degli hash
> Non è sempre vero <mark class="hltr-red">che se due valori hanno lo stesso hash, allora essi sono uguali!</mark>

L’algoritmo dopo aver creato i bucket procede come segue: 
1. Legge e carica in memoria il primo bucket di $R$ e il primo bucket di $S$.
2. Calcola (*in RAM*) i JOIN delle tuple nei due bucket.
3. Passa al successivo bucket di $R$ e di $S$.

Le tuple che sono in <mark class="hltr-red">un bucket di $R$ possono andare in JOIN solo con tuple che stanno nel bucket corrispondente di $S$</mark>, questo perché tutte le tuple con lo stesso valore per un attributo sono <mark class="hltr-orange">nello stesso bucket</mark>.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Il costo totale infine risulta :

$$
\text{Costo}_{\text{hash}} = (P_R+2\cdot|R|) + (P_S+2\cdot|S|)+(P_R+P_S)
$$


Ovviamente, se le tuple sono già distribuite in bucket il costo sarà semplicemente $P_R + P_S$.

## Ottimizzazione degli indici

Da questi algoritmi possiamo vedere come sia fondamentale l'ottimizzazione degli indici sui dati, in modo da ottenere la **miglior performance possibile**.

Dobbiamo quindi valutare in modo dinamico *i requisiti delle query* più comuni nel nostro DB.

Alcuni motivi per rivisitare le scelte iniziali degli indici:
- Alcune query possono richiedere troppo tempo per la mancanza di un indice.
- Alcuni indici possono risultare poco utilizzati.
- Alcuni indici possono essere soggetti ad aggiornamenti troppo frequenti.

