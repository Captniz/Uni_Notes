---
Date created: 05-06-25 • 16:17
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250510_L18-Pipeline.pdf]]"
Related Pages:
---
## L'utilizzo della pipeline

La **pipeline** è un *protocollo/metodo* per risolvere i [[Il Processore#Considerazione conclusiva|problemi]] legati all'esecuzione di <mark class="hltr-orange">una singola istruzione per ciclo</mark>. 

La *catena di montaggio* o **pipeline** è stata ideata da Henry Ford in un contesto industriale.

In un processo dove si devono svolgere multipli *cicli* con le stesse fasi, la pipeline consiste nell'esecuzione delle fasi in maniera **contemporanea**, senza aspettare che un ciclo finisca per iniziarne uno nuovo.

La differenza tra la pipeline e l'esecuzione in sequenza normale è più chiara con un grafico :

---

> [!example] Grafico esempio
>![[pipeline_text.png | center]]

---

La pipeline permette di raggiungere un miglioramento del <mark class="hltr-orange">[[#^d28592|troughtput]]</mark> di `N` volte sopra la tecnica in sequenza, dove `N` è il numero di fasi in un ciclo ( *Nell esempio 4* ).

> [!info]- Troughtput
> E' la **quantità di lavoro o di dati completati**, elaborati o trasmessi in un dato **intervallo di tempo**.

^d28592

## Implementazione della pipeline in RISC-V

### Implementazione

In RISC-V quindi si avranno **5 fasi**, corrispondenti a :
1. Prelievo dell’istruzione dalla memoria.
2. Lettura dei registri e decodifica dell’istruzione.
3. Esecuzione di un’operazione ( *Tipo R* ) o calcolo di un indirizzo.
4. Accesso a un operando nella memoria dati ( *opzionale* ). 
5. Scrittura del risultato in un registro ( *opzionale* ).

Di cui solo le **prime 3** sono *"obbligatorie"*.

---

Guardando le operazioni viste in precedenza vediamo che la `load`  impiega il tempo maggiore ad eseguire.

> [!example]- Tabella tempi delle istruzioni
> ![[tempi_istruzioni.png | center]]

In un processore <mark class="hltr-orange">non-pipelined</mark> sarebbe <mark class="hltr-red">imperativo</mark> scegliere un tempo di clock che renda possibile l'esecuzione dell'**istruzione** `load` ( `>= 800ps` ).

Tuttavia in un processore <mark class="hltr-blue">pipelined</mark> non si guarda l'istruzione più lunga, bensì la **fase** ( *Delle 5 viste in precedenza* ) più lunga, che in questo caso è di `200ps`.

> [!example] Esempio dell'implementazione della pipeline in RISC-V 
> ![[pipeline_riscv.png | center]]

La formula...
$$
Tempo\ pipeline = \frac{Tempo\ senza\ pipeline}{N\ fasi}
$$
Calcola il tempo tra due istruzioni con l'**implementazione della pipeline** dato il *tempo tra due istruzioni SENZA pipeline* e il *numero delle fasi*.

In questo caso, con 5 fasi, ci dice che il tempo con la pipeline è **$1/5$** del tempo con esecuzione in sequenza; tuttavia non vediamo un miglioramento del genere ( *Miglioramento di $1.7$ volte invece che 5* ), questo perchè :

1. Per ottenere un miglioramento di $1/5$ dovremmo portare il clock a `160 ps`, che <mark class="hltr-red">non è possibile</mark> a causa del vincolo del tempo d'esecuzione minimo della fase ( `>=200 ps` ).
2. Considerando poche istruzioni **non abbiamo riempito la pipeline**, e non siamo arrivati al livello massimo di ottimizzazione.

### Vantaggi dell'architettura RISC-V
L' architettura RISC-V è avvantaggiata nell'implementazione della pipeline a causa delle seguenti caratteristiche :
1. **Tutte le istruzioni hanno la stessa lunghezza** ( *1 word* ) facilitandone il prelievo dalla memoria istruzioni.
2. **I codici degli operandi sono in posizione fissa**. Questo permette di accedervi leggendo il register file in parallelo con la decodifica dell’istruzione.
3. **Gli operandi residenti in memoria** sono possibili solo per `ld/lw` e `sd/sw`. 
   Ciò permette di usare la ALU per il calcolo di indirizzi anzichè per l'operazione ( *La ALU, come gli altri "componenti", può essere usata UNA SOLA volta per istruzione* ).
4. L’uso di **accessi allineati** fa sì che gli accessi in memoria avvengano sempre in un ciclo di trasferimento.

### Hazard
Di norma la pipeline permette di eseguire **una fase per ciclo di clock**. 
Alle volte questo <mark class="hltr-red">non è possibile</mark> per il verificarsi di *"condizioni critiche"* detti **hazard**. 

Vediamo alcune tipologie di hazard...

#### Hazard Strutturali
Una condizione di **hazard strutturale** è una condizione per la quale l’architettura dell’elaboratore rende <mark class="hltr-red">impossibile</mark> l’esecuzione di alcune sequenze di istruzioni in pipeline.


> [!example]- Esempio
> Se io disponessi di un’**unica memoria** non potrei, <mark class="hltr-orange">nello stesso ciclo</mark>, caricare istruzioni e memorizzare (o prelevare) operandi dalla memoria.

#### Hazard sui dati
Questo tipo di hazard si verifica quando <mark class="hltr-orange">la pipeline deve essere messa in stallo</mark> per ottenere delle **informazioni dagli stadi precedenti**.

Nel caso del RISC-V, consideriando il seguende codice...

```c
addx1, x2, x3 
sub x4, x1, x5
```

Il problema è che `x1` viene memorizzato nella **quinta fase**, mentre la `sub` ne ha bisogno nella **seconda fase**, quindi è <mark class="hltr-red">costretta ad aspettare per 3 cicli di clock</mark>.

In certi casi possiamo risolvere alla **compilazione**, intervenendo con alcune istruzioni *"vuote"*, tuttavia in generale è utile osservare che **non occorre aspettare di aver memorizzato il risultato**, utilizzando la tecnica dell **[[#Bypass]]**.

##### Bypass

La tecnica del **Bypass** o **Operand forwarding** viene usato per rendere il risultato di un operazione, disponibile alla successiva, bypassando l’operazione di write back ( *Scrittura su registro* ).

> [!example]- Esempio grafico del Bypass
> ![[bypass.png | center]]

Questa tecnica funziona <mark class="hltr-red">solo se la fase a cui il dato viene propagato è successiva nel tempo alla fase dal quale viene prelevato</mark>. Che in parole semplici vuol dire che non si può propagare all'indietro nel tempo ( *ovviamente* ).

Quando il bypass <mark class="hltr-orange">non è operabile</mark> si ricorre allo **stallo della pipeline** attraverso delle *"bolle"*, cioè istruzioni vuote che fanno scorrere i cicli, fino a rendere possibile l'utilizzo della tecnica.

> [!example]- Esempio di propagazione errata all'indietro e soluzione
> Supponiamo di avere il codice ...
> ```c
> ld x1, 0(x2) 
> sub x4, x1, x5
>```
> `x1` sarebbe disponibile solo dopo la **quarta fase** della `ld`, che è troppo tardi per essere usato come input al **terzo stadio** della `sub`.
> 
> Risolviamo quindi attraverso un istruzione *"bolla"* :
> ![[bolle.png | center]]


#### Hazard sul controllo
Questo tipo di hazard riguarda alla **circuiteria di controllo**, e si applica quasi solo ai salti condizionati.

Quando avviene un salto in pipeline, a causa della sua struttura, **non possiamo sapere se il salto avviene o no** prima che venga eseguita l'istruzione successiva.

Per risolvere questo hazard esistono delle circuiterie di **branch-prediction** che *"prevedono"* il comportamento di un branch e, nel la predizione sia errata, correggono l'errore eseguendo l'istruzione corretta.

Esistono circuiterie di predizione sofisticate, che permettono di **memorizzare l’esito del branch** precedente e assumere che il comportamento si mantenga coerente.