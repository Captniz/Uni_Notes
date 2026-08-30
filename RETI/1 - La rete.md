---
Date created: 19-09-25 • 13:45
tags:
  - Reti
Related PDF/DOC:
  - "[[reti_cap1_PC.pdf]]"
Related Pages:
---
## La rete
> *Dispositivi collegati*, attraverso uno o più canali.

Una rete è composta da ...
- **Host**, cioè sistemi *terminali* (*end-point*) che sono i mittenti e destinatari della rete.
- Canali di collegamento
- **Router intermedi**, che fanno da intermediari tra più reti diverse.

Una rete è unita attraverso dei **canali** ( *Rame, fibra ottica, onde elettromagnetiche, satellite* ), che forniscono un <mark class="hltr-purple">ampiezza di banda</mark>, cioè una frequenza di trasmissione (*bit/secondo*).

Un **protocollo** definisce il formato e l’ordine dei messaggi scambiati fra due o più entità in comunicazione.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### L'internet - Rete delle reti
> Infrastruttura di comunicazione per applicazioni distribuite.

In particolare l'internet è una struttura gerarchica di reti, composta principalmente da *Internet* pubblica e varie *Intranet* private.

L'internet offre alle applicazioni che lo utilizzano vari tipi di comunicazione:
- Servizio <mark class="hltr-purple">affidabile</mark> dalla sorgente alla destinazione.
- Servizio <mark class="hltr-orange">“best effort”</mark> ( *non affidabile* ) senza connessione. 
#### Standard e protocolli di internet
> Un protocollo **definisce il formato e l’ordine dei messaggi scambiati tra due o più entità in comunicazione**, così come le azioni intraprese in fase di trasmissione e/o ricezione di un messaggio o di un altro evento.

L'internet è regolato da rigidi standard, contenuti e organizzati nell'RFC. Questi vengono imposti dall'IETF o *Internet Engineering Task Force*.

L'<mark class="hltr-blue">RFC</mark> ( *Request For Comments* ) si tratta di una serie di documenti tecnici che descrivono protocolli, procedure, standard e tecnologie utilizzati su Internet.

L'<mark class="hltr-purple">IETF</mark> è un'organizzazione internazionale composta da esperti che collaborano per progettare, sviluppare e promuovere gli standard tecnici di Internet.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Struttura di internet
> Struttura gerarchica di reti, suddivisa per livelli.

Durante una comunicazione un pacchetto passa attraverso molte reti. Spesso il percorso di andata e ritorno tra due host <mark class="hltr-orange">non è lo stesso</mark>.

Internet e composto da diversi livelli ...
##### Livello 1 - ISP Centrali
> Copertura *nazionale / internazionale*.

Comunicano tra di loro come **“peer”** (pari).

##### Livello 2 - ISP Nazionali
> Copertura strettamente nazionale

Si può <mark class="hltr-orange">connettere solo</mark> ad alcuni **ISP di livello 1** e possibilmente ad altri **ISP di livello 2**.

Un livello 2 paga l’ISP di livello 1 che gli fornisce la *connettività*; quindi un ISP di livello 2 è cliente di un ISP di livello 1.

##### Livello 3 - ISP Locali o di accesso
> Reti *“ultimo salto”* (*last hop network*), le più vicine ai sistemi terminali.

I livelli 3 sono clienti dei livelli 2.

---
### Architettura di comunicazione su una rete
La comunicazione tra host su una rete può seguire una di due architetture diverse ...

#### Architettura client/server
> L’host *client* richiede e riceve un servizio da un programma *server* in esecuzione su un altro host.

In questa architettura gli host sono divisi in ...
- <mark class="hltr-purple">Client</mark> : Si connettono al server e sono solitamente gli utenti del servizio.
- <mark class="hltr-orange">Server</mark> : Gestore del servizio e spesso anche delle comunicazioni client-server e tra diversi client connessi ad esso. 


> [!example]- Esempi di architettura client/server
> - Browser/server Web
> - Client/server e-mail



#### Peer to peer
> Comunicazione diretta tra due host, talvolta mediata al tempo di connessione da un server.


> [!example]- Esempi di architettura peer-to-peer
> - Skype
> - Bit Torrent


---

### Accesso e collegamento ad una rete

Esistono diversi tipi di rete per adattarsi a diversi tipi e numeri di dispositivi ...
- Reti di accesso **residenziale**.
- Reti di accesso **aziendale** ( *università, istituzioni, aziende* ).
- Reti di accesso **mobile**.
#### Accesso residenziale / punto-punto
L'accesso residenziale può utilizzare diverse tecnologie...


> [!example] Schema di una tipica rete domestica
> ![[EMBED/reti_cap1_PC 10.png]]

[[reti_cap1_PC.pdf#page=19&rect=308,34,540,768|reti_cap1_PC, p.19]]


<mark class="hltr-blue">Modem dial-up 56 kbit/s</mark> con accesso diretto al router. Essendo collegato alla linea telefonica non è possibile “navigare” e telefonare nello stesso momento.


> [!example]- Schema 56k
> ![[EMBED/reti_cap1_PC 5.png]]
>
[[reti_cap1_PC.pdf#page=13&rect=111,488,295,740|reti_cap1_PC, p.13]]


<mark class="hltr-blue">DSL</mark> (*Digital Subscriber Line*), cioè un collegamento attraverso linea dedicata gestito dalla società telefonica.


> [!example]- Schema DSL
> ![[EMBED/reti_cap1_PC 7.png]]
>
[[reti_cap1_PC.pdf#page=13&rect=292,388,601,742|reti_cap1_PC, p.13]]

<mark class="hltr-blue">FTTH</mark> (*Fiber To The Home*), cioè un collegamento in fibra ottica gestito dall'ISP che gestisce la rete ethernet.

> [!example]- Schema FTTH
> ![[EMBED/reti_cap1_PC 9.png]]
>
[[reti_cap1_PC.pdf#page=14&rect=121,7,431,547|reti_cap1_PC, p.14]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Accesso aziendale
> Una **LAN** (*Local Area Network*), collega i sistemi terminali di aziende e università (*anche residenziali*) all’edge router.

Spesso collegata attraverso una rete ethernet per velocità e affidabilità. Nei sistemi moderni i sistemi terminali vengono collegati mediante uno switch Ethernet.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Accesso mobile / wireless
> Una rete condivisa d’accesso wireless collega i sistemi terminali al router attraverso una stazione base detta *access point*.

La rete d’accesso wireless geografica (*giga*) è gestita da un provider di telecomunicazioni.

---

### Trasmissione dei dati


> [!info] Glossario
> - **Bit**: Unità di informazione base 0/1, viaggia da un sistema terminale a un altro, passando per una serie di coppie trasmittente-ricevente.
> - **Mezzo fisico**: Strumento che permette il passaggio di bit tra trasmittente e il ricevente.
> 	- **Mezzi guidati**: I segnali si propagano in un mezzo fisico: fibra ottica, filo di rame o cavo coassiale.
> 	- **Mezzi a onda libera**: I segnali si propagano nell’atmosfera, o nello spazio esterno.

#### Mezzi fisici guidati

##### Doppino (TP)
> Due fili di rame avvolti. Tipico cavo usato per lo standard Ethernet.

Può avere diversi tipi di schermatura ...
- Nessuna (*unshielded*)
- Schermatura per doppino (*shielded*)
- Lamina o maglia che avvolge l’intero cavo (*foiled*)
- Entrambe (*screened*)

##### Cavo coassiale
> Due conduttori in rame concentrici con trasmissione bidirezionale.

Può essere a ...
- **Banda base**: Singolo canale sul cavo
- **Banda larga**: Più canali sul cavo

##### Fibra ottica
> Mezzo sottile e flessibile che conduce impulsi di luce.

Alta frequenza trasmissiva e basso tasso di errore (*immune all’interferenza elettromagnetica*). E' il mezzo preferito per collegamenti a lungo raggio.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


#### Mezzi a onda libera (radio)
> Trasportano segnali nello spettro elettromagnetico. Permettono la trasmissione bidirezionale.

La trasmissione dei segnali nello spazio li espone a ...
- Riflessione
- Ostruzione 
- Interferenza

... da parte degli ostacoli.

---


### Il nucleo della rete ( backbone )
> Rete magliata di router che interconnettono i sistemi terminali.

La comunicazione attraverso la backbone può avvenire secondo due architetture... 

#### Commutazione di circuito
> Circuito dedicato per l’intera durata della sessione (*come nella rete telefonica*).

L'**ampiezza di banda** con questa tecnica è determinata dalla capacità del commutatore.

Le *risorse di rete* (*bandwidth*) sono suddivise in porzioni, a cui sono associate vari collegamenti. Se un collegamento non è utilizzato <mark class="hltr-red">rimane inattivo fino alla prossima comunicazione</mark>.


> [!example]- Esempio grafico
> ![[EMBED/reti_cap1_PC 11.png]]
>
[[reti_cap1_PC.pdf#page=27&rect=335,400,568,782|reti_cap1_PC, p.27]]

> [!example]- Esempio numerico
> > Quanto tempo occorre per inviare un file di 640.000 bit dall’host A all’host B su una rete a commutazione di circuito? 
> 
> - Tutti i collegamenti presentano un bit rate di 1,536 Mbit/s
> - Ciascun collegamento utilizza 1 slot di un sistema TDM con 24 slot/secondo
> - Si impiegano 500 ms per negoziare e stabilire un circuito punto-punto


La divisione in porzioni può avvenire secondo il **tempo** o la **frequenza** ...

##### Time Division Multiplexing - TDM


> [!example] Schema di TDM
>
>![[EMBED/reti_cap1_PC.png]]
[[reti_cap1_PC.pdf#page=28&rect=115,28,359,750|reti_cap1_PC, p.28]]

##### Frequency Division Multiplexing

> [!example] Schema di FDM
>
![[EMBED/reti_cap1_PC 1.png]]
>
[[reti_cap1_PC.pdf#page=28&rect=360,33,575,772|reti_cap1_PC, p.28]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


#### Commutazione di pacchetto
> I messaggi di una sessione utilizzano le risorse su richiesta, e di conseguenza potrebbero dover attendere per accedere a un collegamento.

Nella commutazione di pacchetto <mark class="hltr-orange">Il flusso di dati punto-punto viene suddiviso in pacchetti</mark>. Inoltre : 
- I pacchetti **condividono le risorse di rete**
- I pacchetti **utilizzano completamente le risorse fisiche** quando vengono trasmessi.
- Chi inoltra un pacchetto deve **riceverlo per intero** prima di cominciare a trasmettere sul collegamento in uscita (*Store and forward*).

Queste caratteristiche però introducono una <mark class="hltr-purple">contesa per le risorse</mark> e quindi inevitabilmente portano a <mark class="hltr-red">congestioni</mark> (*accodamento dei pacchetti, attesa per l’utilizzo del collegamento*).


> [!example] Esempio di multiplexing TDM per communtazione di pacchetto
> ![[EMBED/reti_cap1_PC 12.png]]
>
[[reti_cap1_PC.pdf#page=31&rect=108,56,433,714|reti_cap1_PC, p.31]]
>
> Dato che la sequenza dei pacchetti A e B non segue uno schema prefissato si assegnano gli slot di tempo secondo un algoritmo statistico.

La commutazione di pacchetto consente a più utenti di usare la rete rispetto alla precedente. Questo perchè non lascia sezioni di banda <mark class="hltr-red">inattivi</mark>.

Tuttavia sono <mark class="hltr-orange">necessari protocolli per il trasferimento affidabile</mark> dei dati e per prevenire o controllare la congestione.


---

### Errori in una rete
#### Ritardi
> Un ritardo accade quando i pacchetti si accodano nei buffer (*memorie*) dei router.

Questo avviene quando il <mark class="hltr-blue">tasso di arrivo dei pacchetti</mark> sul collegamento eccede la <mark class="hltr-purple">capacità di evaderli</mark> del router e dei collegamenti di uscita.

Le attività di una trasmissione che possono creare ritardi sono : 
- **Ritardo di elaborazione del nodo*
	- Controllo errori sui bit.
	- Determinazione della porta o del canale di uscita.
- **Ritardo di accodamento** 
	- Attesa di trasmissione.
	- Livello di congestione del router.
- **Ritardo di trasmissione (L/R)**
	- $R$ : frequenza di trasmissione del collegamento (*in bit/s*).
	- $L$ : Lunghezza del pacchetto (*in bit*).
	- $L/R$ : Tempo (o ritardo) di trasmissione.
- **Ritardo di propagazione (d/s)**
	- $d$ : Lunghezza del collegamento fisico
	- $s$ : Velocità di propagazione del collegamento ($\approx2\cdot108 m/s$).
	-  $d/s$ : Ritardo di propagazione


> [!warning]- Ritardo nascosto : Ritardo di accodamento
> 
![[EMBED/reti_cap1_PC 3.png]]
[[reti_cap1_PC.pdf#page=55&rect=116,42,547,767|reti_cap1_PC, p.55]]



Il ritardo totale di un nodo viene quindi definito come :

$$d_{node} = d_{proc} + d_{queue} +d_{trans} +d_{prop}$$

Dove ...
- $d_{proc}$ : In genere pochi microsecondi
- $d_{queue}$ : Dipende dalla congestione
- $d_{trans}$ : $L/R$ significativo sui collegamenti a bassa velocità
- $d_{prop}$ : Da pochi microsecondi a centinaia di millisecondi,


> [!info]- Traceroute
> > [!QUOTE] Definizione di Traceroute
> > >Programma diagnostico che fornisce una misura del ritardo dalla sorgente al router lungo i percorsi Internet punto-punto verso la destinazione
> >
> > PDF : [[reti_cap1_PC.pdf#page=56&selection=9,0,14,21|reti_cap1_PC, p.56]]
>
> 

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Perdita di pacchetti
Un buffer ha capacità finita di contenere pacchetti, questo vuol dire che quando un pacchetto di troppo arriva in un nodo, questo viene <mark class="hltr-red">scartato</mark>.

Il pacchetto, in seguito, può essere ritrasmesso o dal **mittente** o dal **nodo precedente** oppure non essere ritrasmesso affatto.


> [!warning] Troughtput
>
>> [!QUOTE] Definizione di Throughput
> > Frequenza (*dati/unità di tempo*) alla quale una certa unità dati viene trasferita tra mittente e ricevente: 
> > - **Instantaneo**: in un determinato istante.
> > - **Medio**: in un periodo di tempo più lungo.
> > 
> > PDF : [[reti_cap1_PC.pdf#page=60&selection=6,0,20,32|reti_cap1_PC, p.60]]
> 
> Espresso come :
> $$
>\{bit,\ pacchetti,\ …\} \cdot \{secondo,\ sessione,\ …\}
>$$
>
> > [!info]- Bottleneck
> > Collegamento su un percorso punto-punto che vincola il throughput end to end.
> >
> > ![[EMBED/reti_cap1_PC 13.png]]
> >
> > [[reti_cap1_PC.pdf#page=61&rect=291,19,465,777|reti_cap1_PC, p.61]]
>
> > [!example]- Esempio grafico di throughput
> > ![[EMBED/reti_cap1_PC 4.png]]
> > [[reti_cap1_PC.pdf#page=62&rect=125,37,575,774&color=yellow|reti_cap1_PC, p.62]]

---

### Livelli di protocollo della comunicazione su internet
> Ciascun livello realizza un *servizio*, effettuando determinate azioni all’interno del livello stesso. Ogni livello può utilizzare <mark class="hltr-orange">solo i servizi del livello inferiore</mark>.

Quando si ha a che fare con sistemi complessi suddividere in livelli con una struttura “esplicita” consente l’identificazione dei vari componenti e delle loro relazioni.

Inoltre la modularizzazione facilita la manutenzione e l’aggiornamento di un sistema : 
<mark class="hltr-purple">Le modifiche di un livello risultano trasparenti al resto del sistema</mark>.

#### Layer (Livello)
Ogni layer ...
- Fornisce servizi al layer superiore
- Usa :
	- I servizi del layer inferiore
	- Le proprie funzionalità.

I servizi vengono forniti attraverso i **Service Access Points** (*SAP*).

Il layer $N+1$ conosce solo il servizio $N$ : I layer inferiori ad $N$ sono una *“black box”* per $N+1$.

#### Protocolli
> Lo scambio di informazioni tra entità dello stesso layer è regolata da un protocollo.


> [!example] Schema di protocollo
> ![[EMBED/reti_cap1_PC 14.png]]
>
[[reti_cap1_PC.pdf#page=72&rect=196,61,552,715|reti_cap1_PC, p.72]]

Ad ogni livello è assegnato un protocollo, creando quindi lo **stack dei protocolli internet**; ne esistono due varianti ...


##### TCP

| Protocollo   | Funzione                                                                                            | Esempi                 |
| ------------ | --------------------------------------------------------------------------------------------------- | ---------------------- |
| Applicazione | supporto alle applicazioni di rete                                                                  | FTP, SMTP, HTTP, DNS … |
| Trasporto    | trasferimento dei messaggi di livello applicazione tra il modulo client e server di un’applicazione | TCP, UDP               |
| Rete         | instradamento dei datagrammi dall’origine al destinatario                                           | IP                     |
| Link         | instradamento dei datagrammi attaverso una serie di commutatori di pacchetto                        | PPP, Ethernet          |
| FIsico       | trasferimento dei singoli bit sul canale fisico                                                     |                        |

##### ISO/OSI
> Aggiunge i layer *presentation* e *session*.

| Protocollo   | Funzione                                                                                                                                                                                                                                                                                                                      | Esempi                 |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- |
| Applicazione | supporto alle applicazioni di rete                                                                                                                                                                                                                                                                                            | FTP, SMTP, HTTP, DNS … |
| Presentation | consente alle applicazioni di interpretare il significato dei dati (es. cifratura, compressione, convenzioni specifiche della macchina)                                                                                                                                                                                       |                        |
| Session      | sincronizzazione, controllo, recupero dei dati                                                                                                                                                                                                                                                                                |                        |
| Trasporto    | trasferimento dei messaggi di livello applicazione tra il modulo client e server di un’applicazione. Segmentazione e ricomposizione dei dati e multiplexing e demultiplexing di applicazioni                                                                                                                                  | TCP, UDP               |
| Rete         | instradamento dei datagrammi dall’origine al destinatario.<br><br>Può fornire diversi servizi<br>- Connection-less: Ogni pacchetto viene instradato indipendentemente<br>- Connection oriented: Stabiliscono una rotta all’inizio delle operazioni di trasferimento dati, e la mantengono per tutti i pacchetti da trasferire | IP                     |
| Link         | instradamento dei datagrammi attaverso una serie di commutatori di pacchetto                                                                                                                                                                                                                                                  | PPP, Ethernet          |
| FIsico       | trasferimento dei singoli bit sul canale fisico                                                                                                                                                                                                                                                                               |                        |
<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Data Unit (DU)
In un sistema con $M$ layer, la DU da trasmettere (pacchetto realmente inviato) è detto <mark class="hltr-orange">PDU-M</mark> (*Protocol Data Unit di Layer M*).

Questo è composto da due parti ...

<mark class="hltr-blue">$M-SDU$</mark> ( *Service Data Unit di Layer M*) : Il testo del messaggio, ciò che voglio mandare al destinatario. La <mark class="hltr-red">SDU di layer $M$ è la PDU del layer $M+1$</mark> (*la comunicaizone va verso il basso*).

<mark class="hltr-purple">$M-PCI$</mark> (*Protocol Control Information*) : Dati necessari alla comunicazione tra layer M.


> [!example] Esempio pratico
> **SDU**: testo di una lettera 
> **PCI**: mittente e destinatario 


> [!example] Schema della creazione di una PDU
> ![[EMBED/reti_cap1_PC 15.png]]
>
[[reti_cap1_PC.pdf#page=83&rect=101,111,584,687|reti_cap1_PC, p.83]]

Avendo molti livelli la PDU finale avra un lunghezza importante, per questo le DU possono essere ...
- **Assemblate** : Aggregare più dati di messaggio (*SDU-0*) in una singola PDU 
- **Segmentate** : Opposto dell'assemblare nel caso un messaggio sia troppo lungo; vengono poi ricomposte dal destinatario.

