---
Date created: 13-06-26 • 14:47
tags:
  - Sistemi-Operativi
  - IA-GEN
Related PDF/DOC:
  - "[[13-File_System.pdf]]"
Related Pages:
---
### 1. Il Concetto di File e l'Interfaccia del File System

> Il File System è il meccanismo che permette la memorizzazione e l'accesso a dati e programmi, astraendo dalle caratteristiche fisiche dei supporti tramite una visione logica.

- **File**: Uno spazio di indirizzamento logico e contiguo identificato da un nome. Possono essere di vari tipi (dati numerici, caratteri, video, ecc.) o programmi.
- **Attributi**: Ogni file possiede metadati come nome, tipo, posizione fisica sul disco, dimensione, permessi di protezione e identificazione dell'utente.
- **Operazioni**: Le system call di base includono creazione, scrittura, lettura, riposizionamento del puntatore corrente, cancellazione, troncamento (mantenimento degli attributi ma eliminazione del contenuto), apertura e chiusura.

### 2. Metodi di Accesso e Struttura delle Directory

Esistono due modalità principali per accedere alle informazioni:

- **Accesso Sequenziale**: Lettura e scrittura avvengono in ordine, una dopo l'altra (es. editor e compilatori).
- **Accesso Diretto**: Il file è visto come una sequenza numerata di blocchi, permettendo di leggere o scrivere un blocco specifico $n$ (es. database).

Le **directory** organizzano i file e contengono informazioni su di essi. L'organizzazione logica può essere:

- **A un livello**: Singola cartella per tutti gli utenti (difficile da gestire per nomi duplicati).
- **A due livelli**: Una directory separata per ogni utente.
- **Ad albero**: La struttura standard attuale, con directory correnti e percorsi assoluti o relativi.
- **A grafo (aciclico o generico)**: Permette la condivisione di file tramite **link simbolici** (puntatori al percorso) o **hard link** (contatori di riferimenti).

### 3. Implementazione del File System

L'implementazione segue un'architettura **a livelli**, dove ogni strato utilizza le funzioni di quello inferiore (dal Logical File System fino al controllo dei dispositivi fisici).

- **Strutture su disco**: Includono il blocco di boot, blocchi di controllo delle partizioni, descrittori di file (come l'**i-node** in Unix) e strutture di directory.
- **Strutture in memoria**: Tabelle delle partizioni montate, cache delle directory recenti e tabelle dei file aperti (sia globali che per processo).

### 4. Metodi di Allocazione dello Spazio su Disco

Il sistema operativo deve decidere come assegnare i blocchi fisici ai file:

- **Allocazione Contigua**: Ogni file occupa blocchi adiacenti. È veloce per l'accesso (sia sequenziale che casuale) ma soffre di frammentazione esterna.
- **Allocazione a Lista**: I blocchi sono sparsi e collegati da puntatori. Facile da estendere e senza frammentazione esterna, ma lenta per l'accesso casuale e poco affidabile se si perde un puntatore. Una variante è la **FAT (File-Allocation Table)** di MS-DOS.
- **Allocazione Indicizzata**: Ogni file ha un "blocco indice" con gli indirizzi di tutti i suoi blocchi dati. Supporta bene l'accesso casuale. Unix usa uno **schema combinato** con l'**i-node**, che utilizza puntatori diretti, singoli indiretti, doppi e tripli per gestire file di dimensioni enormi.

### 5. Gestione dello Spazio Libero e Prestazioni

Per tracciare i blocchi liberi si usano:

- **Vettori di bit**: Efficienti per trovare blocchi contigui ma richiedono spazio in memoria.
- **Liste concatenate, raggruppamento o conteggio**: Altre tecniche per gestire la mappa dei blocchi disponibili.

Poiché il disco è spesso un collo di bottiglia, le prestazioni vengono migliorate tramite:

- **RAM Disk**: Una porzione di memoria gestita come un disco ultra-veloce per file temporanei.
- **Buffer Cache**: Porzione di memoria che memorizza i blocchi usati di frequente (località spaziale e temporale).
- **Journaling (Log-structured FS)**: Registra i cambiamenti come transazioni in un log prima di applicarli, garantendo un recupero rapido e sicuro in caso di crash.