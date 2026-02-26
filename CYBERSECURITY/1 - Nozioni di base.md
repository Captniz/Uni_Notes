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


### RISK ( Rischio )
Per capire cos'è il richio dobbiamo prima capire le sue componenti...

- <mark class="hltr-purple">Vulnerabilità</mark>.
- <mark class="hltr-blue">Minaccia</mark>.

> [!QUOTE] Vulnerability (Vulnerabilità)
> > Weakness in an information system, system security procedures, internal controls, or implementation that could be exploited or triggered by a threat source.
>
>Source : [NIST Glossary](https://csrc.nist.gov/Glossary/?term=2436)
> PDF : [[1-BasicNotions-2p.pdf#page=18&selection=6,0,8,74|1-BasicNotions-2p, p.18]]


> [!QUOTE] Threat (Minaccia)
> > Any circumstance or event with the potential to adversely impact organizational operations (*including mission, functions, image, or reputation*), organizational assets, or individuals through an information system via unauthorized access, destruction, disclosure, modification of information, and/or denial of service. 
> > 
> > **Also, the potential for a threat-source to successfully exploit a particular information system vulnerability.**
>
>Source : [NIST Glossary](https://csrc.nist.gov/Glossary/?term=2156)
> PDF : [[1-BasicNotions-2p.pdf#page=19|1-BasicNotions-2p, p.19]]


%%PP 20%%