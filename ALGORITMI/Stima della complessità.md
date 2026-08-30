---
Date created: 16-09-25 • 11:48
tags:
  - Algoritmi
Related PDF/DOC:
  - "[[02-analisi.pdf]]"
Related Pages:
---
## Definizione della complessità
La **complessità** viene definita come 

$$
Dimensione\ input \to Tempo
$$

In particolare ci sono diversi *criteri* per definire la <mark class="hltr-purple">dimensione dell'input</mark> :

- **Costo logaritmico** : La taglia dell’input è il numero di bit necessari per rappresentarlo.
- **Costo uniforme** : La taglia dell’input è il numero di elementi di cui è costituito.

Possiamo assumere che gli *"elementi"* siano rappresentati da un numero costante di bit.

---

Il <mark class="hltr-blue">tempo</mark> invece è definito come **numero di istruzioni elementari** ( *Come visto [[1_Introduzione agli algoritmi#Efficienza|nell'analisi dell'efficienza]]* ). 

In particolare un’istruzione si considera <mark class="hltr-orange">elementare</mark> se può essere eseguita in tempo *"costante"* dal processore.

### Modelli di calcolo
Un **modello di calcolo** è una rappresentazione astratta di un calcolatore; necessaria per definire cos'è un <mark class="hltr-purple">operazione elementare</mark>, in quanto dobbiamo definire che operazioni vengono <mark class="hltr-orange">eseguite in tempo costante</mark>.

In particolare si hanno tre caratteristiche di questi modelli:

- **Astrazione** : Deve permettere di nascondere i dettagli.
- **Realismo** : Deve riflettere la situazione reale.
- **Potenza matematica** : deve permettere di trarre conclusioni "formali" sul costo.


> [!example] Esempio del modello di calcolo che useremo
> ![[EMBED/02-analisi.png]]
>
> [[02-analisi.pdf#page=10&rect=9,74,355,226|02-analisi, p.8]]

### Procedimento per il calcolo della complessità

Per calcolare la complessità si segue questo processo :

![[EMBED/02-analisi 1.png]]
[[02-analisi.pdf#page=11&rect=2,22,350,259|02-analisi, p.9]]

---

( *Un caso più complesso si trova a [[02-analisi.pdf#page=12|pagina 10 del PDF]]* )

### Ordini di complessità
Per classificare la complessità degli algoritmi si usano gli **ordini di complessità**.

---
![[EMBED/02-analisi 2.png]]
[[02-analisi.pdf#page=16&rect=24,99,339,217|02-analisi, p.14]]

 ---
#### Notazione della complessità

Diamo innanzitutto un occhiata alle notazioni che useremo per determinare la complessità :

> [!QUOTE] Definizione – Notazione O
> > Sia $g(n)$ una funzione di costo; indichiamo con $O(g(n))$ l’insieme delle funzioni $f(n)$ tali per cui: 
> > $$∃c > 0, ∃m ≥ 0 : f (n) ≤ cg(n), ∀n ≥ m$$
>
> PDF : [[02-analisi.pdf#page=18&selection=18,0,77,1|02-analisi, p.15]]

O in parole più semplici ... 
>Abbiamo $g(n)$ che è la nostra funzione di costo che fa da <mark class="hltr-orange">limite asinotico superiore</mark> per la nostra serie di funzioni $f(n)\in O(g(n))$. 
>
>Cioè $f(n)$ cresce <mark class="hltr-red">al più</mark> come $g(n)$.

---

Agiscono in modo anche le due altre notazioni :  

> [!QUOTE] Definizione – Notazione $\Omega$
> > Sia $g(n)$ una funzione di costo; indichiamo con $Ω(g(n))$ l’insieme delle funzioni $f(n)$ tali per cui: 
> > $$∃c > 0, ∃m ≥ 0 : f (n) ≥ cg(n), ∀n ≥ m$$
>
> PDF : [[02-analisi.pdf#page=19&selection=18,0,76,1|02-analisi, p.16]]

> [!QUOTE] Definizione - Notazione $\Theta$
> > Sia $g(n)$ una funzione di costo; indichiamo con $Θ(g(n))$ l’insieme delle funzioni $f(n)$ tali per cui: 
> > $$∃c1 > 0, ∃c2 > 0, ∃m ≥ 0 : c1g(n) ≤ f (n) ≤ c2g(n), ∀n ≥ m$$
>
> PDF : [[02-analisi.pdf#page=20&selection=18,0,99,1|02-analisi, p.17]]



> [!example]- Esempio di calcolo di un O(n)
> ![[02-analisi.pdf#page=22|02-analisi, p.19]]
> [[02-analisi.pdf#page=22|02-analisi, p.19]]

#### Complessità dei problemi
Fino adesso abbiamo visto la complessità legata agli algoritmi, cioè alle <mark class="hltr-purple">soluzioni</mark>. Tuttavia possiamo applicare la nozione di complessità anche ai <mark class="hltr-orange">problemi</mark> :

> [!QUOTE] Complessità O nei problemi
> > Un problema ha complessità $O(f (n))$ se esiste almeno un algoritmo che ha complessità $O(f (n))$.
> 
> PDF : [[02-analisi.pdf#page=38&selection=18,0,37,2|02-analisi, p.32]]
> 

> [!QUOTE] Complessità $\Omega$ nei problemi
> > Un problema ha complessità $Ω(f (n))$ se tutti i possibili algoritmi che lo risolvono hanno complessità $Ω(f (n))$.
>
>
> PDF : [[02-analisi.pdf#page=39&selection=17,0,35,1|02-analisi, p.33]]
> 

#### Riassunto della complessità
---
![[EMBED/02-analisi 3.png]]
[[02-analisi.pdf#page=51&rect=6,9,354,232|02-analisi, p.45]]

---

## Sorting
Studieremo i sorting per vedere come in alcuni algoritmi si comportino diversamente a seconda delle caratteristiche dell’input.

Per i sort si analizza il **caso pessimo** per studiarne la complessità; cioè si studia l'input (*indipendentemente dalla dimensione di questo*) per cui il <mark class="hltr-orange">tempo di esecuzione è il maggiore rispetto a ogni altro input</mark>.

### Selection sort
Il selection sort è un algoritmo particolare in cui il tempo di esecuzione è uguale per il caso <mark class="hltr-green">ottimo</mark>, <mark class="hltr-blue">medio</mark> e <mark class="hltr-orange">pessimo</mark>.

---
![[EMBED/02-analisi 4.png]]
[[02-analisi.pdf#page=58&rect=3,115,360,231|02-analisi, p.51]]

---
### Insertion sort
L'insertion sort è un algoritmo efficiente per ordinare **piccoli insiemi di elementi**. Si basa sul principio di ordinamento di una "mano" di carte da gioco.

---
![[EMBED/02-analisi 5.png]]
[[02-analisi.pdf#page=59&rect=8,39,357,172|02-analisi, p.52]]

---

In questo caso il costo di esecuzione non dipende solo dalla dimensione ma anche dalla **distribuzione dei dati in ingresso**.
### Merge sort
L'algoritmo merge sort è basato sulla tecnica *divide-et-impera*.

---
![[EMBED/02-analisi 6.png]]
[[02-analisi.pdf#page=68&rect=5,32,361,224|02-analisi, p.59]]

---

La complessità dell'algoritmo merge sort è **$O(n)$**.
