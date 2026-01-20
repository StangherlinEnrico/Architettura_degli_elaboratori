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

---

## D.2

**Spiegare in dettaglio le differenze tra DRAM ed SRAM discutendone vantaggi e svantaggi.**

---

## D.3

**Descrivere in dettaglio le memorie DRAM.**

---

## D.4

**Descrivere le memorie ROM e le loro varianti (PROM, EPROM, EEPROM, Flash Memory).**

---

## D.5

**Spiegare cos'è un blocco e perché la memoria viene suddivisa in blocchi.**

---

## D.6

**Nel contesto di una gerarchia di memoria, spiegare i possibili modi di realizzazione del mapping dei blocchi (direct mapping, fully associative, set associative), discutendo criticamente vantaggi e svantaggi di ogni modo.**

---

## D.7

**Descrivere le politiche di rimpiazzo della cache (FIFO, LFU, LRU).**

---

## D.8

**Nel contesto di una gerarchia di memoria spiegare come funzionano le politiche di scrittura write-through e write-back. Per ogni politica discutere criticamente i problemi che possono sorgere nell'adottarla.**

---

## D.9

**Nel contesto di una gerarchia di memoria, spiegare come i miss possono essere categorizzati in diversi tipi e dire quali strategie, per ogni tipo, si possono adottare per diminuirne il numero.**

---

## D.10

**Si metta a confronto criticamente il modo in cui un'architettura RISC utilizza l'ampio banco di registri a sua disposizione rispetto alla gestione di una cache.**

---

---

# SEZIONE E: MEMORIE SECONDARIE E RAID

## E.1

**Descrivere l'organizzazione delle memorie ottiche (dischi magnetici): tracce, settori, velocità angolare costante e registrazione a più zone.**

---

## E.2

**Descrivere le prestazioni del disco (seek time, latenza, access time, transfer time, tempo totale).**

---

## E.3

**Discutere le ragioni per cui è stato sviluppato il sistema RAID e descrivere in dettaglio i vari livelli (RAID 0-6).**

---

## E.4

**Descrivere l'organizzazione delle informazioni nel nastro magnetico.**

---

---

# SEZIONE F: RILEVAMENTO E CORREZIONE ERRORI

## F.1

**Descrivere come vengono gestiti gli errori in un calcolatore e spiegare il codice di Hamming.**

---

---

# SEZIONE G: RAPPRESENTAZIONE DEI NUMERI

## G.1

**Spiegare la differenza tra la codifica dei numeri interi in complemento a 2 rispetto a quella in modulo e segno.**

---

## G.2

**Si spieghi in dettaglio la codifica in complemento a due dei numeri interi. Si discutano poi i problemi legati alla realizzazione della moltiplicazione di due interi rappresentati in complemento a due, esemplificando tali problemi su un caso concreto di moltiplicazione.**

---

## G.3

**Si spieghi in dettaglio l'hardware adottato per la realizzazione della somma e sottrazioni di numeri interi rappresentati in complemento a due.**

---

## G.4

**Spiegare in dettaglio la rappresentazione dei numeri reali secondo lo standard IEEE 754.**

---

## G.5

**Spiegare nel dettaglio lo schema per realizzare la moltiplicazione tra numeri reali dello standard IEEE 754.**

---

## G.6

**Spiegare in dettaglio la divisione fra numeri reali secondo lo standard IEEE 754.**

---

---

# SEZIONE H: ISTRUZIONI E FORMATI

## H.1

**Quali sono i fattori che determinano la lunghezza delle istruzioni di una CPU?**

---

## H.2

**Discutere nel dettaglio in cosa consiste il formato variabile per le istruzioni. Dare esempi di formati variabili.**

---

## H.3

**Si descrivano i possibili formati di codifica di un'istruzione, specificando per ogni formato la sua composizione tipica, i pregi e i difetti.**

---

## H.4

**Si descrivano i formati delle istruzioni MIPS visti a lezione, discuterne caratteristiche, pregi e difetti.**

---

## H.5

**Descrivere le operazioni macchina e i tipi di operazioni (trasferimento, aritmetica, I/O, confronto, salto, stop).**

---

## H.6

**Descrivere i tipi di operandi delle istruzioni (indirizzi, numeri, caratteri, dati logici).**

---

---

# SEZIONE I: MODI DI INDIRIZZAMENTO

## I.1

**Si descriva in dettaglio la modalità di indirizzamento indiretto, discuterne pregi e difetti. L'adozione di tale indirizzamento è favorito o sfavorito in un'architettura RISC? Motivare la risposta.**

---

## I.2

**Si descrivano in dettaglio le modalità di indirizzamento con spiazzamento e a pila. In particolare, si confrontino criticamente i 2 modi di indirizzamento e se ne discutano i pregi e i difetti.**

---

## I.3

**Descrivere i modi di indirizzamento: immediato, diretto, indiretto, registro, registro indiretto, spiazzamento.**

---

---

# SEZIONE J: PROCEDURE E REGISTRI

## J.1

**Descrivere i possibili approcci per trattare l'indirizzo di ritorno da una chiamata di procedura.**

---

## J.2

**Cos'è una procedura?**

---

## J.3

**Descrivere i registri: classificazione in registri utente (uso generale e particolare) e registri di stato e controllo (PC, MAR, IR, MBR).**

---

## J.4

**Descrivere l'organizzazione della memoria per segmenti.**

---

---

# SEZIONE K: PIPELINE

## K.1

**Cos'è il Pipelining? Descrivere le fasi (FI, DI, CO, FO, EI, WO).**

---

## K.2

**Descrivere il prefetch delle istruzioni.**

---

## K.3

**Descrivere il fattore velocizzante (speedup) di una pipeline a k stadi.**

---

## K.4

**Descrivere le problematiche della pipeline (sbilanciamento delle fasi, problemi strutturali, dipendenza dai dati, dipendenza dai controlli).**

---

## K.5

**Nel contesto di una pipeline descrivere la problematica della dipendenza dei dati e si discutano in dettaglio le tecniche viste a lezione per trattare il problema.**

---

## K.6

**Nel contesto di una pipeline, descrivere nel dettaglio la tecnica del data forwarding: a cosa serve?**

---

## K.7

**Nel contesto della pipeline MIPS, si illustri in che modo lo stadio ID è in grado di rilevare la dipendenza dei dati.**

---

## K.8

**Nel contesto di una pipeline descrivere la problematica della dipendenza dal controllo e si discuta in particolare la tecnica del buffer circolare, spiegando in quali situazioni tale tecnica è particolarmente efficace.**

---

## K.9

**Nel contesto di una pipeline, descrivere nel dettaglio la tecnica della predizione di salto utilizzando 2 bit di predizione.**

---

## K.10

**Nel contesto di una pipeline si consideri la tecnica che utilizza la tabella della storia dei salti. A cosa serve? Descriverla in dettaglio.**

---

## K.11

**Nel contesto di una pipeline si spieghi in dettaglio la tecnica del salto ritardato. Fornire un esempio.**

---

---

# SEZIONE L: ARCHITETTURE RISC E CISC

## L.1

**Spiegare nel dettaglio la differenza fra un'architettura CISC e una RISC.**

---

## L.2

**Spiegare le motivazioni alla base dell'architettura CISC.**

---

## L.3

**Descrivere l'architettura RISC: caratteristiche, strategie hardware e software, organizzazione in finestre di registri.**

---

## L.4

**Spiegare in dettaglio come le architetture RISC trattino efficacemente le chiamate annidate di procedura.**

---

## L.5

**Spiegare in che modo un compilatore possa aiutare l'utilizzo efficace dei registri da parte di un'architettura RISC.**

---

---

# SEZIONE M: MICROPROGRAMMAZIONE

## M.1

**Descrivere sinteticamente l'implementazione delle istruzioni attraverso la tecnica della microprogrammazione. Dire se questa tecnica viene utilizzata per i processori CISC o RISC, e motivare la risposta.**

---

---

# SEZIONE N: ARCHITETTURA MIPS

## N.1

**Descrivere l'architettura MIPS: caratteristiche generali, codifica delle istruzioni, modi di indirizzamento supportati.**

---

## N.2

**Descrivere la pipeline MIPS e le sue fasi (IF, ID, EX, MEM, WB).**

---

---

# SEZIONE O: PROCESSORI MULTICORE

## O.1

**Discutere le motivazioni alla base dei processori multicore.**

---

## O.2

**Si descrivano le possibilità alternative di organizzazione di un processore multicore.**

---