---
Date created: 04-06-25 • 15:11
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250506_L16-Processore_1.pdf]]"
  - "[[250509_L17-Processore_2.pdf]]"
Related Pages:
---
## Panoramica del processore
Le prime due fasi dell'esecuzione di un'istruzione, *per ogni istruzione a prescindere dall'[[Assembly#ISA|ISA]]*, sono :
- **Prelievo** dell'istruzione dalla memoria.
- **Lettura dei registri** operandi.

I passi successivi dipendono dalla *specifica istruzione*. Le istruzioni della stessa classe ( *Aritmetiche, Accesso alla memoria, Salto, ...* ) hanno passi simili tra loro.

### Basi dell'esecuzione delle istruzioni

> [!error]- Assunzione semplificativa - Clock
>  Assumiamo per questa spiegazione che ...
>  Tutte le istruzioni si svolgano in un singolo ciclo di clock ( *lungo abbastanza* ).

<mark class="hltr-red">Tutti</mark> le istruzioni usano la **ALU** ( __A__*rithmetic* __L__*ogic* __U__*nit* ) dopo aver letto gli operandi : 
- Le istruzioni di accesso alla memoria per **calcolare l’indirizzo**. 
- Le istruzioni aritmetico/logiche per **eseguire quanto previsto dall’istruzione**.
- I salti condizionati per **effettuare il confronto**.

Dopo la ALU iniziano le differenze vere e proprie:
- Le istruzioni di accesso alla memoria **richiedono o salvano il dato** in memoria.
- Le istruzioni aritmetiche/logiche **memorizzano il risultato nel registro target**.
- Le istruzioni di salto condizionato **cambiano il valore del registro PC** secondo l’esito del confronto.

>[!info] Schema basilare del Datapath/Percorso della CPU
>Questo schema è ovviamente incompleto e semplificato.
>
>![[schema_cpu_base.png]]

Molti dei percorsi mancanti o semplificati nello schema dovrebbero in realtà includere un circuito selettore ( *[[Reti logiche#Multiplexer|Multiplexer]]* ) che permette di scegliere tra due dati ( *Es. Scegliere tra PC+4 di default o usare l'offset di una funzione di salto* ).

> Ma il multiplexer come decide la selezione corretta per l'istruzione?

Il segnale che decide la selezione è un **segnale di controllo**, sono presenti all'interno della [[Architettura RISC-V#Codifica delle istruzioni|Codifica delle istruzioni]] e vengono trasmesse al multiplexer attraverso un [[Reti logiche#Decoder|Decoder]]. 

Questi segnali di controllo non sono usati solo per il multiplexer, ma anche gli altri blocchi funzionali hanno ulteriori ingressi di controllo, ad esempio :
- La **ALU** ha diversi ingressi per **decidere quale operazione** effettuare.
- Il **Banco registri** ha degli ingressi per **decidere se scrivere o meno** in un registro.
- La **memoria** dati ha degli ingressi per **decidere se vogliamo effettuare letture o scritture**.


> [!info] Schema intermedio della CPU
> Aggiungendo quello che abbiamo appena letto allo schema otteniamo ...
> 
> ![[schema_cpu_medio.png]]


> [!important]- Appunto sulle reti logiche - Elementi di stato
> Definiamo **rete logica combinatoria** un circuito composto di porte logiche che produce un output che è una funzione statica dell’input. 
> 
> Esistono inoltre elementi detti *"di stato"*. Questi elementi del circuito se vengono salvati in un punto `x` nel tempo e poi successivamente ricaricati, il circuito riprenderà la sua funzione esattamente dal punto `x` senza variazioni.  
> 
> Nel nostro caso gli elementi di stato sono: 
> - Registri 
> - Memoria dati 
> - Memoria istruzioni
>   
> Gli elementi di stato sono detti **sequenziali** perché l’output dipende, oltre che dagli input, dalla storia ( *Sequenza* ) degli ingressi precedenti. 
> 
> Gli elementi di stato hanno *almeno* due ingressi: 
> - Il valore da immettere nello stato.
> - Un clock con cui sincronizzare le transizioni di stato.

#### Temporizzazione

Durante tutte queste operazioni nella CPU è importante stabilire una **temporizzazione**. La metodologia di temporizzazione ci dice quando i segnali possono essere letti o scritti in relazione al clock.

La temporizzazione è importante per evitare sovrascritture o letture prima del tempo dei dati.

La metodologia più utilizzata è quella **sensibile ai fronti di salita**. Questo significa che i dati presi dagli elementi di stato sono **relativi al ciclo precedente**.

Il tempo di clock T <mark class="hltr-red">deve</mark> essere scelto in modo da dare tempo ai dati di **attraversare la rete combinatoria**.

Grazie alla temporizzazione possiamo risolvere <mark class="hltr-orange">l'indecidibilità di una logica combinatoria</mark>.

>[!example]- Esempio della risoluzione dell'indecidibilità
>In questo schema...
>![[indecidibile.png | center]]
>Possiamo vedere che :
>$$s=f(s)$$
>Quindi $s$ è in uno stato indecidibile in quanto ricorsivo.
>
>Aggiungendo la **temporizzazione**, con un tempo di clock $T$ abbiamo invece :
>$$s(t+T)=f(s(t))$$
>che è decidibile.


### Datapath
Il **Datapath** è il percorso che prendono i dati per svolgere un istruzione.

Durante l'esecuzione abbiamo il requisito di eseguire ogni istruzione in un **ciclo di clock**, possiamo usare un’unità funzionale <mark class="hltr-red">una singola</mark> volta per ciclo.

Vedremo ora le varie componenti di questo, divise per sezioni
#### Prelievo dell'istruzione
I tre principali componenti operati in questa sezione sono :
- Memoria istruzioni
- Program Counter
- Addizionatore ( *ALU specializzata per l'incremento del PC* )

La circuiteria di questa sezione può essere rappresentata in questo modo : 

---

![[prelievo_istruzioni.png | center]]

---

#### Accesso ai dati
L'accesso ai dati dei **registri** e della **memoria** è gestito da questo circuito :

---

![[circuito_accesso_dati.png | center]]


> [!example]- Esempio di circuiteria completa ( Aggiunta Jump )
> ![[accesso_dati_completo.png]]


---

Data la sua complessità lo divideremo nelle su due sezioni corrispettive:

##### Accesso ai registri 
L'accesso ai registri è ovviamente adoperato dalle istruzioni di **tipo R** in RISC-V ( *Register* ).

Prendiamo come esempio ...
```c
add x5, x6, x7
```

di cui la codifica sarà :

| **funz7** | **rs2** | **rs1** | **funz3** | **rd**  | **codop** |
| :-------: | :-----: | :-----: | :-------: | :-----: | :-------: |
|  *7 bit*  | *5 bit* | *5 bit* |  *3 bit*  | *5 bit* |  *7 bit*  |
|   31:25   |  24:20  |  19:15  |   14:12   |  11:7   |    6:0    |
| `0000000` | `00111` | `00110` |   `000`   | `00101` | `0110011` |

I blocchi logici e la circuiteria necessaria è la seguente :

---

![[datapath_aritmetiche.png | center]]

---

##### Accesso alla memoria
Prendiamo come esempio le istruzioni ...
```c
ld x5, <offset tipo I> (x6)
sd x5, <offset tipo S> (x6)
```

di cui la codifica sarà :

> [!info] Load ( *Tipo I* )
>
| offset | rs1   | funz3 | rd    | codop |
| ------ | ----- | ----- | ----- | ----- |
| 12 bit | 5 bit | 3 bit | 5 bit | 7 bit |


> [!info] Save ( *Tipo S* )
>
| offset [11:5] | rs2   | rs1   | funz3 | offset [4:0] | codop |
| ------------- | ----- | ----- | ----- | ------------ | ----- |
| 7 bit         | 5 bit | 5 bit | 3 bit | 5 bit        | 7 bit |

Per entrambe si dovrà calcolare l'indirizzo di memoria dato da `offset + x6`, che richiedere accedere ai registri; quindi servono **ALU** e **banco registri**, oltre che alla memoria.

>[!warning]- Estensione dell'offset
>L’offset, essendo memorizzato in un campo a **12 bit**, <mark class="hltr-red">deve</mark> essere esteso a **64 bit** ( *Replicando per 42 volte il bit di segno* ) per rendere possibile l'operazione di somma.
>
>Questo avviene tramite un **componente apposito per l'estensione del segno**.

Dato che abbiamo già visto ALU e banco registri vediamo lo schema della **memoria** :

---

![[memoria_dati.png | center]]

---

#### Selezione del PC - Datapath di un salto

Prendiamo come esempio l' istruzione ...
```c
beq x5, x6, offset
```

codificata come :

| offset [11:5] | rs2   | rs1   | funz3 | offset [4:0] | codop |
| ------------- | ----- | ----- | ----- | ------------ | ----- |
| 7 bit         | 5 bit | 5 bit | 3 bit | 5 bit        | 7 bit |

Anche in questo caso bisogna sommare al PC l’offset ( *Dopo averlo esteso* ), che consente di fare salti con range : $-2^{12} \to 2^{12}$.

Ci sono alcune cose da tenere in mente quando si svolge un salto :
- L’architettura specifica che l’*indirizzo di base* per il calcolo dell’indirizzo di salto è quello dell’<mark class="hltr-red">istruzione di salto stessa</mark>.
- L’architettura stabilisce che il campo `offset` sia spostato di **1 bit a sinistra** per fare sì che l’offset codifichi lo spiazzamento in numero di **mezze parole** ( *Aumentando cosi il range dell'offset* ).
	- La ragione per lo shift di *1* invece che di **2** è dovuta alla presenza di istruzioni compresse a 16 bit.

Vediamo quindi lo schema del circuito corrispondente :

---

![[datapath_salto.png]]

---

### Segnali di controllo
Ora che abbiamo visto la circuiteria dei blocchi funzionali, per ottenere una panoramica completa, dobbiamo aggiungere la **circuteria di controllo**.

#### ALU
La ALU viene usata per ogni tipo di istruzione per vari scopi :
- Effettuare operazioni logico-aritmetiche. 
- Calcolare indirizzi di memoria. 
- Confronti attraverso la sottrazione ( *Per i salti* ).

Ognuna di queste operazioni corrisponde a un set di valori di controllo :

| Control | Operazione  |
| :-----: | :---------: |
|  `0000`   |     AND     |
|  `0001`   |     OR      |
|  `0010`   |    Somma    |
|  `0110`   | Sottrazione |
La selezione dei segnali di controllo avviene grazie a un <mark class="hltr-purple">unità specifica</mark> ( **Unità di controllo della ALU** ) che riceve in input i campi dell'istruzione `funz3` e `funz7`, più due bit detti `ALUop`.


> [!info]- Valori di ALUop 
> - `00` : Somma per istruzioni di accesso alla memoria.
> - `01` : Sottrazione per salti
> - `10` : Operazione di tipo R ( *Funzione specificata da `funz3` e `funz7`* ).

Essendo che ALUop viene generato da un unità di controllo diversa, questa selezione è detta **decodifica a livelli multipli**, in particolare ...
- <mark class="hltr-orange">Livello 1</mark> : ( **Unita di controllo** ) Genera `ALUop` per il *livello 2*.
- <mark class="hltr-blue">Livello 2</mark>  : ( **Unità di controllo della ALU** ) Genera i bit di controllo della ALU partendo da  `funz3`, `funz7` e `ALUop`.

Questa decodifica è a opera di una rete logica *combinatoria*, con un range di $2^{12}$ combinazioni ( *12 bit in input* ).

> [!example]- Tabella di verità semplificata
> ![[tabella_verita_alu_control.png | center ]]

#### Unità di controllo
L'unità di controllo genera la maggior parte dei segnali di controllo della CPU; questi segnali impartiscono le funzioni da utilizzare alle altre unità.


> [!NOTE]- Schema dei segnali di controllo fin ora
> ![[tabella_segnali_contr.png | center]]
> 
> ---
>
>![[schema_segnali_contr.png | center ]]

L'unità di controllo è una rete combinatoria che prende come input il **codice operativo** ( `codop` ) dell’istruzione e genera i segnali.

Per vedere il suo funzionamento vediamo come vengono decodificate varie istruzioni :

##### ADD ( Tipo R )

```c
add x5, x6, x7
```

I passi per eseguire un'istruzione di tipo R sono :
1. **Prelevare l’istruzione** dalla memoria e incrementare il PC.
2. **Leggere `x6` e `x7`** dal register file mentre l’unità di controllo principale calcola il valore da attribuire alle linee di controllo. 
3. **Attivare la ALU** con in input i dati dal register file usando alcuni bit del codice operativo per selezionare l’operazione.
4. **Memorizzare il risultato** in `x5`.

La decodifica sarà quindi ...

| ALUsrc | MemToReg | RegWrite | MemRead | MemWrite | Branch | ALUop1 | ALUop2 |
| :----: | :------: | :------: | :-----: | :------: | :----: | :----: | :----: |
|  `0`   |   `0`    |   `1`    |   `0`   |   `0`    |  `0`   |  `1`   |  `0`   |

E gli effetti sulla CU ( *Control Unit* ) : 

---

![[schema_c_add.png | center]]

---

##### LOAD ( Accesso alla memoria )

```c
ld x5, offset(x6)
```

I passi per eseguire un'istruzione di tipo R sono :
1. **Prelevare l’istruzione** dalla memoria e incrementare il PC.
2. **Leggere `x6`** dal register file. 
3. **Sommare indirizzo e offset ( *esteso* )** attraverso la ALU.
4. **Prelevare il dato dalla memoria dati** attraverso l'indirizzo.
5. **Scrivere in `x5`** il dato letto.

La decodifica sarà quindi ...

| ALUsrc | MemToReg | RegWrite | MemRead | MemWrite | Branch | ALUop1 | ALUop2 |
| :----: | :------: | :------: | :-----: | :------: | :----: | :----: | :----: |
|  `1`   |   `1`    |   `1`    |   `1`   |   `0`    |  `0`   |  `0`   |  `0`   |

E gli effetti sulla CU ( *Control Unit* ) : 

---

![[schema_c_mem.png | center]]

---

##### BEQ ( Salto condizionale )

```c
beq x5, x6, offset
```

I passi per eseguire un'istruzione di tipo R sono :
1. **Prelevare l’istruzione** dalla memoria e incrementare il PC.
2. **Leggere `x5` e `x6`** dal register file. 
3. <mark class="hltr-blue">Allo stesso tempo avviene :</mark>
	1. **Sottrarre `x5` da `x6` per il confronto** attraverso la ALU.
	2. **PC viene sommato con l'offset ( *esteso* )**
4. **Selezione del PC ( *Tra PC+4 e PC+offset* )** attraverso l'output `zero` dell' ALU.

La decodifica sarà quindi ...

| ALUsrc | MemToReg | RegWrite | MemRead | MemWrite | Branch | ALUop1 | ALUop2 |
| :----: | :------: | :------: | :-----: | :------: | :----: | :----: | :----: |
|  `0`   |   `X`    |   `0`    |   `0`   |   `0`    |  `1`   |  `0`   |  `1`   |

E gli effetti sulla CU ( *Control Unit* ) : 

---

![[schema_c_jump.png | center]]

---

### Considerazione conclusiva
Abbiamo visto come le istruzioni in questi esempi vengono <mark class="hltr-red">eseguite tutte in un ciclo</mark>.


> [!error] Problemi con questa semplificazione
> 1. A dettare il **clock** sono le istruzioni più <mark class="hltr-orange">lente</mark> ( *Quelle di accesso alla memoria* ).
> 2. Se si considerano **istruzioni più complesse** di quelle che abbiamo visto, le cose peggiorano. 
> 3. **Non si riesce a fare ottimizzazioni** aggressive sulle operazioni fatte più di frequente.
