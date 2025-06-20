---
Date created: 18-06-25 • 15:54
tags:
  - Programmazione_2
Related PDF/DOC:
  - "[[15_UML.pdf]]"
Related Pages:
---
## Stesura dello schema
### Diagramma di una classe
Questo diagramma definisce una classe e le sue relazioni : 
- Associazione ( *uso* ).
- Generalizzazione ( *ereditarietà* ). 
- Aggregazione ( *contenimento* ).

Ogni classe ha tre componenti fondamentali :
- **Nome** 
- **Attributi** : Caratteristiche / Stato / Variabili
- **Metodi**

---

![[uml_class.png | center]]


---

#### Notazione e caratteristiche
##### Nome
Nel nome è specificato se una classe è :
- **Normale** 
- **Astratta** : Scritto in *italico*
- **Interfaccia** : Specificato con *\<\<interface\>\>*
- **Utility** : Specificato con *\<\<utility\>\>*


> [!info]- Stereotipi
> Gli stereotipi specificano il tipo di alcune classi :
> - **\<\<interface\>\>** : Specifica un interfaccia.
>   **\<\<utility\>\>** : Indica una classe di *"utility"*, di cui ogni metodo è `static`.

##### Attributi e metodi
Per entrambi vale che ...
- **VIsibility** può essere :
	- `-` Private
	- `+` Public
	- `#` Protected
	- `~` Package
- I campi con *[]* possono essere anche **vuoti o assenti**.
- Se sottolineati sono da intendere **Static**
###### Attributi
Per gli attributi si ha la seguente sintassi ...

```
Attribute ::= <visibility> <name>: <type> [= default-value]
```

###### Metodi
Per i metodi si ha la seguente sintassi ...

```
Method :=  <visibility> <name> ([parameter list]) : <return type>

Parameter := <name> : <type>
```

### Associazioni tra classi
Indica una **relazione** tra classi in modo bidirezionale. 

Si ha sempre : 
- Un **nome**, solitamente un verbo.
- La **[[#^4792d9|Molteplicità]]**.

Invece è opzionale
- Una **direzione**, se non evidente.
- Un **[[#^0c02da|Ruolo]]**.
- **Attributi** come una classe, nel caso siano necessari.

> [!important]- Molteplicità
> La molteplicità, indica **il numero di istanze** che prendono parte alla relazione. In particolare specificano : 
> - Se l’associazione è **obbligatoria** oppure no.
> - Il numero **minimo e massimo di oggetti** che possono essere relazionati ad un altro oggetto.
>   
> ---
> ![[molteplicita.png | center]]  
> 
> ---
> 
> La sintassi come vista in figura è una **coppia ( *O un singolo in alcuni casi* ) di interi** per ogni classe nella relazione.

^4792d9

> [!important]- Ruolo
> Un ruolo è un **etichetta** aggiuntiva che si applica alle classi di una relazione per specificarne il ruolo all'interno di essa.
> 
> Questo è utile per *Auto-Associazioni* o *Associazioni multiple* ...
> 
> ---
> 
> ![[ruolo.png | center]]
> 
> ---

^0c02da

Esistono alcune sottocategorie di associazioni ...

#### Aggregazione
È una forma particolare di associazione in cui **una parte è in relazione con un oggetto**.

Nel senso che la *parte* <mark class="hltr-red">non può esistere</mark> senza l'*oggetto intero*; in questo caso <mark class="hltr-orange">la molteplicità della parte sarà sempre 1</mark>. 

---

![[aggregazione.png | center]]

---

#### Associazione riflessiva
Un’associazione è tale se coinvolge **oggetti della stessa classe**, potrebbe dirsi un associazione *ricorsiva*.

---

![[riflessiva.png | center]]

---


### Ereditarietà tra classi - Generalizzazione

#### Ereditarietà semplice

Le classi <mark class="hltr-orange">padri</mark> vengono *"puntate"* dai <mark class="hltr-purple">figli</mark> in un diagramma UML.

---

![[generalizzazione.png | center]]

---

#### Interfacce

Se si <mark class="hltr-blue">implementa</mark> un interfaccia si utilizza una *freccia tratteggiata con punta vuota*.

Mentre se la classe <mark class="hltr-green">dipende</mark> dall'interfaccia, cioè non ne implementa i metodi ma li usa, si usa una *freccia trattegiata semplice*.

---

![[interface_uml.png | center]]

---

#### Package
I package sono *“contenitori”* di classi ad alta coesione, all’interno si hanno Interfacce precise e limitate.