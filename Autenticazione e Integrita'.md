# Autenticazione e Integrità

> ## Protocollo crittografico
>
> - Successione di operazioni
> - Per due o più partecipanti
> - Per ottenere uno o più obiettivi:
>   - Segretezza
>   - Integrità
>   - Autenticazione
>   - Non ripudio
>
> Attacchi:
> - Al protocollo
> - All'algoritmo implementato
> - Passivi
> - Attivi
>
> Possibili attacchi:
> - IP spoofing: utilizzo dell'indirizzo IP di un'altro dispositivo
> - Lettura delle password in chiaro e loro utilizzo
> - Replica dei messaggi (lettura e riutilizzo)
> - Man-in-the-middle
> - Hijacking

## Protocolli di autenticazione

### A chiave simmetrica

(<code>K<sub>AB</sub></code> deve essere precedentemente concordata)

- A -> B: "Sono A"
- B -> A: `R` (nonce)
- A -> B: <code>K<sub>AB</sub>(R)</code>

### A chiave asimmetrica

- A -> B: "Sono A"
- B -> A: `R` (nonce)
- A -> B: <code>d<sub>A</sub>(R)</code>
- opzionale:
  - B -> A: "<code>e<sub>A</sub></code>?"
  - A -> B: <code>e<sub>A</sub></code>

**Questo protocollo non è sicuro se non sono sicuro che <code>e<sub>A</sub></code> sia veramente la chiave pubblica di A**  
Per questo motivo è anche possibile un attacco Man-in-the-middle con l'autenticazione di due comunicazioni A-T e T-B, di questo A e B non si accorgerebbero

### Autenticazione basata su crittografia

Sarebbe pertanto necessario un meccanismo di **distribuzione sicura delle chiavi**:
- Chiave simmetrica: intermediario di fiducia, **Key Distribution Center**
- Chiave asimmetrica: **Certification Authority** che garantisca l'autenticità delle chiavi pubbliche