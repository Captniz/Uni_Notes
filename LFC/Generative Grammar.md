---
Date created: 17-09-26 • 11:05
tags:
  - LFC
Related PDF/DOC:
  - "[[2026_2_grammars_2p.pdf]]"
Related Pages:
---
# La grammatica
## Definizione

> I programmi sono scritti in linguaggi che aderiscono ad una grammatica formale. Se un programma aderisce correttamente alla grammatica viene detto *sintatticamente legale.*
> 
> La grammatica da <mark class="hltr-orange">forma</mark> ( *non significato* ) alle espressioni.


> [!QUOTE] Vocabolario
> > Set di simboli, *terminali* and *non-terminali*.  I simboli terminali sono detti **tokens** dell'output dell'analisi lessicale.
>
> PDF : [[2026_2_grammars_2p.pdf#page=1&selection=18,0,29,66|2026_2_grammars_2p, p.1]]

> [!QUOTE] Produzioni
> > Regole per trasformare le stringhe di token. Una stringa che deve essere rimpiazzata deve contenere almeno un  non-terminale.
>
> PDF : [[2026_2_grammars_2p.pdf#page=2&selection=43,0,49,20|2026_2_grammars_2p, p.3]]

In un vocabolario abbiamo che ...
- Simboli terminali sono rappresentati con lettere minuscole.
- Simboli non-terminali sono rappresentati con lettere maiuscole.
- Il simbolo di inizio è il primo simbolo non terminale ( *quello più a sinistra* ).
- Il carattere $\epsilon$ definisce una parola vuota. 

> [!example]- Esempio di vocabolario
> Sia il vocabulario `{S, a, b}`con `a` e `b` terminali.

Il **simbolo di inizio** genera la prima trasformazione.
Le trasformazioni di una stringa avvengono fino a quando non esiste più alcun carattere *non-terminale* all'interno di essa.

Il processo di trasformazione completa di una stringa è detto **derivazione**.


> [!example] Esempio di processo di trasformazione
> ![[EMBED/2026_2_grammars_2p.png]]
> [[2026_2_grammars_2p.pdf#page=11&rect=126,132,446,300|2026_2_grammars_2p, p.21]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Definizioni formali
#### Grammatica

> Una grammatica è una tupla ...
> $$ (V,T,S, \mathcal P )$$
> Con ...
> - $V$ : Vocabolario di simboli terminali e non
> - $T$ : Set di terminali
> - $S$ : Simbolo di inizio ( *$S\in V-T$* )
> - $\mathcal P$ : Set di produzioni


> [!warning]- Convenzioni di notazione
> - Lettere maiuscole sono simboli non-terminali ($X\in V-T$)
> - Lettere minuscole sono simboli terminali ($x\in T$)
> - Lettere <mark class="hltr-blue">greche</mark> minuscole sono stringhe di simboli del vocabolario ($\alpha \in V^*$).
>   
> > [!info] Stringhe di simboli
> >   Sia $H$ un set di simboli, la notazione $H^*$ definisce <mark class="hltr-red">zero</mark> o più concatenazioni di elementi di $H$.
> >   
> >   Quindi $a$ può essere anche $\epsilon$.

#### Produzione
> Una produzione ha forma generale ...
> $$\delta \to \beta$$
> dove $\delta$ è detto *driver* e $\beta$ è detto *body*.
> 
> Inoltre si ha che ...
> - $\delta \in V^+$ ( Sia $H$ un set di simboli, la notazione $H^*$ definisce <mark class="hltr-orange">uno</mark> o più concatenazioni di elementi di $H$. Quindi $\delta \ne \epsilon$).
> - $\delta$ contiene almeno un non-terminale.


#### Linguaggio
> Data la grammatica ...
> $$\mathcal G = (V,T,S,\mathcal P)$$
> il linguaggio generato da esso è definito come ...
> $$\mathcal L(\mathcal G)=\{w|w \in T^* \text{ and } S\implies^*w\}$$

---
## Proprietà delle grammatiche
### Grammatiche senza contesto o libere

>Una grammatica viene detta *context-free* se ogni sua produzione prende la forma :
$$A\to\beta$$
> Cioè se il driver di tutte le produzioni è un non-terminale semplice.


Un linguaggio è context-free se lo è la grammatica che lo genera.

#### Derivazioni canoniche
In una grammatica libera ( *context-free* ) ogni produzione sostituisce un unico non-terminale.

Una derivazione di una stringa è detta **canonica** se si esegue sempre la trasformazione più a destra (*verso la fine della stringa* ) possibile. ( *Vale anche l'opposto, eseguendo sempre la trasformazione più a sinistra*).

##### Alberi di derivazione
Questi alberi hanno le seguenti caratteristiche :
- Il simbolo di inizio è la radice.
- Sia la trasformazione $A\to \{X_{1}\dots X_{n}\}$, allora il nodo $A$ avrà come figli i nodi $\{X_{1}\dots X_{n}\}$.
- I terminali e il simbolo $\epsilon$ sono foglie dell'albero.
- La stringa derivata è alla frontiera dell'albero.


> [!example] Rappresentazione grafica dell'albero di derivazione
> ![[EMBED/2026_2_grammars_2p 1.png]]
[[2026_2_grammars_2p.pdf#page=16&rect=163,129,449,273|2026_2_grammars_2p, p.31]]

### Grammatiche ambigue
> Una grammatica è detta *ambigua* se esiste $w\in \mathcal L(\mathcal G)$ che può essere generata da due derivazioni canoniche distinte dello stesso tipo ( *o entrambe destre o entrambe sinistre* ).


> [!example] Esempio di grammatica ambigua
> Sia la grammatica ...
> $$\begin{array}{}
> E\implies n \\
> E\implies E*E \\
> E\implies E+E
> \end{array}$$
> 
> ![[EMBED/2026_2_grammars_2p 2.png]]
[[2026_2_grammars_2p.pdf#page=18&rect=171,130,453,292|2026_2_grammars_2p, p.35]]
>
> ![[EMBED/2026_2_grammars_2p 3.png]]
>[[2026_2_grammars_2p.pdf#page=18&rect=164,526,453,670|2026_2_grammars_2p, p.35]]

L'ambiguità di una grammatica è <mark class="hltr-red">indecidibile</mark>.
Nessun algoritmo può provare definitivamente se una grammatica è ambigua o no.
