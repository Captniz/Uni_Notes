---
Date created: 02-05-26 • 19:03
tags:
Related PDF/DOC:
  - "[[06-Scheduling_CPU v0.pdf]]"
Related Pages:
---
> Lo scheduling è definito come l'assegnazione di attività nel tempo (*riferito ai processi*).

La **multiprogrammazione** impone l’esistenza di una strategia per regolamentare:
1. Ammissione dei processi nella <mark class="hltr-orange">memoria</mark> 
2. Ammissione dei processi all’esecuzione (<mark class="hltr-purple">CPU</mark>)


> [!example] Schema del ciclo di esecuzione di un processo
> ![[EMBED/06-Scheduling_CPU v0 1.png]]
>
[[06-Scheduling_CPU v0.pdf#page=5&rect=51,59,659,422|06-Scheduling_CPU v0, p.5]]

---

## TIpi di scheduler


> [!example] Schema della differenza tra long-term e short-term scheduler
> ![[EMBED/06-Scheduling_CPU v0 2.png]]
>
[[06-Scheduling_CPU v0.pdf#page=8&rect=140,23,661,189|06-Scheduling_CPU v0, p.8]]

### Scheduler a breve termine (CPU Scheduler)
> Seleziona dalla memoria quale processo deve essere eseguito dalla *CPU*.

Dato che viene invocato spesso esegue le sue operazioni in tempo
$O(ms)$.
> [!example]- Calcolo del tempo utilizzato dallo scheduler CPU
>
> *Es.* su 100ms concessi a ogni processo, 10ms sono necessari per lo scheduling.
> 
> $10/(110) = 9\%$ del tempo di CPU sprecato per lo scheduling.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Dispatcher
> Modulo del SO che passa il controllo della CPU al processo scelto dallo scheduler. 

Il dispatcher esegue il suo compito in tre parti :
1. *Context switch*.
2. Passaggio alla modalità *user*.
3. Salto alla opportuna locazione nel programma per farlo continuare.

La *latenza di dispatch* è il tempo necessario al dispatcher per fermare un processo e farne ripartire un altro.


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Scheduler a lungo termine (JOB Scheduler)

>Seleziona quali processi devono essere portati dalla memoria alla *ready queue*. 

Determina inoltre il grado di multiprogrammazione del SO.

A differenza dello short-term scheduler può essere più lento (*$O(s)$*).

Controlla il mix di processi entranti nell'esecuzione; diversi processi richiedono diverse risorse e pertanto devono essere spartiti correttamente :
- <mark class="hltr-orange">IO Bound</mark> : Molte operazioni IO, molti CPU burst brevi.
- <mark class="hltr-purple">CPU Bound</mark> : Molti calcoli, pochi CPU burst lunghi.

Il long-term scheduler <mark class="hltr-red">PUO' ANCHE ESSERE ASSENTE</mark> (*presente solitamente in sistemi con risorse limitate*). 

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Scheduler a medio termine
> Lo scheduler a medio termine è necessario per la momentanea rimozione forzata (*swapping*) di un processo dalla RAM

<mark class="hltr-red">SOLO</mark> i SO con *memoria virtuale* prevedono un livello intermedio di scheduling, che è necessario per ridurre il grado di multiprogrammazione.


> [!example] Schema dello scheduler a medio termine
> ![[EMBED/06-Scheduling_CPU v0 3.png]]
>
[[06-Scheduling_CPU v0.pdf#page=11&rect=38,74,690,395|06-Scheduling_CPU v0, p.11]]




%%PP 15%%