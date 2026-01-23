---
Date created: 12-12-25 • 09:41
tags:
  - Databases
Related PDF/DOC:
  - "[[8 - Normalizzazione]]"
Related Pages:
---
## Metodi di normalizzazione

Le **forme normali** sono condizioni che garantiscono che certe anomalie <mark class="hltr-orange">non possano emergere in una relazione</mark>.

Il processo mediante cui una relazione, che non soddisfa una o più forme normali, viene scomposta in due o più relazioni viene detto *normalizzazione*. 

### Normalizzazione con scomposizione

L’intuizione di base è che le anomalie delle relazioni non normalizzate possano essere risolte via *scomposizione* della relazione originale in relazioni minori.


> [!QUOTE] Scomposizione di una relazione
> > Due relazioni $R_1(Y)$ e $R_2(Z)$ sono una scomposizione di una relazione $R(X)$ se : 
> > - $Y,\ Z \subset X$ 
> > - $Y \cup Z=X$
> > 
> >  $Y$ e $Z$ sono quindi *due proiezioni di $R$* tali che *la loro unione contenga tutti gli attributi in $X$*.
>
> PDF : [[08 Normalizzazione.pdf#page=3&selection=18,0,64,46|08 Normalizzazione, p.3]]

Durante una scomposizione di una relazione, è molto importante che si soddisfino due proprietà:
- <mark class="hltr-purple">Scomposizione senza perdita di informazione</mark> (*lossless join*): Nel ricomporre mediante `JOIN` la relazione di partenza a partire dalla sua scomposizione, non devono essere incluse **tuple spurie** (*ovvero non appartenenti alla relazione iniziale*).
-  <mark class="hltr-orange">Conservazione delle DF</mark>: La scomposizione deve conservare le DF della relazione originale, onde preservare i **vincoli di integrità**.

La prima proprietà <mark class="hltr-red">DEVE essere SEMPRE garantita</mark>, la seconda in certi casi può essere *«sacrificata»*.

#### Garantire la scomposizione lossless

> [!QUOTE] Condizione per la scomposizione lossless
> > Sia $R(X)$ una relazione e siano $X_1$ e $X_2$ due sottoinsiemi di $X$ tali che $X = X_1 \cup X_2$; 
> > inoltre, sia $X_0 = X_1∩X_2$.
> > 
> > $R$ si scompone lossless su $X_1$ e $X_2$ se si soddisfa la DF :
> > $$ X_0 \to (X_1 - X_0)\ or\ X_0 \to (X_2 - X_0)$$
>
> PDF : [[08 Normalizzazione.pdf#page=7&selection=8,6,8,9|08 Normalizzazione, p.7]]
> 

Quindi la scomposizione lossless è garantita se gli attributi comuni alle due relazioni, ottenute dalla scomposizione, contengono una chiave di almeno una delle due relazioni.


> [!example]- Esempio di verifica della condizione per la scomposizione lossless
> [[08 Normalizzazione.pdf#page=8|08 Normalizzazione, p.8]]

#### Garantire la conservazione delle DF

> [!QUOTE] Condizione per la conservazione delle DF
> > Le dipendenze sono conservate se è verificata la seguente condizione:
> > 
> > Ognuna delle dipendenze della relazione originale deve *essere ottenibile per proiezione da almeno una delle relazioni ottenute dalla scomposizione*.
>
> PDF : [[08 Normalizzazione.pdf#page=9&selection=3,0,5,20|08 Normalizzazione, p.9]]



> [!example]- Esempio di verifica della condizione per la conservazione delle DF
> [[08 Normalizzazione.pdf#page=10|08 Normalizzazione, p.10]]

---

## Forme normali
> Le forme normali sono forme che assumono le tuple per garantire alcune propietà di una base di dati e per garantire le DF.

### 1a Forma Normale
> [!QUOTE] 1a forma normale
> > La Prima Forma Normale dice che una relazione non debba avere :
> > 
> > - *Tuple ripetute*.
> >  - *Attributi composti* o *multivalore*.
>
> PDF : [[08 Normalizzazione.pdf#page=13&selection=4,0,18,25|08 Normalizzazione, p.13]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### 2a Forma Normale

> [!QUOTE] 2a forma normale
> > La Seconda Forma Normale prescrive che una relazione debba :
> > 
> > - *Essere in 1FN*.
> >  - Avere ogni attributo *non primo* che *dipenda dall'intera chiave primaria*
> >    
> >    
Una relazione in 1FN con chiave primaria *non composta* è <mark class="hltr-red">SEMPRE in 2FN</mark>.
>
> PDF : [[08 Normalizzazione.pdf#page=13&selection=4,0,18,25|08 Normalizzazione, p.13]]


> [!info]- Attributi primi e non
> Un **attributo primo** (*prime attribute*) è un attributo che <mark class="hltr-orange">appartiene ad almeno una chiave candidata</mark>.
> 
Al contrario un attributo non primo non appartiene ad alcuna chiave candidata.

La 2NF richiede che ogni attributo **non primo** dipenda funzionalmente dall’<mark class="hltr-orange">intera chiave primaria</mark>.


> [!important]- Normalizzazione in 2NF
> Per normalizzare in 2NF si segue questa procedura:
> 1. Identificare la chiave primaria e le eventuali chiavi candidate della relazione.
> 2. Identificare tutte le DF della forma $X \to A$ in cui :
>    - $X$ è un sottoinsieme proprio di una della chiave.
>    - $A$ è un attributo non-primo.
> 3. Per ogni DF trovata : 
>    - Creare una nuova relazione $R_1$ con attributi $\{X \cup A\} - π_{\{X U A\}}(R)$ (*$X$ sarà la chiave primaria di $R_1$*).
>    - Sostituire $R$ con $R_2$ rimuovendo $A$ da $R$ (*la chiave di $R_2$ rimane quella di $R$*).
>   4. Procedere così per tutte le altre DF che a sinistra hanno un sottoinsieme proprio della chiave di $R$.
>      
>    [[Updated_08 Normalizzazione.pdf#page=22|Esempio grafico : Updated_08 Normalizzazione, p.22]]

Violare la 2NF causa principalmente <mark class="hltr-orange">ridondanza dei dati</mark>, con conseguenti possibili anomalie di inserimento, cancellazione o aggiornamento.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### 3a Forma Normale

> [!QUOTE] 3a forma normale
> > La Terza Forma Normale prescrive che una relazione debba avere :
> > 
> > - *Essere in 2FN*.
> > -  Non avere attributi *non primi* che *dipendono transitivamente dalla chiave*. 
>
> PDF : [[08 Normalizzazione.pdf#page=13&selection=4,0,18,25|08 Normalizzazione, p.13]]

Si può dimostrare la validità della 3FN se per ogni DF di forma $X \to A$ vale almeno una delle seguenti condizioni:

- $X$ è superchiave di $R$.
- $A$ è attributo primo di $R$.


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Forma Normale di Boyce Codd

La BCFN è più restrittiva rispetto alla 3FN, e per questo non è sempre ottenibile (*a differenza della 3FN che è* <mark class="hltr-red">SEMPRE OTTENIBILE</mark>) poichè ne viola le DF.

> [!QUOTE] Forma normale di Boyce-Codd
> > La Forma normale di Boyce-Codd necessita che :
> > 
> > Ogni insieme di attributi di una relazione $R$ dal quale dipendono altri attributi deve essere una superchiave.
>
> PDF : [[08 Normalizzazione.pdf#page=13&selection=4,0,18,25|08 Normalizzazione, p.13]]


> [!important]- Normalizzazione in BCFN
> Per normalizzare una relazione $R(U)$ in BCFN si scompone la relazione in :
> 1. $R_1(U-A)$
> 2. $R_2(XA)$
>
> Per ogni DF $X \to A$ che viola la forma normale.
>
> Si ripete la procedura su $R_1$ o $R_2$ nel caso non siano ancora in  BCFN.


> [!example]- Esempio di normalizzazione in BCFN
> [[08 Normalizzazione.pdf#page=30|08 Normalizzazione, p.30]]


La BCNF è la forma teoricamente migliore che può assumere una relazione, poichè :


> [!quote] Propietà della BCNF
> > Ogni attributo descrive un’entità *«identificata da una chiave, dall’intera chiave, da nient’altro che dalla chiave»*
> > > ~W.~ ~Kent,~ ~Data~ ~and~ ~Reality,~ ~1978~
>
> PDF : [[08 Normalizzazione.pdf#page=33|08 Normalizzazione, p.33]] 


