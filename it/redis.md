
\thispagestyle{empty}
\changepage{}{}{}{-0.5cm}{}{2cm}{}{}{}
![The Little Redis Book, By Karl Seguin](title.png)\ 

\clearpage
\changepage{}{}{}{0.5cm}{}{-2cm}{}{}{}

## Riguardo questo Libro 

### Licenza

Little Redis Book è rilasciato sotto licenza *Attribution-NonCommercial 3.0 Unported*. Non dovresti aver pagato per questo libro.

Sei libero di copiare, distribuire, modificare o mostrare questo libro. Comunque, è richiesto di attribuire sempre il libro all'autore Karl Seguin, e di non utilizzare il libro per scopi commerciali.

Il *testo completo* della **licenza** è disponibile su:
<http://creativecommons.org/licenses/by-nc/3.0/legalcode>

### Riguardo l'Autore

Karl Seguin è uno sviluppatore con esperienza in vari campi e tecnologie. Contribuisce attivamente a progetti Open-Source, è un technical writer e a volte uno speaker. Ha scritto vari articoli e alcuni strumenti, riguardanti Redis. Redis fornisce classifiche e le statistiche per un suo servizio dedicato a sviluppatori occasionali di giochi: [mogade.com](http://mogade.com/).

Karl ha scritto inoltre [The Little MongoDB Book](http://openmymind.net/2011/3/28/The-Little-MongoDB-Book/), un libro gratuito e popolare riguardante MongoDB.

Il suo blog si trova a <http://openmymind.net> e i suoi tweets su [@karlseguin](http://twitter.com/karlseguin)

### Ringraziamenti 

Un ringraziamento speciale va a [Perry Neal](https://twitter.com/perryneal) per aver *prestato* i suoi occhi, la sua mente e la sua passione. Grazie per l'inestimabile aiuto.


### Ultima Versione 

L'ultima versione di questo libro è disponibile su:  
[http://github.com/karlseguin/the-little-redis-book](http://github.com/karlseguin/the-little-redis-book)

\clearpage


## Introduzione


Durante gli ultimi due anni, le tecniche e gli strumenti usati per la persistenza e la ricerca dei dati hanno avuto una crescita incredibile. Anche se, per il momento, non saranno sicuramente abbandonati i databases relazionali,  possiamo però affermare che il panorama attorno alle basi di dati è cambiato radicalmente e non rimarrà più lo stesso. 

Tra tutti i nuovi strumenti, a mio parere, Redis si è attestato come il più interessante. Come mai? Prima di tutto dato che è incredibilmente facile da imparare: qualche ora con Redis è sufficiente per cominciare a trovarsi a proprio agio. E poi, risolve un insieme di problemi specifico, ma rimanendo al contempo abbastanza generico. Cioè? cosa si intende esattamente con questo? Redis non si propone di permettere ogni tipo di operazione con ogni tipo di dato: fornisce invece i tipi dato base cui sono abituati tutti gli sviluppatori, e permette in modo semplice di combinarli in modo più complesso. Dopo un breve apprendimento, diventerà evidente a cosa sia adatto e a cosa meno. E non appena lo sarà, dal punto di vista dello sviluppo, sarà una gran cosa.


Anche se è possibile creare un sistema completo usando unicamente Redis, penso che la maggior parte delle persone lo troverà più un complemento alla propria soluzione di persistenza - sia essa fornita da un tradizionale database relazionale, un database documentale, o qualche altra cosa ancora. 
Un complemento che di solito permette di fornire delle specifiche funzionalità. A tal proposito risulta abbastanza simile a un sistema di indicizzazione: non si baserebbe mai la propria applicazione unicamente su Lucene, ma per una ricerca efficiente, Lucene fornisce un esperienza d'uso veramente gratificante, sia per l'utente che per l'amministratore. D'altronde le similitudini tra un sistema di indicizzazione e Redis terminano qui.

Il fine di questo libro è porre le basi per padroneggiare Redis:
ci si concentrerà nell'imparare le cinque strutture dati fondamentali e nel conoscere i vari approcci alla modellazione dati. Si introdurranno poi alcuni dettagli amministrativi e infine alcune tecniche di debugging.

## Come Iniziare

Ognuno di noi ha un modo di imparare differente: alcuni preferiscono "sporcarsi subito le mani", altri preferiscono guardare dei video, altri ancora prediligono leggere. Niente aiuterà di più la comprensione di Redis che non provarlo. 
E' facile da installare e di base è fornito di una shell per cominciare subito a sperimentare. Un paio di minuti e sarà già pronto e funzionante sulla propria macchina.

### Su Windows

Redis non supporta ufficialmente Windows, ma c'è comunque questa possibilità: anche se non userei questa versione in produzione, non vi sono limitazioni note per fare delle prove di sviluppo.

E' possibile scaricare da <https://github.com/dmajkic/redis/downloads> la versione più aggiornata (in cima alla lista).

Estrarre lo zip file e accedere, a seconda dell'architettura, alla cartella  `64bit` o `32bit`.

### Su *nix e MacOSX

Per gli utenti *nix e Mac, compilare i sorgenti dovrebbe essere la migliore opzione. Le istruzioni, assieme all'ultima versione, sono disponibili su <http://redis.io/download>. Al momento l'ultima versione è la 2.6.4; per installare tale versione eseguire da terminale:

	wget http://redis.googlecode.com/files/redis-2.4.6.tar.gz
	tar xzf redis-2.4.6.tar.gz
	cd redis-2.4.6
	make

(Alternativamente Redis si può trovare attraverso vari package managers. Per esempio, per gli utenti Mac che abbiano Homebrew installato potrà fare al caso un semplice `brew install redis`.)

Se si sono compilati i sorgenti, gli eseguibili si troveranno entro la cartella `src`. Entrare in `src` (`cd src`).


### Esecuzione e Connessione a Redis

Se è andato tutto a buon fine, gli eseguibili dovrebbero essere nella cartella corrente. Ci si concentrerà dapprima su Redis server e sull'interfaccia a linea di comando. Per far partire il server sotto Windows doppio click su `redis-server`, sotto *nix/MacOSX esegire `./redis-server` da terminale.


Nel messaggio di avvio Redis avvisa la mancanza del file `redis.conf`. Redis userà in questo caso delle impostazioni di default, perfette per i nostri scopi. 

Si esegua quindi l'interfaccia a linea di comando facendo doppio click su `redis-cli` (Windows) o eseguendo `redis-cli` (*nix/MacOSX). Così facendo si effettuerà una connessione al server locale sulla porta di default (6379).

Si può testare che tutto funzioni impartendo il comando `info`. Dovrebbero essere così mostrate una serie di coppie chiave-valore che danno a colpo d'ochio le caratteristiche e lo stato del server.

In caso di problemi è possibile cercare aiuto nel 
[gruppo di supporto ufficiale di Redis](https://groups.google.com/forum/#!forum/redis-db).


## I Drivers per Linguaggio

Come si scoprirà presto, l'approccio di programmazione in Redis è di tipo procedurale. In questo modo, sia che si usi l'interfaccia a linea di comando, o che si usi un driver per il proprio linguaggio favorito, non ci saranno molte differenze. Quindi non ci saranno problemi nel seguire questa guida adottando un qualsiasi linguaggio di programmazione (Saranno presenti in questa guida degli esempi nel linguaggio Ruby). In caso si opti per questa scelta, è possibile scaricare i drivers a partire dalla pagina [client page](http://redis.io/clients).

\clearpage

## Capitolo 1 - Fondamenti

Cosa distingue e rende speciale Redis? Che tipo di problemi risolve? Di cosa si dovrebbero preoccupare gli sviluppatori nell'usarlo? Prima di rispondere a tali domande, si dovrebbe aver ben chiaro cosa Redis sia.

Si sente spesso dire che Redis sia un datastore chiave-valore persistito, ma non  è propriamente così: anche se Redis mantiene i dati in memoria e li persiste su disco, d'altra parte è molto più di un datastore chiave-valore. E' importante abbandonare questo pregiudizio in quanto, in caso contrario, la propria prospettiva riguardo a Redis, e ai problemi che può risolvere, risulterà troppo ristretta.


In effetti Redis espone cinque differenti strutture dati, delle quali solo una è una tipica struttura chiave-valore. Capire queste cinque strutture, come funzionino, quali metodi espongano e cosa si possa modellare con esse è la chiave per capire Redis. Per prima cosa, vediamo come siano queste strutture dati.

Se dovessimo applicare il concetto di struttura dati al mondo relazionale, potremmo dire che i database espongono una e unica struttura dati: la tabella. Le tabelle in tal modo devono gestire sia casi complessi che semplicie quindi essere flessibili. Non esiste molto che non possa essere modellato con esse. D'altra parte, la loro struttura, così generica, può anche dar luogo a problematiche. In particolare, niente è così semplice o veloce, come potrebbe essere. Se invece avessimo a disposizione, invece di una unica struttura per tutto, di strutture dati più specifiche? Ci potrebbero essere delle cose che non si possono più fare (o almeno, non altrettanto bene), ma sicuramente si gudagnerebbe in semplicità e velocità! 


Usare sutrutture dati specifiche per specifici problemi? Ma non è esattamente quello che facciamo quando programmiamo? A mio parere, questo è proprio l'approccio di Redis. Se ci si trova ad operare con scalari, liste, 'hashes', insiemi, perché non persisterle appunto come scalari, liste, 'hashes' e insiemi?
Ancora, perché verificare la presenza di una chiave deve essere più complesso di un `exists(key)` o più lento di O(1) (cioè un tempo constante, indipendente da quanti elementi sono presenti)?


## Elementi Fondamentali

### Basi di Dati

Redis si fonda sugli stessi concetti di un database: persiste un insieme di dati, mantiene inoltre coerenti i dati di un'applicazione tenendoli separati da quelli di una seconda.

In Redis, i database sono identificati da un numero, quello di default è il numero `0`. Per selezionare un database differente è sufficiente usare il comando `select` seguito dal numero identificiativo: ad esempio digitare `select 1`. Redis dovrebbe restituire il messaggio `OK` e il prompt dovrebbe diventare qualcosa del tipo `redis 127.0.0.1:6379[1]>`. Per tornare al database di default è sufficiente digitare `select 0`.

### Comandi, Chiavi e Valori

Anche se Redis è più di un datastore chiave-valore, di base, ognuna delle cinque strutture dati Redis è costituita da almeno una chiave e un valore. E' quindi fondamentale capire il modello chiave-valore prima di proseguire.

Le chiavi identificano porzioni di dati. Nel seguito avremo parecchio a che fare con le chiavi ma, per il momento, è sufficiente sapere che una chiave può essere ad esempio `users:leto`. Se si usa una chiave siffatta, ci si può ragionevolmente attendere che tale chiave si riferisca alle informazioni di un utente (user) chiamato `leto`. I due punti non hanno uno speciale significato, ma usare un separatore per organizzare le chiavi è un ottimo modo per strutturare i propri dati.

I valori rappresentano invece il dato effettivo associato alla chiave. Potrebbero assumere qualsiasi formato: a volte stringhe, altre volte interi, altre volte ancora oggetti serializzati (in JSON, XML o qualche altro formato ancora). Per lo più i valori vengono considerati come array di byte, non importando la loro rappresentazione effettiva. Si noti comunque che i vari drivers trattano la serializzazione in maniera differente tra loro (a volte delegandola direttamente all'utente) per cui in questo libro tratteremo unicamente le stringhe, gli interi e il formato JSON.

Sporchiamoci, come si dice, un po' le mani sulla tastiera e impartiamo il seguente comando:

	set users:leto "{name: leto, planet: dune, likes: [spice]}"

Questo esempio presenta il tipico formato di un comando Redis: per primo abbiamo l'effettivo comando (`set`) Sono quindi presenti i parametri. Il comando `set` possiede due parametri: la chiave che stiamo impostando (`users:leto`) e il valore che stiamo assegnando alla chiave stessa (`"{name: leto, planet: dune, likes: [spice]}"`). Molti comandi, anche se non tutti, accettano come parametro una chiave (e in questo caso è spesso il primo parametro). Dato che si è usato il comando `set` per impostare un valore, che comando si userà mai per ritrovare tale dato? Probabilmente verrà subito in mente il comando `get`:

	get users:leto


Si proceda e si provi con altre combinazioni. Chiavi e valori sono concetti fondamentali, e i comandi `get` e `set`sono il modo più semplice per manipolarli. Si creino più utenti, si provino altri tipi di chiave e altri tipi di valori.

### Interrogazioni

Mentre procederemo, due cose diventeranno chiare. Per quanto riguarda Redis, le chiavi hanno importanza primaria, i valori secondaria. Per esempio, Redis non permette una ricerca all'interno dell'insieme degli oggetti memorizzati come valori. Posto questo, non è possibile trovare gli utenti che siano abitanti del pianeta `dune`, almeno non direttamente.


Per alcuni, la cosa causerà qualche preoccupazione. Viviamo in un mondo in cui la ricerca dei dati è così flessibile e potente che l'approccio di Redis potrà sembrare primitivo e per niente pragmatico: non ci si lasci traviare da questa prima impressione. Come si è detto Redis non è stato pensato per essere una soluzione a tutti i problemi. Ci saranno delle cose che non sarà adatto a risolvere (in particolare data questa limitazione nella ricerca dei dati). Si tenga comunque presente che, in molti casi, un diverso modo di strutturare e modellare i propri dati farà fronte a dette limitazioni.  


Faremo ulteriori esempi ma, per il momento, è importante capire queste scelte di base che hanno guidato lo sviluppo di Redis. Aver capito che Redis si limita a memorizzare, e non a interpretare, i valori ci aiuterà sicuramente a creare e a sviluppare una migliore *forma mentis* per modellare le entità in questo nuovo ambito.


### Memoria e Persistenza

Abbiamo accennato che Redis è un data store in memoria persistente. Per quanto riguarda la persistenza, di default, Redis esegue un'immagine su disco del datastore in base al numero di chiavi che sono cambiate. In particolare è possibile configurare in modo che se X è il numero di chiavi che cambia, allora avvenga un salvataggio ogni Y secondi. Di default, Redis salverà il database ogni 60 secondi se 1000 o più chiavi sono cambiate, oppure ogni 15 minuti se sono cambiate nel frattempo 9 chiavi o meno.


In alternativa (o anche in aggiunta al salvataggio periodico), Redis può funzionare in modalità `aggiunta` (*append*). Ogni qualvolta una chiave subisca un cambiamento, un file viene aggiornato su disco. Vale a dire, a volte è accettabile sacrificare, in caso di un guasto hardware o software, 60 secondi di dati, per avere migliori prestazionei. In altri casi questa perdita non è accettabile: Redis delega all'utilizzatore la scelta. Nel quinto capitolo verrà presentata una terza possilità, delegare la persistenza a un sistema secondario (*slave*)

Per quanto riguarda la memoria, Redis mantiene tutti i propri dati in memoria. Un'ovvia implicazione di questo è che i costi di un sistema Redis si spostano verso la RAM, ad oggi ancora la parte più costosa di un server.

Ad ogni modo si tenga presente quanto poco spazio i dati possano occupare: l'opera completa di Shakespeare occupa approssimativamente 5.5MB di spazio. Inoltre, per quanto riguarda la scalabilità, molte soluzioni tendono ad essere limitate dall'*input-output* o dalla potenza di calcolo (CPU). Quale limitazione (RAM o IO) richieda l'aggiunta di più server dipende dal tipo di dati che si deve memorizzare. A meno che non si debbano memorizzare grandi files multimediali con Redis, la limitazione sulla memoria probabilmente non sarà un problema. Per applicazioni in cui ci sia detta limitazione, si dovrebbe comunque valutare un compromesso tra essere limitati dall'IO o essere limitati dalla memoria RAM.


Redis aveva aggiunto un supporto per la memoria virtuale. Sfortunatamente questa soluzione è stata deprecata in quanto considerata impraticabile dai suoi stessi sviluppatori.


Come nota a margine, i 5.5MB dell'opera completa di Shakespeare possono essere compressi all'incirca a 2MB. Redis non esegue un auto-compressione dei dati, ma dato che considera i dati a livello di bytes, non c'è motivo per cui non possano essere compressi applicativamente, scegliendo un compromesso tra il maggior tempo speso per comprimere/decomprimere e la quantità di RAM occupata.




### Compore il Tutto

Si sono toccati diversi argomenti ad un alto livello. Occorre ora mettere tutte queste cose assieme. In particolare, le limitazioni sulle interrogazioni, le strutture dati e il modo di memorizzare i dati in memoria in Redis.


Quando si mettono queste tre cose assieme si ottiene una cosa fantastica: velocità. Alcune persone potrebbero dire: "Per forza è veloce, tutto è in memoria". Ma questo è solo un aspetto. La vera ragione per cui Redis spicca rispetto ad altre soluzioni sono le sue strutture dati specializzate. 

Ma quanto veloce? Dipende da un gran numero di fattori - che comandi si usino, i tipi di dato e così via. Ad ogni modo le prestazioni di Redis si possono misurare in decine o addirittura centinaia di migliaia di operazioni **per secondo**. Per rendersene conto è sufficiente eseguire `redis-benchmark` dalla stessa cartella in cui si trovano `redis-server` e `redis-cli`.

Una volta ho adattato il codice che usava un modello tradizionale per usare Redis. Occorrevano più di cinque minuti per eseguire un test di carico usando il modello relazionale. Per completare lo stesso test con Redis occorrevano solo 150ms! Non è detto che si abbia sempre questo tipo di miglioramento, ma questo esempio può dare un'idea di quello di cui stiamo parlando...

E' importante capire questo aspetto di Redis, perché può cambiare il modo di farne uso. Solitamente provenendo da un solido background SQL si cerca di minimizzare il numero di interazioni col database. Questo rimane comunque una buona pratica anche in Redis. D'altra parte, dato che si ha a che fare con strutture dati più semplici ed efficienti, a volte sarà necessario interagire col server un numero maggiore di volte per lo stesso scopo. Questo potrebbe risultare forzato in un primo momento: si tenga presente però che il costo di queste interazioni tenderà a diventare ininfluente se confrontato con il guadagno globale di prestazioni offerto da Redis.



### In Questo Capitolo

Anche se in superficie, si sono	toccati una gran varietà di argomenti.
Non ci si preoccupi se non risulta ancora tutto chiaro (per esempio le interrogazioni). Nel prossimo capitolo verranno approfonditi e ogni dubbio verrà auspicabilmente fugato. 

Le parti più importanti del capitolo sono:

* Le chiavi sono stringhe che identificano porzioni di dati (i valori)

* I valori sono qualsivoglia sequenze di byte che Redis non interpreta

* Redis espone (ed è implementato come) cinque strutture base specializzate

* Assieme, queste cose, fanno di Redis un data store veloce e facile da usare, ma ovviamente non adatto a tutti gli scenari

\clearpage

## Capitolo 2 - Le Strutture Dati

E' il momento di vedere in dettaglio le cinque strutture dati costituenti Redis. Cosa ogni struttura sia, che metodi siano disponibili e, infine, che tipo di dati usare con ognuna di esse.

Quando si è usato il comando `set`, che struttura dato usava Redis? Redis inferisce dal comando usato la struttura dato da utilizzare. Per esempio quando si usa il comando `set` si memorizza il valore come stringa. Usando `hset` si vorrà usare un hash. Dato il limitato numero di comandi Redis, il tutto risulta maneggevole.


**[Il sito di Redis](http://redis.io/commands) ha un'ottima documentazione di riferimento. E' inutile ripetere l'ottimo lavoro che hanno già fatto. Si prenderanno in considerazione unicamente i comandi più importanti in modo da capire la finalità delle varie strutture dati.**

Non c'è niente di meglio che divertirsi e provare le cose. E' sempre possibile cancellare tutti gli inserimenti nel proprio database impartendo il comando `flushdb`, per cui non siate timidi e provate le cose più inconsuete!


### Stringhe

Le stringhe sono la struttura dati più semplice in Redis. Quando si pensa a una coppia chiave-valore, si sta pensando a delle stringhe. Ma non ci si confonda per il nome: come sempre, i propri valori possono rappresentare qualsiasi cosa. Personalmente preferisco parlare di `scalari`, ma è semplicemente una preferenza personale.


Si è già visto un comune caso d'uso per le stringhe, memorizzare istanze di oggetti in base a una chiave. E' un caso d'uso molto frequente:

	set users:leto "{name: leto, planet: dune, likes: [spice]}"


Inoltre Redis permette alcune operazioni molto comuni. Per esempio `strlen <key>` può essere usato per determinare la lunghezza di un valore associato a una chiave; `getrange <key> <start> <end>` ritorna l'intervallo specificato di un valore; `append <key> <value>` concatena <value> al valore associato alla chiave se essa esiste altrimenti la crea e la inizializza con <value> nel caso non esistesse precedentemente. Si prosegua provando i suddetti comandi:
	
	> strlen users:leto
	(integer) 42
	
	> getrange users:leto 27 40
	"likes: [spice]"
	
	> append users:leto " OVER 9000!!"
	(integer) 54


Si potrà pensare, utile!... ma che senso può avere estrarre un intervallo o concatenare un valore a una sequenza JSON? Effettivamente, il punto qui è che alcuni comandi, specialmente quelli per la struttura dati stringa, hanno senso unicamente con alcuni tipi di dati.

In precedenza si è detto che Redis non dà importanza ai valori. Questo per la maggior parte è vero, anche se alcuni comandi sono specifici per valori di un certo tipo o struttura. Per esempio, i comandi `append` e `getrange` potrebbero essere utili per un'efficiente serializzazione dati in termini di spazio. Per fare un altro esempio immediato si può ricorrere ai comandi  `incr`, `incrby`, `decr` e `decrby`. Questi comandi incrementano e decrementano il valore di una stringa:

	> incr stats:page:about
	(integer) 1
	> incr stats:page:about
	(integer) 2
	
	> incrby ratings:video:12333 5
	(integer) 5
	> incrby ratings:video:12333 3
	(integer) 8


Come si può intuire, le stringhe Redis sono ottime per costruire delle statistiche. Si provi invece a incrementare `users:leto` (un valore non intero) e si veda cosa succede: si dovrebbe ottenere un errore.

Un esempio avanzato sono i comandi `setbit` e `getbit`. Si veda ad esempio il  [fantastico post](http://blog.getspool.com/2011/11/29/fast-easy-realtime-metrics-using-redis-bitmaps/) in cui Spool usa in maniera molto efficiente questi due comandi per rispondere al questito "quanti visitatori unici si sono avuti oggi?". Un portatile, dati 128 milioni di utenti, genera la risposta in meno di 50 ms occupando meno di 16MB di memoria.

Non è importante capire come le mappe di bit funzionino, o come Spool le usi, ma apprezzare come le stringhe Redis siano più potenti di quanto inizialmente non sembri. Ad ogni modo il caso più comune per le stringhe è memorizzare oggetti (complessi o meno) e contatiri. Inoltre, dato che ottenere un valore per chiave è così veloce, sono spesso usate come una cache per i dati.



### Hashes

Gli hash sono un ottimo esempio del fatto che definire Redis un data store chiave valore non sia accurato. Per molti versi gli hash sono come le stringhe. La differenza fondamentale sta nel fatto che forniscono un ulteriore livello di indirezione tramite un campo (field).  Di conseguenza i corrispondenti di `set` e `get` per un hash diventano:

	hset users:goku powerlevel 9000
	hget users:goku powerlevel

<
Hashes are a good example of why calling Redis a key-value store isn't quite accurate. You see, in a lot of ways, hashes are like strings. The important difference is that they provide an extra level of indirection: a field. Therefore, the hash equivalents of `set` and `get` are:
>

E' possibile impostare/ritornare gruppi di campi in una volta sola, ottenere tutti i campi e i valori, avere la lista di tutti i campi, e infine cancellare un singolo campo della struttura:

	hmset users:goku race saiyan age 737
	hmget users:goku race powerlevel
	hgetall users:goku
	hkeys users:goku
	hdel users:goku age


<
We can also set multiple fields at once, get multiple fields at once, get all fields and values, list all the fields or delete a specific field:
>

Come si può vedere, gli hash forniscono delle possibilità in più rispetto alle semplici stringhe. Invece di salvare i dati di utente come un singolo valore serializzato, si possono usare gli hash per avere una rappresentazione più strutturata e accurata. Si avrebbe così il vantaggio di poter ottenere, aggiornare e cancellare specifiche porzioni del dato, senza dover estrarre o scrivere l'intero valore.

<
As you can see, hashes give us a bit more control over plain strings. Rather than storing a user as a single serialized value, we could use a hash to get a more accurate representation. The benefit would be the ability to pull and update/delete specific pieces of data, without having to get or write the entire value. 
>

Per capire come funzionino, è importante vedere gli hash come oggetti strutturati. Inoltre, dal punto di vista delle performance, un controllo più granulare è certamente utile. D'altra parte, nel prossimo capitolo, si vedrà come gli hash possano essere usati per organizzare i dati e rendere possibili alcuni tipi di interrogazioni altrimenti impossibili. A mia opinione, è in questo che gli hash brillano.

<
Looking at hashes from the perspective of a well-defined object, such as a user, is key to understanding how they work. And it's true that, for performance reasons, more granular control might be useful. However, in the next chapter we'll look at how hashes can be used to organize your data and make querying more practical. In my opinion, this is where hashes really shine.
>

### Liste

Le liste permettono di memorizzare e manipolare serie di valori associati a una data chiave. Si possono aggiungere valori a una lista, avere il primo o l'ultimo elemento e manipolare valori a un dato indice. Le liste mantengono l'ordinamento e forniscono efficienti operazioni basate sull'indice. Si potrebbe considerare la lista dei nuovi utenti che tiene traccia degli ultimi utenti regstriati al nostro sito:


	rpush newusers goku
	ltrim newusers 0 50

<
Lists let you store and manipulate an array of values for a given key. You can add values to the list, get the first or last value and manipulate values at a given index. Lists maintain their order and have efficient index-based operations. We could have a `newusers` list which tracks the newest registered users to our site:
>

Si inserisce un nuovo utente in testa alla lista, quindi si ritaglia (trim) la stessa in modo che contenga gli ultimi 50 utenti, secondo un pattern molto comune. L'operazione `ltrim` ha una complessità O(N), dove N sono il numero degli elementi da rimuovere. Nel nostro caso, dove si effettua il `trim` dopo un singlolo inserimento, si avrà sempre un tempo costante O(1) (dato che N sarà seempre uguale a 1).


<
First we push a new user at the front of the list, then we trim it so that it only contains the last 50 users. This is a common pattern. `ltrim` is an O(N) operation, where N is the number of values we are removing. In this case, where we always trim after a single insert, it'll actually have a constant performance of O(1) (because N will always be equal to 1).
>

Questo è anche il primo esempio in cui si può vedere una relazione tra valori di chiave. Se si volessero quindi i dettagli degli ultimi 10 utenti, si userebbe, tramite il driver Ruby, la seguente combinazione:

	keys = redis.lrange('newusers', 0, 10)
	redis.mget(*keys.map {|u| "users:#{u}"})




<
This is also the first time that we are seeing a value in one key referencing a value in another. If we wanted to get the details of the last 10 users, we'd do the following combination:
>


<
The above is a bit of Ruby which shows the type of multiple roundtrips we talked about before.
>

Naturalmente, le liste non sono adatte solo a memorizzare relazioni ad altre chiavi. I valori possono essere qualsiasi cosa. Si possono usare per salvare logs o tenere traccia del percorso di un utente attraverso un sito. Se si stesse scrivendo un gioco, si potrebbero usare per tracciare una sequenza di azioni.

<
Of course, lists aren't only good for storing references to other keys. The values can be anything. You could use lists to store logs or track the path a user is taking through a site. If you were building a game, you might use it to track a queued user actions.
>


### Insiemi

Gli insiemi Redis (`set`) si possono usare per memorizzare valori univoci. Forniscono inoltre svariate operazioni di tipo insiemistico, ad esempio le unioni. Gli insiemi non sono ordinati ma forniscono nondimeno operazioni sui valori molto efficienti. Una lista di amici è il classico esempio di uso degli insiemi:

	sadd friends:leto ghanima paul chani jessica
	sadd friends:duncan paul jessica alia



<
Set are used to store unique values and provide a number of set-based operations, like unions. Sets aren't ordered but they provide efficient value-based operations. A friend's list is the classic example of using a set:
>

Indipendentemente da quanti amici ciascuno abbia, è possibile in maniera efficiente (O(1)) dire se userX sia amico di userY o meno:

	sismember friends:leto jessica
	sismember friends:leto vladimir


<
Regardless of how many friends a user has, we can efficiently tell (O(1)) whether userX is a friend of userY or not
>

Inoltre è possibile dire se due o più persone condividano le stesse amicizieç:

	sinter friends:leto friends:duncan



<
Furthermore we can see what two or more people share the same friends:
>

e anche salvare il risultato in una nuova chiave:

	sinterstore friends:leto_duncan friends:leto friends:duncan

<
and even store the result at a new key:
>

-- duplicazione
 
Gli insiemi sono perfetti per tenere traccia di proprietà di un oggetto per cui duplicati non abbiano senso o anche per memorizzare oggetti su cui si voglia poter applicare operazioni come intersezione e unione.

<
Sets are great for tagging or tracking any other properties of a value for which duplicates don't make any sense (or where we want to apply set operations such as intersections and unions).
>


### Insiemi ordinati

L'ultima e più potente struttura dati sono gli insiemi ordinati (`sorted set`). Se gli hash sono come le stringhe ma con campi, gli insiemi ordinati sono come gli insiemi ma con una graduatoria. La graduatoria fornisce un criterio di ordinamento e classifica. Se si volesse una lista di amicizie con graduatoria si potrebbe scrivere: 

	zadd friends:leto 100 ghanima 95 paul 95 chani 75 jessica 1 vladimir

<
The last and most powerful data structure are sorted sets. If hashes are like strings but with fields, then sorted sets are like sets but with a score. The score provides sorting and ranking capabilities. If we wanted a ranked list of friends, we might do:
>

Si desisera sapere quanti amici possiede `leto` con rank maggiore uguale a 90?

	zcount friends:leto 90 100
<
Want to find out how many friends `leto` has with a rank of 90 or over?
>

E il posto in classifica di `chani`

	zrevrank friends:leto chani

<
How about figuring out `chani`'s rank?
>

Si noti l'uso di `zrevrank` al posto di `zrank` in quanto l'ordinamento di default di Redis è crescente e in questo caso la graduatoria va chiaramente dall'alto al basso. Si noti l'immediatezza di costruire un `leaderboard system` (una sorta di classifica in tempo realde tra utenti) usando gli insiemi ordinati. In effetti però, data l'estrema genericità della struttura, ogni entità che si possa ordinare tramite un qualche intero, e su cui si vogliano applicare in maniera efficiente delle operazioni basate su tale graduatoria, si presta a essere salvata da un insieme ordinato Redis.

<
We use `zrevrank` instead of `zrank` since Redis' default sort is from low to high (but in this case we are ranking from high to low). The most obvious use-case for sorted sets is a leaderboard system. In reality though, anything you want sorted by an some integer, and be able to efficiently manipulate based on that score, might be a good fit for a sorted set. 
>

Nel prossimo capitolo vedremo come gli insiemi ordinati possano essere usati per tenere traccia di eventi temporali (il tempo diventa in questo caso una sorta di punteggio).

<
In the next chapter we'll look at how sorted sets can be used for tracking events which are time-based (where time is the score); which is another common use-case.
>

### In Questo Capitolo

Si è fatta una panoramica ad alto livello delle cinque strutture dati in Redis. Da questa panoramica, il lettore potrà cominciare a pensare a come estendere l'uso di tali strutture oltre i casi fondamentali. In effetti, ci saranno sicuramente modi di usare le stringhe o gli insiemi ordinati che non sono stati ancora pensati. Ad ogni modo, si tenga presente che, anche se Redis espone cinque strutture, non sono infrequenti i casi in cui per costruire una `feature` se ne usa solo un numero ridotto.

<
una volta compresi i casi d'uso fondamentali delle varie strutture, Redis potrà diventare una soluzione ideale anche per tutti gli altri tipi di problemi. Inoltre,
>

<
That's a high level overview of Redis' five data structures. One of the neat things about Redis is that you can often do more than you first realize. There are probably ways to use string and sorted sets that no one has thought of yet. As long as you understand the normal use-case though, you'll find Redis ideal for all types of problems. Also, just because Redis exposes five data structures and various methods, don't think you need to use all of them. It isn't uncommon to build a feature while only using a handful of commands.
>


\clearpage

``````````````

## Capitolo 3 -  Chapter 3 - Sfruttare al Massimo le Strutture Dati 

Nel precedente capitolo abbiamo trattato le cinque strutture dato e fornito qualche esempio di problemi che possono risolvere. Si può procedere ora ad argomenti e `design pattern` maggiormente avanzati, ancorché comuni.

<
In the previous chapter we talked about the five data structures and gave some examples of what problems they might solve. Now it's time to look at a few more advanced, yet common, topics and design patterns.
>


### La Notazione O-Grande

Già in precedenza ci si è riferiti alla notazione O-grande in particolare a O(n) e a O(1). Tale notazione, detta anche limite asintotico superiore, è usata per spiegare come un algoritmo o un'operazione si comporti, dato un certo numero di elementi su cui agisca. Nella documentazione di Redis, assieme alla descrizione di ogni comando, viene usata tale notazione per esprimere con chiarezza quanto veloce sia il comando al variare della numerosità dell'input o alternativamente degli elementi già contenuti nella struttura. 

<
è usata per esprimere quanto veloce sia un comando in base al numero di elementi che stiamo trattando.

Throughout this book we've made references to the Big O notation in the form of O(N) or O(1). Big O notation is used to explain how something behaves given a certain number of elements. In Redis, it's used to tell us how fast a command is based on the number of items we are dealing with.
>


<
Redis documentation tells us the Big O notation for each of its commands. It also tells us what the factors are that influence the performance. Let's look at some examples.
>

I comandi più veloci sono quelli che hanno tempo costante o O(1). Cioè sia che stiamo trattando 5 elementi o 5 milioni di elementi, avremo le stesse performance. Per esempio il comando `sismember`, che dice se un valore appartiene ad un dato insieme, è O(1). Parecchi comandi Redis sono O(1).

<
The fastest anything can be is O(1) which is a constant. Whether we are dealing with 5 items or 5 million, you'll get the same performance. The `sismember` command, which tells us if a value belongs to a set, is O(1). `sismember` is a powerful command, and its performance characteristics are a big reason for that. A number of Redis commands are O(1).
>

La successiva fascia di performance è quella logaritmica, o O(log(N)). Deriva chiaramente da un approccio `divide et impera`, per il quale vengono considerate via via porzioni sempre più piccole dell'input. In questo modo anche quantità molto grandi vengono ridotte di molto dopo poche iterazioni. Il comando, per esempio, `zadd` possiede complessità O(log(N)), dove N è il numero degli elementi già contenuti nell'insieme.

<
Logarithmic, or O(log(N)), is the next fastest possibility because it needs to scan through smaller and smaller partitions. Using this type of divide and conquer approach, a very large number of items quickly gets broken down in a few iterations. `zadd` is a O(log(N)) command, where N is the number of elements already in the set.
>

In una fascia più alta ancora vi è la complessità lineare o O(N). Un'interrogazione su una colonna non indicizzata in una tabella è un'operazione O(N).

Next we have linear commands, or O(N). Looking for a non-indexed row in a table is an O(N) operation. So is using the `ltrim` command. However, in the case of `ltrim`, N isn't the number of elements in the list, but rather the elements being removed. Using `ltrim` to remove 1 item from a list of millions will be faster than using `ltrim` to remove 10 items from a list of thousands. (Though they'll probably both be so fast that you wouldn't be able to time it.)

`zremrangebyscore` which removes elements from a sorted set with a score between a minimum and a maximum value has a complexity of O(log(N)+M). This makes it a mix. By reading the documentation we see that N is the number of total elements in the set and M is the number of elements to be removed. In other words, the number of elements that'll get removed is probably going to be more significant, in terms of performance, than the total number of elements in the list.

The `sort` command, which we'll discuss in greater detail in the next chapter has a complexity of O(N+M*log(M)). From its performance characteristic, you can probably tell that this is one of Redis' most complex commands.

There are a number of other complexities, the two remaining common ones are O(N^2) and O(C^N). The larger N is, the worse these perform relative to a smaller N. None of Redis' commands have this type of complexity.

It's worth pointing out that the Big O notation deals with the worst case. When we say that something takes O(N), we might actually find it right away or it might be the last possible element.


### Pseudo Multi Key Queries

A common situation you'll run into is wanting to query the same value by different keys. For example, you might want to get a user by email (for when they first log in) and also by id (after they've logged in). One horrible solution is to duplicate your user object into two string values:

	set users:leto@dune.gov "{id: 9001, email: 'leto@dune.gov', ...}"
	set users:9001 "{id: 9001, email: 'leto@dune.gov', ...}"

This is bad because it's a nightmare to manage and it takes twice the amount of memory.

It would be nice if Redis let you link one key to another, but it doesn't (and it probably never will). A major driver in Redis' development is to keep the code and API clean and simple. The internal implementation of linking keys (there's a lot we can do with keys that we haven't talked about yet) isn't worth it when you consider that Redis already provides a solution: hashes.

Using a hash, we can remove the need for duplication:
	
	set users:9001 "{id: 9001, email: leto@dune.gov, ...}"
	hset users:lookup:email leto@dune.gov 9001

What we are doing is using the field as a pseudo secondary index and referencing the single user object. To get a user by id, we issue a normal `get`:

	get users:9001

To get a user by email, we issue an `hget` followed by a `get` (in Ruby):

	id = redis.hget('users:lookup:email', 'leto@dune.gov')
	user = redis.get("users:#{id}")

This is something that you'll likely end up doing often. To me, this is where hashes really shine, but it isn't an obvious use-case until you see it.

### References and Indexes

We've seen a couple examples of having one value reference another. We saw it when we looked at our list example, and we saw it in the section above when using hashes to make querying a little easier. What this comes down to is essentially having to manually manage your indexes and references between values. Being honest, I think we can say that's a bit of a downer, especially when you consider having to manage/update/delete these references manually. There is no magic solution to solving this problem in Redis. 

We already saw how sets are often used to implement this type of manual index:

	sadd friends:leto ghanima paul chani jessica

Each member of this set is a reference to a Redis string value containing details on the actual user. What if `chani` changes her name, or deletes her account? Maybe it would make sense to also track the inverse relationships:

	sadd friends_of:chani leto paul

Maintenance cost aside, if you are anything like me, you might cringe at the processing and memory cost of having these extra indexed values. In the next section we'll talk about ways to reduce the performance cost of having to do extra round trips (we briefly talked about it in the first chapter). 

If you actually think about it though, relational databases have the same overhead. Indexes take memory, must be scanned or ideally seeked and then the corresponding records must be looked up. The overhead is neatly abstracted away (and they  do a lot of optimizations in terms of the processing to make it very efficient). 

Again, having to manually deal with references in Redis in unfortunate. But any initial concerns you have about the performance or memory implications of this should be tested. I think you'll find it a non-issue.

### Round Trips and Pipelining 

We already mentioned that making frequent trips to the server is a common pattern in Redis. Since it is something you'll do often, it's worth taking a closer look at what features we can leverage to get the most out of it.

First, many commands either accept one or more set of parameters or have a sister-command which takes multiple parameters. We saw `mget` earlier, which takes multiple keys and returns the values: 

	keys = redis.lrange('newusers', 0, 10)
	redis.mget(*keys.map {|u| "users:#{u}"})

Or the `sadd` command which adds 1 or more members to a set:

	sadd friends:vladimir piter
	sadd friends:paul jessica leto "leto II" chani

Redis also supports pipelining. Normally when a client sends a request to Redis it waits for the reply before sending the next request. With pipelining you can send a number of requests without waiting for their responses. This reduces the networking overhead and can result in significant performance gains. 

It's worth noting that Redis will use memory to queue up the commands, so it's a good idea to batch them. How large a batch you use will depend on what commands you are using, and more specifically, how large the parameters are. But, if you are issuing commands against ~50 character keys, you can probably batch them in thousands or tens of thousands.

Exactly how you execute commands within a pipeline will vary from driver to driver. In Ruby you pass a block to the `pipelined` method:

	redis.pipelined do
	  9001.times do
		redis.incr('powerlevel')
	  end
	end

As you can probably guess, pipelining can really speed up a batch import!

### Transactions

Every Redis command is atomic, including the ones that do multiple things. Additionally, Redis has support for transactions when using multiple commands.

You might not know it, but Redis is actually single-threaded, which is how every command is guaranteed to be atomic. While one command is executing, no other command will run. (We'll briefly talk about scaling in a later chapter.) This is particularly useful when you consider that some commands do multiple things. For example:

`incr` is essentially a `get` followed by a `set`

`getset` sets a new value and returns the original

`setnx` first checks if the key exists, and only sets the value if it does not

Although these commands are useful, you'll inevitably need to run multiple commands as an atomic group. You do so by first issuing the `multi` command, followed by all the commands you want to execute as part of the transaction, and finally executing `exec` to actually execute the commands or `discard` to throw away, and not execute the commands. What guarantee does Redis make about transactions? 

* The commands will be executed in order

* The commands will be executed as a single atomic operation (without another client's command being executed halfway through)

* That either all or none of the commands in the transaction will be executed

You can, and should, test this in the command line interface. Also note that there's no reason why you can't combine pipelining and transactions.

	multi
	hincrby groups:1percent balance -9000000000
	hincrby groups:99percent balance 9000000000
	exec

Finally, Redis lets you specify a key (or keys) to watch and conditionally apply a transaction if the key(s) changed. This is used when you need to get values and execute code based on those values, all in a transaction. With the code above, we wouldn't be able to implement our own `incr` command since they are all executed together once `exec` is called. From code, we can't do:

	redis.multi()
	current = redis.get('powerlevel')
	redis.set('powerlevel', current + 1)
	redis.exec()

That isn't how Redis transactions work. But, if we add a `watch` to `powerlevel`, we can do:

	redis.watch('powerlevel')
	current = redis.get('powerlevel')
	redis.multi()
	redis.set('powerlevel', current + 1)
	redis.exec()

If another client changes the value of `powerlevel` after we've called `watch` on it, our transaction will fail. If no client changes the value, the set will work. We can execute this code in a loop until it works.

### Time Values

A slightly less common pattern that I'm fond of is using sorted sets to track time value. In Redis' documentation the sorting value is called a *score*, which might limit some people's imagination of different ways they can put it to use. 

Let's say we want to track the stock prices for a symbol. Our key would be the symbol, our *score* would be the timestamp and our value the price:

	redis.zadd('GOOG', Time.now.utc.to_i-100, 625.03)
	redis.zadd('GOOG', Time.now.utc.to_i-95, 623.01)
	redis.zadd('GOOG', Time.now.utc.to_i-95, 625.02)
	redis.zadd('GOOG', Time.now.utc.to_i-92, 624.98)

By using `zrangebyscore` we can get a range of values by a score. In our case that means getting values for a specific date range. If we wanted to get the last 5 seconds worth of prices, we could do:

	redis.zrangebyscore('GOOG', (Time.now.utc - 5).to_i, Time.now.utc.to_i)


### Keys Anti-Pattern

In the next chapter we'll talk about commands that aren't specifically related to data structures. Some of these are administrative or debugging tools. But there's one I'd like to talk about in particular: the `keys` command. This command takes a pattern and finds all the matching keys. This command seems like it's well suited for a number of tasks, but it should never be used in production code. Why? Because it does a linear scan through all the keys looking for matches. Or, put simply, it's slow.

How do people try and use it? Say you are building a hosted bug tracking service. Each account will have an `id` and you might decide to store each bug into a string value with a key that looks like `bug:account_id:bug_id`. If you ever need to find all of an account's bugs (to display them, or maybe delete them if they delete their account), you might be tempted (as I was!) to use the `keys` command:

	keys bug:1233:*

The better solution is to use a hash. Much like we can use hashes to provide a way to expose secondary indexes, so too can we use them to organize our data:

	hset bugs:1233 1 "{id:1, account: 1233, subject: '...'}"
	hset bugs:1233 2 "{id:2, account: 1233, subject: '...'}"

To get all the bug ids for an account we simply call `hkeys bugs:1233`. To delete a specific bug we can do `hdel bugs:1233 2` and to delete an account we can delete the key via `del bugs:1233`.


### In This Chapter	

This chapter, combined with the previous one, has hopefully given you some insight on how to use Redis to power real features. There are a number of other patterns you can use to build all types of things, but the real key is to understand the fundamental data structures and to get a sense for how they can be used to achieve things beyond your initial perspective.

\clearpage

## Chapter 4 - Beyond The Data Structures

While the five data structures form the foundation of Redis, there are other commands which aren't data structure specific. We've already seen a handful of these: `info`, `select`, `flushdb`, `multi`, `exec`, `discard`, `watch` and `keys`. This chapter will look at some of the other important ones.

### Expiration

Redis allows you to mark a key for expiration. You can give it an absolute time in the form of a Unix timestamp (seconds since January 1, 1970) or a time to live in seconds. This is a key-based command, so it doesn't matter what type of data structure the key represents. 

	expire pages:about 30
	expireat pages:about 1356933600

The first command will delete the key (and associated value) after 30 seconds. The second will do the same at 12:00 a.m. December 31st, 2012.

This makes Redis an ideal caching engine. You can find out how long an item has to live until via the `ttl` command and you can remove the expiration on a key via the `persist` command:

	ttl pages:about
	persist pages:about

Finally, there's a special string command, `setex` which lets you set a string and specify a time to live in a single atomic command (this is more for convenience than anything else):

	setex pages:about 30 '<h1>about us</h1>....'

### Publication and Subscriptions

Redis lists have an `blpop` and `brpop` command which returns and removes the first (or last) element from the list or blocks until one is available. These can be used to power a simple queue.

Beyond this, Redis has first-class support for publishing messages and subscribing to channels. You can try this out yourself by opening a second `redis-cli` window. In the first window subscribe to a channel (we'll call it `warnings`):

	subscribe warnings

The reply is the information of your subscription. Now, in the other window, publish a message to the `warnings` channel:

	publish warnings "it's over 9000!"

If you go back to your first window you should have received the message to the `warnings` channel.

You can subscribe to multiple channels (`subscribe channel1 channel2 ...`), subscribe to a pattern of channels (`psubscribe warnings:*`) and use the `unsubscribe` and `punsubscribe` commands to stop listening to one or more channels, or a channel pattern.

Finally, notice that the `publish` command returned the value 1. This indicates the number of clients that received the message.


### Monitor and Slow Log

The `monitor` command lets you see what Redis is up to. It's a great debugging tool that gives you insight into how your application is interacting with Redis. In one of your two redis-cli windows (if one is still subscribed, you can either use the `unsubscribe` command or close the window down and re-open a new one) enter the `monitor` command. In the other, execute any other type of command (like `get` or `set`). You should see those commands, along with their parameters, in the first window.

You should be wary of running monitor in production, it really is a debugging and development tool. Aside from that, there isn't much more to say about it. It's just a really useful tool.

Along with `monitor`, Redis has a `slowlog` which acts as a great profiling tool. It logs any command which takes longer than a specified number of **micro**seconds. In the next section we'll briefly cover how to configure Redis, for now you can configure Redis to log all commands by entering:

	config set slowlog-log-slower-than 0

Next, issue a few commands. Finally you can retrieve all of the logs, or the most recent logs via:

	slowlog get
	slowlog get 10

You can also get the number of items in the slow log by entering `slowlog len`

For each command you entered you should see four parameters:

* An auto-incrementing id

* A Unix timestamp for when the command happened

* The time, in microseconds, it took to run the command

* The command and its parameters

The slow log is maintained in memory, so running it in production, even with a low threshold, shouldn't be a problem. By default it will track the last 1024 logs. 

### Sort

One of Redis' most powerful commands is `sort`. It lets you sort the values within a list, set or sorted set (sorted sets are ordered by score, not the members within the set). In its simplest form, it allows us to do:

	rpush users:leto:guesses 5 9 10 2 4 10 19 2
	sort users:leto:guesses

Which will return the values sorted from lowest to highest. Here's a more advanced example:

	sadd friends:ghanima leto paul chani jessica alia duncan
	sort friends:ghanima limit 0 3 desc alpha

The above command shows us how to page the sorted records (via `limit`), how to return the results in descending order (via `desc`) and how to sort lexicographically instead of numerically (via `alpha`).

The real power of `sort` is its ability to sort based on a referenced object. Earlier we showed how lists, sets and sorted sets are often used to reference other Redis objects. The `sort` command can dereference those relations and sort by the underlying value. For example, say we have a bug tracker which lets users watch issues. We might use a set to track the issues being watched:

	sadd watch:leto 12339 1382 338 9338

It might make perfect sense to sort these by id (which the default sort will do), but we'd also like to have these sorted by severity. To do so, we tell Redis what pattern to sort by. First, let's add some more data so we can actually see a meaningful result:

	set severity:12339 3
	set severity:1382 2
	set severity:338 5
	set severity:9338 4

To sort the bugs by severity, from highest to lowest, you'd do:

	sort watch:leto by severity:* desc

Redis will substitute the `*` in our pattern (identified via `by`) with the values in our list/set/sorted set. This will create the key name that Redis will query for the actual values to sort by.

Although you can have millions of keys within Redis, I think the above can get a little messy. Thankfully sort can also work on hashes and their fields. Instead of having a bunch of top-level keys you can leverage hashes:

	hset bug:12339 severity 3
	hset bug:12339 priority 1
	hset bug:12339 details "{id: 12339, ....}"

	hset bug:1382 severity 2
	hset bug:1382 priority 2
	hset bug:1382 details "{id: 1382, ....}"

	hset bug:338 severity 5
	hset bug:338 priority 3
	hset bug:338 details "{id: 338, ....}"

	hset bug:9338 severity 4
	hset bug:9338 priority 2
	hset bug:9338 details "{id: 9338, ....}"

Not only is everything better organized, and we can sort by `severity` or `priority`, but we can also tell `sort` what field to retrieve:

	sort watch:leto by bug:*->priority get bug:*->details

The same value substitution occurs, but Redis also recognizes the `->` sequence and uses it to look into the specified field of our hash. We've also included the `get` parameter, which also does the substitution and field lookup, to retrieve bug details.

Over large sets, `sort` can be slow. The good news is that the output of a `sort` can be stored:

	sort watch:leto by bug:*->priority get bug:*->details store watch_by_priority:leto

Combining the `store` capabilities of `sort` with the expiration commands we've already seen makes for a nice combo.


### In This Chapter 

This chapter focused on non-data structure-specific commands. Like everything else, their use is situational. It isn't uncommon to build an app or feature that won't make use of expiration, publication/subscription and/or sorting. But it's good to know that they are there. Also, we only touched on some of the commands. There are more, and once you've digested the material in this book it's worth going through the [full list](http://redis.io/commands).

\clearpage

## Chapter 5 - Administration

Our last chapter is dedicated to some of the administrative aspects of running Redis. In no way is this a comprehensive guide on Redis administration. At best we'll answer some of the more basic questions new users to Redis are most likely to have.

### Configuration

When you first launched the Redis server, it warned you that the `redis.conf` file could not be found. This file can be used to configure various aspects of Redis. A well documented `redis.conf` file is available for each release of Redis. The sample file contains the default configuration options, so it's useful to both understand what the settings do and what their defaults are. You can find it at <https://github.com/antirez/redis/raw/2.4.6/redis.conf>.

**This is the config file for Redis 2.4.6. You should replace "2.4.6" in the above URL with your version. You can find your version by running the `info` command and looking at the first value.**

Since the file is well-documented, we won't be going over the settings.

In addition to configuring Redis via the `redis.conf` file, the `config set` command can be used to set individual values. In fact, we already used it when setting the `slowlog-log-slower-than` setting to 0. 

There's also the `config get` command which displays the value of a setting. This command supports pattern matching. So if we want to display everything related to logging, we can do:

	config get *log*

### Authentication

Redis can be configured to require a password. This is done via the `requirepass` setting (set through either the `redis.conf` file or the `config set` command). When `requirepass` is set to a value (which is the password to use), clients will need to issue an `auth password` command.

Once a client is authenticated, they can issue any command against any database. This includes the `flushall` command which erases every key from every database. Through the configuration, you can rename commands to achieve some security through obfuscation:

	rename-command CONFIG 5ec4db169f9d4dddacbfb0c26ea7e5ef	
	rename-command FLUSHALL 1041285018a942a4922cbf76623b741e

Or you can disable a command by setting the new name to an empty string.

### Size Limitations

As you start using Redis, you might wonder "how many keys can I have?". You might also wonder how many fields can a hash have (especially when you use it to organize your data), or how many elements can lists and sets have? Per instance, the practical limits for all of these is in the hundreds of millions.


### Replication

Redis supports replication, which means that as you write to one Redis instance (the master), one or more other instances (the slaves) are kept up-to-date by the master. To configure a slave you use either the `slaveof` configuration setting or the `slaveof` command (instances running without this configuration are or can be masters).

Replication helps protect your data by copying to different servers. Replication can also be used to improve performance since reads can be sent to slaves. They might respond with slightly out of date data, but for most apps that's a worthwhile tradeoff.

Unfortunately, Redis replication doesn't yet provide automated failover. If the master dies, a slave needs to be manually promoted. Traditional high-availability tools that use heartbeat monitoring and scripts to automate the switch are currently a necessary headache if you want to achieve some sort of high availability with Redis.

### Backups

Backing up Redis is simply a matter of copying Redis' snapshot to whatever location you want (S3, FTP, ...). By default Redis saves its snapshop to a file named `dump.rdb`. At any point in time, you can simply `scp`, `ftp` or `cp` (or anything else) this file.

It isn't uncommon to disable both snapshotting and the append-only file (aof) on the master and let a slave take care of this. This helps reduce the load on the master and lets you set more aggressive saving parameters on the slave without hurting overall system responsiveness.

### Scaling and Redis Cluster

Replication is the first tool a growing site can leverage. Some commands are more expensive than others (`sort` for example) and offloading their execution to a slave can keep the overall system responsive to incoming queries.

Beyond this, truly scaling Redis comes down to distributing your keys across multiple Redis instances (which could be running on the same box, remember, Redis is single-threaded). For the time being, this is something you'll need to take care of (although a number of Redis drivers do provide consistent-hashing algorithms). Thinking about your data in terms of horizontal distribution isn't something we can cover in this book. It's also something you probably won't have to worry about for a while, but it's something you'll need to be aware of regardless of what solution you use.

The good news is that work is under way on Redis Cluster. Not only will this offer horizontal scaling, including rebalancing, but it'll also provide automated failover for high availability. 

High availability and scaling is something that can be achieved today, as long as you are willing to put the time and effort into it. Moving forward, Redis Cluster should make things much easier.

### In This Chapter

Given the number of projects and sites using Redis already, there can be no doubt that Redis is production-ready, and has been for a while. However, some of the tooling, especially around security and availability is still young. Redis Cluster, which we'll hopefully see soon, should help address some of the current management challenges.

\clearpage

## Conclusion

In a lot of ways, Redis represents a simplification in the way we deal with data. It peels away much of the complexity and abstraction available in other systems. In many cases this makes Redis the wrong choice. In others it can feel like Redis was custom-built for your data.

Ultimately it comes back to something I said at the very start: Redis is easy to learn. There are many new technologies and it can be hard to figure out what's worth investing time into learning. When you consider the real benefits Redis has to offer with its simplicity, I sincerely believe that it's one of the best investments, in terms of learning, that you and your team can make.