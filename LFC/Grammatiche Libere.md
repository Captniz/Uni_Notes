---
Date created: 23-09-26 • 16:50
tags:
  - LFC
Related PDF/DOC:
  - "[[2026_3_liberi_2p.pdf]]"
Related Pages:
---
## Proprietà delle grammatiche libere
### Lemma 1 - Non-terminal Play Variables

> Sia $\mathcal G$ una grammatica e sia $\mathcal G'$ ottenuta da $\mathcal G$ cambiando le lettere di tutti i suoi non-terminali. Allora $\mathcal L(\mathcal G') = \mathcal L(\mathcal G)$.


> [!example] Esempio
> ![[EMBED/2026_3_liberi_2p.png]]
[[2026_3_liberi_2p.pdf#page=2&rect=145,146,471,255|2026_3_liberi_2p, p.3]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Lemma 2 - Closure wrt union

> La classe delle grammatiche libere è <mark class="hltr-orange">chiusa rispetto all'unione</mark>.


Cioè ...
Siano $\mathcal L_{1}$ e $\mathcal L_{2}$ due linguaggi liberi e sia $\mathcal L_{3} = \mathcal L_{1} \cup \mathcal L_{2}$.
$\mathcal L_{3}$ è un linguaggio libero.



> [!important]- Dimostrazione
> ![[EMBED/2026_3_liberi_2p 1.png]]
[[2026_3_liberi_2p.pdf#page=3&rect=152,143,450,282|2026_3_liberi_2p, p.5]]
<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Lemma 3 - Closure wrt concatenation

> La classe delle grammatiche libere è <mark class="hltr-orange">chiusa rispetto alla concatenazione</mark>.

Cioè ...
Siano $\mathcal L_{1}$ e $\mathcal L_{2}$ due linguaggi liberi e sia $L_{3}=\{w_{1}w_{2}\ |\ w_{1}\in\mathcal L_{1}, w_{2}\in\mathcal L_{2} \}$.
$\mathcal L_{3}$ è un linguaggio libero.


> [!important]- Dimostrazione
> ![[EMBED/2026_3_liberi_2p 2.png]]
[[2026_3_liberi_2p.pdf#page=6&rect=152,146,448,287|2026_3_liberi_2p, p.11]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Lemma 4 - Forma normale di Chomsky
> Sia $\mathcal L$ un linguaggio libero e sia $\mathcal G$ una grammatica libera tale che $\mathcal L(\mathcal G) = \mathcal L - \{\epsilon\}$. Inoltre sia ha che $\mathcal G$ ...
> - Non ha $\epsilon\text{-produzione}$ ( *Non ha produzioni in forma $X\to\epsilon$* ).
> - Non ha produzioni unitarie ( *Non ha produzioni in forma $X\to Y$* ).
> - Non ha non-terminali inutili ( *Cioè terminali che non appaiono in mai in alcune derivazioni di alcune stringhe* )
> - Ogni sua produzione ha forma $X\to x$ o $X\to X_{1}X_{2}$



> [!important] Passaggi per la creazione della forma normale partendo da una grammatica normale
> [[2026_3_liberi_2p.pdf#page=7|2026_3_liberi_2p, p.13]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Lemma 5 - Pumping lemma
> Sia $\mathcal L$ un linguaggio libero. Allora ...
> - $\exists p\in\mathbb N^+\mid  p< \lvert z\rvert\ ,\ \forall z\in\mathcal L$
> - $\exists u,v,w,x,y\ \text{ tale che ... }$
> 	- $z=u\cdot v\cdot w\cdot x\cdot y$
> 	- $|vwx| \le p$
> 	- $|vx|\gt0$
> 	- $\forall i\in\mathbb N. uv^iwx^iy\in\mathcal L$



> [!important]- Dimostrazione
>[[2026_3_liberi_2p.pdf#page=13|2026_3_liberi_2p, p.25]]

