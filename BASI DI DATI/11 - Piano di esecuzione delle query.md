---
Date created: 17-12-25 • 16:42
tags:
  - Databases
Related PDF/DOC:
  - "[[Updated_12 Piani di esecuzione delle query.pdf]]"
Related Pages:
---
Quando scriviamo una query SQL il DBMS non *«esegue»* direttamente la query, dato che essa è per natura dichiarativa (*ci dice cosa vogliamo ottenere, non come*).

Il compito del DBMS è di costruire un **piano** per decidere come ottenere i dati richiesti nel modo più <mark class="hltr-orange">efficiente</mark> possibile.

---
## Ottimizzazioni del DBMS
### Costi e scelte
>Il *query optimizer* del DBMS ha un ruolo chiave nel decidere come eseguire una query SQL nel modo più efficiente possibile.

Il query optimizer valuta diverse possibili *strategie* di esecuzione. In particolare ...

- Genera piani di esecuzione alternativi, identificando le operazioni (*`WHERE`, `JOIN`, `ORDER BY`, …*) e variando l’ordine della loro esecuzione.
- Calcola i costi di ognuno di questi piani (*Costo in operazioni IO*).
- Seleziona il piano ottimale.

> [!example]- Esempi di scelte del query optimizer
> - **Come accedere alle tabelle**: Full table scan o indice
> - **Quando applicare le condizioni filtro (WHERE)** : Prima o dopo un determinato JOIN.
> - **Come eseguire i JOIN** : NLJ, Hash join, Sort merge join, …
> - **Come ordinare i dati** : Usare un indice già ordinato o eseguire un sort.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Per stimare il costo dell’esecuzione di un piano, viene calcolato il <mark class="hltr-purple">costo di ogni singola operazione in termini di I/O</mark>.

Per farlo, l’optimizer si basa su statistiche che il DBMS aggiorna regolarmente:
- **Cardinalità delle tabelle** (*|R|*).
- **Cardinalità dell’indice** (*valori distinti* colonna).
- **Distribuzione dei valori** in ogni colonna (*valori distinti, min, max, …*).
- **Selectivity factor**: Stima della percentuale di tuple che supereranno una clausola di filtro WHERE per un certo valore.

Inoltre, l’optimizer deve conoscere anche: 
- <mark class="hltr-orange">Quali indici sono disponibili</mark> (*e il tipo*).
- <mark class="hltr-orange">Il numero di pagine $P_R$ utilizzate per memorizzare ogni relazione</mark> (*ottenuto da $P / t_R$*).

---

### Rappresentazione di un piano
> Per rappresentare un piano di esecuzione, si usa un **albero**.

Le caratteristiche dell'albero che rappresenta una query sono :
- Le foglie sono le **relazioni di partenza**, più la **modalità di accesso** (*full scan o indice*).
- I nodi intermedi corrispondono a **operazioni algebriche sulle relazioni** coinvolte (*join, filtri, aggregazioni, …*).
- A ogni nodo intermedio viene associato un **costo** (*più il numero di righe della relazione intermedia*).
- La radice corrisponde all’**output della query** (*che è una relazione*).

> [!example] Esempio di rappresentazione di piano
> [[Updated_12 Piani di esecuzione delle query.pdf#page=8|Updated_12 Piani di esecuzione delle query, p.8]]
> 
> <hr style="width: 70%; margin-left: auto;margin-right: auto;">
>
> 
![[EMBED/Updated_12 Piani di esecuzione delle query.png]]
[[Updated_12 Piani di esecuzione delle query.pdf#page=9&rect=47,37,671,410|Updated_12 Piani di esecuzione delle query, p.9]] 



> [!info]- Come visualizzare un piano attraverso il DBMS
> Per conoscere il piano di esecuzione scelto dal DBMS, si può usare il comando : 
> ```sql
> EXPLAIN ANALYZE format=TREE
> ```
