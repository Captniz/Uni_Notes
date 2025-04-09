---
Date created: 02-04-25 • 08:44
tags:
  - Calcolatori
Related PDF/DOC:
  - "[[250306_L4-CodificaTesto.pdf]]"
Related Pages:
  - "[[Aritmetica dei calcolatori]]"
---
## Codifica testuale in binario
Per rappresentare un testo bisogna poter rappresentare le lettere al suo interno tramite sequenze di bit.

> Quanti possibili caratteri dobbiamo rappresentare?
 
Prendiamo come riferimento l'**alfabeto anglosassone**; dobbiamo però tenere conto anche di *lettere maiuscole e minuscole, numeri, punteggiatura, ecc*.

Si è deciso quindi di usare per ogni carattere **$7$ bit** ( $2^7=128$ possibili caratteri ). 

Questa codifica standard nasce come **ASCII** ( *American Standard Code for Information Interchange* ).

### Lo standard ASCII
Questo standard codifica un carattere come $7$ bit, dato che un byte è composto da $8$ **il bit più significativo è sempre $0$**.
### Extended ASCII
L'**extended ASCII** è una versione estesa dello standard che utilizza l'**intero byte** ( $8$ bit ) per permettere la rappresentazione di ulteriori $128$ caratteri meno comuni.

Non esiste un **singolo** extended ASCII, è invece più simile a una serie di estensioni  adibite al supportare vari alfabeti.

 Quindi se il bit più significativo di un carattere ha **valore $1$** non si può interpretare univocamente ma si deve far invece riferimento all'estensione in uso.

 Questo può creare problemi nella condivisione di file.

### UNICODE
Per risolvere il problema dell' univocità dei caratteri si è creato l' **UNICODE**.

Questo standard per poter rappresentare tutti i caratteri in maniera univoca necessità di molto spazio per codificare i caratteri, in particolare fino a **$32$ bit** ( $4$ byte ).

Questo standard è anche chiamato **UTF-32**.

> [!info ]- UTF-16
> Versione dell'UNICODE che utilizza uno spazio variabile per la codifica, a partire da **$16$ bit** ( $2$ byte ) 
> 

> [!info]- UTF-8
> Uguale all' UTF-16 ma con **$8$ bit** ; inoltre è compatibile con l' ASCII

