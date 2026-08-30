---
Date created:
tags:
  - Reti
Related PDF/DOC:
Related Pages:
---
# Servizi e protocolli di trasporto
Forniscono la comunicazione logica tra processi applicativi di host differenti  

I protocolli di trasporto vengono eseguiti nei sistemi terminali  

Lato invio: scinde i messaggi in segmenti e li passa al livello di rete 
Lato ricezione: riassembla i segmenti in messaggi  

Livello di trasporto: Comunicazione logica tra processi  

## TCP
Affidabile, consegne nell’ordine originario:   Controllo di congestione  Controllo di flusso  Setup della connessione  

## UDP
Inaffidabile, consegne senza ordine:  Estensione senza fronzoli del servizio di consegna best effort  

---

# Multiplexing/demultiplexing  
**Multiplexing** al trasmettitore: gestione dati da diverse socket, aggiunta header con PCI del livello transport

**Demultiplexing** al ricevitore: utilizza le PCI nell’header per consegnare i segmenti ricevuti alle socket giuste  

## Demultiplexing
Ogni datagramma  trasporta 1 segmento a livello di trasporto e ha ...
- un indirizzo IP di origine e un indirizzo IP di destinazione 
- un numero di porta di origine e un numero di porta di destinazione  

Il mittente usa gli indirizzi IP e i numeri di porta per inviare il segmento alla socket appropriata  

Il destinatario riceve i datagrammi IP.

L’interfaccia tra l’applicazione e il livello di trasporto è la “porta” :
Mappatura biunivoca tra porta e processo  Identificata da un intero di 16 bit  

I servizi standard vengono esposti da un server utilizzando numeri di porta standard o “well-known”, con valori < 1024 • Esempio: porta 80 per HTTP, porta 25 per SMTP, porta 53 per DNS  

I numeri di porta si possono anche classificare come 
-  Statici (altro nome delle porte “well-known”) • Associati con applicazioni standard (email, web, DNS, …)
-   Dinamici (o “ephemeral”) • Assegnati automaticamente dal sistema operativo quando si apre una connessione o si crea una socket  



Concetto di flusso (flow) e connessione  Gruppo di dati che appartengono alla stessa comunicazione logica  Un’applicazione può aprire multiple connessioni e veicolare molteplici flussi  



---

# UDP :  User Datagram Protocol  
Servizio di consegna “a massimo sforzo” (best effort)  **Senza connessione**  
Ogni segmento UDP è gestito indipendentemente dagli altri

Senza controllo di congestione: UDP può inviare raffiche di pacchetti dati  
Semplice: nessuno stato di connessione nel mittente e destinatario  


I segmenti UDP possono essere: 
- Perduti
- Consegnati fuori sequenza all’applicazione  


Utilizzato spesso nelle applicazioni multimedial  
![[EMBED/reti_cap3_PC 2.png]]

[[reti_cap3_PC.pdf#page=19&rect=153,357,539,769|reti_cap3_PC, p.19]]

## Checksum
Obiettivo: rilevare gli “errori” (bit alterati) nel segmento trasmesso  

Mittente:  
Tratta il contenuto del segmento come una sequenza di interi da 16 bit  

Somma le parole di 16 bit nel segmento e calcola il complemento a 1 della somma  

Destinatario:
Somma tutte le parole di 16 bit del segmento, incluso il checksum  
Controlla se il risultato è una parola di 16 bit tutti uguali a 1  

---

# ARQ : Automatic Repeat reQuest  
Classe di protocolli che cerca di recuperare le perdite di pacchetti  

Usa pacchetti speciali per notificare il trasmettitore di una avvenuta ricezione corretta  : **Acknowledgments (ACK)**

protocolli basati su forme di ARQ  :
- Stop-and-Wait 
- Go-back-N 
- Selective Repeat 
- TCP 
- MAC (livello 2, data link) dei sistemi WiFi  

## STOP and WAIT
### Mittente
1. Invia una PDU (e mantiene una copia locale) 
2. Imposta un timeout 
3. Attende la ricezione del rispettivo ACK
	1. Se non riceve alcun ACK entro il timeout, invia nuovamente la stessa PDU
4. Se riceve l’ACK controlla che l’ACK non contenga errori (checksum) 
5. Controlla il numero di sequenza 
6. procede con l’invio della PDU successiva    

### Ricevente
1. Controlla il numero di sequenza
	1. Se il checksum o il numero di sequenza sono errati, cancella la PDU (drop)
2.  Se è corretto e in ordine, invia l’ACK e passa l’SDU ai layer superiori

![[EMBED/reti_cap3_PC 3.png]]

[[reti_cap3_PC.pdf#page=27&rect=103,8,521,773|reti_cap3_PC, p.27]]

![[EMBED/reti_cap3_PC 4.png]]

[[reti_cap3_PC.pdf#page=28&rect=43,24,543,758|reti_cap3_PC, p.28]]

## Pipelining
il trasmettitore accetta che ci siano diversi pacchetti “in volo”, cioè per i quali non ha ancora ricevuto un ACK  

Va tenuta traccia dei numeri di sequenza dei pacchetti 
Bisogna memorizzare i segmenti sia al trasmettitore sia al ricevitore  

![[EMBED/reti_cap3_PC 5.png]]

[[reti_cap3_PC.pdf#page=30&rect=30,5,542,792|reti_cap3_PC, p.30]]

> [!info] glossario
> > Finestra di trasmissione $W_T$: 
> 
> insieme di PDU che il trasmettitore può trasmettere senza avere ancora ricevuto l’ACK. Al massimo tanto grande quanto la memoria allocata dal sistema operativo del trasmettitore.
> > |$W_T$| (cardinalità di $W_T$) :
> 
> indica la dimensione della finestra 
> > Finestra di ricezione $W_R$: 
> 
> insieme di PDU che il ricevitore può accettare e immagazzinare Al massimo tanto grande quanto la memoria allocata dal sistema operativo del ricevitore 
> >  Puntatore low $W_{LOW}$: 
> 
> puntatore al primo pacchetto nella finestra di trasmissione $W_T$ 
> > Puntatore up $W_{UP}$:
> 
 puntatore all’ultimo pacchetto già trasmesso  Potrebbe non coincidere con l’ultimo pacchetto di $W$  

![[EMBED/reti_cap3_PC 6.png]]

[[reti_cap3_PC.pdf#page=33&rect=102,79,542,729|reti_cap3_PC, p.33]]

### Tipi di ACK

- **ACK individuale**
	- Indica la ricezione corretta di un pacchetto specifico
	- ACK(n) significa “Ho ricevuto il pacchetto n”
- ACK cumulativo 
	- Indica la ricezione corretta di tutti i pacchetti fino a un certo indice 
	- ACK(n) significa ”Ho ricevuto tutto fino al pacchetto n (escluso)” 
- ACK negativo (NACK)
	-  Richiede la ritrasmissione di un pacchetto singolo  
	- NACK(n) significa “Inviami di nuovo il pacchetto n”  
- “*Piggybacking*”  Inserimento di un ACK data in un pacchetto dati  

### Protocolli con pipelining
#### GO-Back-N
Il mittente può avere fino a N pacchetti senza ACK in pipeline 

Il ricevente invia solo ACK cumulativi (Non dà l’ACK di un pacchetto se c’è un gap)

Il mittente ha un timer per il più vecchio pacchetto senza ACK. Se il timer scade, ritrasmette tutti i pacchetti senza ACK  

![[EMBED/reti_cap3_PC 7.png]]

[[reti_cap3_PC.pdf#page=36&rect=1,1,602,794|reti_cap3_PC, p.36]]
#### Selective repeat
Il mittente può avere fino a N pacchetti senza ACK in pipeline 

Il ricevente dà gli ACK solo ai singoli pacchetti 

Il mittente mantiene un timer per ciascun pacchetto che non ha ancora ricevuto ACK. Quando il timer scade, ritrasmette solo i pacchetti che non hanno avuto ACK  

![[EMBED/reti_cap3_PC 8.png]]

[[reti_cap3_PC.pdf#page=37&rect=0,-1,529,793|reti_cap3_PC, p.37]]

![[EMBED/reti_cap3_PC 9.png]]

[[reti_cap3_PC.pdf#page=38&rect=14,9,550,763|reti_cap3_PC, p.38]]

![[EMBED/reti_cap3_PC 10.png]]

[[reti_cap3_PC.pdf#page=39&rect=41,9,595,784|reti_cap3_PC, p.39]]


## TCP
Protocollo punto-punto full duplex: Flusso di dati bidirezionale nella stessa connessione tra un mittente, un destinatario 

Orientato alla connessione  
Flusso di byte affidabile e consegnato in ordine  
Flusso controllato:  Il trasmettitore non sovraccaricherà il ricevitore  
Il controllo di congestione evita di saturare la rete  


Con pipelining: 
- Meccanismi di controllo di flusso e di congestione TCP definiscono la dimensione della finestra 
- Le dimensioni delle finestre sono dinamiche (sia al mittente sia al destinatario) 
- Usa ACK cumulativi  


![[EMBED/reti_cap3_PC 11.png]]

[[reti_cap3_PC.pdf#page=42&rect=107,5,605,791|reti_cap3_PC, p.42]]

### RWND

La Finestra di ricezione (RWND) è un campo di 16 bit e Indica il numero di byte che il ricevitore può immagazzinare. Di conseguenza, rappresenta anche la massima quantità di dati in transito durante un RTT  

Valore massimo: 216 Byte = 65536 Byte = 64 kByte  


> [!warning] BDP : Bandwidth-Delay Product
> Misura usata nelle reti per capire **quanti dati possono essere “in volo” sulla rete** prima che il primo bit torni come conferma.  
> > BDP = capacità del collegamento × ritardo di propagazione/RTT 
>
>
$$\boxed{BDP = Bandwidth \times RTT}$$  
> - **Bandwidth** = velocità del collegamento, in bit/s  
> - **RTT** = tempo di andata e ritorno, in secondi  
>
 il risultato è in **bit**; dividendo per 8 ottieni i **byte**.

### Numero Sequenza e numero di ACK
**Numero di sequenza**: Numero del primo byte del segmento nel flusso di byte

**Numero di ACK**: Numero di sequenza del prossimo byte atteso dall’altro lato (ACK cumulativo )

![[EMBED/reti_cap3_PC 12.png]]

[[reti_cap3_PC.pdf#page=44&rect=105,341,560,781|reti_cap3_PC, p.44]]

### Connessione in TCP
#### Apertura
La procedura di setup della connessione TCP si chiama "three-way handshake“  


> 1. Host A (il client che inizia la connessione): segmento con flag SYN a 1 

- Porta sorgente (= A)
- Porta destinazione (= B) 
- Numero di sequenza iniziale (= x)

> 2.  Host B (server in attesa di connessioni): segmento con flag SYN e ACK a 1

- Porta sorgente (= B)
- Porta destinazione (= A)
- Numero di sequenza iniziale (= y)
- Numero di ACK (= x + 1)

> 3.  Host A: segmento con il flag ACK a 1

- Porta sorgente (= A)
- Porta destinazione (= B)
-  Numero di ACK (= y + 1)  

![[EMBED/reti_cap3_PC 14.png]]

[[reti_cap3_PC.pdf#page=46&rect=10,136,568,770|reti_cap3_PC, p.46]]

#### Chiusura
##### Procedura gentile
1. inviare un segmento TCP con flag FIN a 1
2. Il ricevitore invia un ACK
3. La connessione è semi-chiusa (Si possono ancora mandare dati nella direzione opposta )
4. La chiusura è completa solo quando si invia un FIN e si riceve un ACK anche nell’altra direzione  

![[EMBED/reti_cap3_PC 15.png]]

[[reti_cap3_PC.pdf#page=52&rect=117,41,574,772|reti_cap3_PC, p.52]]

##### Procedura brusca o di reset (RST)
RST si usa per resettare connessioni non gestibili o che si trovano in uno stato errato  

1. Uno degli host invia un segmento con flag RST a 1 
2. Entrambi gli host liberano le risorse allocate dal sistema operativo  

I server possono usare il RST per chiudere connessioni velocemente  


### MSS  Maximum Segment Size  
TCP Cerca invece di inviare segmenti lunghi quanto un MSS  

L’MSS dipende da un parametro del livello di rete sottostante (es. IP) che si chiama Maximum Transfer Unit (MTU)  
A sua volta, l’MTU del livello di rete dipende dall’MTU del livello data link  

<mark class="hltr-red">L’MSS si riferisce alla lunghezza del solo payload!  </mark>

Come scegliere l’MSS?  Non ci sono meccanismi per comunicarlo  "Trial and error": si tenta con MSS sempre più grandi, finché non ci si accorge che qualche segmento viene perso  

### RTT E RTO (Retransmission TimeOut)
L'RTO cioè il tempo da aspettare prima che un pacchetto sia considerato perso e quindi ritrasmesso deve essere <mark class="hltr-orange">più grande di RTT</mark>; tuttavia RTT varia.

- Troppo piccolo: timeout prematuro : Ritrasmissioni non necessarie
- Troppo grande: reazione lenta alla perdita dei segmenti  

RTO viene stimato attraverso **SampleRTT**: 
> Tempo misurato dalla trasmissione del segmento fino alla ricezione di ACK (Ignorando le ritrasmissioni).

Dato che anche SampleRTT varia si usa una media di più misure recenti.

$$EstimatedRTT = (1 - α)*EstimatedRTT + α*SampleRTT$$

Valore tipico: $α = 0,125$

EstimatedRTT più un “margine di sicurezza” o limite superiore, per trovare RTO si  quanto SampleRTT si discosta da EstimatedRTT  
 
$$DevRTT = (1-β)*DevRTT + β*|SampleRTT - EstimatedRTT|$$

tipicamente: $β = 0.25$

Quindi si ha ...

$$RTO = EstimatedRTT + 4*DevRTT  
$$

### Controllo del flusso
Premette al ricevitore di controllare la velocità di trasmissione del mittente 

Garantisce che il buffer del ricevitore non vada in overflow  
(lo costringerebbe a scartare pacchetti corretti per mancanza di spazio)  


Il ricevitore comunica quanto spazio libero ha nel proprio buffer di ricezione, includendo il valore della RWND nell’header TCP di ogni segmento che invia al mittente  

Il mittente limita la quantità di dati “in volo” (ancora non ACKati) al valore di RWND  


### Controllo congestione
Informalmente: “troppi trasmettitori stanno mandando troppi dati e la rete non riesce a gestire tutto questo traffico”  

Come si manifesta: 
- Pacchetti persi (buffer overflow nei router)
- Ritardi lunghi (accodamento nei buffer dei router)  

Un sistema a coda comprende una “fila d’attesa” e un server con ...
- $\lambda$ Tasso (frequenza) di arrivo  : numero medio di pacchetti che entra nella coda per unità di tempo  
- $\mu$ Tasso di servizio :  tempo medio richiesto dal server per trasmettere un pacchetto   

Diversi approcci possibili ...

> Controllo di congestione end-to-end

- Non coinvolge la rete
- Si capisce se c’è congestione osservando perdite di pacchetti e ritardi

> Controllo di congestione assistito dalla rete

- I router forniscono feedback agli host sullo stato della rete
- Es. un singolo bit per indicare la congestione (SNA, DECbit, TCP/IP Explicit Congestion Notification (ECN), ATM)  
- <mark class="hltr-red">NON USATO DAL TCP</mark>


Il controllo di congestione gestisce l’adattamente della finestra di congestione **CWND**, cioè il numero di byte che il trasmettitore può inviare nella rete.

$$|W_T| = min(CWND, RWND) = min(CWND, |W_R|)  $$

($W_T$ detto finestra di trasmissione)

Questa finestra si basa su diversi princìpi ...
- Slow start
- Congestion avoidance  
- Fast retransmit 
- Fast recovery  
#### Cause e costi
- Buffer infiniti
	- No perdite, ma se tasso di arrivo tasso di servizio… 
	- Costo: ritardi molto elevati
- Ritardi dovuti alla congestione: timeout 
	- Ritrasmissioni inutili 
	- Costo: spreco di risorse
- Pacchetti scartati a causa della congestione
	- Più lavoro richiesto per ottenere lo stesso throughput percepito a livello applicazione
	- Tale throughput potrebbe ridursi a zero sprecando gli sforzi fatti  
	- Costi: ritrasmissioni 

#### Algoritmi per la congestione
Non c’è un solo algoritmo in TCP per gestire la congestione  
L’implementazione di TCP spesso dipende dal sistema operativo  

Le implementazioni di TCP ragionano in byte ma Per semplicità, ragioneremo in segmenti


##### AIMD : Additive Increase Multiplicative Decrease   
il mittente aumenta il tasso di trasmissione (cioè la dimensione della finestra) cercando di occupare la banda disponibile, finché non si rilevano perdite  

- **Additive increase**: aumenta la finestra di 1 MSS ogni RTT finché non ci sono perdite  
- **Multiplicative decrease**: riduci la finestra (tipicamente della metà) quando si rileva una perdita  

AIMD mira a ottenere equità tra varie seesione di TCP sullo stesso link di banda

![[EMBED/reti_cap3_PC 16.png]]

[[reti_cap3_PC.pdf#page=78&rect=269,82,495,640|reti_cap3_PC, p.78]]

#### Slow start
> 1. Per ogni ACK valido ricevuto, aumento CWND di 1 MSS

CWND aumenta esponenzialmente nel tempo:
     1. Inizio con CWND = 1 MSS
     2.  Ricevo 1 ACK dopo 1 RTT
     3. CWND = 2 MSS
     4. Ricevo 2 ACK dopo 1 RTT
     5. CWND = 4 MSS

> 2. Quando CWND raggiunge o supera una soglia SSTHRESH si passa a modalità *congestion avoidance*  


Passaggi approfonditi:

1. CWND = 1 MSS
2. SSTHRESH = RWND
3. Ricevo un ACK ...
   - Valido: 
     1. CWND = CWND + 1 MSS
     2. Sposto WLOW al primo byte (o segmento) non ACKed
     3. Se CWND $\ge$ SSTHRESH passo a Congestion Avoidance
     4. Trasmetto nuovi segmenti come consentito da CWND
   - Se scatta un timeout: 
      1. Abbasso SSTHRESH = MAX(CWND / 2, 2)
      2. Aumento RTO = RTO * 2
      3. Reimposto CWND = 1
      4. Ritrasmetto il segmento che ha causato il timeout  

#### Congestion avoidance
> 1. Per ogni ACK valido ricevuto, aumento la CWND di $MSS \cdot \frac{MSS}{CWND}$ O

O $MSS \cdot \frac{1}{CWND}$ parlando di segmenti.

In altre parole, per ogni RTT in cui ricevo esattamente tutti gli ACK attesi, aumento CWND di 1 MSS portando ad un Aumento lineare di CWND nel tempo .


Passaggi approfonditi:

- Se ricevo un ACK valido: 
	1. $CWND = MSS \cdot \frac{MSS}{CWND}$ (ricordate: valori in byte!)
	2. Sposto WLOW al primo segmento non ACKed
	3. Trasmetto nuovi segmenti come consentito da CWND
- Se scatta un timeout: 
	1. Passo a slow start
	2. Abbasso SSTHRESH = MAX(CWND / 2, 2)
	3. Aumento RTO = RTO * 2 
	4. Reimposto CWND = 1 
	5. Ritrasmetto il segmento che ha causato il timeout  

![[EMBED/reti_cap3_PC 17.png]]

[[reti_cap3_PC.pdf#page=91&rect=112,24,520,790|reti_cap3_PC, p.91]]

![[EMBED/reti_cap3_PC 18.png]]

[[reti_cap3_PC.pdf#page=90&rect=89,316,573,766|reti_cap3_PC, p.90]]
#### Fast retransmit
Resettare a slow start per un singolo pacchetto perso è esagerato, Gli ACK duplicati ci dicono che la rete funziona .

Alla ricezione del 3o ACK duplicato  ...
1. Ritrasmetto il segmento indicato dall’ACK (*fast retransmit*) 
2. Entro in in *“fast recovery”*  
3. Mi ricordo il valore di $W_{UP}$ per sapere quanti segmenti sono “in volo”, $RECOVER = W_{UP}$  

#### Fast recovery
Alla ricezione del 3o ACK duplicato 
1. Abbasso SSTHRESH = CWND / 2
2. CWND = SSTHRESH + 3 MSS
3. NON SPOSTO il puntatore WLOW  

 Se arrivano altri ACK duplicati:
1. CWND = CWND + 1 MSS
2. NON SPOSTO il puntatore WLOW

Quando arriva un ACK valido (che include un riscontro per il segmento RECOVER):
- CWND = SSTHRESH
- Passo alla fase Congestion avoidance 
- Sposto WLOW al primo segmento non ACKed 

Se arriva un ACK parziale (conferma un segmento precedente a RECOVER):
1. Ritrasmetto il primo segmento per cui non ho un ACK
2. Riduco CWND = CWND – numero di segmenti ACKed + 1
3. Sposto $W_{LOW}$ al primo segmento non ACKed  

![[EMBED/reti_cap3_PC 20.png]]

[[reti_cap3_PC.pdf#page=95&rect=42,7,584,783|reti_cap3_PC, p.95]]

### Futuro della congestion avoidance
Il controllo della congestione di TCP stabilisce di rallentare la trasmissione basandosi solo sulle indicazioni dei pacchetti persi  

Ad oggi abbiamo bisogno di un algoritmo che risponda alla congestione effettiva, piuttosto che alla perdita di pacchetti  

#### CUBIC
Algoritmo che varia la lunghezza della finestra di congestione secondo una funzione cubica del tempo.

Migliora la scalabilità e la stabilità su reti veloci e a lunga distanza

![[EMBED/reti_cap3_PC 21.png]]

[[reti_cap3_PC.pdf#page=106&rect=34,5,578,754|reti_cap3_PC, p.106]]

#### BBR : Bottleneck Bandwidth and Roundtrip propagation time 
BBR non si basa più sulle perdite ma stima due parametri: 
- La banda sul link (*bottleneck bandwidth*)
-   L’RTT 

Trasmettere pacchetti a una velocità che non *"dovrebbe"* incontrare accodamenti  

Progettato per rispondere alla congestione effettiva, piuttosto che alla perdita di pacchetti  

Server-side algorithm : Non richiede al client di implementare BBR  

**pacing**: invece di inserire nella CWND (e inviare) tutti i pacchetti consentiti, li inserisco al ritmo al quale può inviarli il *nodo più lento*  

![[EMBED/reti_cap3_PC 22.png]]

[[reti_cap3_PC.pdf#page=109&rect=117,20,546,741|reti_cap3_PC, p.109]]

Ottimo insieme ad HTTP/2, che usa una singola connessione
Il risultato finale è un traffico più veloce sulle odierne dorsali ad alta velocità e una larghezza di banda notevolmente aumentata e tempi di download ridotti  

#### QUIC
Due obiettivi 
- Evitare fenomeni di Head-of-Line blocking Utilizzando di base UDP invece di TCP 
- Ridurre la latenza rispetto alle connessioni TCP Compattando i messaggi per ridurre l’overhead di connessione  

Può essere implementato a livello applicativo anziché nel kernel

QUIC incorpora nel processo di handshake iniziale lo scambio delle chiavi di configurazione e dei protocolli supportati  

Quando un client apre una connessione, il pacchetto di risposta include anche i dati per i pacchetti futuri necessari all'uso della crittografia in TLS  

Con TCP Le socket sono identificate dalla quadrupla 
(IP sorgente, porta sorgente, IP destinazione, porta destinazione). Nell'evento di un cambio di rete Tutte le connessioni vanno in timeout e vengono ristabilite.

QUIC include un ID di connessione al server, che non dipende dalla fonte o dalla rete usata, Si può così ristabilire la connessione inviando nuovamente un pacchetto contenente tale ID.

