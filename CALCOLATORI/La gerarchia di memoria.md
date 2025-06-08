---
Date created: 06-06-25 • 10:06
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250511_L19-GerarchiaDiMemoria_1.pdf]]"
  - "[[250511_L20-GerarchiaDiMemoria_2.pdf]]"
Related Pages:
---
## Basi della teoria della memoria
La **memoria** è alla base di un calcolatore e serve a **contenere dati**, bisogna quindi poter leggere e scrivervi; ne esistono diversi tipi, adatti a diversi compiti :

- Memoria **indirizzata direttamente** ( *Memoria principale, Memoria cache* ):  
	- E' di tipo *volatile*, cioè il suo contenuto viene perso se si spegne l’elaboratore. 
	- E' limitata dallo spazio di indirizzamento del processore.
- Memoria indirizzata in modo indiretto ( *Memoria periferica* ):
	- E' di tipo *permanente*: mantiene il suo contenuto anche senza alimentazione. 
	- Ha uno spazio di **indirizzamento *“software”*** non limitato dal processore. 

Le informazioni nella **memoria principale** sono accessibili al processore in <mark class="hltr-orange">qualsiasi momento</mark>, mentre le informazioni nella **memoria periferica** devono prima essere <mark class="hltr-purple">trasferite nella memoria principale</mark>.

Il trasferimento dell’informazione tra memoria principale e memoria periferica è **mediato dal software** ( *Tipicamente il Sistema Operativo* ).


> [!info]- Terminologie
>
> > [!info] Tempo di Accesso
> > Tempo richiesto per **una** operazione di lettura/scrittura nella memoria.
> 
>
> >[!info] Tempo di Ciclo
> > Tempo che intercorre tra l’inizio di due operazioni consecutive tra locazioni diverse. In genere leggermente superiore al tempo di accesso.
> 
>
> > [!info] Accesso Casuale
> > Per le memorie con questa caratteristica vale che **non vi è alcuna relazione** o ordine nei dati memorizzati.
> > 
> > Questo è tipico delle memorie a *semiconduttori*.
> 
>
> > [!info] Accesso Sequenziale
> > Per le memorie con questa caratteristica vale :
> > - L’accesso alla memoria è ordinato o semi-ordinato.
> > - Il tempo di accesso dipende dalla posizione.
> > 
> > Questo è tipico dei dischi e dei nastri.
> 
> > [!info] RAM - Random Access Memory
> > Memoria scrivibile/leggibile a semiconduttori, il cui tempo di accesso è indipendente dalla posizione dell’informazione. 
> 
> > [!info] ROM - Read Only Memory
> > Memoria <mark class="hltr-red">SOLO leggibile</mark> a semiconduttori.
> > 
> > Il tempo di accesso può essere sia sequenziale che casuale.
> 
>
> > [!info] Latenza
> > Tempo di accesso ad una singola parola. 
> > 
> > E' la misura *“principe”* delle prestazioni di una memoria e dà un’indicazione di quanto il processore dovrebbe poter aspettare un dato dalla memoria nel caso peggiore.
> 
> > [!info] Velocità o Banda
> > Velocità di trasferimento massima in FPM.
> >  
> >  Molto importante per le operazioni in FPM che sono legate all’uso di memorie **cache** interne ai processori.
> >  
> >  E' anche importante per le operazioni in DMA, posto che il dispositivo periferico sia veloce.

^b85c4a


## Tipi di memoria
### Memoria principale
La memoria principale è **connessa logicamente** col processore.


> [!example]- Schema della connessione 
> ![[connessione_mem_p.png | center]]

Questo tipo di memoria memorizza **singoli bit**, normalmente organizzati in byte e/o word.

Data una capacità $N$ ( *Es. 512 Kbit* ) la memoria può essere organizzata in diversi modi a seconda del parallelismo $P$ ( *Es. 1, 4, 8* ).


> [!example] Esempi di organizzazione
> - $512K \cdot 1$ 
> - $128K \cdot 4$ 
> - $64K \cdot 8$

L’organizzazione influenza il **numero di pin di I/O del circuito** integrato che implementa la memoria.


> [!example] Schema di memoria 16k x 8
> ![[mem_8_16k.png|center]]

Vediamo ora delle sottocategorie di memoria principale...

#### SRAM
##### Descrizione
Sono memorie in cui i bit possono essere **tenuti indefinitamente** ( *Posto che non manchi l’alimentazione* ). Le loro caratteristiche principali sono :

- Estremamente veloci ( *Nano-secondi* ).
- Consumano poca corrente ( *e quindi non scaldano* ).
- Costano molto perché hanno molti componenti per ciascuna cella di memorizzazione.

##### Funzionamento

> [!example] Schema SRAM
> ![[sram.png | center]]
> $b’ = NOT(b)$

$b$ e $b'$, cioè i circuiti di terminazione della linea di bit ( *sense/write circuit* ) interfacciano il resto del circuito con la cella ( *T1 e T2 sono **chiusi** di base* ). La presenza contemporanea di $b$ e $b'$ permette un minor tasso di errori.

Il funzionamento di questa cella è ...
- **Scrittura** : 
	- La linea di `word` è alta e *apre* `T1` e `T2`. 
	- Il valore presente su $b$ e $b’$, che funzionano da linee di pilotaggio, **viene memorizzato nel latch** a doppio NOT ( *Cella* ).
- **Lettura** : 
	- La linea di `word` è alta e *apre* `T1` e `T2`.
	- Le linee $b$ e $b’$ sono tenute in stato di *[[#^67b54d|alta impedenza]]*
		- Il valore nei punti $X$ e $Y$ viene *“copiato”* ( *Lasciato passare* ) su $b$ e $b’$. 
	- Se la linea di word è bassa, `T1` e `T2` sono interruttori *chiusi*: il consumo è praticamente nullo. 


> [!warning]- Alta impedenza
> Lo **stato di alta impedenza** è una condizione in cui un circuito si *"disconnette"* elettricamente da una linea, **non trasmettendo né `0` né `1`**, ma comportandosi come se **fosse scollegato**.
> 
> In parole semplici è uno stato in cui un'uscita non fa **nulla**: non mette **tensione** sulla linea e non **assorbe corrente** ( *Ground* ). 
> 
> È come se **non esistesse** elettricamente.

^67b54d

#### DRAM
##### Descrizione
Sono le memorie più diffuse nei PC e simili. Economiche e a densità elevatissima ( *1 solo componente per cella* ), dato che la memoria viene ottenuta sotto forma di carica di **un condensatore**.

Queste memorie hanno bisogno di un **aggiornamento** ( *Refresh* ) continuo del proprio contenuto che altrimenti *“svanisce”* a causa delle *correnti parassite*. A causa del refresh continuo si hanno consumi elevati.

##### Funzionamento

> [!example] Schema DRAM
> ![[dram.png | center]]

Il funzionamento di questa cella è ...
- **Scrittura** : 
	- La linea di word è *alta* e apre `T`. 
	- Il valore presente su $b$ viene copiato sul condensatore `C` ( *Quindi carica il transistor* ).
- **Lettura** : 
	- La linea di word è *alta* e apre `T`. 
	- Un apposito circuito ( *Sense amplifier* ) misura la tensione su `C`. 
		- Se è **sopra** una certa soglia data, pilota la linea `b` alla *tensione nominale* di alimentazione, ricaricando `C`.
		- Se è **sotto** la soglia data, *mette a terra* la linea `b` *scaricando* completamente il condensatore C. 

Nel momento in cui `T` viene aperto, il condensatore `C` comincia a scaricarsi ( *O caricarsi a causa delle resistenze parassite* ). E’ necessario quindi fare un *refresh* della memoria prima che i dati *“spariscano”*, e per questo **basta fare un ciclo di lettura**.

In genere il chip di memoria contiene un circuito per il refresh ( *Lettura periodica di tutta la memoria*).

##### Multiplazione degli indirizzi
Data l’elevata [[#^67b25f | integrazione]] delle DRAM il numero di pin di I/O è un problema. E’ usuale *multiplare* nel tempo l’indirizzo delle righe e delle colonne negli stessi fili.

Normalmente le memorie non sono indirizzabili al bit, per cui **righe e colonne si riferiscono a byte e non a bit**.

> [!info]- Integrazione
> Nel contesto dei circuiti integrati, **integrazione** si riferisce al **numero di componenti (transistor, celle di memoria, ecc.) costruiti, in un singolo chip**.

^67b25f

> [!example]- Esempio di indirizzazione e schema della DRAM
> Una memoria $2M \cdot 8$ ( *21 bit di indirizzo* ) può essere organizzata in **4096 righe** ( *12bit di indirizzo* ) per **512 colonne** ( *9bit di indirizzo* ) di 8 bit ciascuno. 
> 
> ![[organizzazione_dram.png | center]]

Spesso i trasferimenti nella memoria avvengono a **blocchi** ( *Pagine* ). Nello schema d'esempio, vengono selezionati prima 4096 bytes e poi tra questi viene scelto quello richiesto.

E’ possibile migliorare le prestazioni semplicemente **evitando di *“riselezionare”* la riga ad ogni accesso** <mark class="hltr-orange">se le posizioni sono consecutive</mark>. Questo viene chiamato **fast page mode** ( *FPM* ) e l’incremento di prestazioni può essere significativo.

##### Sottocategorie di DRAM
###### SDRAM - DRAM Sincrona
Le DRAM viste prima sono dette *“asincrone”* perché <mark class="hltr-orange">non esiste una precisa temporizzazione di accesso</mark>, ma la dinamica viene governata dai segnali `RAS` e `CAS`.

Il processore deve tenere conto di questa potenziale *“asincronicità”* : in caso di refresh in corso può dare problemi. 

Aggiungendo dei **buffer** ( *latch* ) di memorizzazione degli ingressi e delle uscite si può ottenere un funzionamento **sincrono**, <mark class="hltr-purple">disaccoppiando lettura e scrittura dal refresh</mark>, e si può ottenere un accesso FPM pilotato dal clock.


> [!example]- Schema di SDRAM
> ![[SDRAM.png | center]]

> [!example]- Grafico di accesso FPM del clock
> ![[clock_fpm.png | center]]

###### DDR-SDRAM - Double Data Rate SDRAM
Questo tipo di SDRAM consente il trasferimento dei dati in FPM **sia sul fronte positivo che sul fronte negativo del clock**.

La <mark class="hltr-purple">[[#^b85c4a|latenza]] è uguale</mark> a una SDRAM normale, ma la <mark class="hltr-orange">[[#^b85c4a|banda]] è doppia</mark>.

Sono ottenute organizzando la memoria in due banchi separati : 
- Il primo contiene le **posizioni pari** e si accede sul **fronte positivo**
- Il secondo contiene le **posizioni dispari** e si accede sul **fronte negativo**

Le locazioni contigue sono in banchi separati e quindi si può fare l’**accesso in modo interlacciato**.