## Le 3 basi della Cybersecurity

> [!warning] Confidentiality
> - Prevenire la divulgazione non autorizzata di informazioni.
> - Permettere la condivisione/divulgazione autorizzata di informazioni.

> [!done] Integrity
> - Prevenire la modifica non autorizzata delle informazioni.
> - Permettere la modifica di informazioni da utenti autorizzati.

> [!info] Availability
> - Prevenire utenti non autorizzati dal trattenere o bloccare informazioni o servizi.
> - Fornire prontamente servizi o informazioni agli utenti autorizzati.

Queste tre propietà <mark class="hltr-red">NON SONO indipendenti tra loro</mark>. Per esempio un attaccante potrebbe sia leggere file confidenziali che modificarli (*Violazione di Confidentiality e Integrity*).

Se queste tre propietà vengono mantenute si può parlare di <mark class="hltr-purple">sicurezza dei dati</mark>.


<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Confidentiality
> Salvaguardia di restrizioni riguardo all accesso e condivisione di informazioni riservate, oltre che modi di proteggere la privacy personal e informazioni propietarie. 

Questo obbiettivo, genera il <mark class="hltr-orange">requisito di proteggere i dati da accessi non autorizzati o accidentali</mark>. 

La confidentiality copre i dati :
- Memorizzati
- Durante i processi
- Durante in transito

I modi per garantire la confidentiality sono ...
- **Criptazione dei dati**.
- **Access control**, cioè il processo di verifica dei permessi di chiunque voglia  accedere ai dati.



<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Integrity
> Salvaguardia dalla modifica o distruzione, impropria o non autorizzata dei dati; questo include anche la verifica dell'autenticità e provenienza dei dati.

> [!info]- Authenticity 
> The property of being genuine and being able to be verified and trusted; confidence in the validity of a transmission, a message, or message originator.

> [!info]- Non-repudiation
> Protection against an individual falsely denying having performed a particular action. Provides the capability to determine whether a given individual took a particular action such as creating information, sending a message, approving information, and receiving a message.

Questo obbiettivo, genera il <mark class="hltr-orange">requisito di proteggere i dati da tentativi intenzionali o accidentali di violare l'integrità dei dati o dei sistemi</mark>.

I modi per garantire l'integrity sono...
- Sistemi di **Version control**.
- **Audit trails** ( *Tracce/Log che attestano da chi e come è stato modificato un dato* ).

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Availability
> Assicurare l'accesso veloce e affidabile dei dati.

Questo obbiettivo, genera il <mark class="hltr-orange">requisito di bloccare tentativi intenzionali o accidentali di cancellare i dati o di prevenirne l'accesso</mark>.

I modi per garantire l'integrity sono...
- Sistemi di **backup**.
- Piani di **disaster recovery**.
- Sistemi di **storage in cloud**.

---


## Politiche, sistemi e meccanismi di sicurezza

> [!QUOTE] Politiche di sicurezza
> > The rules and requirements established by an organization that governs the acceptable use of its information and services, and the level and means for protecting the confidentiality, integrity, and availability of its information
> 
> Source : [NIST Glossary](https://csrc.nist.gov/Glossary/?term=1268)
> PDF : [[1-BasicNotions-2p.pdf#page=11&selection=26,0,32,35|1-BasicNotions-2p, p.11]] 

> [!QUOTE] Meccanismi di sicurezza
> > A device or function designed to provide one or more security services usually rated in terms of strength of service and assurance of the design; Implementation of a security policy.
> 
> Source : [NIST Glossary](https://csrc.nist.gov/Glossary/?term=1262)
> PDF : [[1-BasicNotions-2p.pdf#page=11&selection=26,0,32,35|1-BasicNotions-2p, p.11]]


> [!QUOTE] Servizi di sicurezza 
> > A capability that supports one, or more, of the security requirements (Confidentiality, Integrity, Availability). Examples of security services are key management, access control, and authentication.
> 
> Source : [NIST Glossary](https://csrc.nist.gov/Glossary/?term=1268)
> PDF : [[1-BasicNotions-2p.pdf#page=11&selection=42,0,46,15|1-BasicNotions-2p, p.11]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


![[EMBED/1-BasicNotions-2p 2.png]]
[[1-BasicNotions-2p.pdf#page=11&rect=68,128,452,390|1-BasicNotions-2p, p.11]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">


> [!example]- Esempi di politiche, servizi e meccanismi
> ![[1-BasicNotions-2p.pdf#page=12|1-BasicNotions-2p, p.12]]


---


## RISK ( Rischio )
>The probability that a particular security *threat will exploit a system vulnerability*. 
>
>A measure of the extent to which an entity is threatened by a potential circumstance or event, and typically a function of: 
>1. The adverse impacts that would arise if the circumstance or event occurs
> 2. The likelihood of occurrence.

Per capire cos'è il *richio* dobbiamo prima capire le sue componenti...

- <mark class="hltr-purple">Vulnerabilità</mark>.
- <mark class="hltr-blue">Minaccia</mark>.

> [!QUOTE] Vulnerability (Vulnerabilità)
> > Weakness in an information system, system security procedures, internal controls, or implementation that could be exploited or triggered by a threat source.
>
>Source : [NIST Glossary](https://csrc.nist.gov/Glossary/?term=2436)
> PDF : [[1-BasicNotions-2p.pdf#page=18&selection=6,0,8,74|1-BasicNotions-2p, p.18]]


> [!QUOTE] Threat (Minaccia)
> > The potential for a threat-source to successfully exploit a particular information system vulnerability.
> > 
> > Any circumstance or event with the potential to adversely impact organizational operations (*including mission, functions, image, or reputation*), organizational assets, or individuals through an information system via unauthorized access or tampering of information, and/or denial of service. 
> > 
>
>Source : [NIST Glossary](https://csrc.nist.gov/Glossary/?term=2156)
> PDF : [[1-BasicNotions-2p.pdf#page=19|1-BasicNotions-2p, p.19]]

Le <mark class="hltr-blue">minacce</mark> vengono caratterizzate da ...
- **Intento** : propensità all'attacco
- **Fattibilità** : abilità di succedere nell'attacco.

Mentre le  <mark class="hltr-purple">vulnerabilità</mark> sono caratterizzate da quanto è semplice ...
- **Identificarle** 
- **Sfruttarle**

Minacce e vulnerabilità ci dicono la <mark class="hltr-orange">probabilità di un attacco</mark>.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Il rischio porta alla possibilità di un **attacco**.

> [!quote] Attack (Attacco)
> > Any kind of malicious activity that attempts to **collect, disrupt, deny, degrade, or destroy information** system resources or the information itself.
> > 
> > Also an attack can be an attempt to gain unauthorized access to system services, resources, or information, or an attempt to compromise the system CIA triad.
> 
> Source: [NIST Glossary](https://csrc.nist.gov/glossary/term/attack )
> PDF : [[1-BasicNotions-2p.pdf#page=20|1-BasicNotions-2p, p.20]]



L' impatto di un attacco deve essere valutato nel rispetto degli interessi degli *stakeholder* (*persone interessate al sistema*).


> [!EXAMPLE] Esempio di attacco e conseguenze
> Trapelazione non autorizzata di informazioni personali di pazienti ospedalieri può avere conseguenze miti per l'ospedale, ma catastrofiche per le singole persone.



<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Threat model
>A threat model is a representation of all the information that affects the security of an application.
>
> It is a *view of the application* and its environment *through the lens of security*.

Per studiare meglio la probabilità di un attacco dobbiamo usare un Threat Model, composto da ...

- **Descrizione del sistema** da modellare.
- **Assunzioni di difficoltà future** che possono riscontrarsi se avviene un cambiamento delle possibili threat.
- **Minacce potenziali al sistema**.
- **Controlli da eseguire** per mitigare ogni minaccia.
- Controllo della **validità e correttezza del modello** (*verifica dell'efficacia dei controlli*).


> [!example]- Esempio di threat model
> ![[EMBED/1-BasicNotions-2p 3.png]]
> [[1-BasicNotions-2p.pdf#page=27&rect=90,155,506,370|1-BasicNotions-2p, p.27]]



Il threat model è utile quando messo in relazione con un **attaccante**, caratterizzato da...
- Motivazioni
- Capacità tecniche
- Minacce

La <mark class="hltr-orange">sicurezza di un sistema è sempre messa in relazione con un determinato attaccante</mark>.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

Le strategie di mitigazione delle minacce sono relative a :
- **Persone** (*E.g. regole per le password*)
- **Processi** (*E.g. aggiornare il software regolarmente*)
- **Tecnologia** (*E.g. autenticazione e access control*)

I controlli vengono classificati e divisi in ...
- **Preventivi** (*Prima dell'evento*)
- **Rilevativi** (*Durante l'evento*)
- **Correttivi** (*Dopo l'evento*)

I controlli sono basati sul <mark class="hltr-blue">risk management</mark> (*risolto nel threat model*) e il <mark class="hltr-purple">fattore umano</mark>, che impone che i controlli debbano essere semplici da usare e ricordare.