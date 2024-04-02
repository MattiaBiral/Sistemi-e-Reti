# Sicurezza nell'infrastruttura di rete

# Virtual Private Network

Caratteristiche:
- **Sicurezza**: possono accedere solo le persone autorizzate
- **Riservatezza**: le informazioni non sono visibili dall'esterno
- **Trasparenza alle applicazioni**

Due modelli:
- **Overlay** o **Tunnel Mode**
- **Peers** o **Transport Mode**

## Overlay

Solo i router di frontiera sono a conoscenza dell'esistenza della VPN e devono essere configurati  
Realizzazione tramite tunneling: quando un router do frontiera riceve un pacchetto di livello 3 dall'interno lo imbusta in un altro pacchetto di livello 3 con destinatario un'altro router di frontiera che estrarrà il pacchetto contenuto

Problemi:
- **Overhead**
- Perdita della QoS
- Routing potenzialmente non efficiente

## Peers

Tutti i router della rete conoscono e gestiscono la VPN  
Routing classico

Problemi:
- Incompatibilità con piano di indirizzamento privato
- Maggiore difficoltà nel garantire la sicurezza (alcuni router potrebbero essere non controllati)

# IP Security

Famiglia di protocolli per funzionalità di sicurezza a livello 3

Due protocolli principali per la realizzazione di VPN:
- **Authentication Header**:
  - Autenticazione
  - Integrità
- **Encapsulation Security Payload**
  - Autenticazione
  - Integrità
  - Segretezza

Due protocolli per la gestione delle chiavi:
- **Internet Key Exchange**: per lo scambio delle chiavi
- **Internet Security Association and Key Management Protocol**: per instaurare, negoziare, modificare ed eliminare Security Association

### Security Association

Connessione simplex a livello 3 univocamente identificata nel **Security Association Database** da:
- **Security Parameter Index**: identificatore a 32 bit della SA
- **Indirizzo IP del destinatario**
- **Identificatore del protocollo di sicurezza**

## Authentication Header

| IP Header   | Authentication Header | Payload (TCP/UDP) |
| ----------- | --------------------- | ----------------- |
| Protocol 51 |                       | Authenticated     |

Authentication Header:
- Identificatore del protocollo trasportato
- Lunghezza dell'header
- **Security Parameter Index**
- **Integrity Chech Value** multiplo di 32 bit per IPv4 o di 64 bit per IPv6 contenente il **Message Authentication Code** del payload per verificarne l'integrità
- Numero di sequenza a 32 bit per difesa da attacchi di replica e man-in-the-middle

### Tunneling

| New IP Header | AH | IP Header | Payload (TCP/UDP) |
| ------------- | -- | --------- | ----------------- |

## Encapsulation Security Payload

| IP Header   | ESP Header | Payload (TCP/UDP) | ESP Trailer | ESP Authentication |
| ----------- | ---------- | ----------------- | ----------- | ------------------ |
| Protocol 50 |            <td colspan="2">Ecnrypted</td>    | MAC                |
|             <td colspan="3">Authenticated</td>             |                    |

ESP Header:
- **Security Parameter Index**
- Numero di sequenza a 32 bit

ESP Trailer:
- Padding
- Padding length
- Next Header

ESP Authentication
- **Integrity Check Value**

### Tunneling

| New IP Header | ESP Header | IP Header | Payload (TCP/UDP) | ESP Trailer | ESP Authentication |
| ------------- | ---------- | --------- | ----------------- | ----------- | -------------- |