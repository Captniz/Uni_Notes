---
Date created: 12-03-25 • 08:41
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250312_L6-Assembly.pdf]]"
Related Pages:
---
# Linguaggio macchina
>Le **CPU** capiscono ed eseguono **solo** il linguaggio macchina specifico per la loro architettura.

Il linguaggio macchina è una **sequenza di 0 e 1**, rendendolo molto difficile da manipolare per un umano. Il programmatore, a sua volta, scrive principalmente con linguaggi di alto livello ( *C++, Java, Python* ) incomprensibili per la macchina.

Il linguaggio **Assembly** ha proprio il compito di agire da ponte tra queste due realtà.
## Assembly
> Linguaggio di bassissimo livello, che permette di scrivere programmi molto vicini all' linguaggio macchina agli umani.

Le caratteristiche principali dell'assembly sono:
- Codici mnemonici ( *Leggibili e memorizzabili da un umano* )
- Stretta corrispondenza tra **linguaggio macchina** e assembly
- Ogni riga/istruzione in assembly corrisponde un istruzione in L.M. ( *ci sono eccezioni* )

Assembly deve comunque essere **compilato** e tradotto in linguaggio macchina per essere interpretabile dalla macchina; questa compilazione è compito dell' **Assembler** ( *Compilatore per Assembly* ).

Il linguaggio Assembly non è globale: **ogni CPU necessita delle proprie istruzioni** e di conseguenza del proprio linguaggio Assembly. L'insieme di queste istruzioni riconosciute dalla CPU si chiama **ISA** ( *Instruction Set Architecure* ).

L' ISA *non* è una semplice lista di istruzioni, contiene:
- Sintassi e semantica
- Accesso ai dati
	- **Registri** della CPU
	- Modalità di accesso alla memoria e **indirizzamento**


> [!info] Diverse ISA per diverse CPU
> Un ISA può definire anche più linguaggi assembly ( *Intel 32/64 bit* ) o più ISA possono essere presenti in una singola CPU ( *Casi rari* ).

### Struttura di un programma Assembly
La struttura di un programma Assembly deriva direttamente dalla struttura della CPU corrispondente.

Durante l'esecuzione di un istruzione avvengono tre operazioni:

`fetch`->`decode`->`execute`

| Categoria |                                                        Descrizione                                                        |
| :-------: | :-----------------------------------------------------------------------------------------------------------------------: |
|  `Fetch`  | Preleva un istruzione dalla memoria da un registro apposito <br>( **Program conunter "PC" / Instruction counter "IC" ** ) |
| `Decode`  |                                   Decodifica l'istruzione per derivarne il significato                                    |
| `Execute` |                                                    Esegue l'operazione                                                    |

Queste istruzioni all'interno di un programma in assembly, di norma sono eseguite in ordine sequenziale; tuttavia esistono istruzioni, dette **salti ( jump )** che permettono di saltare ad altre istruzioni.

> [!warning] Cicli
> In Assembly non esistono cicli come `for` o `while`, questi sono sostituiti da diversi tipi di `jump` con condizioni.

--- 

I tipi di dati su cui operano le istruzioni dell' Assembly sono principalmente:
- Operandi **logici o aritmetici**
- Dati da muovere
- Indirizzi di memoria
- Sostituzione di valori all'interno dell' **PC e IC**

Questi dati sono **Immediati**, **costanti** e **contenuti in registri o in memoria**.

> [!info] Registri
> I registri sono il dispositivo di memoria più **piccolo** all'interno di una macchina; allo stesso tempo tuttavia sono i più **veloci**, dal momento che sono fisicamente contenuti **all' interno della CPU**.
> 
> Fisicamente sono delle batterie ( *nel senso di serie* ) di **Flip-Flop tipo D**.

Il banco di registri a disposizione dell' Assembly, **general-purpose o specializzati**, è definito dall' ISA e generalmente se ne mettono a disposizione un numero compreso tra `4` e `64` e il loro nome viene deciso dall' ISA.

%%TODO PP.5 PDF. L6%%