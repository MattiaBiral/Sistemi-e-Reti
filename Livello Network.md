# Livello Network

## Internet Protocol version 4

Indirizzi a 32bit, 4 byte nel formato decimale 9.35.225.45  
Frammentazione e riassemblaggio  
Rilevazione degli errori

### Header
![IPv4 Header](./img/ipv4_header.png)
- IHL: Internet Header Length
- TOS: Type Of Service
- TTL: Time To Live
- Protocol: Transported Protocol

### Classes
- A  
  `0*******`, 255.0.0.0, 1 -> 127  
  Privata 10.0.0.0
- B  
  `10******`, 255.255.0.0, 128 -> 191  
  Private 172.16.0.0 -> 172.31.0.0
- C  
  `110*****`, 255.255.255.0, 192 -> 223  
  Privata 192.168.0.0
- Multicast  
  `1110****`, 224 -> 239
- Usi futuri  
  `1111****`, 240 -> 255

### Frammentazione

Diverse LAN, diversi Maximum Transmission Unit

---

## Address Resolution Protocol

Utilizzando la trasmissione broadcast delle LAN risolve gli indirizzi MAC dei rispettivi IPv4

1. Viene inviato in broadcast di livello 2 un pacchetto contenente l'indirizzo IPv4 di cui si cerca il corrispondente MAC -> **ARP Request**
2. L'host interessato risponde con il proprio indirizzo MAC -> **ARP Reply**

---

## Internet Protocol version 6

Indirizzi a 128bit, 16 byte nel formato esadecimale fe80:0000:0000:0000:0000:0000:0000:0001, che può essere abbreviato come fe80::1
Connectionless  
TTL -> hop count  
Subnetmask -> Classless Inter-Domain Routing  
Niente NAT, non c'è la necessità