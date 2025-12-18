---
Date created: 15-12-25 • 16:51
tags:
  - Reti
Related PDF/DOC:
Related Pages:
---
# Guida all'installazione della VM
> [Link al download della VM](https://tinyurl.com/unitn-reti-vm)


> [!info] Versioni
> - Mac con chip M1-M5 → *`ARM version`*
> - Altri sistemi → *`Versione standard`*

> [!info] Credenziali
> - Username: `network`
> - Password: `network`


## Importazione

1. Aprire VirtualBox
2. File → Importa Appliance
3. Selezionare il file `.ova` scaricato
4. Completare l'importazione


E' importante <mark class="hltr-red">aggiornare all'ultima versione</mark> :
```bash
cd networking-kathara-labs
git pull
```


---


# Guida all'uso di Kathara
## Struttura di un laboratorio

```sh
lab/
├── lab.conf
├── pc1.startup
├── pc1/
│   └── etc/
│       └── resolv.conf
├── dnsroot.startup
├── dnsroot/
│   └── etc/
│       └── bind/
│           ├── named.conf
│           ├── named.conf.options
│           └── db.root
```

### File `lab.conf`

```bash
# Nome della macchina[interfaccia]="nome_rete"
pc1[0]="A"                    # pc1 connesso alla rete "A" tramite eth0
pc1[image]="kathara/base"     # immagine Docker da usare

# Server con immagine personalizzata
frontend[0]="A"
frontend[image]="dns-lab/frontend"

# Router con più interfacce
r1[0]="A"                     # eth0 connesso alla rete A
r1[1]="B"                     # eth1 connesso alla rete B

# Configurazioni speciali (es. Wireshark)
wireshark[bridged]=true
wireshark[port]="3000:3000" # Apri wireshark sulla porta 3000 (collegabile da web browser)
wireshark[image]="lscr.io/linuxserver/wireshark"
```

- `[0]`, `[1]`, `[2]` = interfacce di rete (`eth0`, `eth1`, `eth2`)
- Il nome tra virgolette (es. `"A"`) è il **collision domain** (rete condivisa)
- Le macchine nella stessa rete possono comunicare direttamente
- Potete vedere il **collision domain** come se fosse uno switch connesso a tutte le macchine di quel dominio

---

### File `*macchina*.startup`

Ogni macchina può avere un file `.startup` (es. `pc1.startup`) che contiene i comandi da eseguire all'avvio.

```bash
# Assegnare indirizzo IP
ip address add 192.168.0.111/24 dev eth0

# Attivare l'interfaccia
ip link set eth0 up

# Impostare il gateway di default
route add default gw 192.168.0.100 dev eth0
```

- `pc1` ha indirizzo ip `192.168.0.111` nella sottorete `192.168.0.0/24`, collegata tramite interfaccia `eth0` (connessa nel **collision domain** `A`)
  - `/24` = subnet mask (255.255.255.0)
  - Rete: `192.168.0.0`
  - Host: `.111`
  - Broadcast: `192.168.0.255`
  - Indirizzi disponibili: `192.168.0.1` - `192.168.0.254`
- tutte le comunicazioni verso macchine non di questa sottorete vengono mandate di default alla macchina `192.168.0.100`, impostata come _gateway_ di default, che viene incaricata di inoltrarle
- comunicazioni verso macchine della stessa sottorete passano direttamente dal **collision domain**

---

### File `*router*.startup`

```bash
ip a add 192.168.0.100/24 dev eth0
ip link set eth0 up
ip a add 192.168.1.100/24 dev eth1
ip link set eth1 up
```

Un router è connesso a più sottoreti e più **collision domain**, e viene usato come tramite (_gateway_) per comunicazioni tra entrambe. Per ogni interfaccia, settiamo l'indirizzo IP e avviamo l'interfaccia di rete Ethernet.

#### Connessioni ad altri router


> [!warning] Posizione della config
> <mark class="hltr-red">Da aggiungere a</mark> `router.startup`.


```bash
# Sintassi generale
ip route add <rete_destinazione> via <gateway>

# Esempi
ip route add 192.168.2.0/24 via 192.168.0.2
ip route add 10.0.0.0/16 via 10.0.2.2

# Default gateway
route add default gw 192.168.0.100 dev eth0
# Sintassi getaway : gw <getaway> <source>
# Quindi tutti i pacchetti arrivati sull'interfaccia eth0 non riconosciuti escono su 192.168.0.100
```

#### Collision-domains tra router


> [!quote] Definizione di collision domain
> > In una rete di calcolatori, un dominio di collisione è *un insieme di nodi che concorrono per accedere allo stesso mezzo trasmissivo* e successivamente trasmettere.
> > > ~Wikipedia~ ~:~ ~Dominio~ ~di~ ~collisione~ 


Due router sono generalmente collegati tra loro da un **collision domain** unico

- **LINK_RI** (R_ROOT ↔ R_IT): 192.168.10.0/24
- **LINK_IN** (R_IT ↔ R_NET): 192.168.20.0/24
- **LINK_RN** (R_ROOT ↔ R_NET): 192.168.30.0/24

```bash
# Router ROOT
r_root[0]="ROOT"       # eth0 → rete ROOT
r_root[1]="LINK_RI"    # eth1 → link verso R_IT
r_root[2]="LINK_RN"    # eth2 → link verso R_NET

# Router IT
r_it[0]="IT"           # eth0 → rete IT
r_it[1]="LINK_RI"      # eth1 → link da R_ROOT
r_it[2]="LINK_IN"      # eth2 → link verso R_NET

# Router NET
r_net[0]="NET"         # eth0 → rete NET
r_net[1]="LINK_IN"     # eth1 → link da R_IT
r_net[2]="LINK_RN"     # eth2 → link da R_ROOT
```

#### Algoritmo di Dijkstra

L'algoritmo di Dijkstra trova il percorso più breve in un grafo pesato :

1. Parti dal nodo sorgente (distanza = 0)
2. Tutti gli altri nodi hanno distanza = $∞$
3. Visita il nodo con distanza minima non ancora visitato
4. Aggiorna le distanze dei vicini se trovi un percorso più breve
5. Ripeti finché tutti i nodi sono visitati



> [!example] *Title*
> ```sh
>            [R_ROOT]
>           /        \
>LINK_RI  /          \  LINK_RN
>cost=1  /            \  cost=4
>        /              \
>   [R_IT]---LINK_IN---[R_NET]
>             cost=2
>```


Quindi partendo da `R_ROOT` ...

| Iterazione | Nodo Corrente | R_ROOT | R_IT  | R_NET |
| ---------- | ------------- | ------ | ----- | ----- |
| Init       | -             | **0**  | $∞$   | $∞$   |
| 1          | R_ROOT        | 0      | **1** | **4** |
| 2          | R_IT          | 0      | 1     | **3** |
| 3          | R_NET         | 0      | 1     | 3     |

Vediamo che :
- **R_IT**: costo 1 (*diretto via LINK_RI*).
- **R_NET**: costo 3 (*via R_IT,* <mark class="hltr-red">non diretto!</mark>).



> [!important] Propietà dell'algoritmo di Djikistra
> Il percorso diretto R_ROOT → R_NET costa 4, ma passando da R_IT costa solo 1+2=3. <mark class="hltr-orange">Dijkstra trova automaticamente il percorso migliore</mark>.



---

### FIle `dnsserver.startup`

```bash
# Come prima
ip address add 192.168.0.5/24 dev eth0
ip link set eth0 up
route add default gw 192.168.0.100 dev eth0

# Avviare il servizio DNS (BIND)
systemctl start named
```

#### BIND

##### File `Named.conf`
> Config principale


All'interno di **`named.conf`** (*Config principale*), la propietà `recursion yes` fa si che il DNS faccia query per conto del client (*ricorsivo*). 

Con `no`, restituisce solo referral.

```json
options {
    directory "/var/cache/bind";
    recursion yes;    # Abilita query ricorsive
    allow-query { any; };
};

zone "." {
    type master;
    file "/etc/bind/db.root";
};
```

##### File **`db.root`**
>File per il root DNS

```
$TTL    604800
@       IN      SOA     ROOT-SERVER. admin.root. (
                        2         ; Serial
                        604800    ; Refresh
                        86400     ; Retry
                        2419200   ; Expire
                        604800 )  ; Negative Cache TTL

; Nameserver per questa zona
@               IN      NS      ROOT-SERVER.
ROOT-SERVER.    IN      A       192.168.0.5

; Deleghe ai TLD
it.             IN      NS      dnsit.it.
dnsit.it.       IN      A       192.168.0.1

net.            IN      NS      dnsnet.net.
dnsnet.net.     IN      A       192.168.0.2
```


> [!info]- Tipi di Record DNS
> 
| Tipo      | Significato                             | Esempio                                      |
| --------- | --------------------------------------- | -------------------------------------------- |
| **A**     | Address - mappa nome → IP               | `api.example.com. IN A 192.168.0.112`        |
| **NS**    | Nameserver - indica il DNS autoritativo | `example.com. IN NS dns.example.com.`        |
| **CNAME** | Alias per un altro nome                 | `www.example.com. IN CNAME api.example.com.` |
| **MX**    | Mail Exchange                           | `example.com. IN MX 10 mail.example.com.`    |


#### Gerarchia DNS
Il DNS è **gerarchico**: il root conosce i *TLD*, i TLD conoscono i domini di secondo livello, e così via.

```
ROOT (.) → TLD (.it, .net, .com) → Authoritative (startup.net)
```


#### Flusso di una Query DNS

Quando `pc1` cerca `api.startup.net`:

1. **pc1** chiede al **Local DNS** (192.168.0.110)
2. **Local DNS** chiede al **Root DNS** (192.168.0.5)
3. **Root DNS** risponde: "Per .net vai a dnsnet.net (192.168.0.2)"
4. **Local DNS** chiede a **dnsnet** (192.168.0.2)
5. **dnsnet** risponde: "Per startup.net vai a dnsstart (192.168.0.22)"
6. **Local DNS** chiede a **dnsstart** (192.168.0.22)
7. **dnsstart** risponde: "api.startup.net = 192.168.0.112"
8. **Local DNS** invia la risposta a **pc1**

---

## Server HTTP

Supponiamo di avere un server http già compilato come immagine docker, che vogliamo testare.

In `lab.conf` usiamo l'immagine apposita fornitaci :

```bash
# Server con immagine personalizzata
frontend[0]="A"
frontend[image]="dns-lab/frontend"
```

E nel `frontend.startup` avviamo il server :

```bash
ip address add 192.168.0.113/24 dev eth0
ip link set eth0 up
route add default gw 192.168.0.100 dev eth0

# Avviare il server web
cd /opt/frontend
python3 -m uvicorn app.main:app --host 0.0.0.0 --port 80 &  # & = esegui in background
```

---

## Avviare un laboratorio

```bash
# Avvia il laboratorio
kathara lstart
# Senza aprire i terminali
kathara lstart --noterminals

# Fermare e pulire il laboratorio
kathara lclean

# Connettersi a una macchina specifica
kathara connect pc1
kathara connect r1
kathara connect dnsroot

# Aggiungere un collision domain a Wireshark per poterlo analizzare
kathara lconfig -n wireshark --add A
kathara lconfig -n wireshark --add B
```

### Comandi dentro una macchina

```bash
# Visualizzare le interfacce di rete
ip a
ip addr show

# Visualizzare la tabella di routing
route -n
ip route show

# Test di connettività
ping 192.168.0.1
ping -c 4 192.168.0.1    # solo 4 pacchetti

# Tracciare il percorso dei pacchetti
traceroute 192.168.0.1

# Query DNS con dig
dig A example.com                    # Record A (indirizzo IP)
dig NS example.com                   # Record NS (nameserver)
dig @192.168.0.5 A example.com       # Query a un DNS specifico

# Query DNS con nslookup
nslookup example.com
nslookup example.com 192.168.0.5

# Richiesta HTTP semplice
curl http://api.example.com/

# Richiesta con timeout
curl --max-time 3 http://api.example.com/delay?seconds=10

# Richiesta con output "non-buffered" (streaming)
curl -N 'http://llm.example.com/ask?q=What_is_DNS?'
```
