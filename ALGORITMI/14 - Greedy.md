---
Date created: 30-08-26 • 23:16
tags:
  - Algoritmi
Related PDF/DOC:
  - "[[14-greedy.pdf]]"
Related Pages:
---
La filosofia degli algoritmi greedy consiste nel <mark class="hltr-orange">selezionare l'unica decisione che appare ottima</mark> (*localmente ottima*) ogni ciclo. Tuttavia è necessario **dimostrare** che seguendo questo metodo si ottiene un ottimo globale.

La tecnica greedy si può solitamente applicare quando ...
- E' possibile dimostrare che esiste una scelta greedy :
  > Fra le molte scelte possibili, ne può essere facilmente individuata una che porta sicuramente alla soluzione locale ottima.
- Il problema ha sottostruttura ottima :
  > Fatta tale scelta, resta un sottoproblema con la stessa struttura del problema principale.

## PROBLEMA - Insieme indipendente massimale di intervalli 
> Siano dati $n$ intervalli distinti (*aperti a destra*) $[a_1, b_1) \dots [a_n, b_n)$ della retta reale, dove all’intervallo $i$ è associato un profitto $w_i$.
> 
> Trovare un insieme indipendente massimale, cioè un sottoinsieme di massima cardinalità formato da intervalli disgiunti tra loro.

[[13 - Programmazione Dinamica#PROBLEMA - Insieme indipendente di intervalli pesati|Soluzione di problema simile attraverso programmazione dinamica]]

