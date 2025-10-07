---
Date created: 23-09-25 • 11:25
tags:
Related PDF/DOC:
  - "[[04-funzioni.pdf]]"
Related Pages:
---
## Caratteristiche e proprietà della notazione asintotica

### Regola generale della notazione asintotica

Nella notazione asintotica, il limite inferiore e superiore, sono definiti dal <mark class="hltr-orange">grado maggiore nell'espressione</mark> : 


> [!example] Dimostrazione per il limite superiore
> 
![[EMBED/04-funzioni.png]]
[[04-funzioni.pdf#page=19&rect=3,19,358,220|04-funzioni, p.14]]


### Proprietà della notazione


> [!warning]- Propietà dei logaritmi
> Pur non essendo direttamente una propietà della notazione, le propietà degli algoritmi sono ugualmente importanti.
>  ![[EMBED/04-funzioni 7.png]]
> [[04-funzioni.pdf#page=28&rect=4,109,360,227|04-funzioni, p.22]]
> ---
> ![[EMBED/04-funzioni 8.png]]
>[[04-funzioni.pdf#page=29&rect=5,11,360,223|04-funzioni, p.22]]


#### Dualità
La **Dualità**, espressa con la formula :

$$
f(n) = O(g(n)) \iff g(n) = \Omega(O(n))
$$


> [!info]- Dimostrazione
> ![[EMBED/04-funzioni 1.png]]
>[[04-funzioni.pdf#page=22&rect=58,44,309,146|04-funzioni, p.16]]


#### Eliminazione delle costanti
L' **Eliminazione delle costanti**, espressa con la formula :
$$
f(n) = O(g(n)) \iff af(n) = O(g(n)), \forall a>0
$$

Questa vale anche per $\Omega$.


> [!info]- Dimostrazione
>![[EMBED/04-funzioni 2.png]]
>[[04-funzioni.pdf#page=23&rect=43,46,333,124|04-funzioni, p.17]]

#### Sommatoria
La **Sommatoria** o **Sequenza di algoritmi**, espressa con la formula :
$$
f_1(n) = O(g_1(n)),\ f_2(n) = O(g_2(n)) \Rightarrow  f_1(n)+f_2(n)=O(max(g_1(n),g_2(n)))
$$

Questa vale anche per $\Omega$.


> [!info]- Dimostrazione
>![[EMBED/04-funzioni 3.png]]
> [[04-funzioni.pdf#page=24&rect=10,15,356,136|04-funzioni, p.18]]


%%TODO: spiega meglio le prop%%
#### Prodotto
Il **Prodotto** o **Cicli annidati**, espressa con la formula :
$$
f_1(n) = O(g_1(n)),\ f_2(n) = O(g_2(n)) \Rightarrow  f_1(n)*f_2(n)=O(g_1(n)*g_2(n))
$$

Questa vale anche per $\Omega$.


> [!info]- Dimostrazione
> ![[EMBED/04-funzioni 4.png]]
> [[04-funzioni.pdf#page=25&rect=8,45,358,138|04-funzioni, p.19]]


#### Simmetria
La **Simmetria** , espressa con la formula :
$$
f(n) = \Theta(g(n)) \iff g(n) = \Theta(f(n))
$$


> [!info]- Dimostrazione
> ![[EMBED/04-funzioni 5.png]]
> [[04-funzioni.pdf#page=26&rect=11,60,354,161|04-funzioni, p.20]]

#### Transitività

La **Trasnitività** , espressa con la formula :
$$
f(n) = O(g(n)),g(n) = O(h(n)) \Rightarrow f(n) = O(h(n))
$$

>[!info]- Dimostrazione
> ![[EMBED/04-funzioni 6.png]]
> [[04-funzioni.pdf#page=27&rect=11,61,354,160|04-funzioni, p.21]]

%%TODO: PP 24%%

## Metodi per la risoluzione delle ricorrenze

![[EMBED/04-funzioni 9.png]]
### Analisi per livelli
### Metodo della sostituzione

> [!QUOTE] Processo del metodo della sostituzione
> > È un metodo in cui si cerca di *“indovinare”* una soluzione, in base alla propria esperienza, e si dimostra che questa soluzione è corretta tramite induzione.
>
> PDF : [[04-funzioni.pdf#page=57&selection=16,0,18,27|04-funzioni, p.35]]

### Metodo esperto

---
![[EMBED/04-funzioni 10.png]]
[[04-funzioni.pdf#page=95&rect=10,32,354,219|04-funzioni, p.49]]

---