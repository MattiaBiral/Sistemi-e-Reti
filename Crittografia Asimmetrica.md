# Crittografia Asimmetrica

> Tecnica del lucchetto unico: funzione facile da utilizzare ma difficile da invertire

## Aritmetica modulare

Simboli:
- `a mod m` indica il resto della divisione intera `a / m`
- `a ≡ b mod m` indica che `a mod m = b mod m`

### Proprietà

Somma:  
`[(a mod m) + (b mod m)] mod m = (a + b) mod m`

Differenza:  
`[(a mod m) – (b mod m)] mod m = (a – b) mod m`

Moltiplicazione:  
`[(a mod m) * (b mod m)] mod m = (a * b) mod m`

Potenza:  
<code>(a mod m)<sup>k</sup> mod m = a<sup>k</sup> mod m</code>

### Logaritmo discreto

La potenza è facilmente invertibile in aritmetica ordinaria con il logaritmo, ma **il logaritmo in aritmetica modulare è computazionalmente molto difficile da calcolare**

<code>a<sup>b</sup> mod m = c</code>: noti `a`, `m` e `c` non è facile calcolare `b`

## Algoritmo di Diffie-Hellman

**Non è un algoritmo crittografico**  
Attualmente si considera sicuro al 100% scegliendo:
- `p` primo con almeno 300 cifre
- `a` e `b` con almeno 100 cifre
- `g` molto piccolo

Procedimento:
- Alice e Bob scelgono due numeri **pubblici** `g` e `p`, con `p` numero **primo**
- Alice sceglie **segretamente** `a` e calcola <code>A = g<sup>a</sup> mod p</code> che invia a Bob
- Bob sceglie **segretamente** `b` e calcola <code>B = g<sup>b</sup> mod p</code> che invia ad Alice
- Alice calcola <code>k = B<sup>a</sup> mod p</code> (che corrisponde a <code>g<sup>b*a</sup> mod p</code>)
- Alice calcola <code>k = A<sup>b</sup> mod p</code> (che corrisponde a <code>g<sup>a*b</sup> mod p</code>)
- Entrambi hanno ora k, che non è facilmente ricavabile da un terzo anche avendo a disposizione `g`, `p`, `A` e `B` (logaritmo discreto)  
  Tuttavia non possono scegliere direttamente un `k` desiderato

## Algoritmi a chiave asimmetrica

Devono soddisfare la proprietà <code>d<sub>B</sub>(e<sub>B</sub>(p)) = p = e<sub>B</sub>(d<sub>B</sub>(p))</code>  
Dove:
- <code>e<sub>B</sub></code> è la chiave pubblica di Bob, diffusa al pubblico
- <code>d<sub>B</sub></code> è la chiave private di Bob, segreta
- `p` è il plaintext

Quindi:
- un messaggio cifrato con la chiave pubblica può essere decriptato solo con la chiave privata corrispondente -> **Sicurezza**
- un messaggio cifrato con la chiave privata può essere decriptato solo con la chiave pubblica corrispondente -> **Autenticazione**

**Da una chiave è computazionalmente impossibile derivare l'altra**

## Algoritmo Rivest Shamir Adleman

Si basa sul problema della fattorizzazione del prodotto di due numeri primi molto grandi (2048bit)

Due componenti:
- Algoritmo di generazione delle chiavi
- Algoritmo di cifratura e decifratura

### Generazione delle chiavi

- Vengono scelti due numeri primi `p` e `q` sufficientemente grandi
- Viene calcolato `n`, prodotto di `p` e `q`, chiamato **modulo**
- Viene calcolato `φ(n) = (p - 1) * (q - 1)`, chiamato **funzione toziente**
- Viene scelto `e` coprimo e minore di `φ(n)`, chiamato esponente pubblico
- Viene calcolato `d` con l'algoritmo di Euclide, tale che `d * e mod φ(n) ≡ 1`
- `(n, e)` costituisce la chiave pubblica
- `(n, d)` costituisce la chiave privata

E' impossibile riuscire a derivare una chiave conoscendo l'altra, in quanto sarebbe necessario fattorizzare il modulo per poter calcolare il toziente