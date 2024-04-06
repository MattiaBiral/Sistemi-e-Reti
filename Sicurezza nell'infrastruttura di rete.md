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

<table>
  <thead>
    <tr>
      <th>IP Header</th>
      <th>ESP Header</th>
      <th>Payload (TCP/UDP)</th>
      <th>ESP Trailer</th>
      <th>ESP Authentication</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="2">Protocol 50</td>
      <td></td>
      <td colspan="2">Encrypted</td>
      <td rowspan="2">MAC</td>
    </tr>
    <tr>
      <td colspan="3">Authenticated</td>
    </tr>
  </tbody>
</table>

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

# Secure Socket Layer/Transport Layer Security

Utilizza TCP aggiungendo riservatezza e integrità dei dati e autenticazione di client e server  
Può essere utilizzato da qualsiasi applicazione che usa TCP  
Fornisce un'API che forniscono l'interfaccia socket SSL allo sviluppatore

**SSL**: originariamente proposto da Netscape

**TLS**: basato su SSL e standardizzato

Il browser contiene una lista di CA fidate, autentica il server prima dell'invio di dati  
E' possibile anche l'autenticazione del client

Le informazioni scambiate all'interno di una sessione SSL/TLS sono cifrate con algoritmi a chiave simmetrica, le chiavi simmetriche sono scambiate tramite la crittografia a chiave pubblica

## Handshake

- Client -> Server: SYN
- Server -> Client: SYN + ACK
- Client -> Server: ACK, SSL Hello
- Server -> Client: Certificate, Key Length, Algorithms, Nonce
- Client -> Server: E<sub>S</sub>(PMS)

Client e server concordano gli algoritmi da utilizzare nella fase di handshake:
- Nell'Hello il client specifica una **lista di algoritmi** supportati
- Il server ne sceglie **uno a chiave simmetrica e uno a chiave pubblica** e lo comunica insiame al **certificato** (che contiene la sua chiave pubblica per l'algoritmo scelto), alla **lunghezza della chiave** e ad un **nonce**
- Il client **verifica il certificato**, estrae la chiave pubblica del server e genera un **Pre Master Secret**, lo cifra con la chiave pubblica del server e lo invia
- Client e server derivano il **Master Secret** dal PMS e dal nonce scelto dal server e lo usano per **generare le due chiavi di cifratura e i due Message Authentication Code** (uno per direzione di comunicazione)
- Client e server calcolano l'hash di tutti i messaggi della fase di handshake e inviano il MAC per verificare che non siano avvenute manomissioni

## Derivazione delle chiavi

Dal Master Secret vengono derivati:
- K<sub>S</sub>: chiave di sessione per cifrare il traffico dal server al client
- M<sub>S</sub>: Message Authentication Code per autenticare il traffico dal server al client
- K<sub>C</sub>: chiave di sessione per cifrare il traffico dal client al server
- M<sub>C</sub>: Message Authentication Code per autenticare il traffico dal client al server

## Record

Il flusso di dati viene suddiviso in Record, che sono numerati (il numero di sequenza non è incluso nel record), autenticati e poi cifrati

<table>
  <thead>
    <tr>
      <th>Type</th>
      <th>Version</th>
      <th>Length</th>
      <th>Payload</th>
      <th>HMAC</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Indica se il record è di handshake, dati applicativi o chiusione della connessione SSL/TLS</td>
      <td></td>
      <td></td>
      <td colspan="2">Encrypted</td>
    </tr>
    <tr>
      <td colspan="4">Authenticated</td>
      <td></td>
    </tr>
  </tbody>
</table>

# Firewall

Combinazione di hardware e software che permette di separare una rete privata dall'esterno e consente di controllare e gestire il flusso del traffico con l'esterno  
Se non è progettato o installato correttamente può essere violato  
Processo attivo su un sistema in grado di controllare il contenuto dei pacchetti al fine di bloccare il traffico non autorizzato  
Collega una rete **trust** (amministrata) ad una considerata non sicura  

- **Personal Firewall**: sofware eseguito su un'host che controlla il traffico sulla scheda di rete di un singolo computer
- **Perimeter Firewall**: software eseguito su router di frontiera, che hanno accesso alla rete pubblica
- **Hardware Firewall**: costituiti da un apparato hardware apposito, può anche svolgere la funzione di router
- **Application-layer Firewall**: firewall perimetrali applicativi di livello 7 che vengono eseguiti su un server con più interfacce di rete e con applicazioni di rete specifiche per il controllo delle connessioni e l'ottimizzazione del traffico (proxy)

## Filtraggio di pacchetto

Analizza l'header di livello 3 e 4 di ciascun pacchetto in modo indipendente  
Blocca o lascia transitare i pacchetti in base alle regole definite  
Disponibile anche sulla maggior parte dei router in commercio  
Non consente l'autenticazione degli utenti

Regole generalmente basate su:
- Indirizzo IP sorgente e destinatario
- Protocollo
- Porte sorgente e destinazione
- Flag TCP
- Tipo di messaggio ICMP
- Regole diverse in entrata e in uscita
- Regole diverse per le diverse interfaccie del router

## Filtraggio con memoria di stato

**Stateful inspection**

Effettua un'analisi in tempo reale dello stato dei flussi, registrando l'inizio e la fine di ogni connessione  
Intercetta violazioni dei parametri di protocollo  
Ispeziona anche i dati a livello applicativo

## Intrusioni

- **IP Spoofing**: il firewall dovrebbe bloccare pacchetti con indirizzi privati da e verso una rete pubblica
- **Denial of Service**
- **IP Smurfing**: il firewall dovrebbe bloccare ICMP Echo Request con destinazione broadcast
- **Traceroute**: il firewall dovrebbe bloccare tutti i messaggi ICMP di tipo Destination Unreachable

# Application Gateway

Specifico server attraverso cui tutti i dati delle applicazioni sono vincolati a passare, applica politiche in base ai dati applicativi  
Collega due segmenti di rete operando a livello applicativo  
Permette l'autenticazione degli utenti  
Maschera l'origine del colleamento operando come procuratore, **proxy**  
Il filtraggio dovrebbe bloccare tutti i collegamenti tranne quelli con l'indirizzo dell' AG come IP destinazione

## Policy di accesso

Possono essere:
- Fascie orarie
- IP black list
- Keyword list
- Tipo di dati scambiati
- Personalizzate per utenti o gruppi

# DeMilitarized Zone

Zona smilitarizzata  
Porzione di rete che si trova tra la rete protetta e Internet  
I servizi accessibili dall'esterno vengono collocati in DMZ, i server che li hostano sono detti **bastion host**, sono gli unici visibili ed attaccabili, poiche la rete interna è protetta da un ulteriore firewall