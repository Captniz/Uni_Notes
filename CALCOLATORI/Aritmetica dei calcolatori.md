---
Date created: 26-02-25 • 08:37
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250226_L2-3-AritmeticaCalcolatori.pdf]]"
Related Pages: []
---
## Base del trattamento dei dati su un calcolatore
Un computer memorizza e manipola solo sequenze di $0$ e $1$, di cui la singola cifra è detta **Bit**, mentre una sequenza di $8$ è detta **Byte**.

Queste sequenze devono poter rappresentare **tutti** i tipi di informazione che un computer gestisce:
- <mark class="hltr-red">Valori numerici </mark>
- <mark class="hltr-orange">Caratteri o simboli </mark>
- <mark class="hltr-yellow">Immagini</mark>
- <mark class="hltr-green">Suoni</mark>
- <mark class="hltr-blue">Programmi</mark>

Per far si che una sequenza di bit possa essere interpretata come informazione utile, deve essere stabilita una **codifica** (rappresentazione digitale).

> [!info]- Codifica
> **Funzione che associa ad un oggetto/simbolo una sequenza di bit.**
>  
> **Codifica su n bit**: associa una sequenza di n bit ad ogni entità da codificare ( *Permette di codificare $2^{n}$ entità / simboli distinti* ).
> 
> Esempio: 
> Per codificare i semi delle carte bastano 2 bit
>  
> • Picche → `00` 
> • Quadri → `01`
> • Fiori → `10` 
> • Cuori → `11`

In particolare il dato va portato in **binario**. 

Per ridurre il numero di cifre e di conseguenza renderlo più facilmente leggibile si può usare la base **esadecimale per rappresentare un byte con due cifre**.

> [!example]- Esempio
> $$(10)_{10} = (0000.1010)_2 \to (A0)_{16}$$
### Codifica di interi ( $base\ 10 \to base\ \beta$ )
Per tradurre un numero intero in un valore interpretabile dal sistema ( 
*in base $\beta$, o meglio in base 2*) basta usare la procedura del cambio base da base 10 a una qualunque base.

> [!example]- Esempio base 2
> $$X=1000\ , \ (1000)_{10}=(?)_{2}$$
>
|  $X$   | $X/2$ | $X$%$2$ |                 |
| :----: | :---: | :-----: | :-------------: |
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
>$$(1000)_{10}=(1111101000)_{2}$$


### Decodifica di interi ( $base\ \beta \to base\ 10$ )
Per tradurre un dato binario in un numero "leggibile" ( *in base 10* ) basta usare la procedura del cambio base da una qualunque base a base 10.

> [!example]- Esempio in base 2
> 
> $$(101110)_{2}=(?)_{10}$$
> 
> |     1     |     0     |     1     |     1     |     1     |     0     |
> | :-------: | :-------: | :-------: | :-------: | :-------: | :-------: |
> | $1*2^{5}$ | $0*2^{4}$ | $1*2^{3}$ | $1*2^{2}$ | $1*2^{1}$ | $0*2^{0}$ |
> |    32     |     0     |     8     |     4     |     2     |     0     |
> 
> $$32+8+4+2=46\ \to\ (46)_{10}=(101110)_{2}$$
> 


---

## Rappresentazione ( Codifica ) dei numeri reali

Rappresentare esattamente un numero reale $x ∈ R$ <mark class="hltr-red">NON E' SEMPRE POSSIBILE</mark>: 

E' il caso dei numeri **irrazionali** (*es. π*), ovvero numeri reali con un numero **infinito** di cifre dopo la virgola **non periodiche**, quindi non rappresentabili con un numero finito di bit.

Esistono due tipi di rappresentazioni di numeri reali:
- Virgola **fissa**
- Virgola **mobile**
### Rappresentazione in virgola fissa
In questa rappresentazione, su un numero di $k$ cifre si dedica <mark class="hltr-orange">$k-f$</mark> cifre alla parte <mark class="hltr-orange">intera</mark> e <mark class="hltr-yellow">$f$</mark> cifre a quella <mark class="hltr-yellow">frazionaria</mark>.

Proprietà e vantaggi di questa rappresentazione:
- **Operazioni semplici**: somme, sottrazioni, moltiplicazioni si svolgono come se fossero numeri interi e in seguito si ri-scala il risultato.
- **I risultati sono rappresentabili**, le operazioni non introducono errori di approssimazione.
- **Due $\pm0$** entrambi con esponente e mantissa $0$
- **Due valori di infinito $\pm\infty$** entrambi con esponente pari a $255$ e mantissa $0$
- **Un valore $NaN$** rappresentato con esponente pari a $255$ e mantissa diversa da $0$
#### Conversione da decimale a binario
Per convertire da decimale a binario **separo** la parte intera da quella frazionaria e converto quella intera [[#Conversione da decimale a binario|normalmente]].

Per convertire la parte frazionaria la **moltiplico ricorsivamente per $2$**, tante volte quanti i **bit della parte frazionaria**. Per ogni ricorsione il bit corrispondente è uguale alla parte intera del numero.

Poi si unisce i due numeri separati dalla virgola.

> [!example]- Esempio
> Vogliamo convertire $6.125$
> $$(6)_{10}\to(110)_2$$
>
| Iterazione | Risultato | Parte intera \| Bit | Parte frazionaria |
| :--------: | :-------: | :-----------------: | :---------------: |
|    `0`     |           |                     |      $0.125$      |
|    `1`     |  $0.250$  |         `0`         |      $0.250$      |
|    `2`     |  $0.500$  |         `0`         |      $0.500$      |
|    `3`     |   $1.0$   |         `1`         |        $0$        |
> $$(.125)_{10}\to(001)_2$$
> 
> Risultato: $(6.125)_{10}$ -> $(110.001)_2$
#### Conversione da binario a decimale
La formula per ri-convertire a base 10 un numero rappresentato come: 

$$x_2=c_{k-1}\ c_{k-2}\ \ ...\ \ c_{f}\ \ .\ c_{f-1}\ \ ...\ \ c_{0}$$
$c_i=$ *cifra in posizione $i$*

è questa:

$$x_{10} = \sum_{i=0}^{k−1} c_iB^{i−f} = \sum_{i=0}^{k−1} c_iB^i · B^{−f} = (\sum_{i=0}^{k−1} c_iB^i ) · B^{−f}$$
$B=2$ *( Base )*

In altre parole vale a convertire il numero in base 10 come se non avesse virgola e poi moltiplichiamo per $B^{−f}$.


### Rappresentazione in virgola mobile
Un numero reale in base $10$ può essere rappresentato come :
$$x=M\cdot{B^E}$$
di cui la **M** è detta *mantissa* e la **E** *esponente*.

Quindi *$x$* può essere rappresentato su *$k$ bit totali* con : 
- <mark class="hltr-orange">$m$</mark> bit di <mark class="hltr-orange">mantissa</mark>
- <mark class="hltr-yellow">$e = k-m$</mark> bit di <mark class="hltr-yellow">esponente</mark>
#### Valori di rappresentazione
Nella rappresentazione a virgola mobile hanno segno sia la **mantissa $M$**, che determina il segno del numero, sia l'**esponente $E$**. 

Questo perchè il segno della mantissa determina il segno del numero finale, mentre il segno dell'esponente determina essenzialmente la **grandezza del numero** come [[#^negpos|vedremo tra poco]].
##### Base
La base **$B$ è fissa a 2** in quanto rappresenta il sistema binario in questo caso.
##### Mantissa
Per calcolare la mantissa si inizia con il calcolare la codifica in [[#Rappresentazione in virgola fissa|virgola fissa]] del numero.

A questo punto si sposta la virgola dopo la prima cifra ( *Che sarà sempre 1* ), **normalizzando** il numero. 


> [!important]- Mantissa a 1
> **In binario** la mantissa si inizia a rappresentare dal secondo bit in quanto il primo è scontato che sia posto a **$1$**; inoltre questo metodo risparmia un bit e aiuta a migliorare la precisione dei reali.

> [!danger] Validità del primo bit a 1
> Vale **SOLO** in binario

La mantissa sarà quindi la <mark class="hltr-red">parte dopo la virgola del numero</mark>.

Con questo calcolo troviamo anche <mark class="hltr-red">$E'$</mark> , cioè quante volte muoviamo la virgola a **destra o a sinistra**: 

quando la muoviamo a *sinistra* $E'$ è **positivo**, 
al contrario muovendola a *destra* $E'$ è **negativo**.
^negpos


> [!example]- Esempio
> Voglio rappresentare $12.345$ :
> $(12)_{10}\ .\ (345)_{10} \to (1100)_2\ .\ (010110)_2\ \ \ (approssimato\ a\ 6\ cifre)$
> $(1100.010110) \to (1^{implicito}.100010110)\cdot2^3$
> - $(100010110)_2=\ mantissa$
> - $(3)_{10}=\ E'$ 
##### Esponente
Dato che l'esponente $E$ determina lo spostamento della virgola, il segno ne determina la **direzione di spostamento**:

Se $E>0$ la virgola si muoverà verso destra **accrescendo** il numero, 
al contrario se $E<0$ la virgola si muoverà verso sinistra **riducendo** il numero.

> [!warning]- Numeri reali molto piccoli
> Nel caso in cui i numeri che vogliamo rappresentare siano molto piccoli dobbiamo utilizzare i numeri **[[#Numeri denormalizzati|denormalizzati]]**, di cui parleremo dopo.

L'esponente deve rispettare la formula : 
$$-2^{e-1}+1 < E' <= 2^{e-1}-1$$
in quanto deve rispettare ciò che abbiamo detto prima riguardo al segno e deve essere possibile rappresentarlo **con $e$ bit di memoria**.

Quindi possiamo ottenere la formula per calcolare l'esponente $E$ come : 
$$E=E'+(2^{e-1}-1)$$
Ovviamente bisogna **convertire $E$ in binario** per poi rappresentarlo.

> [!tip]- Semplificazione con il bias
> La formula $2^{e-1}-1$ serve a calcolare il numero di **bit disponibili alla rappresentazione dell'esponente**. Sapendo il numero di bit usati nello standard per rappresentare i numeri reali si può rendere noto questo valore come *bias*:
> - `float | 32bit` : `1bit segno  8bit esponente | 23bit mantissa`
>   Quindi il bias è **127 per i float**
> - `double | 64bit` : `1bit segno | 11bit esponente | 52bit mantissa`
>   Quindi il bias è **1023 per i double**

#### Rappresentazione standard o normalizzata
Quindi ottenendo tutti i dati per la formula possiamo rappresentare un numero decimale **normalizzato** seguendo questi passi:

1. Converto il numero con la *virgola fissa*
2. *Normalizzo* il numero
3. Converto l'esponente con la formula o il *bias*
4. Converto l'esponente in *binario*
5. Memorizzo i dati

>[!example]- Esempio 
 Voglio convertire $0.12345$ in binario:
 > 
 > ---
  *1* ) Converto in **binario le due parti:**
> $$(0)_{10}\to(0)_2$$
> $$(12345)_{10}\to(00011111100110100111)_2$$
 > ---
 *2* ) Unisco le parti e **normalizzo il numero:**
> $$(0.00011111100110100111)\to(1.1111100110100111)\cdot2^{-4}$$
 > ---
 *3* ) Converto l'**esponente in binario:**
> $$E=-4+(2^{e-1}-1)\to E=-4+127_{float\ bias}=123$$ 
> $$(123)_{10}\to(01111011)_2$$
> ---
 *4* ) **Memorizzo i dati:**
 >
| Segno \| `1 bit` | Mantissa \| `23 bit (Approssimato a 10)` | Esponente \| `8 bit` |
| :--------------: | :--------------------------------------: | :------------------: |
| `0` $(Positivo)$ |               `1111100110`               |      `01111011`      |
 
#### Numeri denormalizzati
I numeri **denormalizzati** vengono usati per rappresentare numeri particolarmente *piccoli*, a discapito della precisione.

A differenza di un numero normalizzato, in cui $E=(E'+bias)$ nei numeri denormalizzati <mark class="hltr-orange">$E=(1-bias)$</mark>.

Inoltre i numeri denormalizzati <mark class="hltr-red">NON HANNO</mark> il primo **bit implicito a $1$**, bensì **lo hanno a **<mark class="hltr-red">$0$</mark>.

#### Massimi, minimi e limiti

| **Categoria**  |  **$E$**   |   **$M$**   | **$Segno$** |
| :------------- | :--------: | :---------: | :---------: |
| Normalizzati   | $1 .. 254$ | $qualunque$ |  $0_{or}1$  |
| Denormalizzati |    $0$     |    $!=0$    |  $0_{or}1$  |
| $\pm0$         |    $0$     |     $0$     |  $0_{or}1$  |
| $\pm\infty$    |   $255$    |     $0$     |  $0_{or}1$  |
| $NaN$          |   $255$    |    $!=0$    |  $0_{or}1$  |

|                                    |      **Max**      |       **Min**        |
| :--------------------------------- | :---------------: | :------------------: |
| **Valore normalizzato**            | $3.4\cdot10^{38}$ |  $-3.4\cdot10^{38}$  |
| **Valore normalizzato positivo**   |                   | $1.755\cdot10^{-38}$ |
| **Valore denormalizzato positivo** |                   |  $1.4\cdot10^{-45}$  |

---
## Aritmetica dei dati in binario
### Operazioni di base
**Somma**, **Sottrazione** e di conseguenza **Divisione** e **Moltiplicazione** si svolgono allo stesso modo delle operazioni in base decimale.

Dato che queste operazioni ci risultano meno naturali dato il cambio di base è consigliabile svolgerle in colonna per semplificarne lo svolgimento.

> [!info]- Left shift
   **Moltiplicare per 10** in base 10 aggiunge uno zero a destra al numero. 
   La stessa cosa avviene in **binario** quando si **moltiplica per 2  ($(10)_{2}$ )**.
   >
  Questa operazione è detta **shifting** o **shift a sinistra**.
### Numeri negativi nei calcolatori
Per rappresentare un numero **binario negativo** in un calcolatore, in cui non possiamo aggiungere un meno all'inizio, si sono sviluppati tre protocolli di rappresentazione:
- Modulo e segno
- Complemento 1
- Complemento 2

> [!attention]- Complemento 2
> **Complemento 2** è attualmente il metodo più diffuso ( *default* ) per la rappresentazione di numeri binari nei calcolatori

>[!example]- Esempi di codifica
>
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
Si usano **$k-1$ Bit** per rappresentare il valore del numero e **$1$ singolo bit per la rappresentazione del segno** ( *Positivo: 0 , Negativo: 1* ).

> [!info]- Range
> Il range ( *spazio di rappresentazione dei numeri* ) di questa codifica è: $$-2^{k-1}+1 \leftrightarrow 2^{k-1}-1$$
>
Questo perché con questa codifica si hanno due rappresentazioni per il numero $0$ :
>- $(1\ 0000 ...)$ : **Negativo**
>- $(0\ 0000 ...)$ : **Positivo**

#### Complemento 1
Si rappresentano i numeri negativi facendo il **complemento a 1** del valore assoluto del numero.

In pratica **si scambiano gli $1$ e $0$ del numero in binario** quando si vuole rappresentare il numero in negativo.

Anche in questo caso si applicano le stesse proprietà del modulo e segno:
- Bit più significativo **rappresenta il segno** ( *Positivo: 0 , Negativo: 1* ).
- Sono presenti due rappresentazioni del numero 0 
	- $(1\ 0000 ...)$ : **Negativo**
	- $(0\ 0000 ...)$ : **Positivo**
- **Inoltre** le operazioni di somma e sottrazioni risultano più facili

##### Somma in complemento 1
1. Sommo i due numeri in binario
2. Sommo il riporto finale sul risultato

> [!attention]- Somma ambivalente
> Ovviamente la somma vale anche per la sottrazione se rappresentata come:
> $x + (-y)$ 

> [!danger] Overflow
> Se **I segni** dei due operandi **sono uguali** ma **opposti** a quello **del risultato**, quest' ultimo non è attendibile, ovvero non è rappresentabile su K Bit, e parliamo di **Overflow** .

> [!example]-
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
1. Faccio il [[#Complemento 1|complemento 1]] del numero
2. Sommo **$1$** al numero

Anche in questo caso si ha che:
1. Bit più significativo **rappresenta il segno** ( *Positivo: 0 , Negativo: 1* ).
2. Codifica <mark class="hltr-red">unica</mark> dello 0.
3. Somma ancora più semplice.

> [!danger] Somma e Overflow in complemento 2 
> Per sommare in **complemento 2** si seguono i passaggi del complemento 1, **esclusa** la somma del riporto. 
> 
> Nel caso ci sia del riporto il numero **NON E' RAPPRESENTABILE** con quel numero di Bit e quindi si tratta di un caso di **Overflow**.