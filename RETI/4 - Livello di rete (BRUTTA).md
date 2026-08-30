Obiettivo: spostare pacchetti dal mittente al destinatario  

Al mittente, incaspula i segmenti in datagrammi  
Al ricevente, recupera i segmenti e li consegna al livello di trasporto  

Ci sono protocolli di livello rete in tutti gli host e router  
I router ispezionano tutti gli header dei datagrammi che li attraversano per prendere decisioni di inoltro  

Le due funzioni del livello di rete  :
> Inoltro (forwarding)

- Operazione locale con scala locale
- Spostamento del pacchetto da un ingresso del router a un’uscita  

Legato al Data plane (“Piano dati”) 
- Funzione locale a ogni router 
- Determina come inoltrare un datagramma da una porta di arriva a una porta di uscita del router 
- Funzione di forwarding  

> Instradamento (routing)  

- Operazione locale con scala globale 
- Determinare il percorso che un pacchetto deve seguire 
- Algoritmi di routing  

Legato al Control plane (“Piano controllo”) :
- Logica globale di rete 
- Determina come instradare un datagramma in un percorso end-to-end, cioè dall’host mittente all’host destinatario  

## Il router
![[EMBED/reti_cap4_e_5_PC.png]]

[[reti_cap4_e_5_PC.pdf#page=12&rect=27,169,679,510|reti_cap4_e_5_PC, p.12]]

Obiettivo: non introdurre ritardi ulteriori, prendendo una decisione alla stessa velocità con cui la porta di ingresso riceve i dati  
Utilizzando i valori nei campi dell’header livello 3, trova la porta di uscita corretta usando la tabella di inoltro nella porta di ingresso  

Accodamento nel buffer: se i datagrammi arrivano più in fretta del tasso di smistamento del commutatore  

### Sistemi di commutazione
Trasferiscono i pacchetti dall’ingresso all’uscita appropriata  

**Tasso di commutazione** : frequenza alla quale i pacchetti vengono trasferiti dagli ingressi alle uscite  

Si hanno tre strutture tipo: 
![[EMBED/reti_cap4_e_5_PC 1.png]]

[[reti_cap4_e_5_PC.pdf#page=13&rect=17,30,668,216|reti_cap4_e_5_PC, p.13]]

#### Com. a memoria
Di fatto, computer tradizionali dove la commutazione è controllata dalla CPU.

- Pacchetti copiati nella memoria del computer
- Velocità di commutazione limitata dalla banda dati della memoria 

#### Com. a bus
Si usa un bus dati condiviso interno al router per trasmettere i datagrammi dagli ingressi alle uscite

Contesa del bus: la velocità di commutazione è limitata dalla banda del bus (cioè dalla velocità di trasferimento dati sul bus) 

sufficiente per router di accesso e router interni alle reti aziendali  


#### Com. a matrice
Matrice di punti di interconnessione tra linea di ingresso e linea di uscita.

Supera i limiti di velocità della commutazione a bus

Design avanzato: frammentazione dei datagrammi in celle di lunghezza fissa , che transitano poi attraverso la matrice   

![[EMBED/reti_cap4_e_5_PC 2.png]]

[[reti_cap4_e_5_PC.pdf#page=16&rect=462,47,669,248|reti_cap4_e_5_PC, p.16]]

### Accodamento sulle porte di ingresso
Tre tipi di accodamenti:
- Un commutatore più lento della velocità complessiva delle porte di ingresso causa accodamenti agli ingressi  
- Accodamento sulle porte di uscita
- Blocco “head-of-line” (HOL): un datagramma accodato al primo posto della coda impedisce a quelli successivi di avanzare  

![[EMBED/reti_cap4_e_5_PC 3.png]]

[[reti_cap4_e_5_PC.pdf#page=17&rect=81,13,656,265|reti_cap4_e_5_PC, p.17]]

![[EMBED/reti_cap4_e_5_PC 4.png]]

[[reti_cap4_e_5_PC.pdf#page=19&rect=378,110,681,340|reti_cap4_e_5_PC, p.19]]

La memoria richiesta di un buffer su una porta è pari a un “tipico” RTT moltiplicato la velocità del link C  

### Scheduling
I datagrammi potrebbero essere scartati se le memorie si riempiono  

Scheduling: scelta di quale pacchetto trasmettere su un link  

#### FIFO
inviare i pacchetti in ordine di arrivo nella coda  

Politica di scarto : 
- “Tail drop”:scarto i pacchetti in arrivo
-  “Priority drop”:scarto o cancello basandomi su un livello di priorità
-  “Random drop”:scarto o cancello casualmente  


## Protocollo IP
### Composizione del datagramma
![[EMBED/reti_cap4_e_5_PC 5.png]]

[[reti_cap4_e_5_PC.pdf#page=23&rect=39,57,680,426|reti_cap4_e_5_PC, p.23]]

![[EMBED/reti_cap4_e_5_PC 6.png]]

[[reti_cap4_e_5_PC.pdf#page=24&rect=175,36,535,468|reti_cap4_e_5_PC, p.24]]

- VER (4 bit): numero di versione IP (solo 4 o 6) 
- LUNGHEZZA HEADER (4 bit): indica il numero di parole di 32 bit nell’header (=5 se non ci sono opzioni)
- TIPO DI SERVIZIO (Type of service, ToS) (8 bit): classe di servizio del datagramma ❖Potenzialmente usato per funzioni chiamate “DiffServ ” e “Explicit Congestion Notification” (ECN), Raramente usato in pratica
- LUNGHEZZA TOTALE (16 bit):numero totale di byte nel datagramma , includendo sia l’header sia i dati  
- IDENTIFICAZIONE (ID) DEL DATAGRAMMA (16 bit):numero (di solito sequenziale ) assegnato al datagramma. Usato per raccogliere eventuali frammenti multipli e riassemblare datagramma complessivo 
- FLAG (3 bit): campo i cui bit specificano se il datagramma è un frammento di un datagramma più lungo. Se è così , dicono anche se questo è l’ultimo frammento o no 
- OFFSET DEL FRAMMENTO (13 bit): indica in quale punto del datagramma originale va inserito questo frammento ❖Espresso in multipli di 8 Byte 
- TIME TO LIVE (TTL) (8 bit): intero inizializzato dal mittente che: Viene ridotto di 1 da ogni router per cui passa il datagramma Se raggiunge il valore zero, il router scarta il datagramma e invia un messaggio di notifica al mittente 
- PROTOCOLLO SUPERIORE (8 bit): specifica quale protocollo di livello superior aspettarsi all’inizio dei dati incapsulati nel datagramma (es. 6 = TCP, 17 = UDP)
- CHECKSUM DELL’HEADER (16 bit): come in UDP a 1 della somma con riporto di tutte le parole di 16 bit dell’header 
- INDIRIZZO IP SORGENTE (32 bit): indirizzo IP del mittente INIZIALE
- INDIRIZZO IP DESTINAZIONE (32 bit): indirizzo IP della destinazione FINALE  
- OPZIONI IP: in alcuni casi , usato per controllare l’elaborazione e instradamento dei datagrammi ❖Nella maggioranza dei casi , il campo è vuoto
- PADDING: bit a zero aggiunto se le opzioni non terminano ad un multiplo di 32 bit, in modoche l’header sia un multiplo di 32 bit  

### MTU e frammentazione
Ciascun tipo di hardware specifica un limite di dati che una trasmissione può trasportare (Maximum Transmission Unit, MTU)  

Ciascun datagramma deve contenere al più un numero di Byte pari alla MTU  

Un router potrebbe interfacciarsi a reti con MTU diverse  
Un datagramma su una di queste reti potrebbe essere troppo grande per inviarlo su una delle altre reti  

In questo caso si reframmenta il datagramma:

- I frammenti hanno lo stesso formato di un datagramma normale
- I flag nell’header indicano se un datagramma è un frammento o no 
- Il campo “Offset del frammento” specifica come concatenarli 
- I campi dell'header sono quelli del frammento originale 

Conoscendo l’MTUe la dimensione dell’header, il router calcola quanti frammenti servono, e quanti Byte di dati contiene ciascuno  

Alcuni flag servono alla frammentazione:
- `D`: “do not fragment” (seservisse frammentare , scarta il pacchetto )
- `M`: “more fragments” (=1 perogni frammento) • Eccetto l’ultimo: M=0, ma fragment offset > 0  

La destinazione <mark class="hltr-orange">finale</mark> raccoglie e riassembla i frammenti  
in questo modo I frammenti possono seguire percorsi diversi 

Un datagramma frammentato non può essere ricomposto finché non arrivano tutti i frammenti  

Il protocollo IP specifica il tempo massimo da attendere dopo la ricezione del primo frammento  nel caso gli altri non arrivino

#### Realtà
La frammentazione a livello IP è praticamente disabilitata in Internet  

Se un datagramma è troppo grande viene scartato  

Ragioni:
- I firewall richiedono l’ispezione dell’header TCP/UDP  
- Attacco “overlapping fragments”  
- Attacchi di riempimento memoria (tipo DDoS) ottenuti omettendo di proposito alcuni frammenti  
- mplementazioni errate del codice di assemblaggio dei frammenti  

### Indirizzi IP
Gli host e i router devono usare le stesse convenzioni di indirizzo  

Ogni indirizzo IP pubblicamente raggiungibile deve essere unico e Ogni interfaccia ha (almeno) un indirizzo IP  

I router prendono decisioni di inoltro e instradamento basandosi unicamente sul destination address  

#### Gerarchia
Gli indirizzi IP sono generalmente divisi in due parti 
- Un prefisso : identifica la rete alla quale l’host è allacciato (network ID, o NetID) • Ogni rete in Internet è identificata da un numero unico al mondo • 
- Un suffisso : identifica un’interfaccia di rete allacciata a quella rete (ID dell’host, o HostID) • Ogni interfaccia di rete ha un indirizzo IP unico su quella rete  

##### indirizzamento Classful
Diverse “classi ” di indirizzi , con diverse lunghezze dei prefissi : 
- Classe A (primo bit = 0): 7 bit per il NetID, 24 bit perl’HostID 
- Classe B (primi bit = 10): 14 bit per il NetID, 16 bit perl’HostID 
- Classe C (primi bit = 110): 21 bit per il NetID, 8 bit perl’HostID  

Tutti volevano classi A e B per poter crescere se necessario ❖Lo spazio degli indirizzi fu presto esaurito  

![[EMBED/reti_cap4_e_5_PC 7.png]]

[[reti_cap4_e_5_PC.pdf#page=42&rect=21,111,698,410|reti_cap4_e_5_PC, p.42]]

##### Indirizzamento classless
Suddivisione tra prefisso e suffisso completamente arbitraria  

per conoscere il limite tra prefisso e suffisso si memorizzano 32 bit dove gli unici bit a 1 sono quelli del prefisso,  questa informazione è chiamata “**subnet mask**  





#### Autorità per assegnazione
Internet Corporation for Assigned Names and Numbers (ICANN)  

Creata per gestire l’assegnazione di indirizzi ed arbitrare eventuali dispute 

ICANN non assegna i prefissi direttamente, Invece, autorizza diverse entità dette “registrar” a farlo  

I registrar consentono agli Internet Service Provider (ISP) di accaparrarsi “blocchi ” di indirizzi Gli ISP provvederanno poi ad assegnare sotto-blocchi di questi indirizzi ai loro clienti  


### Indirizzi IP pubblici e privati  
Alcuni blocchi di indirizzi IP sono riservati  : Non possono essere usati come indirizzi di destinazione in Internet  

- Da 10.0.0.0 a 10.255.255.255 (10.0.0.0/8)
- Da 172.16.0.0 a 172.31.255.255 (172.16.0.0/12)
- Da 192.168.0.0 a 192.168.255.255 (192.168.0.0/16)  

Riutilizzabili in tutte le reti private Non escono dalla rete e Sono sempre univoci all’interno di una rete privata. Di solito, sono assegnati dinamicamente  

per passare da un IP pubblico a privato esistono Due metodi:
- Proxy : Computer che ha un indirizzo pubblico e uno privato, e “media” la connessione per applicazioni specifiche 
- NAT: apparato allacciato sia alla rete privata sia ad Internet. Come un divisorio
  
#### NAT : network address translation  
Il NAT cambia l’IP e la porta sorgente del datagramma  con delle versioni "pubbliche".

Supporta Fino a 60,000 connessioni simultanee con un solo IP pubblico  
soluzione alla mancanza di IP pubblici


Soluzione controversa:
- Viola l’architettura a livelli : il NAT di fatto è un router e non dovrebbe cambiare le porte
- la mancanza di indirizzi dovrebbe essere risolta da IPv6
- Costringe i programmatori a tenere conto che potrebbe esistere (es. P2P)

Talvoltaun NAT è l’unico modo di connettere due reti se non si controllano tutti i router  (se una tabella di routing non è completa)

##### Attraversamento del NAT
come fa un client a connettersi a un server che sta dietro un NAT?  
- Port-forwarding : Inoltra tutte le connessioni sulla porta 22 all’host 192.168.0.x
- Hole punching
- UPNP o STUN


### Indirizzi IP speciali  
Gli indirizzi che denotano :
- **la rete in se** = xxx.xxx.xxx.0/yy ( tutti i bit dopo la mask sono a 0)
- L’indirizzo **broadcast** di una rete (“Directed Broadcast Address”)  = 
  xxx.xxx.xxx.1/yy ( come rete ma tutti a 1)
- L’indirizzo **broadcast di rete locale** (“Limited Broadcast Address”). Si riferisce al fatto che i datagrammi con questo indirizzo destinazione non escono mai dalla rete locale : Nessun router li inoltra  = 255.255.255.255  
- Questo **computer senza ip** ( boots up per DHCP) = 000.000.000.000
- Questo computer ( **loopback** ) = 127.0.0.0/8  o 127.0.0.1/8  
- Gli indirizzi **multicast** : Servono a inviare i pacchetti a un gruppo di host distribuito su Internet, anche globalmente  = Da 224.0.0.0 a 239.255.255.255 
- Gli indirizzi link-local  : Sottorete riservata per consentire le comunicazioni quando un host non riesce a “trovare” un indirizzo IP  = 169.254.0.0/16  

 
### ARP : Address Resolution Protocol  
Se l’host destinatario di un datagramma IP si trova sulla mia rete, voglio inviargli il pacchetto direttamente  

1. indirizzo IP del destinatario è C, il mittente invia in broadcast una richiesta “Mi serve il MAC address che corrisponde a C ”
2. La richiesta raggiunge tutti gli host sulla rete
3. L’host con indirizzo IP C risponde con l'indirizzo MAC  

ARP Si fa solo per comunicazioni punto-punto sulla stessa rete  


I messaggi ARP sono interpretati come dati da trasportare e incapsulati nel payload del frame  

![[EMBED/reti_cap4_e_5_PC 8.png]]

[[reti_cap4_e_5_PC.pdf#page=89&rect=45,193,684,324|reti_cap4_e_5_PC, p.89]]

#### ARP caching
per evitare di inviare un messaggio ARP per ogni datagramma , si mantengono in memoria (“cache”) le risposte ARP ricevute  

- Se si riceve una nuova risposta per una certa corrispondenza tra indirizzo IP e indirizzo MAC, questa sovrascrive la precedente
- Le corrispondenze più vecchie vengono cancellate periodicamente  
- La cache viene consultata sempre prima di inviare una richiesta  

#### Proxy ARP 
Un proxy ARP è una macchina che restituisce un indirizzo MAC facendo le veci di un host che si trova su una rete diversa  

Per esempio i router di accesso   isponde con il proprio MAC ai messaggi ARP  E poi si incarica di inoltrare i pacchetti  

### ICMP
 protocollo “partner” di IP Usato per notificare errori alla sorgente di un datagramma
 
Ottimo strumento per gestione e manutenzione delle reti  

interdipendenti:
IP ha bisogno di ICMP per segnalare errori 
ICMP si serve di IP per trasportare i messaggi

![[EMBED/reti_cap4_e_5_PC 9.png]]

[[reti_cap4_e_5_PC.pdf#page=93&rect=27,72,684,192|reti_cap4_e_5_PC, p.93]]

![[EMBED/reti_cap4_e_5_PC 10.png]]

[[reti_cap4_e_5_PC.pdf#page=94&rect=2,58,717,411|reti_cap4_e_5_PC, p.94]]

Due classi di messaggi :
- errori 
- recupero informazioni Es., Echo Request e Echo Reply  

Echo Request/Reply → usati dal comando ping  

I messaggi ICMP funzionano come qualunque pacchetto IP e sono inviati senza particolari priorità  

![[EMBED/reti_cap4_e_5_PC 11.png]]

[[reti_cap4_e_5_PC.pdf#page=96&rect=29,140,683,355|reti_cap4_e_5_PC, p.96]]

### DHCP
Consente di assegnare un indirizzo IP a un host quando questo si accende , in maniera del tutto automatica  

Permette di liberare indirizzi quando non utilizzati   

La procedura è: 
1. L’host invia un messaggio “DHCP discover” (opzionale)
2. Il server DHCP ( software che gira in un altro host, o in un router) risponde con un messaggio “DHCP offer” (opzionale)
3. L’host richiede in indirizzo IP con un messaggio “DHCP request”
4. Il server DHCP invia un messaggio “DHCP ack  

L’indirizzo IP fornito da DHCP rimane valido per un tempo limitato  
Scaduto il client può decider di cambiare indirizzo IP o negoziare un’estensione  

DHCP usa UDP, che non è un protocollo di trasporto affidabile ma è progettato per essere robusto a perdite e duplicati
- Nessuna risposta dal server: l’host ritrasmette la richiesta 
- Se arriva una risposta duplicata : l’host ignora la copia extra  

![[EMBED/reti_cap4_e_5_PC 12.png]]

[[reti_cap4_e_5_PC.pdf#page=104&rect=110,62,632,426|reti_cap4_e_5_PC, p.104]]

![[EMBED/reti_cap4_e_5_PC 13.png]]

[[reti_cap4_e_5_PC.pdf#page=105&rect=24,69,704,427|reti_cap4_e_5_PC, p.105]]

![[EMBED/reti_cap4_e_5_PC 14.png]]

[[reti_cap4_e_5_PC.pdf#page=106&rect=21,196,688,437|reti_cap4_e_5_PC, p.106]]

DHCP può restituire anche
- Nome e l’indirizzo IP del DNS 
- La maschera di rete per suddividere il prefisso dal suffisso
- Altre informazioni (es. il percorso di un file con le istruzioni per configurare un host all’avvio)  

L’indirizzo IP dei messaggi DHCP è quello broadcast di rete locale (255.255.255.255)

Se non c’è nessun DHCP  il client DHCP imposta un indirizzo tipo link-local 169.254.0.0/16  

Passi:
1. Si sceglie casualmente un indirizzo tra 169.254.0.1 e 169.254.255.254
2. Si cerca se esiste un interfaccia di rete con questo indirizzo  Con una risoluzione ARP
3.  Se la si trova, si torna al punto 1
4.  Altrimenti, l’host si tiene l’indirizzo scelto al punto 1  

### IPV6
ampliare la quantità di indirizzi disponibili prima che quelli di IPv4 finissero  

Il formato dell’header velocizza l’elaborazione dei datagrammi e Facilita la gestione della qualità del servizio  

- Lunghezza dell’header fissata ad esattamente 40 byte
-  Frammentazione proibita
- Checksum: rimosso  
- Options: rimosse dall’header, ma consentite fuori dall’header (indicando un valore apposito nel campo “Next Header” 

![[EMBED/reti_cap4_e_5_PC 15.png]]

[[reti_cap4_e_5_PC.pdf#page=118&rect=165,10,558,282|reti_cap4_e_5_PC, p.118]]


ICMPv6 :
- Messaggi aggiuntivi di errore, ad esempio “Packet Too Big” 
- Funzioni di gestione per i gruppi multicast  


#### Transizione IPV4 -> IPV6
Non tutti i router possono essere aggiornati simultaneamente  

Soluzione è Tunneling: i datagrammi IPv6 viaggiano come dati incapsulati dentro datagrammi IPv4  

#### Formato degli indirizzi
128 bit : 8 gruppi da 4 cifre esadecimali

Per accorciarlo :
1. si omettono gli zeri all’inizio di ogni campo, e si scrivono gli zeri consecutivi con un solo “0"
	- `2a03:2880:f108:0083:face:b00c:0000:25de` diventa `2a03:2880:f108:83:face:b00c:0:25de`
2.  I gruppi consecutivi di zeri si rappresentano con un `::` (solo una volta) : 
	- `2a03:2880:f108:0000:0000:0000:0000:25de` diventa `2a03:2880:f108::25de`  

#### Indirizzi speciali

- Non specificato , o “questo computer” → ::/128 (tutti 0)
- Loopback (localhost) → ::1/128 (tutti 0 con un 1 alla fine)
- Indirizzo IPv4 mappato su IPv6 → ::ffff:0:0/96
	- Quindi ::ffff:xxyy:zzww , dove xx.yy.zz.ww sono i bit dell’indirizzo IPv4, espresso in esadecimale 
	- Es: 
		- IPv4 = 193.175.55.16 = c1.af.37.10
		- IPv6 = ::ffff:c1af:3710 
- Multicast: ff00::/8 
- link-local unicast: fe80::/10 

---
## Protocolli di intradamento
Ora vediamo come si crea una tabella di routing  

Obiettivo: determinare “buoni” percorsi da un mittente a un destinatario, attraverso una rete  

- Costo più basso 
- Più veloce 
- Meno congestionato  

Informazioni globali o distribuite?  :
- Globali:
	1. Tutti i router conoscono la topologia della rete ed i costi dei link
	2. Algoritmi a “link state
- Distribuite:
	1. I router conoscono gli altri router cui sono collegati , e il costo dei link verso di essi
	2. Processo iterativo di calcolo percorsi e scambio informazioni con i router vicini 
	3. Algoritmi a “distance vector”  


Statico o dinamico?  
- Statico :  I percorsi cambiano molto poco nel tempo 
- Dinamico : Le rotte cambiano più frequentemente 
	1. Aggiornamenti periodici 
	2. Tipicamente a causa di cambiamenti nel costo dei link  

### Alg. Djikistra - Link state
Assunzione : topologia di rete e costi dei link noti a tutti i nodi  
(ottenute circolando le informazioni sullo stato dei link)  

Output: il percorso a costo minimo da un nodo a tutti gli altri nodi  

dopo k iterazioni, si conosce il percorso a costo minimo verso almeno k destinazioni  

![[EMBED/reti_cap4_e_5_PC 16.png]]

[[reti_cap4_e_5_PC.pdf#page=138&rect=-1,58,691,442|reti_cap4_e_5_PC, p.138]]

![[EMBED/reti_cap4_e_5_PC 17.png]]

[[reti_cap4_e_5_PC.pdf#page=137&rect=370,129,695,395|reti_cap4_e_5_PC, p.137]]

 Complessità dell’algoritmo con una rete di n nodi  : O($n^2$) con implementazioni più efficienti : O($n\cdot log(n)$)  

### Alg. distance vector
Algoritmo distribuito che Non richiede conoscenza della topologia di rete  

Richiede che ogni nodo di rete conosca 
- I propri vicini 
- Il costo dei link verso questi vicini  

La presenza di altri nodi posizionati oltre i propri vicini viene notificata con dei messaggi  

![[EMBED/reti_cap4_e_5_PC 19.png]]

[[reti_cap4_e_5_PC.pdf#page=153&rect=17,48,718,313|reti_cap4_e_5_PC, p.153]]![[EMBED/reti_cap4_e_5_PC 20.png]]

[[reti_cap4_e_5_PC.pdf#page=154&rect=-1,39,707,441|reti_cap4_e_5_PC, p.154]]![[EMBED/reti_cap4_e_5_PC 21.png]]

[[reti_cap4_e_5_PC.pdf#page=155&rect=11,52,686,435|reti_cap4_e_5_PC, p.155]]

Se i costi dei link cambiano
- Le buone notizie (costi minori) viaggiano in fretta
- Le cattive notizie (costi più alti) viaggiano lentamente  

#### Count-to-infinity
Problema dell algortimo

Soluzioni : 
- Massimo numero di hop per la propagazione dei DV
- Split Horizon  : Quando un nodo manda aggiornamenti a un vicino, omette le rotte apprese da quel vicino  
- Poisoned reverse  : Finché un nodo x raggiunge un nodo z attraverso y, x comunica ad y che Dx(z) = ∞  



### AS : autonomous systems
Internet è formata da moltissime sottoreti Idealmente ...
- Amministrativamente autonoma : Algoritmo di routing, configurazione … 
- Capace di collegarsi a tutte le altre reti  

I router sono organizzati in “autonomous systems” 
un AS è un *gruppo di router sotto lo stesso controllo amministrativo*

Ogni AS è identificato da un numero (RFC 1930), ed i numeri di AS sono assegnati centralmente dai registri regionali ICANN 

#### Intra-AS routing: Open Shortest Path First  
OSPF, protocollo di routing basato su link state, Dissemina pacchetti con lo stato dei link. Calcola i percorsi usando l’algoritmo di Dijkstra  

Assunzione : topologia nota a tutti i router 
Usa datagrammi IP  

Implementa tre procedure :
- Protocollo di “Hello” : Messaggi di mantenimento: controllano i link funzionanti, e quindi verificano quali altri nodi sono vicini 
- Protocollo di “Exchange” : Usato per informare i vicini che si sono appena “conosciuti ” sulla topologia della rete nota al momento 
- Protocollo di “Flooding” : Informa tutti i router di un cambio nello stato dei link  

##### Flooding controllato
Invia messaggi ricevuti su un’interfaccia a tutte le altre interfacce  

##### OSPDF gerarchico
Usato in reti con molti router ha una Gerarchia a due livelli : 
- dorsale (“backbone”) 
- reti di area

I messaggi con i link state circolano solo nelle reti di area
I nodi conoscono la topologia di rete solo all’interno della propria area, e uno shortest path verso le altre aree  

I router di bordo di ciascuna area “riassumono ” la distanza verso le reti che fanno parte della propria area agli altri router  

I router della dorsale fanno girare OSPF solo per la dorsale  

I router di bordo connettono ad altri AS  

![[EMBED/reti_cap4_e_5_PC 18.png]]

[[reti_cap4_e_5_PC.pdf#page=148&rect=25,43,722,465|reti_cap4_e_5_PC, p.148]]

##### Manipolazione di OSPF
L’unica cosa che influenza la scelta dei percorsi è il costo dei link  
Molti operatori potrebbero “agire” sul costo dei link per ottenere un certo controllo  

Esempio : se so che il traffico che entra nella mia rete da un certo router è quasi tutto destinato ad un’altra rete specifica , faccio in modo che OSPF scelga il percorso che desidero io tra i due punt  i


#### Intra-AS: Routing Information Protocol  
Protocollo che Implementa distance vector
Vantaggi : 
- Semplice da implementare e gestire 
Svantaggi 
- Convergenza lenta
- Dimensione della rete limitata

l costo dei link è il numero di hop  : Al massimo = 15,mentre 16 = ∞  

Ogni 30 secondi (o se cambiano le tabelle di routing) RIP invia i distance vector   
( RIP advertisement )
Ogni messaggio contiene un elenco comprendente :
- Fino a 25 sottoreti di destinazione all’interno dell’AS 
- La distanza del mittente rispetto a ciascuna di tali sottoreti

Se un router non riceve notizie dal suo vicino per 180 s :
1. Il nodo adiacente/il collegamento viene considerato guasto
2. RIP modifica la tabella d’instradamento locale
3. Propaga l’informazione mandando annunci ai router vicini
4. I vicini inviano nuovi messaggi (se la loro tabella d’instradamento è cambiata) 
5. L’informazione che il collegamento è fallito si propaga su tutta la rete 
6. Poisoned reverse per evitare i loop (distanza infinita = 16 hop  )

Un processo sul router esegue RIP, ossia mantiene le informazioni d’instradamento e scambia messaggi con i processi routed nei router vicini .

![[EMBED/reti_cap4_e_5_PC 22.png]]

[[reti_cap4_e_5_PC.pdf#page=185&rect=6,46,712,535|reti_cap4_e_5_PC, p.185]]

#### inter-AS networking  : BGP Border Gateway Protocol  

Far parlare i router di bordo tra loro per realizzare questa connessione tra diversi AS  

Gli AS comunicano tra loro per condividere info di raggiungibilità  
Ogni AS decide autonomamente i propri punti di ingresso e di uscita  
Ogni AS può decidere di condividere informazioni con alcuni vicini ma non con altri  

Esistono Transit AS  che non condividono conoscenze sulla propria rete, ma fanno solo da tramite  
 
Principio chiave: libertà amministrativa per ogni AS, che:
- Può decidere se publicizzare info di raggiungibilità per le proprie reti interne oppure no 
- Può decidere di ritrarre la raggiungibilità di qualunque propria rete 
- Può decidere offrire o meno transito verso altri AS in ogni momento  

Ogni AS ha un certo numero di router detti «BGP speaker»  
BGP possono parlare attraverso una connessione TCP tra loro  
Una volta connessi,  possono scambiarsi informazioni di raggiungibilità  

BGP è un Path vector protocol : Le informazioni condivise includono non solo la raggiungibilità di un certo prefisso, ma anche il percorso per raggiungerlo  

La struttura base di BGP è data da una macchina a stati finiti ed il funzionamento del protocollo è dettato dallo stato in cui ci si trova

##### Best - path BGP
Il best path in BGP è diverso dal Best Path di OSPF o RIP, in cui si preferiva il percorso più breve

Il concetto di “preferito” in BGP è vago e dipende strettamente dalle policy degli AS 
Non è detto che il best path BGP sia il percorso più veloce  


I best path vengono solitamente installati nella routing table del router che fa da speaker BGP  

##### Messaggi
 BGP usa 4 messaggi :
 - Open : Usato per aprire una nuova connessione con un altro nodo BGP
 - Notification : Usato per condividere errori
 - KeepAlive :Usato per mantenere attiva la connessione 
 - Update : Usato per inoltrare conoscenza  

###### OPEN
Se l’OPEN message viene accettato, viene inviato un messaggio KEEPALIVE di conferma e a connessione viene considerata aperta  

L’hold time è una proposta che si scambiano i due BGP speaker riguardo al tempo di validità della rotta  

###### KEEPALIVE
Devono essere inviati in modo tale da non lasciar scadere l’ hold timer che è stato scelto per la connessione

Possono essere disattivati impostando un Hold Timer di 0  

###### UPDATE
Contengono informazioni di raggiungibilità ed attributi correlati
I messaggi di update sono i responsabili per la disseminazione delle informazioni 

Le informazioni possono essere di due tipi: 
- Additive : Portano informazioni di raggiungibilità riguardanti nuovi percorsi
- Sottrattive : Rimuovono percorsi per raggiungere una destinazione  

##### Withdraw
Un withdraw è l’azione di rimozione di una rotta 
All’interno di un pacchetto di update vi è una sezione apposita per le rotte che vengono cancellate  

Si effettua un withdraw quando non vi è più nessun percorso disponibile verso un certo AS  

 Nel caso in cui un nodo BGP conoscesse un percorso alternativo per raggiungere una rotta, potrebbe decidere di ...
 1. Rimuovere la rotta precedente 
 2. Utilizzare il nuovo percorso e condividerlo  

Implicit WITHDRAW : Permette di non inviare un pacchetto di withdraw seguito da uno di update, ma solamente uno di “aggiornamento” del percorso migliore  

##### Filtri
I filtri controllano ciò che entra e ciò che esce  

 Possono esserci filtri specifici per ogni connessione BGP 
 - Ingress filters 
 - Egress filters  

Un pacchetto che non supera i filtri viene scartato  


##### Interconnessioni tra AS  
Client – Provider:
Il client paga il provider per ottenere raggiungibilità 

Provider – Client:
Il provider viene pagato dal client per fornirgli raggiungibilità

Peer:
Due nodi hanno una relazione di peering nel momento in cui condividono tutte le proprie conoscenze di raggiungibilità  

##### Policies
Le interconnessioni tra AS sono controllate da contratti 

Le policies controllano ciò che sono abilitato a condividere con un certo vicino e cosa no 

Ingress Policies :Politiche applicate alle rotte che voglio importare nel mio sistema 
Egress Policies : Politiche applicate alle rotte prima che vengano esportate  

  
##### RIB : Routing information Base   
BGP sostanzialmente utilizza 3 tabelle per le rotte  

- Routing Table : Contiene i best path attuali
- ADJ_RIB_IN :Contiene le rotte che state accettate in ingresso, Usato per valutare percorsi alternativi
-  ADJ_RIB_OUT : Contiene le rotte che hanno superato i filtri in uscita e che devono essere condivise  

