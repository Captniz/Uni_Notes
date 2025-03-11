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

#### Codifica di interi ( $base\ 10 \to base\ \beta$ )
Per tradurre un numero in un valore interpretabile dal sistema ( 
*in base $\beta$, o meglio in base 2*) basta usare la procedura del cambio base da base 10 a una qualunque base.

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

## Rappresentazione ( Codifica )dei numeri reali

Rappresentare esattamente un numero reale $x ∈ R$ **NON E' SEMPRE POSSIBILE**: 
è il caso dei numeri irrazionali (*es. π*), ovvero numeri reali con un numero **infinito** di cifre “dopo la virgola” **non periodiche**; quindi non rappresentabili con un numero finito di bit.

Esistono due tipi di rappresentazioni di numeri reali:
- Virgola fissa
- Virgola mobile

### Rappresentazione in virgola fissa
In questa rappresentazione, su un numero di $k$ cifre si dedica $k-f$ cifre alla parte **intera** e $f$ cifre a quella **frazionaria**.

La formula per riconvertire a base 10 un numero rappresentato come: 

$c_i=cifra\ in\ posizione\ i$
$$x_2=c_{k-1}\ c_{k-2}\ \ ...\ \ c_{f}\ \ .\ c_{f-1}\ \ ...\ \ c_{0}$$

è questa:
$$x_{10} = \sum_{i=0}^{k−1} c_iB^{i−f} = \sum_{i=0}^{k−1} c_iB^i · B^{−f} = (\sum_{i=0}^{k−1} c_iB^i ) · B^{−f}$$

In altre parole vale a convertire il numero in base 10 come se non avesse virgola e poi moltiplichiamo per $B^{−f}$.

Proprietà e vantaggi di questa rappresentazione:
- **Operazioni semplici**: somme, sottrazioni, moltiplicazioni si svolgono come se fossero numeri interi e in seguito si ri-scala il risultato.
- **I risultati sono rappresentabili**, le operazioni non introducono errori di approssimazione.

%%TODO: Continua pp. 37%%
### Rappresentazione in virgola mobile

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

Esempi di codifica:

| Base 10 | Mod. e Seg. |  Compl. 1   | **Compl. 2** |
| :-----: | :---------: | :---------: | :------: |
|  $-4$   |             |             |  **$100$**   |
|  $-3$   |    $111$    |    $100$    |  **$101$**   |
|  $-2$   |    $110$    |    $101$    |  **$110$**   |
|  $-1$   |    $101$    |    $110$    |  **$111$**   |
|   $0$   | $100$/$000$ | $000$/$111$ |  **$000$**   |
|   $1$   |    $001$    |    $001$    |  **$001$**   |
|   $2$   |    $010$    |    $010$    |  **$010$**   |
|   $3$   |    $011$    |    $011$    |  **$011$**   |


#### Modulo e segno
Si usano **$k-1$ Bit** per rappresentare il valore del numero e **un singolo Bit per la rappresentazione del segno** ( *Positivo: 0 , Negativo: 1* ).

**Range** di questa codifica: $-2^{k-1}+1 \leftrightarrow 2^{k-1}-1$ . 
Questo perché con questa codifica si hanno due rappresentazioni per il numero $0$ :
- $(1\ 0000 ...) Negativo$
- $(0\ 0000 ...) Positivo$

#### Complemento 1
Si rappresentano i numeri negativi facendo il **complemento a 1** del valore assoluto del numero; cioè **si scambiano gli $1$ e $0$ del numero in binario** quando si vuole rappresentare il numero in negativo.

Anche in questo caso si applicano le stesse proprietà del modulo e segno:
- Bit più significativo **rappresenta il segno** ( *Positivo: 0 , Negativo: 1* ).
- Sono presenti due rappresentazioni del numero 0 
	- $(1\ 1111 ...) Negativo$
	- $(0\ 0000 ...) Positivo$
Inoltre
- Le operazioni di somma e sottrazioni risultano più facili

##### Somma in complemento 1
###### Procedura
1. Sommo i due numeri in binario
2. Sommo il riporto finale sul risultato ( *Guarda* [esempio pratico](Aritmetica%20dei%20calcolatori#Esempio%20pratico) )

> [!attention] Somma ambivalente
> Ovviamente la somma vale anche per la sottrazione se rappresentata come:
> $x + (-y)$ 

> [!danger] Overflow
> Se **I segni** dei due operandi **sono uguali** ma **opposti** a quello **del risultato**, quest' ultimo non è attendibile, ovvero non è rappresentabile su K Bit, e parliamo di **Overflow** .

###### Esempio pratico:
$6+(-3)$
$=$
$(00110)+(compl(00011))\to(00110)+(11100)$
$=$
$(00110)+(11100)\to(00010)\ riporto\ 1$
$=$
$somma\ del\ riporto\ \to (00010)+1=(00011)$
$=$
$(00011)_2 = (3)_{10}$

#### Complemento a 2
Rappresento un numero negativo attraverso il complemento 2, cioè:
1. Faccio il complemento 1 del numero
2. Sommo 1 al numero

Anche in questo caso si ha che:
1. Bit più significativo **rappresenta il segno** ( *Positivo: 0 , Negativo: 1* ).
2. Codifica **unica** dello 0
3. Somma ancora più semplice

> [!danger] Somma e Overflow in complemento 2 
> Per sommare in **complemento 2** si seguono i passaggi del complemento 1, **esclusa** la somma del riporto. 
> 
> Nel caso ci sia del riporto il numero **NON E' RAPPRESENTABILE** con quel numero di Bit e quindi si tratta di un caso di **Overflow**.


