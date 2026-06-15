---
Date created: 13-06-26 • 14:47
tags:
  - Sistemi-Operativi
  - IA-GEN
Related PDF/DOC:
  - "[[14-RAID.pdf]]"
Related Pages:
---
>Il sistema **RAID** (*Redundant Array of Independent Disks*) nasce per superare i limiti dei dischi singoli (SLED) attraverso l'uso di più dischi in parallelo. Gli obiettivi principali sono:

- **Migliorare l'affidabilità:** Ottenuta tramite la <mark class="hltr-orange">ridondanza</mark>, ovvero la memorizzazione di informazioni supplementari necessarie a ricostruire i dati in caso di guasto.
- **Incrementare le prestazioni:** Ottenuto tramite il <mark class="hltr-purple">parallelismo</mark> delle operazioni di lettura e scrittura.

Le tecniche fondamentali utilizzate sono:

- **Mirroring (Copiato speculare):** Ogni disco logico corrisponde a due dischi fisici; ogni scrittura avviene su entrambi.
- **Data Striping (Sezionamento):** Distribuzione dei dati su più dischi per aumentare la velocità di trasferimento. Può essere a ... 
	- Livello di bit
	- Livello di blocco

## Codici di Correzione e Parità

Per garantire l'integrità dei dati senza ricorrere al costoso raddoppio dei dischi (mirroring), si utilizzano tecniche di rilevamento errori:

- **Bit di parità:** Un bit supplementare associato a ogni byte per identificare errori su singoli bit.
- **Codice di Hamming:** Consente non solo di rilevare l'errore, ma anche di correggere guasti su singoli bit.
- **Codici Reed-Solomon:** Utilizzati in configurazioni avanzate per gestire guasti contemporanei su più dischi.

## Livelli RAID 

### RAID Standard

- **RAID 0 (Non-redundant striping):** Utilizza il sezionamento a livello di blocco senza ridondanza. Offre alte prestazioni ma l'affidabilità cala all'aumentare dei dischi.
- **RAID 1 (Mirroring):** Duplicazione dei dati senza sezionamento. Alta affidabilità e velocità in lettura, ma costi elevati e bassa scalabilità.
- **RAID 2:** Sezionamento a livello di bit con utilizzo di codici ECC (Hamming) memorizzati su dischi separati per la ricostruzione dei dati.
- **RAID 3:** Sezionamento a livello di byte con un singolo disco dedicato alla parità. I controllori rilevano il byte danneggiato e lo ricostruiscono tramite la parità degli altri dischi.
- **RAID 4:** Sezionamento a livello di blocco con un disco dedicato alla parità. Consente letture parallele, ma il disco di parità può diventare un collo di bottiglia nelle scritture.
- **RAID 5:** Sezionamento a livello di blocco con **parità distribuita** su tutti i dischi. Evita il collo di bottiglia del RAID 4 ed è una delle implementazioni più diffuse.
- **RAID 6:** Evoluzione del RAID 5 con doppia parità (o codici diversi). È in grado di tollerare il guasto simultaneo di due dischi.

### RAID Ibridi (Annidati)

Questi livelli combinano le caratteristiche di velocità dello striping e di sicurezza del mirroring:

- **RAID 0+1:** I dati vengono prima sezionati (RAID 0) e poi l'intero set viene duplicato (RAID 1). Non supporta la rottura simultanea di due dischi se non appartengono allo stesso _stripe_.
- **RAID 1+0:** I dati vengono prima duplicati (RAID 1) e poi le coppie vengono sezionate (RAID 0). Risulta **più robusto** del 0+1 perché ogni disco di ogni coppia può guastarsi senza compromettere il sistema.

## Riepilogo 

- **Lettura:** Generalmente migliorata in quasi tutti i livelli (tranne RAID 0) grazie alla possibilità di accedere a più copie o sezioni di dati contemporaneamente.
- **Scrittura:** Può subire rallentamenti nei livelli che prevedono il calcolo della parità (RAID 3, 4, 5, 6) o la doppia scrittura (RAID 1), sebbene i controllori dedicati possano mitigare l'impatto sulla CPU.

| **Livello RAID** | **Caratteristiche Tecniche**                                        | **Metodo di Ridondanza**                        | **Vantaggi**                                                                                     | **Svantaggi**                                                                   | **Tolleranza ai Guasti**                                                      | **Fonte** |
| ---------------- | ------------------------------------------------------------------- | ----------------------------------------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | --------- |
| RAID 0           | Sezionamento a livello di blocco (striping)                         | Nessuno                                         | Economico, alte prestazioni in lettura/scrittura grazie al parallelismo                          | Affidabilità ridotta (cala all'aumentare dei dischi)                            | Nessuna                                                                       | [1]       |
| RAID 1           | Copia speculare (mirroring) senza sezionamento                      | Mirroring (duplicazione dei dati)               | Alta affidabilità (aumenta linearmente con le copie), migliori prestazioni in lettura            | Alto costo, bassa scalabilità, velocità di scrittura può dimezzarsi             | Guasto di un disco (i dati si perdono solo se si guastano entrambi)           | [1]       |
| RAID 2           | Sezionamento a livello di bit con codici di correzione errori (ECC) | Codici Hamming                                  | Migliore affidabilità rispetto a RAID 0, richiede meno dischi di RAID 1 (es. 3 extra per 4 dati) | Costoso                                                                         | Rileva e corregge errori su singolo bit tramite ECC                           | [1]       |
| RAID 3           | Sezionamento a livello di byte con disco di parità dedicato         | Bit di parità su disco dedicato                 | Velocità di trasferimento n volte RAID 1, più efficiente di RAID 2                               | Meno operazioni I/O al secondo, scritture lente (calcolo parità)                | Ricostruzione del bit mancante tramite calcolo parità degli altri dischi      | [1]       |
| RAID 4           | Sezionamento a livello di blocco con disco di parità dedicato       | Blocco di parità su disco separato              | Tolleranza ai guasti, letture veloci (parallelismo)                                              | Disco di parità come collo di bottiglia, scritture lente                        | Sopravvive al guasto di un singolo disco                                      | [1]       |
| RAID 5           | Sezionamento a livello di blocco con parità distribuita             | Blocchi intercalati a parità distribuita        | Elimina il collo di bottiglia del disco di parità, letture e scritture contemporanee             | Scritture lente (calcolo parità)                                                | Sopravvive al guasto di un disco (parità distribuita)                         | [1]       |
| RAID 6           | Sezionamento a livello di blocco con doppia parità distribuita      | Codici Reed-Solomon (doppia parità)             | Altissima ridondanza                                                                             | Molto costoso, scritture molto lente                                            | Tolleranza al guasto simultaneo di 2 dischi                                   | [1]       |
| RAID 0+1         | Combinazione di striping (0) e mirroring (1)                        | Mirroring di stripe (RAID 1 applicato a RAID 0) | Prestazioni migliori di RAID 5, alta affidabilità                                                | Richiede raddoppio dei dischi, non supporta guasti simultanei in stripe diversi | Supporta guasti multipli solo se nello stesso stripe                          | [1]       |
| RAID 1+0         | Combinazione di mirroring (1) e striping (0)                        | Stripe di mirror (RAID 0 applicato a RAID 1)    | Più robusto di RAID 0+1                                                                          | Costoso                                                                         | Sopravvive al guasto di più dischi purché non appartengano allo stesso mirror |           |
