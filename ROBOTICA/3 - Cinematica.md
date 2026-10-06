---
Date created: 02-10-26 • 10:12
tags:
  - Robotica
Related PDF/DOC:
  - "[[3-kinematica.pdf]]"
Related Pages:
---
## Cinematica dei lavoratori

Un manipolatore è costituito da una serie di corpi rigidi chiamati **link**, l'intera struttura forma una *catena cinematica*.  
  
Una catena cinematica è caratterizzata da un numero di gradi di libertà,ognuno associato ad un giunto, che determinano <mark class="hltr-orange">univocamente</mark> la sua postura.
  
L'obiettivo della cinematica diretta è trovare la posa dell'effettore date le variabili di giunto.

### Cinematica diretta
Sia un manipolatore ...

> [!example] Esempio cinematica diretta
> ![[EMBED/3-kinematica 1.png]]
[[3-kinematica.pdf#page=6&rect=57,85,163,172|3-kinematica, p.6]]
>

 Possiamo rappresentare la trasformazione del manipolatore dal sistema base $O_b-x_by_bz_b$ alla posizione finale tramite una matrice di trasformazione omogenea :
 $$T^b_e(q)=
\begin{bmatrix}
R^b_e(q) & p^b_e(q)\\
0 & 1
\end{bmatrix}=
\begin{bmatrix}
n^b_e(q) & s^b_e(q) & a^b_e(q) & p^b_e(q)\\
0 & 0 & 0 & 1
\end{bmatrix}$$

dove ...
- $q$ : Vettore contenente le variabili delle giunture.
- $n_e, s_e, a_e$ : Vettori unitari associati al sistema legato all'effettore e sono scelti tenendo conto della sua geometria.


> [!example] Esempio di scelta degli assi $n_e, s_e, a_e$
> Se l'effettore è una pinza a due dita ...
> - $a_e$ è la direzione di avvicinamento
> - $s_e$ è la direzione di scorrimento e
> - $n_e$ è scelto ortogonale agli altri due in modo che il riferimento $(n_e,s_e,a_e)$ sia destrorso.

