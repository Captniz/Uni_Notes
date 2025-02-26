---
Date created: 26-02-25 • 08:37
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250226_CalcLect2-3-AritmeticaCalcolatori.pdf]]"
Related Pages:
---
## Trattamento dei dati su un calcolatore

>  Un computer e un *insieme di circuiti elettronici*

Un computer memorizza e manipola solo sequenze di 0 e 1, di cui la singola cifra è detta **Bit**, mentre una sequenza di 8 è detta **Byte**.

Queste sequenze devono poter rappresentare **tutti** i tipi di informazione che un computer gestisce:

- Valori numerici 
- Caratteri o simboli 
- Immagini
- Suoni
- Programmi

Per far si che una sequenza di bit possa essere interpretata come informazione utile, deve essere stabilita una **codifica** (rappresentazione digitale).

> [!note] Codifica
> **Funzione che associa ad un oggetto/simbolo una sequenza di bit.**
>  
> **Codifica su n bit**: associa una sequenza di n bit ad ogni entità da codificare ( *Permette di codificare $2^{n}$ entità / simboli distinti* ).
> 
> Esempio: 
> Per codificare i semi delle carte bastano 2 bit
>  
> • Picche → `00` 
> • Quadri → `01`
> • Fiori → `10` 
> • Cuori → `11`

Codifica e decodifica: [[Aritmetica dei calcolatori#Codifica e decodifica]]

Una volta riportato il dato in **binario** per ridurre il numero di cifre e di conseguenza renderlo più facilmente leggibile si può usare la base **esadecimale per rappresentare un byte con due cifre**.

Esempio:
$(1110\ 1000)_{2}=(E8)_{16}$
