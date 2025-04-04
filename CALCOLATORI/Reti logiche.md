---
Date created: 02-04-25 • 09:12
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250311_L5-RetiLogiche.pdf]]"
Related Pages:
---
## Valori logici
I computer sono realizzati tramite circuiti elettronici. ^790a6f

Trattandosi di elementi digitali avremo due livelli fondamentali: 
- **Alto** - **Asserito** `1`: associato alla tensione di alimentazione VDD 
- **Basso** - **Negato** `0`: associato alla massa

---
## Tipologie di reti logiche
Le reti logiche sono dei circuiti che trasformano alcuni valori logici in ingresso in altri valori logici in uscita.

Si hanno di due tipi:
- **Combinatorie**
	- Relazione funzionale tra I/O
	- Non hanno memoria
	- Output dipende **SOLO** dall' Input attuale
- **Sequenziali**
	- Output dipende ANCHE dalla storia degli Input passati
	- Hanno memoria ( *stato* ) 

---
## Algebra di Boole e operatori logici
Una maniera compatta di specificare le funzioni logiche combinatorie è tramite **espressioni algebriche definite con l’algebra di Boole**.

### Operatori logici di base
#### AND
Simbolo : •

| Input 1 | Input 2 | Output |
| :-----: | :-----: | :----: |
|    `1`    |    `1`    |   `1`    |
|    `1`    |    `0`    |   `0`    |
|    `0`    |    `1`    |   `0`    |
|    `0`    |    `0`    |   `0`    |
#### OR
Simbolo : +

| Input 1 | Input 2 | Output |
| :-----: | :-----: | :----: |
|   `1`   |   `1`   |  `1`   |
|   `1`   |   `0`   |  `1`   |
|   `0`   |   `1`   |  `1`   |
|   `0`   |   `0`   |  `0`   |
#### NOT
Simbolo : $\overline{X}$

| Input 2 | Output |
| :-----: | :----: |
|   `1`   |  `0`   |
|   `0`   |  `1`   |

### Algebra di Boole
#### Regole fondamentali
L'algebra di Boole ha alcune fondamentali proprietà e regole:
- **Identità :** $(A+0),(A\cdot1=A)$
- **Regola "zero e uno" :** $(A+1=1),(A\cdot0=0)$
- **Regola dell'inversa :** $(A+\overline{A}=1)(A\cdot\overline{A}=0)$
- **Propietà commutativa :** $(A+B=B+A),(A\cdot{B}=B\cdot{A})$ 
- **Propietà associativa :** $(A+[B+C]=[A+B]+C),(A\cdot[B\cdot{C}]=[A\cdot{B}]\cdot{C})$
- **Propietà distributiva :** $(A\cdot[B+C]=[A\cdot{B}]+[A\cdot{C}])$

#### Regole di De Morgan
In più esistono due regole dette di **De Morgan**:

1. $\overline{A\cdot{B}}=\overline{A}+\overline{B}$
2. $\overline{A+B}=\overline{A}\cdot\overline{B}$

Queste due regole ci dicono che se abbiamo un elemento logico **NAND** ( *NOT AND* ), oppure **NOR** ( *NOT OR* ), **tutti gli altri operatori logici si possono ricavare da questo operatore**.

##### Esempio
>Algebra di Boole standard :
>$$E = (A · B · \overline{C}) + (A · C · \overline{B}) + (B · C · \overline{A})$$

>De Morgan : 
>$$E = \overline{(\overline{A} + \overline{B} + C) \cdot (\overline{A} + \overline{C} + B) \cdot (\overline{B} + C + A)}$$

---
## Circuiti senza memoria ( Combinatori )
I circuiti logici possono operare su **piccoli quantitativi di input e output** ( *uno o due* ) o su **dati complessi**, sotto forma di array di input, anche a 32 bit o più.

> [!important] BUS
> Il circuito fisico che rappresenta vari bit di IO è il **BUS**, cioè una serie di cavi ( *di numero uguale ai bit che si vogliono rappresentare* ). Il bus da un punto di vista logico è visto come **un singolo segnale logico**.

^4a0bf4

### Porte logiche
Alla base dei circuiti all'interno di una macchina ( *computer* ) ci sono le **porte logiche**, cioè cioè dei circuiti fisici che implementano l'operatore logico corrispondente. 

![[porte_logiche.png]]

Con la rappresentazione grafica è anche possibile rappresentare il *NOT* sotto forma di puntino per combinarlo con gli altri operatori:

![[not_dot.png]]

### Altri circuiti
#### Decoder
Circuito utilizzato per decodificare una serie ( *stringa* ) di $n$ bit in un singolo bit univoco per ogni possibilità ( *$2^n$ possibilità* ).

![[decoder.png]]

Il simbolo `\` sul segnale di input rappresenta un [[#^4a0bf4|BUS]].
#### Multiplexer
Il **multiplexer** è un circuito molto importante; il suo scopo è quello di lasciar passare un singolo input tra due in entrata.

![[multiplexer.png]]

Da questa immagine si può vedere che in entrata il Multiplexer ha il segnale $A$ e $B$ ;
attraverso il segnale di *selezione* $S$, che può assumere valore $0$ o $1$ lascia passare in output $C$ solo uno dei due segnali.

Si può anche avere un multiplexer con **più di due** input attraverso l'implementazione di un [[#Decoder|Decoder]] come segnale di *selezione $S$* ...

![[multiplexer_n.png]]

... o un multiplexer con due segnali in input e uno di output, rappresentati da dei [[#^4a0bf4|BUS]].

Questo tipo di multiplexer a livello fisico non è altro che un **array di multiplexer standard** che si dividono i bit del BUS e usano un unico selettore.

![[bus_multiplexer.png]]


> [!caution] Differenza
> La differenza tra questi ultimi multiplexer è:
> - Multiplexer $N$ entrate
> 	- $N$ entrate
> 	- $1$ uscita
> 	- Selezione attraverso un Decoder
> - Multiplexer con BUS
> 	- $2$ entrate ( *Il BUS è visto come un SINGOLO segnale logico* )
> 	- $1$ uscita
> 	- Selezione attraverso un singolo segnale/bit


#### PLA ( Programmable Logic Array )
Questo circuito non ha una configurazione fissa poiché è essenzialmente la rappresentazione di una funzione logica attraverso un circuito. 

Il **PLA** è composto da due sezioni: la prima è composta da **AND** ( *detta min-termini* ) e la seconda da **OR**.

Il PLA ha due caratteristiche importanti che lo prevengono dal diventare troppo complesso:
- Ci sono porte logiche **solo** per le configurazioni di input **che producono $1$ in uscita**
- Se un *min-termine* è condiviso tra più uscite è possibile riutilizzarlo.

![[PLA.png | center]]

---
## Forma canonica SP
La **forma canonica SP** ( *Sum of Products* ) di una funzione logica in [[#Algebra di Boole]] è la forma l'output è rappresentato dalla somma delle moltiplicazioni degli input.
### Traduzione da tabella di verità
Per calcolare l'espressione logica equivalente a una tabella di verità basta prendere ogni riga di qui il risultato in output ha **valore $1$** e seguire queste regole:
- Negare ogni input con valore $0$ ( $A$ -> $\overline{A}$ )
- Moltiplicare ( **AND** ) tutti i valori di input della riga
- Sommare ( **OR** ) tutte le righe con output di valore $1$

#### Esempio

|  A  |  B  |  C  |  **D**  |  **E**  |  **F**  |
| :-: | :-: | :-: | :-----: | :-----: | :-----: |
| `0` | `0` | `0` | **`0`** | **`0`** | **`0`** |
| `0` | `0` | `1` | **`1`** | **`0`** | **`0`** |
| `0` | `1` | `0` | **`1`** | **`0`** | **`0`** |
| `0` | `1` | `1` | **`0`** | **`1`** | **`0`** |
| `1` | `0` | `0` | **`1`** | **`0`** | **`0`** |
| `1` | `0` | `1` | **`0`** | **`1`** | **`0`** |
| `1` | `1` | `0` | **`0`** | **`1`** | **`0`** |
| `1` | `1` | `1` | **`1`** | **`0`** | **`1`** |

>$D = (\overline{A}\cdot\overline{B}\cdot{C})+(\overline{A}\cdot{B}\cdot\overline{C})+(A\cdot\overline{B}\cdot\overline{C})+(A\cdot{B}\cdot{C})$
$E=\ ...$
$F=\ ...$
### Traduzione a circuito con il PLA
Una volta che si ottiene la forma canonica la si può tradurre in circuito con il [[#PLA ( Programmable Logic Array )|PLA]].

#### Esempio
>$D=A+B+C$
>$E = (A\cdot{B}\cdot\overline{C})+(A\cdot\overline{B}\cdot{C})+(\overline{A}\cdot{B}\cdot{C})$
>$F= A\cdot{B}\cdot{C}$
>
>![[pla_esempio_1.png]]

---
## Minimizzazione di una rete
### Costo di una rete
Per **costo** di una rete logica si intende la somma del numero di porte e e del numero di ingressi della rete.

> [!info] Complessità
> Essenzialmente il costo simboleggia la **complessità/efficienza di una rete**. Diverse implementazioni della **stessa rete** possono avere **costi diversi**.

### Processo di minimizzazione
La **minimizzazione**, cioè la **riduzione del costo** della rete, talvolta è banale; in altri casi è necessario applicare le regole algebriche in maniera meno intuitiva.

Esistono metodi di minimizzazione sistematici basati sull' **applicazione iterativa di regole** o metodi basati su **rappresentazioni grafiche** ( *mappe di Karnaugh* ), ma si applicano solo a casi più semplici.

Questo processo è detto **sintesi logica**.
#### Esempio
>$f\ (A,B,C)$
>$=(\overline{A}\cdot\overline{B}\cdot\overline{C})+(\overline{A}\cdot\overline{B}\cdot{C})+({A}\cdot\overline{B}\cdot\overline{C})+({A}\cdot\overline{B}\cdot{C})$
> $=\overline{A}\cdot\overline{B}\cdot(\overline{C}+{C})+{A}\cdot\overline{B}\cdot(\overline{C}+{C})$
> $={A}\cdot\overline{B}+\overline{A}\cdot\overline{B}$
> $=\overline{B}\cdot(\overline{A}+A)$
> $=\overline{B}$

$$(\overline{A}\cdot\overline{B}\cdot\overline{C})+(\overline{A}\cdot\overline{B}\cdot{C})+({A}\cdot\overline{B}\cdot\overline{C})+({A}\cdot\overline{B}\cdot{C})=\overline{B}$$

---
## Reti con memoria ( Reti sequenziali )
All'interno di un calcolatore la funzione di **memorizzazione** dei dati è fondamentale per svolgere varie operazioni.

La memoria in una rete logica si ottiene con una *“reazione”*, cioè prendendo l'output è passarlo nuovamente come input della rete. In questo modo si forma un *loop* in cui **gli ingressi dipendono dalle uscite e viceversa**.

Questa *reazione* complica in modo significativo l’analisi e la sintesi di una rete logica.

### Circuiti con memoria ( Sequenziali )
#### Elemento Bistabile
L'**elemento bistabile** è il circuito alla base della memorizzazione. Dato un bit in input su $Q$ questo verrà tenuto in memoria nel circuito; su $Q+$ si trova il suo valore normale e su $Q-$ il suo valore negato.

![[bistabile.png | center | 320]]

### R-S Latch ( Set / Reset )
L' **RS-Latch** prende l' [[#Elemento Bistabile]] è gli aggiunge le funzioni di **Set** e **Reset** per renderlo più utilizzabile.

![[rs_latch.png]]

Quando il bit di **Set** ha valore **$1$** e **Reset** ha valore **$0$** si avrà sull'output **$Q+$ il valore $1$** e su **$Q-$ il valore $0$** ; al contrario se **Reset** è a **$1$** e **Set** a **$0$** si avrà **$Q+$ a 0** e **$Q-$ a 1**.

Quando entrambi i valori di input sono a **$0$** si è in uno stato di **memorizzazione** dove i bit di uscita non variano.

| S | R |     $Q+$     |     $Q-$     |
| :---: | :---: | :--------------: | :--------------: |
|  `0`  |  `0`  |   *No change*    |   *No change*    |
|  `0`  |  `1`  |       `0`        |       `1`        |
|  `1`  |  `0`  |       `1`        |       `0`        |
|  `1`  |  `1`  | **INDECIDIBILE** | **INDECIDIBILE** |

Ci sono due problemi fondamentali con questo tipo di circuito di memorizzazione che permettono uno stato **indecidibile**:
- Entrambi i bit di entrata **possono avere valore 1 allo stesso tempo**.
- A causa delle uscite reazionate e al tempo non nullo della trasmissione dei segnali i bit di entrata **non hanno un tempo di cambio fisso**.  

> [!important] Indecibilità
> Quando non si può affermare con sicurezza in che stato sia il bit di uscita rendendolo ambiguo. Nel caso del **RS-Latch** se entrambi i bit di entrata sono a **$1$**  il bit di uscita continuerebbe a cambiare di valore.

### Gated Latch
Il **Gated Latch** è una versione **temporizzata** dell' [[#R-S Latch ( Set / Reset ) |RS-Latch]] che ne risolve il problema del *tempo di cambio non fisso*.

![[gated_latch.png | center | 600]]

![[sym_g_latch.png | center]]

La temporizzazione avviene attraverso un segnale alternato tra valore di $0$ e $1$ a tempi fissi chiamato **Clock**. Questo segnale scandisce il tempo in cui i valori di input possono essere variati ( *Ingresso di abilitazione* ).

![[tempo_g_latch.png | center | 450]]

Tuttavia questo circuito non risolve il problema posto dall'avere **entrambi i bit di entrata a valore $1$**.

### D Latch ( Data )
Il **D latch** è un tipo di latch che a sua volta prende il [[#Gated Latch]] e gli risolve il problema di avere entrambi i bit di entrata con valore $1$.

Per fare ciò si serve di una porta **NOT** che si sostituisce al *Set* e *Reset* e forza **l'uno essere la negazione dell'altro**.

![[D_latch.png | center | 500]]

![[sym_D_latch.png | center | 500]]

### Flip-Flop
Il **Flip-Flop** è un circuito formato da [[#D Latch ( Data )|D Latch]] la cui funzione è praticamente uguale a quest'ultimo, con la differenza che il *tempo di cambio* non avviene quando il valore del **Clock** è a **$1$**, bensì quando è a **$0$**.

![[flip-flop.png | center | 500]]

![[tempo_flip-flop.png | center]] 

![[sym_flip-flop.png | center | 200]]

Inoltre la conformazione **Master-Slave** gli fornisce un ulteriore caratteristica di temporizzazione, cioè di essere **Edge Triggered**. 

Questo perchè il valore del **Master** può cambiare di stato in qualsiasi momento in cui il Clock ha valore **$1$** , tuttavia lo **Slave** deve attendere il valore **$0$** del Clock. Una volta che avviene il cambio di stato del clock **lo Slave si aggiorna una singola volta sul fronte di variazione** dato che il Master è bloccato dal Clock con valore $0$.

> [!caution] Edge Triggered
> Essere **Edge Triggered** definisce la proprietà di un circuito di variare il suo stato **NON** in base alla tensione attuale del Clock ma bensì rispettivamente al **fronte di variazione** di quest'ultimo ( *Quando avviene il cambio di stato 1->0 o 0->1* ).

### Registri
I **Registri** sono il circuito alla base della memoria **all'interno** ( *fisicamente* ) della **CPU**. Sono impiegati per memorizzare **word** di dati ( *$1$ byte / $8$ bit* ) e accedervi velocemente durante le operazioni.

Sono formati da un array di **Latch D edge triggered** ( [[#Flip-Flop]] ), in particolare caricano gli input in memoria sul **fronte di salita del Clock**.

![[registro.png | center | 400]]

Oltre alla funzione di memorizzazione operano come una **barriera tra input e output** per fornire le **funzioni del Clock** alle operazioni nella **CPU**. 
Sul fronte di salita **memorizzano gli input**, mentre **l'output è sempre disponibile**.

![[funz_registro.png | center]]

Il vantaggio dell'**Edge triggered** in questo caso è di prevenire situazioni di **corsa** ( *Rush situation* ).

> [!important] Rush situation
> Situazione in cui un valore/stato è deciso da quale input/output **arriva prima**.
> 
> Se, in assenza del registro, l'input $Y$ ( *nell'esempio* ) arrivasse troppo presto, il dato $X$ sarebbe sovrascritto, e l'operazione in cui era richiesto otterrebbe il dato $Y$ errato. 

---
## Macchine a stati
Circuiti in cui: 
- Lo stato successivo **dipende da quello presente e dall'input**. 
- L'output **dipende dallo stato presente e dall'input o solo dallo stato presente**.
### Circuito accumulatore
Esempio di macchina a stati in cui a ogni ciclo l'input viene caricato in un registro e accumulato nuovamente.

![[circ_accumul.png | center]]

![[tempo_circ_accumul.png | center]]