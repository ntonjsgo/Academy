# **E-commerce Strategico e Automazione per Academy Tema Safety & Training**

## **Introduzione**

L’analisi dello stato attuale e delle prospettive di crescita di Academy Tema Safety & Training evidenzia un’opportunità di trasformazione digitale di vasta portata, mirata a consolidare la leadership nel settore della formazione professionale per la sicurezza marittima, industriale, civile e militare.

Fondata nel 2001 a Taranto, l’Academy rappresenta oggi un’eccellenza tecnica, supportata da un campo prove di oltre 10.000 metri quadrati e da accreditamenti ministeriali di alto profilo, tra cui quelli rilasciati dal Ministero delle Infrastrutture e dei Trasporti, dal Ministero dell'Interno e dalla Maritime and Coastguard Agency (MCA). Tuttavia, questa solidità operativa è attualmente limitata da un modello di gestione amministrativa e commerciale prevalentemente manuale, che non riflette la modernità tecnologica dell’ente formatore.

La situazione attuale vede la gestione di un volume significativo di discenti, stimato tra i 400 e i 500 mensili, attraverso processi basati su telefonate, scambi di email per la raccolta documentale e una gestione frammentata dei pagamenti, spesso limitata ad acconti presenziali o bonifici non riconciliati automaticamente. Tale assetto genera colli di bottiglia operativi che rallentano l'espansione del catalogo formativo: molti corsi accreditati non vengono sviluppati o pubblicizzati proprio a causa dell'onere gestionale che comporterebbero. In aggiunta, la quasi totale assenza di un posizionamento SEO strutturato impedisce all'azienda di intercettare la crescente domanda organica, rendendo il business dipendente dal passaparola o dalla partecipazione a bandi pubblici.

Il presente piano di progetto delinea la transizione verso un ecosistema e-commerce "ibrido" basato sullo stack tecnologico MedusaJS, finalizzato all'automazione totale del funnel di vendita e della gestione burocratica.

L'obiettivo fondamentale è la riduzione del carico di lavoro manuale per la segreteria, trasformando l'Academy in un centro di profitto digitale scalabile, capace di gestire discenti B2C nell'immediato e aziende B2B in una fase successiva. Il documento analizza l'integrazione di flussi complessi per l'upload di documenti ministeriali, sistemi di alert automatizzati per le scadenze dei certificati e una strategia SEO/GEO (Generative Engine Optimization) orientata al 2026\.

### **Filosofia del Rilascio Iterativo**

Il progetto è strutturato su un approccio modulare per rispondere alla necessità di un go-live rapido entro settembre, evitando le complicazioni derivanti da integrazioni ERP massive nella fase iniziale. La segmentazione del progetto permette di isolare i rischi e di formare il personale dell'Academy in modo progressivo, sradicando abitudini operative obsolete senza generare shock organizzativi. L'architettura prevede che ogni componente funzionale (checkout, gestione documenti, marketing automation) sia sviluppata come un modulo indipendente all'interno dell'ecosistema Medusa.

###

### **Transizione Tecnologica e Fasi di Evoluzione**

La strategia prevede un focus iniziale sull'utenza B2C (singolo discente), che rappresenta attualmente il volume maggiore di clienti e la fonte principale di micro-gestione manuale. Per le aziende (B2B), la gestione rimarrà inizialmente ibrida, utilizzando il sito come piattaforma di _lead generation_ avanzata attraverso form segmentati, per poi integrare la completa autonomia di acquisto collettivo nella Fase 2\. Questo permette di non "sovra-ingegnerizzare" il sistema prima di aver validato i flussi B2C sul campo.

L'esclusione dell'integrazione con Mago dalla Fase 1 è una scelta tecnica mirata a garantire la velocità di caricamento delle pagine e la semplicità del backend per gli operatori. Durante il periodo transitorio (settembre \- dicembre), la segreteria opererà sulla dashboard di Medusa, che fungerà da "unica fonte di verità" per le iscrizioni, effettuando un allineamento manuale semplificato verso Mago, operazione che risulterà comunque sensibilmente più rapida rispetto al recupero di dati da catene di email e telefonate.

## **Progetto Academy**

L'Academy Tema Safety & Training non è solo un centro di formazione, ma un hub di certificazione normativa. Il progetto digitale deve riflettere questa autorevolezza istituzionale attraverso un portale che integri informazione tecnica e capacità transazionale. Il catalogo, composto da circa 500 corsi, richiede una classificazione gerarchica complessa che permetta all'utente di identificare immediatamente il percorso formativo corretto in base alla propria carriera e alle scadenze dei propri titoli.

### **Struttura del Catalogo e User Persona**

La navigazione del portale sarà segmentata per settori industriali, ognuno caratterizzato da requisiti d'ingresso e documentali specifici:

- **Settore Marittimo (STCW):** Rappresenta il nucleo storico dell'Academy. Include corsi obbligatori per la gente di mare, dai livelli base (Basic Safety Training) ai livelli direttivi (High Voltage Technology, Crowd and Crisis Management). La complessità risiede nella gestione delle propedeuticità: molti corsi avanzati richiedono il possesso verificato di certificati base.
- **Settore Industriale (81/08):** Focalizzato sulla sicurezza sul lavoro, include la formazione per datori di lavoro, dirigenti, preposti e lavoratori, oltre a corsi specialistici per l'uso di attrezzature (carrelli elevatori, piattaforme elevabili) e lavori in ambienti confinati.
- **Settore Offshore e Civile:** Corsi specialistici per operatori in piattaforme petrolifere (GWO/OPITO) e per il settore civile/militare, caratterizzati da simulazioni pratiche intense effettuate nel campo prove di Taranto.

Ogni scheda corso sarà ottimizzata per fornire tutte le informazioni necessarie: durata in ore, numero di crediti formativi (CFP), validità del certificato, modalità di erogazione (solo in presenza presso l'Academy) e requisiti documentali.

###

###

###

### **La Sfida della "Rieducazione" Digitale**

Uno degli aspetti critici identificati è l'età media di alcuni corsisti (fino a 60-70 anni) e la loro resistenza ai sistemi digitali. L'Academy ha espresso la necessità di una transizione "ferma ma guidata". Il sistema informativo dovrà quindi essere estremamente semplificato dal punto di vista della User Experience (UX), offrendo istruzioni chiare in ogni step e feedback immediati in caso di errore. L'obiettivo è minimizzare la necessità per l'utente di chiamare l'amministrazione, rendendo l'autonomia digitale più vantaggiosa e veloce rispetto al canale tradizionale.

### **Gestione dei Contenuti Istituzionali (CMS)**

Oltre al motore e-commerce per la formazione, il portale deve coprire anche le aree istituzionali emerse nella fase di analisi: **Consulenza**, **Vigilanza Antincendio** e il prodotto **Dante 4.0**. Queste sezioni verranno gestite tramite un **CMS headless** integrato nello stack Next.js, che consente al team di marketing interno di aggiornare autonomamente testi, immagini, casi studio e referenze clienti senza alcun intervento tecnico.

L'architettura headless garantisce la separazione netta tra i contenuti editoriali e il motore transazionale MedusaJS, preservando le performance del sito. Le pagine vetrina contribuiranno inoltre alla strategia SEO come contenuti istituzionali autorevoli, rafforzando il posizionamento organico su keyword di settore legate alla consulenza antincendio e ai sistemi di rilevazione. La scelta specifica del CMS headless (es. Sanity, Strapi o soluzione equivalente) sarà definitiva nella fase di Pianificazione e Modeling.

##

## **Requisiti Funzionali**

L'architettura funzionale del progetto è progettata per automatizzare l'intero ciclo di vita del discente, dalla scoperta del corso alla gestione del certificato post-erogazione. Di seguito vengono analizzate le quattro funzionalità chiave identificate come prioritarie per la riduzione del carico di lavoro manuale.

### **1\. Flusso di Acquisto a Step Obbligati**

Il processo di checkout non è una semplice transazione, ma una raccolta strutturata di dati anagrafici e professionali valida ai fini ministeriali. L'implementazione su MedusaJS utilizzerà i workflow personalizzati per garantire che nessuna vendita venga conclusa senza i dati necessari.

**A. Selezione Data ed Edizione:** L'utente non acquista un prodotto generico, ma una specifica sessione di addestramento legata a un calendario predefinito. Il sistema mostrerà solo le date con posti ancora disponibili, gestendo l'inventario in tempo reale attraverso il modulo Inventory di Medusa.

**B. Raccolta Anagrafica e Requisiti:** In questa fase verranno richiesti i dati necessari per la compilazione dei registri d'esame: Codice Fiscale, luogo e data di nascita, matricola della gente di mare (per i corsi marittimi). Il sistema effettuerà una validazione sintattica immediata tramite schemi Zod integrati nel backend.

**C. Upload Documenti Ministeriali:** In base alla tipologia di corso selezionata, il sistema attiverà campi di upload dinamici.

- Esempio STCW: Libretto di navigazione, certificato di idoneità medica, attestati propedeutici.
- Esempio Sicurezza Industriale: Dichiarazione della società armatrice o contratto di lavoro. L'upload avverrà su storage protetto (AWS S3) tramite il FileService di MedusaJS, garantendo la non indicizzabilità dei documenti personali.

**D. Pagamento e Chiusura Ordine:** Il sistema richiederà il pagamento dell'intero importo, eliminando la gestione manuale degli acconti. L'integrazione con gateway moderni permetterà di accettare carte di credito, bonifici bancari istantanei con riconciliazione tramite webhooks e pagamenti rateali.

### **2\. Gestione Documentale Avanzata**

La segreteria di Academy Tema Safety & Training spende attualmente ore nel verificare documenti ricevuti via email in formati spesso illeggibili o con scadenze superate.

- **Dashboard di Validazione:** Gli operatori disporranno di un'interfaccia nel pannello Admin di Medusa per visualizzare i documenti per ogni ordine. Potranno approvare il documento o richiedere un nuovo upload fornendo una causale predefinita.
- **Vault Personale del Discente:** Ogni utente registrato avrà un'area "Documenti" dove potrà caricare i propri titoli una sola volta e utilizzarli per iscrizioni future, aggiornandoli solo in caso di scadenza.
- **Tracciamento Scadenze:** Il sistema estrarrà (manualmente dagli operatori in Fase 1, o tramite OCR in futuro) le date di scadenza dei documenti caricati (es. certificato medico biennale).2 Il sistema impedirà l'iscrizione a corsi che si svolgono oltre la data di scadenza del documento necessario.

### **3\. Automazione Conferma Corsi (Soglia 20 Persone)**

Per garantire la sostenibilità economica delle sessioni pratiche che richiedono attivazione di campo prove e istruttori multipli, è stato fissato un quorum di 20 partecipanti.

- **Logica di Monitoraggio:** Uno Scheduled Job di Medusa monitorerà periodicamente lo stato delle iscrizioni per ogni edizione.
- **Stato di Conferma:** Al raggiungimento della soglia (20/20 paganti), il sistema cambierà automaticamente lo stato dell'edizione in "Confermata".
- **Workflow di Notifica:** Il trigger di conferma attiverà una comunicazione massiva via email a tutti i discenti con i dettagli logistici (orari, navetta, abbigliamento pratico) e agli istruttori interni per la preparazione dell'addestramento.
- **Gestione Sotto-soglia:** Qualora la soglia non venisse raggiunta entro 7 giorni dall'inizio del corso, il sistema invierà un alert alla direzione per decidere se confermare manualmente l'edizione o procedere con il rinvio/rimborso automatico tramite i workflow di compensazione di Medusa.

### **4\. Sistema di Alert e Marketing Automation**

La fidelizzazione del discente è fondamentale in un settore dove le certificazioni scadono periodicamente.

- **Remind Scadenza Certificati:** Il sistema invierà notifiche automatiche via email e SMS a scadenze predefinite: 90, 60 e 30 giorni prima della scadenza di un titolo ottenuto presso l'Academy. Questa funzione trasforma la conformità normativa in un'opportunità di vendita ricorrente.
- **Cross-selling Formativo:** Sulla base dei corsi frequentati, il sistema suggerirà percorsi avanzati o complementari (es. dopo il "Antincendio Base", suggerimento automatico per il "Antincendio Avanzato" o "Security Awareness").
- **Recupero Carrelli Abbandonati:** Data la complessità dell'upload documentale, è probabile che alcuni utenti interrompano il processo. Il sistema invierà reminder automatici per invitare al completamento dell'iscrizione prima che i posti si esauriscano.

### **5\. Motore di Prenotazione e Gestione Calendario**

L'intera logica commerciale dell'Academy è fondata su un principio che differenzia radicalmente questo progetto da un e-commerce convenzionale: ogni acquisto non è la compravendita di un prodotto generico, ma la prenotazione di un posto fisico in una sessione d'addestramento datata, con una capienza reale e requisiti normativi specifici. Questa specificità impone l'implementazione di un modulo di booking nativo su MedusaJS, che governi il ciclo di vita completo di ogni edizione corso, dall'apertura delle iscrizioni alla registrazione dell'esito post-formazione.

**A. Calendario Interattivo per la Selezione dell'Edizione:** La scheda di ogni corso esporrà un calendario mensile che renderà immediatamente visibile la disponibilità delle sessioni programmate. Ogni data sarà codificata cromaticamente — verde per i posti disponibili con indicazione del numero residuo, giallo per le edizioni quasi esaurite, rosso per i corsi al completo con accesso alla lista d'attesa, grigio per le date già trascorse o temporaneamente bloccate per esigenze logistiche. Su dispositivi mobili, la vista si adatterà a un formato a lista cronologica per garantire la piena fruibilità anche a chi accede da impianti industriali o imbarcazioni con connettività limitata.

**B. Gestione Capienza, Quorum e Lista d'Attesa:** Ogni edizione è configurata con una capienza massima e con la soglia minima di attivazione di 20 partecipanti già definita nella funzionalità 3. Quando un'edizione raggiunge il pieno, il sistema attiva automaticamente una lista d'attesa: all'eventuale cancellazione di un posto, il primo iscritto in lista riceve una notifica con un collegamento esclusivo di prenotazione valido per 48 ore. Allo scadere di questa finestra senza risposta, il sistema avanza in autonomia alla posizione successiva, garantendo il riempimento delle sessioni senza alcun intervento manuale della segreteria.

**C. Politica di Cancellazione e Rimborso Scaglionato:** Per tutelare la sostenibilità economica delle sessioni, che richiedono l'attivazione del campo prove e la disponibilità degli istruttori, il sistema applicherà una politica di cancellazione strutturata per fasce temporali: rimborso integrale per le disdette effettuate con più di 14 giorni di anticipo, rimborso del 50% nella finestra tra i 14 e i 7 giorni, nessun rimborso per le cancellazioni nell'ultima settimana prima dell'inizio. L'intera procedura, inclusa la restituzione dei fondi sul gateway di pagamento originale, sarà automatizzata attraverso i workflow di compensazione di MedusaJS, eliminando la gestione manuale dei rimborsi dalla segreteria. Nel caso di cancellazione imputabile all'Academy, il rimborso sarà sempre integrale e automatico, indipendentemente dalla tempistica.

**D. Prezzi Early Bird e Servizi Aggiuntivi:** Il modulo supporta regole di prezzo differenziate in funzione dell'anticipo con cui viene effettuata l'iscrizione. La configurazione di uno sconto per iscrizione anticipata — attivabile per singola edizione o come regola globale — incentiva la raccolta delle adesioni con largo anticipo, permettendo all'Academy di confermare le sessioni prima e di pianificare le risorse logistiche con maggiore certezza. Parallelamente, il discente potrà selezionare servizi accessori durante il checkout, come il trasporto navetta andata/ritorno o il kit di materiale didattico supplementare, aumentando il valore medio dell'ordine senza appesantire il processo di iscrizione.

**E. Controlli Automatici e Integrità del Processo:** Il motore di prenotazione è governato da controlli che operano in trasparenza per il discente ma rappresentano la spina dorsale della correttezza operativa. Le iscrizioni si chiudono automaticamente 48 ore prima dell'inizio di ogni sessione, prevenendo complicazioni logistiche dell'ultimo minuto. Il sistema blocca la prenotazione qualora i documenti obbligatori presenti nel Vault personale del discente risultino scaduti alla data dell'edizione selezionata, comunicando in modo esplicito quali titoli richiedono aggiornamento prima di procedere. Infine, è impedita l'iscrizione contemporanea a due edizioni con date sovrapposte, una casistica frequente tra i discenti che gestiscono autonomamente più percorsi formativi in parallelo.

**F. Gestione Operativa per la Segreteria:** La dashboard Admin di Medusa sarà estesa con una vista calendario dedicata che consente agli operatori di monitorare in tempo reale lo stato di tutte le edizioni programmate. Le funzionalità chiave includono la conferma manuale di un'edizione in deroga al quorum, il blocco temporaneo delle iscrizioni per esigenze logistiche, lo spostamento di una sessione con notifica automatica agli iscritti e l'esportazione della lista partecipanti in formato CSV per la compilazione dei registri d'esame ministeriali. Al termine di ogni sessione, gli operatori potranno registrare la presenza di ogni discente, alimentando lo storico formativo che guida i meccanismi di cross-selling e di alert scadenze certificati descritti nella funzionalità 4.

## **SEO e Architettura**

La visibilità organica è il pilastro su cui Academy Tema Safety & Training costruirà la propria crescita indipendente dai bandi e dal passaparola.2 La strategia proposta si articola su tre livelli: ottimizzazione tecnica, autorità semantica e preparazione all'intelligenza artificiale generativa (GEO).5

### **Architettura Tecnica per le Performance**

Il sito sarà sviluppato con Next.js (Storefront) connesso via API a MedusaJS (Backend). Questa configurazione garantisce velocità di caricamento fulminee, essenziali per il ranking su Google nel 2025 e 2026\.49

- **Core Web Vitals:** Ottimizzazione metrica LCP (Largest Contentful Paint) per assicurare che le schede corsi siano visualizzabili in meno di 2 secondi anche su reti mobili instabili (tipiche di chi naviga da imbarcazioni o siti industriali).49
- **Server Side Rendering (SSR):** Tutte le pagine dei corsi saranno generate lato server per permettere una scansione profonda da parte dei crawler di Google, indicizzando i calendari e le disponibilità in tempo reale.17
- **Domain Strategy:** Consolidamento dell'identità digitale su un unico dominio autorevole, utilizzando tag hreflang per supportare la futura espansione internazionale (italiano/inglese).2

### **Strategia Content e SEO Semantica**

L'attuale assenza di posizionamento richiede un lavoro massivo sulla struttura dei contenuti. Non punteremo solo a keyword secche, ma a cluster di argomenti che riflettano l'expertise dell'Academy.48

- **Cluster STCW Marittimo:** Creazione di guide approfondite su "Come rinnovare il libretto di navigazione" o "Cosa prevede la convenzione STCW 2025", intercettando le ricerche informative che precedono l'acquisto.23
- **Cluster Sicurezza Industriale:** Pagine dedicate alle novità dell'Accordo Stato-Regioni 2025, posizionando l'Academy come fonte autorevole e aggiornata sulle normative vigenti.54
- **Schema Markup:** Implementazione di dati strutturati Course e EducationEvent per ogni edizione. Questo permetterà di visualizzare nei risultati di ricerca Google i "Rich Snippets" con date, prezzi e durata del corso, aumentando drasticamente il Click-Through Rate (CTR).5

### **GEO: Generative Engine Optimization**

Con l'ascesa di ChatGPT, Perplexity e Google AI Overviews, la strategia SEO deve evolvere in GEO.57 L'obiettivo è che le AI citino l'Academy come risposta alle domande degli utenti.6

- **Semantic Chunking:** I testi saranno organizzati in blocchi densi di fatti e statistiche, facilmente "digeribili" dai Large Language Models (LLM).48
- **E-E-A-T (Esperienza, Competenza, Autorevolezza, Affidabilità):** Valorizzazione della storia aziendale (2001), del campo prove da 10.000mq e dei nomi degli istruttori certificati. Questi "segnali di fiducia" sono fondamentali per essere scelti dalle AI come fonti autorevoli.1
- **FAQ Conversazionali:** Sviluppo di sezioni Q\&A basate su domande reali poste dai corsisti (es. "Posso fare il corso BST se non ho ancora il libretto di sbarco?"), ottimizzate per la ricerca vocale e i motori di risposta.50

| Azione SEO/GEO        | Risultato Atteso                               | Impatto Business                             |
| :-------------------- | :--------------------------------------------- | :------------------------------------------- |
| Markup JSON-LD        | Rich Snippet in SERP Google                    | \+40% Visibilità organica corsi              |
| Cluster Normativi     | Top 3 per keyword "Accordo Stato-Regioni 2025" | Lead Generation B2B qualificata              |
| Semantic Optimization | Citazioni in Perplexity/ChatGPT                | Branding come leader di pensiero nel settore |
| Local SEO Puglia      | Dominio ricerche locali Taranto/Brindisi/Bari  | Riempimento costante aule fisiche            |

## **Panoramica Rilascio**

Il rilascio della piattaforma Academy Tema Safety & Training è pianificato per settembre 2026, con un percorso di implementazione rigoroso suddiviso in milestone tecniche e operative.2 La strategia mira a garantire la massima stabilità del sistema documentale e di pagamento sin dal primo giorno.2

### **Milestone di Progetto**

1. **Pianificazione e Modeling (Aprile \- Maggio):** Definizione del database corsi e dei requisiti documentali per ogni categoria ministeriale. Configurazione iniziale dell'ambiente MedusaJS.10
2. **Sviluppo Core (Giugno \- Luglio):** Implementazione dei workflow di checkout, gestione upload protetto AWS S3 e integrazione dei gateway di pagamento.13 Sviluppo dell'interfaccia Admin personalizzata per la segreteria.39 In questa stessa finestra verrà costruito il Modulo Booking, con il calendario edizioni, la logica di gestione della capienza, la lista d'attesa, i workflow di cancellazione e rimborso scaglionato e la configurazione degli add-on, stimati in circa 7 settimane di sviluppo con supporto di agenti AI.
3. **SEO & Content Ingestion (Luglio \- Agosto):** Caricamento dei 500 corsi con descrizioni ottimizzate SEO/GEO. Configurazione del sistema di alert scadenze e templates Email.14
4. **Testing & QA (Agosto):** Test di carico per simulare l'afflusso simultaneo di centinaia di discenti. User Acceptance Testing (UAT) con il team amministrativo per validare l'efficienza dei nuovi processi.2
5. **Go-Live B2C (Settembre):** Lancio ufficiale. Avvio delle campagne social (LinkedIn/Facebook) coordinate per spingere l'acquisto diretto sul portale.2

### **Gestione del Cambiamento e Supporto Interno**

Il successo del progetto non dipende solo dalla tecnologia, ma dall'adozione da parte dello staff dell'Academy.2

- **Formazione Staff:** Sono previste sessioni di addestramento sull'uso del pannello Admin di Medusa per la validazione documenti e la gestione ordini.2
- **Monitoraggio Post-Lancio:** Per i primi 90 giorni verrà istituito un "war room" tecnico per monitorare ogni transazione e raccogliere feedback dagli utenti finali, intervenendo tempestivamente su eventuali criticità della UX.2
- **Scalabilità Fase 2:** Già in fase di rilascio a settembre, l'architettura sarà pronta per accogliere l'integrazione con Mago e i moduli B2B per la vendita a gruppi aziendali, prevista per l'inizio del 2027\.2

### **Analisi dei Rischi e Mitigazione**

- **Rischio Tecnico:** Complessità dell'upload massivo di documenti pesanti. _Mitigazione:_ Utilizzo di caricamento asincrono e ridimensionamento automatico dei file lato client per non sovraccaricare il server.
- **Rischio Normativo:** Cambiamenti repentini nelle richieste ministeriali. _Mitigazione:_ L'architettura modulare di Medusa permette di aggiungere o modificare campi del checkout in poche ore senza dover riscrivere il sistema.
- **Rischio di Adozione:** Utenti "tradizionalisti" che continuano a chiamare. _Mitigazione:_ Politica commerciale che incentiva l'acquisto online (es. sconti early bird o priorità di conferma) e supporto guidato sul sito.
- **Rischio di Overbooking:** In condizioni di elevata concorrenza sulle prenotazioni — tipica dell'apertura iscrizioni per corsi molto richiesti come il Basic Safety Training — più utenti potrebbero tentare di acquistare l'ultimo posto disponibile simultaneamente. _Mitigazione:_ La transazione di prenotazione sarà atomica a livello database, con un meccanismo di lock sul record dell'edizione che garantisce la sequenzialità del decremento della disponibilità, prevenendo qualsiasi forma di doppia vendita.
- **Rischio di Blocco per Documenti Scaduti:** Discenti con certificati in scadenza imminente potrebbero non completare il processo di iscrizione, generando carrelli abbandonati e chiamate alla segreteria. _Mitigazione:_ Il sistema di alert scadenze (funzionalità 4) notificherà il discente con 90, 60 e 30 giorni di anticipo, consentendo il rinnovo dei titoli prima che diventino un ostacolo all'iscrizione a nuove edizioni.

### **Voci di Costo – Brand Identity**

L'assenza totale di un'identità visiva strutturata è un prerequisito bloccante per l'avvio del progetto digitale: senza linee guida grafiche definite non è possibile progettare il sito, i template email o i materiali di comunicazione. Il piano include pertanto un intervento di **rebranding completo**, con le seguenti voci di costo.

**Opzione di ingresso – Rebranding logo e asset grafici digitali**
_(mockup di ambientazione, no esecutivi da stampa)_

| Opzione             | Prezzo        |
| :------------------ | :------------ |
| 2 varianti grafiche | 1.200 € + IVA |
| 3 varianti grafiche | 1.500 € + IVA |
| Naming aziendale    | 900 € + IVA   |

**Pacchetto completo Brand Identity**

| #   | Attività                                                                                                                                        |
| :-- | :---------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Rebranding logo con 2 proposte                                                                                                                  |
| 2   | Progettazione immagine coordinata con produzione brand manual (linee grafiche, palette colori, tipografie e declinazioni visive)                |
| 3   | Kit materiali grafici in coordinato (biglietti da visita 2 proposte + 5 nominativi inclusi, carta intestata 2 proposte, firma email 2 proposte) |
| 4   | Esportazione logo/copertine per social vari e per web                                                                                           |
| 5   | Impostazione PPT corporate (template vuoto)                                                                                                     |
|     | **Budget totale attività 1–5: 2.850 € + IVA**                                                                                                   |

## **Conclusioni**

Il piano di progetto per l’Academy Tema Safety & Training delinea una visione ambiziosa ma tecnicamente solida per trasformare un ente formatore d'eccellenza in una potenza digitale automatizzata. L'integrazione di MedusaJS permetterà non solo di vendere corsi, ma di orchestrare flussi burocratici complessi, riducendo i costi operativi e migliorando la soddisfazione del discente.

Attraverso l'automazione della raccolta documenti, della conferma sessioni e degli alert scadenze, l'Academy potrà finalmente scalare il proprio business, liberando la segreteria da compiti ripetitivi e permettendo alla direzione di concentrarsi sull'apertura di nuovi settori formativi. La strategia SEO e GEO assicurerà che questa innovazione sia supportata da un flusso costante di nuovi clienti organici, pronti a navigare nel futuro della sicurezza professionale con Academy Tema Safety & Training. Il go-live di settembre rappresenta solo l'inizio di una roadmap di crescita continua, scalabile verso il mercato B2B e integrata con i sistemi aziendali.
