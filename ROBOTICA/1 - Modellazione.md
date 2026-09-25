---
Date created: 24-09-26 • 15:33
tags:
  - Robotica
Related PDF/DOC:
  - "[[1-modellazione_old.pdf]]"
Related Pages:
---
# Terminologia e anatomia di un robot
> Lo scopo in robotica è imprimere una macchina con un certo grado di intelligenza.
> 
> L'intelligenza di un robot è espressa attraverso interazioni fisiche col mondo esterno.

Esistono due macrotipi di robot :
- <mark class="hltr-orange">MANIPOLATORI</mark> : Robot con **base fissa**.
- <mark class="hltr-purple">ROBOT MOBILI</mark> : Robot con **base mobile**.

( *Ovviamente esistono combinazioni di questi.* )
## Manipolatori
Un manipolatore è formato da vari componenti, tra cui i più importanti ...
- **Braccio** : Braccio mobile del robot
- **Polso** : Posto tra braccio e effettore e aumenta la destrezza del robot. Il polso è una *giuntura*.
- **Effettore-Terminale** : Utensile sulla terminazione del braccio con cui il robot manipola gli oggetti.

Dal punto di vista geometrico il braccio è composto da *links*, cioè i segmenti di corpo rigido e da *joints* (*giunture*), cioè le articolazioni meccaniche.

> [!example] Schema di un manipolatore
> ![[EMBED/lect2.png]]
[[1-modellazione_old.pdf#page=7&rect=81,13,786,442|lect2, p.7]]

### Giunture
>Le giunture collegano due parti del braccio è forniscono mobilità al manipolatore.

Ne esistono due tipi :
- <mark class="hltr-orange">PRISMATICHE</mark> : Permette un movimento di **traslazione** relativo tra due link.
- <mark class="hltr-purple">RIVOLVENTI</mark> : Permette un movimento di **rotazione** relativo tra due link.


> [!example] Schema di una giuntura
> ![[EMBED/lect2 1.png]]
[[1-modellazione_old.pdf#page=8&rect=37,48,933,353|lect2, p.8]]

Le giunture definiscono due proprietà di un robot ...
- <mark class="hltr-green">Destrezza</mark> : L'abilità di un robot di adattarsi a una varietà di oggetti e azioni.
- <mark class="hltr-blue">Rigidità</mark> : L'abilità del corpo di un robot di resistere a deformazioni. Formalmente ...
	> Forza necessaria per indurre un movimento su uno dei gradi di libertà del robot.

#### Gradi di libertà | DoF
> In una struttura meccanica un **DoF** è definito come uno specifico modo in cui un robot può muoversi/articolarsi.
> 
> Tipicamente in un robot, ogni giuntura viene creata attraverso un *attuatore* ( *motore brushless o simile* ) e fornisce alla struttura un nuovo DoF.

Dati i DoF, possiamo definire la **workspace** come l'area che l'effettore può raggiungere.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Polso
Tutti i manipolatori hanno un polso che collega un effettore , 
quest'ultimo varia in forma dipendentemente dallo scopo del robot.

La configurazione del polso che ne massimizza la destrezza è un **[[#Sferici|manipolatore sferico]]** .


> [!example] Polso
>  ![[EMBED/lect2 7.png]]
[[1-modellazione_old.pdf#page=17&rect=561,109,841,387|lect2, p.17]]


<hr style="width: 70%; margin-left: auto;margin-right: auto;">


### Tipi di manipolatori

In base a numero e tipo di giunture ( *e di conseguenza di DoF* ) si definiscono diversi tipi di manipolatori ...
- Cartesian 
- Cylindrical 
- Spherical 
- SCARA 
- Antropomorphic

#### Cartesiani
Caratterizzati da **tre giunture prismatiche** che operano su tre assi perpendicolari tra loro ( *$[x,y,z]$* ). La workspace risultante è parallelepipeda.

Caratteristiche :
- Buona rigidità meccanica.
- Buona precisione all'interno di tutta la workspace.
- Ridotta destrezza a causa delle giunture prismatiche.
  ( *Eg. Non puo operare con l'effettore sui lati o a testa in giu* )


> [!example] M. Cartesiano
> ![[EMBED/lect2 2.png]]
[[1-modellazione_old.pdf#page=12&rect=582,53,932,374|lect2, p.12]]


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Cilindrici
Composti da **due giunture prismatiche e una rivolvente**.
La workspace è ovviamente cilindrica cava.

Caratteristiche :
- Meno precisione su movimenti orizzontali.
- Buona rigidità.


> [!example] M. Cilindrico
> ![[EMBED/lect2 3.png]]
[[1-modellazione_old.pdf#page=13&rect=585,33,938,356|lect2, p.13]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Sferici
Formato da **due giunture rivolventi e una prismatica**.
La workspace è una sfera cava.

Caratteristiche :
- Precisione ridotta all'aumentare dello *stroke radiale*.


> [!info]- Definizione di Stroke radiale
> - **Direzione radiale** : Direzione dell'attuatore rispetto all'asse centrale del manipolatore. ( *avanti o indietro rispetto al centro* ).
> - **Stroke radiale** : Distanza tra l'attuatore e l'asse centrale-


> [!example] M.Sferico
> ![[EMBED/lect2 4.png]]
[[1-modellazione_old.pdf#page=14&rect=568,89,905,429|lect2, p.14]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### SCARA |  Selective Compliance Assembly Robot Arm 
Stesso numero e tipo di giunture di un manipolatore sferico ma disposti in maniera differente : le **due g. rivolventi sono disposte parallele** l'una all'altra.

Caratteristiche :
- Rigidità alta a carichi verticali.
- Conforme a carichi orizzontali.
- La precisione diminuisce con la distanza radiale. 


> [!example] M.SCARA
>  ![[EMBED/lect2 5.png]]
[[1-modellazione_old.pdf#page=15&rect=569,106,906,398|lect2, p.15]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Antropomorfi
Caratterizzati da **tre giunture rivolventi**, due parallele e una perpendicolare ( *base* ). La workspace è sferica.

Simile ad un arto umano, pertanto la seconda giuntura è detta *spalla* e la terza è detto *gomito*.

Caratteristiche :
- Destrezza massima.
- Precisione del polso dipendente dalla posizione all'interno della workspace.



> [!example] M.Antropomorfo
> ![[EMBED/lect2 6.png]]
[[1-modellazione_old.pdf#page=16&rect=682,237,954,495|lect2, p.16]]

---

## Robot mobili
Un robot mobile è caratterizzato da una base mobile. Si dividono in due sottocategorie principali :
- <mark class="hltr-orange">Con ruote</mark> : Un corpo rigido ( *chassis* ) e un sistema di ruote che provvedono al movimento. Può essere parte di un sistema di rimorchi, connessi da giunture rivolventi.
- <mark class="hltr-purple">Con gambe</mark> : Corpi rigidi multipli connessi da giunture. Alcuni link inferiori rimangono a contatto col terreno per fornire la locomozione.

Proseguiremo analizzando strettamente **robot su ruote**.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Ruote
Esistono tre tipi di ruote con scopi diversi :
- <mark class="hltr-blue">Fisse</mark> : Può ruotare solo attorno a un asse passante per il centro e ortogonale al piano della ruota.
- <mark class="hltr-purple">Orientabile/Sterzabile</mark> : Ha due assi di rotazione, l'asse standard ( *uguale alle fisse* ) e un asse verticale passante per il centro. 
- <mark class="hltr-orange">Caster/Piroettante</mark> : Simile a una ruota orientabile, ma il suo asse verticale ha un disassamento (*offset*) dal centro della ruota. 
  Questa ruota ha la particolarità di allinearsi automaticamente con la direzione di movimento del telaio durante un movimento. 



> [!example] tipi di ruota
> ![[EMBED/lect2 8.png]]
[[1-modellazione_old.pdf#page=19&rect=484,135,885,387|lect2, p.19]]


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Tipi di movimento su ruote
Dato un set di ruote possiamo definire diverse strutture kinematiche.

#### Veicolo a guida differenziale | Differential drive
In un robot a guida differenziale si hanno **due ruote fisse** attuate (*possono ricevere comani*) sullo stesso asse e **una ruota caster** per mantenere il robot in equilibrio statico.


Il robot può ruotare applicando una velocità diversa alle ruote fisse, o muoversi in rettilineo applicando la stessa velocità angolare; può inoltre ruotare sul posto impostando le due velocità su valori opposti.



> [!example] Differential drive
>![[EMBED/lect2 9.png]]
[[1-modellazione_old.pdf#page=20&rect=556,75,922,334|lect2, p.20]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Robot Synchro Drive
Nella configurazione synchro drive ci sono **tre ruote orientabili collegate da una catena**. inoltre sono presenti due motori: 
- Uno ruota le tre ruote *"sincronamente"* 
- L'altro trasmette il movimento a tutte le ruote

Può eseguire la stessa cinematica del differential drive, ma richiede un solo motore per muoversi in rettilineo.


> [!example] Synchro drive
> ![[EMBED/lect2 10.png]]
[[1-modellazione_old.pdf#page=21&rect=612,6,956,232|lect2, p.21]]
> 
> ---
>
> Diciture dello schema Synchro Drive:
> - **steering pulley** (puleggia di sterzata) 
> - **driving pulley** (puleggia di trazione)
> - **wheel steering axis** (asse di sterzata della ruota)
> - **drive belt** (cinghia di trazione)
> - **steering belt** (cinghia di sterzata)
> - **steering motor** (motore di sterzata)
> - **drive motor** (motore di trazione)
> - **rolling axis** (asse di rotolamento)


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Robot a triciclo
In un triciclo abbiamo **due ruote fisse** e azionate da un motore e **una ruota orientabile attorno all'asse verticale**, di cui la rotazione è governata da un altro motore.

È anche possibile che i due motori operino entrambi sulla ruota sterzante per girarla e per garantire la locomozione.


> [!example] Triciclo
> ![[EMBED/lect2 11.png]]
[[1-modellazione_old.pdf#page=22&rect=499,104,862,366|lect2, p.22]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Tipo automobile | Carlike
Simile al triciclo, ma **con due ruote sterzanti**.
Anche in questo caso la trazione può essere sulle ruote fisse (*trazione posteriore*) o sulle ruote anteriori (*trazione anteriore*).


Contrariamente a un robot a guida differenziale, un carlike o un triciclo non possono ruotare sul posto e ha un raggio di curvatura limitato.


> [!example] Carlike
> ![[EMBED/lect2 12.png]]
[[1-modellazione_old.pdf#page=23&rect=549,109,909,361|lect2, p.23]]


##### Sterzo di Ackerman
Nel caso le due ruote sterzanti operino allo stesso angolo si presenta un problema di stabilità durante una curva; questo è risolto dallo <mark class="hltr-orange">sterzo Ackerman</mark>.

La soluzione comporta che le due ruote debbano seguire la stessa traiettoria circolare.

Per seguire lo stesso cerchio, la **ruota interna deve girare più di quella esterna**.
Nelle auto moderne, questo problema viene risolto attraverso la geometria dell'asse.

il problema viene *generalizzato* nel caso di rimorchi.

> [!example] Schema di una sterzata secondo schema Ackerman
> ![[EMBED/lect2 13.png]]
[[1-modellazione_old.pdf#page=25&rect=511,226,893,486|lect2, p.25]]
> 
> ---
> 
> ![[EMBED/lect2 14.png]]
[[1-modellazione_old.pdf#page=25&rect=513,2,845,224|lect2, p.25]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Omnibot | Robot con range completo di movimento
A differenza dei manipolatori, i robot mobili non hanno uno spazio di lavoro limitato e possono raggiungere qualsiasi posizione nello spazio euclideo.

Tuttavia diversi tipi di robot presentano vincoli di movimento ( *E.g. robot a guida differenziale non può muoversi lateralmente lungo l'asse che collega le ruote azionate.* ); questi vengono detti **vincoli non olonomi**.

Gli omnibot <mark class="hltr-red">non presentano vincoli di movimento e possono muoversi in qualsiasi direzione</mark> : Sono caratterizzati da tre ruote caster azionate autonomamente ( *in posizioni simmetriche* ).


> [!example] Omnibot
> ![[EMBED/lect2 15.png]]
[[1-modellazione_old.pdf#page=27&rect=526,73,836,385|lect2, p.27]]

