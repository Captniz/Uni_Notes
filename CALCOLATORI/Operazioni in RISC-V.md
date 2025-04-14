---
Date created: 13-04-25 • 22:29
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250320_L8-RISC-V_1.pdf]]"
Related Pages:
  - "[[Procedure in RISC-V]]"
  - "[[Architettura RISC-V]]"
---
## Operazioni logiche ( BIT a BIT )
 Anche se la maggior parte delle operazioni si svolge su **words** intere si ha anche la necessità di operare su *porzioni* di words o addirittura sul **singolo bit**.
 
RISC-V fornisce alcune istruzioni che permettono di fare questo in maniera <mark class="hltr-purple">semplificata</mark> ( *rispetto ad altre architetture* ) ma <mark class="hltr-blue">efficace</mark>.

### Left Shift Logico
L’idea è di inserire $n$ zeri nella posizione **meno significativa** ( *destra* ) e **traslare tutto a sinistra** perdendo i bit più significativi.

>[!example]- Esempio
> Facciamo un **left-shift di 4** sul registro `x19` contenente il valore $9$ ( `00001001` ) in `x11`:
> ```asm
 > /*x19 = 0000 1001*/
 > 
 > slli x11, x19, 4
 > 
 > /*x11 = 1001 0000*/
>```
>
>Quindi ora avremo in `x11` il valore $(9\cdot2^4 = 144)_{10} = (1001000)_2$

> [!error] Casi di Overflow
> Questa operazione non porta sempre al numero desiderato poichè si potrebbe incorrere in casi di **Overflow**.
> 
> Come nel caso di un numero rappresentato in **[[Aritmetica dei calcolatori#Complemento a 2|complemento a 2]]**:
> Se eseguiamo il left-shift ( *Es. di 4* ) su un <mark class="hltr-orange">numero negativo</mark> come ...
> 
> `1000 0000 ... ( 6 byte a 0 ) ... 0000 0001` 
> 
> Otteniamo :
> 
> `0000 0000 ... ( 6 byte a 0 ) ... 0001 0000`
> 
> Cioè il numero $16$ **positivo**, in questo caso incontriamo un caso di <mark class="hltr-red">Overflow che cambia il significato del dato</mark>.
> 
> Questo <mark class="hltr-orange">non si applica</mark> a tutti i numeri negativi in complemento a due.
> Per esempio il left-shift su -2 di $1$ porta **correttamente a -4**. 
> 
>Se si rimane nel **range di rappresentabilità** la moltiplicazione di potenze di $2$ tramite left-shift <mark class="hltr-green">funziona correttamente</mark>. 


### Right Shift Logico
Analogamente allo [[#Left Shift Logico|shift logico a sinistra]], si può avere uno **shift logico a destra**. 

Inoltre se il left-shift corrisponde alla <mark class="hltr-purple">moltiplicazione per potenze di 2</mark> il **right-shift** corrisponde a <mark class="hltr-blue">dividere per potenze di 2</mark>.

Anche in questo caso si applicano i casi di <mark class="hltr-red">overflow che cambiano il dato</mark>.

>[!example]- Esempio
>Shift logico a destra su -2 ( `x11` ) di 4 bit: 
> ```
> /*1111 1111 ... ( 6 byte a 1 ) ... 1111 1110 */
> srli x9, x11, 4
> /*0000 1111 ... ( 6 byte a 1 ) ... 1111 1111*/
>```
>Otteniamo un <mark class="hltr-orange">numero positivo</mark>.

^2f4c13

### Right shift aritmetico

Nel [[#^2f4c13|caso precedente]] ( *L'esempio* ) si può risolvere il problema con lo **shift aritmetico a destra**, che al posto di aggiungere <mark class="hltr-orange">0</mark> a destra aggiunge <mark class="hltr-green">bit uguali al bit di segno</mark>.

Questo tipo di shift <mark class="hltr-red">non ha alcun senso</mark> se fatto **a sinistra** tuttavia, quindi RISC-V **non lo ha come istruzione** ( *altre architetture si* ).


### And Logico
Il compito dell'**AND Logico** è, dati due registri, eseguire l'operatore AND per coppie su ogni bit dei registri.

>[!example]- Esempio
> Svolgiamo l'AND Logico su i registr `x10` e `x11` in `x9`:
> ```
> x10 = ... 1001 1110
> x11 = ... 1111 0000
>```
>Quindi scriviamo ...
>```asm
> and x9, x10, x11
>```
>Che risulta in
>```asm
>x9 = ... 1001 0000
>```

Si può immaginare questa istruzione anche come un **operazione di masking** dove il secondo operatore decide che bit *"lasciar passare"* mettendo i propri bit a **1**, <mark class="hltr-purple">mascherando</mark> il primo operatore.

### Or Logico
L'**OR Logico** si comporta come l'[[#And Logico|AND logico]] ovviamente con al posto dell'AND l'operatore OR. 

Analogamente all'AND, l'OR si comporta come **maschera**, ma al posto di mettere i bit che non vogliamo a **0** <mark class="hltr-orange">rimuovendo bit</mark> con l'OR possiamo <mark class="hltr-purple">aggiungere bit</mark> a **1**.

> [!example]- Esempio
> Vogliamo impostare a `1` i 4 bit più significativi di una word contenuta in `x9`:
> ```asm
addi x10, x0, 0x000F       /*x10 = 0000 0000 ... 0000 1111*/
slli x10, x10, 60          /*x10 = 1111 0000 ... 0000 0000*/
or x9, x9, x10             /*x9  = 1111 xxxx ... xxxx xxxx*/
>```

### XOR Logico ( Or esclusivo ) 
Il funzionamento si comporta esattamente come gli altri due operatori logici, ovviamente con l'operatore **XOR**.

Al posto di funzionare come una maschera, questo operatore funziona come **annullatore** se applicato sullo stesso registro, <mark class="hltr-orange">qualunque sia la word al suo interno</mark>.

>[!example]- Esempio
>Annulliamo il valore di `x9` :
>```asm
>x9 = xxxx xxxx ... xxxx xxxx 
>```
>Se eseguiamo lo **XOR** di `x9` con se stesso...
>```asm
>xor x9, x9, x9
>```
> Il valore di `x9` sarà :  `0000 0000 ... 0000 0000`
> 
> <mark class="hltr-red">Qualunque</mark> fosse il valore precedente.

### NOT Logico
Il **NOT Logico** in RISC-V <mark class="hltr-red">non esiste come istruzione</mark>.
Questo perchè essendo un operatore **unario** non può essere rappresentato da <mark class="hltr-orange">tre</mark> operandi di tipo registro.

Tuttavia si possono comunque ottenere le funzionalità dell NOT attraverso lo [[#XOR Logico ( Or esclusivo )|XOR]] del dato con un registro con **tutti i bit a 1**.  

>[!example]- Esempio
> Eseguiamo il NOT del valore contenuto in `x9` :
> ```asm
> x9 = 0000 0000 ... 1100 1101
> x10 = 1111 1111 ... 1111 1111
>```
>Quindi utilizziamo l'operatore **XOR** ...
>```asm
> xor x9, x9, x10
>```
>E otteniamo:
>```asm
x9 = 1111 1111 ... 0011 0010
>```

## Operazioni di controllo del flusso
La caratteristiche fondamentale che contraddistingue i **calcolatori** dalle calcolatrici è la possibilità di *alterare il flusso* del programma al verificarsi di certe condizioni ( *come con gli `if`* ).

Il linguaggio macchina delle varie architetture supporta questa possibilità fornendo istruzioni di _**«salto condizionato»**_.

Le sequenze di istruzioni tra due salti condizionati ( *conditional branch* ) sono così importanti che viene dato loro un nome: **[[#^7eb143|blocchi di base]]**. 

Inoltre la loro individuazione è una delle **prime fasi della compilazione**.

>[!important]- Blocchi di base
 Un **blocco di base** è una sequenza di istruzioni che non contiene né istruzioni di salto ( *a parte quella finale* ) né [[#^a63875|Label]] di destinazione ( *a parte la prima* ).
 ><mark class="hltr-orange">Tutti</mark> i cicli in linguaggio ad *alto livello* vengono implementati con **blocchi di base**.

^7eb143

### BEQ e BNE
Queste due istruzioni sono le principali per quanto riguarda i **salti condizionati**. 

Equivalgono a <mark class="hltr-blue">Branch Equal</mark> e <mark class="hltr-purple">Branch Not Equal</mark>, cioè salta se il valore dei due registri <mark class="hltr-blue">è uguale</mark> o salta se <mark class="hltr-purple">non sono uguali</mark>.

>[!example]- Esempio
>Usiamo le due istruzioni per saltare alla riga con *[[#^a63875|il label]]* `L1`:
>```asm
>beq rs1, rs2, L1          /*Salta se i due registri sono uguali*/
>bne rs1, rs2, L1          /*Salta se i due registri sono diversi*/
>```

#### Struttura IF
Con queste istruzioni possiamo realizzare degli `if` come questo ...

---

![[if_no_text.png| center]]

---

che tradotto in assembly sarà :
```asm
bne x22, x23, ELSE         /* Salta a ELSE se x22 div. da x23*/
 
add x19, x20, x21          /* f = g + h */
beq x0, x0, ESCI           /* Salto incondizionato a ESCI */

ELSE: 
sub x19, x20, x21          /* f = g - h */ 

ESCI: 
…
```

>[!important]- Label
> Le **Label** sono indicatori di posizione per i **salti**. Durante la compilazione vengono tradotte in <mark class="hltr-orange">indirizzi</mark> dal compilatore.

^a63875

#### Struttura WHILE
Sempre con questi salti possiamo realizzare delle strutture di `while`:

```cpp
while (salva[i]==k){
	i+=1
}
```

Questo ciclo viene tradotto in:
```asm
CICLO: 
slli x10, x22, 3         /* Registro temp. x10 = 8*i */
add x10, x10, x25        /* Ind. di salva[i] in x10  */
ld x9, 0(x10)            /* Carica salva[i] in x9    */
 
bne x9, x24, ESCI        /* Esci se raggiunto limite */
 
addi x22, x22, 1         /* i = i+1                  */
 
beq x0, x0, CICLO        /* Salto incondizionato a CICLO */
 
 
ESCI: 
…
```

### Altre operazioni di salto
Oltre a **[[#BEQ e BNE]]** si hanno altre operazioni di salto che potrebbero essere utili come:
- **BLT** :LiArrowRight: *Branch Less Than*
- **BGE** :LiArrowRight: *Branch Grater Equal*
- **BLTU** :LiArrowRight: *Branch Less Than Unsigned*
- **BGEU** :LiArrowRight: *Branch Greater Than Unsigned*

>[!example]- Esempio di implementazione
> Implementeremo questo `if` in assembly ...
> ```cpp
> if (i<j){
> 	f=h+h;
> }else{
> 	f=g-h;
> }
>```
> Come:
>```asm
>bge x22, x23, ELSE               /* Salta a ELSE se x22 >= x23 */
>
>add x19, x20, x21                /* f = g + h */
> 
>beq x0, x0, ESCI                 /* Salto incondizionato a ESCI */
>
>ELSE: 
>sub x19, x20, x21                /* f = g - h */ 
>
>ESCI: 
>…
>```

>[!important]- Controllo di un indice
Possiamo anche usare l'operazione **unsigned** `BGEU` per controllare se un indirizzo è *fuori dal limite* di un array.
>
> Per esempio prendiamo l'indirizzo `x20` e l'array `[0, x11]`. 
> Possiamo controllare se l'indirizzo è valido con questa operazione:
> ```asm
> bgeu x20, x11, FuoriLimite
>``` 
>Poichè **BGEU** considera `x20` come **unsigned** se il numero è <mark class="hltr-orange">positivo</mark> allora si applica il `>=` in modo *normale*.
>
>Invece se `x20` è <mark class="hltr-purple">negativo</mark> RISC-V cambia la **codifica di lettura** ( *non altera il bit di segno, ma legge ogni bit come di valore* ) portando `x20` a un valore a causa della codifica a [[Aritmetica dei calcolatori#Complemento a 2|Complemento 2]]  

#### Struttura IF ELSE / CASE SWITCH
Grazie a queste istruzioni di salto aggiuntive si possono implementare strutture più complesse come il **Case Switch**.

Questo costrutto viene implementato attraverso 3 parti :
- Memorizzare i vari indirizzi del codice da eseguire in una tabella ( *una sequenza di word* ).
 - Caricare in un registro l’indirizzo a cui saltare.
-  Fare un salto all’indirizzo puntato dal registro tramite l’istruzione `jalr` ( *«jump and link register»* )

