---
Date created: 30-04-26 • 09:48
tags:
  - Sistemi-Operativi
Related PDF/DOC:
  - "[[05-IPC.pdf]]"
Related Pages:
---
## Relazione tra processi
I processi possono avere diversi rapporti tra loro:
- <mark class="hltr-blue">Processi Indipendenti</mark>
- <mark class="hltr-purple">Processi Comunicanti</mark>

### Processi indipendenti
>Non influenza, né viene influenzato da altri processi

Essenzialmente un processo indipendente non condivide alcun dato con altri processi.

I processi indipendenti sono **deterministici e riproducibili**, in quanto l'output dipende solo dall'input.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Processi comunicanti
>Influenza e può essere influenzato da altri processi

Al contrario dei processi indipendenti l'**output non è deterministico**.

I vantaggi di un processo comunicante sono legati alla condivisione di informazioni tra processi :
- **Accelerazione** del calcolo attraverso l'esecuzione parallela di <mark class="hltr-orange">subtask</mark>.
- **Modularità** e divisione dei compiti tra processi.


> [!example] Schema dei modelli di comunicazione tra processi
> ![[EMBED/05-IPC.png]]
>
[[05-IPC.pdf#page=4&rect=48,40,699,456|05-IPC, p.4]]



---


## Message passing
>Meccanismi utilizzati dai processi per comunicare e sincronizzare le loro azioni.

I processi non condividono le variabili per comunicare, invece scambiano dati attraverso **messaggi**. 


> [!info]- Lunghezza messaggi
> La lunghezza del messaggio può essere fissa o variabile.


Le IPC forniscono due operazioni per la comunicazione:
- `send` 
- `receive`
 
Prima che avvenga lo scambio di messaggi è necessario stabilire un canale di comunicazione tra i due processi; un canale può essere ...
- <mark class="hltr-purple">Fisico</mark> (*eg. shared memory, hardware bus*)
- <mark class="hltr-blue">Logico</mark> (*eg. proprieta logiche*)


> [!info]- FAQ propietà dei canali IPC - (*ChatGPT*)
> > Come vengono stabiliti i canali?
> 
> Dipende dal tipo di IPC:
> - **Implicitamente dal sistema operativo**: ad esempio con le _pipe_ create tra processi padre-figlio.
> - **Esplicitamente dai processi**: ad esempio creando socket, code di messaggi o segmenti di memoria condivisa tramite chiamate di sistema.
> - Possono essere identificati tramite **handle, file descriptor o nomi** (es. named pipe).
>
> ---
>
> > Può un canale essere associato a più processi?  
> 
> Sì, ma dipende:
> 
> Alcuni meccanismi sono **1 a 1** (es. pipe anonime classiche).
> 
> Altri supportano **1 a molti o molti a molti** (es. message queue, socket server con più client, memoria condivisa).
>
> ---
>
> > Quanti canali ci possono essere per ogni coppia di processi comunicanti?
>
> In generale **uno o più canali**.
> 
> Ad esempio:
> - Una sola pipe bidirezionale (se supportata).
> - Due canali separati (uno per ogni direzione).
> - Più canali per separare diversi tipi di dati.
>   
>   ---
>   
> >Qual è la capacità di un canale?
>
> Può essere:
> - **Limitata (buffer finito)** → il caso più comune (pipe, socket buffer, code di messaggi).
> - **Illimitata (concettualmente)** → in teoria, ma nella pratica sempre limitata dalla memoria.
>   
>  Quando il buffer è pieno Il mittente può **bloccarsi** oppure ricevere un errore (modalità non bloccante).
>  
>  ---
>  
> >  La lunghezza dei messaggi è fissa o variabile?
> 
> Dipende dal meccanismo:
> - **Fissa** → raro (alcuni sistemi embedded o protocolli specifici).
> - **Variabile** → comune (message queue, socket).
>  - Nei flussi (pipe, socket TCP) non esiste il concetto di “messaggio”, ma solo **stream di byte**.
> 
>  ---
>  
>  > Il canale è unidirezionale o bidirezionale?
>  
>  Può essere:
>  - **Unidirezionale (half-duplex)** → es. pipe tradizionali.
>  - **Bidirezionale (full-duplex)** → es. socket.
>
> Spesso la bidirezionalità si ottiene con **due canali unidirezionali**.

### Comunicazione diretta e indiretta
#### Comunicazioni dirette
> I processi devono nominarsi **esplicitamente**.

I processi, all'interno del codice devono chiamarsi <mark class="hltr-orange">esplicitamente per nome</mark>.


> [!info] Comunicazione diretta Simmetrica e Asimmetrica
> La differenza tra i due tipi sta nel modo in cui il destinatario riceve il messaggio:
> 
> - <mark class="hltr-blue">Simmetrica</mark>  : `send (P1, message) | receive (P2, message)`
> - <mark class="hltr-purple">Asimmetrica</mark> : `send (P1, message) | receive (id, message)`
>   
>  (*Asimmetrica riceve messaggi da tutti e in id si trova il nome del processo che ha eseguito send*)

Lo svantaggio di questo metodo sta nel fatto che <mark class="hltr-red">se un processo cambia nome devo ri-codificare gli altri</mark>.


<hr style="width: 70%; margin-left: auto;margin-right: auto;">


#### Comunicazioni indirette
>I messaggi sono spediti e ricevuti da mailboxes (*porte*).

Ogni mailbox ha un unico **id**, e i <mark class="hltr-orange">processi possono comunicare solo se condividono una mailbox</mark>.

Prima di iniziare la comunicazione deve essere creata una mailbox, che viene rimossa a comunicazione terminata; tuttavia una mailbox <mark class="hltr-red">NON E' UN CANALE</mark>.

<mark class="hltr-orange">Il canale deve e può essere stabilito solo se esiste una mailbox comune.</mark>

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

La comunicazione indiretta tuttavia crea dei problemi di organizzazione; prendiamo l'esempio ...

> Se P1 manda un messaggio e P2,P3 ricevono; chi ottiene prima il messaggio?

Possiamo avere diverse soluzioni :
- Permettere solo canali con un mittente e un ricevente.
- Permettere a solo un processo alla volta di eseguire il receive.
- Permettere al sistema di selezionare in modo arbitrario il ricevente, e notificare il mittente su chi ha ricevuto il messaggio.

---
### Sincronizzazione dei messaggi
Per sincronizzare mittente e ricevente si hanno due tipi di scambio di messaggi ...
- <mark class="hltr-blue">Bloccante</mark> (*sincrono*)
- <mark class="hltr-purple">Non-Bloccante</mark> (*asincrono*)


> [!example] Esempi di comunicazioni sincrone e asincrone
> > SEND
> 
> ![[EMBED/05-IPC 1.png]]
>
[[05-IPC.pdf#page=16&rect=34,102,691,421|05-IPC, p.16]]
>
> ---
> > RECEIVE 
> 
> ![[EMBED/05-IPC 2.png]]
>
[[05-IPC.pdf#page=17&rect=42,118,687,435|05-IPC, p.17]]

#### Comunicazione bloccante o sincrono
Il `send` **bloccante** ferma il mittente dal procedere con l'esecuzione finchè il messaggio non viene ricevuto.

Il `receive` **bloccante** ferma il destinatario dal procedere con l'esecuzione finchè non viene ricevuto un messaggio attraverso il canale.

Il ciclo di invio e ricevimento di una comunicazione sincrona è detta *rendezvous*.


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Comunicazione non-bloccante o asincrono
Una comunicazione asincrona semplicemente non ferma nessuna esecuzione...

Il `send` del mittente spedisce un messaggio e procede con l'esecuzione.

Il `receive` del destinatario prova a ricevere un messaggio che può risultare in un messaggio valido o nulla. 


---


## Memoria condivisa
### Condivisione di segmenti di memoria
I processi possono esplicitamente creare segmenti di memoria condivisa che poi possono essere implementati da altri attraverso un id.

Finita la comunicazione i processi collegati si *"staccano"* dal segmento, che poi viene rimosso.

> [!example]- Codice per condivisione di memoria (POSIX)
> ```c
> //Create
> segment id = shmget(IPC_PRIVATE, size, S_IRUSR | S_IWUSR);
> 
> //Attach
> shared memory = (char *) shmat(id, NULL, 0);
> 
> //Write
> sprintf(shared memory, "Writing to shared memory");
> 
> //Detach
> shmdt(shared memory);
> 
> //Remove/Drop
> shmctl(shm_id, IPC_RMID, NULL);
> 
> ```


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Pipe
> *Condotti* che permettono a **2 soli** processi di comunicare.

#### Pipe ordinarie
Normalmente le Pipe permettono solo la comunicazione in stile <mark class="hltr-orange">unidirezionale</mark> (*produttore-consumatore*). 

Le pipe ordinarie inoltre richiedono una relazione <mark class="hltr-orange">parent-child</mark> tra i due processi.


> [!example] Schema di una pipe
> ![[EMBED/05-IPC 3.png]]
>
[[05-IPC.pdf#page=23&rect=56,188,669,391|05-IPC, p.23]]

#### Pipe nominate
> Nelle pipe con nome **le comunicazioni sono bidirezionali**.

Le pipe con nome rimuovono le limitazioni dalle pipe ordinarie:
- Relazione <mark class="hltr-orange">parent-child non necessaria</mark>.
- <mark class="hltr-orange">Non unidirezionali</mark>.
- <mark class="hltr-orange">Più di 2 processi possono comunicare</mark>.