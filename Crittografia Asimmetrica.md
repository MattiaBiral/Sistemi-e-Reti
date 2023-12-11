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

La potenza è facilmente invertibile in aritmetica ordinaria con il logaritmo, ma il logaritmo in aritmetica modulare è computazionalmente molto difficile da calcolare

<code>a<sup>b</sup> mod m = c</code>: noti `a`, `m` e `c` non è facile calcolare `b`