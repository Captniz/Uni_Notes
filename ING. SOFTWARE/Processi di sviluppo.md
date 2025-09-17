---
Date created: 17-09-25 • 08:50
tags:
  - Ing_Software
Related PDF/DOC:
  - "[[2-Metodologie-Processi.pdf]]"
Related Pages:
---
## Le basi dell'ingegneria del software

> [!QUOTE] Definizione di ingegneria del software
> > L’ingegneria del software è l’istituzione e l’impego di principi ingegneristici ben fondati, allo scopo di ottenere in modo economico software affidabile ed efficiente.
> >>  ~Fritz~ ~Bauer~
>
> PDF : [[2-Metodologie-Processi.pdf#page=4&selection=12,0,21,48|2-Metodologie-Processi, p.4]]

Vediamo i princìpi fondamentali di questa disciplina ...

### Formalità
La rigorosità e formalità sono necessari a complemento della creatività per aumentare il livello di correttezza.
### Modularità
Dividere un sistema in pezzi semplici: **moduli** 
- Alta coesione 
- Basso accoppiamento

---



![[EMBED/2-Metodologie-Processi.png]]
[[2-Metodologie-Processi.pdf#page=9&rect=69,47,679,268|2-Metodologie-Processi, p.9]]

---

### Separazione dei problemi
E' meglio separare i problemi per meglio gestire la complessità.

### Astrazione
Per semplificare e mantenere coerente il progetto, spesso è meglio **ignorare i dettagli implementativi**.

---

![[EMBED/2-Metodologie-Processi 1.png]]
[[2-Metodologie-Processi.pdf#page=10&rect=70,62,704,318|2-Metodologie-Processi, p.10]]
---

### Previsione dei cambiamenti
Con questo ci si riferisce alla capacità di affrontare l’**evoluzione** del progetto.


> [!example] Gestione delle configurazioni
>  Mantenere traccia delle relazione fra i vari moduli e le evoluzioni delle configurazioni dei vari moduli :
> 
> >La verione 3.0 e’ stata derivata dalla 2.95 in funzione di certi tipi di cambiamenti, la 2.95 e’ compatibile con la 2.7 di un altro modulo.


### Generalità
Si cerca sempre di risolvere il **problema generale**, <mark class="hltr-red">non solo l’istanza</mark>. 

Questo porta a una maggiore **riusabilità** ( *Possiamo riusare la soluzione generale in istanze e situazioni diverse* ). 

Tuttavia bisogna stare attenti a bilanciare i <mark class="hltr-orange">costi</mark> : Generalizzare ha un costo e chiaramente non vogliamo avere la soluzione per tutti i problemi.

### Incrementalità
Cioè sviluppare il progetto per passi continui ( *man mano* ), aggiungendo funzionalità.


> [!example] Sviluppo per prototipi incrementali
> Iterazioni successive con il cliente che vede crescere il sistema. Questo porta a un aumento dei costi e dei tempi.

## Il processo di sviluppo
> [!QUOTE] Definizione di processo di sviluppo
> > Stabilisce quando e come qualcuno fa cosa, per raggiungere un determinato obiettivo.
>
> PDF : [[2-Metodologie-Processi.pdf#page=15&selection=4,0,17,34|2-Metodologie-Processi, p.15]]

Diversi modelli hanno diverse caratteristiche :
- Flusso delle attività, dei task e loro interdipendenza. 
- Attività di controllo del progetto.
- Dettaglio e rigore del processo.
- Coinvolgimento degli [[#^a05c9e|stakeholders]].
- Autonomia del team.
- Prescrizione dei ruoli e loro organizzazione.


> [!NOTE]- Stakeholders
> > Tutti i soggetti, individui od organizzazioni, attivamente coinvolti in un’iniziativa economica [...], il cui interesse è negativamente o positivamente influenzato dal risultato dell’esecuzione.
> ~Enciclopedia~ ~Treccani~

^a05c9e

---

Prima di scegliere un modello bisogna **valutare il processo**.

La valutazione del processo consiste nel raccogliere informazioni sul processo di sviluppo, confrontarle con standard o best practice e determinare <mark class="hltr-orange">quanto il processo sia maturo, efficace e controllabile</mark>.

Vediamo dei tipi di processo...

### Modello a cascata
Il **processo a cascata** è possibilmente il più *lineare*.

Prima si vengono a conoscere e analizzare i requisiti, per poi procedere seguendo il processo.

---
![[EMBED/2-Metodologie-Processi 2.png]]
[[2-Metodologie-Processi.pdf#page=18&rect=40,41,710,234|2-Metodologie-Processi, p.18]]

---


> [!example]- Esempio pratico IBM
> 
![[EMBED/2-Metodologie-Processi 3.png]]
[[2-Metodologie-Processi.pdf#page=19&rect=64,5,644,425|2-Metodologie-Processi, p.19]]

### Modello incrementale
Il **processo incrementale** si basa sulla continua aggiunta di funzionalità, rilasciando una versione del progetto per volta.

---


![[EMBED/2-Metodologie-Processi 4.png]]
[[2-Metodologie-Processi.pdf#page=20&rect=33,40,697,423|2-Metodologie-Processi, p.20]]



![[EMBED/2-Metodologie-Processi 5.png]]
[[2-Metodologie-Processi.pdf#page=21&rect=21,45,708,436|2-Metodologie-Processi, p.21]]

---

### Modello a prototipi
Quando il cliente ha un bisogno legittimo ma non riesce a fornire indicazioni precise sui dettagli, il primo passo consiste nello sviluppo di un **prototipo**.

---

![[EMBED/2-Metodologie-Processi 6.png]]
[[2-Metodologie-Processi.pdf#page=22&rect=182,12,561,331|2-Metodologie-Processi, p.22]]

---

> [!QUOTE]- Definizione di prototipo
> > Una rappresentazione di un prodo.o o di un sistema, o di una sua parte, che, anche se in qualche modo limitata, può essere u6lizzata a scopo di valutazione.
> > > ~ISO-13407~
>
> PDF : [[2-Metodologie-Processi.pdf#page=24&selection=8,0,14,11|2-Metodologie-Processi, p.24]]


Un prototipo non deve necessariamente essere funzionante, spesso può essere semplicemente un **mock-up** o un **wireframe**.

| Wireframe                                                                     | Mock-up                                                                                        |
| ----------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| Il focus è il design dell'UI (*trovare la migliore user experience*).         | Rappresenta la fase successiva del wireframe, in cui gli elementi del design entrano in gioco. |
| Crea una bozza grafica su un foglio di carta oppure su un software specifico. | Si hanno più dettagli per dimostrare il *look and feel* del prodotto finito.                   |

Gli svantaggi di questo approccio sono:
- Il cliente ha per le mani una versione del software che sembra essere quella finale.
- Gli sviluppatori scendono a compromessi per sviluppare rapidamente il prototipo.
- Qualità scadente


### Modello a spirale
Unisce le iterazioni del [[#Modello a prototipi]] e il sistema controllato del [[#Modello a cascata]].

---
![[EMBED/2-Metodologie-Processi 7.png]]
[[2-Metodologie-Processi.pdf#page=27&rect=53,0,662,315|2-Metodologie-Processi, p.27]]

---

### Model Driven Development (MDD)
Un **modello**, in questo caso, si riferisce a una rappresentazione del sistema/progetto ( *forma particolare di prototipo* ). La notazione più comune per un modello è l'<mark class="hltr-orange">UML</mark>.


Il modello può *evolvere* con lo sviluppo, ed è inoltre molto facile generare il codice e i **test-cases** ( *anche automatico* ). 


%%pp29%%