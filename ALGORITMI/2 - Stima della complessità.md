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

Il <mark class="hltr-blue">tempo</mark> invece è definito come **numero di istruzioni elementari** ( *Come visto [[1 - Introduzione agli algoritmi#Efficienza|nell'analisi dell'efficienza]]* ). 

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
Per classificare la complessità degli algoritmi si usano gli **ordini di complessità**; questi vengono espressi con la <mark class="hltr-orange">notazione asintotica</mark>.

---
![[EMBED/02-analisi 2.png]]
[[02-analisi.pdf#page=16&rect=24,99,339,217|02-analisi, p.14]]

---

> [!QUOTE] Definizione – Notazione O
> > Sia $g(n)$ una funzione di costo; indichiamo con $O(g(n))$ l’insieme delle funzioni $f(n)$ tali per cui: 
> > $$∃c > 0, ∃m ≥ 0 : f (n) ≤ cg(n), ∀n ≥ m$$
>
> PDF : [[02-analisi.pdf#page=18&selection=18,0,77,1|02-analisi, p.15]]

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

%%
- La $c$ serve per rendere comparabili i risultati?
- $log_2n$ nelle slide
%%
