---
Date created: 28-11-25 • 13:46
tags:
  - Reti
Related PDF/DOC:
  - "[[reti_cap6_PC.pdf]]"
Related Pages:
---

>Composto da *Host* e *Router* detti **nodi** si occupa di stabilire un canale di comunicazione che collega due nodi adiacenti lungo un certo percorso (*link*).

Un pacchetto di livello 2 è detto <mark class="hltr-orange">frame</mark>.

>[!warning] Caratteristiche principali
>![[EMBED/reti_cap6_PC.png]]
[[reti_cap6_PC.pdf#page=5&rect=14,66,712,443|reti_cap6_PC, p.5]]

---

## Funzioni del data-link
Il livello 2 offre questi servizi:
- Creazione di un frame di livello 2, **accesso al link**.
- **Consegna affidabile** tra nodi adiacenti.

>[!important]- Accesso al link
>Per svolgere questo compito il livello 2 ...
>- Incapsula un datagramma in un frame, aggiunge header e trailer.
>- Fornisce un meccanismo di accesso al canale se il mezzo di comunicazione è condiviso con altri dispositivi.
>- Utilizza indirizzi di livello 2 (*MAC Address*) negli header dei frame per identificare mittente e destinatario.

>[!important]- Consegna affidabile
>Con questo si intende dei sistemi di controllo di errore, che tendono ad essere applicati <mark class="hltr-red">SOLO</mark> su link **wireless**.
>
>Solitamente i link fisici hanno un basso tasso di errore. 

%%PP 7 pdf%%

---

%%APPunti veloci%%

