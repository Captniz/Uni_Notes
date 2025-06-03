---
Date created: 13-04-25 • 22:36
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250325_L9-RISC-V_2.pdf]]"
Related Pages:
  - "[[Operazioni in RISC-V]]"
  - "[[Architettura RISC-V]]"
---
## Protocollo delle procedure in RISC-V
La cosa importante in Assembly è definire chiaramente un **protocollo** ( *o convenzione* ) di chiamata alle procedure.

Il protocollo per la chiamata di procedure in Assembly è strutturato secondo questi passi:

1. Caricare i *parametri* di input della procedura in **posti noti a priori**. 
2. Trasferire il **controllo alla procedura**. 
	1. Acquisire le risorse necessarie.
	2. Eseguire il compito.
	3. **Caricare i valori di ritorno** in posti noti. 
	4. **Restituire il controllo** al chiamante ( _Attraverso l'[[#^80cd2d|RA]]_ ).
3. Prendere il valore di ritorno della procedura e pulire i registri.

Per quanto riguarda **RISC-V**, tiene fede all'idea di cercare di usare i registri il più possibile per passare i parametri.

I registri quindi sono divisi in questo modo:

| Registro          | Compito                                                              |
| ----------------- | -------------------------------------------------------------------- |
| `x10 -> x17`      | Parametri in ingresso e valori di ritorno                            |
| `x1`              | Indirizzo di ritorno ( **_[[#^80cd2d\|RA]]_** ) al punto di chiamata |
| `x5-x7 / x28-x31` | Registri temporanei, non salvati in caso di chiamata a procedura     |
| `x8-x9 / x18-x27` | Registri da salvare in caso di chiamata a procedura                  |

> [!info]- Registri Temporanei / Da salvare
> <mark class="hltr-orange">Registri Temporanei</mark> : 
> - **Registri:** `t0–t6`, `a0–a7`, `ra`
> - **Chi li salva?** : Il chiamante ( `caller` )
> - **Quando?** : Se **il chiamante** ha bisogno di riusarli dopo la funzione chiamata. Altrimenti possono essere sovrascritti liberamente dalla funzione
>   
> <mark class="hltr-purple">Registri Salvati</mark> :
> - **Registri** : `s0–s11`    
> - **Chi li salva?** : La funzione chiamata (`callee`)
> - **Quando?** : Se la funzione li usa/modifica, <mark class="hltr-red">deve</mark> **salvarli su stack** all'inizio ( **[[#Prologo|Prologo]]** ) e **ripristinarli** alla fine ( **[[#Epilogo|Epilogo]]** ).

^1d2696


Nel caso in cui ...
- I registri per i parametri non bastino. 
- Si vogliano memorizzare delle variabili locali.
- Si voglia mantenere delle variabili memorizzate su indirizzi non mantenuti al ritorno. 

si utilizza lo **Stack**, poi pulendolo al ritorno della procedura.

> [!warning] Stack
> 
Lo Stack è una *struttura dati* gestita in **modalità LIFO** in cui è possibile caricare, a partire da una posizione nota alla procedura ( *Puntata dal registro `x2`, chiamato anche `sp` Stack Pointer* ).
>
> A partire dalla testa dello stack `sp` questo cresce verso il **basso**.

^4376db

> [!warning] Return Address
Il registro di **RA** contiene l'indirizzo dell'<mark class="hltr-orange">istruzione che ha chiamato la procedura</mark> ( *un jump* ) **+4** ( *l'istruzione successiva* ).
>
Questo registro è `x1` e viene usato nelle istruzioni `jal` e `jalr` ( *Jump And Link / Register* ). Queste istruzioni saltano all'indirizzo specificato come **offset + [[Assembly#^99422c|PC]]** e memorizzano il <mark class="hltr-purple">RA</mark> nel registro `x1`.
>
>>[!example]- Esempio
>>Questa istruzione salta a PC+100 e mette l'indirzzo della `jal` $+ 4$ in `x1` :  
>>```asm
>>jal x1, 100
>>```
>>Questa istruzione fa lo stesso ma con al posto del PC il registro `x5` :
>>```asm
>>jalr x1, (x5)100
>>```
>
Una volta <mark class="hltr-red">terminata</mark> la procedura si esegue un salto attraverso l'istruzione `jalr x0, 0(x1)` per tornare all'indirizzo di chiamata.

^2b9569

### Prologo ed epilogo
Il <mark class="hltr-orange">prologo</mark> è una **sequenza di istruzioni all'inizio di una funzione** che prepara lo stack e i registri per l'esecuzione corretta della funzione.

L’<mark class="hltr-purple">epilogo</mark> è la **parte finale di una funzione** in linguaggio assembly e, in parole povere, il “contrario” del **prologo**. 

Prendiamo per esempio questo codice ...
```c
long long int esempio_foglia(long long int g, long long int h, long long int i, long long int j) { 
	long long int f; 
	f = (g+h) – (i+j); 
	return f; 
}
```
Lo terremo conto per la successiva spiegazione delle funzioni di queste due sequenze di istruzioni.
#### Prologo
Le funzioni del prologo sono:
- Salvare i registri che verranno modificati ( *Per rispettare le convenzioni di chiamata* ).
- Allocare spazio sullo stack per variabili locali.
- Impostare il **frame pointer**.
- Salvare il **return address**.

> [!info]- Chiamata di una funzione
> Alla chiamata di una funzione, per prima cosa il compilatore sceglie un’**etichetta** associata all’ indirizzo di entrata della procedura ( _Nel nostro caso, **esempio_foglia**_ ).
>
In fase di *collegamento/linking* l’etichetta sarà collegata a un’indirizzo.

Nella fase di Prologo si salva in memoria tutti i registri che la procedura usa, in modo da poterli ripristinare in seguito. 

In questo caso si tratta di `x20 = f`, `x5 = g+h`, `x6 = i+j`.
1. Decrementiamo `sp` di 24, per far posto a 3 *double word*.
2. Salvataggio di `x5` in `sp+16`
3. Salvataggio di `x6` in `sp+8`
4. Salvataggio di `x20` in `sp+0`


> [!error] Inutilità della memorizzazione
> Come abbiamo visto prima, `x5`e `x6` sono registri **[[#^1d2696|Temporanei]], quindi non ha senso salvarli prima nel prologo.**


```c
esempio_foglia: addi sp, sp, -24 
				sd x5, 16(sp) 
				sd x6, 8(sp) 
				sd x20, 0(sp)
```

#### Epilogo
Le funzioni dell'epilogo sono :
- **Ripristinare i registri salvati** (`s0-s11`, `ra`, ecc.).
- **Deallocare** lo stack frame della funzione.
- **Ripristinare il puntatore dello stack** (`sp`).
- **Eseguire il ritorno** alla funzione chiamante (usando `ret` o `jalr x0, ra, 0`).

Dopo aver eseguito la procedura ( *L'obiettivo della funzione* ), si ripristinano i registri usati e si setta il valore di ritorno attraverso l'epilogo:
1. Trasferisco `x20` in `x10` ( *Valore di ritorno, [[#^2b9569|Return Address]]* ).
2. Ripristino i registri prelevando i valori esattamente da dove li avevo messi.
3. Ripristino lo [[#^4376db|stack]] ( *Tutto ciò che è sotto `sp` non è significativo* ).
4. Salto al punto da dove sono arrivato.

```c
addi x10, x20, 0 
ld x20, 0(sp) 
ld x6, 8(sp) 
ld x5, 16(sp) 
addi sp, sp, 24 
jalr x0, 0(x1)
```

%%pp20 l 9%%