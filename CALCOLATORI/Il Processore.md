---
Date created: 06-05-25 • 13:50
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250506_L16-Processore_1.pdf]]"
Related Pages: 
---
Nell’ esecuzione delle istruzioni, a prescindere dall’[[Assembly#ISA|ISA]], vi sono tratti comuni. Le prime due fasi, per ogni istruzione, sono : 
- **Prelievo dell’istruzione** dalla memoria. 
- **Lettura del valore di uno o più registri** operandi che vengono estratti direttamente dai campi dell’istruzione.

<mark class="hltr-red">Tutte</mark> le istruzioni usano la **ALU** ( __A__*rithmetic* __L__*ogic* __U__*nit* ) dopo aver letto gli operandi:
- Le istruzioni di **accesso alla memoria** <mark class="hltr-purple">richiedono</mark> o <mark class="hltr-orange">salvano</mark> il dato in memoria.
- Le istruzioni **aritmetiche/logiche** <mark class="hltr-purple">memorizzano</mark> il risultato nel registro target.
- Le istruzioni di **salto** condizionato <mark class="hltr-purple">cambiano il PC</mark> secondo l’esito del confronto.