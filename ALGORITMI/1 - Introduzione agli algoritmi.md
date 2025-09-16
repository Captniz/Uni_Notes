---
Date created: 16-09-25 • 11:08
tags:
  - Algoritmi
Related PDF/DOC:
  - "[[01-introduzione.pdf]]"
Related Pages:
---
## Gli algoritmi
> Dato un **[[#^039f63|problema computazionale]]**, un algoritmo è un procedimento effettivo, espresso tramite un insieme di *passi elementari ben specificati in un sistema formale di calcolo*, che risolve il problema in tempo finito.


> [!NOTE]- Problema computazionale
> Dati un dominio di input e un dominio di output, un problema com- putazionale è rappresentato dalla *relazione matematica* che associa ogni elemento del dominio di input ad uno o più elementi del dominio di output.

^039f63

### Come descrivere un algoritmo
Il modo migliore per definire un algoritmo, in modo che sia il più possibile formale e indipendente dal linguaggio è lo <mark class="hltr-purple">pseudo-codice</mark>.
### Valutazione dell'efficienza e correttezza di un algoritmo
#### Efficienza
Per valutare l'**efficienza** di un algoritmo dobbiamo intanto sapere che alcuni problemi <mark class="hltr-red">non possono</mark> essere risolti in modo efficiente ( *per dimostrazione matematica* ), mentre altri possono avere una <mark class="hltr-blue">soluzione ottima</mark> ( *Non plus ultra* ).


> [!note] Complessità di un algoritmo
> Analisi delle **[[#^5add2f|risorse]]** impiegate da un algoritmo per risolvere un problema, in funzione della dimensione e dalla tipologia dell’input.


> [!important] Risorse degli algoritmi
> - **Tempo** : Tempo impiegato per completare l’algoritmo ( *Definito come umero di operazioni che caratterizzano lo scopo dell’algoritmo/operazioni rilevanti* ).
> - **Spazio** :  Quantità di memoria utilizzata.
> - **Banda** : Quantità di bit sprediti.
> 
> ![[01-introduzione.pdf#page=19]]

^5add2f

#### Correttezza
Nell'analisi della correttezza ( *per dimostrazione matematica* ) esistono problemi che <mark class="hltr-red">non possono</mark> essere risolti ( *non saranno mai corretti* ), mentre altri possono essere risolti  solo in <mark class="hltr-orange">maniera approssimata</mark> .

Per la valutazione della correttezza dobbiamo conoscere alcune definizioni di valori *invariabili*:

- **Invariante** : Condizione sempre vera in un certo punto del programma.
- **Invariante di ciclo**: Una condizione sempre vera all’inizio dell’iterazione di un ciclo.
- **Invariante di classe** :  Una condizione sempre vera al termine dell’esecuzione di un metodo della classe.

##### Dimostrazione della correttezza di un algoritmo iterativo
Per dimostrare la correttezza di un algoritmo iterativo si seguono le regole dell'<mark class="hltr-purple">induzione matematica</mark>, in particolare seguiamo 3 passi :

- **Inizializzazione** ( *Caso Base* ) : La condizione è vera alla prima iterazione di un ciclo.
- **Conservazione** ( *P. Induttivo* ) : Se la condizione è vera prima di un’iterazione del ciclo, allora rimane vera al termine ( *quindi prima della successiva iterazione* ).
- **Conclusione** : Quando il ciclo termina, l’invariante deve <mark class="hltr-red">rappresentare la “correttezza” dell’algoritmo</mark>.


> [!example]- Esempio
> ![[01-introduzione.pdf#page=33]]
