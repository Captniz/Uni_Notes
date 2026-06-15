---
Date created: 13-06-26 • 14:47
tags:
  - Sistemi-Operativi
  - IA-GEN
Related PDF/DOC:
  - "[[12-Memoria Secondaria.pdf]]"
Related Pages:
---
### Tipologie di Supporto

I dispositivi di memoria secondaria si dividono principalmente in tre categorie:

- **Nastri magnetici:** Caratterizzati da un **accesso sequenziale**, sono molto più lenti dei dischi per quanto riguarda i tempi di posizionamento della testina. Oggi vengono utilizzati quasi esclusivamente per il **backup** dei dati.
- **Dischi magnetici (HDD):** Organizzati fisicamente in piatti, cilindri, tracce e settori. La lettura e la scrittura avvengono tramite una testina sospesa sulla superficie.
- **Dispositivi a stato solido (SSD):** Utilizzano memorie flash NAND. Rispetto agli HDD, sono **più silenziosi**, hanno **tempi di accesso inferiori** e non necessitano di deframmentazione, ma hanno un limite sul numero di scritture possibili.

### Prestazioni e Tempi di Accesso

Il tempo di accesso a un disco magnetico è la somma di tre componenti:

1. **Seek time:** Il tempo necessario per spostare la testina sulla traccia desiderata. È la **componente dominante** nelle prestazioni.
2. **Latency time (latenza):** Il tempo necessario affinché il settore desiderato ruoti fin sotto la testina.
3. **Transfer time:** Il tempo effettivo di trasferimento dei dati mentre il settore passa sotto la testina.

Per massimizzare la banda (byte trasferiti per unità di tempo) e minimizzare il tempo di accesso totale, il sistema operativo deve ottimizzare gli spostamenti della testina.

###  Algoritmi di Disk Scheduling

Gli algoritmi di scheduling decidono l'ordine in cui servire le richieste di I/O pendenti per ridurre il _seek time_:

- **FCFS (First-Come, First-Served):** Serve le richieste nell'ordine di arrivo.
- **SSTF (Shortest Seek Time First):** Serve la richiesta più vicina alla posizione attuale della testina. Può causare **starvation** (attesa indefinita) per le richieste lontane.
- **SCAN (Algoritmo dell'ascensore):** La testina si sposta da un'estremità all'altra del disco servendo le richieste lungo il percorso, per poi invertire la direzione.
- **C-SCAN (SCAN Circolare):** Simile a SCAN, ma quando raggiunge un'estremità torna immediatamente all'inizio senza servire richieste nel tragitto di ritorno, offrendo tempi di attesa più uniformi.
- **LOOK e C-LOOK:** Varianti di SCAN/C-SCAN dove la testina inverte la marcia o resetta la posizione non appena non ci sono più richieste nella direzione attuale, senza dover raggiungere l'estremità fisica del disco.
- **LIFO (Last-In-First-Out):** Utile in casi di elevata località dei dati, ma presenta rischio di starvation.

###  Gestione e Formattazione del Disco

- **Vista Logica:** Il sistema operativo vede il disco come un vettore unidimensionale di **blocchi logici (cluster)**, che rappresentano l'unità minima di trasferimento.
- **Formattazione:**
    - **Fisica (basso livello):** Divide il disco in settori leggibili dal controllore e aggiunge codici di correzione errore (ECC).
    - **Logica:** Il sistema operativo crea il **file system**, le partizioni e le strutture dati (come la FAT o la lista dei blocchi liberi).
- **Blocchi difettosi (Bad Blocks):** Identificati tramite l'ECC. Possono essere gestiti **off-line** (marcandoli come inutilizzabili nel file system) o **on-line** tramite il **sector sparing**, dove il controllore rimappa il blocco difettoso su uno di riserva (spare block) in modo trasparente al sistema operativo.

### 5. Area di Swap

L'area di swap viene utilizzata dalla memoria virtuale come estensione della RAM.

- **Collocazione:** Può risiedere all'interno del normale file system (meno efficiente) o in una **partizione separata** dedicata.
- **Gestione:** Una partizione dedicata evita l'overhead del file system e viene gestita da un processo specifico (_swap daemon_). Lo spazio può essere allocato alla creazione del processo o solo quando una pagina deve essere espulsa dalla memoria fisica.

### Interfacce di Connessione

- **IDE:** Utilizza una trasmissione dati **parallela**, che limita velocità e lunghezza dei cavi.
- **SATA:** Utilizza una trasmissione **seriale**, permettendo velocità di trasferimento molto più elevate e maggiore flessibilità nei collegamenti.