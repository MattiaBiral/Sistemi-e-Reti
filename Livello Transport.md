# Livello Transport

# Port number

E' l'indirizzo del livello di trasporto, identifica un processo  
E' un intero a 16bit, da 0 a 65535

| Porta       | Descrizione     |
|-------------|-----------------|
| 0-1023      | Well known      |
| 1024-49152  | Registered      |
| 49152-65535 | Dynamic/Private |

# User Datagram Protocol

Non affidabile, non connesso  
Trasmissione di tipo best effort

## Datagram

![Datagramma UDP](./img/udp_datagram.png)
- **Header**
  - **Source Port**: numero porta porcesso sorgente
  - **Destination Port**: numero porta processo destinatario
  - **UDP Length**: lughezza totale del datagramma
  - **Chechsum**: codice di controllo del datagramma
- **Data**: payload

La dimensione massima è 64KiB (dimensione massima pacchetto IP) - 20B (dimensione minima header IP) - 8B (dimensione header UDP) = 65508B

## Vantaggi

- Non richiede di stabilire una connessione
- Overhead minimo

## UDP-Lite

E' nato dalla necessità di non scartare i datagammi UDP errati comunque utili per alcune applicazioni come il VoIP o streaming audio/video  
Il campo **UDP Length** diventa **Checksum Coverage Length** e indica per quanti byte del pacchetto viene calcolato il checksum  
Di conseguenza la lunghezza del datagramma viene calcolata da quella del pacchetto IP

# Transmission Control Protocol

Connesso e affidabile  
Consegna tutti i byte e nell'ordine corretto, si parla di flusso di byte  
Full-duplex, si può trasmettere e ricevere dati contemporaneamente, anche grazie alle due differenti sequenze di riscontro

## Datagram

![Datagramma TCP](./img/tcp_datagram.png)
- **Header**
  - **Source Port**: numero porta porcesso sorgente
  - **Destination Port**: numero porta processo destinatario
  - **Sequence Number**: numero di sequenza progressivo del primo byte di dati
  - **Acknowledgement Number**: ha significato solo se il flag ACK è impostato a 1, è il prossimo sequence number atteso
  - **Data Offset** o **TCP Header Length**: numero di 32bit word dell'header
  - **Flags**
    - **URG**: una parte o tutto il pacchetto contiene dati urgenti, è necessario leggere l'urgent pointer
    - **ACK**: il campo **Acknowledgement Number** contiene un numero di riscontro
    - **PSH**: indica che i dati in arrivo devono essere passati subito al livello superiore
    - **RST**: a volte utilizzato per la chiusura forzata della connessione
    - **SYN**: il **Sequence Number** è da utilizzare come Initial Sequence Number
    - **FIN**: utilizzato per la chiusura della connessione
  - **Window Size**: utilizzato per comunicare il numero massimo di byte che si possono ancora ricevere
  - **Checksum**: codice di controllo del datagramma
  - **Urgent Pointer**: indica il numero dell'ultimo byte urgente nel campo **Data**
  - **Options**: da 0 a 10 32bit word
- **Data**: payload