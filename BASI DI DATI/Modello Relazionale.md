---
Date created: 17-09-25 • 14:16
tags:
  - databases
Related PDF/DOC:
  - "[[02 Modello Relazionale.pdf]]"
Related Pages:
---
## Concetti generali del modello relazionale

> [!QUOTE] Definizione di relazione
> > Informalmente, una relazione può essere vista come una tabella con un insieme di valori su ogni riga
> >
>
> PDF : [[02 Modello Relazionale.pdf#page=3&selection=4,0,17,4|02 Modello Relazionale, p.3]]


> [!info]- Storia
> [[02 Modello Relazionale.pdf#page=2|02 Modello Relazionale, p.2]]


> [!info]- Terminologia
> ![[EMBED/02 Modello Relazionale 1.png]]
[[02 Modello Relazionale.pdf#page=10&rect=87,61,644,405|02 Modello Relazionale, p.10]]


Ci sono due livelli che definiscono una relazione: 
- Lo <mark class="hltr-orange">schema</mark> della relazione (*livello intensionale*).
- <mark class="hltr-purple">Istanze</mark> della relazione (*livello estensionale*).

### Struttura di una relazione
#### Definizione intensionale
Lo schema di una relazione definisce :

- Il **nome** della relazione (*STUDENTI*).
- Il nome di ogni **attributo** (*SID, NOME, LOGIN, …*).
	- Il numero di attributi definisce il **grado** (*arietà*) della relazione.
- Il **dominio** di ogni attributo, ovvero l’insieme dei valori che quell’attributo può assumere (*INTEGER, STRING, …*).


> [!example]- Esempio grafico di una relazione
> 
![[EMBED/02 Modello Relazionale.png]]
[[02 Modello Relazionale.pdf#page=5&rect=122,252,565,402|02 Modello Relazionale, p.5]]

#### Definizione estensionale

> [!QUOTE] Definizione di istanza
> > Un’istanza di uno schema di relazione è un insieme di tuple (*record*), ognuna delle quali ha lo stesso numero di campi dello schema della relazione.
>
> PDF : [[02 Modello Relazionale.pdf#page=6&selection=4,1,17,28|02 Modello Relazionale, p.6]]

E' imperativo che <mark class="hltr-red">NON ESISTANO RECORD DUPLICATI</mark> per assicurare la correttezza pratica e teorica.

Il numero di istanze della definisce la **cardinalità** della relazione.

%%Salto lo stato della relazione pk sticazzi [[02 Modello Relazionale.pdf#page=7|02 Modello Relazionale, p.7]]%%

### Vincoli sulle relazioni
Un vincolo è definito come una condizione che <mark class="hltr-orange">DEVE</mark> valere affinché lo stato di una relazione sia **valido**.

I vincoli determinano quali stati di una relazione in una base di dati relazionale sono ammissibili e quali non lo sono. 
Ne esistono tre categorie:

1. **Vincoli impliciti** : Dipendono dal data model stesso (*per es. il modello relazionale non ammette liste come valore di alcun attributo*).
2. **Vincoli basati sullo schema** : Sono definiti nello schema usando gli strumenti forniti dal modello (*per es. un vincolo di partecipazione totale nel modello ER*).
3. **Vincoli applicativi o semantici**: Si tratta di vincoli che vanno al di là del potere espressivo del modello e devono essere imposti a livello di programma applicativo (*per es. che un libro deve essere restituito alla biblioteca entro 30 giorni dal prestito*) (*spesso imposti dal cliente*).

Inoltre esistono diversi tipi di <mark class="hltr-purple">vincoli espliciti</mark> :
- Vincolo di **dominio**. 
- Vincolo di **chiave**.
- Vincolo di **integrità delle entità**.
- Vincolo di **integrità referenziale**.
#### Vincolo di chiave
Ogni riga di una relazione ha un campo (*o multipli*) il cui valore (*o valore combinato*) <mark class="hltr-red">identificano univocamente</mark> quella riga in quella tabella. Questo campo è detto **chiave della relazione** (*key*).

Talvolta si usano valori *convenzionali* per identificare una riga in una tabella, come per esempio un numero incrementale. In questo caso si parla di <mark class="hltr-purple">chiavi artificiali</mark> o <mark class="hltr-orange">chiavi surrogate</mark>.

Esistono diversi tipi di vincoli di chiave...

##### Superchiave
Una **superchiave** è un insieme di attributi $S_K$ di $R$ tali che:
- Non esistono due tuple di $r(R)$ in cui gli attributi in $S_K$ hanno lo stesso valore.
- La condizione deve essere rispettata in ogni stato valido di $R$.

##### Superchiave minimale
Una chiave è detta **superchiave minimale** se:
- E' una superchiave.
- La rimozione di qualsiasi attributo da $S_K$ produrrebbe un insieme di attributi che <mark class="hltr-orange">non è più una superchiave</mark> di $R$.

Ogni chiave minimale è detta anche una **chiave candidata**.

Se una relazione si ha più di una chiave candidata, una viene scelta come **chiave primaria**. In generale, viene scelta la chiave candidata *più piccola*. 

I valori della chiave primaria sono usati per <mark class="hltr-red">identificare in modo univoco ogni tupla della relazione</mark>, inoltre può essere usata per fare *riferimento* a quella tupla da tuple di un’altra relazione.

#### Vincolo sulle entità
Si possono anche imporre dei vincoli sui campi inseriti in una tupla.


> [!example] Esempio di integrità di un entità
> Nessuno degli attributi che compongono la chiave primaria $P_K$ di una relazione $R$ può avere valore `NULL` in alcuna tupla di $r(R)$.

#### Vincolo di integrità referenziale
A differenza degli altri vincoli, un vincolo di integrità referenziale coinvolge **due relazioni**, una <mark class="hltr-orange">referenziante</mark> (`R1`) e una <mark class="hltr-purple">referenziata</mark> (`R2`).

In `R1` c’è un insieme di attributi <mark class="hltr-orange">FK</mark> (*Foreign Key/Chiave esterna*) che fanno **riferimento** agli attributi della chiave primaria <mark class="hltr-purple">PK</mark> (*Primary Key*) di `R2`.


> [!example] Esempio di referenza
> 
![[EMBED/02 Modello Relazionale 2.png]]
[[02 Modello Relazionale.pdf#page=18&rect=7,6,694,420|02 Modello Relazionale, p.18]]

> [!warning]- Auto-referenza
> <mark class="hltr-red">NON</mark> è imperativo che `R1` & `R2` siano due relazioni diverse.
>  > [!example] Esempio di auto-referenza
> >![[EMBED/02 Modello Relazionale 3.png]]
[[02 Modello Relazionale.pdf#page=20&rect=40,7,684,230|02 Modello Relazionale, p.20]]



> [!warning]- NULL nella Foreign Key
> `NULL` nei DBMS può essere interpretato in due modi:
> - Non esiste valore possibile per il campo.
> - Se in una reference, non si ha quello che si cerca nell'altra relazione.
>
>Questo vuol dire che i valori degli attributi della chiave esterna `FK` della relazione referenziante (`R1`) <mark class="hltr-orange">possono essere NULL</mark>.
>
>Tuttavia se assumono questo valore <mark class="hltr-red">NON POSSONO FAR PARTE DELLA PRIMARY KEY</mark> di `R1`.


### Stato di una base relazionale

> [!QUOTE] Definizione di Stato di una base relazionale
> > Uno stato di una base di dati relazionale con schema $S$ è un insieme di stati delle relazioni $\{r_1, r_2, ..., r_m\}$ tali che ogni $r_i$ è uno stato di $R_i$ e tale che $r_i$ soddisfa i vincoli di integrità relazionale in IC.
>
> PDF : [[02 Modello Relazionale.pdf#page=24&selection=16,0,52,49|02 Modello Relazionale, p.24]]
> 

Uno stato di una base di dati relazionale viene talvolta chiamato un’**istantanea** (*snapshot*).

Uno stato di base di dati che <mark class="hltr-orange">non rispetta i vincoli in IC</mark> è uno stato <mark class="hltr-red">non valido</mark>.

Ogni volta che la base di dati è modificata, si passa in un nuovo stato della base di dati; le operazioni per modificare un database si suddividono in:
- **INSERT**: inserimento di una nuova tupla in una relazione.
- **DELETE**: cancellazione di una tupla da una relazione. 
- **MODIFY**: modifica di un attributo di una tupla.

E' per questo che è importante che queste azioni <mark class="hltr-red">NON VIOLINO I VINCOLI DI INTEGRITA'</mark>.


> [!example]- Esempio di violazione di vincoli con un INSERT
> L’operazione INSERT può violare tutti i vincoli:
> - **Vincoli di dominio**: Il valore di uno o più attributi della/e nuova/e tupla/e non appartiene al dominio specificato nel modello.
> - **Vincolo di chiave**: Inserimento di tupla/e in cui il valore della chiave già esiste. 
> - **Integrità referenziale**: Valore della chiave esterna che fa riferimento a valori della chiave primaria della relazione referenziata che non esistono.
> - **Integrità dell’entità**: Il valore della chiave primaria della/e nuova/e tupla/e è NULL.

#### Preservare l'integrità referenziale
Ci sono vari modi per mantenere l'integrità referenziale quando si fa una modifica al campo referenziato dalla foreign key. Questi sono i modi principali: 

- **RESTRICT** (*NO ACTION*): rifiutare l’operazione.
- **CASCADE**: cancellare tutte le tuple che referenziavano la chiave primaria della tupla cancellata o modificata. 
- **SET NULL**: assegnare il valore `NULL` alla chiave esterna delle tuple che referenziavano la chiave primaria della tupla cancellata o modificata.
- **SET DEFAULT**: assegnare un valore di default alle chiavi esterne che referenziavano la chiave primaria della tupla cancellata o modificata.

%%saltate pp 22->23%%