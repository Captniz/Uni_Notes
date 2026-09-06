---
Date created: 04-09-26 • 10:46
tags:
  - Algoritmi
Related PDF/DOC:
  - "[[15-local.pdf]]"
Related Pages:
---
# Ricerca locale
> Se si conosce una soluzione ammissibile (*non ottima*) ad un problema di ottimizzazione, si può cercare una soluzione migliore nelle *"vicinanze"* di quella precedente.

Questa tecnica potrebbe però portare a <mark class="hltr-blue">soluzioni ottime locali</mark>
## Reti di flusso
### Teoria
>Una rete di flusso $G = (V, E, s, t, c)$ è data da: 
>- Un grafo orientato $G = (V, E)$ 
>- Un nodo $s ∈ V$ detto <mark class="hltr-orange">sorgente</mark> 
>- Un nodo $t ∈ V$ detto <mark class="hltr-purple">pozzo</mark> 
>- Una funzione di capacità $c : V × V → R^{≥0}$, tale che $(x, y) \not∈ E ⇒ c(x, y) = 0$.
>
>Si assume inoltre che :
>- Per ogni nodo $x ∈ V$ , esiste un cammino $s ⇝ x ⇝ t$ da $s$ a $t$ che passa per $x$.  

> [!QUOTE] Definizione : Flusso
> > Un flusso in $G$ è una funzione $f : V ×V → R$ che soddisfa le seguenti proprietà: 
> > - Vincolo sulla capacità: $∀x, y ∈ V , f (x, y) ≤ c(x, y)$ 
> > - Antisimmetria: $∀x, y ∈ V , f (x, y) = −f (y, x)$ 
> > - Conservazione del flusso: $∀x ∈ V − \{s, t\}, \sum_{y∈V}  f (x, y) = 0$
>
> PDF : [[15-local.pdf#page=7&selection=8,0,102,5|15-local, p.5]]
>  

> [!info]- Spiegazione informale sille proprietà dei flussi
> 
> Il **vincolo sulla capacità** impone un vincolo $c(x,y)$ sulla capacità dell'arco.
> 
> 
> > [!example] Vincolo sulle capacità
> > ![[EMBED/15-local 1.png]]
> > [[15-local.pdf#page=8&rect=113,105,251,177|15-local, p.6]]
> 
> ---
> 
> L'**antisimmetria** dice che il flusso che attraversa un arco in direzione $(x, y)$ è l’opposto del flusso che attraversa l’arco in direzione $(y, x)$.
> 
> 
> > [!example] Antisimmetria
> > ![[EMBED/15-local 2.png]]
> > [[15-local.pdf#page=9&rect=108,78,255,164|15-local, p.7]]
> 
> ---
> 
> La **conservazione del flusso** ci dice che per ogni nodo, *diverso da sorgente e pozzo*, la somma dei flussi entranti deve essere uguale alla somma dei flussi uscenti.  
> 
> 
> > [!example] Conservazione del flusso
> > ![[EMBED/15-local 3.png]]
> 
> [[15-local.pdf#page=10&rect=100,17,264,171|15-local, p.8]]
> 


Il valore di un flusso $f$ è definito come ...
$$
|f| = \sum_{(s,x)\in E}f(s,x)
$$

ovvero come la quantità di flusso uscente da $s$.  

Definiamo inoltre ...

**Flusso massimo** :
$|f^*| = max(|f|)$ è il flusso (*di valore*) massimo tra tutti i flussi possibili nella rete. 

**Flusso nullo** :
$f_{0} : V × V → R^{≥0}$  tale che  $∀x, y ∈ V : f (x, y) = 0$  è detto *flusso nullo*. ($|f|=0$)


**Flusso somma** :
$g = f_1 + f_2$ tale che $∀x, y ∈ V : g(x, y) = f_1(x, y) + f_2(x, y)$ è detto *flusso somma*. 

**Capacità residua** :
$c_f : V × V → R^{≥0}$ tale che $∀x, y ∈ V : c_f (x, y) = c(x, y) − f (x, y)$ è detto *flusso residuo*.  

La capacità residua per definizione crea degli archi all'indietro (*se non esistenti*) con capacità massima $0$.
  
**Rete residua** :
Una *rete residua* è una rete $G_f = (V, E_f , s, t, c_f )$, tale per cui: $∀x, y ∈ V : (x, y) ∈ E_f ⇔ c_f (x, y) > 0$  

Possiamo inoltre dire che se $f$ è un flusso in $G$ e $g$ è un flusso in $G_f$ , allora $f + g$ è un flusso in $G$. *A $f+g$ si applicano quindi tutte le proprietà dei flussi*.
  

> [!example]- DIMOSTRAZIONE : Applicazioni delle proprietà dei flussi a $f+g$ 
> [[15-local.pdf#page=22|15-local, p.20]]



<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Algoritmo
Possiamo provare che esiste un algoritmo generale per la soluzione del problema :

> [!example] Algoritmo base per il flusso massimo
> ![[EMBED/15-local 4.png]]
> [[15-local.pdf#page=15&rect=9,82,353,229|15-local, p.13]]
> 
> ---
> 
> ![[EMBED/15-local 5.png]]
>
>[[15-local.pdf#page=21&rect=10,89,354,223|15-local, p.19]]

Bisogna quindi capire <mark class="hltr-orange">come trovare un flusso aggiuntivo $g$</mark>.
Si fa questo attraverso l'algoritmo dei *cammini aumentati* ...

Sia un cammino $C = v_0 \dots  v_n$ , con $s = v_0 e t = v_n$ nella rete residua $G_f$.
Su questo cammino troviamo la **capacità del cammino** $c_f(C)$, cioè la capacità minore tra quelle degli archi incontrati (*collo di bottiglia*).

Per trovare il cammino si esegue una visita in ampiezza con costo $O(V+E)$.

Creiamo ora un flusso addizionale $g$ tale che ...
- $g(v_{i−1}, v_i) = c_f (C)$
- $g(v_i, v_{i−1}) = −c_f (C)$ (*antisimmetria*) 
- $g(x, y) = 0$ per tutte le altre coppie $(x, y)$  


> [!example] Flusso $g$
> ![[EMBED/15-local 6.png]]
>[[15-local.pdf#page=27&rect=58,21,315,132|15-local, p.22]]

Per la proprietà della capacità residua vengono a formarsi degli <mark class="hltr-red">archi inversi utilizzabili</mark>.


> [!example] Archi inversi utilizzabili
> ![[EMBED/15-local 7.png]]
> [[15-local.pdf#page=34&rect=14,91,278,226|15-local, p.24]]
>
> ---
> 
> ![[EMBED/15-local 8.png]]
>
> [[15-local.pdf#page=37&rect=18,89,278,218|15-local, p.24]]



> [!example] TEOREMA - Dimostrazione di correttezza dell'algoritmo
> [[15-local.pdf#page=42|15-local, p.28]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Considerando capacità intere, l'algoritmo per i cammini aumentanti ha complessità $O((V + E)|f^∗|)$ (*liste*) o $O(V^2|f^∗|)$ (*matrice*), poichè ...

- L’algoritmo parte dal flusso nullo e termina quando il valore totale del flusso raggiunge $|f^∗|$
- Ogni incremento del flusso aumenta il flusso di almeno un’unità
- Ogni ricerca di un cammino richiede una visita del grafo, con costo $O(V + E)$ o $O(V^2$) 
- La somma dei flussi e il calcolo della rete residua può essere effettuato in tempo $O(V + E)$ o $O(V^2)$  

Inoltre dobbiamo aggiungere il costo della visita in ampiezza, che considerando le stesse condizione ha complessità $O(VE^2)$.

A questo punto prendiamo il limite superiore minore tra i due.


> [!example] TEOREMA : Dimostrazione della complessità
> [[15-local.pdf#page=60|15-local, p.41]]
