# STRATO · stratolabs.it

Sito ufficiale di **STRATO · Boutique Creative Digital Studio**.
Build statica + una Netlify Function per il form "Richiedi una Demo"
(integrazione Resend).

---

## 📂 Struttura della cartella

```
stratolabs-site/
├── index.html                    Entry point del sito
├── strato_single.html            Versione single-file portatile (HTML+CSS+JS+SVG inline)
├── 404.html                      Pagina di errore brand-coherent
├── idea-logo-clean.svg           Logo S-mark vettoriale (usato dall'intro + monument)
├── strato-layer-top.svg          Strato superiore (compat con bundle React)
├── strato-layer-bottom.svg       Strato inferiore (compat)
├── strato-paths-all.svg          Logo completo (compat)
├── static/
│   ├── css/   main.eac28141.css · strato-intro.css · strato-legal.css
│   └── js/    main.27d648b9.js  · strato-intro.js  · strato-legal.js · strato-contact.js
├── netlify/
│   └── functions/
│       └── contact.js            Serverless function per il form (Resend)
├── netlify.toml                  Build config + redirect /api/strato/contact
├── package.json                  Dipendenza `resend` per la Function
├── .env.example                  Esempio di variabili ambiente
├── .gitignore
└── README.md                     Questo file
```

---

## 🚀 Deploy su Netlify (con dominio stratolabs.it)

### 1. Push su GitHub
```bash
cd stratolabs-site
git init
git add .
git commit -m "STRATO · initial deploy"
git branch -M main
git remote add origin git@github.com:<tuo-user>/<repo>.git
git push -u origin main
```

### 2. Collega Netlify al repository
1. Vai su https://app.netlify.com/start e seleziona il repository GitHub.
2. Netlify rileverà `netlify.toml`:
   - **Publish directory**: `.`
   - **Build command**: *(vuoto — non serve)*
   - **Functions directory**: `netlify/functions`

### 3. Variabili d'ambiente
Site settings → **Environment variables** → aggiungi:

| Chiave                  | Valore                                 |
|-------------------------|----------------------------------------|
| `RESEND_API_KEY`        | la tua chiave `re_…` (Resend.com → API keys) |
| `SENDER_EMAIL`          | `onboarding@resend.dev` (default, finché il dominio non è verificato) |
| `DEMO_RECIPIENT_EMAIL`  | `strato.digitalab@gmail.com`           |

Quando avrai verificato `stratolabs.it` su Resend (DNS records SPF/DKIM/MX),
cambia `SENDER_EMAIL=demo@stratolabs.it`.

### 4. Collega il dominio stratolabs.it
1. Site settings → **Domain management** → **Add custom domain** → `stratolabs.it`.
2. Sul registrar dove hai comprato `stratolabs.it`:
   - **Opzione A — nameserver Netlify (consigliata)**: punta i nameserver
     a quelli mostrati da Netlify (`dns1.p01.nsone.net`, …). Netlify
     gestirà DNS + SSL automaticamente.
   - **Opzione B — record A/CNAME**: crea un record `A` su `@` puntato a
     `75.2.60.5` e un record `CNAME` su `www` puntato al subdomain
     Netlify (`tuo-sito.netlify.app`).
3. Netlify rilascia automaticamente certificato SSL (Let's Encrypt) entro
   pochi minuti.
4. In **Domain management → HTTPS** abilita *Force HTTPS*.

### 5. (Opzionale) verifica il dominio su Resend
1. Resend → **Domains** → Add domain → `stratolabs.it`.
2. Copia i 3 record DNS (SPF, DKIM, DMARC) e aggiungili sul tuo provider DNS.
3. Quando lo status passa a *Verified*, aggiorna su Netlify:
   `SENDER_EMAIL=demo@stratolabs.it` (o qualsiasi alias del tuo dominio).

---

## 🧪 Sviluppo locale

Servire il sito + le Netlify Functions in locale:
```bash
# Installa Netlify CLI una volta
npm i -g netlify-cli

# Dalla root del progetto
cp .env.example .env       # poi metti la tua RESEND_API_KEY dentro
netlify dev                # apre http://localhost:8888
```

In alternativa, solo statico (senza form):
```bash
python3 -m http.server 8000
# → http://localhost:8000
```

---

## 🛠️ Modifiche editoriali rapide

| Cosa vuoi cambiare                      | Dove                                                        |
|------------------------------------------|-------------------------------------------------------------|
| Email destinatario delle richieste demo  | Variabile `DEMO_RECIPIENT_EMAIL` su Netlify                |
| Sender Resend                            | Variabile `SENDER_EMAIL` su Netlify                        |
| Testo informativa privacy / titolare     | `static/js/strato-legal.js`                                |
| Timing/coreografia intro                 | `static/js/strato-intro.js` (sezione "Phase choreography") |
| Posizione/dimensioni monument hero       | Variabili CSS in `static/js/strato-intro.js` → `applyAnchor()` |
| Sezioni e copy del sito                  | `static/js/main.27d648b9.js` (bundle React precompilato)   |

---

## ✅ Cosa fa già funzionare il sito

- Intro cinematografica (void → emerge → settle → reveal → monument)
- Logo monument grande con glow soft volumetrico
- Cookie banner GDPR + modale privacy/cookie/imprint
- Form "Richiedi una Demo" → Resend (interno + conferma utente)
- Idempotency anti-doppi-invii (5 min, IP + email + messaggio)
- Patch runtime per asset SVG con path assoluti del bundle React
- Patch runtime per il link Instagram (→ @strato.digitalab)
- Logo fluttuante atmosferico **rimosso solo su mobile** (hero compatta)
- Sezione "Per Chi" pulita (solo settori positivi, niente esclusioni)
- 404 brand-coherent

## 🔮 Quando il dominio sarà attivo

Quando `stratolabs.it` punta a Netlify e la SSL è up:
1. Visita https://stratolabs.it
2. Compila il form "Richiedi una Demo"
3. Verifica che arrivino due email:
   - una a `strato.digitalab@gmail.com` con i dati del lead
   - una di conferma all'utente

Se qualcosa non funziona, controlla **Netlify → Functions → contact**:
nei log vedi ogni invocazione e ogni errore.

---

© STRATO · Boutique Creative Digital Studio
