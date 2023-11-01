# Crittografia Simmetrica

## Sicurezza informatica

> Misure di carattere informativo, tecnologico ed organizzativo tese ad assicurare a ciascun utente e nessun altro i soli servizi previsti

> Protezione di informazioni e risorse ad accessi non autorizzati

Insieme di:
- **Riservatezza o Segretezza**: il contenuto del messaggio dovrebbe poter essere letto soltanto dagli autorizzati, segretezza anche del fatto stesso che la comunicazione sia avvenuta
- **Integrità**: il destinatario deve potersi accorgere di modifiche del messaggio
- **Autenticità o Autenticazione**: verifica dell'identità del mittente e del destinatario tramite l'acquisizione di dati che identificano univocamente un soggetto:
  - Qualcosa che si sa: password
  - Qualcosa che si ha: badge
  - QUalcosa che si è: impronta digitale
- **Disponibilità** del servizio

## Crittografia

Permette di ottenere la sicurezza delle informazioni  
Insieme alla crittoanalisi è una branca della crittologia  
Permette di ottenere:
- **Segretezza**
- **Integrità**
- **Autenticazione**
- **Non ripudio**

### Cifratura / Encryption

Plain Text + Encryption Algorithm + Key = Cypher Text
Testo in chiaro + Algoritmo di cifratura + Chiave = Testo cifrato

### Decifratura / Decryption

Cypher Text + Encryption Algorithm + Key = Plain Text
Testo cifrato + Algoritmo di cifratura + Chiave = Testo in chiaro

### Principio di Kerckhoffs

Un sistema crittografico deve essere sicuro anche se un attaccante dovesse conoscere tutti i dettagli del sistema tranne la chiave segreta

### Chiavi

- **Algoritmi a chiave simmetrica**: la stessa chiave è utilizzata per cifrare e decifrare  
  - La chiave deve essere scambiata in modo sicuro
  - Utilizzano funzioni matematiche molto semplici e molto efficienti
- **Algoritmi a chiave asimmetrica**: un paio di chiavi è generato, una si utilizza per la cifratura e l'altra per la decifratura:
  - Cifratura con chiave pubblica: segretezza
  - Cifratura con chiave privata: autenticazione

## Cifrari a trasposizione

Le lettere del messaggio non vengono sostituite ma ne viene alterata la posizione:
- Il testo cifrato è una permutazione del testo in chiaro
- La chiave è la permutazione stessa
- Un attacco esaustivo dovrebbe esplorare fino a `n!` permutazioni, con `n` il numero di caratteri

### Scitala lacedemonica

Su un nastro srotolato le lettere venivano trasposte in modo tale che solo l’adozione di un bastone identico (chiave) a quello usato per la scrittura del messaggio consentiva di ricostruire la posizione originaria delle lettere  
Caso particolare del cifrario Rail-Fence

### Cifrario Rail-Fence

Il testo in chiaro viene scritto in sequenze diagonali e viene letto per le linee orizzontali

`ciaoatutti` -> `caautiotti`  
`c-a-a-u-t-`  
`-i-o-t-t-i`

### Cifrario Route

Il testo in chiaro viene scritto per righe in una tabella (matrice), le cui dimensioni sono la chiave, e letto per colonne

`ciaoatutti` -> `catittauiotx`  
`ciao`  
`atut`  
`ttix`

La complessità potrebbe essere aumentata aggiungendo un'ulteriore cifratura con una tabella di dimensione diversa o permutando le colonne

## Cifrari a sostituzione (a chiave simmetrica)

Le lettere del messaggio in chiaro non vengono spostate ma sostutuite con altre lettere  
La sostituzione deve essere reversibile  
Per ogni elemento la chiave determina l'elemento che dev'essere usato per sostituirlo  
Massima sicurezza se len(key) >= len(message)  

Due tipologie di cifrari:
- Monoalfabetici: schema di sostituzione fisso
- Polialfabetici: schema di sostutuzione dipendente dalla posizione della lettera e/o dalle altre lettere del messaggio

### Cifrario di Cesare

Scorrimento fisso circolare dell'alfabeto  

e<sub>k</sub>(p<sub>i</sub>) = C<sub>i</sub> = (p<sub>i</sub> + k) mod 26  
d<sub>k</sub>(C<sub>i</sub>) = p<sub>i</sub> = (C<sub>i</sub> - k) mod 26

Solo `26` possibili chiavi

### Cifrario di Cesare generalizzato

Ogni lettera viene sostituita con un'altra lettera, la chiave è la tabella di corrispondenza tra l'alfabeto in chiaro e l'alfabeto cifrato  
Ci sono `26!` possibili chiavi  
Attacco esaustivo praticamente impossibile  
Con la crittoanalisi statistica delle ricorrenze si può decifrare facilmente

> La crittoanalisi statistica analizza le frequenze con cui compaiono le lettere, in ogni lingua ogni lettara ha una frequenza caratteristica che non viene alterata dalla cifratura monoalfabetica

### Cifrario di Vigenère

La chiave viene ripetuta fino a raggiungere una lunghezza pari a quella del testo in chiaro  
Si utilizza una lettera del testo in chiaro (che seleziona la colonna) e la lettera corrispondente della chiave (che seleziona la riga) per trovare in carattere cifrato

Debolezze:
- Ripetizione della chiave: sequenze identiche di caratteri ad una certa distanza tra loro potrebbero corrispondere ad un multiplo della lunghezza della chiave
- Ogni volta che la chiave si ripete le lettere vengono cifrate con gli stessi cifrari monoalfabetici

> Test di Friedman o K-test stima la lunghezza della chiave

### Cifrario Playfair

Il testo viene diviso in digrammi  
La chiave viene scritta senza ripetizioni di lettere in una tabella 5x5 che viene poi rimpita con le lettere rimanenti  
Se il digramma contiene due lettere uguali si spezza e si riempie con una `x` o un carattere di riempimento  
Per ogni digramma:
- Se le due lettere si trovano sulla stessa riga: si sostituisce con la lettera a destra
- Se le due lettere si trovano sulla stessa colonna: si sostituisce con la lettera sotto
- Negli altri casi: si sostituisce con la lettera nella stessa riga che corrisponde alla colonna dell'altra lettera

Per decifrare la procedura si applica al contrario

Vantaggi:
- Ogni lettera può essere cifrata in modi diversi a seconda del digramma
- 25 x 24 possibili digrammi, attacco statistico più complesso