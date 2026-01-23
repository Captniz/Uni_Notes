# Basi di dati 
## Fondamenti
> Una base di dati è una raccolta organizzata (strutturata) di informazioni che devono essere gestite per un periodo di tempo lungo, gestita da un database management system.
### Il DBMS ( DataBase Management System)
I compiti di un DBMS sono:

- Supportare la memorizzazione di grandi quantità di dati.
- Gestire la robustezza dei dati e il loro recupero in caso di fallimenti, errori o azioni malevole. 
- Controllare l’accesso ai dati ...
    - Di più utenti contemporaneamente (*concorrenza*). 
    - Evitando interazioni indesiderate tra utenti (*isolation*).
    - Evitando modifiche incomplete dei dati (*atomicity*).

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Esistono diversi linguaggi specializzati per la gestione di dati :

- **DDL** (*Data Definition Language*) : Linguaggio che permetta di definire schemi per strutturare i dati.
- **DML** (*D. Manipulation L.*) : Per modificare e estrarre i dati.
- **DCL** (*D. Control L.*) : Per la gestione dell'accesso ai dati.
- **TCL** (*Transaction Control L.*) : Per gestire la sicurezza delle transazioni.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Le interazioni con DBMS avvengono tramite <mark class="hltr-purple">Queries</mark> (_Accesso ai dati_) e <mark class="hltr-orange">Transactions</mark>(_Sequenze atomiche di azioni_).

Queste azioni si basano sul principio d'<mark class="hltr-red">indipendenza programmi-dati</mark>, che permette di **modificare le strutture dati e l’organizzazione fisica dei dati** senza dover cambiare l’accesso dei programmi alla base di dati via DBMS.

Da questo principio otteniamo due definizioni :


> [!quote] Indipendenza Logica dei Dati
> > La capacità di modificare lo schema logico del database senza dover alterare lo schema esterno o le applicazioni che utilizzano il database.
> 
> PDF : [[01 Basi di Dati e loro utenti.pdf#page=10|01 Basi di Dati e loro utenti, p.10]]


> [!quote] Indipendenza Fisica dei Dati
> > La capacità di modificare lo schema fisico del database senza influenzare lo schema logico o le applicazioni.
> 
> PDF : [[01 Basi di Dati e loro utenti.pdf#page=11|01 Basi di Dati e loro utenti, p.11]]

---

### Astrazione dei dati
I dati sono rappresentati in tre schemi :

> [!warning] Schema Logico 
> Modello che rappresenta i dati nei termini del modello proprio del DBMS. 
> 
> *Per esempio*: il modello relazionale nei RDBMS.

> [!ok] Schema Fisico
> Fornisce dettagli su come le relazioni descritte dallo schema logico sono memorizzate su memoria secondaria. 
> 
> *Per esempio*: file di record ordinati / non ordinati, creazione di indici, ecc. 
 
>[!done] Schema esterno
> Permette di creare una o più viste specifiche per velocizzare / facilitare / autorizzare l’accesso a insiemi di dati a specifici utenti o gruppi di utenti.


---

### Sicurezza delle transazioni

La condivisione dei dati e transazioni *multi-utente* permettono a gruppi di utenti di leggere e scrivere contemporaneamente sulla stessa base di dati.

Questo introduce un problema di <mark class="hltr-red">modifica concorrente dei dati</mark>.

Il **controllo della concorrenza** nel DBMS garantisce che ogni transazione sia eseguita correttamente o annullata.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Il sottosistema di **recovery** assicura che ogni transazione correttamente completata sia <mark class="hltr-orange">memorizzata in modo permanente nella base di dati</mark>. 

Questo permette di eseguire centinaia di transazioni concorrenti al secondo mantenendo validità e coerenza dei dati (*OLTP : Online Transaction Processing*).

>[!info]- OLTP vs OLAP
> <mark class="hltr-blue">OLTP</mark>
> > OLTP (*Online Transaction Processing*) ha lo scopo di gestire e facilitare le transazioni quotidiane di un'azienda.
>
> Caratteristiche:
> - Alta frequenza di transazioni con operazioni brevi e semplici (`insert`, `update`, `delete`).
> - Data integrity e coerenza sono fondamentali. 
> - Schema di database normalizzato per minimizzare la ridondanza. 
> - Risposta rapida alle query per garantire l'efficienza delle operazioni.
> 
> ---
>   
>  <mark class="hltr-purple">OLAP</mark>
>   >  OLAP (*Online Analytical Processing*) ha lo scopo di supportare l'analisi complessa dei dati per il business intelligence e il reporting.
>   
> Caratteristiche: 
> - Bassa frequenza di operazioni, ma con query complesse e di lunga durata.
> - Ottimizzato per query ad-hoc e analisi multidimensionali. 
> - Schema di database denormalizzato (*es. star schema*) per migliorare le performance delle query.
> - Aggregazione e storicizzazione dei dati per analisi tendenziali e strategiche.   

---
### Utenti di un DBMS

Solitamente in un DBMS si hanno due tipi di utente dalla parte della manutenzione ...
- **Amministratori** :
  E’ la figura responsabile dell’autorizzazione di accesso alla base di dati, del coordinamento e del monitoraggio del suo utilizzo e delle sure risorse software e hardware.
- **Progettisti** :
  E’ la figura responsabile della definizione del contenuto, dello schema, dei vincoli e delle transazioni che possono essere eseguite sulla BD.

Oltre ovviamente agli **end-user**, suddivisi per livello di esperienza con il DBMS.

---

## Modello relazionale

[[2 - Modello Relazionale]]

## Algebra relazionale
> Linguaggio **procedurale** di interrogazione per il modello relazionale. Base teorica di SQL e dell’ottimizzazione delle query.

Nell'AR Input e output di ogni operazione sono **relazioni** → algebra **chiusa**.

Concetti fondamentali :

- **Relazione**: insieme di tuple (niente duplicati)
- **Schema**: nome relazione + attributi (con domini)
- **Espressione di AR**: combinazione di operatori che produce una relazione

---

### Operatori Unari

#### SELECT (σ)
> Filtra le tuple che soddisfano una condizione

```
σ<condizione>(R)
```

- Non cambia lo schema
- Riduce (o mantiene) il numero di tuple
- **Commutativo**
- Selezioni multiple → un’unica selezione con AND

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### PROJECT (π)
>Seleziona un sottoinsieme di attributi

```
π<lista_attributi>(R)
```

- Rimuove **attributi**
- Elimina automaticamente i **duplicati**
- Non commutativo
- Se include una chiave → nessuna perdita di tuple

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


#### RENAME (ρ)
> Rinomina relazioni o attributi

```
ρ(NuovaRelazione(nuovoNome1, ...), Espressione)
```

- Utile per evitare conflitti di nomi (*es. nei join*).

---

### Operatori Insiemistici

#### UNION (∪)

```
R ∪ S
```

- Tuple in R **o** S
- Elimina duplicati
- Commutativa e associativa

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### INTERSECTION (∩)

```
R ∩ S
```

- Tuple in R **e** S
- Commutativa e associativa


<hr style="width: 70%; margin-left: auto;margin-right: auto;">


#### DIFFERENCE (−)

```
R − S
```

- Tuple in R ma non in S
- **Non commutativa**


<hr style="width: 70%; margin-left: auto;margin-right: auto;">



> [!error] Compatibilità degli operandi
> Per UNIONE, INTERSEZIONE, DIFFERENZA gli operandi devono essere compatibili :
> - Stesso numero di attributi
> - Domini compatibili (stesso tipo)


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### PRODOTTO CARTESIANO (×)

```
R × S
```

- Combina ogni tupla di R con ogni tupla di S
- Schema = attributi di R + attributi di S
- Cardinalità: |R| × |S|
- Base per la definizione dei JOIN

---

### JOIN

#### θ-JOIN (JOIN generale)

```
R ⋈<condizione> S = σ<condizione>(R × S)
```

- Commutativo e associativo
- Cardinalità ≤ prodotto cartesiano

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### EQUIJOIN

- θ-JOIN con sole uguaglianze (=)
- L’attributo di join compare **una sola volta** nel risultato

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


#### NATURAL JOIN (⋈)

- EQUIJOIN automatico su **tutti gli attributi con lo stesso nome**
- Elimina gli attributi duplicati
- Richiede attributi omonimi (altrimenti usare RENAME)

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### OUTER JOIN

Mantiene anche le tuple senza corrispondenza (con NULL)

- **LEFT OUTER JOIN**: tutte le tuple di sinistra
- **RIGHT OUTER JOIN**: tutte le tuple di destra
- **FULL OUTER JOIN**: tutte le tuple di entrambe

---

### DIVISIONE (÷)

- Operatore derivato (non sempre supportato)
- Query del tipo: "tutti quelli che hanno tutte le X"

Dato:

- A(x, y)
- B(y)

Risultato:

- Tutti i valori di x associati **a tutti** i valori di y in B



---

### Query Tree

- Struttura ad albero che rappresenta il piano di esecuzione
- Foglie: relazioni di base
- Nodi interni: operazioni (σ, π, ⋈, …)
- Usato per stimare costi e ottimizzare le query

---

### Ottimizzazione Algebrica (idee chiave)

**Push Selections Down**
- Applicare σ il prima possibile
- Riduce le tuple intermedie
 
 **Push Projections Down**
- Eliminare colonne inutili subito
- Riduce spazio e costi

**Riordino dei JOIN**
- Eseguire prima i join più selettivi
- Meno risultati intermedi

