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
 L’utilità di un computer sarebbe molto limitata se non si potessero chiamare **procedure** ( *funzioni* ). 
 
La cosa importante nell'Assembly è definire chiaramente un **protocollo** ( *o convenzione* ) di chiamata alle procedure.

## Protocollo delle procedure in RISC-V
Il protocollo per la chiamata di procedure in Assembly è strutturato secondo questi passi:

1. Caricare i *parametri* di input della procedura in **posti noti a priori**. 
2. Trasferire il **controllo alla procedura**. 
	1. Acquisire le risorse necessarie.
	2. Eseguire il compito.
	3. **Caricare i valori di ritorno** in posti noti. 
	4. **Restituire il controllo** al chiamante.
3. Prendere il valore di ritorno della procedura e pulire i registri.

Per quanto riguarda **RISC-V**, tiene fede all'idea di cercare di usare i registri il più possibile per passare i parametri.

I registri quindi sono divisi in questo modo:

| Registro          | Compito                                                          |
| ----------------- | ---------------------------------------------------------------- |
| `x10 -> x17`      | Parametri in ingresso e valori di ritorno                        |
| `x1`              | Indirizzo di ritorno ( **_RA_** ) al punto di chiamata           |
| `x5-x7 / x28-x31` | Registri temporanei, non salvati in caso di chiamata a procedura |
| `x8-x9 / x18-x27` | Registri da salvare in caso di chiamata a procedura              |

### RA - Return Address
Il registro di **RA** contiene l'indirizzo dell'<mark class="hltr-orange">istruzione che ha chiamato la procedura</mark> ( *un jump* ) **+4** ( *l'istruzione successiva* ).

Questo registro è `x1` e viene usato nelle istruzioni `jal` e `jalr` ( *Jump And Link / Register* ). Queste istruzioni saltano all'indirizzo specificato come **offset + [[Assembly#^99422c|PC]]** e memorizzano il <mark class="hltr-purple">RA</mark> nel registro `x1`.

>[!example]- Esempio
>Questa istruzione salta a PC+100 e mette l'indirzzo della `jal` $+ 4$ in `x1` :  
>```asm
>jal x1, 100
>```
>Questa istruzione fa lo stesso ma con al posto del PC il registro `x5` :
>```asm
>jalr x1, (x5)100
>```

Una volta <mark class="hltr-red">terminata</mark> la procedura si esegue un salto attraverso l'istruzione `jalr x0, 0(x1)` per tornare all'indirizzo di chiamata.

%%pp 6%%