---
Date created: 19-09-25 • 13:45
tags:
  - Reti
Related PDF/DOC:
  - "[[reti_cap1_PC.pdf]]"
Related Pages:
---
%%MANCA LA PRIMA PARTE 0 -> 25 circa%%

## La comunicazione attraverso la rete
Per ora pensiamo alla rete come una **rete magliata di router**; come avviene quindi la comunicazione?


### Commutazione di circuito
> [!QUOTE] Definizione di commutazione di circuito
> > Circuito dedicato per l’intera durata della sessione (*come nella rete telefonica*).
>
> PDF : [[reti_cap1_PC.pdf#page=25&selection=22,0,26,37|reti_cap1_PC, p.25]]

L'**ampiezza di banda** con questa tecnica è determinata dalla capacità del commutatore.
#### Divisione delle risorse in porzioni

Le *risorse di rete* (*bandwidth*) sono suddivise in porzioni, a cui sono associate vari collegamenti. Se un collegamento non è utilizzato <mark class="hltr-red">rimane inattivo fino alla prossima comunicazione</mark>.

La divisione in porzioni può essere fatta attraverso diverse tecniche:

##### Time Division Multiplexing - TDM
---
![[EMBED/reti_cap1_PC.png]]
[[reti_cap1_PC.pdf#page=28&rect=115,28,359,750|reti_cap1_PC, p.28]]

---

##### Frequency Division Multiplexing
---

![[EMBED/reti_cap1_PC 1.png]]

[[reti_cap1_PC.pdf#page=28&rect=360,33,575,772|reti_cap1_PC, p.28]]

---

### Commutazione di pacchetto
> [!QUOTE] Definizione di commutazione di pacchetto
> > I messaggi di una sessione utilizzano le risorse su richiesta, e di conseguenza potrebbero dover attendere per accedere a un collegamento.
>
> PDF : [[reti_cap1_PC.pdf#page=25&selection=30,0,37,12|reti_cap1_PC, p.25]]

Nella commutazione di pacchetto <mark class="hltr-orange">Il flusso di dati punto-punto viene suddiviso in pacchetti</mark>. Inoltre : 
- I pacchetti **condividono le risorse di rete**
- I pacchetti **utilizzano completamente le risorse fisiche** quando vengono trasmessi.
- Chi inoltra un pacchetto deve **riceverlo per intero** prima di cominciare a trasmettere sul collegamento in uscita (*Store and forward*).

Queste caratteristiche però introducono una <mark class="hltr-purple">contesa per le risorse</mark> e quindi inevitabilmente portano a <mark class="hltr-red">congestioni</mark> (*accodamento dei pacchetti, attesa per l’utilizzo del collegamento*).


%%pp31->34%%

### Ritardi di una rete
Esistono diversi modi in cui si possono verificare eventi spiacevoli in una rete :

Solitamente un **ritardo** accade quando il tasso di arrivo dei pacchetti sul collegamento eccede la capacità di evaderli del router e dei collegamenti di uscita.

Le attività di una trasmissione che possono creare ritardi sono : 
- **Ritardo di elaborazione del nodo** : 
	- Controllo errori sui bit.
	- Determinazione della porta o del canale di uscita.
- **Ritardo di accodamento** : 
	- Attesa di trasmissione.
	- Livello di congestione del router.
- **Ritardo di trasmissione (L/R) :**
	- $R$ : frequenza di trasmissione del collegamento (*in bit/s*).
	- $L$ : Lunghezza del pacchetto (*in bit*).
	- $L/R$ : Tempo (o ritardo) di trasmissione.

---
Esiste inoltre un *ritardo nascosto* relativo al **ritardo di accodamento** :

![[EMBED/reti_cap1_PC 3.png]]
[[reti_cap1_PC.pdf#page=55&rect=116,42,547,767|reti_cap1_PC, p.55]]

---

In generale il ritardo su un nodo si può calcolare attraverso la formula :
$$
d_{node} = d_{proc} +d_{queue}+d_{trans}+d_{prop} 
$$
Dove : 
- $d_{proc}$   : ritardo di elaborazione.
- $d_{queue}$ : ritardo di accodamento.
- $d_{trans}$ : ritardo di trasmissione.
- $d_{prop}$ : ritardo di propagazione.

> [!warning]- Traceroute
> > [!QUOTE] Definizione di Traceroute
> > >Programma diagnostico che fornisce una misura del ritardo dalla sorgente al router lungo i percorsi Internet punto-punto verso la destinazione
> >
> > PDF : [[reti_cap1_PC.pdf#page=56&selection=9,0,14,21|reti_cap1_PC, p.56]]
>
> 


### Perdita di pacchetti in una rete
Un buffer ha capacità finita di contenere pacchetti, questo vuol dire che quando un pacchetto di troppo arriva in un nodo, questo viene <mark class="hltr-red">scartato</mark>.

Il pacchetto, in seguito, può essere ritrasmesso o dal **mittente** o dal **nodo precedente**; oppure non essere ritrasmesso affatto.

#### Throughput

> [!QUOTE] Definizione di Throughput
> > Frequenza (*dati/unità di tempo*) alla quale una certa unità dati viene trasferita tra mittente e ricevente: 
> > - **Instantaneo**: in un determinato istante.
> > - **Medio**: in un periodo di tempo più lungo.
>
> PDF : [[reti_cap1_PC.pdf#page=60&selection=6,0,20,32|reti_cap1_PC, p.60]]
> 


>[!info]- Bottleneck
> Collegamento su un percorso punto-punto che vincola il throughput end to end.


La formula si può esprimere come :
$$
\{bit,\ pacchetti,\ …\} \cdot \{secondo,\ sessione,\ …\}
$$

>[!example]- Esempio grafico di throughput
> ![[EMBED/reti_cap1_PC 4.png]]
>[[reti_cap1_PC.pdf#page=62&rect=125,37,575,774&color=yellow|reti_cap1_PC, p.62]]

%%spacing%%
^space

## La struttura di Internet
La struttura di Internet di basa su una **gerarchia**.

---
Alla base della rete sono gli <mark class="hltr-orange">ISP di livello 1</mark>, di cui le caratteristiche sono :
- Copertura nazionale / **internazionale**.
- Comunicano tra di loro come “pari” (*peer*).
	- Gli ISP di livello 1 sono direttamente connessi tra loro attraverso il cosiddetto *“peering”*.

---
Al livello sottostante sono gli <mark class="hltr-blue">ISP di livello 2</mark> :
- Nazionali o distrettuali.
- Può connettersi sia a ISP di livello 1 che 2
	- Di base un ISP2 paga un ISP1 (*o multipli*) che gli fornisce la connettività per il resto della rete.
	- Un ISP2 può essere direttamente connesso a un altro ISP2, anche per essi si parla di *“peering"*.

---
Infine sono presenti gli <mark class="hltr-green">ISP di livello 3</mark>, le più vicine agli utenti :
- Locali, molto vicine agli utenti.
- Clienti degli ISP superiori che gli forniscono connettività
---


> [!example]- Esempio di infrastruttura Internet più vicina alla realtà
> 
![[EMBED/reti_cap1_PC 2.png]]
[[reti_cap1_PC.pdf#page=39&rect=90,17,598,782|reti_cap1_PC, p.39]]

## I protocolli
%%pp66%%


## Lo stack ISO/OSI e TCP/IP