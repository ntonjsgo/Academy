# Analisi Funzionale – Modulo Booking per Academy Tema Safety & Training

### _Replica funzionale di YITH Booking for WooCommerce su stack MedusaJS + Next.js_

**Data:** 9 marzo 2026  
**Versione:** 1.0  
**Progetto padre:** E-commerce Strategico e Automazione – Academy Tema Safety & Training

---

## 1. Obiettivo del Documento

Il presente documento definisce i requisiti funzionali, le entità dati, i flussi operativi e l'architettura tecnica necessari per implementare un sistema di prenotazione corsi nativo su MedusaJS, equivalente funzionale di **YITH Booking for WooCommerce**.

Poiché l'Academy opera interamente in presenza a Taranto con sessioni a calendario fisso, il modulo di booking è il nucleo operativo dell'intero e-commerce. Ogni vendita è, sostanzialmente, una prenotazione di un posto in una sessione con una data e una capienza.

---

## 2. Analisi di YITH Booking for WooCommerce – Feature Mapping

Di seguito l'inventario delle funzionalità di YITH Booking, classificate per priorità di implementazione nel contesto Academy.

| #   | Feature YITH Booking                          | Priorità Academy | Note di adattamento                                       |
| --- | --------------------------------------------- | ---------------- | --------------------------------------------------------- |
| 1   | Prodotto prenotabile con date disponibili     | 🔴 Critica       | Ogni corso = prodotto, ogni edizione = slot prenotabile   |
| 2   | Calendario interattivo per selezione data     | 🔴 Critica       | Sostituisce selezione variante generica                   |
| 3   | Gestione capienza massima per slot            | 🔴 Critica       | Max posti = quorum minimo 20 + max aula                   |
| 4   | Stato disponibilità in tempo reale            | 🔴 Critica       | Posti liberi, quasi pieno, esaurito                       |
| 5   | Quorum minimo per conferma                    | 🔴 Critica       | 20 partecipanti per attivazione sessione                  |
| 6   | Workflow di conferma / cancellazione          | 🔴 Critica       | Auto-conferma a soglia, alert sotto-soglia                |
| 7   | Gestione buffer / black-out dates             | 🟡 Alta          | Date bloccate per manutenzione campo prove                |
| 8   | Politica di cancellazione con rimborso        | 🟡 Alta          | Scaglioni: 100% entro N giorni, 50% entro M giorni        |
| 9   | Prezzi differenziati per periodo (early bird) | 🟡 Alta          | Sconto iscrizione anticipata                              |
| 10  | Extra / Add-on prenotabili                    | 🟡 Alta          | Navetta, pernottamento convenzionato, materiale didattico |
| 11  | Lista d'attesa (waitlist)                     | 🟡 Alta          | Subentro automatico in caso di cancellazione              |
| 12  | Calendario admin (vista globale sessioni)     | 🟡 Alta          | Dashboard operativa per la segreteria                     |
| 13  | Blocco manuale posti da admin                 | 🟠 Media         | Riserva posti per gruppi aziendali (Fase 1 parziale)      |
| 14  | Prenotazioni multiple nello stesso ordine     | 🟠 Media         | Un'azienda iscrive N dipendenti in una transazione        |
| 15  | Reminder automatici pre-corso                 | 🟠 Media         | Email/SMS 7gg, 3gg, giorno prima                          |
| 16  | Integrazione Google Calendar / iCal           | 🟢 Bassa         | Export evento per il discente                             |
| 17  | Rapporti e statistiche prenotazioni           | 🟢 Bassa         | Tasso riempimento per sessione, trend per corso           |

---

## 3. Modello dei Dati

### 3.1 Entità Principali

Il modulo introduce tre nuove entità nel database MedusaJS, collegate al `Product` e all'`Order` esistenti.

```
┌─────────────────────────────────────────────────────────────────────┐
│                         PRODUCT (corso)                             │
│  id, title, description, category, required_documents[], ...        │
└───────────────────────────────┬─────────────────────────────────────┘
                                │ 1:N
┌───────────────────────────────▼─────────────────────────────────────┐
│                    COURSE_EDITION (edizione/slot)                   │
│  id                  UUID                                           │
│  product_id          FK → Product                                   │
│  start_date          DATETIME                                       │
│  end_date            DATETIME                                       │
│  location            STRING  (default: "Taranto – Campo Prove")     │
│  max_capacity        INT     (es. 30)                               │
│  min_threshold       INT     (default: 20)                          │
│  current_bookings    INT     (calcolato, non stored)                │
│  status              ENUM    [DRAFT, OPEN, CONFIRMED,               │
│                               CANCELLED, COMPLETED]                 │
│  price_overrides[]   JSONB   (early bird, listino standard)         │
│  blackout            BOOL    (blocco manuale admin)                 │
│  instructor_ids[]    UUID[]  FK → Staff                             │
│  notes               TEXT                                           │
│  created_at          DATETIME                                       │
│  updated_at          DATETIME                                       │
└───────────────────────────────┬─────────────────────────────────────┘
                                │ 1:N
┌───────────────────────────────▼─────────────────────────────────────┐
│                        BOOKING (prenotazione)                       │
│  id                  UUID                                           │
│  edition_id          FK → CourseEdition                             │
│  order_id            FK → Order (Medusa)                            │
│  customer_id         FK → Customer (Medusa)                         │
│  status              ENUM    [PENDING, CONFIRMED, WAITLISTED,       │
│                               CANCELLED, ATTENDED, NO_SHOW]         │
│  seats               INT     (default: 1, multi per B2B)           │
│  addons[]            JSONB   (navetta, pernottamento, ...)          │
│  cancellation_date   DATETIME NULL                                  │
│  refund_amount       DECIMAL NULL                                   │
│  attended            BOOL    NULL (registrato post-corso)           │
│  created_at          DATETIME                                       │
│  updated_at          DATETIME                                       │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                          WAITLIST_ENTRY                             │
│  id                  UUID                                           │
│  edition_id          FK → CourseEdition                             │
│  customer_id         FK → Customer                                  │
│  position            INT     (ordine di priorità)                  │
│  notified_at         DATETIME NULL                                  │
│  expires_at          DATETIME NULL (finestra di accettazione 48h)   │
│  created_at          DATETIME                                       │
└─────────────────────────────────────────────────────────────────────┘
```

### 3.2 Entità di Supporto

```
┌─────────────────────────────────────────────────────────────────────┐
│                     BOOKING_ADDON_DEFINITION                        │
│  id, name, description, price, available_for_course_ids[]          │
│  Esempi: "Navetta A/R da Taranto CS" (€15),                        │
│          "Kit materiale didattico aggiuntivo" (€25)                │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                    CANCELLATION_POLICY                              │
│  id, product_id (NULL = default globale)                           │
│  rules[]: { days_before: INT, refund_percent: INT }                │
│  Esempio Academy:                                                   │
│    > 14 gg → rimborso 100%                                         │
│    7–14 gg → rimborso 50%                                          │
│    < 7 gg  → rimborso 0%                                           │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                      PRICE_RULE (early bird)                        │
│  id, edition_id, label, discount_percent, valid_until              │
│  Esempio: "Iscrizione anticipata" → -10% se acquisto > 30gg prima  │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 4. Flussi Operativi

### 4.1 Flusso Utente – Prenotazione Corso

```
[1] Utente naviga catalogo corsi
        │
        ▼
[2] Scheda corso → Sezione "Prossime Edizioni"
    Calendario interattivo mostra:
    🟢 Verde  = posti disponibili (con numero rimanente)
    🟡 Giallo = quasi pieno (< 5 posti)
    🔴 Rosso  = esaurito / lista d'attesa
    ⬛ Grigio  = data passata o blackout
        │
        ▼
[3] Selezione data edizione
    → Sistema verifica disponibilità in tempo reale
    → Se ESAURITO: opzione "Mettimi in lista d'attesa"
        │
        ▼
[4] Configurazione prenotazione
    ├─ Numero posti (default 1, max limitato per B2C)
    ├─ Selezione add-on disponibili (navetta, kit, ecc.)
    └─ Visualizzazione prezzo finale (con eventuale sconto early bird)
        │
        ▼
[5] Checkout Step 1 – Anagrafica e documenti
    (flusso già definito nel Piano Progetto padre)
        │
        ▼
[6] Checkout Step 2 – Pagamento
    → Al successo del pagamento:
       • Creazione record BOOKING (status: PENDING)
       • Decremento posti disponibili in CourseEdition
       • Email di conferma prenotazione (con riepilogo edizione)
        │
        ▼
[7] Monitoraggio soglia (Scheduled Job ogni 30 min)
    ├─ IF bookings >= min_threshold AND status = OPEN
    │     → status = CONFIRMED
    │     → Notifica massiva a tutti i partecipanti
    │     → Notifica istruttori
    └─ IF start_date - 7gg AND bookings < min_threshold
          → Alert direzione per decisione manuale
```

### 4.2 Flusso Admin – Creazione Edizione

```
[1] Admin → "Gestione Edizioni" → "Nuova Edizione"
        │
        ▼
[2] Form di creazione:
    • Selezione corso (Product)
    • Data/ora inizio e fine
    • Capienza massima
    • Soglia minima (pre-compilata a 20, modificabile)
    • Istruttori assegnati
    • Regola di prezzo (standard / early bird)
    • Note interne
        │
        ▼
[3] Salvataggio → status = DRAFT
        │
        ▼
[4] Admin pubblica → status = OPEN
    → Edizione visibile nel calendario storefront
```

### 4.3 Flusso Lista d'Attesa

```
[Slot ESAURITO]
        │
        ▼
[Utente clicca "Lista d'attesa"]
    → Creazione WAITLIST_ENTRY con position = N+1
    → Email conferma posizione in lista
        │
        ▼
[Cancellazione di un booking esistente]
        │
        ▼
[Scheduled Job detecta posto liberato]
    → Recupera primo WAITLIST_ENTRY (position = 1)
    → Invia email con link esclusivo di prenotazione
    → expires_at = now + 48 ore
        │
        ├─ IF utente prenota entro 48h → BOOKING confermato
        │     → Notifica agli altri in lista: "Posto non più disponibile"
        └─ IF timeout → si avanza al prossimo in lista (position = 2)
```

### 4.4 Flusso Cancellazione con Rimborso

```
[Utente richiede cancellazione dall'area personale]
        │
        ▼
[Sistema calcola giorni rimanenti all'inizio corso]
    → Applica CancellationPolicy associata al corso
    → Mostra rimborso previsto prima di confermare
        │
        ▼
[Utente conferma cancellazione]
        │
        ▼
[Medusa Workflow: compensation transaction]
    ├─ BOOKING.status = CANCELLED
    ├─ CourseEdition.current_bookings -= seats
    ├─ Rimborso via gateway originale
    ├─ Email conferma cancellazione con importo rimborsato
    └─ Trigger lista d'attesa (se applicabile)
```

---

## 5. Interfaccia Storefront (Next.js)

### 5.1 Componente Calendario Edizioni

Il componente centrale del modulo è un **calendario interattivo** da posizionare nella scheda prodotto corso, sotto la descrizione e sopra il CTA di acquisto.

**Comportamento:**

- Visualizzazione mensile (default) con switch a vista settimanale
- Ogni giorno con un'edizione mostra un badge colorato (verde/giallo/rosso/grigio)
- Click su un giorno apre un pannello laterale (drawer) con:
  - Orario inizio/fine
  - Posti disponibili: `X / MAX` con barra di riempimento visiva
  - Prezzo (con badge "Early Bird" se applicabile)
  - Pulsante "Prenota questa edizione" → aggiunge al carrello con `edition_id` come metadata
- Su mobile: vista a lista verticale per settimana corrente + 4 settimane successive

**Dati esposti (API pubblica):**

```json
GET /store/courses/{product_id}/editions?from=2026-03-01&to=2026-05-31

Response:
[
  {
    "id": "ced_01ABC",
    "start_date": "2026-03-25T08:00:00Z",
    "end_date": "2026-03-27T17:00:00Z",
    "status": "OPEN",
    "available_seats": 8,
    "max_capacity": 30,
    "price": 450.00,
    "early_bird_price": 405.00,
    "early_bird_valid_until": "2026-03-10T23:59:59Z",
    "addons": [...]
  }
]
```

### 5.2 Area Personale Discente – "Le Mie Prenotazioni"

Sezione dedicata nell'account utente che mostra:

| Sezione              | Contenuto                                                                        |
| -------------------- | -------------------------------------------------------------------------------- |
| **Prossimi corsi**   | Lista prenotazioni con stato (Confermato / In attesa di quorum / Lista d'attesa) |
| **Corsi completati** | Storico con possibilità di scaricare l'attestato (Fase 2)                        |
| **Cancellazioni**    | Pulsante cancella con anteprima rimborso, solo entro policy                      |
| **Lista d'attesa**   | Posizione corrente, stima tempi, opzione di uscita dalla lista                   |

---

## 6. Interfaccia Admin (Medusa Dashboard)

### 6.1 Sezione "Gestione Calendario"

Vista calendario operativa per la segreteria con:

- **Filtri:** per corso, per istruttore, per stato (OPEN / CONFIRMED / CANCELLED)
- **Vista Kanban** per stato edizione (alternativa alla vista calendario)
- **Card edizione** mostrante: nome corso, data, `bookings / max_capacity`, lista partecipanti scaricabile
- **Azioni rapide:**
  - Conferma manuale edizione (override quorum)
  - Blocco iscrizioni (blackout)
  - Spostamento sessione (con notifica automatica agli iscritti)
  - Cancellazione massiva con rimborso collettivo

### 6.2 Dettaglio Edizione

Pagina drill-down su singola edizione con:

- Elenco prenotazioni con stato di ogni booking
- Stato documenti per ogni discente (link alla gestione documentale)
- Sezione "Lista d'attesa" con posizioni e date di inserimento
- Log eventi dell'edizione (chi ha prenotato/cancellato/quando)
- Esportazione CSV lista partecipanti per registro d'esame ministeriale

### 6.3 Configurazione Add-on e Policy

Pannello dedicato (Settings → Booking) per:

- Creazione e modifica add-on (nome, prezzo, disponibilità per corso)
- Definizione policy di cancellazione globale e per singolo corso
- Configurazione regole early bird (percentuale sconto, deadline)
- Impostazione soglia minima globale (default 20, override per corso)

---

## 7. Sistema di Notifiche

| Trigger                                                | Destinatario                    | Canale      | Template                  |
| ------------------------------------------------------ | ------------------------------- | ----------- | ------------------------- |
| Prenotazione ricevuta                                  | Discente                        | Email       | `booking_confirmed`       |
| Edizione confermata (quorum raggiunto)                 | Tutti gli iscritti + istruttori | Email       | `edition_confirmed`       |
| Edizione a rischio cancellazione (< 7gg, sotto soglia) | Direzione                       | Email       | `edition_at_risk`         |
| Edizione cancellata dall'admin                         | Tutti gli iscritti              | Email + SMS | `edition_cancelled`       |
| Posto disponibile (da waitlist)                        | Primo in lista                  | Email       | `waitlist_seat_available` |
| Cancellazione da utente                                | Discente                        | Email       | `booking_cancelled`       |
| Reminder pre-corso 7 giorni                            | Discente                        | Email       | `reminder_7d`             |
| Reminder pre-corso 1 giorno                            | Discente                        | Email + SMS | `reminder_1d`             |
| Spostamento data sessione                              | Tutti gli iscritti              | Email       | `edition_rescheduled`     |

Tutti i template utilizzano il `NotificationProvider` di MedusaJS (es. SendGrid per email, Twilio per SMS), già pianificato nel progetto padre.

---

## 8. API Endpoints – Riepilogo

### Store API (pubblica, autenticata per azioni)

| Metodo   | Endpoint                      | Descrizione                                 |
| -------- | ----------------------------- | ------------------------------------------- |
| `GET`    | `/store/courses/:id/editions` | Lista edizioni con disponibilità            |
| `POST`   | `/store/bookings`             | Crea prenotazione (richiede autenticazione) |
| `GET`    | `/store/bookings`             | Prenotazioni del cliente corrente           |
| `DELETE` | `/store/bookings/:id`         | Cancella prenotazione (applica policy)      |
| `POST`   | `/store/waitlist`             | Aggiungi a lista d'attesa                   |
| `DELETE` | `/store/waitlist/:id`         | Esci dalla lista d'attesa                   |

### Admin API (riservata)

| Metodo   | Endpoint                         | Descrizione                                |
| -------- | -------------------------------- | ------------------------------------------ |
| `GET`    | `/admin/editions`                | Lista tutte le edizioni                    |
| `POST`   | `/admin/editions`                | Crea nuova edizione                        |
| `PATCH`  | `/admin/editions/:id`            | Modifica edizione (data, stato, capienza)  |
| `DELETE` | `/admin/editions/:id`            | Cancella edizione con rimborsi             |
| `GET`    | `/admin/editions/:id/bookings`   | Lista prenotazioni per edizione            |
| `PATCH`  | `/admin/bookings/:id`            | Modifica stato booking (check-in, no-show) |
| `POST`   | `/admin/editions/:id/confirm`    | Conferma manuale override quorum           |
| `POST`   | `/admin/editions/:id/reschedule` | Sposta data con notifica automatica        |
| `GET`    | `/admin/editions/:id/export`     | Export CSV partecipanti                    |

---

## 9. Scheduled Jobs (Background Tasks)

| Job                            | Frequenza             | Logica                                                                           |
| ------------------------------ | --------------------- | -------------------------------------------------------------------------------- |
| `check-edition-thresholds`     | Ogni 30 minuti        | Verifica se edizioni OPEN hanno raggiunto min_threshold → auto-CONFIRMED         |
| `at-risk-editions-alert`       | Ogni giorno ore 09:00 | Notifica direzione per edizioni che iniziano in < 7 giorni ancora sotto soglia   |
| `waitlist-expiry-check`        | Ogni ora              | Scade offerte waitlist non accettate entro 48h, avanza alla posizione successiva |
| `pre-course-reminders`         | Ogni giorno ore 08:00 | Invia reminder a discenti con corsi nei prossimi 7 e 1 giorno                    |
| `edition-status-auto-complete` | Ogni giorno ore 23:00 | Marca come COMPLETED le edizioni la cui `end_date` è passata                     |

---

## 10. Regole di Business Critiche

1. **Atomicità della prenotazione:** La creazione del booking e il decremento del posto disponibile devono avvenire in una singola transazione database per evitare overbooking in condizioni di alta concorrenza (race condition). Usare `SELECT FOR UPDATE` sul record `CourseEdition`.

2. **Impedimento iscrizione se documenti scaduti:** Prima di creare il booking, il sistema verifica che tutti i documenti obbligatori del discente (già caricati nel Vault) abbiano `expiry_date > edition.start_date`. Se uno o più documenti sono scaduti o mancanti, il checkout è bloccato con messaggio specifico.

3. **Incompatibilità di calendario:** Il sistema impedisce a un discente di prenotare due edizioni con date sovrapposte (es. non può iscriversi a due corsi diversi che si svolgono nello stesso periodo).

4. **Freeze prenotazioni:** Le iscrizioni per una singola edizione si chiudono automaticamente **48 ore prima** dell'inizio del corso (`start_date - 48h`). Dopo questo termine il sistema imposta `blackout = true` sull'edizione e rimuove il CTA di prenotazione dal frontend.

5. **Quorum e rimborso automatico:** Se un'edizione viene cancellata dall'admin con booking attivi, il workflow di rimborso si attiva in batch, processando tutti gli ordini associati. I rimborsi vengono erogati al 100% indipendentemente dalla policy, in quanto la cancellazione è imputabile all'Academy.

---

## 11. Integrazioni con il Piano Progetto Padre

| Modulo Piano Padre                         | Punto di contatto con Booking                                                                                                                              |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Checkout a Step Obbligati**              | Il Booking è il "prodotto" aggiunto al carrello con metadata `edition_id`; il checkout multi-step raccoglie dati e documenti per quella specifica edizione |
| **Gestione Documentale**                   | Prima della conferma booking, si verifica il Vault del discente; i documenti mancanti bloccano il pagamento con lista di upload richiesti                  |
| **Automazione Conferma Corsi (soglia 20)** | Interamente gestita dal Booking Module tramite `check-edition-thresholds` job                                                                              |
| **Alert e Marketing Automation**           | I reminder pre-corso si integrano con il sistema di alert scadenze; il cross-selling post-corso usa lo storico di `BOOKING.status = ATTENDED`              |
| **SEO / Schema Markup**                    | Ogni `CourseEdition` esposta pubblicamente genera un tag `EducationEvent` con `startDate`, `endDate`, `remainingAttendeeCapacity`                          |
| **Modulo B2B (Fase 2)**                    | La colonna `seats` nel `BOOKING` permette già prenotazioni multi-posto; il modulo B2B userà questa struttura per la gestione gruppi aziendali              |

---

## 12. Considerazioni UX per Utenti "Tradizionalisti"

Come evidenziato nel Piano Progetto padre, una parte del target ha bassa alfabetizzazione digitale. Il calendario di prenotazione deve rispettare queste linee guida:

- **Nessun gergo tecnico:** "Posti rimasti: 8" invece di "Available capacity: 8"
- **Feedback proattivo:** Se l'utente seleziona una data esaurita, suggerire immediatamente la prossima data disponibile dello stesso corso
- **Conferma visiva forte:** Dopo la prenotazione, pagina di conferma con riepilogo grande e leggibile (data, ora, indirizzo, cosa portare)
- **CTA singolo e chiaro:** In qualunque stato del processo, deve essere presente un solo pulsante principale d'azione
- **Numero di telefono visibile:** In ogni pagina del flusso booking, il numero della segreteria è visibile per chi si "perde" nel processo digitale

---

## 13. Piano di Implementazione

### 13.1 Metodologia di Stima con Agenti AI

Le stime seguenti considerano un modello di sviluppo **human-in-the-loop con agenti AI** (es. GitHub Copilot Agent, Cursor Agent, o agenti personalizzati su Claude/GPT-4o). Il modello si basa su tre livelli di accelerazione:

| Livello                  | Tipo di lavoro                                                                        | Fattore di accelerazione     | Rationale                                                                            |
| ------------------------ | ------------------------------------------------------------------------------------- | ---------------------------- | ------------------------------------------------------------------------------------ |
| 🚀 **Alto** (3×–4×)      | Scaffolding, CRUD boilerplate, migrazioni DB, template email, test unitari ripetitivi | 70–80% del tempo risparmiato | L'AI genera l'80–90% del codice in modo autonomo; il dev revisa e aggiusta           |
| ⚡ **Medio** (2×–2.5×)   | Business logic custom, workflow Medusa, componenti UI complessi, scheduled jobs       | 50–60% del tempo risparmiato | L'AI produce la struttura; la logica di dominio richiede supervisione attiva         |
| 🧑‍💻 **Basso** (1.3×–1.5×) | QA end-to-end, UAT con utenti reali, revisione architetturale, test di carico         | 20–35% del tempo risparmiato | Non automatizzabile: richiede esecuzione umana, feedback discenti, validazione reale |

> **Assunzione:** 1 sviluppatore senior (tech lead) + agenti AI come copiloti attivi. Non si assume un team allargato.

---

### 13.2 Confronto Stime: Sviluppo Tradizionale vs. AI-Assisted

| Sprint  | Attività                                                                   | ⏱ Stima Tradizionale | 🤖 Stima AI-Assisted | Accelerazione | Cosa fa l'AI                                                                                                                                            |
| ------- | -------------------------------------------------------------------------- | -------------------- | -------------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **B-1** | Modello dati: migrazione DB, entities MedusaJS, repository layer           | 2 sett.              | **4–5 gg**           | 🚀 Alto       | Genera entities TypeScript, migration files Mikro-ORM, repository pattern completo da schema SQL descritto in linguaggio naturale                       |
| **B-2** | API Admin: CRUD edizioni, gestione manuale booking, export CSV             | 2 sett.              | **5–6 gg**           | 🚀 Alto       | Scaffolding completo dei route handlers MedusaJS, validazione Zod, paginazione e filtri; l'export CSV è generato da template                            |
| **B-3** | API Store: endpoints pubblici, logica prenotazione con transazione atomica | 2 sett.              | **7–8 gg**           | ⚡ Medio      | L'AI genera la struttura degli endpoint e i test; la logica `SELECT FOR UPDATE` e i casi edge richiedono review approfondita                            |
| **B-4** | Storefront: componente calendario Next.js, drawer dettaglio edizione       | 2 sett.              | **6–7 gg**           | ⚡ Medio      | L'AI genera il componente calendario (React + TailwindCSS) da specifiche UI descritte in testo; il dev rifinisce accessibilità mobile e logica di stato |
| **B-5** | Storefront: area personale "Le mie prenotazioni", gestione cancellazione   | 1 sett.              | **2–3 gg**           | 🚀 Alto       | Pagine con pattern CRUD standard; l'AI genera le view e i form di cancellazione con anteprima rimborso                                                  |
| **B-6** | Scheduled Jobs: threshold check, reminder, waitlist expiry                 | 1 sett.              | **2–3 gg**           | 🚀 Alto       | Pattern schedulato in MedusaJS è altamente ripetibile; l'AI genera tutti e 5 i job da specifiche funzionali                                             |
| **B-7** | Notifiche: template email/SMS per tutti i trigger (9 template)             | 1 sett.              | **1–2 gg**           | 🚀 Alto       | L'AI genera i template HTML/testo per tutti i trigger in una singola sessione di prompting strutturato                                                  |
| **B-8** | QA, test di carico (200 prenotazioni concorrenti), UAT con segreteria      | 2 sett.              | **7–9 gg**           | 🧑‍💻 Basso      | L'AI genera suite di test Vitest/Playwright e script k6 per il load test; l'UAT con gli utenti reali rimane non comprimibile                            |

---

### 13.3 Riepilogo Temporale

```
SVILUPPO TRADIZIONALE          AI-ASSISTED (range)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Sprint B-1  ██████████  2 sett.  →  ████  4–5 gg
Sprint B-2  ██████████  2 sett.  →  █████ 5–6 gg
Sprint B-3  ██████████  2 sett.  →  ████  7–8 gg (*)
Sprint B-4  ██████████  2 sett.  →  █████ 6–7 gg (*)
Sprint B-5  █████       1 sett.  →  ██    2–3 gg
Sprint B-6  █████       1 sett.  →  ██    2–3 gg
Sprint B-7  █████       1 sett.  →  █     1–2 gg
Sprint B-8  ██████████  2 sett.  →  ████  7–9 gg
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TOTALE:     13 settimane         →  ~7 settimane
                                    (range: 6.5–8 sett.)

(*) Sprint con componenti di dominio complesso: buffer di revisione umana incluso
```

**Risparmio stimato: 5–6 settimane** (38–46% del tempo totale)

---

### 13.4 Calendario di Progetto Proposto (con AI)

Assumendo kick-off **2 giugno 2026** e go-live **settembre 2026**:

| Periodo         | Sprint         | Attività                                                              |
| --------------- | -------------- | --------------------------------------------------------------------- |
| 2–13 giu        | B-1 + B-2      | Modello dati + API Admin (parallelizzabili con AI su branch separati) |
| 16–27 giu       | B-3            | API Store con logica transazionale                                    |
| 30 giu–10 lug   | B-4            | Componente calendario storefront                                      |
| 13–17 lug       | B-5 + B-6      | Area personale + Scheduled Jobs (in parallelo)                        |
| 20–22 lug       | B-7            | Template notifiche                                                    |
| 23 lug–7 ago    | B-8            | QA, load test, UAT segreteria                                         |
| **11–31 ago**   | **Buffer**     | Raffinamenti post-UAT, ottimizzazioni mobile, fix emergenti           |
| **1 settembre** | 🚀 **Go-Live** | Lancio B2C                                                            |

> Il buffer di 3 settimane ad agosto è deliberatamente conservativo: con AI-assisted development il rischio di sforamento sprint si riduce, ma l'UAT con utenti "tradizionalisti" può generare richieste di modifica UX imprevedibili.

---

### 13.5 Condizioni Abilitanti per le Stime AI

Queste stime sono valide **solo se** vengono rispettate le seguenti precondizioni:

1. **Analisi funzionale completa prima del coding** — Questo documento è la precondizione #1. L'AI produce codice di qualità quando il contesto di dominio è preciso e strutturato. Prompt ambigui generano refactoring costosi.

2. **Naming convention e ADR definiti a priori** — Stabilire prima del B-1 le convenzioni (nomi tabelle, struttura cartelle MedusaJS, stile componenti Next.js) per evitare che l'AI generi codice inconsistente tra sprint diversi.

3. **Test-driven prompting** — Ogni prompt all'agente AI include le specifiche del test atteso (input → output). Questo riduce le iterazioni di debug e aumenta la qualità del codice generato al primo tentativo.

4. **Review umana obbligatoria per la logica di business critica** — I punti della Sezione 10 (Regole di Business Critiche), in particolare la transazione atomica anti-overbooking e la verifica documenti, devono essere revisionati manualmente. L'AI non ha contesto legale/ministeriale.

5. **Un dev senior dedicato** — L'AI non sostituisce il ragionamento architetturale. Serve un tech lead che diriga gli agenti, validi l'output e prenda decisioni di trade-off.

**Durata totale stimata (AI-assisted):** ~7 settimane → kick-off 2 giugno, completamento entro fine luglio, buffer agosto per UAT e rifinitura.

---

## 14. Rischi Specifici del Modulo Booking

| Rischio                                        | Probabilità | Impatto | Mitigazione                                                                                     |
| ---------------------------------------------- | ----------- | ------- | ----------------------------------------------------------------------------------------------- |
| **Overbooking per race condition**             | Media       | Alto    | Transazione atomica con lock ottimistico su `CourseEdition`                                     |
| **Sessione confermata con documenti invalidi** | Alta        | Alto    | Double-check documenti a T-48h dall'inizio corso con alert alla segreteria                      |
| **Edizione cancellata last minute**            | Bassa       | Alto    | Workflow rimborso batch pre-testato; comunicazione SMS immediata                                |
| **Discente prenota edizione sbagliata**        | Alta        | Medio   | Finestra di modifica/cambio edizione gratuita entro 24h dall'acquisto (se capienza disponibile) |
| **Calendar component non usabile su mobile**   | Media       | Alto    | Fallback a lista testuale con date in ordine cronologico; test su iOS Safari e Chrome Android   |

---

## 15. Glossario

| Termine        | Definizione                                                                               |
| -------------- | ----------------------------------------------------------------------------------------- |
| **Edizione**   | Singola sessione datata di un corso (equivalente a "slot" o "appointment" in YITH)        |
| **Booking**    | Prenotazione di uno o più posti in un'edizione da parte di un discente                    |
| **Quorum**     | Soglia minima di 20 partecipanti paganti per confermare l'attivazione della sessione      |
| **Blackout**   | Blocco manuale di un'edizione da parte dell'admin (nessuna nuova iscrizione)              |
| **Early Bird** | Prezzo scontato per iscrizioni effettuate con largo anticipo rispetto alla data del corso |
| **Waitlist**   | Lista d'attesa per edizioni esaurite; scorrimento automatico in caso di cancellazione     |
| **Freeze**     | Chiusura automatica iscrizioni 48h prima dell'inizio del corso                            |
