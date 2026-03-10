# Stime di Progetto – Academy E-commerce

## Stack: EC2 AWS · MedusaJS · Next.js · PayloadCMS · Algolia / MeiliSearch

**Data:** 10 marzo 2026
**Versione:** 1.0
**Progetto padre:** E-commerce Strategico e Automazione – Academy Tema Safety & Training

---

## 1. Metodologia e Assunzioni

### Team di Riferimento

| Parametro           | Dettaglio                                                                                                                                                                       |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Team Umano**      | 1–2 senior dev full-stack (BE + FE) senza assistenza AI attiva                                                                                                                  |
| **Team AI**         | Stesso team equipaggiato con **Cursor + Claude Sonnet / GPT-4o** come copilota attivo su ogni work package                                                                      |
| **Giornata tipo**   | 8 ore produttive, escludendo meeting, stand-up, review commerciali                                                                                                              |
| **Unità di misura** | Giorni-persona (gg) nel formato **ottimistico–pessimistico**                                                                                                                    |
| **Parallelismo**    | Le stime sono in giorni-persona singoli. Con 2 dev in parallelo applicare un **fattore di compressione ~1.6x** (non 2x: overhead di coordinamento, code review, merge conflict) |

### Riduzione Stimata con Agenti AI

| Tipologia di Lavoro                                         | Riduzione Stimata |
| ----------------------------------------------------------- | ----------------- |
| Boilerplate, scaffolding, CRUD, configurazioni              | 45–55%            |
| Logica di business complessa (workflow, scheduling)         | 20–30%            |
| Frontend componenti UI standard                             | 35–50%            |
| Frontend logica custom (calendario interattivo, multi-step) | 20–30%            |
| Testing, script di ingestion, SEO markup                    | 35–45%            |

> **Nota motore di ricerca:** Le righe **Algolia** e **MeiliSearch** sono alternative mutuamente esclusive. Vanno valutate e scelta una sola prima dell'ingresso in Fase Sviluppo (giugno). Algolia = hosted/SaaS, integrazione più rapida, costo mensile ricorrente. MeiliSearch = self-hosted su EC2, nessun costo ricorrente, ~1–2 gg aggiuntivi di setup infra.

> **Dipendenza esterna critica:** La riga _Preparazione dati sorgente_ è a carico dell'Academy (non del team di sviluppo). Un ritardo nella fornitura dei dati corsi impatta direttamente l'ingestion e lo slot di agosto.

---

## 2. Fase 1 — Core (Go-Live Settembre 2026)

| #   | Work Package                                                                                                                                                                       | Milestone | BE 👤    | FE 👤   | TOT 👤        | BE 🤖  | FE 🤖  | TOT 🤖        |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | -------- | ------- | ------------- | ------ | ------ | ------------- |
| 1   | **Infra & DevOps** — EC2, Nginx, SSL, ambienti staging/prod, CI/CD pipeline (GitHub Actions)                                                                                       | Apr–Mag   | 3–5 gg   | 0–1 gg  | **3–6 gg**    | 2–3 gg | 0–1 gg | **2–4 gg**    |
| 2   | **MedusaJS core** — installazione, data modeling custom (corsi, edizioni, booking, documenti), plugin structure                                                                    | Apr–Mag   | 4–7 gg   | —       | **4–7 gg**    | 2–4 gg | —      | **2–4 gg**    |
| 3   | **PayloadCMS setup** — installazione, modellazione contenuti istituzionali (Consulenza, Vigilanza, Dante 4.0, Brand Portfolio, News)                                               | Apr–Mag   | 2–4 gg   | 1–2 gg  | **3–6 gg**    | 1–2 gg | 1–2 gg | **2–4 gg**    |
| 4   | **Integrazione PayloadCMS → Next.js** — API routes, ISR/SSR content pages, preview mode, sezione Brand Portfolio                                                                   | Giu–Lug   | 2–3 gg   | 2–4 gg  | **4–7 gg**    | 1–2 gg | 1–2 gg | **2–4 gg**    |
| 5   | **Modulo Booking custom** ⚠️ — entità `CourseEdition` / `Booking` / `Waitlist`, scheduled jobs, workflow conferma quorum, rimborso scaglionato, blackout dates, early bird pricing | Giu–Lug   | 10–15 gg | 5–8 gg  | **15–23 gg**  | 6–9 gg | 3–5 gg | **9–14 gg**   |
| 6   | **Checkout multi-step** — raccolta anagrafica ministeriale, upload documenti S3, validazione Zod, integrazione gateway pagamento (carta, bonifico, rateale)                        | Giu–Lug   | 5–8 gg   | 4–6 gg  | **9–14 gg**   | 3–5 gg | 2–4 gg | **5–9 gg**    |
| 7   | **Dashboard admin segreteria** — calendario edizioni, validazione documenti (approvazione / richiesta nuovo upload), export CSV registri, gestione quorum manuale                  | Giu–Lug   | 3–5 gg   | 4–7 gg  | **7–12 gg**   | 2–3 gg | 2–4 gg | **4–7 gg**    |
| 8   | **Next.js storefront** — catalogo corsi (filtri per settore), schede corso con calendario interattivo, carrello, account discente (prenotazioni, vault documenti)                  | Giu–Lug   | 2–4 gg   | 8–12 gg | **10–16 gg**  | 1–2 gg | 5–8 gg | **6–10 gg**   |
| 9a  | **Ricerca — Algolia** _(opzione A: hosted)_ — indexing prodotti/edizioni, InstantSearch UI, relevance tuning, filtri                                                               | Lug–Ago   | 2–3 gg   | 1–2 gg  | **3–5 gg**    | 1–2 gg | 1 gg   | **2–3 gg**    |
| 9b  | **Ricerca — MeiliSearch** _(opzione B: self-hosted su EC2)_ — installazione, configurazione indici, sync job con Medusa, UI search                                                 | Lug–Ago   | 3–5 gg   | 1–2 gg  | **4–7 gg**    | 2–3 gg | 1–2 gg | **3–5 gg**    |
| 10  | **Marketing automation** — alert scadenze certificati (90/60/30 gg), recupero carrello abbandonato, reminder pre-corso, cross-selling formativo                                    | Lug–Ago   | 4–6 gg   | 1–2 gg  | **5–8 gg**    | 2–4 gg | 1 gg   | **3–5 gg**    |
| 11  | **SEO tecnico** — JSON-LD `Course` / `EducationEvent`, sitemap XML, SSR ottimizzato, Core Web Vitals, hreflang IT/EN (struttura)                                                   | Lug–Ago   | 1–2 gg   | 3–5 gg  | **4–7 gg**    | 1 gg   | 2–3 gg | **3–4 gg**    |
| 12  | **Preparazione dati sorgente** ⛔ _dipendenza esterna Academy_ — normalizzazione, categorizzazione e consegna dati 500 corsi in formato strutturato                                | Lug       | —        | —       | **5–10 gg\*** | —      | —      | **5–10 gg\*** |
| 13  | **Ingestion catalogo 500 corsi** — script import, QA dati, ottimizzazione SEO schede, associazione requisiti documentali per categoria                                             | Lug–Ago   | 3–5 gg   | 0–1 gg  | **3–6 gg**    | 2–3 gg | 0–1 gg | **2–4 gg**    |
| 14  | **Testing & QA / UAT** — test di carico (afflusso simultaneo), test E2E (Playwright), UAT con staff Academy su flussi segreteria                                                   | Ago       | 3–5 gg   | 2–4 gg  | **5–9 gg**    | 2–3 gg | 1–3 gg | **3–6 gg**    |

_\* Giorni stimati lato Academy, non imputabili al team di sviluppo. Il valore non è incluso nei totali._

---

### Totali Fase 1

> Totali calcolati con **Algolia (opzione A)** come motore di ricerca. Per MeiliSearch aggiungere 1–2 gg (BE).

|                            | BE 👤 | FE 👤 | **TOT 👤** | BE 🤖 | FE 🤖 | **TOT 🤖** | Risparmio AI |
| -------------------------- | ----- | ----- | ---------- | ----- | ----- | ---------- | ------------ |
| **Minimo (ottimistico)**   | 39 gg | 31 gg | **70 gg**  | 23 gg | 19 gg | **43 gg**  | ~38%         |
| **Massimo (pessimistico)** | 65 gg | 54 gg | **119 gg** | 37 gg | 34 gg | **71 gg**  | ~40%         |

#### Con 2 Dev in Parallelo (fattore compressione 1.6x)

|                        | Team Umano | Team AI    |
| ---------------------- | ---------- | ---------- |
| **Minimo calendario**  | ~44 giorni | ~27 giorni |
| **Massimo calendario** | ~74 giorni | ~44 giorni |

> Con go-live target a **settembre 2026** e kick-off aprile, la finestra disponibile è ~22 settimane (~110 giorni lavorativi). Il piano è compatibile in entrambi gli scenari, con margine di buffer maggiore nel caso AI.

---

## 3. Fase 2 — Opzionali (Post Go-Live, da Ottobre 2026)

> Questi moduli non sono bloccanti per il lancio di settembre. Possono essere schedulati in parallelo al periodo di stabilizzazione post-lancio o pianificati per Q1 2027.

| #    | Work Package                                                                                                                                     | BE 👤        | FE 👤        | TOT 👤       | BE 🤖        | FE 🤖        | TOT 🤖       |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | ------------ | ------------ | ------------ | ------------ | ------------ |
| F2-1 | **B2B multi-seat** — aziende che iscrivono N dipendenti in una transazione, gestione ordini collettivi, ruoli aziendali                          | 4–7 gg       | 3–5 gg       | **7–12 gg**  | 2–4 gg       | 2–3 gg       | **4–7 gg**   |
| F2-2 | **Integrazione Mago ERP** — sync ordini, clienti, fatturazione verso gestionale esistente, riconciliazione automatica                            | 8–14 gg      | 1–2 gg       | **9–16 gg**  | 5–9 gg       | 1–2 gg       | **6–11 gg**  |
| F2-3 | **OCR automatico scadenze documenti** — estrazione automatica date di scadenza da PDF/immagini caricati nel Vault discente                       | 4–7 gg       | 1–2 gg       | **5–9 gg**   | 2–4 gg       | 1–2 gg       | **3–6 gg**   |
| F2-4 | **Chatbot AI orientamento corsi** — assistente conversazionale per guidare il discente nella scelta del percorso formativo e rispondere alle FAQ | 3–6 gg       | 3–5 gg       | **6–11 gg**  | 2–4 gg       | 2–3 gg       | **4–7 gg**   |
| F2-5 | **Bilingue IT/EN completo** — traduzioni schede corso, email templates, gestione hreflang, disclaimer lingua formazione in italiano              | 1–2 gg       | 4–7 gg       | **5–9 gg**   | 1–2 gg       | 2–4 gg       | **3–6 gg**   |
| F2-6 | **Export iCal / Google Calendar** — link di aggiunta evento post-iscrizione per il discente, sync automatico                                     | 1–2 gg       | 1–2 gg       | **2–4 gg**   | 1 gg         | 1–2 gg       | **2–3 gg**   |
| F2-7 | **Download attestati digitali** — generazione PDF attestato post-corso nell'area personale del discente                                          | 2–4 gg       | 2–4 gg       | **4–8 gg**   | 1–2 gg       | 1–3 gg       | **2–5 gg**   |
|      | **Totale Fase 2**                                                                                                                                | **23–42 gg** | **15–27 gg** | **38–69 gg** | **14–26 gg** | **10–19 gg** | **24–45 gg** |

---

## 4. Riepilogo Generale

| Fase                   | TOT 👤         | TOT 🤖        |
| ---------------------- | -------------- | ------------- |
| **Fase 1 — Core**      | 70–119 gg      | 43–71 gg      |
| **Fase 2 — Opzionali** | 38–69 gg       | 24–45 gg      |
| **Totale complessivo** | **108–188 gg** | **67–116 gg** |

---

## 5. Rischi di Stima

| #   | Rischio                                                                                                                                                                                                                                                                   | Work Package Impattato               | Fattore di Variabilità                                                                       |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------- |
| R1  | **Complessità Modulo Booking** — il lavoro più critico dell'intero progetto. I workflow di conferma quorum, rimborso scaglionato e lista d'attesa sono logicamente interdipendenti e difficili da testare in isolamento. Un bug in produzione può bloccare vendite reali. | #5 Booking                           | **Alto** — è il singolo blocco che può far slittare l'intero calendario                      |
| R2  | **Qualità dati sorgente Academy** — se i 500 corsi non vengono forniti in formato strutturato (CSV/JSON pulito con categorie, requisiti documentali, durate), il tempo di normalizzazione può triplicare.                                                                 | #12 Preparazione dati, #13 Ingestion | **Alto** — dipendenza esterna non controllabile dal team                                     |
| R3  | **Scelta motore di ricerca** — ritardare la decisione Algolia vs MeiliSearch oltre giugno blocca l'architettura infra e il setup degli indici, con effetto cascata su storefront e admin.                                                                                 | #9a / #9b Ricerca                    | **Medio** — bloccante se non risolto prima del kick-off sviluppo                             |
| R4  | **Integrazione gateway pagamento** — bonifico con riconciliazione via webhook e pagamento rateale aggiungono complessità di testing non immediata da stimare a priori.                                                                                                    | #6 Checkout                          | **Medio** — dipende dal provider scelto e dalla qualità della documentazione API             |
| R5  | **UAT con staff Academy** — sessioni di User Acceptance Testing con operatori non tecnici possono generare cicli di revisione UX imprevisti, soprattutto sul flusso admin documenti.                                                                                      | #14 Testing & QA                     | **Medio** — mitigabile pianificando l'UAT con almeno 3 settimane di buffer prima del go-live |
