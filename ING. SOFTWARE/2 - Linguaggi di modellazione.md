---
Date created: 24-09-25 • 08:43
tags:
  - Ing_Software
Related PDF/DOC:
  - "[[5-Linguaggi-Modellazione.pdf]]"
Related Pages:
---
## Il modello

> [!QUOTE] Definizione di Modello
> > Rappresentazione di un'enità in modo **semplificato**. 
> > - Astrazione di aspetti o dettagli irrilevanti per lo scopo a cui è destinato il modello. 
> > - Focus sulle informazioni importanti.
>
> PDF : [[5-Linguaggi-Modellazione.pdf#page=4&selection=4,0,17,28|5-Linguaggi-Modellazione, p.4]]


Il linguaggio di modellazione preponderante è l'**UML**, tuttavia non è l'unico; è importante decidere un linguaggio adatto alle proprie necessità di rappresentazione. 

Inoltre, linguaggi diversi possono essere utilizzati all’interno delle varie fasi dello sviluppo del software.

## Linguaggi di modellazione
### UML - (Unified Modeling Language)
Il linguaggio UML è detto *Entity-Relationship*, cioè mette in <mark class="hltr-orange">relazione</mark> tramite costrutti grafici varie <mark class="hltr-purple">entità</mark> e le loro caratteristiche.


> [!example] Esempio di diagramma UML
> 
![[EMBED/5-Linguaggi-Modellazione.png]]
[[5-Linguaggi-Modellazione.pdf#page=11&rect=38,33,691,394|5-Linguaggi-Modellazione, p.11]]


> [!info]- Costrutti di base
>![[EMBED/5-Linguaggi-Modellazione 1.png]]
> [[5-Linguaggi-Modellazione.pdf#page=10&rect=358,33,698,521|5-Linguaggi-Modellazione, p.10]]


In particolare UML <mark class="hltr-red">NON E'</mark> :
- Un metodo di sviluppo (*metodologia*).
- Non è legato ad una metodologia particolare.
- Un linguaggio di programmazione.

Mentre <mark class="hltr-green">E'</mark> :
- Un linguaggio o notazione.
- Un linguaggio di modellazione.
- Usato dai metodi più classici fino a quelli agili.
### BPMN - (Business Process Modeling Notation)

BPMN è lo standard per la rappresentazione di **processi di business** e il coordinamento delle attività.


> [!example] Esempio di diagramma BPMN 
> 
![[EMBED/5-Linguaggi-Modellazione 2.png]]
[[5-Linguaggi-Modellazione.pdf#page=14&rect=34,26,699,407|5-Linguaggi-Modellazione, p.14]]

### Linguaggi orientati alla sicurezza
Molti approcci alla **SRE** (*Security Requirements Engineering*) usano linguaggi per costruire modelli del sistema.

In particolare in questo gruppo si possono usare molti linguaggi diversi, dipendentemente da cosa voglio mettere in evidenza :
- UML-based
- Goal-oriented
- Machine-oriented
- Business processes
- Socio-technical systems

>[!example] Esempio di diagramma UML per Misuse-Case
> ![[EMBED/5-Linguaggi-Modellazione 3.png]]
> [[5-Linguaggi-Modellazione.pdf#page=17&rect=39,7,678,431|5-Linguaggi-Modellazione, p.17]]

---

>[!example] Esempio di diagramma con linguaggio Goals-Based
> 
![[EMBED/5-Linguaggi-Modellazione 4.png]]
[[5-Linguaggi-Modellazione.pdf#page=18&rect=85,18,701,414|5-Linguaggi-Modellazione, p.18]]

---

> [!example] Esempio di diagramma Social View
> 
> 
![[EMBED/5-Linguaggi-Modellazione 5.png]]
[[5-Linguaggi-Modellazione.pdf#page=20&rect=14,13,708,510|5-Linguaggi-Modellazione, p.20]]


