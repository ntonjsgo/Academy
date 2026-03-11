# Analisi Comparativa – Shopify + App Custom vs MedusaJS Custom

**Data:** 11 marzo 2026
**Versione:** 1.0
**Branch:** feature/shopifyAnalysis
**Autore:** Team Synesthesia (PM Review)
**Progetto padre:** E-commerce Strategico e Automazione – Academy Tema Safety & Training

---

## 1. Premessa e Obiettivo

Questo documento risponde alla richiesta del Project Manager di valutare la fattibilità di sostituire lo stack **MedusaJS custom** con una soluzione basata su **Shopify** (piano Standard/Advanced) integrato con app di terze parti e automazioni native.

L'analisi è strutturata per requisito funzionale, seguendo la stessa suddivisione del Piano di Progetto principale. Per ciascun requisito viene assegnato un giudizio di fattibilità e un confronto stimato di effort.

---

## 2. Stack Shopify Proposto

| Layer                          | Strumento                               | Funzione                                                                     |
| ------------------------------ | --------------------------------------- | ---------------------------------------------------------------------------- |
| **Piattaforma**                | Shopify Advanced / Plus                 | Motore e-commerce, checkout, pagamento                                       |
| **Booking & Calendario**       | BTA (BookThatApp) o Cowlendar 2026      | Selezione edizioni corsi, gestione capienza, manual approval                 |
| **Form & Raccolta Dati**       | Pify Form Builder o FormCRM AI          | Form multi-step, campi condizionali, creazione Draft Order, upload documenti |
| **Validazione & Approvazione** | Approovly o Shopify Flow (nativo)       | Workflow approvazione segreteria, trigger link pagamento                     |
| **Automazioni**                | Shopify Flow                            | Scadenza Draft Order, reminder, gestione posti                               |
| **Storage Documenti**          | Google Drive / Dropbox via Zapier       | Vault documenti, link allegati all'ordine bozza                              |
| **Marketing Automation**       | Klaviyo                                 | Alert scadenze certificati, recupero carrello, cross-selling                 |
| **CMS Istituzionale**          | Shopify Pages + metafields o Contentful | Pagine Consulenza, Vigilanza, Dante 4.0                                      |
| **SEO**                        | Shopify SEO nativo + app (Schema App)   | JSON-LD, sitemap, meta tag                                                   |
| **Frontend Storefront**        | Liquid (tema custom) o Hydrogen (React) | UI/UX personalizzata                                                         |

---

## 3. Analisi per Requisito Funzionale

### 3.1 Flusso di Acquisto a Step Obbligati (PENDING_DOCS_REVIEW)

**Fattibilità: ⚠️ MEDIA (70%) — Richiede architettura workaround**

Il requisito fondamentale dell'Academy è che il pagamento avvenga **dopo** la validazione documentale, non prima. Questo inverte il flusso nativo di Shopify.

#### Approccio Shopify

Il meccanismo si basa sui **Draft Orders**:

1. L'utente compila un form multi-step (Pify / FormCRM AI) che bypassa il checkout classico.
2. Il form crea un Draft Order via Shopify API con i dati anagrafici e i link ai documenti.
3. La segreteria valida nell'admin e, se corretti, converte il Draft Order in invoice ("Invia fattura").
4. L'utente riceve l'email standard di Shopify con link al checkout sicuro.

#### Criticità

| Problema                                                                   | Impatto  | Mitigazione                                                                                             |
| -------------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------- |
| Checkout Shopify non è personalizzabile in profondità (salvo Shopify Plus) | 🔴 Alto  | Usare Draft Order come descritto, ma si perde la UI del checkout custom                                 |
| Scadenza 48h del link di pagamento non è nativa                            | 🟡 Medio | Shopify Flow + webhook custom per cancellare ordini non pagati                                          |
| Convalida sintattica CF / matricola gente di mare                          | 🟡 Medio | Logica Zod sostituita da validazione lato app form (Pify supporta regex)                                |
| Su Shopify Standard il checkout è bloccato (no customizations)             | 🔴 Alto  | Necessario **Shopify Advanced** (€354/mese) o **Shopify Plus** (€2.300/mese) per checkout extensibility |

#### Confronto Effort

| Metrica                       | MedusaJS Custom             | Shopify + App                                       |
| ----------------------------- | --------------------------- | --------------------------------------------------- |
| Sviluppo workflow             | 11–16 gg BE                 | 3–5 gg (configurazione app + Flow)                  |
| Personalizzazione UI checkout | 6–9 gg FE                   | 4–7 gg (Liquid custom o Hydrogen)                   |
| Costo ricorrente              | ~€80–200/mese (EC2 + infra) | €354–2.300/mese (piano Shopify) + €150–400/mese app |
| Flessibilità futura           | Illimitata                  | Vincolata alle API Shopify e ai ToS                 |

---

### 3.2 Gestione Documentale Avanzata (Vault Personale, Dashboard Validazione)

**Fattibilità: ⚠️ BASSA (50%) — Gap funzionale rilevante**

#### Approccio Shopify

- I documenti vengono caricati nel form (Pify) e salvati come link nelle note del Draft Order o pushati su Google Drive via Zapier.
- La segreteria entra nel Draft Order per visualizzare i link.
- **Non esiste un Vault Personale del Discente nativo**: ogni iscrizione è indipendente, i documenti non sono riutilizzabili tra ordini diversi.

#### Criticità

| Problema                                          | Impatto  | Soluzione                                                                  |
| ------------------------------------------------- | -------- | -------------------------------------------------------------------------- |
| Nessun Vault Personale nativo                     | 🔴 Alto  | Sviluppo app custom Shopify (Node.js embedded app) con storage S3 separato |
| Anteprima PDF nell'admin di Shopify               | 🔴 Alto  | Non supportata nativamente — i link aprono Drive/Dropbox esterno           |
| Badge contatore "Documenti da validare" real-time | 🔴 Alto  | Richiede app embedded custom con webhook                                   |
| Tracciamento scadenze documenti                   | 🔴 Alto  | Non esiste in nessuna app disponibile — sviluppo 100% custom               |
| Re-upload documenti con conservazione slot        | 🟡 Medio | Gestibile via note ordine bozza + notifica manuale                         |

**Nota critica:** Il Vault Personale e il tracciamento scadenze sono i differenziatori chiave del prodotto per l'Academy. L'assenza di questi moduli su Shopify obbliga a sviluppare una app embedded custom, annullando parte del risparmio teorico.

---

### 3.3 Motore di Prenotazione: Calendario, Capienza, Waitlist

**Fattibilità: ✅ BUONA (75%) — Dipende dalla scelta dell'app booking**

#### Approccio Shopify

**BTA (BookThatApp)** e **Cowlendar** sono le soluzioni più mature per Shopify:

| Feature                                    | BTA                                | Cowlendar     | MedusaJS Custom |
| ------------------------------------------ | ---------------------------------- | ------------- | --------------- |
| Calendario interattivo con date            | ✅                                 | ✅            | ✅ custom       |
| Capienza massima per sessione              | ✅                                 | ✅            | ✅              |
| Manual Approval (blocco senza pagamento)   | ✅ BTA Pro                         | ✅            | ✅              |
| Quorum minimo (X persone per conferma)     | ❌ non nativa                      | ❌ non nativa | ✅              |
| Lista d'attesa automatica                  | ✅ BTA                             | ⚠️ parziale   | ✅              |
| Notifica subentro da waitlist con link 48h | ⚠️ parziale                        | ❌            | ✅              |
| Early Bird pricing automatico              | ⚠️ via Shopify Scripts (Plus only) | ❌            | ✅              |
| Blackout dates manuali                     | ✅                                 | ✅            | ✅              |
| Vista calendario admin segreteria          | ✅                                 | ✅            | ✅ custom       |
| Export CSV partecipanti                    | ✅                                 | ⚠️ parziale   | ✅              |

#### Gap critici

- **Il quorum minimo** (attivazione sessione ad N partecipanti) non è gestito da nessuna app Shopify. Richiede una automazione Shopify Flow custom che controlli il conteggio degli ordini confermati per ogni edizione.
- **Early Bird pricing** richiede Shopify Plus (Shopify Scripts) o una app di pricing terza (Discountly, Bold Discounts) che aggiunge costo ricorrente.
- Il **sincronismo BTA ↔ Draft Order ↔ Booking** richiede una integrazione custom via webhook per mantenere il posto bloccato durante tutto il flusso documentale.

---

### 3.4 Automazione: Alert Scadenze Certificati, Cross-Selling, Carrello Abbandonato

**Fattibilità: ✅ BUONA (80%) — Klaviyo gestisce la maggior parte**

Questa è l'area dove Shopify + Klaviyo **supera** il MedusaJS custom per maturità delle funzionalità out-of-the-box.

| Funzione                                      | Shopify + Klaviyo                               | MedusaJS Custom         |
| --------------------------------------------- | ----------------------------------------------- | ----------------------- |
| Recupero carrello abbandonato                 | ✅ nativo Klaviyo                               | ✅ custom scheduled job |
| Alert scadenze certificati 90/60/30 gg        | ⚠️ custom (richiede campo scadenza nel profilo) | ✅ scheduled job        |
| Cross-selling formativo post-acquisto         | ✅ Klaviyo flows                                | ✅ custom               |
| Reminder pre-corso (7/3/1 gg prima)           | ✅ Klaviyo + metafield data corso               | ✅ custom               |
| Dashboard analytics corsi (tasso riempimento) | ❌ non nativa — serve app custom                | ✅ dashboard custom     |

#### Nota su alert scadenze

Le date di scadenza dei certificati non sono un campo nativo Shopify. Andrebbero salvate come **customer metafields** e lette da Klaviyo tramite API. La logica di "90 giorni prima della scadenza, invia email" è costruibile in Klaviyo come flow con trigger su data, ma richiede che la fonte dati (la data di scadenza) sia popolata correttamente — una responsabilità che ricade sul team di sviluppo con una app embedded custom.

---

### 3.5 Dashboard Operativa Segreteria

**Fattibilità: ⚠️ BASSA (45%) — L'admin di Shopify non è estendibile sufficientemente senza Shopify Plus**

L'admin di Shopify standard permette di vedere ordini e clienti, ma non è costruita per:

- Una **coda di validazione** con badge contatore real-time.
- **Anteprima PDF** inline dei documenti allegati.
- L'azione **Approva / Rifiuta** con causale predefinita.
- La selezione della **modalità pagamento** (intero vs acconto) al momento dell'approvazione.
- La **vista calendario** globale delle edizioni.

Per costruire queste funzionalità su Shopify è necessario sviluppare una **Shopify Embedded App** (Next.js + Shopify App Bridge + Polaris UI), che è essenzialmente un'applicazione web custom hostata esternamente e integrata nell'admin di Shopify tramite iframe. Questo equivale a costruire comunque un backend custom, con in più il vincolo delle API di Shopify e dei rate limit.

---

## 4. Analisi dei Costi Comparativa

### 4.1 Costi Ricorrenti Mensili

| Voce                           | MedusaJS su EC2   | Shopify Standard | Shopify Advanced | Shopify Plus    |
| ------------------------------ | ----------------- | ---------------- | ---------------- | --------------- |
| Piattaforma                    | — (open source)   | €36/mese         | €354/mese        | €2.300/mese     |
| Hosting (EC2 t3.medium + RDS)  | ~€80–150/mese     | Incluso          | Incluso          | Incluso         |
| BTA BookThatApp                | —                 | €19/mese         | €19/mese         | €19/mese        |
| Pify Form Builder / FormCRM AI | —                 | €15–49/mese      | €15–49/mese      | €15–49/mese     |
| Approovly Order Approvals      | —                 | €19/mese         | €19/mese         | €19/mese        |
| Klaviyo (fino a 10k contatti)  | ~€100/mese        | €100/mese        | €100/mese        | €100/mese       |
| Schema App (JSON-LD SEO)       | —                 | €19/mese         | €19/mese         | Incluso         |
| Storage documenti (S3 o Drive) | ~€5–20/mese       | ~€0 (Drive)      | ~€0 (Drive)      | ~€0 (Drive)     |
| **Totale mensile stimato**     | **€185–270/mese** | **€208/mese** ⚠️ | **€526/mese**    | **€2.553/mese** |

> **Nota:** Shopify Standard (€36/mese) non include il Checkout Extensibility necessario per il flusso custom. Senza Shopify Advanced, il form pre-ordine bypassa il checkout ma la UX è frammentata e meno professionale. Il piano **Advanced è il minimo realistico** per questo progetto.

> **Shopify Plus** diventa necessario se si vogliono: Shopify Scripts (early bird pricing nativo), checkout completamente brandizzato, SLA enterprise e API rate limit più alti.

### 4.2 Costi di Sviluppo (Effort)

| Work Package                    | MedusaJS 🤖 (con AI) | Shopify + App 🤖 (con AI)                  | Delta          |
| ------------------------------- | -------------------- | ------------------------------------------ | -------------- |
| Setup infra / piattaforma       | 2–4 gg               | 0.5–1 gg (Shopify gestisce)                | −2.5 gg        |
| Modulo Booking (core)           | 8–13 gg              | 2–4 gg (BTA config + Flow)                 | −7 gg          |
| Checkout multi-step + docs      | 7–11 gg              | 4–6 gg (Form Builder + Draft Order)        | −4 gg          |
| Dashboard admin segreteria      | 5–9 gg               | **8–14 gg** (Shopify Embedded App)         | **+7 gg**      |
| Gestione documentale / Vault    | 4–7 gg               | **7–12 gg** (tutto custom S3 + metafields) | **+6 gg**      |
| Marketing automation            | 3–5 gg               | 1–2 gg (Klaviyo flows prebuilt)            | −3 gg          |
| SEO tecnico JSON-LD             | 3–4 gg               | 2–3 gg (Schema App + liquid)               | −1.5 gg        |
| FE Storefront (Liquid/Hydrogen) | 25–40 gg             | 20–35 gg (tema base + custom)              | −6 gg          |
| Quorum minimo + Flow custom     | —                    | 3–5 gg                                     | +4 gg          |
| Testing & QA                    | 3–6 gg               | 3–6 gg                                     | =              |
| **TOTALE STIMATO**              | **60–99 gg**         | **50–88 gg**                               | **−10 gg ca.** |

> Il risparmio è modesto (~10%) e concentrato nell'infra e nel booking core. Viene **azzerato o invertito** se si include la Shopify Embedded App per la dashboard segreteria e il Vault documenti.

### 4.3 Scenario C — Shopify 100% Custom (Nessuna App di Terze Parti, AI-Assisted)

Questo scenario risponde alla domanda: **quanto costerebbe sostituire ogni app di terze parti con una app custom privata sviluppata internamente con agenti AI?**

Lo stack tecnico per ogni app custom è: **Remix (framework ufficiale Shopify) + Shopify App Bridge + Polaris UI** per le embedded app admin; **Shopify Theme Extensions + Hydrogen** per i componenti storefront; **AWS Lambda o Cloudflare Workers** per i job schedulati che Shopify Flow non copre nativamente.

Ogni app custom è hostata esternamente (es. Railway o Fly.io, ~€10–20/mese) e comunicano con Shopify via Admin API GraphQL e webhook.

---

#### App 1 — Booking Engine Custom (sostituisce BTA / Cowlendar)

**Funzioni da implementare:**

- Calendario interattivo edizioni con badge verde/giallo/rosso/grigio e contatore posti
- Capienza massima configurabile per sessione
- **Quorum minimo** con scheduled job di controllo (non disponibile in nessuna app nativa)
- Manual Approval: blocco del posto senza pagamento immediato
- Lista d'attesa con subentro automatico e link dedicato con scadenza 48h
- Blackout dates impostabili da admin
- Early Bird pricing (regola prezzo in funzione dei giorni di anticipo)
- Vista calendario globale segreteria nell'admin Shopify (Embedded App)

**Tech stack:** Remix + Shopify App Bridge + Polaris + Shopify Metaobjects (entità `CourseEdition`) + AWS Lambda (scheduled quorum check) + Shopify Admin API GraphQL

|                                                             | Ottimistico | Pessimistico |
| ----------------------------------------------------------- | ----------- | ------------ |
| **BE** (logica booking, API, Lambda) 🤖                     | 7 gg        | 12 gg        |
| **FE** (calendario interattivo storefront + vista admin) 🤖 | 6 gg        | 10 gg        |
| **Totale**                                                  | **13 gg**   | **22 gg**    |

**Risparmio mensile vs BTA Pro:** €39/mese — Costo sviluppo @€400/gg: **€5.200–€8.800** → **break-even: ~133–226 mesi** (non conveniente)

---

#### App 2 — Form Multi-Step + Document Upload (sostituisce Pify / FormCRM AI)

**Funzioni da implementare:**

- Form multi-step con logica condizionale per tipo corso (STCW vs Industriale vs Offshore)
- Validazione sintattica CF, matricola gente di mare, data di nascita (regex + Zod equivalente)
- Upload documenti direttamente su **AWS S3 proprio** (non Drive/Dropbox di terze parti — obbligatorio per GDPR su dati ministeriali)
- Creazione automatica Draft Order in Shopify via API con metafields anagrafici e link S3 allegati alle note ordine

**Tech stack:** Shopify Theme App Extension (React) + AWS S3 pre-signed URLs + Shopify Admin API (Draft Orders)

|                                                           | Ottimistico | Pessimistico |
| --------------------------------------------------------- | ----------- | ------------ |
| **BE** (S3 + Draft Order API + validazione) 🤖            | 4 gg        | 7 gg         |
| **FE** (stepper multi-step, drag&drop upload, preview) 🤖 | 4 gg        | 7 gg         |
| **Totale**                                                | **8 gg**    | **14 gg**    |

**Risparmio mensile vs Pify:** €49/mese — Costo sviluppo @€400/gg: **€3.200–€5.600** → **break-even: ~65–114 mesi** (non conveniente)

> ⚠️ Nota: questa app **è necessaria indipendentemente** dallo scenario B o C, perché Pify e FormCRM AI non supportano l'upload su S3 proprio. Il costo di sviluppo è quindi un overhead **inevitabile** anche nello Scenario B.

---

#### App 3 — Workflow Approvazione e Trigger Pagamento (sostituisce Approovly)

**Funzioni da implementare:**

- Coda "Documenti da Validare" nell'admin Shopify con badge contatore real-time (webhook da Draft Order)
- Interfaccia Approva / Rifiuta con causale predefinita selezionabile
- Selezione modalità pagamento al momento dell'approvazione (intero o acconto configurabile)
- Conversione Draft Order in Invoice → Shopify invia email standard con link pagamento
- Scheduled job per cancellazione Draft Order non pagati dopo 48h e liberazione posto nel booking engine

**Tech stack:** Remix + Shopify App Bridge + Polaris + webhook listener + Shopify Admin API

|                                                     | Ottimistico | Pessimistico |
| --------------------------------------------------- | ----------- | ------------ |
| **BE** (webhook, stato ordine, scheduled cancel) 🤖 | 3 gg        | 5 gg         |
| **FE** (coda admin, Approva/Rifiuta UI, badge) 🤖   | 4 gg        | 7 gg         |
| **Totale**                                          | **7 gg**    | **12 gg**    |

**Risparmio mensile vs Approovly:** €19/mese — Costo sviluppo @€400/gg: **€2.800–€4.800** → **break-even: ~147–253 mesi** (non conveniente)

---

#### App 4 — Vault Personale Discente + Tracciamento Scadenze (funzionalità non coperta da alcuna app)

**Funzioni da implementare:**

- Area "Documenti" nell'account cliente Shopify (Storefront Extension)
- Upload una-tantum di titoli e loro riutilizzo automatico nelle iscrizioni future
- Salvataggio date di scadenza come **Customer Metafields** su Shopify
- Scheduled job (Lambda) che legge i metafield scadenza e notifica il discente a 90/60/30 giorni

**Tech stack:** Shopify Customer Account Extension + S3 + Customer Metafields API + AWS Lambda (cron)

|                                               | Ottimistico | Pessimistico |
| --------------------------------------------- | ----------- | ------------ |
| **BE** (S3, metafields, Lambda cron) 🤖       | 4 gg        | 7 gg         |
| **FE** (vault UI, upload, status scadenze) 🤖 | 3 gg        | 6 gg         |
| **Totale**                                    | **7 gg**    | **13 gg**    |

**Risparmio mensile vs alternativa SaaS:** N/A (nessuna app copre questa funzione) — **sviluppo obbligatorio** anche nello Scenario B.

---

#### App 5 — Dashboard Analytics Corsi (funzionalità non coperta da alcuna app)

**Funzioni da implementare:**

- Dashboard nell'admin con tasso di riempimento per edizione
- Corsi più venduti, edizioni a rischio sotto-soglia
- KPI operativi per il team marketing interno

**Tech stack:** Remix + Polaris + Shopify Analytics API + aggregazioni su Metaobjects

|               | Ottimistico | Pessimistico |
| ------------- | ----------- | ------------ |
| **Totale** 🤖 | **3 gg**    | **5 gg**     |

---

#### App 6 — SEO JSON-LD Strutturato (sostituisce Schema App)

**Funzioni da implementare:**

- Dati strutturati `Course` e `EducationEvent` generati dinamicamente da metafields prodotto/edizione
- Tema Liquid con snippet JSON-LD iniettati nell'`<head>` di ogni scheda corso

**Tech stack:** Shopify Liquid + metafields — nessuna app necessaria, è sviluppo tema puro.

|               | Ottimistico | Pessimistico |
| ------------- | ----------- | ------------ |
| **Totale** 🤖 | **1 gg**    | **2 gg**     |

**Risparmio mensile vs Schema App:** €19/mese — Costo sviluppo @€400/gg: **€400–€800** → **break-even: ~21–42 mesi** — unica voce dove il custom è ragionevolmente sostenibile; tutte le altre app hanno break-even superiore ai 5 anni.

---

#### Totale Effort Scenario C — Shopify 100% Custom 🤖

| Work Package                                 | Ottimistico | Pessimistico |
| -------------------------------------------- | ----------- | ------------ |
| App 1 — Booking Engine custom                | 13 gg       | 22 gg        |
| App 2 — Form multi-step + S3 upload          | 8 gg        | 14 gg        |
| App 3 — Workflow approvazione                | 7 gg        | 12 gg        |
| App 4 — Vault Personale + scadenze           | 7 gg        | 13 gg        |
| App 5 — Dashboard analytics                  | 3 gg        | 5 gg         |
| App 6 — SEO JSON-LD (Liquid)                 | 1 gg        | 2 gg         |
| FE Storefront (Hydrogen / Liquid custom)     | 20 gg       | 35 gg        |
| Marketing automation (Klaviyo integration)   | 1 gg        | 2 gg         |
| Setup Shopify + hosting app (Railway/Fly.io) | 1 gg        | 2 gg         |
| Testing & QA                                 | 4 gg        | 7 gg         |
| **TOTALE SCENARIO C**                        | **65 gg**   | **114 gg**   |

---

### 4.4 Confronto Effort Tre Scenari (con AI)

| Scenario                          | Effort Min 🤖 | Effort Max 🤖 | Delta vs MedusaJS |
| --------------------------------- | ------------- | ------------- | ----------------- |
| **A — MedusaJS Custom**           | 60 gg         | 99 gg         | —                 |
| **B — Shopify + App Terze Parti** | 50 gg         | 88 gg         | −10 gg            |
| **C — Shopify 100% Custom**       | 65 gg         | 114 gg        | **+12 gg**        |

> Lo Scenario C è quello con il **maggior effort di sviluppo** in assoluto, perché si costruiscono da zero funzionalità (booking engine, vault, analytics) che su MedusaJS sono moduli nativi o già previsti nell'architettura. Su Shopify, ogni funzionalità non standard richiede di navigare i vincoli dell'API Shopify, il rate limiting, il sistema di metafields/metaobjects e il deployment separato di ciascuna app.

---

### 4.5 Costi Ricorrenti Scenario C

| Voce                                                       | Costo Mensile       |
| ---------------------------------------------------------- | ------------------- |
| Shopify Advanced                                           | €354/mese           |
| Hosting app custom (Railway / Fly.io, ~3 app)              | €30–60/mese         |
| AWS S3 (documenti) + Lambda (job schedulati)               | €15–30/mese         |
| Klaviyo (fino a 10k contatti)                              | €100/mese           |
| Shopify Advanced transaction fee (0,5% su gateway esterno) | ~€400–1.000/mese ⚠️ |
| **Totale (esclusa fee transazioni)**                       | **€499–544/mese**   |
| **Totale (inclusa fee stimata)**                           | **€899–1.544/mese** |

> La fee di transazione (0,5% su Shopify Advanced con Stripe/Nexi) rimane il **costo nascosto dominante** e non è eliminabile sviluppando le app in house: è una fee di piattaforma Shopify, rimovibile solo con Shopify Plus a €2.300/mese.

---

### 4.6 Total Cost of Ownership a 36 Mesi — Tutti e Tre gli Scenari

Stima conservativa: effort a costo giornaliero di riferimento **€400/gg** (1 senior dev con AI assist, interno); fee transazioni calcolate su 400 iscrizioni/mese × €250 ticket medio = €100.000/mese di transato.

| Voce                                             | Scenario A (MedusaJS) | Scenario B (Shopify + App) | Scenario C (Shopify Custom) |
| ------------------------------------------------ | --------------------- | -------------------------- | --------------------------- |
| **Costo sviluppo** (media ott/pess, @€400/gg)    | ~€31.800              | ~€27.600                   | ~€35.800                    |
| **Ricorrente 36 mesi** (escluse fee trans.)      | ~€8.460               | ~€18.936                   | ~€19.584                    |
| **Fee transazioni 36 mesi** (0,5% su €100k/mese) | €0                    | ~€21.600                   | ~€21.600                    |
| **TCO 36 mesi totale**                           | **~€40.260**          | **~€68.136**               | **~€77.084**                |

> I costi di sviluppo sono calcolati sulla media ottimistico/pessimistico. La fee transazioni è un limite inferiore reale: se il transato mensile supera €100k (obiettivo plausibile a regime), la differenza aumenta ulteriormente a favore di MedusaJS.

---

## 5. Matrice Rischi Shopify

| Rischio                                                                                                                                 | Probabilità | Impatto  | Mitigazione                                                                     |
| --------------------------------------------------------------------------------------------------------------------------------------- | ----------- | -------- | ------------------------------------------------------------------------------- |
| **Lock-in Shopify**: l'Academy è vincolata ai ToS, alle fee di transazione (0,5–2% per gateway esterni) e ai roadmap changes di Shopify | 🔴 Alta     | 🔴 Alto  | Accettare o migrare in futuro a costo rilevante                                 |
| **Transaction fee** su ogni vendita (0,5% su Advanced, 0,15% su Plus per gateway esterni come Stripe/Nexi)                              | 🔴 Alta     | 🟡 Medio | Usare Shopify Payments (non disponibile in Italia) o passare a Plus             |
| **Shopify Payments non disponibile in Italia**: obbliga a Stripe/Nexi con fee aggiuntiva su ogni ordine                                 | 🔴 Certa    | 🔴 Alto  | Solo Shopify Plus elimina la extra fee — costo di €2.300/mese                   |
| **API Rate Limit** (40 req/s su Advanced): problemi durante picchi di apertura iscrizioni                                               | 🟡 Media    | 🟡 Medio | Throttling e queue lato app embedded                                            |
| **Dipendenza app terze parti**: BTA, Pify, Approovly possono cambiare pricing, essere dismesse o rompere l'integrazione                 | 🟡 Media    | 🔴 Alto  | Scegliere app con storia pluriennale; clausola contrattuale con fornitore       |
| **Draft Order come backbone**: flusso fragile, documentazione Shopify ne sconsiglia l'uso per logiche di business complesse             | 🟡 Media    | 🔴 Alto  | Testare estensivamente; avere piano B                                           |
| **GDPR e documenti personali**: i file caricati finiscono su Drive/Dropbox di terze parti, non su storage controllato                   | 🔴 Alta     | 🔴 Alto  | Sviluppare app embedded con AWS S3 proprio — obbligatorio per dati ministeriali |
| **Scalabilità B2B (Fase 2)**: gestione acquisti collettivi aziendali su Shopify richiede app B2B (Shopify Plus nativo o SparkLayer)     | 🟡 Media    | 🟡 Medio | Budget aggiuntivo per Fase 2                                                    |

---

## 6. Raccomandazione

### Verdetto: ⚠️ SHOPIFY NON RACCOMANDATO per questo progetto

Il modello di business dell'Academy Tema Safety & Training è strutturalmente **incompatibile** con il paradigma Shopify per i seguenti motivi irrisolvibili:

1. **Shopify Payments non è disponibile in Italia.** Ogni transazione su gateway esterno (Stripe, Nexi) subisce una fee aggiuntiva dello 0,5% (Advanced) o 0,15% (Plus). Con un volume stimato di 400–500 iscrizioni/mese e un ticket medio di €200–400, la fee mensile ammonta a €400–1.000/mese **in aggiunta** al piano.

2. **Il flusso PENDING_DOCS_REVIEW non è nativo.** Il workaround dei Draft Orders è documentato da Shopify stesso come una soluzione temporanea soggetta a deprecazione nelle future versioni delle API. Costruire il core business su un workaround è un rischio operativo rilevante.

3. **Il Vault Personale e il tracciamento scadenze** richiedono comunque una app embedded custom con database e storage propri. Il costo di sviluppo per questa app da sola supera il risparmio teorico sull'infra.

4. **I dati ministeriali (documenti personali dei discenti) non possono risiedere su Google Drive/Dropbox di terze parti** senza una valutazione GDPR approfondita. La soluzione corretta — AWS S3 controllato — è uguale sia su MedusaJS sia su Shopify, azzerando il vantaggio di semplicità.

5. **Il costo totale di ownership su 36 mesi** (horizon progetto) è strutturalmente più alto con Shopify Advanced (€526/mese ricorrente vs €270/mese con infra EC2), e diventa proibitivo con Shopify Plus.

### Quando Shopify potrebbe essere valutato

Shopify diventa una scelta razionale **solo se**:

- Il flusso documentale viene ridotto a un semplice upload post-pagamento (rinunciando alla validazione preventiva).
- L'Academy accetta di non avere un Vault Personale del Discente nella Fase 1.
- Si accetta la dipendenza da 3–4 app di terze parti con contratti separati e rischio di disallineamento.
- Il budget mensile ricorrente può assorbire la fee di transazione + piano Advanced.

In assenza di queste condizioni, **lo stack MedusaJS custom rimane la scelta tecnicamente superiore e finanziariamente più conveniente su orizzonti di 18 mesi o più.**

---

## 7. Tavola Sinottica Finale

| Criterio                                          | Scenario A — MedusaJS Custom | Scenario B — Shopify + App Terze Parti | Scenario C — Shopify 100% Custom    |
| ------------------------------------------------- | ---------------------------- | -------------------------------------- | ----------------------------------- |
| **Effort sviluppo** (con AI assist)               | 60–99 gg                     | 50–88 gg                               | 65–114 gg                           |
| **Costo ricorrente mensile** (escluse fee trans.) | €185–270                     | €526                                   | €499–544                            |
| **Fee transazione Shopify**                       | ❌ N/A                       | +0,5% (Advanced) / +0,15% (Plus)       | +0,5% (Advanced)                    |
| **TCO 36 mesi stimato**                           | **~€40.260**                 | **~€68.136**                           | **~€77.084**                        |
| **Flusso PENDING_DOCS_REVIEW**                    | ✅ Nativo, macchina a stati  | ⚠️ Draft Order workaround              | ⚠️ Draft Order workaround           |
| **Vault Personale Discente**                      | ✅ Nativo nel design         | ❌ Dev custom obbligatorio             | ✅ App custom inclusa nello scope   |
| **Dashboard segreteria + PDF preview**            | ✅ Admin Medusa custom       | ❌ Dev embedded app obbligatorio       | ✅ App custom inclusa nello scope   |
| **GDPR S3 controllato per documenti**             | ✅                           | ⚠️ Dev aggiuntivo obbligatorio         | ✅ Incluso nelle app custom         |
| **Quorum minimo sessioni**                        | ✅ Scheduled job nativo      | ⚠️ Lambda esterno                      | ✅ Lambda custom incluso            |
| **Early Bird pricing**                            | ✅ Logica custom             | ❌ Solo Shopify Plus                   | ✅ App booking custom               |
| **Alert scadenze certificati**                    | ✅ Scheduled job nativo      | ⚠️ Metafield + Lambda custom           | ✅ Lambda custom incluso            |
| **Lock-in piattaforma**                           | ✅ Open source, portabile    | 🔴 Alto (Shopify + 3–4 vendor)         | 🔴 Alto (Shopify)                   |
| **Dipendenza vendor terze parti**                 | ✅ Nessuna                   | 🔴 Alta (BTA, Pify, Approovly…)        | ✅ Nessuna                          |
| **Scalabilità B2B (Fase 2)**                      | ✅ Modello dati già pronto   | ⚠️ Shopify Plus + app aggiuntiva       | ⚠️ Shopify Plus obbligatorio        |
| **Manutenibilità nel tempo**                      | ✅ Codebase unica            | ⚠️ Frammentata tra vendor              | ⚠️ 5–6 codebase da mantenere        |
| **Time-to-market**                                | Settembre 2026               | Settembre 2026                         | ⚠️ Rischio slittamento (più effort) |
| **Complessità operativa team**                    | Media (1 stack)              | Alta (multi-vendor)                    | Alta (multi-app+Shopify)            |

---

_Documento preparato per review interna Synesthesia — non distribuire al cliente senza validazione del PM._
