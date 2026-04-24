---
Date created: 18-03-26 • 22:42
tags:
  - Algoritmi
Related PDF/DOC:
  - "[[Montresor.pdf]]"
  - "[[13_1-p_dinamica.pdf]]"
  - "[[13_2-p_dinamica.pdf]]"
  - "[[13_3-p_dinamica.pdf]]"
Related Pages:
---
Una filosofia simile al **divide-et-impera**, ma portata all’estremo, si basa sulla massima:

>  “Il pesce grosso mangia tutti i pesci più piccoli”.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Rispetto al divide-et-impera, la programmazione dinamica presenta tre grandi differenze: 
1. E' iterativa e non ricorsiva.
2. Affronta i sottoproblemi *“dal basso verso l’alto”* anziché *“dall’alto verso il basso”*.
3. Memorizza i risultati dei sottoproblemi in una tabella. 

Comunque, mentre il divide-et-impera risulta vantaggioso quando i
sottoproblemi più piccoli da risolvere sono indipendenti, <mark class="hltr-orange">la programmazione dinamica è conveniente quando i sottoproblemi non sono indipendenti, ma ne condividono altri</mark>.

Procedendo dal basso verso l’alto, <mark class="hltr-blue">la programmazione dinamica risolve un sottoproblema comune una sola volta</mark>, mentre il divide-et-impera lo risolverebbe più volte.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Perché sia applicabile, occorre che:
1. Sia possibile combinare le soluzioni dei sottoproblemi per trovare la soluzione di un problema più grande.
2. Le decisioni prese per risolvere in modo ottimo un sottoproblema rimangano valide quando il sottoproblema diviene un “pezzo” di un problema più grande.
3. Una tecnica divide-et-impera richieda di risolvere più volte gli stessi sottoproblemi e sia pertanto inutilizzabile da un punto di vista computazionale.

Le proprietà (1) e (2) prendono il nome di ***sottostruttura ottima***.

Inoltre, affinché un algoritmo basato sulla programmazione dinamica abbia complessità polinomiale, occorre che :

4. Ci sia un numero polinomiale di sottoproblemi da risolvere.
5. Per evitare di risolvere più di una volta lo stesso sottoproblema, si usi una tabella in cui si memorizzano le soluzioni di tutti i sottoproblemi, senza preoccuparsi se la soluzione di un particolare sottoproblema verrà poi utilizzata oppure no.
6. Il tempo per combinare le soluzioni dei sottoproblemi e trovare la soluzione del problema più grande sia polinomiale.