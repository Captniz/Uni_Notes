---
Date created: 08-05-26 • 08:54
tags:
  - Sistemi-Operativi
Related PDF/DOC:
  - "[[08-Sincronizzazione.pdf]]"
Related Pages:
---
## Modello Produttore-Consumatore

> Il modello base della comunicazione tra processi è il *produttore-consumatore* : un processo manda messaggi e l'altro li riceve.

L'esecuzione concorrente dei due processi è permessa da un buffer (*queue*) che contiene i messaggi.

Ovviamente però, per una corretta comunicazione, si devono <mark class="hltr-orange">sincronizzare le tempistiche</mark> di invio e ricevimento dei messaggi.

### Buffer di tipo P/C
> Il buffer nel modello P/C ha una struttura *circolare*.


> [!example] Implementazione del codice del buffer (C++)
> ![[EMBED/08-Sincronizzazione.png]]
>
[[08-Sincronizzazione.pdf#page=5&rect=34,77,693,424|08-Sincronizzazione, p.5]]
>
> ---
>
> Il counter per semplicità di rappresentazione indica gli elementi nel buffer :
> - `ctr = 0` : Vuoto
> - `ctr = N` : Pieno

Il codice del buffer introduce un problema fondamentale della sincronizzazione tra processi : <mark class="hltr-red">Le rush conditions causate dall'accesso simultaneo agli stessi dati</mark>.


> [!example] Rush condition tra P/C sul counter
> ![[EMBED/08-Sincronizzazione 1.png]]
>
[[08-Sincronizzazione.pdf#page=7&rect=28,229,688,400|08-Sincronizzazione, p.7]]

Dobbiamo quindi introdurre il concetto di <mark class="hltr-orange">sezione critica del codice</mark> ...

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Sezione critica
> Porzione di codice in cui si accede ad una risorsa condivisa.

Questa sezione rispetta tre criteri fondamentali :
- **Mutua esclusione** : Solo un processo alla volta può accedere alla sezione critica.
- **Progress** : Solo i processi che *stanno per entrare* nella sezione critica possono decidere chi entra.
- **Bounded Waiting** : Esiste un massimo di volte per cui un processo può aspettare (*consecutive*).

/call