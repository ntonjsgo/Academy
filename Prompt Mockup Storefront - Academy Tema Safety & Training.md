# Prompt Mockup Storefront – Academy Tema Safety & Training

> Raccolta di prompt per la generazione di wireframe/mockup ad alta fedeltà delle pagine principali del portale e-commerce. Stack: **Next.js Storefront** + **MedusaJS Backend**. Design: autorevole, istituzionale, ottimizzato per utenza mista (B2C adulta, 30–70 anni) e per fruizione mobile da cantieri / imbarcazioni.

---

## 1. Homepage

**Prompt:**

> Genera il mockup della homepage di "Academy Tema Safety & Training", un portale e-commerce per la formazione professionale marittima, industriale e antincendio con sede a Taranto. La pagina deve trasmettere autorevolezza istituzionale e guidare rapidamente l'utente verso i corsi.
>
> **Struttura verticale della pagina (scroll order):**
>
> 1. **Header fisso:** Logo Academy (sinistra), navigazione principale con voci `Corsi`, `Settori`, `Consulenza`, `Vigilanza Antincendio`, `Dante 4.0`, `Chi Siamo` (centro), pulsante `Accedi / Area Riservata` e CTA `Iscriviti ora` (destra). Banner sottile con avviso informativo (es. accreditamenti ministeriali attivi).
> 2. **Hero section a tutta larghezza:** Immagine di background del campo prove da 10.000 m² con esercitazione in corso. Titolo principale: `"Formazione Professionale Certificata – Marittima, Industriale, Antincendio"`. Sottotitolo con i 3 settori principali. Due CTA primari: `Esplora i Corsi` e `Scopri la nostra sede`. Badge di accreditamento (MIT, Ministero dell'Interno, MCA) posizionati in basso al hero.
> 3. **Strip numerica di credibilità:** 4 counter animati — `Fondati nel 2001`, `500+ Corsi attivi`, `10.000 m² Campo Prove`, `Accreditamenti Ministeriali`. Sfondo scuro a contrasto.
> 4. **Esplora per Settore:** 3 card grandi (Marittimo STCW, Industriale D.Lgs 81/08, Offshore & Civile) con icona, breve descrizione e link `Vedi tutti i corsi →`. Layout a griglia 3 colonne su desktop, stack verticale su mobile.
> 5. **Corsi in evidenza / Prossime edizioni:** Carosello orizzontale di 4–6 card corso con nome corso, settore (tag colorato), data prossima edizione, posti disponibili (badge verde/giallo/rosso), prezzo e CTA `Prenota posto`.
> 6. **Come funziona (3 step):** Icon + testo per: (1) Scegli il corso e la data, (2) Carica i documenti richiesti, (3) Ricevi conferma e accedi all'addestramento. Stile minimalista con frecce tra gli step.
> 7. **Sezione Brand & Portfolio Clienti:** Griglia di loghi aziendali in bianco/grigio (es. aziende marittime, cantieri, enti pubblici) su sfondo chiaro. Titolo: `"Si formano con noi"`.
> 8. **Testimonianze:** 3 citazioni di corsisti con foto, nome, ruolo professionale. Layout a card con stelle.
> 9. **Footer:** 4 colonne — Logo + payoff, Link rapidi, Contatti (via, email, telefono, Taranto), Newsletter signup. Certificazioni e loghi accreditamento in fondo. Lingua switcher IT / EN.
>
> **Palette:** Blu navy (#0A1628) + arancione safety (#E85D04) + bianco. Font: titoli sans-serif bold, corpo light. Stile: istituzionale, non "startup", no gradienti pastello.

---

## 2. Pagina Catalogo Corsi (PLP – Product Listing Page)

**Prompt:**

> Genera il mockup della pagina catalogo corsi di Academy Tema Safety & Training. La pagina deve permettere a un utente di orientarsi rapidamente tra circa 500 corsi organizzati per settore, normativa e livello.
>
> **Layout:**
>
> - **Sidebar sinistra (filtri):** Accordion con sezioni — Settore (Marittimo, Industriale, Offshore, Civile/Militare), Normativa (STCW, D.Lgs 81/08, GWO/OPITO), Durata (< 1 giorno, 1–3 giorni, > 3 giorni), Modalità (Solo in presenza), Disponibilità (Con posti disponibili, Con prossima edizione entro 30 giorni), Prezzo (slider range). Pulsante `Azzera filtri`. Su mobile: sidebar si trasforma in drawer bottom sheet.
> - **Area contenuto (destra):** Breadcrumb `Home > Corsi`. Intestazione con numero risultati (`"124 corsi trovati"`). Ordinamento dropdown (Data prossima edizione, Più richiesti, Prezzo crescente/decrescente). Toggle vista griglia / lista.
> - **Card corso (vista griglia):** Immagine o illustrazione settore, tag settore (colorato per categoria), titolo corso, durata + CFP, data prossima edizione, badge disponibilità posti (🟢 verde / 🟡 giallo / 🔴 esaurito), prezzo, CTA `Vedi corso`.
> - **Card corso (vista lista):** Medesime informazioni su riga orizzontale con più dettaglio testuale e spazio per breve abstract.
> - **Stato "Nessun risultato":** Illustrazione + suggerimento di rimuovere filtri o contattare la segreteria.
> - **Paginazione** in fondo: numerata + `Carica altri`.
>
> **Nota UX:** Il tag settore della card deve essere color-codificato coerentemente in tutto il sito (es. blu per Marittimo, verde per Industriale, arancione per Offshore).

---

## 3. Scheda Corso (PDP – Product Detail Page)

**Prompt:**

> Genera il mockup della pagina di dettaglio di un singolo corso. Usa come esempio "Basic Safety Training (BST) – STCW A-VI/1", corso marittimo obbligatorio della durata di 5 giorni.
>
> **Struttura della pagina:**
>
> 1. **Breadcrumb:** `Home > Corsi > Marittimo > Basic Safety Training`
> 2. **Intestazione corso:** Titolo grande, tag settore colorato, numero normativa di riferimento (es. STCW A-VI/1-4), durata in ore, CFP, validità certificato (es. "5 anni"), badge "Accreditato MIT". Indicatore lingua: 🇮🇹 Italiano (erogazione in presenza – Taranto).
> 3. **Tab di navigazione sticky:** `Descrizione | Requisiti | Calendario & Prezzi | Documenti Richiesti | FAQ`
> 4. **Tab Descrizione:** Testo strutturato — Obiettivi formativi, Contenuti del corso (lista puntata), A chi è rivolto, Esito (attestato rilasciato).
> 5. **Tab Requisiti:** Lista documenti e prerequisiti richiesti per l'iscrizione. Avviso evidenziato se il corso ha propedeuticità obbligatorie.
> 6. **Tab Calendario & Prezzi (elemento principale del mockup):**
>    - Calendario mensile interattivo con date codificate a colore:
>      - 🟢 Verde: posti disponibili (con contatore "8 posti liberi")
>      - 🟡 Giallo: quasi esaurito (< 5 posti)
>      - 🔴 Rosso: esaurito → pulsante `Entra in lista d'attesa`
>      - ⬛ Grigio: data passata o bloccata
>    - Al click su una data: pannello laterale/modal con dettaglio edizione (date inizio/fine, istruttore, orari, sede), price breakdown (prezzo standard + eventuale sconto early bird evidenziato), servizi add-on selezionabili (Navetta A/R €15, Kit didattico €25).
>    - CTA principale `Prenota questo corso →` verde/arancione.
>    - Vista mobile: lista cronologica a sostituzione del calendario.
> 7. **Tab Documenti Richiesti:** Lista dinamica dei documenti necessari all'upload durante il checkout (es. Libretto di navigazione, Certificato medico, Attestati propedeutici). Icona di stato per ogni documento (✅ presente nel Vault / ⚠️ scaduto / ➕ da caricare).
> 8. **Tab FAQ:** Accordion con 5–8 domande frequenti relative al corso.
> 9. **Sidebar destra (sticky su desktop):** Riepilogo selezione corrente (data, posti, add-on, totale), CTA `Procedi alla prenotazione`, `Condividi corso`, link `Hai bisogno di aiuto? Contattaci`.
>
> **Nota UX:** Il calendario deve essere l'elemento visivamente dominante della pagina. Ottimizzazione mobile critica per utenti che accedono da imbarcazioni con banda ridotta.

---

## 4. Checkout – Flusso Multi-Step

**Prompt:**

> Genera il mockup del flusso di checkout di Academy Tema Safety & Training, articolato in 4 step progressivi con indicatore di avanzamento. Il checkout non è un acquisto diretto ma una raccolta dati + prenotazione con pagamento differito.
>
> **Step indicator:** Barra orizzontale in alto con 4 step: `1. Dati Anagrafici → 2. Documenti → 3. Riepilogo → 4. Conferma`. Step completati in verde, step corrente in evidenza, step futuri grigi.
>
> **Step 1 – Dati Anagrafici:**
> Form con campi: Nome, Cognome, Codice Fiscale (con validazione sintattica in-line), Data di nascita, Luogo di nascita, Email, Telefono. Per corsi marittimi: campo aggiuntivo `Numero Matricola Gente di Mare`. Recapito azienda (opzionale). Lingua interfaccia IT/EN.
>
> **Step 2 – Upload Documenti:**
>
> - Lista dinamica dei documenti richiesti per il corso selezionato.
> - Per ogni documento: area di upload drag-and-drop con anteprima (PDF/immagine), indicatore di avanzamento upload, stato validazione (in attesa / caricato / errore).
> - Sezione `Usa documenti dal tuo Vault`: se l'utente è già registrato, mostra i documenti già caricati in precedenza con data caricamento e stato di validità (✅ valido / ⚠️ in scadenza / ❌ scaduto).
> - Avviso: `"I tuoi documenti verranno esaminati dalla segreteria entro 1 giorno lavorativo. Il posto è riservato durante la revisione."`.
>
> **Step 3 – Riepilogo Ordine:**
>
> - Ricepilogo visivo: corso + data edizione, dati anagrafici inseriti, documenti caricati (lista con ✅), add-on selezionati, prezzo totale con breakdown (quota corso + add-on + eventuale sconto early bird).
> - Checkbox accettazione Privacy Policy e Termini e Condizioni (con link).
> - CTA `Invia richiesta di prenotazione →`.
> - Nota informativa: `"Il pagamento non verrà richiesto ora. Riceverai un link di pagamento via email solo dopo la validazione dei documenti da parte della segreteria."`.
>
> **Step 4 – Conferma Ricezione:**
>
> - Pagina di thank-you: icona animata di conferma, titolo `"Richiesta ricevuta!"`, riepilogo prenotazione, prossimi passi (step visivi: "Documenti in revisione → Link pagamento via email → Conferma iscrizione"), CTA `Vai all'area riservata` e `Esplora altri corsi`.
>
> **Sidebar destra (persiste su tutti gli step):** Riepilogo ordine compatto (corso, data, posti, prezzo), non modificabile dopo lo step 1.

---

## 5. Area Riservata – Dashboard Discente

**Prompt:**

> Genera il mockup dell'area riservata per il discente registrato su Academy Tema Safety & Training. L'interfaccia deve essere semplice e accessibile per utenti con bassa confidenza digitale (fino a 70 anni).
>
> **Layout:** Sidebar sinistra con navigazione — `Dashboard`, `Le mie Prenotazioni`, `Documenti (Vault)`, `I miei Certificati`, `Impostazioni Account`. Header con nome utente e avatar.
>
> **Dashboard (home area riservata):**
>
> - Sezione `In corso / In attesa`: card delle prenotazioni attive con stato visibile (es. `"Documenti in revisione"`, `"In attesa di pagamento – scade tra 23h"`, `"Confermato – 12 febbraio 2026"`). Ogni card ha CTA contestuale (es. `Completa pagamento`, `Ricarica documento`).
> - Sezione `Prossima scadenza certificati`: alert con contatore giorni per i certificati in scadenza (90/60/30 giorni). Link diretto al corso di rinnovo.
> - Sezione `Corsi consigliati`: 3 card corso suggerite in base ai corsi frequentati (cross-selling).
>
> **Pagina "Le mie Prenotazioni":**
>
> - Tabella/lista con filtro (Tutte / In attesa / Confermate / Completate / Cancellate).
> - Per ogni prenotazione: nome corso, data edizione, stato (badge colorato), importo pagato / da pagare, CTA `Dettaglio` / `Cancella`.
>
> **Pagina "Documenti (Vault)":**
>
> - Griglia di documenti caricati, ognuno con: nome documento, data caricamento, data scadenza (con semaforo verde/giallo/rosso), stato validazione, pulsante `Aggiorna documento`.
> - CTA `+ Carica nuovo documento`.
>
> **Pagina "I miei Certificati":**
>
> - Lista certificati conseguiti presso l'Academy con data conseguimento, corso, data scadenza, pulsante `Scarica PDF` e `Prenota rinnovo`.
>
> **Nota UX:** Font grande (min 16px), pulsanti ampi, messaggi di stato in linguaggio semplice (no tecnicismi), icone affiancate al testo. Ogni sezione deve avere un breve testo esplicativo.

---

## 6. Pagina Istituzionale – Chi Siamo / About

**Prompt:**

> Genera il mockup della pagina istituzionale "Chi Siamo" di Academy Tema Safety & Training.
>
> **Struttura:**
>
> 1. **Hero:** Foto del campo prove con istruttori in azione. Titolo: `"Eccellenza nella Formazione per la Sicurezza dal 2001"`. Sottotitolo con sintesi della missione.
> 2. **Storia e valori:** Timeline verticale con tappe principali (fondazione 2001, primo accreditamento MIT, espansione campo prove, accreditamento MCA, ecc.). Colore secondario per accent.
> 3. **Numeri chiave:** 4 counter — Anno di fondazione, Corsi accreditati, Discenti formati all'anno, m² campo prove.
> 4. **Accreditamenti:** Griglia di loghi ministeriali e organismi di certificazione (MIT, Ministero dell'Interno, MCA) con breve descrizione dell'accreditamento e anno di ottenimento.
> 5. **Il Team / Istruttori:** Card degli istruttori senior con foto, nome, specializzazione, certificazioni. (contribuisce ai segnali E-E-A-T per SEO/GEO).
> 6. **Il Campo Prove:** Sezione fotografica con descrizione delle aree — simulatore incendio, vasca nautica, area antincendio, aula teorica. Mappa localizzazione Taranto.
> 7. **Portfolio Clienti:** Griglia loghi aziende e gruppi industriali clienti. Titolo: `"Aziende che si affidano a noi"`.
> 8. **CTA finale:** `"Porta la tua squadra a Taranto"` con link a modulo contatto B2B e `"Esplora i corsi"`.

---

## 7. Pagina Settore (Landing SEO per categoria)

**Prompt:**

> Genera il mockup di una pagina di atterraggio SEO dedicata a un settore specifico. Usa come esempio il **Settore Marittimo STCW**.
>
> **Obiettivo SEO:** Posizionamento su keyword come "corsi STCW Italia", "rinnovo certificati marittimi", "Basic Safety Training 2026".
>
> **Struttura:**
>
> 1. **Hero:** Immagine tematica (gente di mare, simulatore navale). Titolo H1 SEO-ottimizzato: `"Formazione Marittima STCW – Corsi Obbligatori Certificati MIT"`. Breadcrumb `Home > Marittimo`.
> 2. **Intro editoriale:** Blocco testo 200–300 parole che spiega il quadro normativo STCW 2010 (Manila Amendments), i certificati obbligatori per la gente di mare, le scadenze di rinnovo. Tono autorevole, ottimizzato E-E-A-T.
> 3. **Catalogo corsi del settore:** Griglia filtrabile di tutti i corsi STCW disponibili con mini-card (titolo, normativa, durata, disponibilità, prezzo). Filtri: livello (Base / Avanzato / Direttivo), durata, disponibilità immediata.
> 4. **Percorsi consigliati:** 2–3 "learning path" visualizzati come flow diagram (es. BST → Advanced Firefighting → Crowd & Crisis Management) con descrizione del profilo professionale a cui è destinato.
> 5. **FAQ Normativa:** Accordion con 6–8 domande tipiche (es. "Ogni quanto si rinnova il certificato STCW?", "Posso fare il BST senza libretto di navigazione?"). Ottimizzato per rich snippet Google e ricerca vocale.
> 6. **Social proof:** 2–3 testimonianze di marittimi professionisti. Badge accreditamento MIT.
> 7. **CTA:** `"Trova il corso giusto per te"` → link al catalogo filtrato per settore marino.

---

## 8. Pagina Contatti e Richiesta Informazioni B2B

**Prompt:**

> Genera il mockup della pagina Contatti di Academy Tema Safety & Training, che deve servire sia il singolo discente sia l'azienda che vuole iscrivere più dipendenti (lead generation B2B).
>
> **Layout a due colonne:**
>
> **Colonna sinistra – Informazioni di contatto:**
>
> - Indirizzo sede (Taranto), mappa interattiva Google Maps embedded.
> - Email segreteria, telefono, orari di apertura.
> - Link WhatsApp diretto per richieste rapide.
> - Card accreditamenti.
>
> **Colonna destra – Form di contatto segmentato:**
>
> - Toggle iniziale: `Sono un privato` / `Rappresento un'azienda`.
> - **Form Privato:** Nome, Email, Telefono, Corso di interesse (dropdown), Messaggio, Privacy checkbox, CTA `Invia richiesta`.
> - **Form Azienda:** Ragione sociale, P.IVA, Nome referente, Email aziendale, Telefono, Numero dipendenti da formare, Settore industriale (dropdown: Marittimo / Industriale / Offshore / Antincendio), Corsi di interesse (multi-select), Note, Privacy + Marketing checkbox, CTA `Richiedi preventivo personalizzato`.
> - Microcopy sotto il form: `"Il nostro team risponde entro 1 giorno lavorativo."`.
>
> **Sezione lower:** FAQ rapide sulle modalità di contatto. Link all'area FAQ del sito.

---

## 9. Pagina di Errore / Esaurito – Waitlist

**Prompt:**

> Genera il mockup della pagina o modal che appare quando un utente seleziona un'edizione di corso **esaurita** e decide di iscriversi alla lista d'attesa.
>
> **Elemento:** Modal centrato sopra la scheda corso (non una pagina separata).
>
> **Contenuto del modal:**
>
> - Titolo: `"Corso esaurito – Entra in lista d'attesa"`.
> - Sottotitolo: `"Verrai avvisato via email non appena si libera un posto. Hai 48 ore per completare l'iscrizione dalla nostra notifica."`.
> - Indicatore posizione stimata in lista (es. `"Attualmente ci sono 3 persone prima di te"`).
> - Form compatto: Nome, Email, Telefono, checkbox `Notificami anche per le prossime edizioni di questo corso`.
> - CTA `Iscrivimi alla lista d'attesa` + link `Vedi altre date disponibili →`.
> - Nota sulla privacy.
>
> **Stato post-iscrizione:** Animazione conferma, messaggio `"Sei in lista! Ti avviseremo appena disponibile un posto."`, CTA `Esplora altri corsi`.

---

## 10. Email Transazionale – Conferma Prenotazione / Link Pagamento

**Prompt:**

> Genera il mockup di due template email responsive:
>
> **Email 1 – "Documenti ricevuti, in revisione"**
>
> - Header con logo Academy + sfondo navy.
> - Saluto personalizzato con nome discente.
> - Riepilogo prenotazione: corso, data edizione, numero di riferimento.
> - Status tracker visivo (3 step: `✅ Documenti caricati → ⏳ In revisione → Link pagamento`).
> - Messaggio: `"La segreteria esaminerà i tuoi documenti entro 1 giorno lavorativo."`.
> - Contatti segreteria per urgenze.
> - Footer con accreditamenti e link disiscriz.
>
> **Email 2 – "Documenti approvati – Completa il pagamento"**
>
> - Header uguale.
> - Titolo urgency: `"Documenti approvati! Completa il pagamento entro [DATA ORE]"`.
> - Countdown visivo / data scadenza in evidenza (48 ore).
> - Riepilogo costo: quota corso + eventuali add-on + totale.
> - Pulsante CTA grande arancione: `"Paga ora – €[importo]"`.
> - Nota: `"Dopo il pagamento riceverai la conferma definitiva con tutti i dettagli logistici del corso."`.
> - Footer standard.

---

_Fine documento — 10 prompt per 10 schermate / componenti principali dello storefront._
