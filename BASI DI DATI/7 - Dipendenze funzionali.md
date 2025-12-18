---
Date created: 10-12-25 • 14:39
tags:
  - Databases
Related PDF/DOC:
  - "[[07 Dipendenze Funzionali.pdf]]"
Related Pages:
---
La modellazione concettuale ci fornisce un insieme di relazioni e vincoli di integrità che costituiscono una buona base di partenza.

Tuttavia, i vincoli di integrità possono essere utilizzati per un *ulteriore raffinamento* dello schema relazionale che evita anomalie e inconsistenze dalla base di dati.


> [!warning] Tipi di inconsistenza da risolvere
> Prendendo come esempio questa tabella ...
> 
> 
> ![[EMBED/07 Dipendenze Funzionali.png]]
[[07 Dipendenze Funzionali.pdf#page=4&rect=325,361,705,539|07 Dipendenze Funzionali, p.4]]
>
> possiamo vedere le seguenti inconsistenze :
> - **Ridondanza** : Non è necessario che lo stipendio venga ripetuto in ogni tupla in cui compare l'impiegato.
>  - **Aggiornamento** : Cambiare lo stipendio di un impiegato comporta il cambiamento di molteplici tuple.
>   - **Inserimento** : NON E' POSSIBILE aggiungere un impiegato se non è assegnato ad alcun progetto. 
>    - **Cancellazione** : Se un impiegato smette di partecipare a tutti i progetti bisogna cancellarlo dalla tabella.


La sorgente della maggior parte delle anomalie è la <mark class="hltr-orange">non separazione di informazioni diverse</mark>.

Per studiare questi aspetti di una base di dati dobbiamo introdurre il concetto di **Dipendenza Funzionale.**

> [!QUOTE] Dipendenza Funzionale
> > Una DF descrive legami di tipo funzionale tra gli attributi di una relazione
>
> PDF : [[07 Dipendenze Funzionali.pdf#page=7&selection=32,0,34,27|07 Dipendenze Funzionali, p.7]]

> [!QUOTE]- Formalizzazione di Dipendenza Funzionale
> > Sia $r$ una relazione sullo schema $R(X)$ e siano $Y$ e $Z$ due sottoinsiemi non vuoti di $X$.
> > $Z$ dipende funzionalmente da $Y$ $(Y \to Z)$ se per ogni coppia di tuple $t_1$ e $t_2$ di $r$ vale che:
> > $$t_1[Y]=t_2[Y] \implies t_1[Z]=t_2[Z]$$
>
> PDF : [[07 Dipendenze Funzionali.pdf#page=9&selection=2,0,35,9|07 Dipendenze Funzionali, p.9]]

---

## Le Closure

### Closure delle DF
Vediamo le definizioni necessarie a comprendere le Closure:

> [!QUOTE] Implicazione Logica
> > Dato un insieme $F$ di DF, è possibile calcolare altre DF *logicamente implicate* da $F$.
> > 
> > Es:
> > $$X \to Z\ ,\ Z \to Y \implies X \to Y$$
>
> PDF : [[07 Dipendenze Funzionali.pdf#page=19|07 Dipendenze Funzionali, p.19]]


> [!QUOTE] Closure di un insieme di DF
> > Sia un insieme $F$ di DF che implica logicamente una DF $X \to Y$ (*Ogni istanza che soddisfa F soddisfa anche $X \to Y$*).
> > 
> > L'insieme di tutte le DF logicamente implicate da $F$ si chiama **Closure di $F$** e viene indicata con la notazione:  
> > 
> > $$F^+$$
>
> PDF : [[07 Dipendenze Funzionali.pdf#page=19|07 Dipendenze Funzionali, p.19]]

In parole povere una closure di una attributo, ad esempio $A$, sono tutti i restanti attributi implicati (*che si possono raggiungere*) da quest'ultimo.

> [!example] Esempio di closure
> Immagina di avere una relazione (tabella) con attributi **A, B, C, D, E** e le seguenti dipendenze funzionali:
>- A → B
>- B → C
>- C → D
>- D → E
>
>Ora calcoliamo la **closure di `{A}`**, cioè tutti gli attributi che si possono determinare partendo da A:
>
> 1. Partiamo da `A⁺ = {A}`
> 2. Da **A → B**, aggiungiamo B → `A⁺ = {A, B}`
> 3. Da **B → C**, aggiungiamo C → `A⁺ = {A, B, C}`
> 4. Da **C → D**, aggiungiamo D → `A⁺ = {A, B, C, D}`
> 5. Da **D → E**, aggiungiamo E → `A⁺ = {A, B, C, D, E}`
>
> Alla fine `{A}⁺ = {A, B, C, D, E}`: partendo da A posso ottenere _tutti_ gli altri attributi.


#### Assiomi di Armstrong

Una Closure può essere calcolata attraverso gli *Assiomi di Armstrong*.

- **Assioma di riflessività**: $Y \subseteq X \implies X \to Y$
- **Assioma di aumento**:  $X \to Y \implies XZ \to YZ$ 
- **Assioma di transitività**: $X \to Y\ ,\ Y \to Z \implies X \to Z$

Da questi assiomi si ottengono inoltre due regole derivate:

- **Unione**: $X \to Y\ ,\ X \to Z \implies X \to YZ$
- **Scomposizione**: $X \to YZ \implies X \to Y\ ,\ X \to Z$

Questi assiomi infine si possono definire <mark class="hltr-orange">Sound (Corretti)</mark> e/o <mark class="hltr-purple">Complete (Completi)</mark>.

- <mark class="hltr-orange">Sound</mark> : Se gli AA generano una nuova dipendenza funzionale $f$ a partire da un insieme $F$ (*$f$ è logicamente implicata da $F$*).
- <mark class="hltr-purple">Complete</mark> : Se una dipendenza funzionale $f$ è logicamente implicata da $F$, allora gli AA sono in grado di generarla.

Questo equivale a dire che gli AA sono in grado di <mark class="hltr-red">generare TUTTE e SOLE le DF che sono logicamente implicate da $F$</mark>.


> [!example]- Esempio di applicazione degli AA
> [[07 Dipendenze Funzionali.pdf#page=22|07 Dipendenze Funzionali, p.22]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Closure di un insieme di attributi

> [!QUOTE] Closure di un insieme di attributi
> > Sia $X$ un insieme di attributi e si ponga $(X)^+ =X$. 
> > Se esiste una DF $Z \to W$ con $Z \subseteq (X)^+$ allora $W$ viene aggiunta a $(X)^+$.
> > 
> > Queste operazioni si ripetono finchè *non è possibile aggiungere ulteriori elementi a $(X)^+$*
>
> PDF : [[07 Dipendenze Funzionali.pdf#page=24&selection=2,5,2,11|07 Dipendenze Funzionali, p.24]]



> [!example]- Esempio di closure di un insieme di attributi
> [[07 Dipendenze Funzionali.pdf#page=25|07 Dipendenze Funzionali, p.25]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Copertura minimale di un insieme di DF
Il <mark class="hltr-orange">calcolo della closure di un insieme $F$ di DF può essere molto complesso</mark> se richiede di derivare un numero esponenziale di DF.

Per semplificare il processo possiamo usare una **copertura minimale**, cioè una forma più semplice, ma logicamente equivalente, dell'insieme $F$.

> [!QUOTE] Copertura minimale di un insieme di DF
> > Siano $F$ e $F_{min}$ due insiemi di DF; *$F_{min}$ è detto una copertura minimale di $F$*, se:
> > - $F^+_{min}=F^+$
> > - $\forall f\in F_{min}\text{ sono nella forma }X\to A$
> > - $\text{Rimuovendo una DF o attributo sinistro da una DF in }F_{min} \implies F^+_{min}\neq F^+$
> >  
>
> PDF : [[07 Dipendenze Funzionali.pdf#page=26&selection=2,0,2,24|07 Dipendenze Funzionali, p.26]]


In parole povere una copertura minimale di un insieme **$F$ contiene la minima quantità di informazioni per descrivere logicamente la totalità di $F$**.

#### Calcolo della copertura minimale
Per calcolare una copertura minimale si seguono tre passi:


> [!example]- Esempio di creazione di una copertura minimale
> [[07 Dipendenze Funzionali.pdf#page=30|07 Dipendenze Funzionali, p.30]]

##### Step 1 - Normalizzazione
Ogni DF con più di un attributo sul lato destro va normalizzata separandola in più DF.


> [!example] Step 1
> $$\begin{array}{} X \to ABC\ :\\
> X \to A \\
> X \to B \\
> X \to C \\ 
> \end{array}$$

##### Step 2 - Rimozione attributi ridondanti
Dato un insieme $M$ di DF e una DF di forma $XB \to A$ dobbiamo verificare se <mark class="hltr-orange">$B$ è ridondante</mark>.

Per fare ciò dobbiamo calcolare la **closure** dell'insieme di attributi $(X)^+$. 
Se <mark class="hltr-red">$A\in (X)^+$ allora $B$ è ridondante</mark> e può essere rimosso dalla DF, creando un nuovo insieme $M'$.

Ripetiamo i passaggi con $M'$ fino a non avere più attributi ridondanti.

##### Step 3 - Rimozione DF ridondanti
Dato un insieme $M$ di DF per una relazione $R$ dobbiamo eliminare le DF ridondanti.

Per trovarle dobbiamo ottenere $M'$ rimuovendo una DF di forma $X \to A$ da $M$.
Se <mark class="hltr-red">$A\in(X)^+,\ X\in M'$ allora $A$ è ridondante</mark> e può essere rimossa.

Proseguiamo da $M'$ per ogni DF di forma $X\to A$.

---

## DF e Chiavi
Prendendo una chiave $K$ di una relazione $R$ si può facilmente verificare che esiste una DF tra <mark class="hltr-purple">$K$</mark> e <mark class="hltr-orange">ogni altro attributo</mark> dello schema di $R$:

Per definizione, in $R$ non possono esistere due tuple con lo stesso valore su $K$, e quindi una DF che ha $K$ al primo membro sarà sempre soddisfatta.

Per questo si dice che il concetto di **DF generalizza quello di vincolo di chiave**.


> [!info]- Ridondanze sulle DF con chiavi
> Una DF che contiene una chiave nella sua parte sinistra, per definizione, **NON** può generare ridondanze.
> 
> <mark class="hltr-red">Le anomalie sono perciò causate solo dalle DF $Y \to Z$ tali che $Y$ non contiene una chiave.</mark>

### Trovare Chiavi dalle DF
Sia una DF $Y\to Z$ su uno schema $R(X)$; questa degenera in un **vincolo di chiave** se <mark class="hltr-purple">$Y \cup Z = X$</mark>. 

Cioè se un insieme di uno o più campi $Y$ (*Presunta chiave*), unita a $Z$, cioè il resto dei campi, forma la tupla intera.

In tal caso, infatti, <mark class="hltr-orange">$Y$ è chiave per lo schema $R(X)$</mark>.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Calcolo delle chiavi dalla Closure

Possiamo utilizzare una closure di un insieme di attributi $X$ per verificare se quest'ultimo è una chiave per una relazione $R$.

> [!QUOTE] Chiavi di una relazione a partire dalla closure
> > Sia $R(U)$ una relazione con attributi $U$ e con l'insieme di DF $F$. Dato un insieme di attributi $X \subseteq U$ e calcolato $(X)^+$, allora:
> > $$((X)^+=U\iff X \to U)\implies X \text{ superchiave di } R(U)$$
>
> PDF : [[07 Dipendenze Funzionali.pdf#page=31&selection=0,7,0,12|07 Dipendenze Funzionali, p.31]]


> [!example]- Esempio di chiave trovata attraverso una closure
> [[07 Dipendenze Funzionali.pdf#page=32|07 Dipendenze Funzionali, p.32]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Algoritmo per trovare le chiavi candidate
Per trovare le chiavi candidate di una relazione, possiamo seguire un algoritmo in 8 passi :

1. Determinare l’insieme $I$ degli attributi di $R(U)$ che non sono <mark class="hltr-orange">né sul lato sinistro né sul lato destro</mark> di una DF (*attributi Isolati*).
2. Determinare l’insieme $L$ degli attributi che sono <mark class="hltr-orange">solo sul lato sinistro</mark> di una qualsiasi DF (*attributi Left*).
3. Determinare l’insieme $R$ degli attributi che sono <mark class="hltr-orange">solo sul lato destro</mark> di una qualsiasi DF (*attributi Right*).
4. Determinare l'insieme $S$ degli <mark class="hltr-orange">attributi rimanenti, presenti in ambo i lati</mark> di una qualsiasi DF (*tutti gli attributi rimanenti*).
5. Combinare $I$ e $L$ in $A$  ( $A=I\cup L$ ).
6. Calcolare la closure di $A$ ($A^+$)
	1. Se <mark class="hltr-purple">$A^+ = U$ allora gli attributi in A costituiscono l'unica chiave candidata.</mark>
	2. <mark class="hltr-red">L'algoritmo termina.</mark> 
7. Combinare $R$ e $S$ in $B$ ($B = R \cup S$)
8. Partendo dagli attributi in $A^+$ (*se esiste*), combinarli uno alla volta con tutti i sottoinsiemi degli attributi in $B$ e determinare le closure di attributi che sono uguali a $U$.
	1. Con $X$ insieme degli attributi, se <mark class="hltr-red">$X=U$, non serve più aggiungere altri attributi a $X$</mark>, dato che si troverebbero solo eventuali altre superchiavi non minimali.


> [!example] Esempio di calcolo delle chiavi candidate
> 
![[EMBED/07 Dipendenze Funzionali 1.png]]
> [[07 Dipendenze Funzionali.pdf#page=34&rect=9,16,697,414|07 Dipendenze Funzionali, p.34]]

#### Grafo delle dipendenze
Per calcolare le chiavi candidate è possibile usare come aiuto il *grafo delle dipendenze*.


> [!example] Esempio grafico di grafo delle dipendenze
> ![[EMBED/07 Dipendenze Funzionali 2.png]]
[[07 Dipendenze Funzionali.pdf#page=35&rect=268,175,443,309|07 Dipendenze Funzionali, p.35]]

Nel grafo, le frecce rappresentano le dipendenze della relazione. Seguendo le linee, è possibile stabilire se da un certo insieme di attributi posso raggiungere altri attributi.

Da un grafo delle dipendenze possiamo vedere che:
- Se un nodo non ha nodi entranti, <mark class="hltr-red">allora appartiene a ogni chiave candidata</mark>.
- Se da un insieme di nodi $X$ riesco a raggiungere tutti gli altri nodi, allora <mark class="hltr-red">$X$ è una chiave candidata</mark>.