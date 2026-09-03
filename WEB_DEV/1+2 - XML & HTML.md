---
Date created: 01-09-26 • 17:11
tags:
  - Web
Related PDF/DOC:
  - "[[01-XML.pdf]]"
  - "[[02-html.pdf]]"
Related Pages:
---
# Linguaggi di markup

> [!QUOTE] Linguaggi di markup
> > A markup language is a system for annotating a document in a way that is syntactically distinguishable from the text.
> > 
> > The language specifies a code for formatting, both the layout and style, within a text file. The code used to specify the formatting are called *tags*,  and in most cases is human-readable.
> >  
> >  A markup language is NOT a programming language.
>
> PDF : [[01-XML.pdf#page=1&selection=12,0,36,22|01-XML, p.1]]

Markup è usato per ...
- Memorizzare, gestire, condividere e strutturare dati.
- Specificare vari attributi dei dati ad una macchina.
- Aiuta il riutilizzo dei dati.

### Tipologie di linguaggi di markup
#### Presentazione
Usati dai *word-processor* con effetto WYSIWYG per l'utente. Solitamente trasparenti all'utente.

#### Procedurale
Provvedono istruzioni per programmi per processare del testo (*Eg. Markdown*).

Il processore legge il testo dal'inizio alla fine in maniera sequenziale e segue le istruzioni che trova.

Questo tipo di linguaggio è direttamente accessibile all'utente.

#### Descrittivo o semantico
Serve ad *annotare* parti di un documento su cosa dovrebbero rappresentare.

Serve a descrivere come dovrebbe essere renderizzato un documento.
Questo è il caso di <mark class="hltr-orange">HTML</mark>.

### SGML
> Metalinguaggio che definisce altri linguaggi di markup. XML è basato su SGML.

Composto da :
- **Contenuto** del documento.
- **Grammatica**, che definisce la sintassi accettata.
- Lo **stylesheet**, che definisce come dovrebbe essere renderizzato l'output.

---
## XML
> Formalmente un gruppo di tecnologie, generalmente un linguaggio di markup.

XML viene utilizzato per molteplici scopi, ma in <mark class="hltr-red">se per se non fa nulla</mark>. Descrive solo i dati che poi vengono interpretati da diversi processi in maniera differenta a seconda dell'utilizzo.

Inoltre è anche facilmente leggibile dagli umani.

Un documento XML è anche rappresentabile attraverso un **albero** data la sua natura; questo albero è detto <mark class="hltr-orange">DOM</mark>.
### Struttura di un documento XML
Un documento XML deve essere *well-formed*, cioè segurie delle regole :
- XML è case-sensitive per quanto riguarda tag e regole
- Deve essere presente una singola *root tag* che contiene tutte le altre
- Ogni tag o deve essere chiuso attraverso la closing tag o la short form se vuoto.
- I tag devono essere annidati corrttamente.
- I tag devono seguire le regole definite nel XML-Schema o DTD
- ...

#### Prologo
> Definizione di versione e codifica.

Opzionale ma se presente è la prima riga.

#### Tag
XML non ha tag predefiniti, vengono descritti dall'utente a seconda dell'utilizzo.

I tag sono composti da ...
- Definizione del tag
- Regole per i tag (*come utilizzarli all'interno del documento*)
- Attributi ammessi
- Regole di contenimento (*quali tag possono includere altri tag e come*)

La definizione dei tag in realtà è <mark class="hltr-orange">opzionale</mark>.

I tag possono essere anche raggruppati in **namespaces** (*esattamente come in C*) per differenziare tag con lo stesso nome di autori diversi.

##### XML Schema o DTD
Le regole relative ai tag sono definite in uno di questi due documenti.

Se il documento è conforme si dice *valido*, tuttavia questa validazione è opzionale.

---

## HTML
> *Hypertext Markup Language* is the standard markup language for creating Web pages.

Un *ipertesto* (*hypertext*) è un testo renderizzato su un dispositivo elettronico che contiene referenze (*link*) ad altri testi; questi possono essere usati in qualunque momento per vedere il testo referenziato.

HTML ha due princìpi :
- Graceful degradation
- HTML dovrebbe converie la struttura di un testo e non come presentarlo all'utente (*regola filosofica, ormai obsoleta*)


> [!info] Graceful degradation
> L'atto di rimuovere tag o funzioni non supportate, magari a causa di una versione vecchia di HTML da parte dell'utente, senza compromettere il resto della pagina.

HTML dal punto sintattico ...
- Non è space-sensitive
- Non è case-sensitive

### Struttura di un file HTML
Il file inizia con il **doctype**, che descrive semplicemente tipo e versione del documento (*html, vers. 5 standard*). Il doctype inoltre porta con se il file **[[#XML Schema o DTD|DTD]]**, tuttavia <mark class="hltr-orange">HTML 5 non ha alcun file DTD</mark>.  

Segue sempre un tag `html` che contiene tutto il resto della pagina

Dopo il tag `html` si ha un `head` e un `body`. 

#### Head
L'head contiene i *metadati* del documento come charset, autore, descrizione, titolo e <mark class="hltr-purple">riferimenti a codice stylesheet o javascript</mark>.


> [!info] Charset
> Il charset è il set di caratteri da usare per mostrare la pagina; può essere :
> - **ASCII** : 128 Caratteri, vecchio standard
> - **ISO-8859** : 256 Caratteri, standard HTML 4
> - **ANSI** : 282 Caratteri, standard Windows
> - **UTF-8** : standard unicode HTML 5
>   
>   ![[EMBED/02-html 1.png]]
>[[02-html.pdf#page=7&rect=128,144,474,336|02-html, p.7]]


#### Body
Corpo del documento. Contiene i tag che saranno visibili sulla pagina.

### Tag HTML
#### Regole
A differenza di XML e altri markup HTML è tollerante su alcune regole relative ai tag :
- Alcuni tag non richiedono per forza un tag di chiusura o la shotrhand
- Si può fare nesting incrociato tra tag :
  ```html
  <B> The winner is: <I> Sofia Goggia! </B>Congratulations! </I>
  ```

#### Formattazione e rappresentazione
I tag hanno due possibili valori di display :
- **Blocco** : Iniziano su una nuova linea, hanno un margine e riempiono tutto lo spazio orizzontale disponibile.
- **Inline** : Rimangono in linea e occupano solo lo spazio orizzontale necessario.

#### Tag
##### Hyperlink
Possono indirizzare a ...
- URL
- Numeri di telefono (*tel:*)
- Indirizzi mail (*mailto:*)
- Altri elementi della pagina attraverso gli ID (*#*)


##### Form
Permettono di collezzionare e inviare informazioni a applicazioni in backend.

Un form ha gli attributi :
- `action` : Che contiene l'url dello script in backend
- `method` : Metodo HTTP per l'invio dei dati
- `autocomplete` : Se autocompletare con valori precedentemente inseriti dall'utente.
- `enctype` : MIMETYPE dei dati. Utilizzabile solo con il metodo post

##### Button e input
Simili all'interno dei form. 

Input può contenere solo testo ma ha diverse funzioni per questo.

Button può contenere altri tag e immagini.

Entramb possono avere l'attributo `type` come `submit`,`reset` e `button` (*funzione js generale*). In HTML 5 attraverso l'attributo `formaction` i tipi submit possono sovrascrivere l'azione del form e indirizzare i risultati ad un altro script.

