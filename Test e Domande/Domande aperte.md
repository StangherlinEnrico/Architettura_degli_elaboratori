# SEZIONE A: CONCETTI FONDAMENTALI E ARCHITETTURA DI BASE

## A.1

**Descrivere la differenza tra architettura e organizzazione di un calcolatore.**

- **Architettura**: Riguarda le caratteristiche del sistema visibili al programmatore, come il set di istruzioni, il numero di bit utilizzati per rappresentare i dati e le tecniche di indirizzamento della memoria.
- **Organizzazione**: Si riferisce alle unità operative e alle loro interconnessioni che realizzano fisicamente le specifiche architetturali, come le interfacce con le periferiche e le tecnologie di memoria impiegate.

---

## A.2

**Descrivere l'architettura di Von Neumann.**

- **Principi fondamentali**
	1. **Memoria condivisa**: Dato e istruzioni risiedono nella stessa memoria principale, accessibile sia in lettura che in scrittura.
	2. **Indirizzamento**: La memoria è organizzata in celle indirizzabili per locazione, indipendentemente dal contenuto.
	3. **Sequenzialità**: Le istruzioni vengono eseguire in ordine sequenziale, salvo salti espliciti.
- **Componenti strutturali**
	- **CPU**: Costituita da un'Unità di Controllo (che interpreta le istruzioni) e una ALU (per operazioni logico-aritmetiche). Utilizza registri critici come il *PC* (Program Counter) per tracciare la prossima istruzioni e l'*IR* (Instruction Register) per l'istruzione corrente.
	- **Memoria Principale**: Immagazzina il programma e i dati.
	- **Moduli I/O**: Gestiscono il trasferimento dati verso le periferiche esterne.
	- **Bus di Sistema**: Collega CPU, memoria e I/O.
- **Funzionamento**
  Si basa sul concetto di "programma memorizzato", eseguendo ciclicamente due fasi principali: **Fetch** (prelievo dell'istruzione dalla memoria) ed **Execute** (esecuzione dell'operazione).

---

## A.3

**Cos'è un programma? Qual è la differenza tra programma hardware e programma software?**

- **Programma**: è una sequenza di istruzioni che dirige il funzionamento del calcolatore. Secondo l'architettura di Von Neumann, il programma può essere modificato e risiede nella memoria insieme ai dati.
	- **Programma Hardware** ("cablato"): è realizzato tramite configurazioni fisse di circuiti elettronici. È non modificabile e dedicato esclusivamente all'esecuzione di operazioni predeterminate.
	- **Programma Software**: si basa su un hardware generico programmabile. I codici di istruzione vengono forniti a un interprete (parte della CPU) che li traduce in segnali di controllo. Questo permette di cambiare il programma, e quindi le operazioni svolte, senza dover modificare fisicamente i circuiti.

---

## A.4

**Descrivere in dettaglio il ciclo di fetch/execute delle istruzioni.**

1. **Instruction Address Calculation**: determina l'indirizzo della prossima istruzione da eseguire.
2. **Instruction Fetch**: l'istruzione viene letta dalla memoria e trasferita alla CPU.
3. **Instruction Operation Decoding**: l'unità di controllo analizza il codice operativo per identificare l'operazione e i modi di indirizzamento.
4. **Operand Address Calculation**: se l'operazione richiede dati in memoria, ne viene calcolato l'indirizzo (può includere l'indirizzamento indiretto).
5. **Operand Fetch**: i dati (operandi) vengono prelevati dalla memoria o dai registri.
6. **Data Operation (Execute)**: l'ALU esegue l'operazione logica o aritmetica richiesta.
7. **Operand Store Calculation**: si determina l'indirizzo di destinazione del risultato.
8. **Store Result**: il risultato viene scritto in memoria o in un registro.

*Aggiunta* **Gestione delle interruzioni**: Al termine della fase di esecuzione e scrittura, la CPU controlla se sono presenti richieste di interruzione. Se abilitate e presenti, la CPU sospende il programma corrente, salva il contesto (PC e registri) e salta alla routine di gestione dell'interruzione.

---

## A.5

**Descrivere in dettaglio il ciclo di esecuzione con trattamento delle interruzioni.**

- **Ciclo base (Fetch/Execute)**:
	- **Instruction Fetch**: calcolo dell'indirizzo e prelievo dell'istruzione dalla memoria.
	- **Decode & Operand Fetch**: decodifica dell'operazione, calcolo dell'indirizzo degli operandi e loro prelievo.
	- **Execute & Store**: elaborazione dei dati e scrittura del risultato in memoria o nei registri.
- **Fase di controllo Interruzioni**: al termine della fase di esecuzione (e scrittura), la CPU verifica se sono richieste di interruzione pendenti e se queste sono abilitate.
- **SE INTERRUZIONE PRESENTE**
	1. **Sospensione**: l'esecuzione del programma corrente viene sospesa.
	2. **Salvataggio del contesto**: il contenuto del PC (indirizzo della prossima istruzione da eseguire) e dei registri (PSW) viene salvato in una locazione di memoria specifica (spesso nello stack).
	3. **Caricamento del Gestore**: il PC viene aggiornato con l'indirizzo della routine di gestione dell'interruzione (interrupt handler)
	4. **Esecuzione e Ripristino**: la CPU esegue la routine di servizio e, al termine, ripristina il contesto salvato per riprendere il programma interrotto dal punto esatto in cui era stato sospeso.

---

---

# SEZIONE B: BUS E COMUNICAZIONE

## B.1

**Spiegare a cosa serve il bus di sistema, com'è strutturato e in che modo viene utilizzato dal calcolatore.**

**Funzione**: il bus di sistema è il principale mezzo di interconnessione che collega la CPU, la memoria principale e i modulo I/O, permettendo il trasferimento di dati tra queste unità. Essendo un mezzo condiviso, i segnali trasmessi da un dispositivo sono disponibili a tutti gli altri, ma solo un dispositivo alla volta può trasmettere.
**Struttura**: è costituito da un insieme di linee parallele suddivise in tre gruppi funzionali:
- **Bus dati**: trasporta dati e istruzioni. La sua ampiezza (numero di linee) influisce direttamente sulle prestazioni del sistema.
- **Bus indirizzi**: identifica la sorgente o la destinazione dei dati. La sua ampiezza determina la massima capacità di memoria indirizzabile.
- **Bus di controllo**: trasmette segnali di comando e temporizzazione per gestire l'accesso e l'uso delle linee dati e indirizzi.
**Utilizzo**: il calcolatore utilizza il bus per coordinare le operazioni tra le unità. Per inviare dati, un dispositivo deve prima ottenere l'accesso al bus e poi trasferire l'informazione; per ricevere dati, deve ottenere l'accesso, inviare una richiesta e attendere la risposta. Le operazioni sono spesso sincronizzate da un clock condiviso (temporizzazione sincrona), dove gli eventi iniziano all'avvio del ciclo di clock.

---

## B.2

**Descrivere la differenza tra bus singoli e multipli.**

- **Bus Singolo**: tutti i dispositivi (CPU, memoria, I/O) sono collegati a un unico mezzo di trasmissione condiviso. Sebbene semplice, questa architettura soffre di ritardi di propagazione e congestione quando il volume di traffico dati aumenta.
- **Bus Multipli**: il sistema utilizza diverse linee di interconnessione gerarchiche (come bus locale, di sistema, ad alta velocità e di espansione). Questa struttura separa il traffico in base alla velocità e al tipo di dispositivo connesso, migliorando l'efficienza complessiva.

---

---

# SEZIONE C: INTERRUZIONI E I/O

## C.1

**Descrivere il meccanismo degli interrupt e la loro funzione.**

**Funzione**: lo scopo principale è migliorare l'efficienza del sistema evitando che la CPU rimanga inattiva (attesa attiva) mentre attende il completamento di operazioni di I/O, che sono molto più lente del processore. Permettono ai moduli esterni di segnalare alla CPU quando sono pronti.

**Meccanismo di funzionamento**: il ciclo di fetch/execute viene modificato aggiungendo una fase di controllo al termine dell'esecuzione di ogni istruzione. La sequenza è la seguente:

1. **Rilevamento:** La CPU verifica la presenza di segnali di interruzione abilitati.
2. **Sospensione e Salvataggio:** Se presente, il programma corrente viene sospeso e il suo contesto (inclusi il Program Counter e i registri) viene salvato in una locazione specifica (spesso nello stack).
3. **Gestione:** Il PC viene aggiornato con l'indirizzo della routine di gestione dell'interruzione (interrupt handler) che viene quindi eseguita.
4. **Ripristino:** Al termine della routine, il contesto salvato viene recuperato per riprendere il programma interrotto dal punto esatto in cui era stato sospeso.

**Gestione delle priorità**: In caso di interruzioni multiple, la CPU può operare in due modi: disabilitare le nuove interruzioni mentre ne serve una (gestione sequenziale) oppure utilizzare un sistema a priorità, dove un'interruzione più urgente può interromperne una a priorità inferiore (annidamento).

---

## C.2

**Spiegare la differenza fra interruzioni multiple ed interruzioni annidate discutendo criticamente le differenti modalità di trattamento da loro richieste.**

La gestione delle **interruzioni multiple** si riferisce alla strategia adottata dalla CPU quando si verificano più richieste di interruzione simultaneamente o mentre un'altra è in corso.
Esistono due approcci fondamentali:

- **Trattamento Sequenziale (Disabilitazione):** la CPU ignora le nuove richieste mentre sta eseguendo una routine di gestione dell'interruzione. Le interruzioni vengono elaborate rigorosamente una dopo l'altra. Sebbene semplice, questo approccio è critico perché non distingue l'urgenza: eventi prioritari devono attendere il completamento di eventi meno importanti.
- **Trattamento Annidato (Priorità):** consente a un'interruzione a priorità più alta di interrompere una routine di servizio a bassa priorità già in esecuzione. Questo crea un annidamento (nesting) in cui il contesto viene salvato più volte. È la modalità più efficiente per sistemi complessi, poiché garantisce tempi di risposta rapidi per eventi critici, gestendo il ripristino dell'esecuzione "a cascata".

---

## C.3

**Descrivere in dettaglio la gestione da programma I/O (I/O da programma).**

**Definizione e Funzionamento**: è una tecnica in cui la CPU mantiene il controllo diretto e completo sull'operazione di Input/Output.
La procedura segue questi passi:

1. La CPU invia un comando al modulo I/O specificando l'indirizzo del dispositivo e l'operazione (controllo, test o lettura/scrittura).
2. La CPU attende il completamento dell'operazione controllando periodicamente i bit di stato del modulo (tecnica di *polling*).
3. Il modulo I/O non invia interruzioni, ma si limita ad aggiornare il proprio stato.

**Inefficienza (Busy Waiting)**: il principale svantaggio è l'**attesa attiva**: la CPU spreca cicli di elaborazione limitandosi a verificare lo stato del dispositivo, senza poter svolgere altro lavoro utile fino al termine del trasferimento.

**Tipi di comandi I/O**: i comandi inviati dalla CPU si dividono in:

- **Controllo:** per configurare il dispositivo.
- **Test:** per verificare lo stato (es. alimentazione o errori).
- **Lettura/Scrittura:** per il trasferimento effettivo dei dati tramite buffer.

---

## C.4

**Descrivere in che modo vengono gestite le interruzioni (sia per la componente hardware che per quella software) nel caso di I/O interrupt driven.**

**Gestione Hardware**

- **Segnalazione:** il modulo I/O invia un segnale di interruzione alla CPU quando è pronto per il trasferimento, evitando l'attesa attiva del processore.
- **Rilevamento:** la CPU controlla la presenza di interruzioni al termine di ogni ciclo di istruzione.
- **Cambio di contesto:** se l'interruzione è presente, l'hardware salva automaticamente il contesto del programma corrente (Program Counter e registri) nello stack e aggiorna il PC con l'indirizzo della routine di gestione dell'interruzione.

**Gestione Software**

- **Esecuzione del Gestore:** la CPU esegue la routine software (interrupt handler) che effettua la lettura o scrittura dei dati dal modulo I/O.
- **Ripristino:** al termine della routine, il software ripristina il contesto salvato (registri e PC), permettendo al programma interrotto di riprendere l'esecuzione dal punto esatto della sospensione.

---

## C.5

**Descrivere la gestione di I/O tramite DMA.**

**Definizione e Scopo**: è una tecnica che utilizza un modulo hardware aggiuntivo (controllore DMA) per gestire il trasferimento diretto dei dati tra memoria centrale e periferiche. Il suo scopo è minimizzare l'intervento della CPU, che altrimenti sarebbe limitata dal dover gestire ogni singolo dato, migliorando l'efficienza complessiva del sistema.

**Procedura di Funzionamento**: il processo si articola in tre fasi:

1. **Inizializzazione:** la CPU invia al DMA i parametri del trasferimento: tipo di operazione (lettura/scrittura), indirizzo del dispositivo, indirizzo di memoria di partenza e quantità di dati da trasferire.
2. **Trasferimento:** la CPU delega il lavoro e prosegue con altre attività. Il controllore DMA gestisce autonomamente il transito dei dati sul bus.
3. **Conclusione:** al termine del trasferimento, il DMA invia un segnale di interruzione alla CPU per notificare il completamento.

**Modalità di Accesso al Bus**: il DMA può operare principalmente in due modi:

- **Cycle Stealing:** il DMA ottiene il controllo del bus per un solo ciclo alla volta (trasferendo una parola). La CPU viene sospesa solo per quell'istante, senza cambiare contesto.
- **Burst Mode:** il DMA acquisisce il bus per una serie consecutiva di trasferimenti (blocchi di dati). È più efficiente nell'uso del bus, ma blocca la CPU per un periodo più lungo.

**Configurazioni**: l'efficienza dipende anche dalla connessione fisica:

- **Bus singolo (controller isolato):** il bus viene usato due volte per parola (I/O $\rightarrow$ DMA, DMA $\rightarrow$ Memoria).
- **Bus singolo (controller integrato) o Bus I/O separato:** il bus di sistema viene usato una sola volta (DMA $\rightarrow$ Memoria), ottimizzando le prestazioni,.

---

## C.6

**Descrivere le tecniche di gestione Input/Output (I/O da programma, I/O guidato da interrupt, DMA) e le diverse configurazioni del bus di sistema per il DMA.**

**Tecniche di Gestione I/O**

- **I/O da Programma:** la CPU mantiene il controllo diretto sull'operazione. Invia il comando e attende attivamente (busy waiting) che il dispositivo sia pronto controllandone ciclicamente lo stato. È una tecnica inefficiente poiché la CPU spreca cicli di elaborazione in attesa,.
- **I/O guidato da Interrupt:** elimina l'attesa attiva. La CPU invia il comando al modulo I/O e prosegue con altri compiti. Quando il dispositivo è pronto, il modulo invia un'interruzione alla CPU, che sospende il programma corrente, esegue il trasferimento dati e poi riprende l'esecuzione,.
- **DMA (Direct Memory Access):** utilizza un processore dedicato (DMA controller) per gestire il trasferimento diretto di blocchi di dati tra memoria e periferiche. La CPU si limita a inizializzare il trasferimento (indicando indirizzi e quantità di dati) e viene interrotta solo al termine dell'operazione, minimizzando il suo coinvolgimento.

**Configurazioni del Bus di Sistema per il DMA**

- **Bus singolo, controller isolato:** il DMA e i moduli I/O sono separati e condividono il bus di sistema. È poco efficiente poiché ogni trasferimento richiede l'uso del bus due volte (dal dispositivo al DMA e dal DMA alla memoria).
- **Bus singolo, controller integrato:** il DMA è integrato nel modulo I/O (o viceversa). Il bus viene impegnato una sola volta per trasferire il dato direttamente in memoria.
- **Bus di I/O separato:** i moduli I/O risiedono su un bus separato gestito dal DMA. Il bus di sistema viene utilizzato una sola volta per lo scambio con la memoria, ottimizzando le prestazioni complessive.

---

---

# SEZIONE D: MEMORIA E GERARCHIA DI MEMORIA

## D.1

**Si descrivano le caratteristiche e le operazioni principali delle memorie a semiconduttore, considerando sia memorie RAM che ROM.**

**Classificazione generale**: le memorie a semiconduttore si dividono in due macro-categorie in base alla volatilità:
- **Memorie volatili (RAM)**: perdono i dati alla rimozione dell'alimentazione
- **Memorie non volatili (ROM e derivate)**: conservano i dati permanentemente

**RAM (Random Access Memory)**

Caratteristiche generali: accesso casuale, lettura/scrittura, volatilità, memorizzazione temporanea. Si distinguono in statiche (SRAM) e dinamiche (DRAM).

- **DRAM**: i bit sono memorizzati come cariche elettriche in condensatori. La carica decade nel tempo, rendendo necessario un refresh periodico. Ogni cella è costituita da un transistor e un condensatore. Sono più lente delle SRAM ma meno costose. Impiegate per la memoria principale.
  - *Write*: si applica tensione alla linea di bit, poi si attiva la linea indirizzo per trasferire la carica al condensatore.
  - *Read*: si seleziona la linea indirizzo, il transistor si attiva, la carica fluisce verso un amplificatore che determina il valore. Dopo la lettura serve refresh.

- **SRAM**: i bit sono memorizzati tramite flip-flop (porte logiche). Non c'è perdita di carica né necessità di refresh. Costruzione più complessa (6 transistor per cella), costo maggiore, velocità superiore. Usate per la cache.

**ROM (Read Only Memory)**

Memorie non volatili destinate alla memorizzazione permanente di microprogrammi, BIOS, subroutine di libreria.

- **ROM a maschera**: scritte in fase di produzione; antieconomiche per piccole tirature.
- **PROM**: programmabili una sola volta dall'utente con strumentazione speciale.
- **EPROM**: cancellabili interamente tramite raggi ultravioletti.
- **EEPROM**: cancellabili elettricamente byte per byte; la scrittura è molto più lenta della lettura.
- **Flash**: cancellazione elettrica a livello di blocchi; evoluzione delle EEPROM.

---

## D.2

**Spiegare in dettaglio le differenze tra DRAM ed SRAM discutendone vantaggi e svantaggi.**

|Caratteristica|DRAM|SRAM|
|---|---|---|
|Volatilità|Sì|Sì|
|Meccanismo di memorizzazione|Cariche in condensatori|Flip-flop (porte logiche)|
|Complessità costruttiva|Bassa (1 transistor + 1 condensatore)|Alta (6 transistor per cella)|
|Densità|Alta|Bassa|
|Costo|Basso|Alto|
|Refresh|Necessario (carica decade)|Non necessario|
|Velocità|Minore|Maggiore|
|Impiego tipico|Memoria principale|Cache|

**Vantaggi DRAM**:
- Costruzione semplice e costo contenuto
- Alta densità (più memoria per unità di superficie)
- Economica per grandi capacità

**Svantaggi DRAM**:
- Più lenta delle SRAM
- Richiede circuiti di refresh che complicano il sistema
- Operazione sostanzialmente analogica

**Vantaggi SRAM**:
- Velocità superiore
- Nessuna necessità di refresh
- Operazione puramente digitale

**Svantaggi SRAM**:
- Costruzione complessa
- Costo maggiore
- Bassa densità

---

## D.3

**Descrivere in dettaglio le memorie DRAM.**

**Definizione**: le DRAM (Dynamic RAM) sono memorie volatili ad accesso casuale in cui i bit sono memorizzati come cariche elettriche in condensatori.

**Struttura della cella**: ogni cella è costituita da un transistor e un condensatore. La carica presente nel condensatore determina il valore digitale (0 o 1). Le DRAM operano in modo sostanzialmente analogico: il livello di carica viene confrontato con un riferimento per determinare il valore.

**Problema del decadimento**: la carica nel condensatore decade nel tempo anche durante l'alimentazione, rendendo necessario un **refresh periodico** per mantenere i dati.

**Operazioni**:
- **Write**: si applica tensione alla linea di bit (alta = 1, bassa = 0), poi si attiva la linea indirizzo per trasferire la carica al condensatore.
- **Read**: si seleziona la linea indirizzo, il transistor si attiva, la carica fluisce verso un amplificatore che la confronta con un riferimento per determinare il valore. Dopo la lettura, la carica deve essere ristabilita (refresh).

**Caratteristiche**:
- Costruzione semplice e costo contenuto
- Alta densità (molti bit per unità di superficie)
- Più lente delle SRAM
- Richiedono circuiti di refresh

**Impiego tipico**: memoria principale del calcolatore, dove è necessaria grande capacità a costo contenuto.

---

## D.4

**Descrivere le memorie ROM e le loro varianti (PROM, EPROM, EEPROM, Flash Memory).**

**ROM (Read Only Memory)**: memorie non volatili destinate alla memorizzazione permanente di microprogrammi, BIOS, subroutine di libreria e funzioni tabulate.

**Varianti**:

- **ROM a maschera**: scritte in fase di produzione mediante un processo fotolitografico. Il contenuto è definito dalla maschera utilizzata durante la fabbricazione. Antieconomiche per piccole tirature, convenienti solo per produzioni su larga scala.

- **PROM (Programmable ROM)**: programmabili una sola volta dall'utente con strumentazione speciale (programmatore di PROM). Permettono la personalizzazione dopo la produzione, ma una volta scritte non possono essere modificate.

- **EPROM (Erasable PROM)**: cancellabili interamente tramite esposizione a raggi ultravioletti attraverso una finestra trasparente sul chip. Dopo la cancellazione possono essere riprogrammate. La cancellazione è totale (non selettiva).

- **EEPROM (Electrically Erasable PROM)**: cancellabili elettricamente byte per byte, senza necessità di rimuovere il chip dal circuito. La scrittura è molto più lenta della lettura. Permettono la modifica selettiva di singole locazioni.

- **Flash Memory**: evoluzione delle EEPROM con cancellazione elettrica a livello di blocchi (non byte per byte). Più veloce delle EEPROM per operazioni di cancellazione/scrittura di grandi quantità di dati. Utilizzata in chiavette USB, schede SD e SSD.

---

## D.5

**Spiegare cos'è un blocco e perché la memoria viene suddivisa in blocchi.**

**Definizione**: un blocco è l'unità minima indivisibile di trasferimento tra livelli adiacenti della gerarchia di memoria. La memoria è logicamente suddivisa in blocchi di dimensione fissa.

**Struttura dell'indirizzo**: l'indirizzo di un dato è composto da:
- Indirizzo del blocco (identifica quale blocco contiene il dato)
- Posizione del dato all'interno del blocco (offset)

**Motivazioni della suddivisione in blocchi**:

1. **Sfruttamento della località spaziale**: i programmi tendono ad accedere a locazioni di memoria contigue. Trasferendo un intero blocco, si caricano anche i dati vicini che probabilmente verranno richiesti a breve.

2. **Efficienza del trasferimento**: trasferire blocchi invece di singole parole riduce l'overhead di gestione e sfrutta meglio la banda del bus. Il trasferimento cache-memoria centrale avviene per blocco.

3. **Ottimizzazione del tempo medio di accesso**: il tempo medio di accesso dipende dalla probabilità di hit, che aumenta trasferendo blocchi più grandi (entro certi limiti).

4. **Semplificazione della gestione**: la gestione della cache è più semplice con unità di trasferimento predefinite piuttosto che con trasferimenti di dimensione variabile.

**Trade-off**: blocchi più grandi sfruttano meglio la località spaziale ma aumentano il tempo di trasferimento e possono causare più miss per conflitto.

---

## D.6

**Nel contesto di una gerarchia di memoria, spiegare i possibili modi di realizzazione del mapping dei blocchi (direct mapping, fully associative, set associative), discutendo criticamente vantaggi e svantaggi di ogni modo.**

**Associazione Diretta (Direct Mapping)**

Ogni blocco del livello inferiore può essere allocato in una sola specifica posizione (linea/slot) del livello superiore.

**Formula**: ILS = ILI mod N (dove ILS = Indirizzo Livello Superiore, ILI = Indirizzo Livello Inferiore, N = numero di blocchi in cache)

**Struttura indirizzo**: etichetta (tag) | linea | parola

*Vantaggi*:
- Semplicità di traduzione e implementazione
- Determinazione veloce di hit/miss
- Hardware semplice ed economico

*Svantaggi*:
- Swap frequenti per accesso a dati di blocchi che mappano sulla stessa linea (miss per conflitto)
- Necessità di tag per identificare il blocco presente

**Associazione Completa (Fully Associative)**

Ogni blocco del livello inferiore può essere posto in qualunque posizione del livello superiore.

**Struttura indirizzo**: etichetta (tag) | parola

*Vantaggi*:
- Massima efficienza di allocazione
- Eliminazione dei miss per conflitto
- Massima flessibilità

*Svantaggi*:
- Determinazione onerosa della corrispondenza ILS-ILI
- Verifica hit/miss richiede confronto con tutte le etichette (hardware complesso)
- Costo elevato per cache grandi

**Associazione a Gruppi (K-way Set Associative)**

Ogni blocco può essere allocato liberamente in uno specifico gruppo di K posizioni del livello superiore.

**Struttura indirizzo**: etichetta (tag) | set | parola

*Vantaggi*:
- Compromesso ottimale tra associazione diretta e completa
- Buona efficienza di allocazione
- Complessità di ricerca accettabile (solo K confronti)

*Svantaggi*:
- Hardware più complesso della mappatura diretta
- Necessita di politica di rimpiazzo all'interno del set

L'hit ratio migliora all'aumentare del grado di associatività e della dimensione della cache. Vale la **regola del 2:1**: una cache a N blocchi con associazione diretta ha prestazioni simili a una cache N/2 con associazione a 2 vie.

---

## D.7

**Descrivere le politiche di rimpiazzo della cache (FIFO, LFU, LRU).**

Le politiche di rimpiazzo determinano quale blocco sostituire in cache quando si verifica un miss e la cache è piena (o il set è pieno nell'associazione a gruppi).

**FIFO (First In First Out)**

Sostituisce il blocco rimasto più a lungo in cache, indipendentemente da quanto recentemente sia stato utilizzato.

*Vantaggi*: implementazione semplice (basta una coda)
*Svantaggi*: non considera la frequenza d'uso; può rimuovere blocchi usati frequentemente

**LFU (Least Frequently Used)**

Sostituisce il blocco con il minor numero di accessi. Mantiene un contatore per ogni blocco.

*Vantaggi*: preserva i blocchi usati più frequentemente
*Svantaggi*: può mantenere blocchi usati molto in passato ma non più necessari; overhead per i contatori

**LRU (Least Recently Used)**

Sostituisce il blocco che non è stato acceduto da più tempo. Preserva la località temporale.

*Vantaggi*: sfrutta efficacemente la località temporale; prestazioni generalmente superiori
*Svantaggi*: implementazione più complessa (richiede tracciamento degli accessi)

**Casuale**

Seleziona casualmente il blocco da sostituire.

*Vantaggi*: implementazione semplicissima; occupazione omogenea dello spazio
*Svantaggi*: non sfrutta la località

**Confronto**: LRU offre probabilità di miss inferiori rispetto al rimpiazzo casuale, specialmente con cache piccole e alta associatività. Tuttavia, per gradi di associatività elevati, le differenze tra le politiche si riducono.

---

## D.8

**Nel contesto di una gerarchia di memoria spiegare come funzionano le politiche di scrittura write-through e write-back. Per ogni politica discutere criticamente i problemi che possono sorgere nell'adottarla.**

La scrittura genera incoerenza tra il blocco in cache e il blocco corrispondente nei livelli inferiori. Le politiche di scrittura definiscono come gestire questa incoerenza.

**Write Through**

**Funzionamento**: ogni scrittura in cache viene propagata contemporaneamente anche al livello inferiore (memoria principale). I dati sono sempre coerenti tra i livelli.

*Vantaggi*:
- Dati sempre coerenti tra cache e memoria
- Recupero da guasti semplificato
- Facilita la gestione in sistemi multiprocessore

*Problemi*:
- Aumento del traffico sul bus per scritture frequenti nello stesso blocco
- Rallentamento delle operazioni di scrittura (devono attendere la memoria più lenta)
- Soluzione parziale: utilizzo di buffer di scrittura asincroni per non bloccare la CPU

**Write Back**

**Funzionamento**: la scrittura in memoria inferiore viene differita al momento del rimpiazzo del blocco. Si utilizza un **dirty bit** per ricordare se sono avvenute scritture sul blocco.

*Vantaggi*:
- Ottimizza il traffico tra livelli (scritture multiple sullo stesso blocco generano un solo trasferimento)
- Maggiore velocità delle operazioni di scrittura
- Minor utilizzo del bus

*Problemi*:
- Causa periodi di incoerenza tra cache e memoria
- Problematico con operazioni di I/O (DMA potrebbe leggere dati obsoleti)
- Complica la gestione in sistemi multiprocessore (problema della coerenza delle cache)
- Soluzioni: monitoraggio del bus, trasparenza hardware, memoria non-cacheable per dati condivisi

---

## D.9

**Nel contesto di una gerarchia di memoria, spiegare come i miss possono essere categorizzati in diversi tipi e dire quali strategie, per ogni tipo, si possono adottare per diminuirne il numero.**

**Tipi di Miss (le "3 C")**

**1. Miss di primo accesso (Compulsory Miss)**

Si verifica quando un blocco viene acceduto per la prima volta. È inevitabile poiché il dato non può essere in cache se non è mai stato richiesto.

*Strategie di riduzione*:
- Aumentare la dimensione del blocco (si caricano più dati contigui)
- Prefetching (caricare anticipatamente blocchi che si prevede saranno necessari)

**2. Miss per capacità insufficiente (Capacity Miss)**

Si verifica quando la cache non può contenere tutti i blocchi necessari all'esecuzione del programma e deve rimpiazzare blocchi che verranno richiesti nuovamente.

*Strategie di riduzione*:
- Aumentare la dimensione della cache
- Ottimizzazione del codice tramite compilatore (migliorare la località)
- Cache multilivello (L1, L2, L3)

**3. Miss per conflitto (Conflict Miss)**

Si verifica quando più blocchi mappano sulla stessa linea/gruppo e si contendono lo spazio, anche se la cache non è piena. Tipico dell'associazione diretta.

*Strategie di riduzione*:
- Aumentare l'associatività (passare da direct mapping a set-associative)
- Victim cache (piccola cache fully-associative per blocchi rimpiazzati di recente)
- Ottimizzazione del compilatore nel posizionamento dei dati

**Altre tecniche generali**:
- Separazione tra cache dati e cache istruzioni
- Fusione di vettori in strutture (migliora località spaziale)
- Trasformazione di iterazioni annidate (accesso sequenziale alla memoria)

---

## D.10

**Si metta a confronto criticamente il modo in cui un'architettura RISC utilizza l'ampio banco di registri a sua disposizione rispetto alla gestione di una cache.**

|Aspetto|Ampio banco di registri a finestre|Cache|
|---|---|---|
|**Contenuto**|Tutti gli scalari locali della procedura attiva|Scalari locali usati di recente|
|**Granularità**|Variabili individuali|Blocchi di memoria|
|**Variabili globali**|Assegnate dal compilatore a registri dedicati|Caricate se usate di recente|
|**Save/Restore**|Basato sulla profondità di annidamento delle chiamate|Basato sull'algoritmo di sostituzione (LRU, FIFO, ecc.)|
|**Indirizzamento**|A registro (pochi bit)|A memoria (indirizzo completo + confronto tag)|

**Vantaggi dell'approccio RISC con registri a finestre**:

- **Velocità di accesso agli scalari locali**: per riferirsi a scalari locali i registri sono più veloci. Con le finestre basta combinare numero di finestra e numero di registro relativo, mentre con cache serve un intero indirizzo di memoria con confronto dei tag.

- **Gestione automatica delle chiamate di procedura**: il cambio di finestra è automatico e non richiede salvataggio/ripristino esplicito dei registri (finché non si esaurisce il buffer circolare).

- **Passaggio parametri efficiente**: i temporary registers di una procedura coincidono con i parameter registers della procedura chiamata, permettendo il passaggio parametri senza trasferire dati.

**Vantaggi della cache**:

- **Flessibilità**: gestisce qualsiasi tipo di dato, non solo scalari locali
- **Trasparenza**: non richiede supporto esplicito del compilatore
- **Gestione automatica della località**: sfrutta automaticamente località spaziale e temporale

**Conclusione**: i due meccanismi sono complementari. I registri a finestre ottimizzano l'accesso alle variabili locali scalari (le più frequenti), mentre la cache gestisce array, strutture e variabili globali.

---

---

# SEZIONE E: MEMORIE SECONDARIE E RAID

## E.1

**Descrivere l'organizzazione delle memorie ottiche (dischi magnetici): tracce, settori, velocità angolare costante e registrazione a più zone.**

**Organizzazione dei dati nei dischi magnetici**

I dischi magnetici utilizzano un rivestimento di ossido di ferro su substrato di vetro (precedentemente alluminio). I dati sono disposti su **tracce concentriche** divise in **settori**.

- **Traccia**: cerchio concentrico sulla superficie del disco
- **Settore**: porzione di traccia contenente un blocco di dati (dimensione minima di blocco)
- **Cilindro**: insieme delle tracce in stessa posizione su piatti multipli (nei dischi con più superfici)

**Velocità Angolare Costante (CAV - Constant Angular Velocity)**

Il disco ruota sempre alla stessa velocità angolare, indipendentemente dalla posizione della testina.

*Caratteristiche*:
- Stesso numero di bit per traccia (indipendentemente dal raggio)
- Densità lineare maggiore nelle tracce interne
- Spreco di spazio nelle tracce esterne (potrebbero contenere più dati)
- Accesso più semplice (non serve regolare la velocità)

**Registrazione a Più Zone (MZR - Multiple Zone Recording)**

Il disco è diviso in zone concentriche, con più settori nelle tracce esterne rispetto a quelle interne.

*Caratteristiche*:
- Densità lineare approssimativamente uniforme su tutto il disco
- Maggiore capacità complessiva rispetto a CAV
- Complessità maggiore nel posizionamento
- Il numero di settori per traccia varia da zona a zona

**Meccanismo di lettura/scrittura**

- **Scrittura**: la corrente nella testina genera campi magnetici con direzione opposta per rappresentare 0 e 1
- **Lettura tradizionale**: i campi magnetici inducono corrente nella bobina
- **Lettura moderna**: sensori magneto-resistivi (MR) che variano la resistenza elettrica in base alla direzione del campo

---

## E.2

**Descrivere le prestazioni del disco (seek time, latenza, access time, transfer time, tempo totale).**

Il tempo di accesso ai dati su un disco magnetico è composto da diverse componenti:

**Seek Time (Tempo di posizionamento)**

Tempo necessario per spostare la testina dalla traccia corrente alla traccia desiderata. Dipende dalla distanza da percorrere.

- Valore tipico: 5-20 ms
- È la componente più significativa del tempo di accesso

**Latenza Rotazionale (Rotational Latency)**

Tempo di attesa affinché il settore desiderato passi sotto la testina dopo che questa è stata posizionata sulla traccia corretta.

- Dipende dalla velocità di rotazione (RPM) del disco
- In media: metà del tempo di una rotazione completa
- Esempio: disco a 7200 RPM → rotazione completa = 8.33 ms → latenza media = 4.17 ms

**Access Time (Tempo di accesso)**

Somma di seek time e latenza rotazionale:
$$T_{access} = T_{seek} + T_{latency}$$

Rappresenta il tempo per posizionarsi sul primo bit del dato richiesto.

**Transfer Time (Tempo di trasferimento)**

Tempo necessario per trasferire effettivamente i dati una volta che la testina è posizionata.

$$T_{transfer} = \frac{b}{r \cdot N}$$

Dove:
- b = byte da trasferire
- N = byte per traccia
- r = rotazioni al secondo

**Tempo Totale**

$$T_{totale} = T_{seek} + T_{latency} + T_{transfer}$$

Per accessi multipli, il tempo totale dipende anche dalla strategia di scheduling degli accessi al disco.

---

## E.3

**Discutere le ragioni per cui è stato sviluppato il sistema RAID e descrivere in dettaglio i vari livelli (RAID 0-6).**

**Motivazioni del RAID**

RAID (Redundant Array of Independent/Inexpensive Disks) è stato sviluppato per:
- Aumentare le prestazioni tramite parallelismo
- Migliorare l'affidabilità tramite ridondanza
- Utilizzare dischi economici per ottenere capacità e prestazioni elevate
- Presentare più dischi fisici come un unico dispositivo logico al sistema operativo

**Livelli RAID**

**RAID 0 (Striping)**
- Nessuna ridondanza
- Dati distribuiti in strisce con striping round-robin su più dischi
- Aumenta prestazioni tramite parallelismo e riduzione conflitti
- Nessuna tolleranza ai guasti

**RAID 1 (Mirroring)**
- Mirroring completo: ogni dato è scritto su due dischi separati
- Recupero semplice da guasto, nessun downtime
- Lettura parallela possibile
- Costoso (efficienza 50%)

**RAID 2**
- Dischi sincronizzati con accesso parallelo
- Unità di informazione piccole (byte/word)
- Codici di Hamming per correzione errori
- Non commercializzato per elevata ridondanza richiesta

**RAID 3**
- Un solo disco di parità indipendentemente dal numero di dischi
- Parità bit-interleaved
- Alta velocità di trasferimento per accessi sequenziali
- Dati ricostruibili in caso di guasto di un disco

**RAID 4**
- Dischi indipendenti, parità a livello di blocco su disco dedicato
- Ottimo per alte richieste I/O
- Non commercializzato per collo di bottiglia del disco di parità (ogni scrittura richiede aggiornamento della parità)

**RAID 5**
- Come RAID 4 ma con parità distribuita su tutti i dischi (round-robin)
- Elimina il collo di bottiglia del disco di parità
- Comune nei server di rete
- Buon compromesso tra prestazioni, capacità e ridondanza

**RAID 6**
- Doppia parità con metodi distinti su dischi differenti
- Richiede N+2 dischi
- Tollera guasto di due dischi contemporaneamente
- Scrittura più lenta (doppio calcolo di parità)

---

## E.4

**Descrivere l'organizzazione delle informazioni nel nastro magnetico.**

**Caratteristiche generali**

Il nastro magnetico è un supporto di memorizzazione caratterizzato da:
- **Accesso seriale**: i dati devono essere letti in sequenza
- **Velocità**: lento rispetto a dischi magnetici e SSD
- **Costo**: molto economico per unità di storage
- **Utilizzo tipico**: backup e archiviazione a lungo termine

**Organizzazione fisica**

I dati sono organizzati su **tracce multiple parallele** che corrono lungo la lunghezza del nastro. La lettura/scrittura avviene con un pattern **a serpentina**: la testina legge una traccia in una direzione, poi passa alla traccia adiacente e la legge in direzione opposta.

**Struttura dei dati**

- I dati sono organizzati in **blocchi** separati da gap inter-blocco
- I gap permettono l'avvio e l'arresto del nastro tra operazioni
- I blocchi possono contenere uno o più record logici

**Vantaggi**
- Costo per GB molto basso
- Grande capacità di archiviazione
- Durata elevata per conservazione a lungo termine
- Basso consumo energetico quando non in uso

**Svantaggi**
- Accesso sequenziale (tempo di accesso elevato per dati non consecutivi)
- Lentezza nelle operazioni
- Non adatto per accesso casuale frequente

---

---

# SEZIONE F: RILEVAMENTO E CORREZIONE ERRORI

## F.1

**Descrivere come vengono gestiti gli errori in un calcolatore e spiegare il codice di Hamming.**

**Tipologie di errore**

- **Hard Failure**: guasti hardware permanenti che richiedono riparazione o sostituzione del componente.
- **Soft Error**: errori casuali, non distruttivi, che non danneggiano permanentemente la memoria. Possono essere causati da radiazioni, interferenze elettromagnetiche o fluttuazioni di tensione.

**Gestione degli errori**

Gli errori vengono gestiti tramite codici di rilevamento e correzione che aggiungono bit ridondanti ai dati.

**Codice di Hamming**

Il codice di Hamming permette di **rilevare e correggere errori singoli** (SEC - Single Error Correction).

**Struttura**:
- I bit di controllo (K) sono inseriti nelle posizioni che sono potenze di 2: 1, 2, 4, 8, 16, ...
- Il numero di bit di controllo necessari soddisfa: $2^K - 1 \geq M + K$ dove M è il numero di bit di dati

**Overhead per bit di controllo**:

|Bit di dati|Bit di controllo|% incremento|
|---|---|---|
|8|4|50%|
|16|5|31,25%|
|32|6|18,75%|
|64|7|10,94%|

**Funzionamento**:
- Ogni bit di controllo verifica un sottoinsieme di posizioni determinato dalla rappresentazione binaria della posizione stessa
- Il calcolo dei bit di controllo avviene tramite operazione **XOR** sui bit di dati corrispondenti
- In lettura, il confronto tra i bit di controllo memorizzati e quelli ricalcolati genera una **sindrome** che indica la posizione dell'eventuale errore (se la sindrome è zero, nessun errore)

**SEC-DED (Single Error Correction, Double Error Detection)**

L'estensione SEC-DED aggiunge un ulteriore bit di parità globale, consentendo di:
- Correggere errori singoli
- Rilevare (senza correggere) errori doppi

---

---

# SEZIONE G: RAPPRESENTAZIONE DEI NUMERI

## G.1

**Spiegare la differenza tra la codifica dei numeri interi in complemento a 2 rispetto a quella in modulo e segno.**

**Rappresentazione in Modulo e Segno**

Il bit più a sinistra indica il segno (0 = positivo, 1 = negativo), il resto rappresenta il valore assoluto.

Esempio su 8 bit: +5 = 00000101, -5 = 10000101

*Problemi*:
- Le operazioni aritmetiche devono considerare separatamente modulo e segno
- Esistono due rappresentazioni per lo zero (+0 = 00000000 e -0 = 10000000)
- Hardware più complesso per le operazioni

**Rappresentazione in Complemento a Due**

Il bit più significativo ha peso $-2^{n-1}$. Con n bit si rappresentano numeri da $-2^{n-1}$ a $+2^{n-1}-1$.

Formula generale per una sequenza $a_{n-1}...a_0$:
$$\text{numero} = -2^{n-1} \times a_{n-1} + \sum_{i=0}^{n-2} 2^i \times a_i$$

*Interpretazione*:
- Numeri positivi: bit più significativo a 0, lettura diretta
- Numeri negativi: bit più significativo a 1; -1 è rappresentato da n uni

*Conversione da k a -k*:
- Metodo 1: copiare i bit da destra fino al primo 1 incluso, poi complementare i rimanenti
- Metodo 2: complementare tutti i bit e sommare 1

**Vantaggi del complemento a due**:
- Unica rappresentazione dello zero
- Operazioni aritmetiche semplici (somma e sottrazione con lo stesso circuito)
- Negazione facile
- Overflow rilevabile facilmente

**Intervalli rappresentabili**:
- Su 8 bit: da -128 a +127
- Su 16 bit: da -32768 a +32767

---

## G.2

**Si spieghi in dettaglio la codifica in complemento a due dei numeri interi. Si discutano poi i problemi legati alla realizzazione della moltiplicazione di due interi rappresentati in complemento a due, esemplificando tali problemi su un caso concreto di moltiplicazione.**

**Codifica in Complemento a Due**

Il bit più significativo ha peso $-2^{n-1}$. Con n bit si rappresentano numeri da $-2^{n-1}$ a $+2^{n-1}-1$.

Formula: $\text{numero} = -2^{n-1} \times a_{n-1} + \sum_{i=0}^{n-2} 2^i \times a_i$

- Numeri positivi: bit MSB = 0, gli altri bit rappresentano il valore
- Numeri negativi: bit MSB = 1, il valore è dato dalla formula

Conversione da k a -k:
1. Complementare tutti i bit e sommare 1
2. Oppure: copiare da destra fino al primo 1 incluso, poi complementare

**Problemi della moltiplicazione in complemento a due**

La moltiplicazione diretta (come per i numeri senza segno) **non funziona** se almeno un operando è negativo.

**Esempio concreto** (su 4 bit):
- Moltiplichiamo 3 × (-2)
- 3 in complemento a 2: 0011
- -2 in complemento a 2: 1110

Se eseguiamo la moltiplicazione binaria diretta:
```
    0011  (3)
  × 1110  (-2)
  ------
    0000
   0011
  0011
 0011
--------
 0101010  → interpretato come +42, non -6!
```

Il risultato corretto dovrebbe essere -6 = 11111010 (su 8 bit).

**Soluzioni**:

1. **Conversione**: convertire gli operandi negativi in positivi, moltiplicare, poi correggere il segno del risultato in base ai segni originali.

2. **Algoritmo di Booth**: algoritmo specifico per la moltiplicazione in complemento a due che gestisce correttamente i numeri negativi senza conversioni preliminari.

**Nota**: il prodotto di due numeri a n bit può richiedere 2n bit per essere rappresentato correttamente.

---

## G.3

**Si spieghi in dettaglio l'hardware adottato per la realizzazione della somma e sottrazioni di numeri interi rappresentati in complemento a due.**

**Somma in complemento a due**

La somma si esegue come normale somma binaria, ignorando l'eventuale riporto finale oltre il bit più significativo.

**Hardware: Full Adder (Sommatore completo)**

Un full adder per ogni bit calcola:
- **Sum** = A XOR B XOR Cin (riporto in ingresso)
- **Cout** = (A AND B) OR (Cin AND (A XOR B))

n full adder collegati in cascata (ripple carry adder) realizzano la somma di due numeri a n bit.

**Sottrazione in complemento a due**

La sottrazione A - B si realizza come somma: A + (-B)

Dove -B è ottenuto complementando tutti i bit di B e sommando 1.

**Hardware: Adder/Subtractor**

Lo stesso circuito può eseguire sia somma che sottrazione:
- Un segnale di controllo SUB/ADD
- Se SUB=1: i bit di B passano attraverso XOR con 1 (complemento) e Cin del primo full adder = 1 (somma 1)
- Se SUB=0: i bit di B passano invariati e Cin = 0 (somma normale)

**Rilevamento dell'Overflow**

L'overflow si verifica quando due numeri dello stesso segno producono un risultato di segno opposto.

Condizione: Overflow = Cn XOR Cn-1
(riporto in ingresso al bit di segno diverso dal riporto in uscita)

Oppure: overflow quando i due operandi hanno lo stesso segno ma il risultato ha segno opposto.

**Ottimizzazioni hardware**:
- Carry lookahead adder: calcola i riporti in parallelo per velocizzare l'operazione
- Carry select adder: calcola in parallelo risultati con carry 0 e 1, poi seleziona

---

## G.4

**Spiegare in dettaglio la rappresentazione dei numeri reali secondo lo standard IEEE 754.**

**Rappresentazione in virgola mobile**

I numeri reali si rappresentano come:
$$\pm 1.\text{mantissa} \times 2^{\text{esponente}}$$

**Normalizzazione**: il bit più significativo della mantissa è sempre 1 e non viene memorizzato (bit implicito). La mantissa memorizzata rappresenta la parte frazionaria dopo l'1 implicito.

**Esponente polarizzato (biased)**: si memorizza $e_p$ e il vero esponente è:
$$e = e_p - bias$$

**Formati IEEE 754**

|Formato|Bit totali|Segno|Esponente|Mantissa|Bias|
|---|---|---|---|---|---|
|Singola precisione|32|1|8|23|127|
|Doppia precisione|64|1|11|52|1023|

**Valori speciali**

- **Esponente 1-254, qualsiasi mantissa**: numeri normalizzati $\pm 2^{e-127} \times 1.f$
- **Esponente 0, mantissa 0**: zero (positivo o negativo, a seconda del bit di segno)
- **Esponente 255 (o 2047), mantissa 0**: infinito (positivo o negativo)
- **Esponente 0, mantissa ≠ 0**: numeri denormalizzati $\pm 2^{-126} \times 0.f$ (permettono rappresentazione graduale verso lo zero)
- **Esponente 255 (o 2047), mantissa ≠ 0**: NaN (Not a Number, risultato di operazioni indefinite come 0/0)

**Intervallo rappresentabile (32 bit)**

- Positivi: da circa $2^{-126}$ (≈ 1.18 × 10⁻³⁸) a circa $2^{128}$ (≈ 3.4 × 10³⁸)
- Negativi: simmetrici

**Limitazioni**:
- Overflow: numeri troppo grandi in valore assoluto
- Underflow: numeri troppo piccoli in valore assoluto
- I numeri rappresentabili non sono equidistanti: la densità è maggiore vicino allo zero

---

## G.5

**Spiegare nel dettaglio lo schema per realizzare la moltiplicazione tra numeri reali dello standard IEEE 754.**

**Schema della moltiplicazione in virgola mobile**

Dati due numeri: $A = M_A \times 2^{E_A}$ e $B = M_B \times 2^{E_B}$

Il risultato è: $C = (M_A \times M_B) \times 2^{E_A + E_B}$

**Fasi dell'algoritmo**:

**1. Controllo dello zero**
- Se uno degli operandi è zero, il risultato è zero
- Si evitano calcoli inutili

**2. Somma degli esponenti e sottrazione del bias**
- $E_C = E_A + E_B - bias$
- La sottrazione del bias è necessaria perché entrambi gli esponenti sono già polarizzati
- Esempio (singola precisione): $E_C = (e_A - 127) + (e_B - 127) + 127 = e_A + e_B - 127$

**3. Moltiplicazione delle mantisse**
- Si moltiplicano le mantisse (includendo il bit implicito 1)
- $M_C = M_A \times M_B$
- Il risultato può essere compreso tra 1 e 4 (dato che $1 \leq M_A, M_B < 2$)

**4. Normalizzazione del risultato**
- Se $M_C \geq 2$, si shifta a destra e si incrementa l'esponente
- Si riporta la mantissa nella forma 1.xxx...

**5. Arrotondamento**
- Si arrotonda la mantissa al numero di bit disponibili
- Metodi: al più vicino (default), verso +∞, verso -∞, verso zero

**6. Determinazione del segno**
- $Segno_C = Segno_A \oplus Segno_B$ (XOR dei segni)
- Positivo se i segni sono uguali, negativo se diversi

**7. Controllo eccezioni**
- Overflow dell'esponente (risultato troppo grande)
- Underflow dell'esponente (risultato troppo piccolo)

---

## G.6

**Spiegare in dettaglio la divisione fra numeri reali secondo lo standard IEEE 754.**

**Schema della divisione in virgola mobile**

Dati due numeri: $A = M_A \times 2^{E_A}$ e $B = M_B \times 2^{E_B}$

Il risultato è: $C = \frac{M_A}{M_B} \times 2^{E_A - E_B}$

**Fasi dell'algoritmo**:

**1. Controllo degli operandi speciali**
- Se il dividendo è zero e il divisore non è zero: risultato = 0
- Se il divisore è zero: risultato = infinito (o NaN se anche il dividendo è zero)
- Se uno degli operandi è NaN: risultato = NaN

**2. Sottrazione degli esponenti e aggiunta del bias**
- $E_C = E_A - E_B + bias$
- L'aggiunta del bias compensa la sottrazione (opposto della moltiplicazione)
- Esempio (singola precisione): $E_C = (e_A - 127) - (e_B - 127) + 127 = e_A - e_B + 127$

**3. Divisione delle mantisse**
- Si dividono le mantisse (includendo il bit implicito 1)
- $M_C = \frac{M_A}{M_B}$
- Il risultato può essere compreso tra 0.5 e 2 (dato che $1 \leq M_A, M_B < 2$)

**4. Normalizzazione del risultato**
- Se $M_C < 1$, si shifta a sinistra e si decrementa l'esponente
- Si riporta la mantissa nella forma 1.xxx...

**5. Arrotondamento**
- Si arrotonda la mantissa al numero di bit disponibili
- Metodi: al più vicino (default), verso +∞, verso -∞, verso zero (troncamento)

**6. Determinazione del segno**
- $Segno_C = Segno_A \oplus Segno_B$ (XOR dei segni)
- Positivo se i segni sono uguali, negativo se diversi

**7. Controllo eccezioni**
- Overflow dell'esponente (risultato troppo grande)
- Underflow dell'esponente (risultato troppo piccolo)
- Divisione per zero

---

---

# SEZIONE H: ISTRUZIONI E FORMATI

## H.1

**Quali sono i fattori che determinano la lunghezza delle istruzioni di una CPU?**

La lunghezza delle istruzioni è influenzata da diversi fattori:

**1. Dimensione e organizzazione della memoria**
- Maggiore è lo spazio di memoria indirizzabile, più bit servono per gli indirizzi
- L'organizzazione in segmenti può richiedere campi aggiuntivi

**2. Struttura del bus**
- La larghezza del bus dati influenza la dimensione ottimale delle istruzioni
- Istruzioni allineate con la larghezza del bus ottimizzano il fetch

**3. Complessità e velocità della CPU**
- CPU più complesse supportano più operazioni, richiedendo codici operativi più lunghi
- Maggiore velocità può essere ottenuta con istruzioni più lunghe ma più potenti

**4. Numero di operandi**
- Istruzioni a tre indirizzi richiedono più bit di quelle a un indirizzo
- Trade-off: meno indirizzi = istruzioni più corte ma programmi più lunghi

**5. Modi di indirizzamento supportati**
- Più modi di indirizzamento richiedono bit aggiuntivi per specificarli
- Modi complessi (spiazzamento, indicizzazione) richiedono campi più ampi

**6. Numero di registri**
- Più registri richiedono più bit per identificarli (es. 4 bit per 16 registri, 5 bit per 32)

**7. Granularità di indirizzamento**
- Indirizzamento al byte vs. alla parola influisce sulla dimensione degli indirizzi

**8. Compromesso tra potenza del repertorio e risparmio di spazio**
- Repertorio ampio = codici operativi più lunghi
- Istruzioni potenti = meno istruzioni per programma ma istruzioni più lunghe

---

## H.2

**Discutere nel dettaglio in cosa consiste il formato variabile per le istruzioni. Dare esempi di formati variabili.**

**Definizione**

Il formato variabile prevede istruzioni di lunghezza diversa all'interno dello stesso set di istruzioni. La lunghezza dipende dal tipo di operazione e dagli operandi richiesti.

**Caratteristiche**
- Istruzioni semplici occupano meno spazio
- Istruzioni complesse possono essere più lunghe
- Il decodificatore deve determinare la lunghezza dell'istruzione durante il fetch
- Complessità maggiore nella gestione della pipeline

**Vantaggi**
- Risparmio di spazio per istruzioni semplici
- Flessibilità nella definizione del set di istruzioni
- Possibilità di supportare molti modi di indirizzamento

**Svantaggi**
- Decodifica più complessa
- Difficoltà nell'allineamento delle istruzioni
- Complicazioni nella pipeline (non si sa quanti byte leggere)

**Esempi di architetture con formato variabile**

**PDP-11**: istruzioni da 1 a 3 parole (16 bit ciascuna). I campi sorgente e destinazione contengono 3 bit per modo di indirizzamento e 3 bit per numero di registro.

**VAX**: formato altamente variabile, da 1 a 37 byte. Opcode di 1 o 2 byte seguito da zero a sei specificatori di operando. Ogni specificatore ha 4 bit per il modo di indirizzamento.

**x86**: formato variabile composto da:
- Prefissi opzionali (0-4 byte)
- Opcode (1-3 byte)
- Byte ModR/M opzionale (specifica modo e registri)
- Byte SIB opzionale (scala, indice, base)
- Displacement opzionale (0-4 byte)
- Immediato opzionale (0-4 byte)

---

## H.3

**Si descrivano i possibili formati di codifica di un'istruzione, specificando per ogni formato la sua composizione tipica, i pregi e i difetti.**

**Formato a tre indirizzi**

Composizione: OP A, B, C dove A ← B OP C

*Pregi*: istruzioni potenti, programmi più brevi, nessun registro implicito
*Difetti*: istruzioni molto lunghe, maggiore uso di memoria

**Formato a due indirizzi**

Composizione: OP A, B dove A ← A OP B (un operando è anche destinazione)

*Pregi*: istruzioni più corte rispetto a tre indirizzi, buon compromesso
*Difetti*: un operando viene sovrascritto, potrebbe servire copia preliminare

**Formato a un indirizzo**

Composizione: OP A dove AC ← AC OP A (usa implicitamente l'accumulatore)

*Pregi*: istruzioni corte, hardware semplice
*Difetti*: programmi più lunghi, necessità di istruzioni LOAD/STORE esplicite, collo di bottiglia sull'accumulatore

**Formato a zero indirizzi**

Composizione: opera su una pila (stack) con modalità LIFO

Esempio: push a, push b, add, pop c → calcola c = a + b

*Pregi*: istruzioni molto compatte, nessun campo indirizzo, naturale per espressioni
*Difetti*: overhead per push/pop, non adatto a tutte le operazioni, difficile ottimizzazione

**Formato fisso vs. variabile**

*Formato fisso* (tipico RISC):
- Pregi: decodifica veloce, pipeline efficiente, allineamento garantito
- Difetti: spreco di spazio per istruzioni semplici

*Formato variabile* (tipico CISC):
- Pregi: risparmio di memoria, flessibilità
- Difetti: decodifica complessa, difficoltà nella pipeline

---

## H.4

**Si descrivano i formati delle istruzioni MIPS visti a lezione, discuterne caratteristiche, pregi e difetti.**

L'architettura MIPS (e la derivata RISC-V) utilizza formati fissi a 32 bit con campi in posizioni predefinite.

**Formato R (Register)**

Usato per: istruzioni aritmetiche e logiche tra registri

Struttura (RISC-V): funz7 (7 bit) | rs2 (5 bit) | rs1 (5 bit) | funz3 (3 bit) | rd (5 bit) | codop (7 bit)

Esempio: `add x1, x2, x3` → x1 ← [x2] + [x3]

**Formato I (Immediate)**

Usato per: load, operazioni con immediato, shift

Struttura: immediato (12 bit) | rs1 (5 bit) | funz3 (3 bit) | rd (5 bit) | codop (7 bit)

Esempio: `lw x8, 1200(x9)` → x8 ← Mem[1200 + [x9]]
Esempio: `addi x1, x8, 4` → x1 ← [x8] + 4

**Formato S (Store)**

Usato per: store in memoria

Struttura: immediato diviso (7+5 bit) | rs2 (5 bit) | rs1 (5 bit) | funz3 (3 bit) | codop (7 bit)

Esempio: `sw x1, 1000(x2)` → Mem[[x2]+1000] ← [x1]

**Formato SB (Branch)**

Usato per: salti condizionali. L'immediato codifica offset in multipli di 2.

**Formato U (Upper)**

Usato per: caricamento costanti a 20 bit (LUI, AUIPC)

**Formato UJ (Jump)**

Usato per: salti incondizionati (JAL)

**Pregi**:
- Formato fisso: decodifica veloce e parallela
- Campi in posizioni fisse: accesso ai registri simultaneo alla decodifica
- Allineamento ottimale per il fetch
- Pipeline efficiente

**Difetti**:
- Spreco di bit per istruzioni semplici
- Immediati limitati (12 o 20 bit)
- Per costanti a 32 bit servono due istruzioni

---

## H.5

**Descrivere le operazioni macchina e i tipi di operazioni (trasferimento, aritmetica, I/O, confronto, salto, stop).**

**Operazioni di trasferimento dati**

Spostano dati tra registri, memoria e dispositivi I/O. Specificano sorgente, destinazione e lunghezza del dato.
- LOAD: memoria → registro
- STORE: registro → memoria
- MOVE: registro → registro

**Operazioni aritmetiche**

Eseguono calcoli matematici su numeri interi e in virgola mobile.
- Somma, sottrazione, moltiplicazione, divisione
- Incremento, decremento
- Negazione, valore assoluto

**Operazioni logiche**

Operazioni bit a bit eseguibili in parallelo su tutti i bit di un registro.
- AND, OR, NOT, XOR, EQUAL
- L'AND può fungere da maschera per isolare porzioni di dato

**Operazioni di shift e rotazione**

- Shift logico: inserisce zeri (sinistra o destra)
- Shift aritmetico: preserva il segno (per divisione/moltiplicazione per potenze di 2)
- Rotazione: fa circolare i bit senza perdita

**Operazioni di I/O**

Comunicazione con dispositivi esterni.
- IN: legge da dispositivo
- OUT: scrive su dispositivo

**Operazioni di confronto e test**

Confrontano operandi e impostano flag/codici di condizione.
- Compare: confronta due valori
- Test: verifica condizioni specifiche (zero, negativo, overflow)

**Operazioni di salto (trasferimento del controllo)**

- Salto incondizionato (JUMP): salta sempre all'indirizzo specificato
- Salto condizionato (BRANCH): salta solo se una condizione è verificata
- Chiamata a procedura (CALL): salva indirizzo di ritorno e salta
- Ritorno (RETURN): ripristina esecuzione dal punto di chiamata

**Operazioni di sistema**

Istruzioni privilegiate per la gestione del sistema operativo (es. HALT, gestione interrupt).

---

## H.6

**Descrivere i tipi di operandi delle istruzioni (indirizzi, numeri, caratteri, dati logici).**

**Indirizzi**

Interi senza segno che indicano locazioni di memoria. Possono riferirsi a:
- Memoria principale o virtuale
- Registri della CPU
- Dispositivi di I/O

La dimensione dell'indirizzo determina lo spazio di memoria indirizzabile.

**Numeri**

- **Interi in virgola fissa**: rappresentati in complemento a due (con segno) o binario puro (senza segno). Dimensioni tipiche: 8, 16, 32, 64 bit.

- **Numeri in virgola mobile**: rappresentati secondo lo standard IEEE 754. Formati: singola precisione (32 bit), doppia precisione (64 bit).

- **Decimali impaccati (Packed BCD)**: ogni cifra decimale occupa 4 bit. Inefficiente (usa solo 10 delle 16 configurazioni possibili) ma evita errori di conversione nelle operazioni di I/O con dati decimali.

**Caratteri**

Codificati tipicamente in:
- **ASCII** a 7 bit: 128 caratteri (lettere, cifre, simboli, caratteri di controllo)
- **ASCII esteso** a 8 bit: 256 caratteri, include bit di parità per rilevazione errori
- **Unicode**: standard moderno per rappresentare caratteri di tutte le lingue

**Dati logici**

Sequenze di n bit manipolabili individualmente. Ogni bit rappresenta un valore booleano (vero/falso, 0/1).

Utilizzi:
- Flag e codici di condizione
- Maschere per operazioni bit a bit
- Insiemi e bitmap
- Campi di bit all'interno di strutture dati

---

---

# SEZIONE I: MODI DI INDIRIZZAMENTO

## I.1

**Si descriva in dettaglio la modalità di indirizzamento indiretto, discuterne pregi e difetti. L'adozione di tale indirizzamento è favorito o sfavorito in un'architettura RISC? Motivare la risposta.**

**Descrizione dell'indirizzamento indiretto**

Il campo indirizzo dell'istruzione contiene l'indirizzo di una cella di memoria che a sua volta contiene l'indirizzo dell'operando effettivo.

Schema: Istruzione → Indirizzo A → Memoria[A] contiene B → Operando in Memoria[B]

**Pregi**:
- Ampio spazio di indirizzamento: permette di indirizzare $2^N$ entità con parole di lunghezza N bit
- Flessibilità: l'indirizzo effettivo può essere modificato a runtime cambiando il contenuto della cella intermedia
- Utile per implementare puntatori e strutture dati dinamiche
- Permette l'indirizzamento di memoria più grande del campo indirizzo

**Difetti**:
- Richiede due accessi in memoria: uno per leggere l'indirizzo intermedio, uno per leggere l'operando
- Prestazioni inferiori rispetto all'indirizzamento diretto
- Complessità maggiore nel ciclo fetch/execute

**Architetture RISC e indirizzamento indiretto**

L'indirizzamento indiretto è **sfavorito** nelle architetture RISC per diverse ragioni:

1. **Richiede accessi multipli alla memoria**: viola il principio RISC di minimizzare gli accessi alla memoria (solo LOAD e STORE dovrebbero accedere alla memoria).

2. **Complessità della pipeline**: gli accessi multipli complicano la gestione della pipeline e introducono stalli.

3. **Semplicità del set di istruzioni**: le architetture RISC preferiscono pochi e semplici modi di indirizzamento (registro e spiazzamento).

4. **Alternativa RISC**: l'indirizzamento indiretto può essere simulato con due istruzioni separate (LOAD dell'indirizzo in un registro, poi LOAD tramite registro indiretto), mantenendo istruzioni semplici.

---

## I.2

**Si descrivano in dettaglio le modalità di indirizzamento con spiazzamento e a pila. In particolare, si confrontino criticamente i 2 modi di indirizzamento e se ne discutano i pregi e i difetti.**

**Indirizzamento con spiazzamento (Displacement)**

Combina indirizzamento diretto e registro indiretto. Il campo indirizzo ha due sottocampi: un valore A e un registro R. L'indirizzo effettivo è: EA = [R] + A

**Varianti**:
- **Relativo al PC**: R è il Program Counter; EA = A + [PC]. Utile per salti relativi.
- **Registro-base**: R contiene l'indirizzo base, A è lo spiazzamento. Utile per accesso a strutture dati.
- **Indicizzazione**: A è la base fissa, R contiene lo spiazzamento variabile. Utile per scorrere array.

*Pregi*:
- Flessibilità: permette di accedere a dati relativi a una base variabile
- Efficiente per strutture dati e array
- Un solo accesso in memoria per l'operando
- Permette codice rilocabile (con indirizzamento relativo al PC)

*Difetti*:
- Richiede somma per calcolare l'indirizzo effettivo
- Campo spiazzamento limitato in dimensione

**Indirizzamento a pila (Stack)**

La pila è una sequenza LIFO di locazioni in memoria. Il registro SP (Stack Pointer) punta alla cima. L'operando è implicitamente sulla cima della pila.

*Pregi*:
- Istruzioni molto compatte (nessun campo indirizzo esplicito)
- Naturale per la valutazione di espressioni e chiamate di procedura
- Gestione automatica degli indirizzi tramite SP
- Utile per salvare/ripristinare contesto

*Difetti*:
- Accesso limitato alla sola cima della pila
- Overhead per operazioni push/pop
- Non efficiente per accesso casuale ai dati
- Difficile ottimizzazione del codice

**Confronto critico**

|Aspetto|Spiazzamento|Pila|
|---|---|---|
|Flessibilità|Alta (accesso a qualsiasi offset)|Bassa (solo cima)|
|Compattezza|Media|Alta|
|Calcolo indirizzo|Somma necessaria|Implicito (SP)|
|Uso tipico|Array, strutture, variabili locali|Espressioni, chiamate|
|Ottimizzazione|Facile|Difficile|

---

## I.3

**Descrivere i modi di indirizzamento: immediato, diretto, indiretto, registro, registro indiretto, spiazzamento.**

**Indirizzamento Immediato**

L'operando è contenuto direttamente nell'istruzione, nel campo indirizzo.

- Nessun accesso in memoria per recuperare l'operando
- Valore limitato dalla dimensione del campo indirizzo
- Utile per costanti e inizializzazioni

**Indirizzamento Diretto**

Il campo indirizzo contiene l'indirizzo effettivo dell'operando in memoria.

- Richiede un singolo accesso in memoria
- Spazio di indirizzamento limitato dalla dimensione del campo
- Semplice ma poco flessibile

**Indirizzamento Indiretto**

Il campo indirizzo contiene l'indirizzo di una cella di memoria che contiene l'indirizzo dell'operando.

- Permette di indirizzare $2^N$ entità con parole di lunghezza N
- Richiede due accessi in memoria
- Flessibile ma lento

**Indirizzamento a Registro**

L'operando si trova in un registro specificato nel campo indirizzo.

- Pochi bit per identificare il registro (registri sono pochi)
- Istruzioni più corte e fetch più veloci
- Nessun accesso in memoria

**Indirizzamento Registro Indiretto**

L'operando è in una cella di memoria il cui indirizzo è contenuto in un registro.

- Grande spazio di indirizzamento ($2^n$ con registro di n bit)
- Un solo accesso in memoria (meno dell'indiretto puro)
- Flessibile per puntatori e strutture dinamiche

**Indirizzamento con Spiazzamento**

L'indirizzo effettivo è la somma del contenuto di un registro R e di un valore A contenuto nell'istruzione: EA = [R] + A

Varianti:
- **Relativo al PC**: R = PC; per salti relativi e codice rilocabile
- **Registro-base**: R contiene base, A è offset; per strutture dati
- **Indicizzazione**: A è base fissa, R è indice variabile; per scorrere array

---

---

# SEZIONE J: PROCEDURE E REGISTRI

## J.1

**Descrivere i possibili approcci per trattare l'indirizzo di ritorno da una chiamata di procedura.**

Quando viene effettuata una chiamata di procedura (CALL), è necessario salvare l'indirizzo di ritorno per poter riprendere l'esecuzione al completamento della procedura.

**1. Salvataggio in un registro dedicato**

L'indirizzo di ritorno viene salvato in un registro specifico (es. Link Register).

*Vantaggi*: veloce, nessun accesso in memoria
*Svantaggi*: non supporta chiamate annidate (il registro verrebbe sovrascritto)

**2. Salvataggio all'inizio della procedura**

L'indirizzo di ritorno viene memorizzato nella prima locazione della procedura chiamata.

*Vantaggi*: semplice
*Svantaggi*: non supporta ricorsione (la procedura sovrascrive il proprio indirizzo di ritorno), la procedura non può essere in memoria di sola lettura

**3. Salvataggio in una pila (Stack)**

L'indirizzo di ritorno viene impilato (push) nello stack al momento della chiamata e recuperato (pop) al ritorno.

*Vantaggi*:
- Gestisce naturalmente le chiamate annidate e la ricorsione
- Ordine LIFO corrisponde all'ordine di ritorno
- Permette di salvare anche parametri e variabili locali (stack frame)

*Svantaggi*: richiede accessi in memoria

**Approccio preferito**

La pila è il meccanismo più flessibile e universalmente adottato. Lo **stack frame** di ogni procedura contiene:
- Indirizzo di ritorno
- Parametri passati
- Variabili locali
- Registri salvati

Le architetture RISC moderne combinano: registro per l'indirizzo di ritorno (veloce per procedure "foglia" che non chiamano altre procedure) e stack per chiamate annidate.

---

## J.2

**Cos'è una procedura?**

Una **procedura** (o subroutine, funzione) è un blocco di codice riutilizzabile che può essere invocato da più punti del programma principale o da altre procedure.

**Caratteristiche**:
- Contiene una sequenza di istruzioni che svolge un compito specifico
- Può ricevere parametri in ingresso
- Può restituire un valore
- Al termine, il controllo ritorna al punto di chiamata

**Vantaggi**:
- **Economia di codice**: sequenze ripetute vengono scritte una sola volta
- **Modularità**: il programma è diviso in blocchi logici indipendenti
- **Riusabilità**: le procedure possono essere usate in programmi diversi
- **Leggibilità**: codice più organizzato e comprensibile
- **Manutenibilità**: modifiche localizzate alla procedura si propagano ovunque sia usata

**Meccanismo di chiamata**:

1. L'istruzione **CALL** (o equivalente):
   - Salva l'indirizzo di ritorno (tipicamente nello stack)
   - Trasferisce il controllo alla procedura (carica nel PC l'indirizzo della procedura)

2. Esecuzione del corpo della procedura

3. L'istruzione **RETURN**:
   - Recupera l'indirizzo di ritorno
   - Trasferisce il controllo al chiamante (ripristina il PC)

**Elementi coinvolti**:
- Passaggio dei parametri (registri, stack, memoria)
- Salvataggio/ripristino del contesto (registri utilizzati)
- Allocazione di variabili locali
- Gestione dello stack frame

---

## J.3

**Descrivere i registri: classificazione in registri utente (uso generale e particolare) e registri di stato e controllo (PC, MAR, IR, MBR).**

I registri costituiscono lo "spazio di lavoro" della CPU e rappresentano il livello più alto della gerarchia della memoria.

**Registri Utente**

Utilizzati dal programmatore (umano o compilatore) per memorizzare dati da elaborare.

*Registri ad uso generale*:
- Possono contenere dati o indirizzi
- Maggiore flessibilità ma richiedono più bit per specificarli
- Numero ottimale: tipicamente tra 8 e 32 (con meno di 8 aumentano gli accessi in memoria, oltre 32 non si ottengono ulteriori benefici)

*Registri particolari (specializzati)*:
- **Accumulatore**: registro implicito per operazioni aritmetiche
- **Registri indice**: per l'indicizzazione di array
- **Registri base**: per l'indirizzamento segmentato
- **Stack Pointer (SP)**: punta alla cima dello stack

*Registri per codici di condizione*:
- Bit individuali che indicano condizioni (zero, negativo, overflow, carry)
- Letti implicitamente dalle istruzioni di salto condizionato

**Registri di Controllo e di Stato**

Usati dall'unità di controllo e dal sistema operativo.

*Program Counter (PC)*:
- Contiene l'indirizzo della prossima istruzione da eseguire
- Incrementato dopo ogni fetch, modificato da salti e chiamate

*Instruction Register (IR)*:
- Contiene l'istruzione corrente in esecuzione
- Caricato durante la fase di fetch

*Memory Address Register (MAR)*:
- Contiene l'indirizzo di memoria da accedere
- Collegato al bus degli indirizzi

*Memory Buffer Register (MBR)*:
- Contiene i dati letti dalla memoria o da scrivere in memoria
- Collegato al bus dei dati

*Program Status Word (PSW)*:
- Contiene flag di stato e controllo: segno, zero, overflow, carry, abilitazione interrupt, modo supervisore

---

## J.4

**Descrivere l'organizzazione della memoria per segmenti.**

**Definizione**

La memoria principale può essere organizzata logicamente come un insieme di **segmenti** (o spazi di indirizzamento multipli), visibili al programmatore. Ogni segmento è un'area logica di memoria con caratteristiche proprie.

**Struttura dell'indirizzamento**

Una locazione di memoria viene riferita specificando:
- **Identificatore del segmento**: quale segmento contiene il dato
- **Offset**: posizione del dato all'interno del segmento

**Registri necessari**

Per supportare la segmentazione occorrono registri che memorizzino:
- **Indirizzo base del segmento**: dove inizia il segmento nella memoria fisica
- **Limite del segmento**: dimensione massima del segmento (per protezione)

**Vantaggi**:
- **Protezione**: ogni segmento può avere permessi diversi (lettura, scrittura, esecuzione)
- **Condivisione**: segmenti possono essere condivisi tra processi
- **Organizzazione logica**: separazione tra codice, dati, stack
- **Rilocazione**: i segmenti possono essere spostati in memoria fisica

**Segmenti tipici**:
- **Segmento codice (Code Segment)**: contiene le istruzioni del programma
- **Segmento dati (Data Segment)**: contiene variabili globali e statiche
- **Segmento stack (Stack Segment)**: contiene lo stack per chiamate di procedura
- **Segmento extra**: per dati aggiuntivi

**Esempio: architettura x86**

I registri di segmento (CS, DS, SS, ES) indicizzano una tabella di descrittori che contiene:
- Indirizzo base del segmento
- Limite del segmento
- Diritti di accesso (lettura, scrittura, esecuzione, livello di privilegio)

---

---

# SEZIONE K: PIPELINE

## K.1

**Cos'è il Pipelining? Descrivere le fasi (FI, DI, CO, FO, EI, WO).**

**Definizione**

Il pipelining applica il principio della catena di montaggio all'esecuzione delle istruzioni. Un'istruzione viene decomposta in fasi successive, ciascuna realizzata da un'unità funzionale diversa. In ogni istante, istruzioni diverse si trovano in fasi diverse, realizzando parallelismo.

**Principio di funzionamento**

Mentre un'istruzione è in esecuzione, la successiva è in decodifica, quella dopo ancora è in fetch, e così via. A regime, ogni ciclo di clock completa un'istruzione.

**Fasi tipiche del ciclo esecutivo**

**FI (Fetch Instruction)**: prelievo dell'istruzione dalla memoria e caricamento nell'IR. Il PC viene incrementato.

**DI (Decode Instruction)**: decodifica del codice operativo per identificare l'operazione e i modi di indirizzamento.

**CO (Calculate Operand address)**: calcolo dell'indirizzo effettivo degli operandi (se necessario).

**FO (Fetch Operand)**: prelievo degli operandi dalla memoria o dai registri.

**EI (Execute Instruction)**: esecuzione dell'operazione da parte della ALU.

**WO (Write Operand)**: scrittura del risultato in memoria o nei registri.

**Vantaggi**:
- Aumento del throughput (istruzioni completate per unità di tempo)
- Migliore utilizzo delle risorse hardware

**Nota**: la pipeline non riduce il tempo di esecuzione di una singola istruzione (che richiede comunque k cicli per k stadi), ma aumenta il numero di istruzioni completate nell'unità di tempo.

---

## K.2

**Descrivere il prefetch delle istruzioni.**

**Definizione**

Il prefetch (prelievo anticipato) è una tecnica che prevede il caricamento dell'istruzione successiva dalla memoria durante l'esecuzione dell'istruzione corrente.

**Motivazione**

Durante la fase di fetch si accede alla memoria principale, mentre durante l'esecuzione tipicamente la memoria non è utilizzata (se l'operazione è tra registri). È quindi possibile sfruttare questo tempo per prelevare anticipatamente l'istruzione successiva.

**Funzionamento**

1. Mentre l'istruzione I₁ è in esecuzione, viene effettuato il fetch di I₂
2. Al termine di I₁, I₂ è già disponibile e può iniziare immediatamente l'esecuzione
3. Contemporaneamente viene effettuato il fetch di I₃

**Limitazioni**

Il prefetch non raddoppia le prestazioni per due ragioni:

1. **Istruzioni di salto**: se l'istruzione corrente è un salto (jump/branch), l'istruzione precaricata potrebbe non essere quella effettivamente eseguita. Il prefetch risulta inutile e l'istruzione target deve essere caricata.

2. **Sbilanciamento delle fasi**: la fase di fetch è tipicamente più breve della fase di esecuzione, quindi il guadagno è limitato.

**Evoluzione**

Il prefetch è il precursore del pipelining completo. L'idea viene estesa:
- Prelevando più istruzioni (buffer di prefetch)
- Aggiungendo più fasi per sfruttare meglio il parallelismo
- Realizzando una pipeline a più stadi

---

## K.3

**Descrivere il fattore velocizzante (speedup) di una pipeline a k stadi.**

**Tempo di ciclo**

Il tempo di ciclo τ è determinato dalla fase più lenta più il ritardo di commutazione dei registri di pipeline:

$$\tau = \max(\tau_i) + d$$

dove $\tau_i$ è il tempo della fase i-esima e $d$ è il ritardo dei registri.

**Tempo totale di esecuzione**

Per eseguire n istruzioni con una pipeline a k stadi:

$$T_k = [k + (n-1)] \cdot \tau$$

- Le prime k fasi riempiono la pipeline (k cicli)
- Poi ogni ciclo completa un'istruzione (n-1 cicli aggiuntivi)

**Speedup**

Il fattore di accelerazione rispetto all'esecuzione sequenziale (senza pipeline) è:

$$S_k = \frac{T_{sequenziale}}{T_k} = \frac{n \cdot k \cdot \tau}{[k + (n-1)] \cdot \tau} = \frac{nk}{k + (n-1)}$$

**Comportamento asintotico**

- Per $n \rightarrow \infty$: $S_k \rightarrow k$
- Lo speedup massimo teorico è pari al numero di stadi k
- Con poche istruzioni, l'overhead di riempimento della pipeline riduce lo speedup

**Esempio numerico**

Con k=5 stadi e n=100 istruzioni:
$$S_5 = \frac{100 \times 5}{5 + 99} = \frac{500}{104} \approx 4.8$$

**Limitazioni reali**

Lo speedup teorico k non viene mai raggiunto a causa di:
- Sbilanciamento delle fasi
- Problemi strutturali (conflitti di risorse)
- Dipendenze dai dati
- Dipendenze dal controllo (salti)

---

## K.4

**Descrivere le problematiche della pipeline (sbilanciamento delle fasi, problemi strutturali, dipendenza dai dati, dipendenza dai controlli).**

**1. Sbilanciamento delle fasi**

Le fasi hanno durate diverse e non tutte le istruzioni richiedono le stesse fasi.

*Problema*: il tempo di ciclo è determinato dalla fase più lenta; le fasi veloci restano inattive.

*Soluzioni*:
- Decomposizione delle fasi onerose in sottofasi
- Duplicazione degli esecutori delle fasi più lente

**2. Problemi strutturali (Structural Hazards)**

Due o più fasi competono per la stessa risorsa nello stesso ciclo di clock.

*Esempio tipico*: FI, FO e WO accedono contemporaneamente alla memoria.

*Soluzioni*:
- Introduzione di fasi non operative (NOP/bubble)
- Suddivisione della memoria in cache separate per istruzioni e dati
- Duplicazione delle risorse contese

**3. Dipendenza dai dati (Data Hazards)**

Un'istruzione necessita del risultato di un'istruzione precedente ancora in pipeline.

*Tipi*:
- **RAW (Read After Write)**: l'istruzione successiva legge prima che la precedente scriva
- **WAW (Write After Write)**: scritture fuori ordine
- **WAR (Write After Read)**: caso raro

*Soluzioni*:
- Stalli (inserimento di NOP)
- Data forwarding (propagazione anticipata del risultato)
- Riordino delle istruzioni da parte del compilatore

**4. Dipendenza dal controllo (Control Hazards)**

Le istruzioni di salto, chiamate a procedura e interrupt alterano la sequenzialità.

*Problema*: le istruzioni caricate dopo un branch potrebbero essere errate (branch penalty).

*Soluzioni*:
- Flussi multipli
- Prefetch dell'istruzione target
- Buffer circolare (loop buffer)
- Predizione dei salti
- Salto ritardato (delayed branch)

---

## K.5

**Nel contesto di una pipeline descrivere la problematica della dipendenza dei dati e si discutano in dettaglio le tecniche viste a lezione per trattare il problema.**

**Problema della dipendenza dai dati**

Si verifica quando un'istruzione necessita di un dato prodotto da un'istruzione precedente che non ha ancora completato la scrittura del risultato.

**Esempio**:
```
ADD R1, R2, R3    ; R1 ← R2 + R3
SUB R4, R1, R5    ; R4 ← R1 - R5 (usa R1 prima che ADD lo scriva)
```

**Tipi di hazard**:
- **RAW (Read After Write)**: il più comune; lettura prima della scrittura
- **WAW (Write After Write)**: scritture fuori ordine
- **WAR (Write After Read)**: raro nelle pipeline semplici

**Tecniche di gestione**

**1. Stalli (Pipeline Stall / Bubble)**

Si inseriscono cicli di attesa (NOP) finché il dato non è disponibile.

*Svantaggio*: riduce le prestazioni della pipeline

**2. Data Forwarding (Bypassing)**

Il risultato viene propagato direttamente dall'uscita della ALU (o dal registro di pipeline) all'ingresso della fase che lo richiede, senza attendere la fase di writeback.

*Esempio*: il risultato di ADD disponibile dopo EX viene inoltrato direttamente all'ingresso della ALU per SUB.

*Vantaggio*: riduce o elimina gli stalli
*Richiede*: hardware aggiuntivo (multiplexer, comparatori)

**3. Riordino delle istruzioni (Compiler Scheduling)**

Il compilatore riorganizza le istruzioni per distanziare le dipendenze, inserendo istruzioni indipendenti tra quelle dipendenti.

*Esempio*: se C non dipende da A né da B:
```
ADD R1, R2, R3    ; A
istruzione C      ; indipendente, inserita qui
SUB R4, R1, R5    ; B (ora R1 è pronto)
```

**4. Combinazione di tecniche**

Spesso si usano forwarding + stalli residui + ottimizzazione del compilatore.

---

## K.6

**Nel contesto di una pipeline, descrivere nel dettaglio la tecnica del data forwarding: a cosa serve?**

**Scopo**

Il data forwarding (o bypassing) serve a ridurre o eliminare gli stalli causati da dipendenze dai dati nella pipeline. Invece di attendere che un risultato venga scritto nel register file e poi riletto, il dato viene propagato direttamente da dove è disponibile a dove è necessario.

**Funzionamento**

1. **Rilevamento**: comparatori verificano se i registri sorgente di un'istruzione in ID/EX coincidono con il registro destinazione di istruzioni precedenti in EX/MEM o MEM/WB.

2. **Propagazione**: multiplexer sugli ingressi della ALU selezionano la sorgente del dato:
   - Dal register file (nessuna dipendenza)
   - Dal registro EX/MEM (risultato ALU del ciclo precedente)
   - Dal registro MEM/WB (dato dalla memoria o risultato precedente)

**Segnali di controllo (esempio RISC-V)**

PropagaA e PropagaB controllano i multiplexer:
- **00**: valore dal register file
- **10**: valore propagato da EX/MEM (risultato ALU)
- **01**: valore propagato da MEM/WB

**Condizioni per EX hazard**:
- EX/MEM.RegisterRd = ID/EX.RegisterRs1 → ForwardA = 10
- EX/MEM.RegisterRd = ID/EX.RegisterRs2 → ForwardB = 10

**Condizioni per MEM hazard**:
- MEM/WB.RegisterRd = ID/EX.RegisterRs1 (e non c'è EX hazard) → ForwardA = 01
- MEM/WB.RegisterRd = ID/EX.RegisterRs2 (e non c'è EX hazard) → ForwardB = 01

**Limitazione**

Il forwarding non elimina tutti gli stalli. Ad esempio, dopo una LOAD, il dato è disponibile solo dopo la fase MEM. Se l'istruzione immediatamente successiva usa quel dato, serve comunque uno stallo.

---

## K.7

**Nel contesto della pipeline MIPS, si illustri in che modo lo stadio ID è in grado di rilevare la dipendenza dei dati.**

**Rilevamento in fase ID**

Nella pipeline MIPS/RISC-V, tutte le dipendenze dai dati possono essere individuate nella fase ID (Instruction Decode). Questo permette di gestire le dipendenze prima che l'istruzione venga rilasciata (issued) alle fasi successive.

**Informazioni disponibili in ID**

In fase ID sono noti:
- I registri sorgente dell'istruzione corrente (rs1, rs2 decodificati dall'IR)
- I registri destinazione delle istruzioni nelle fasi successive (disponibili nei registri di pipeline ID/EX, EX/MEM, MEM/WB)
- Il tipo di operazione (per sapere se scrive in un registro)

**Meccanismo di rilevamento**

L'**Hazard Detection Unit** confronta:

1. **Per EX hazard**: confronta i registri sorgente dell'istruzione in ID con il registro destinazione dell'istruzione in EX
   - Se EX/MEM.RegisterRd = ID/EX.RegisterRs1 o Rs2 → dipendenza rilevata

2. **Per MEM hazard**: confronta i registri sorgente con il registro destinazione dell'istruzione in MEM
   - Se MEM/WB.RegisterRd = ID/EX.RegisterRs1 o Rs2 → dipendenza rilevata

3. **Caso speciale LOAD-USE**: se l'istruzione in EX è una LOAD e l'istruzione in ID usa il registro destinazione della LOAD → stallo necessario (il forwarding non basta)

**Condizioni aggiuntive verificate**:
- RegWrite deve essere attivo (l'istruzione precedente deve effettivamente scrivere)
- Il registro destinazione non deve essere x0 (che contiene sempre 0)

**Azioni conseguenti**

- Se la dipendenza è risolvibile con forwarding → configurazione dei multiplexer
- Se serve stallo → segnale Stall = 1, che inserisce una bubble e blocca PC e IF/ID

---

## K.8

**Nel contesto di una pipeline descrivere la problematica della dipendenza dal controllo e si discuta in particolare la tecnica del buffer circolare, spiegando in quali situazioni tale tecnica è particolarmente efficace.**

**Problema della dipendenza dal controllo**

Le istruzioni di salto (branch), chiamate a procedura e interrupt alterano il flusso sequenziale di esecuzione. Quando si verifica un salto condizionato:

1. Le istruzioni già caricate nella pipeline dopo il branch potrebbero essere errate
2. Se il salto viene effettuato, queste istruzioni devono essere scartate
3. Si perde tempo (branch penalty) per caricare le istruzioni corrette

**Tecnica del Buffer Circolare (Loop Buffer)**

**Struttura**: una piccola memoria veloce (tipicamente nella fase di fetch) che mantiene le ultime n istruzioni prelevate, organizzata come buffer circolare.

**Funzionamento**:
1. Ogni istruzione prelevata viene memorizzata nel buffer
2. Se un branch ha come target un'istruzione già presente nel buffer, non serve accedere alla memoria
3. Il fetch avviene direttamente dal buffer

**Vantaggi**:
- Se il target è nel buffer, si elimina la penalità di fetch
- Accesso più veloce rispetto alla cache
- Hardware semplice

**Situazioni particolarmente efficaci**

Il buffer circolare è particolarmente efficace per i **cicli (loop)**:

1. **Loop piccoli**: se il corpo del ciclo è contenuto interamente nel buffer, tutte le iterazioni (tranne la prima) usano solo il buffer
2. **Loop annidati**: il ciclo interno beneficia del buffer
3. **Pattern ripetitivi**: sequenze di codice eseguite più volte

**Esempio**: un loop di 8 istruzioni con buffer da 16 posizioni:
- Prima iterazione: fetch dalla memoria, istruzioni caricate nel buffer
- Iterazioni successive: tutte le istruzioni sono nel buffer, nessun accesso a memoria

**Limitazioni**: inefficace per salti a indirizzi distanti o mai visitati prima.

---

## K.9

**Nel contesto di una pipeline, descrivere nel dettaglio la tecnica della predizione di salto utilizzando 2 bit di predizione.**

**Motivazione**

La predizione a 1 bit ha un problema: nei cicli, commette due errori a regime (uno all'ingresso e uno all'uscita). La predizione a 2 bit migliora le prestazioni richiedendo due errori consecutivi per cambiare predizione.

**Schema a 2 bit**

Si utilizzano 4 stati codificati con 2 bit:

- **00**: Strongly Not Taken (predici "non saltare")
- **01**: Weakly Not Taken (predici "non saltare")
- **10**: Weakly Taken (predici "saltare")
- **11**: Strongly Taken (predici "saltare")

**Transizioni di stato**

```
                  Taken                    Taken
    00 ─────────────> 01 ─────────────> 11
   (SNT)  <───────── (WNT) <───────── (ST)
            Not Taken         Not Taken

    00 ─────────────> 10 ─────────────> 11
         Taken              Taken
```

- Da uno stato "Strongly": un errore porta allo stato "Weakly" corrispondente
- Da uno stato "Weakly": un altro errore cambia la predizione
- Una predizione corretta rafforza lo stato (verso "Strongly")

**Comportamento nei cicli**

In un ciclo tipico (molte iterazioni, poi uscita):
- Durante il ciclo: stato 11 (Strongly Taken), predice sempre "saltare" ✓
- All'uscita: errore, passa a 10 (Weakly Taken)
- Al prossimo ingresso nel ciclo: predice "saltare" ✓

**Risultato**: solo 1 errore per ciclo invece di 2 (rispetto alla predizione a 1 bit).

**Implementazione**

I 2 bit di predizione sono memorizzati in una **branch history table** insieme all'indirizzo dell'istruzione di salto e all'indirizzo target.

---

## K.10

**Nel contesto di una pipeline si consideri la tecnica che utilizza la tabella della storia dei salti. A cosa serve? Descriverla in dettaglio.**

**Scopo**

La **Branch History Table (BHT)** o **Branch Target Buffer (BTB)** serve a memorizzare informazioni sui salti condizionati già incontrati, permettendo di predire il comportamento dei salti futuri e ridurre la penalità da branch.

**Struttura**

La tabella contiene per ogni entry:
- **Indirizzo dell'istruzione di branch**: per identificare il salto
- **Indirizzo target**: dove saltare se il branch è taken
- **Bit di predizione**: 1 o 2 bit che indicano la predizione corrente
- **Bit di validità**: indica se l'entry è valida

**Funzionamento**

1. **Fetch**: quando viene prelevata un'istruzione, il suo indirizzo viene cercato nella BHT

2. **Hit nella tabella**:
   - L'istruzione è un branch già incontrato
   - Si usa la predizione memorizzata
   - Se predizione = "taken": si inizia il fetch dall'indirizzo target
   - Se predizione = "not taken": si continua sequenzialmente

3. **Miss nella tabella**:
   - Branch mai incontrato o entry non valida
   - Si assume una predizione di default (tipicamente "not taken")

4. **Aggiornamento**:
   - Quando il branch viene risolto (fase EX o MEM), si aggiorna la tabella
   - Se la predizione era corretta: si rafforza lo stato
   - Se la predizione era errata: si modifica lo stato e si svuota la pipeline

**Organizzazione**

- **Indirizzamento diretto**: parte dell'indirizzo del PC come indice
- **Associativa**: confronto con tutti gli indirizzi memorizzati
- **Set-associativa**: compromesso tra le due

**Vantaggi**

- Predizione disponibile molto presto (in fase IF)
- Indirizzo target già disponibile senza calcolo
- Efficace per branch ricorrenti

---

## K.11

**Nel contesto di una pipeline si spieghi in dettaglio la tecnica del salto ritardato. Fornire un esempio.**

**Definizione**

Nel **delayed branch** (salto ritardato), l'istruzione che si trova nel **branch delay slot** (la posizione immediatamente dopo il branch) viene sempre eseguita, indipendentemente dall'esito del salto.

**Motivazione**

Quando viene decodificato un branch, l'istruzione successiva è già stata prelevata. Invece di scartarla, si definisce che verrà sempre eseguita. Questo elimina la penalità di un ciclo.

**Funzionamento**

```
BRANCH condizione, target
<istruzione nel delay slot>  ← sempre eseguita
...
```

L'istruzione nel delay slot viene eseguita sia che il salto venga effettuato sia che non venga effettuato.

**Strategie di riempimento del delay slot**

Il compilatore deve inserire un'istruzione utile nel delay slot:

**1. "From before"**: si sposta nel delay slot un'istruzione indipendente che precede il branch.

```
Prima:                      Dopo:
ADD R1, R2, R3              BRANCH cond, target
BRANCH cond, target    →    ADD R1, R2, R3    (delay slot)
```

**2. "From target"**: si inserisce la prima istruzione del target (efficace quando il salto è molto probabile).

```
target: SUB R4, R5, R6      BRANCH cond, target
        ...             →   SUB R4, R5, R6    (delay slot)
```
(Il SUB viene duplicato o rimosso dal target)

**3. NOP**: se non si trova un'istruzione utile, si inserisce un NOP (nessun vantaggio ma nessun danno).

**Esempio completo**

```
; Codice originale          ; Con delayed branch
LOOP: LW R1, 0(R2)          LOOP: LW R1, 0(R2)
      ADD R3, R3, R1              ADD R3, R3, R1
      ADDI R2, R2, 4              BNE R2, R4, LOOP
      BNE R2, R4, LOOP            ADDI R2, R2, 4  (delay slot)
```

L'istruzione ADDI viene spostata nel delay slot; viene eseguita sia che il loop continui sia che termini.

---

---

# SEZIONE L: ARCHITETTURE RISC E CISC

## L.1

**Spiegare nel dettaglio la differenza fra un'architettura CISC e una RISC.**

**CISC (Complex Instruction Set Computer)**

- **Set di istruzioni ampio**: centinaia di istruzioni diverse
- **Istruzioni complesse**: operazioni sofisticate in una singola istruzione
- **Formato variabile**: istruzioni di lunghezza diversa (da 1 a molti byte)
- **Molti modi di indirizzamento**: diretto, indiretto, indicizzato, con spiazzamento, ecc.
- **Operazioni memoria-memoria**: istruzioni che operano direttamente sulla memoria
- **Unità di controllo microprogrammata**: flessibile ma più lenta
- **Pochi registri**: tipicamente 8-16

*Esempi*: x86, VAX, IBM 370

**RISC (Reduced Instruction Set Computer)**

- **Set di istruzioni ridotto**: poche decine di istruzioni semplici
- **Un'istruzione per ciclo di clock**: a regime, ogni ciclo completa un'istruzione
- **Formato fisso**: tutte le istruzioni della stessa lunghezza (tipicamente 32 bit)
- **Pochi modi di indirizzamento**: principalmente registro e spiazzamento
- **Operazioni registro-registro**: tranne LOAD e STORE, le operazioni sono tra registri
- **Unità di controllo cablata**: meno flessibile ma più veloce
- **Molti registri**: tipicamente 32 o più, spesso organizzati a finestre

*Esempi*: MIPS, RISC-V, ARM, SPARC

**Confronto sintetico**

|Caratteristica|CISC|RISC|
|---|---|---|
|Numero istruzioni|200+|~100|
|Lunghezza istruzioni|Variabile|Fissa|
|Modi di indirizzamento|Molti (10+)|Pochi (2-3)|
|Registri|Pochi (8-16)|Molti (32+)|
|Accesso memoria|Qualsiasi istruzione|Solo LOAD/STORE|
|Controllo|Microprogrammato|Cablato|

---

## L.2

**Spiegare le motivazioni alla base dell'architettura CISC.**

**Il problema del gap semantico**

I linguaggi di alto livello (HLL) permettono di esprimere algoritmi in modo conciso. Tra istruzioni HLL e istruzioni macchina esiste un **gap semantico** che causa:
- Esecuzione inefficiente
- Dimensione eccessiva dei programmi compilati
- Complessità del compilatore

**Risposta tradizionale CISC**

I progettisti hardware hanno risposto al gap semantico:

1. **Ampliando il set di istruzioni**: più istruzioni per coprire più costrutti HLL

2. **Introducendo svariati modi di indirizzamento**: per gestire diverse strutture dati

3. **Implementando in hardware costrutti HLL**: es. istruzione CASE su VAX

**Motivazioni principali**

1. **Semplificare il compilatore**: istruzioni complesse permettono traduzione diretta di costrutti HLL

2. **Migliorare l'efficienza**: sequenze di codice più brevi e veloci (in teoria)

3. **Risparmiare memoria**: programmi più compatti grazie a istruzioni potenti (la memoria era costosa)

4. **Microcodice efficiente**: il microcodice può essere ottimizzato e memorizzato in ROM veloce

5. **Compatibilità**: nuove versioni possono aggiungere istruzioni mantenendo retrocompatibilità

**Criticità emerse successivamente**

- Le istruzioni più frequenti sono le più semplici
- Le istruzioni complesse sono difficili da sfruttare per i compilatori
- L'unità di controllo complessa rallenta anche le istruzioni semplici
- La dimensione dei programmi RISC e CISC è simile
- La pipeline è più difficile da implementare con formato variabile

---

## L.3

**Descrivere l'architettura RISC: caratteristiche, strategie hardware e software, organizzazione in finestre di registri.**

**Caratteristiche principali**

- **Un'istruzione per ciclo di clock**: a regime ogni ciclo termina un'istruzione
- **Operazioni registro-registro**: tranne LOAD e STORE
- **Pochi e semplici modi di indirizzamento**: registro e spiazzamento relativo a PC
- **Pochi e semplici formati fissi**: campi e opcode a dimensione fissa
- **Unità di controllo cablata**: più veloce rispetto a quella microprogrammata

**Strategie hardware**

1. **Ampio numero di registri**: per mantenere gli operandi nei registri il più possibile
2. **Pipeline efficiente**: formato fisso permette decodifica e fetch registri simultanei
3. **Cache separate**: per istruzioni e dati, per evitare conflitti strutturali
4. **Unità di controllo cablata**: veloce e ottimizzata per il set ridotto di istruzioni

**Strategie software**

1. **Ottimizzazione del compilatore**: allocazione efficiente dei registri
2. **Riordino delle istruzioni**: per evitare stalli nella pipeline
3. **Riempimento dei delay slot**: per i salti ritardati

**Organizzazione in finestre di registri**

Le variabili locali cambiano ad ogni chiamata/rientro da procedura. I registri sono suddivisi in gruppi (finestre), ogni procedura ha la propria finestra.

**Struttura di una finestra**:
- **Parameter registers**: parametri passati e valore di ritorno
- **Local registers**: variabili locali
- **Temporary registers**: scambio parametri con procedure chiamate

Le finestre adiacenti si sovrappongono: i temporary della procedura chiamante coincidono con i parameter della chiamata.

**Buffer circolare**: le finestre sono organizzate circolarmente. Il Current Window Pointer (CWP) indica la finestra attiva. Quando si esauriscono le finestre, la più vecchia viene salvata in memoria.

**Registri globali**: un gruppo separato accessibile da tutte le procedure.

---

## L.4

**Spiegare in dettaglio come le architetture RISC trattino efficacemente le chiamate annidate di procedura.**

**Il problema delle chiamate annidate**

Le chiamate di procedura sono tra le operazioni più costose. Ogni chiamata richiede:
- Salvataggio dell'indirizzo di ritorno
- Passaggio dei parametri
- Allocazione delle variabili locali
- Al ritorno: ripristino del contesto

Con le chiamate annidate (procedura A chiama B che chiama C), il problema si moltiplica.

**Soluzione RISC: finestre di registri sovrapposte**

**Meccanismo**:

1. I registri sono organizzati in **finestre** di dimensione fissa
2. Ogni procedura ha la propria finestra automaticamente assegnata
3. Una chiamata di procedura **cambia automaticamente la finestra** invece di salvare registri in memoria

**Struttura delle finestre sovrapposte**:

```
Procedura A:  | Param A | Local A | Temp A |
Procedura B:           | Param B | Local B | Temp B |
                        ↑
                        Sovrapposti!
```

I **Temporary registers** della procedura chiamante (A) coincidono fisicamente con i **Parameter registers** della procedura chiamata (B).

**Vantaggi**:
- Il passaggio dei parametri non richiede copia: i dati sono già nei registri della nuova finestra
- Nessun salvataggio in memoria per chiamate poco profonde
- Cambio di finestra molto veloce (modifica di un puntatore)

**Gestione del buffer circolare**:

- Il **Current Window Pointer (CWP)** punta alla finestra attiva
- Il **Saved Window Pointer (SWP)** indica l'ultima finestra salvata
- Quando CWP raggiunge SWP: interrupt, la finestra più vecchia viene salvata in memoria, SWP viene incrementato
- Un buffer a N finestre supporta N-1 procedure annidate senza accesso a memoria

**Misurazioni empiriche**: le chiamate tipicamente coinvolgono meno di 6 parametri e meno di 6 variabili locali, rendendo le finestre molto efficaci.

---

## L.5

**Spiegare in che modo un compilatore possa aiutare l'utilizzo efficace dei registri da parte di un'architettura RISC.**

**Obiettivo**

Mantenere gli operandi nei registri il più possibile, minimizzando i trasferimenti con la memoria (LOAD/STORE). Le architetture RISC hanno molti registri (32+), e il compilatore deve sfruttarli al meglio.

**Tecniche di ottimizzazione del compilatore**

**1. Allocazione delle variabili ai registri**

Il compilatore analizza il codice per:
- Identificare le variabili più usate
- Assegnarle ai registri per tutta la durata del loro utilizzo
- Variabili poco usate restano in memoria

**2. Analisi del tempo di vita delle variabili**

Il compilatore determina per ogni variabile l'intervallo di tempo in cui è "viva" (dal primo assegnamento all'ultimo uso).

Se due variabili non sono mai vive contemporaneamente, possono condividere lo stesso registro.

**3. Colorazione del grafo (Graph Coloring)**

Il problema dell'allocazione dei registri è equivalente alla colorazione di un grafo:
- **Nodi**: registri simbolici (variabili)
- **Archi**: collegano variabili vive nello stesso intervallo
- **Colori**: registri fisici

Due variabili collegate non possono avere lo stesso colore (registro). Se il grafo è colorabile con k colori (k = numero di registri), l'allocazione è possibile senza spilling.

**Nota**: decidere se un grafo è colorabile con k colori è NP-completo nel caso generale, ma esistono algoritmi euristici efficienti.

**4. Spilling**

Se i registri non sono sufficienti, alcune variabili vengono mantenute in memoria (spilling). Il compilatore sceglie le variabili da "spillare" minimizzando l'impatto sulle prestazioni (tipicamente quelle meno usate).

**5. Risultati pratici**

Con 32-64 registri fisici, i compilatori ottimizzanti riescono a mantenere in registri la maggior parte delle variabili scalari locali.

---

---

# SEZIONE M: MICROPROGRAMMAZIONE

## M.1

**Descrivere sinteticamente l'implementazione delle istruzioni attraverso la tecnica della microprogrammazione. Dire se questa tecnica viene utilizzata per i processori CISC o RISC, e motivare la risposta.**

**Descrizione della microprogrammazione**

La microprogrammazione è una tecnica per implementare l'unità di controllo del processore. Ogni istruzione macchina viene tradotta in una sequenza di **microistruzioni** memorizzate in una memoria di controllo (Control Store).

**Struttura**:
- **Microistruzione**: specifica i segnali di controllo da attivare in un microciclo
- **Microprogramma**: sequenza di microistruzioni che implementa un'istruzione macchina
- **Control Store**: memoria (tipicamente ROM) che contiene i microprogrammi
- **Microprogram Counter**: indica la microistruzione corrente
- **Sequencer**: determina la prossima microistruzione

**Funzionamento**:
1. L'istruzione macchina viene decodificata
2. Il codice operativo viene usato come indice nel Control Store
3. Viene eseguita la sequenza di microistruzioni corrispondente
4. Ogni microistruzione attiva i segnali di controllo appropriati

**Utilizzo: CISC, non RISC**

La microprogrammazione è tipicamente utilizzata nei processori **CISC** per diverse ragioni:

**Perché CISC usa microprogrammazione**:
1. **Set di istruzioni complesso**: le istruzioni complesse richiedono molti segnali di controllo, gestibili facilmente con microprogrammi
2. **Flessibilità**: correggere o aggiungere istruzioni modificando il microprogramma
3. **Compattezza**: un'istruzione complessa corrisponde a molte microistruzioni, riducendo la dimensione del codice
4. **Varietà di formati**: formati variabili gestiti dal microprogramma

**Perché RISC non usa microprogrammazione**:
1. **Istruzioni semplici**: ogni istruzione richiede pochi segnali, implementabili direttamente in hardware
2. **Velocità**: il controllo cablato è più veloce (nessun accesso al Control Store)
3. **Formato fisso**: semplifica la decodifica hardware
4. **Pipeline efficiente**: il controllo cablato si integra meglio con la pipeline

**Conclusione**: le architetture RISC usano **controllo cablato** (meno flessibile ma più veloce), mentre le CISC usano **controllo microprogrammato** (più flessibile ma più lento).

---

---

# SEZIONE N: ARCHITETTURA MIPS

## N.1

**Descrivere l'architettura MIPS: caratteristiche generali, codifica delle istruzioni, modi di indirizzamento supportati.**

**Caratteristiche generali (MIPS/RISC-V a 32 bit)**

- **Istruzioni**: tutte di dimensione fissa a 32 bit
- **Operazioni sui dati**: esclusivamente registro-registro
- **Operazioni sulla memoria**: solo LOAD e STORE (nessuna operazione memoria-memoria)
- **Registri**: 32 registri di 32 bit (x0-x31 in RISC-V), dove x0 contiene sempre 0
- **Dati**: i registri possono essere caricati con byte, mezze parole e parole

**Codifica delle istruzioni (formati RISC-V)**

**Formato R** (aritmetiche/logiche): 6 campi
- funz7 (7 bit) | rs2 (5 bit) | rs1 (5 bit) | funz3 (3 bit) | rd (5 bit) | codop (7 bit)
- Esempio: `add x1, x2, x3` → x1 ← [x2] + [x3]

**Formato I** (load, immediati):
- immediato (12 bit) | rs1 (5 bit) | funz3 (3 bit) | rd (5 bit) | codop (7 bit)
- Esempio: `lw x8, 1200(x9)` → x8 ← Mem[1200 + [x9]]

**Formato S** (store):
- immediato diviso (7+5 bit) | rs2 | rs1 | funz3 | codop
- Esempio: `sw x1, 1000(x2)` → Mem[[x2]+1000] ← [x1]

**Formato SB** (branch): per salti condizionali
**Formato U** (upper): per costanti a 20 bit (LUI, AUIPC)
**Formato UJ** (jump): per salti incondizionati (JAL)

**Modi di indirizzamento supportati**

1. **Immediato**: `addi x2, x2, 4` (operando nell'istruzione)
2. **Displacement**: `lw x1, offset(x2)` (base + spiazzamento)

**Modi derivabili**:
- **Registro indiretto**: displacement con offset 0
- **Assoluto**: displacement usando x0 come base (sempre 0)
- **Relativo al PC**: per salti condizionati e incondizionati

**Vantaggi dell'architettura**:
- Decodifica semplice e veloce (formato fisso)
- Pipeline efficiente
- Campi sempre nella stessa posizione

---

## N.2

**Descrivere la pipeline MIPS e le sue fasi (IF, ID, EX, MEM, WB).**

La pipeline MIPS/RISC-V prevede 5 stadi, ciascuno completato in un ciclo di clock. A regime, ogni ciclo termina un'istruzione.

**IF (Instruction Fetch)**

- L'istruzione viene prelevata dalla memoria all'indirizzo contenuto nel PC
- IR ← Mem[PC]
- Il PC viene incrementato: PC ← PC + 4
- NPC (Next PC) viene calcolato per eventuali branch

**ID (Instruction Decode / Register Fetch)**

- Decodifica dell'istruzione
- Lettura dei registri sorgente dal register file
- Calcolo dell'immediato (estensione del segno)
- Per formato R: A ← Regs[rs1]; B ← Regs[rs2]
- Per formato I: A ← Regs[rs1]; Imm ← immediato esteso a 32 bit
- Rilevamento delle dipendenze dai dati

**EX (Execute / Address Calculation)**

- La ALU esegue l'operazione richiesta
- Per riferimento a memoria: ALUOutput ← A + Imm (calcolo indirizzo)
- Per ALU registro-registro: ALUOutput ← A funct B
- Per ALU registro-immediato: ALUOutput ← A op Imm
- Per branch: calcolo condizione e indirizzo target

**MEM (Memory Access / Branch Completion)**

- Per LOAD: LMD ← Mem[ALUOutput]
- Per STORE: Mem[ALUOutput] ← B
- Per branch: se condizione vera, PC ← Target
- Per salto incondizionato: PC ← Target

**WB (Write Back)**

- Scrittura del risultato nel register file
- Per LOAD: Regs[rd] ← LMD
- Per operazioni ALU: Regs[rd] ← ALUOutput

**Registri di pipeline**

Tra ogni stadio ci sono registri che memorizzano i dati per lo stadio successivo:
- **IF/ID**: IR, NPC
- **ID/EX**: A, B, Imm, segnali di controllo
- **EX/MEM**: ALUOutput, Target, Cond, B
- **MEM/WB**: ALUOutput o LMD

**Caratteristiche**:
- Due cache separate (istruzioni/dati) evitano conflitti
- Scrittura nel register file nella prima metà del ciclo, lettura nella seconda
- Forwarding per gestire le dipendenze dai dati

---

---

# SEZIONE O: PROCESSORI MULTICORE

## O.1

**Discutere le motivazioni alla base dei processori multicore.**

**Limiti dell'aumento delle prestazioni del singolo core**

1. **Complessità crescente**: tecniche come pipeline superscalare e SMT aumentano la complessità della logica, l'area del chip e le difficoltà di progettazione e debug.

2. **Consumo energetico**: la potenza cresce esponenzialmente con la densità del chip e la frequenza di clock. L'aumento della frequenza non è più sostenibile termicamente.

3. **Regola di Pollack**: le prestazioni sono proporzionali alla radice quadrata dell'incremento in complessità. Raddoppiare la complessità di un core produce solo ~40% di prestazioni aggiuntive.

4. **Diminuzione dei ritorni**: innovazioni architetturali su singolo core offrono miglioramenti sempre minori.

**Vantaggi dei processori multicore**

1. **Scalabilità**: più core permettono miglioramento quasi lineare delle prestazioni (per carichi parallelizzabili).

2. **Efficienza energetica**: più core a frequenza moderata consumano meno di un singolo core ad alta frequenza a parità di prestazioni.

3. **Sfruttamento dell'area**: invece di aumentare la complessità di un core, si replicano core più semplici.

4. **Utilizzo della cache**: un singolo core difficilmente utilizza efficacemente tutta la cache disponibile su chip moderni.

5. **Parallelismo intrinseco**: molte applicazioni moderne sono naturalmente parallele (server, multimedia, calcolo scientifico).

**Limitazioni**

- Lo speedup dipende dalla parallelizzabilità del codice
- Il codice sequenziale limita i benefici (legge di Amdahl): 10% di codice sequenziale su 8 core limita lo speedup a ~4.7x
- Overhead di comunicazione e sincronizzazione

---

## O.2

**Si descrivano le possibilità alternative di organizzazione di un processore multicore.**

**Variabili progettuali**

Le scelte principali riguardano:
- Numero di core per chip
- Numero di livelli di cache
- Quantità di cache condivisa vs. dedicata

**Organizzazioni della cache**

**1. L1 dedicata + L2 condivisa**

Ogni core ha la propria cache L1, tutti condividono la L2.

*Vantaggi L2 condivisa*:
- Riduzione dei miss totali (dati condivisi presenti una sola volta)
- Allocazione dinamica dello spazio ai core in base alle necessità
- Comunicazione inter-processo facilitata
- Problema di coerenza confinato a L1

**2. L1 e L2 dedicate**

Ogni core ha cache L1 e L2 proprie.

*Vantaggi*:
- Accesso più rapido a L2
- Migliori prestazioni per thread con forte località
- Nessuna contesa per la cache

**3. L1 e L2 dedicate + L3 condivisa**

Configurazione comune nei processori moderni. Esempio: Intel Core i7.

**Architetture dei core**

- **Superscalare**: singolo program counter, singolo banco registri, esecuzione parallela di più istruzioni
- **SMT (Simultaneous Multithreading)**: multipli program counter, banchi registri replicati, un core esegue thread multipli
- **Multicore omogeneo**: tutti i core identici
- **Multicore eterogeneo**: core di tipo diverso (es. big.LITTLE ARM)

**Esempi di architetture reali**

- **Intel Core Duo (2006)**: 2 core superscalari, L1 dedicata (32KB), L2 condivisa (2MB)
- **Intel Core i7 (2008)**: core SMT, L1 (32KB) e L2 (256KB) dedicate, L3 condivisa (8MB)
- **Intel Core i7-5960X (2014)**: 8 core, L2 dedicate (256KB), L3 condivisa (20MB)
- **ARM Cortex-A15 MPCore**: multicore omogeneo per mobile, cache L1 dedicate, L2 condivisa, Snoop Control Unit per coerenza

---