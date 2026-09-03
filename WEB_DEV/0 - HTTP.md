---
Date created: 01-09-26 • 16:13
tags:
  - Web
Related PDF/DOC:
  - "[[0 - HTTP]]"
Related Pages:
---
# Il protocollo HTTP
> The Hypertext Transfer Protocol (HTTP) is an *application level protocol* for distributed, collaborative, hypermedia information systems. It is a generic, *stateless* protocol [...]


HTTP è :
- Stateless
- Generico
- Asimmetrico (*differenze server-client*)
- Basato su request-response
- Basato su TCP

Un client manda una richiesta al server che ritorna un messaggio di risposta conenente un qualche tipo di informazione.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

HTTP usa una connessione TCP su porta `80` per la comunicazione. Una volta chiusa la connessione il server <mark class="hltr-red">NON MANTIENE ALCUNA INFORMAZIONE SULLE COMUNICAZIONI PASSATE COL CLIENT</mark>, questo è il significato di **stateless**.

Un protocollo *stateful* è complicato poichè ...
- Richiede la memorizzazione dello stato.
- Richiede dei meccanismi di risincronizzazione dello stato in caso di crash della connessione.

Dopo che si è stabilita una connessione TCP (*grazie agli ultimi protocolli HTTP*) è permesso eseguire diverse richieste HTTP attraverso una singola connessione. Le richieste possono avvenire anche in contemporanea l'una con l'altra (*non si attendono le risposte*) attraverso un processo detto **pipelining**.
## Pagine web - HTTP + HTML
Una pagina web è composta da una pagina base HTML e da vari altri oggetti referenziati.

Ogni oggetto (*e pagina*) è descritta da un URL (*universal resource locator*) che permette di eseguire una richiesta HTTP per tale oggeto.

### SSL
Secure Socket Layer è un protocollo gemello ad HTTP che permette il trasferimento sicuro (*criptazione*) dei dati nel body su HTTP

## Anatomia dei messaggi HTTP
### Richieste
#### Formato

> [!example] Schema generale di una richiesta HTTP 
> ![[EMBED/0-HTTP.png]]
> [[0-HTTP.pdf#page=10&rect=151,463,435,664|0-HTTP, p.10]]


> [!example] Schema specifico di una richiesta HTTP 
> ![[EMBED/0-HTTP 1.png]]
> [[0-HTTP.pdf#page=10&rect=158,172,441,337|0-HTTP, p.10]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Metodi

| TYPE       | FUNCTION                                                                                                                                                                                                                    |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| HEAD       | Recupera solo gli header di una data risorsa. Utile per analitiche o manutenzione                                                                                                                                           |
| GET        | Recupera una risorsa. Supporta argomenti della query **passati attraverso l'url**.                                                                                                                                          |
| POST       | Recupera una risorsa o passa dei dati. A differenza di `GET` può passare molti più argomenti; inoltre questi vengono inclusi nella richiesta **nel body** e non nell'url. Questo permette un livello maggiore di sicurezza. |
| OPTIONS    | Ritorna i metodi supportati dal server.                                                                                                                                                                                     |
| TRACE      | Ritorna un trace diagnostico delle azioni del server.                                                                                                                                                                       |
| CONNECT    | Permette il tunneling di altri protocolli attraverso HTTP.                                                                                                                                                                  |
| PUT,DELETE | Usati per il publishing HTTP.                                                                                                                                                                                               |
Questi metodi vengono detti <mark class="hltr-orange">idempotenti</mark>, poichè eseguire la stessa richiesta più volte senza cambiare l'effetto e lasciando il server nel medesimo stato (*non ha side effects*).


> [!example]- Esempio di metodo idempotente
> Sia una richiesta relativa ad un database `DELETE ELEMENT 1`, che cancella il primo oggetto del database.
> 
> Un implementazione idempotente cancellerebbe il record con $id=1$ o simile, e non sarebbe permesso aggiungere record con $id=1$.
> 
> Un implementazione <mark class="hltr-orange">non idempotente</mark> cancellerebbe il primo record trovato ogni volta.
> 

Invece alcuni metodi vengono detti <mark class="hltr-purple">safe</mark> se non alterano lo stato del server, solitamente operazioni *read-only*.

### Risposte
#### Formato

> [!example] Esempio di risposta HTTP
> ![[EMBED/0-HTTP 2.png]]
> [[0-HTTP.pdf#page=16&rect=123,163,447,342|0-HTTP, p.16]]

#### Codici di risposta

| CODICE                         | SIGNIFICATO                                          |
| ------------------------------ | ---------------------------------------------------- |
| 200 OK                         | Successo.                                            |
| 301 Moved Permanently  <br>    | Oggetto richiesto è stato mosso. Segue il nuovo URL. |
| 400 Bad Request                | Messaggio di richiesta non capito dal server.        |
| 404 Not Found                  | Oggetto richiesto non trovato                        |
| 505 Http Version Not Supported | Auto-esplicativo                                     |
### Headers
Esistono 4 tipi di header:
- **Generali** : Informazioni sui messaggi
- **Request** : Informazioni specifiche per le richieste
- **Response** : Informazioni specifiche per le risposte
- **Entity** : Informazioni sugli oggetti

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


Header generali :

| HEADER        | FUNZIONE                                                |
| ------------- | ------------------------------------------------------- |
| Connection    | Utili per gestire la connessione (*keep-alive, close*). |
| Date          | Data del messaggio.                                     |
| Via           | Informazioni sui proxy attraversati dal messaggio.      |
| Cache-Control | Direttive per il caching dei messaggi.                  |

Header request :


| HEADER              | FUNZIONE                                                         |
| ------------------- | ---------------------------------------------------------------- |
| Host                | Hostname e porta del server destinazione.                        |
| Referer             | URL della risorsa da cui proviene l'URI.                         |
| User-Agent          | Info sull'app che ha inviato la richiesta.                       |
| Accept `& varianti` | Info sulle capacità e preferenze del client per la risposta.     |
| Cookie              | Campo dove il client passa al server le informazioni sui cookie. |

Header response :


| HEADER     | FUNZIONE                                                     |
| ---------- | ------------------------------------------------------------ |
| Server     | Nome e versione del server                                   |
| Set-Cookie | Campo dove il server passa informazioni sui cookie al client |

Header entity :


| HEADER           | FUNZIONE                                                             |
| ---------------- | -------------------------------------------------------------------- |
| Allow            | Lista dei metodi disponibili per l'entità                            |
| Location         | Informazioni sulla location dell'entità                              |
| Content-Encoding | Specifica la codifica del body del messaggio (*spesso compressione*) |
| Content-Length   | Dimensione in byte dell'entità                                       |
| Content-Type     | Tipo del media dell'entità (*MIME type*)                             |
