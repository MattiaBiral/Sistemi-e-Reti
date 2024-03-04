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

# File Transfer Protocol

## Server Ports

**Control**: TCP 21  
**Data**: TCP 20

## Modes

### Normal

- Client Control -> Server Control: \<Data Port\>
- Server Control -> Client Control: OK
- Server Data -> Client Data: Data Channel
- Client Data -> Server Data: OK

### Passive

- Client Control -> Server Control: Passive
- Server Control -> Client Control: OK, \<Data Port\>
- Client Data -> Server Data: Data Channel
- Server Data -> Client Data: OK

A causa del NAT/PAT la normal mode non funziona, deve essere il client ad aprire le connessioni, perciò si utilizza la passive mode

## Commands

- **USER \<username\>**: comunica il nome utente
- **PASS \<password>**: comunica la password
- **LIST [filename/directory]**: fornisce informazioni sul file se specificato, altrimenti lista i contenuti della cartella corrente o di quella specificata
- **RETR \<filename\>**: riceve una copia del file
- **STOR \<filename\>**: invia un file al server
- **CWD \<directory\>**: cambia la working directory
- **QUIT**: disconnette

## Reply Codes

- `2..`: il comando ha avuto successo
- `4..` o `5..`: il comando è fallito
- `1..` o `3..`: errore o risposta incompleta
- `.0.`: sintassi
- `.1.`: informazione
- `.2.`: connessione
- `.3.`: autenticazione
- `.4.`: non definito
- `.5.`: filesystem

# Email Protocols

**Email Address**: username@domain
**Mail User Agent**: client di posta
**Mail Transport Agent**: server di posta
**Modalità di accesso**:
- **POP mail**: utilizzando un client di posta
- **Web mail**: dove il webserver è il client di posta

### Multipurpose Internet Mail Extensions

Permette l'inserimento di file audio/video e altro specificando il tipo di contenuto

Header di un documendo MIME:
- **MIME version**
- **Content-Transfer-Encoding**
- **Content-Type**
- **Content-ID**
- **Content-Description**

## Simple Mail Transfer Protocol

**Server Port**: TCP 25

Protocollo per l'invio di posta elettronica

### Commands

- **HELO \<domain\>**: comunica il dominio del proprio server di posta
- **MAIL FROM:\<username@domain\>**: inizia la composizione di un'email con mittente l'indirizzo specificato
- **RCPT TO:\<username@domain\>**: indica un destinatario dell'email
- **DATA**: inizia la composizione del corpo del messaggio
  - **From: ["\<username\>"] \<\<username@domain\>\>**: header che indica il mittente
  - **To: ["\<username\>"] \<\<username@domain\>\>**: header che indica un destinatario
  - **Cc: ["\<username\>"] \<\<username@domain\>\>**: header che indica un mittente in copia carbone
  - **Bcc: ["\<username\>"] \<\<username@domain\>\>**: header che indica un mittente in copia carbone nascosta
  - **Date: \<datetime\>**: header che indica la data e ora
  - **Reply to: ["\<username\>"] \<\<username@domain\>\>**: header che indica a chi si sta rispondendo
  - **Subject: \<subject\>**: header che indica l'oggetto
- **QUIT**: chiude la connessione

> Esempio:
> - S: `220 smtp.example.com ESMTP Postfix`
> - C: `HELO relay.example.org`
> - S: `250 Hello relay.example.org, I am glad to meet you`
> - C: `MAIL FROM:<bob@example.org>`
> - S: `250 Ok`
> - C: `RCPT TO:<alice@example.com>`
> - S: `250 Ok`
> - C: `RCPT TO:<theboss@example.com>`
> - S: `250 Ok`
> - C: `DATA`
> - S: `354 End data with <CR><LF>.<CR><LF>`
> - C: `From: "Bob Example" <bob@example.org>`
> - C: `To: "Alice Example" <alice@example.com>`
> - C: `Cc: theboss@example.com`
> - C: `Date: Tue, 15 Jan 2008 16:02:43 -0500`
> - C: `Subject: Test message`
> - C:
> - C: `Hello Alice.`
> - C: `This is a test message with 5 header fields and 4 lines in the message body.`
> - C: `Your friend,`
> - C: `Bob`
> - C: `.`
> - S: `250 Ok: queued as 12345`
> - C: `QUIT`
> - S: `221 Bye`

## Post Office Protocol

**Server Port**: TCP 110

Protocollo per la lettura della posta elettronica  
Quando l'utente si connette scarica in automatico tutte le email presenti con gli allegati e svuota la casella postale sul server  
Non organizza la casella in cartelle sul server

### Fasi

- **Authorization**: il client si identifica e il server verifica le autorizzazioni di accesso alla casella postale
- **Transaction**: la posta viene scaricata
- **Update**: il server elimina tutti i messaggi scaricati

### Commands

- **USER \<username@domain\>**: comunica il proprio indirizzo
- **PASS \<password\>**: comunica la propria password
- **STAT**: richiede informazioni sullo stato della casella postale
- **LIST**: richiede informazioni su uno o tutti i messaggi in casella
- **RETR \<n\>**: scarica il messaggio specificato
- **DELE \<n\>**: marca il messaggio come da eliminare
- **QUIT**: chiude la connessione

## Internet Message Access Protocol

**Server Port**: TCP 143

Protocollo per la lettura della posta elettronica e gestione della casella postale

### Funzionalità aggiuntuve rispetto a POP

- Rinominazione della casella
- Eliminazione dei singoli messaggi senza doverli scaricare
- Leggere le intestazioni dei messaggi
- Leggere solo porzioni dei messaggi
- Effettuare l'accesso simultaneo