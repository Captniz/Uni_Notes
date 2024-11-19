---
Date created: 29-10-24 • 08:46
tags:
  - Programmazione
Related PDF/DOC:
  - "[[TEOR_1.pdf]]"
Related Pages:
---
## Compilazione di un file
> Processo di compilazione di un file di C

*Singolo file:*
![[Compilazione.png]]


*Multipli file:*
![[Compilazione2.png]]

## Parole chiave
> Insieme di simboli il cui significato è stabilito dal linguaggio e non può essere ridefinito in quanto il loro uso è riservato.

| **Lettera** | Parola chiave                                 |
| :---------: | --------------------------------------------- |
|     *A*     | asm, auto                                     |
|     *B*     | break                                         |
|     *C*     | case catch char class const continue          |
|     *D*     | default delete do double                      |
|     *E*     | else enum extern <br>                         |
|     *F*     | float for friend <br>                         |
|     *G*     | goto <br>                                     |
|     *I*     | if inline int <br>                            |
|     *L*     | long <br>                                     |
|     *N*     | new <br>                                      |
|     *O*     | operator <br>                                 |
|     *P*     | private protected public <br>                 |
|     *R*     | register return <br>                          |
|     *S*     | short signed sizeof static struct switch <br> |
|     *T*     | template this throw try typedef <br>          |
|     *U*     | union <br>                                    |
|     *V*     | virtual void volatile <br>                    |
|     *W*     | while                                         |
|     *...*     | ...                                           |

## Caratteri di escape
> Caratteri di controllo con funzioni speciali vengono rappresentati da combinazioni di caratteri che iniziano con un backslash '*\\*'

|      Nome       | Descrizione                                  | Abbreviazione | **Sequenza di escape** |
| :-------------: | -------------------------------------------- | :-----------: | :--------------------: |
|    New line     | Va a capo                                    |   NL ( LF )   |          *\n*          |
| Tab orizzontale | ...                                          |      HT       |          *\t*          |
|  Tab verticale  | ...                                          |      VT       |          *\v*          |
|    Backspace    | Cancella l'ultimo carattere nell'output      |      BS       |          *\b*          |
|      Space      | ...                                          |      SP       |          *\s*          |
| Carriage return | Ritorna all'inizio della riga                |      CR       |          *\r*          |
|    Form feed    | Va a capo ma non torna all'inizio della riga |      FF       |          *\f*          |
|  Alarm / Bell   | Suono di allarme                             |      BEL      |          *\a*          |
|    Backslash    | ...                                          |       \       |         *\\\\*         |
|      Apice      | ...                                          |       '       |         *\\\'*         |
|   Virgolette    | ...                                          |       "       |         *\\\"*         |
|      NULL       | ...                                          |     NULL      |          *\0*          |

## Rappresentazione di interi
> Diversi tipi di rappresentazione di base di numeri all'interno del codice

```cpp
int decimal = 100;
int octal = 0100;
int exadecimal = 0x100;
```

## Operatori
> Alcuni caratteri speciali e loro combinazioni sono usati come operatori, cioè servono a denotare certe operazioni nel calcolo delle espressioni.

| **Simbolo** | Nome                                         |
| :---------: | -------------------------------------------- |
|     *+*     | Somma                                        |
|     *-*     | Sottrazione                                  |
|   * \* *    | Moltiplicazione                              |
|     */*     | Divisione                                    |
|     *%*     | Resto                                        |
|     *^*     | Xor binario                                  |
|     *&*     | And binario                                  |
|    *\|*     | Or binario                                   |
|     *~*     | Not binario                                  |
|     *!*     | Not                                          |
|     *=*     | Assegna                                      |
|     *<*     | Minore                                       |
|    *\>*     | Maggiore                                     |
|    *+=*     | Incrementa di                                |
|    *-=*     | Decrementa di                                |
|    *\*=*    | Moltiplica per                               |
|    */=*     | Dividi per                                   |
|    *%=*     | Resto della divizione per                    |
|    *^=*     | Xor uguale binario                           |
|    *&=*     | And uguale binario                           |
|    *\|=*    | Or uguale binario                            |
|    *<<*     | Scalo a sinistra binario                     |
|   *\>\>*    | Scalo a destra binario                       |
|    *<<=*    | Scalo a sinistra uguale binario              |
|   *\>\>=*   | Scalo a destra uguale binario                |
|    *==*     | Uguale                                       |
|    *!=*     | Diverso                                      |
|    *<=*     | Minore uguale                                |
|    *\>=*    | Maggiore uguale                              |
|    *&&*     | And                                          |
|   *\|\|*    | Or                                           |
|    *++*     | Incremento                                   |
|    *--*     | Decremento                                   |
|     *,*     | Separatore                                   |
|  * ->\* *   | Reference a membro di un oggetto (puntatore) |
|   * .\* *   | Reference a membro di un oggetto             |
|    *::*     | Accesso a namespace                          |
|    *()*     | Chiamata di funzione                         |
|    *[]*     | Chiamata di array                            |
|    *?:*     | Operatore ternario                           |
|   * \* *    | Valore di puntatore                          |
|     *&*     | Indirizzo di                                 |
### Propietà degli operatori
- La **posizione** rispetto ai suoi operandi: 
	- **Prefisso** se precede gli argomenti 
	- **Postfisso** se segue gli argomenti 
	- **Infisso** se sta fra gli argomenti 
- Il **numero di argomenti** *(arità)* 
- La **precedenza** *(o priorità)* nell’ ordine di esecuzione
	- *ES:* 
	  **1+2\*3** viene calcolata come **1+(2\*3)** e non come **(1+2)\*3** 
- **Associatività**: l’ordine in cui vengono eseguiti operatori della stessa priorità gli operatori possono essere associativi a destra o a sinistra