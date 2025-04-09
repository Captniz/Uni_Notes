---
Date created: 12-03-25 • 08:41
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250312_L6-Assembly.pdf]]"
Related Pages:
  - "[[Reti logiche]]"
---
## Linguaggio macchina
I calcolatori per comunicare istruzioni alla CPU utilizzano il **linguaggio macchina**, cioè una **sequenza di $0$ e $1$**, rendendolo molto difficile da maneggiare da un umano.

Il programmatore, a sua volta, scrive principalmente con linguaggi di alto livello ( *C++, Java, Python* ) incomprensibili per la macchina.
 
> [!important]- Istruzioni specifiche
> Una CPU interpreta <mark class="hltr-red">SOLO</mark> il linguaggio macchina **specifico per la sua architettura**. Questo perchè ogni CPU necessita di istruzioni diverse in base alle sue capacità.

^d87574

Il linguaggio **Assembly** ha proprio il compito di agire da ponte tra queste due realtà.

---
## Assembly
L'**assembly** è un linguaggio di bassissimo livello, che permette ai programmatori di scrivere istruzioni molto vicine al [[#Linguaggio macchina]].

Le caratteristiche principali dell'assembly sono:
- Codici mnemonici ( *Leggibili e memorizzabili da un umano* ).
- Stretta corrispondenza tra **linguaggio macchina** e assembly.
- Ogni riga/istruzione in assembly corrisponde un istruzione in L.M. ( *ci sono eccezioni* ).

> [!important]- Linguaggio compilato
> Assembly deve essere poi **compilato** e tradotto in linguaggio macchina per essere interpretabile dal calcolatore, questa compilazione è compito dell' **Assembler**.

^space
### ISA
Il linguaggio [[#^d87574|Assembly non è globale]]: ogni CPU necessita delle proprie istruzioni e di conseguenza del proprio linguaggio Assembly. L'insieme di queste istruzioni riconosciute dalla CPU si chiama **ISA** ( **I**_nstruction_ **S**_et_ **A**_rchitecture_ ).

L'ISA <mark class="hltr-red">NON</mark> è una semplice lista di istruzioni, contiene anche:
- Sintassi e semantica.
- Accesso ai dati:
	- Modalità di accesso ai **[[#^53308d|Registri]]** della CPU.
	- Modalità di accesso alla memoria e **indirizzamento**.


> [!info]- Diverse ISA per diverse CPU
> Un ISA può definire anche <mark class="hltr-orange">più linguaggi assembly</mark> ( *Intel 32/64 bit* ) o <mark class="hltr-orange">più ISA possono essere presenti in una singola CPU</mark> ( *Casi rari* ).

^space
### Architetture per ISA
Esistono dei vincoli ( *Architetture* ) applicati a istruzioni e operandi:
- **RISC**: ( **R**_educed_ **I**_nstruction_ **S**_et_ **C**_omputer_)
- **CISC**: ( **C**_omplex_ **I**_nstruction_ **S**_et_ **C**_omputer_)

| CISC                                                                                                                                                                                       | RISC                                                                                                                                                                                                                                                                            |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| - Semplifica la scrittura di programmi Assembly<br>- Maggior numero di istruzioni <br>- Istruzioni più potenti<br>- Tutte le istruzioni possono avere operandi (o destinazione) in memoria | - Semplifica l’implementazione della CPU<br>- ISA più semplice e regolare <br>- Istruzioni aritmetico/logiche non hanno operandi o destinazione in memoria<br>- Uniche istruzioni che accedono alla memoria: `load` e `store` <br>- In generale, meno istruzioni e meno potenti |

^7b8b5b

#### CISC
L'obiettivo di questa architettura è fornire <mark class="hltr-blue">istruzioni Assembly più potenti / flessibili / performanti </mark>.

Infatti le istruzioni [[#Istruzioni Aritmetiche & Logiche|aritmetico/logiche]] in CISC possono avere operandi e destinazione **in memoria**. Spesso tuttavia, esistono limiti o vincoli a questa caratteristica.

>[!example]-
**Intel**: al massimo un operando o la destinazione in memoria

Inoltre in CISC si hanno <mark class="hltr-orange">molte più istruzioni</mark>, con <mark class="hltr-orange">comportamenti più complessi</mark>, tuttavia queste hanno <mark class="hltr-orange">sintassi meno regolare</mark> ( *Alcune istruzioni hanno destinazione implicita* ).

La codifica istruzioni, a differenza del RISC, ha **numero variabile di bit**.
#### RISC
L'obiettivo di questa architettura è <mark class="hltr-blue">semplificare</mark> al massimo la struttura della CPU.

Le istruzioni [[#Istruzioni Aritmetiche & Logiche|aritmetico/logiche]] in RISC sono strutturate ...

```asm
<opcode> <dst>, <arg1>, <arg2>;
```

Dove:
- `opcode` è il nome dell'istruzione ( *codice mnemonico* ).
- `dst` ( *destination* ) e `arg1` sono **registri**.
- `arg2` **registro o valore immediato**.  

Mentre le istruzioni di accesso alla memoria ( *che sono essenzialmente 2* ) sono strutturate ...

```asm
load <reg> <mem_addr>
store <reg> <mem_addr>
```

Questa comporta la <mark class="hltr-red">necessità</mark> di **più registri**.

Inoltre la ridotta quantità di istruzioni porta a un <mark class="hltr-orange">numero fisso di bit</mark> necessario per la **codifica** delle istruzioni.

---

## Struttura di un programma Assembly
La struttura di un programma Assembly deriva direttamente dalla struttura della CPU corrispondente.

Durante l'esecuzione di un istruzione avvengono tre operazioni in sequenza:

| Categoria |                                                        Descrizione                                                        |
| :-------: | :-----------------------------------------------------------------------------------------------------------------------: |
|  `Fetch`  | Preleva un istruzione dalla memoria da un registro apposito <br>( **Program conunter "PC" / Instruction pointer "IP" ** ) |
| `Decode`  |                                   Decodifica l'istruzione per derivarne il significato                                    |
| `Execute` |                                                    Esegue l'operazione                                                    |


> [!important]- Program Counter / Instruction Pointer
> Il PC e l'IP sono due nomi usati per definire un **[[#^53308d|registro]]** che tiene conto della prossima istruzione da eseguire all'interno di un programma.

^99422c

### Dati di un programma Assembly
I tipi di dati su cui operano le istruzioni dell' Assembly sono principalmente:
- Operandi **logici o aritmetici**
- Dati da muovere
- Indirizzi di memoria
- Sostituzione di valori all'interno del **[[#^99422c|PC e IP]]**

Questi dati sono <mark class="hltr-orange">Immediati</mark>, <mark class="hltr-blue">costanti</mark> e <mark class="hltr-purple">contenuti in registri o in memoria</mark>.

> [!info]- Registri
> I [[Reti logiche#Registri|registri]] sono il dispositivo di memoria più **piccolo** all'interno di una macchina; allo stesso tempo tuttavia sono i più **veloci**, dal momento che sono fisicamente contenuti **all'interno della CPU**.
> 
> Fisicamente sono delle batterie ( *nel senso di serie* ) di **[[Reti logiche#Flip-Flop|Flip-Flop tipo D]]**.
> Un registro di $k$ bit è composto da $k$ Flip-Flop.

^53308d

Il numero e il nome dei registri a disposizione dell'Assembly, **general-purpose o specializzati**, è definito dall'[[#ISA]] e generalmente ne vengono messi a disposizione un numero compreso tra $4$ e $64$.
### Istruzioni in assembly
In assembly un istruzione è definita dal codice mnemonico ( *Nome dell'istruzione* ) seguito da eventuali operandi.

Possono essere di tipo:
- **Aritmetico logiche**
- **Movimento dati**
- **Controllo del flusso** ( *Jumps* )

Le istruzioni all'interno di un programma in assembly, di norma sono eseguite in ordine sequenziale, tuttavia esistono istruzioni dette **[[#^4c2e19|jump]]** che permettono di saltare ad altre istruzioni.

> [!warning]- Cicli
> In Assembly non esistono cicli come `for` o `while`, questi sono sostituiti da diversi tipi di `jump` con condizioni.

^space
#### Istruzioni condizionate
Alcune istruzioni vanno eseguite solo se determinate condizioni si verificano, come le istruzioni di controllo del flusso ( *Jump condizionati* ). 

Questa caratteristica è necessaria per implementare <mark class="hltr-orange">selezioni e cicli</mark>.

> [!info]- Altre istruzioni condizionate
> In alcune ISA, altre istruzioni possono avere esecuzione condizionale. 
> In particolare in **ARM** <mark class="hltr-red">tutte le istruzioni</mark> possono esserlo.
^space

Queste condizioni vengono espresse come **confronti valori nei registri general-purpose** o valori di **Flag** ( *valore indicatore* ) in un registro specifico:

|                                                                                  Confronti tra registri                                                                                  |                                                                                                                        Registro di Flag                                                                                                                        |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| Esegui se due registri hanno contenuto uguale ( *o diverso* ).<br><br><mark class="hltr-red">NECESSITANO</mark> operazioni di confronto.<br><br>*Es.*<br>`JEQ (jump equal), x0, x1, pos` | Esegui un operazione rispetto al valore del registro di Flag.<br><br>Le operazioni aritmetiche/logiche settano automaticamente il valore di Flag rispetto al valore di ritorno<br><br>*Es.* <br>`se return 0 -> flag = 0`<br>`se return overflow -> flag = -1` |


> [!important]- Istruzioni di comparazione specifiche
> Esistono anche alcune operazioni di confronto particolari all'interno di alcune [[#ISA]]:
> - `set if less than` ( *o simile* ): Confronta due registri general-purpose e setta un terzo registro in base al risultato.
> - `cmp`: esegue sottrazione settando flag, ma scarta il risultato.
> 
^space  

#### Istruzioni Aritmetiche & Logiche
Queste istruzioni sono implementate dalla **ALU** ( **A**_rithmetic_ **L**_ogic_ **U**_nit_ ) e in genere vengono generalmente definite attraverso <mark class="hltr-orange">due operandi</mark> e <mark class="hltr-green">una destinazione</mark> ( *Talvolta implicita* ). 

I valori degli operandi e del valore di ritorno possono seguire due filosofie diverse per quanto riguarda la loro **posizione**:

1. Tutto nei **registri** ( *operandi immediati* ).
2. Possibilità di avere operandi o destinazione **in memoria**.


Queste due scelte portano alla scelta della [[#Architetture per ISA|Architettura]] della **CPU**:
1. CPU più veloce e implementazione più semplice :LiArrowRight: **[[#RISC|RISC]]**
2. Assembly più potente e più semplice da usare :LiArrowRight: **[[#CISC|CISC]]**
##### Operazioni Logiche
Le operazioni **logiche** possono essere separate in due gruppi:

**Movimento dati**:
- Sposta dati tra i registri.
- Carica *costanti* in registri o in memoria.
- Carica *dati* dalla memoria ai registri o viceversa.

**Salto / Controllo del flusso** ^4c2e19
- Manipolano il contenuto di **[[#^99422c|PC e IP]]**
- <mark class="hltr-red">Necessaria</mark> esecuzione condizionale 
- Invocazione di **subroutine e ritorno da subroutine** ( *Funzioni* ) che necessitano salvare il valore del PC prima di cambiarlo.


%%TODO PP 15%%