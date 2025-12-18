---
Date created: 12-12-25 • 14:42
tags:
  - Databases
Related PDF/DOC:
  - "[[09 Modello di costo.pdf]]"
  - "[[10 Indici.pdf]]"
Related Pages:
  - "[[9b - Modello di costo]]"
---

## Organizzazione dei file di un database
> In un database si parla di **organizzazione dei file** per determinare come le *tuple* vengono fisicamente memorizzate sul disco.

> [!important]- Glossario
> - **Dati persistenti**: La Base di dati.
> - **Dati temporanei**: Dati presenti durante l'esecuzione di programmi.
> - **Record**: Collezione di valori tra loro collegati (*Ogni valore è un campo del record*).
> - **Data Types**: Tipi di dato dei valori (*Int/String/...*).
> - **BLOB**(_**B**inary **L**arge **O**bjects_): Data type per oggetti binari senza struttura (*Solitamente immagini o file*).

I dati di un database vengono memorizzati *"a lungo termine"* sulla <mark class="hltr-blue">memoria secondaria</mark> (*Hard disk e simili*), tuttavia quando si svolgono operazioni sui dati questi vengono caricati in quella <mark class="hltr-purple">primaria</mark> (*RAM*).

### Tuple, tabelle e pagine
#### Pagine

Sul disco, i record vengono allocati in blocchi, detti **pagine**.

Il DBMS organizza logicamente il disco, raggruppando più pagine in un settore del disco. Per ogni operazione da eseguire, il DBMS <mark class="hltr-red">carica sempre pagine intere</mark> (*mai parti di pagina*).

---
#### Tuple
Le tuple, memorizzate su file vengono dette **record**. 

I record hanno diverse propietà di cui tenere conto, come la *lunghezza* ($t_r$): dato che la lunghezza di una tupla è di natura imprevedibile, dobbiamo decidere se memorizzarle in spazi di lunghezza <mark class="hltr-orange">statica</mark> o <mark class="hltr-purple">variabile</mark>.

Per esempio, avere <mark class="hltr-purple">lunghezza variabile</mark> ci permette di avere campi opzionali o di lunghezza variabile.

> [!info]- Record esteso
> Un **Record esteso** quando supera lo spazio disponibile in un blocco, viene parzialmente memorizzato su due blocchi diversi, tramite un puntatore.
> 
> La loro controparte sono i **record non estesi**, che non possono superare i limiti dei blocchi.

^464874



<hr style="width: 70%; margin-left: auto;margin-right: auto;">



Esistono diversi modi di memorizzare le tuple di una relazione su un file ...

##### Heap File
> Le tuple sono scritte una dopo l’altra nell’ordine di inserimento.

Le propietà di un heap file sono :
- Inserimento di nuove tuple molto veloce. 
- Le tuple vengono memorizzate in pagine in base all’ordine di inserimento.
- Quando una pagina è piena, si passa alla successiva (*[[#^464874|Record NON esteso]]*) (*si evita l'eventualità di leggere due pagine per una singola tupla*).
- Il [[9b - Modello di costo#Selectivity factor|Selectivity Factor]].

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

##### Sorted file
> Le tuple sono ordinate in base al valore di un attributo o insieme di attributi (*Generalmente la p-key*).

Il Sorted file nasce per abbassare il costo di alcune operazioni comuni.



<hr style="width: 70%; margin-left: auto;margin-right: auto;">

##### Indexed file
> Viene creato un indice separato rispetto al file che contiene le tuple vere e proprie della relazione.

Un ulteriore modo per rendere più efficienti le operazioni sul file dei dati è quello di creare degli **indici esterni**, indipendentemente dal fatto che il file sia ordinato o meno.


Si vengono a creare quindi due file separati : 
- *Data File* : Che contiene le tuple.
- _**Index File**_ : Che contiene gli indici dei dati (<mark class="hltr-orange">possono esistere molteplici</mark>). 

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Gli indici possono avere diverse caratteristiche e proprietà :

Innanzitutto si dividono categorie rispetto al contenuto ...

- **Indici primari** :  Contengono la chiave primaria della relazione; di conseguenza non ci sono duplicati nelle entries.
- **Indici secondari** : Non contengono la chiave primaria e possono contenere duplicati.
- **Indici Unici** : Contengono chiavi candidate; quindi non sono la chiave ma non possono ugualmente avere dei duplicati.

Nel caso degli <mark class="hltr-purple">indici secondari</mark>, quindi <mark class="hltr-red">con duplicati</mark>, si possono avere due approcci :
- **Chiave di indice estesa** : Alla chiave viene aggiunto il record ID (*RID*), in modo da renderla unica.
- **Lista di riferimenti** : La chiave compare una sola volta nel rispettivo <mark class="hltr-orange">nodo foglia</mark>, ma associa a quel valore un elenco (*bucket*) di puntatori ai RID delle diverse tuple con lo stesso valore.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Poi si possono avere **clustered** e **unclustered** ...

Gli *Indici clustered* sono indice in cui l’ordine fisico di memorizzazione delle righe di dati <mark class="hltr-orange">corrisponde all’ordine logico specificato dalla chiave dell’indice</mark>.

Si può avere quindi <mark class="hltr-red">un solo indice clustered per tabella</mark>.

Gli *indici unclustered* sono ovviamente l'opposto.

---


Gli indici dei dati  possono essere rappresentati come <mark class="hltr-purple">B+Tree</mark> o <mark class="hltr-purple">Hash</mark> ...

###### B+ Tree
>Si tratta di un tipo di indice dinamico ad albero, dove i puntatori ai dati sono memorizzati solo nei nodi foglia.

I nodi interni dell’albero contengono *"index entries"* e servono a <mark class="hltr-orange">guidare la ricerca</mark>. Al contrario i nodi foglia contengono i puntatori ai dati del data file. 
I nodi foglia sono collegati tra di loro (*sequence set*). ^5c4a69

> [!example]- Visualizzazione di un B+ Tree
> [Link](https://roy2220.github.io/bptree/visualization)

Le propietà di questo tipo di albero sono :
- Inserimento e cancellazione mantengono l'albero bilanciato (*tutte le foglie sono alla stessa profondità*).
- È garantita un’occupazione almeno al 50% di ogni nodo dell’albero (*ad eccezione della radice*).
- La ricerca richiede sempre di discendere l’albero fino all’appropriato nodo foglia (*Il tempo è uguale per tutti i record*).
- Siccome l’albero è bilanciato, possiamo definire come altezza dell’albero la lunghezza di qualunque percorso dalla radice alle foglie.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

###### Hash
> Per questa rappresentazione si sfruttano le propietà degli hash

Per applicare questo metodo si sceglie una *funzione di Hash*, ad esempio $h(x) = x\mod N$ per i numeri interi, dove $N$ è l’operatore resto della divisione. Si allocano poi i diversi valori di un certo attributo in un *bucket* (*«secchio»*) in base al valore della funzione di hash prescelta.

Se la pagina del bucket contiene troppi puntatori, il DBMS dovrà creare una <mark class="hltr-orange">pagina di overflow</mark>, che sarà connessa alla pagina principale del bucket e che potrà contenere i nuovi puntatori al file di dati.



> [!example]- Visualizzazione di indicizzazione con Hash
> ![[EMBED/10 Indici.png]]
[[10 Indici.pdf#page=18&rect=162,34,563,425|10 Indici, p.18]]

L’Hash index è molto efficiente, <mark class="hltr-orange">tranne che per la ricerca per intervallo</mark>. 
Tuttavia, osserviamo che, prima o poi, i bucket richiederanno sempre più pagine di overflow e <mark class="hltr-red">quindi il costo di lookup aumenterà</mark>. 

Il DBMS dovrà periodicamente riorganizzare l’index file, utilizzando una funzione di hash con più bucket. Questo però richiederà di spostare i vari puntatori nei nuovi bucket, con relativo costo. Questo approccio si chiama **Extendible Hash Index**.
