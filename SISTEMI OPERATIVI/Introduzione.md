---
Date created: 26-02-26 • 09:03
tags:
  - Sistemi-Operativi
Related PDF/DOC:
  - "[[02-Definizione_e_storia.pdf]]"
Related Pages:
---
> Un **sistema operativo** e' un insieme di programmi che agisce come intermediario tra HW e uomo: Facilita l’uso del computer, rende efficiente l’uso dell’HW ed evita conflitti nella allocazione delle risorse HW/SW.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Il sistema operativo è il primo processo avviato all'accensione della macchina ed è <mark class="hltr-red">sempre</mark> in memoria centrale. Inoltre ha sempre il <mark class="hltr-orange">controllo</mark>; talvolta può cederlo ad altri programmi ma può anche sempre riprenderselo.

L'OS è un programma *"event driven"* cioè comunica e viene controllato/controlla attraverso **eventi**.

Gli OS possono essere di due tipi : 
- <mark class="hltr-blue">General Puprose</mark>
- Special Purpose

Nell'SO sono anche inclusi dei programmi di base, detti *core*.


---
%%Interrupt / DMA / IO%%

---

### Buffering e spooling
Il **buffering** avviene quando si devono interfacciare due componenti/periferiche con velocità/tempismi diversi.

In pratica il buffering mette a disposizione un buffer intermedio tra CPU e periferiche che accumula un numero di input dalla periferica per poi passarla in blocco alla CPU.

---

Lo **spooling** invece permette di avere operazioni diverse simultanee sullo stesso dispositivo


---
%%Multiprog / Time sharing%%

---
### Protezione dei dati
Dopo l'introduzione dei SO multiutente è diventato necessario isolare e proteggere gli utenti e i loro dati dalle azioni degli altri.

Inoltre i sistemi multi-programma introducevano altri problemi di sicurezza : 
- Cicli infiniti prevengono l’uso della CPU di altri programmi.
- Un programma può leggere più dati del dovuto, a cui magari non ha accesso.
- ...

Esistono tre tipi fondamentali di protezione per i sistemi multi-programma...

#### Protezione IO
Per prevenire l'uso improprio di periferiche il SO introduce due metodi diversi di esecuzione dei programmi :
- <mark class="hltr-blue">USER Mode</mark> : I processi <mark class="hltr-red">NON</mark> possono accedere direttamente alle risorse IO.
- <mark class="hltr-orange">KERNEL (o SUPERVISOR) Mode</mark> : l sistema operativo può accedere alle risorse di IO.



Tutte le operazioni di I/O sono *privilegiate*, cioè devono essere autorizzate dal SO. Questo è il flow dell'accesso all'IO da parte di programmi in user mode :
-  Istruzioni per accesso ad I/O invocano delle ***system call***.
> [!info] System call
> Interrupt software che cambia la modalità di esecuzione da USER a SUPERVISOR.
> 
- Al termine della system call il S.O. ripristina la modalità USER

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Protezione della Memoria
La protezione dello spazio dei vari processi, cioè delle aree di memoria riservate a ogni programma.

Inoltre si occupa anche della ***Protezione del monitor*** (Protezione della memoria del Sistema operativo).

Questa viene realizzata associando dei <mark class="hltr-orange">registri limite</mark> ad ogni processo; questi registri possono essere <mark class="hltr-red">modificati solo dal S.O.</mark> con istruzioni privilegiate.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Protezione della CPU
Cioè la garanzia che il <mark class="hltr-red">S.O. mantenga il controllo del sistema</mark>. 

Viene realizzata tramite <mark class="hltr-orange">timer</mark> di scadenza (*necessità di passare il controlla al SO*), associato ad ogni processo.

Ovviamente se il processo si rifiuta di ripassare il controllo al SO si hanno dei modi per forzare il processo.


----


%%Il resto%%