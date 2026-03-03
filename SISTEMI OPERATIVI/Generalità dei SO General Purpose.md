---
Date created: 27-02-26 • 09:13
tags:
  - Sistemi-Operativi
Related PDF/DOC:
  - "[[03-Componenti.pdf]]"
Related Pages:
---
## Gestione dei processi

> [!info] Processi
> Programma in esecuzione

Il s
## Memoria primaria




----


## Shell
In unix a differenza degli altri SO vecchi la shell si occupa solo dell'interpretazione del comando utente e chiama un riferimento a un programma esterno. Negli altri SO la shell stessa contiene l'interpretazione del comando

- UNIX : rm - -> shell - -> rm.sh - -> exec 
- OTHERS : rm - -> shell - -> exec 

<mark class="hltr-red">LA SHELL NON E' IL TERMINALE, E' SOLO L'INTEPRETE DEI COMANDI</mark>

I programmi di batch sono i programmi che contengono script per la shell
(.bat Win / .sh Unix)


La shell è per gli utenti
## System call per programmi

I programmi non usano le syscall pk sono pesanti (molti parametri), piuttosto vengono usate le API di sistema 