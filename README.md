<img src="img/logo.png" alt="" width="72" align="right">

# Moduli PrestaShop su misura

Moduli che ho sviluppato e mantengo in produzione su negozi e-commerce reali: ricambi per smartphone ed elettrodomestici, B2B e B2C, in più lingue, da PrestaShop 1.7.8 a 9.x.

**Stack:** PHP 7.4 – 8.2 · PrestaShop hook e Grid API · Symfony (event subscriber, servizi) · Smarty · JavaScript vanilla · MySQL · API esterne (Claude, Groq, ShippyPro)

> Il codice è in un repository privato perché è lavoro per clienti: posso mostrarlo durante un colloquio. Moduli pubblici: [prestashop-bo2fa](https://github.com/FilippoBonanno/prestashop-bo2fa) (2FA per il back office) e [prestashop-abwatermark](https://github.com/FilippoBonanno/prestashop-abwatermark) (filigrana immagini), più il caso di studio [abtecnology-prestashop](https://github.com/FilippoBonanno/abtecnology-prestashop).

## Sicurezza

### Anti-Bot Registration Shield
Protegge il modulo di registrazione clienti dai bot senza servizi esterni e senza cookie di terze parti:
- **ALTCHA** proof-of-work self-hosted (niente Google reCAPTCHA, niente chiavi API), con captcha matematico locale come riserva;
- honeypot invisibile, marker JavaScript e tempo minimo di compilazione;
- **rate limit** per IP ed email, blocco dei domini di email temporanee;
- log nel back office, con reset per cliente o IP.

### Verifica in due passaggi per il back office · [repo pubblico](https://github.com/FilippoBonanno/prestashop-bo2fa)
TOTP (RFC 6238) scritto senza librerie, controllo su ogni richiesta sia delle pagine legacy sia di quelle Symfony, codici di backup, dispositivi ricordati, obbligo per profilo.

## Ricerca

### AI Spare Parts Search
Ricerca ricambi per smartphone su un catalogo di circa 17.000 prodotti:
- in fase di indicizzazione un LLM a scelta (**Claude** o **Groq**) classifica ogni prodotto in JSON strutturato: tipo di componente, marca, modelli compatibili ("12/12 Pro" diventa `iphone_12`, `iphone_12_pro`);
- indice **full-text** MySQL proprio, aggiornato a ogni salvataggio o cancellazione di prodotto;
- autocomplete, paginazione, aggiunta al carrello direttamente dai risultati;
- l'interpretazione della domanda del cliente con lo stesso LLM è predisposta per la fase successiva.

### Groq AI Search Filter
Estende un modulo di ricerca commerciale (JoliSearch) con un **override del provider di ricerca**: i risultati passano da Groq che scarta quelli non pertinenti.

### Ricerca prodotto per ID o SKU
Barra di ricerca rapida per il personale e i clienti B2B: AJAX con token, scorciatoie per ID e riferimento. La versione 3 è riscritta per PrestaShop 9, PHP 8.2 e il tema Hummingbird.

## Vendite e ordini

### Prodotti suggeriti
Fino a due prodotti abbinati per scheda (per ID o SKU), separati per negozio in multistore, con prezzo e disponibilità della combinazione predefinita e aggiunta al carrello AJAX che aggiorna i componenti nativi del tema.

### Order Stats Filter
Statistiche degli ordini filtrate per data, stato e metodo di pagamento, con totali, percentuali e **confronto tra due giorni** con grafico.

### Corriere nella griglia ordini
Aggiunge la colonna "corriere", con filtro funzionante, alla griglia ordini del back office, usando la **Grid API** di PrestaShop (definizione della griglia e query builder).

### BRT FermoPoint → ShippyPro
Quando il cliente sceglie un punto di ritiro BRT, il modulo lo associa all'ordine in **ShippyPro** tramite API, così il corriere riceve l'ID del punto senza inserimenti a mano.

### Notifica cambio stato ordine
Email all'amministratore quando un ordine entra in uno stato da seguire (ad esempio un pagamento PayPal in attesa).

---
**Filippo Bonanno** · sviluppatore web (PHP, PrestaShop, Laravel, JavaScript)
