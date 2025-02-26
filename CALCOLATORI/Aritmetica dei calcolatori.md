---
Date created: 26-02-25 • 08:37
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250226_CalcLect2-3-AritmeticaCalcolatori.pdf]]"
Related Pages:
  - "[[Teoria dei calcolatori]]"
---
## Codifica e decodifica
Teoria sottostante: [[Teoria dei calcolatori]]
### Codifica ( $base\ 10 \to base\ \beta$ )
> Traduzione di un valore in un dato interpretabile dal sistema con cui interagiamo.
> Nel caso dei calcolatori parliamo di codificare un qualsiasi tipo di dato in binario.


#### Decodifica di interi ( $base\ 10 \to base\ \beta$ )
Per tradurre un numero in un valore interpretabile dal sistema ( *in base $\beta$, o meglio in base 2* ) basta usare la procedura del cambio base da base 10 a una qualunque base.

**Esempio con base 2:**

$X=1000\ , \ (1000)_{10}=(?)_{2}$

|  $X$   | $X/2$ | $X$%$2$ |             |
| :----: | :---: | :-----: | :---------: |
| $1000$ | $500$ |   $0$   | **:LiArrowUp:** |
| $500$  | $250$ |   $0$   |     **\|**      |
| $250$  | $125$ |   $0$   |     **\|**      |
| $125$  | $62$  |   $1$   |     **\|**      |
|  $62$  | $31$  |   $0$   |     **\|**      |
|  $31$  | $15$  |   $1$   |     **\|**      |
|  $15$  |  $7$  |   $1$   |     **\|**      |
|  $7$   |  $3$  |   $1$   |     **\|**      |
|  $3$   |  $1$  |   $1$   |     **\|**      |
|  $1$   |  $0$  |   $1$   |     **\|**      |
$(1000)_{10}=(1111101000)_{2}$

---
### Decodifica
> Traduzione di un dato codificato nel suo corrispettivo valore. 
> Nel caso dei calcolatori parliamo di decodificare da binario a un qualsiasi tipo di dato.

#### Decodifica di interi ( $base\ \beta \to base\ 10$ )
Per tradurre un dato binario in un numero "leggibile" ( *in base 10* ) basta usare la procedura del cambio base da una qualunque base a base 10.

**Esempio con base 2:**

$(101110)_{2}=(?)_{10}$$

|     1     |     0     |     1     |     1     |     1     |     0     |
| :-------: | :-------: | :-------: | :-------: | :-------: | :-------: |
| $1*2^{5}$ | $0*2^{4}$ | $1*2^{3}$ | $1*2^{2}$ | $1*2^{1}$ | $0*2^{0}$ |
|    32     |     0     |     8     |     4     |     2     |     0     |

$32+8+4+2=46\ \to\ (46)_{10}=(101110)_{2}$

## Aritmetica dei dati in binario
### Operazioni di base
**Somma**, **Sottrazione** e di conseguenza **Divisione** e **Moltiplicazione** si svolgono allo stesso modo delle operazioni in base decimale.

Dato che queste operazioni ci risultano meno naturali dato il cambio di base è consigliabile svolgerle in colonna per semplificarne lo svolgimento.
### Particolarità
- Come in base decimale **moltiplicare per 10** aggiunge al numero uno zero a destra, la stessa cosa avviene in base **binaria** quando si **moltiplica per 2 ($(10)_{2}$)**.
  Questa operazione è detta **shifting** o **shift a sinistra**.
- $k$ Bit in binario creano **$2^{k}$ possibili diverse combinazioni**, quindi possono rappresentare **$2^{k}$ simboli/valori/numeri**

### Numeri negativi nei calcolatori
Per rappresentare un numero **binario negativo** in un calcolatore, in cui non possiamo aggiungere un meno all'inizio, si sono sviluppati tre protocolli di rappresentazione:
- Modulo e segno
- Complemento 1
- Complemento 2

> [!attention] Complemento 2
> **Complemento 2** è attualmente il metodo più diffuso ( *default* ) per la rappresentazione di numeri binari nei calcolatori

#### Modulo e segno
Si usano **$k-1$ Bit** per rappresentare il valore del numero e **un singolo Bit per la rappresentazione del segno** ( *Positivo: 0 , Negativo: 1* ).

**Range** di questa codifica: $-2^{k-1}+1 \leftrightarrow 2^{k-1}-1$ . 
Questo perché con questa codifica si hanno due rappresentazioni per il numero $0$ :
- $(1\ 0000 ...) Negativo$
- $(0\ 0000 ...) Positivo$

#### Complemento 1

TODO: 
pp. 21