# Livello Application

# Domain Name System

E' sia un **protocollo** di livello applicativo sia un **database distribuito**

## Domain Name

Stringa che **identifica una risorsa**, composta da parti separate da un punto (**dot-separated**)  
**Top-Level Domain**: parte più significativa, la prima partendo da destra  
Facile da ricordare, ma non dà alcuna informazione sulla collocazione dell'host nella rete

Il DNS crea corrispondenze tra DN e **valori**, non solo indirizzi IP  
**Forward lookup**: traduzione di nomi in indirizzi  
Utilizza la porta **UDP 63** (la replica dei dati utilizza la porta **TCP 53**)

## Alias

Host aliasing: un host può avere **uno o più alias**, un alias può corrispondere a **più canonical names** a seconda della zona e del protocollo

## Load Distribution

Il DNS può essere utilizzato per distribuire i carico tra server replicati, quindi **un hostname** può corrispondere ad una **lista di indirizzi IP**

## Database distribuito e gerarchico

Classi di server DNS:
- **Root Name Server**
- **Top-Level Domain Name Server**
- **Authoritative Name Server**

Interrogazione (`www.google.com`):
- L'host (se la ricerca in cache risulta in MISS) interroga il server DNS locale per risolvere
- Il server DNS locale interroga il Root Name Server per ottenere l'indirizzo del TLD Name Server di `com`
- Il server DNS locale interroga il TLD Name Server di `com` per ottenere l'indirizzo dell ANS di `google.com`
- Il server DNS locale interroga l'ANS di `google.com` per risolvere l'hoostname `www.google.com`
- Il server DNS locale risponde all'host con l'indirizzo di `www.google.com`

**DNS Iterator**: server DNS locale

**Zona**: insieme di record di risorsa

**Resource Record**:
- **Name** (es: `www.google.com`)
- **Time To Live**
- **Class** (es: `IN` -> Internet)
- **Type** (es: `A` -> IPv4 Address)
- **Value** (es: `158.110.1.2)

**Type**:
- **A**: IPv4 Address
- **AAAA**: IPv6 Address
- **MX**: Main eXchange
- **CNAME**: Canonical NAME
- **NS**: Name Server

## Messaggi DNS

### Header

| 16             | 16             |
|----------------|----------------|
| Transaction ID | Flags          |
| Questions      | Answer RRs     |
| Authority RRs  | Additional RRs |

- **Transaction ID**: numero identificativo che serve ad associare una richiesta con una risposta
- **Flags**:
  - **Query / Response**
  - **Opcode**
  - **Authoritative Answer**
  - **TrunCation**
  - **Recursion Desired**
  - **Recursion Available**
  - **Zero**
  - **rCode**
- **Questions**: numero di domande nella Question Section
- **Answer RRs**: numero di RR di risposta, `0` nei messaggi di query
- **Authority RRs**: numero di RR autoritativi, `0` nei messaggi di query
- **Additional RRs**: numero di RR aggiuntivi, `0` nei messaggi di query

### Body

- **Queries**:
  - **Name**
  - **Type**
  - **Class**
- **Answers**:
  - **Name**
  - **Type**
  - **Class**
  - **Time To Live**
  - **Value**