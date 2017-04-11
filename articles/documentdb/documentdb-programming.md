<properties 
    pageTitle="Programiranje DocumentDB: pohranjene procedure, okidača baze podataka i korisnički definiranih funkcija | Microsoft Azure" 
    description="Saznajte kako koristiti DocumentDB za pisanje pohranjene procedure, okidača baze podataka i korisnički definirane funkcije (UDF) u JavaScript. Pronađite savjete programing baze podataka i još mnogo toga." 
    keywords="Okidača baza podataka pohranjena procedura, pohranjena procedura, programa za baze podataka, sproc, documentdb, azure, Microsoft azure"
    services="documentdb" 
    documentationCenter="" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="andrl"/>

# <a name="documentdb-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>Programiranje poslužiteljsko DocumentDB: pohranjene procedure, okidača baze podataka i korisnički definiranih funkcija

Saznajte kako integrirati Azure DocumentDB jezik, transakcijskih izvršavanje JavaScripta omogućuje razvojnim inženjerima pisanje **pohranjene procedure**, **okidača** i **korisnički definirane funkcije (UDF)** nativno u JavaScript. To vam omogućuje da pisanje logike aplikacije program baze podataka koji se može otpremljene i izvršava izravno na particije prostora za pohranu baze podataka 

Preporučujemo da prvi koraci sljedeće videozapisu gdje Liu promjena daje kratak Uvod u model programiranja za DocumentDB na strani poslužitelja baze podataka. 

> [AZURE.VIDEO azure-demo-a-quick-intro-to-azure-documentdbs-server-side-javascript]

Zatim se vratiti u ovom se članku gdje ćete Saznajte odgovore na sljedeća pitanja:  

- Kako napisati je pohranjena procedura, okidača ili UDF pomoću JavaScript?
- Kako DocumentDB jamči ACID?
- Koliko je transakcija funkcioniraju u DocumentDB?
- Što su unaprijed pokreće i naknadno pokreće i Kako napisati nešto?
- Kako registrirati i izvršavanje pohranjena procedura, okidača ili UDF RESTful način putem HTTP-a?
- Što DocumentDB SDK-ovi dostupni su za stvaranje i izvršavanje pohranjene procedure, okidača i UDF-ove?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Uvod u pohranjene Procedure i UDF programiranje

Taj se način *"JavaScript kao Moderna dan T SQL"* oslobađa razvojnim inženjerima iz complexities vrsta sustava nepodudarnosti i tehnologije objekt relacijski mapiranje. Ima brojne intrinzična prednosti koje možete iskoristiti za izgradnju obogaćene aplikacije:  

-   **Proceduralne logika:** JavaScript kao visoku razinu programskom jeziku, omogućuje bogata i poznato sučelje da biste izrazili poslovne logike. Na raspolaganju vam složene nizove bliže operacija s podacima.

-   **Atomske transakcije:** DocumentDB jamčiti da izvršiti postupaka baze podataka u jednom pohranjena procedura ili okidača su atomske. Omogućuje aplikacije kombinirali povezane operacije u jednu grupu tako da ih sve uspjeti ili ništa nije uspjela. 

-   **Performanse:** Fact JSON intrinsically mapirani sustavu vrsta jezika Javascript, a ujedno osnovna jedinica za pohranu u DocumentDB omogućuje za broj optimizacije kao što su drži materialization JSON dokumenata u skupinu međuspremnika i da ih dostupna na zahtjev za izvršni kod. Dva su performanse pogodnosti pridružene dostave poslovne logike u bazu podataka:
    -   Grupno slanje promjena – razvojnim inženjerima možete grupirati operacija kao što su umeće i pošaljite ih masovno. Na promet latenciju mreže cijena i indirektni pohrane da biste stvorili zasebnom transakcije su znatno smanjiti. 
    -   Prije sastavljanja – DocumentDB precompiles pohranjene procedure, okidača i korisnički definirane funkcije (UDF) da biste izbjegli JavaScript sastavljanja trošak za svaki poziva. Indirektni izgradnje šifru bajt proceduralne logike je amortized minimalnu vrijednost.
    -   Određivanje redoslijeda – mnogo operacije treba li vam strani-efekta ("okidača") koji je potencijalno uključuje način jedan ili više sekundarne maloprodajno. Osim atomicity, to je više performant kada Premjesti na poslužitelj. 
-   **Encapsulation:** Pohranjene procedure može se koristiti da biste grupirali poslovne logike na jednom mjestu. To ima dvije prednosti:
    -   Dodaje se sloj apstrakcije pri vrhu sirovim podacima, čime se omogućuje projektantima podataka da biste poslovanja svojim aplikacijama nezavisno od podataka. Ovo je osobito zbrajate kad podaci sheme – manje, zbog brittle pretpostavke može biti potrebno pečeni u aplikaciju ako imaju radnju izravno s podacima.  
    -   U ovom apstrakcije omogućuje korporacije zaštiti svoje podatke tako da bi access iz skripte.  

Stvaranje i izvršavanje okidača baze podataka, pohranjena procedura i operatora prilagođene upita podržan je kroz [REST API -JA](https://msdn.microsoft.com/library/azure/dn781481.aspx), [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)i [klijent SDK-ovi](documentdb-sdk-dotnet.md) mnogo platforme uključujući .NET, Node.js i JavaScript.

**Pomoću ovog praktičnog vodiča koristi [SDK Node.js s Promises pitanja](http://azure.github.io/azure-documentdb-node-q/) ** da biste ilustrirali sintaksa i korištenje funkcije pohranjene procedure, okidača i UDF-ove.   

## <a name="stored-procedures"></a>Pohranjene procedure

### <a name="example-write-a-simple-stored-procedure"></a>Primjer: Pisanje jednostavne pohranjena procedura 
Započnimo s jednostavne pohranjena procedura koja vraća "Pozdrav svijeta" odgovor.

    var helloWorldStoredProc = {
        id: "helloWorld",
        body: function () {
            var context = getContext();
            var response = context.getResponse();
    
            response.setBody("Hello, World");
        }
    }


Pohranjene procedure registrirane po zbirci pa možete raditi na bilo koji dokument i privitak za tu zbirku. Sljedeći isječak prikazuje kako registrirati helloWorld pohranjene procedure s zbirku. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Kada je pohranjena procedura registrirana, ne možemo možete izvršiti protiv zbirke, i čitati rezultate natrag na klijentskom računalu. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


Kontekstni objekt omogućuje pristup sve operacije može izvoditi na DocumentDB prostora za pohranu i pristup objektima zahtjeva i odgovora. U ovom slučaju koristi objekt odgovora da biste postavili tijelo odgovora koja je poslana natrag na klijentu. Dodatne pojedinosti potražite [DocumentDB JavaScript server SDK dokumentaciji](http://azure.github.io/azure-documentdb-js-server/).  

Javite nam proširite u ovom primjeru i dodati više povezanih funkcije pohranjena procedura baze podataka. Pohranjene procedure možete stvoriti, ažurirati, čitanje, upita i brisanje dokumenata i privitke unutar zbirke.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Primjer: Pisanje pohranjena procedura za stvaranje dokumenta 
Sljedeći isječak pokazuje kako koristiti Kontekstni objekt za interakciju s DocumentDB resursa.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        body: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


U ovom pohranjena procedura uzima kao unos documentToCreate, u tijelu dokumenta će biti stvoren u trenutnoj zbirci. Sve operacije su asinkronog i ovise o JavaScript funkcija pozive. Funkcija povratnog ima dva parametra, jedan za objekt pogreške u slučaju operacija ne uspije i jedan za stvoreni objekt. Unutar povratnog, korisnici možete rukovati iznimku ili vratiti pogrešku. U slučaju da je povratnog nije naveden, a zatim je došlo do pogreške, izvođenja DocumentDB throws pogrešku.   

U gornjem primjeru povratnog throws pogrešku ako operacija nije uspjela. U suprotnom postavlja id stvorenog dokumenta kao tijela odgovor na klijentu. Evo kako se izvršava postupak spremljene s ulaznih parametara.

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
    
            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };
    
            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });

    
Imajte na umu ovo pohranjena procedura moguće je izmijeniti da biste snimili niz tijela dokumenta kao ulaz i ih stvorite u iste izvršenje pohranjena procedura umjesto više mreža zahtjevi za svaku od njih pojedinačno stvaranje. To se može koristiti za implementaciju programa uvoz učinkovitog skupno za DocumentDB (opisan u nastavku ovog praktičnog vodiča).   

Primjer opisano što je prikazano kako koristiti pohranjene procedure. Ne možemo obrađuje okidača i korisnički definirane funkcije (UDF-ove) kasnije u ovom praktičnom vodiču.

## <a name="database-program-transactions"></a>Program transakcije baze podataka
Transakcije u bazi podataka za uobičajene može se definirati kao niz operacije obavljene kao jedna jedinica logičke rada. Pojedine transakcije nudi **ACID jamčiti**. ACID je dobro poznatog akronim koja označava za četiri svojstva - Atomicity, dosljednost, odvajanja i rok trajanja.  

Ukratko, atomicity jamčiti sve obavljen unutar transakcije se smatra jedna jedinica mjesto ili sve to nastoji ili ništa. Dosljednost jamči da se podaci nalaze uvijek u dobar Interna stanje preko transakcije. Odvajanja jamčiti da nema dvije transakcije ometati međusobno – Općenito, većina komercijalne sustavi sadrže više razine odvajanja koji se mogu koristiti na temelju potreba za aplikaciju. Rok trajanja osigurava da promjene pridaje u bazi podataka uvijek biti prezentacija.   

U DocumentDB, jezika JavaScript nalazi u istom memorijskom prostoru kao baza podataka. Dakle, zahtjevi za koje izvršite u pohranjene procedure i okidača izvršavanje u istom opsega sesije baze podataka. Time se omogućuje DocumentDB da bi ACID za sve operacije koje su dio jedan pohranjene procedure/okidač. Razmotrite sljedeće definiciju pohranjena procedura:

    // JavaScript source code
    var exchangeItemsSproc = {
        name: "exchangeItems",
        body: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            var player1Document, player2Document;
    
            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);
    
                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];
    
                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });
    
            if (!accept) throw "Unable to read player details, abort ";
    
            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;
    
                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";
    
                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });
    
                        if (!accept2) throw "Unable to update player 2, abort";
                    });
    
                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }
    
    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

U ovom pohranjena procedura koristi transakcije unutar igraće aplikacija u trgovini stavke između dva igrača u jednoj operaciji. Pohranjena procedura pokušava pročitajte svaki odgovara reproduktora ID-a kao argument proslijeđen u dokumentima. Ako se pronađe oba dokumenta reproduktora, pohranjena procedura dokumente prilagođava tako zamjena svoje stavke. Ako bilo koji su pogreške usput, throws iznimku JavaScript koje implicitno prekida transakcije.

Ako je registrirana zbirke pohranjena procedura protiv jednom zbirka je, a zatim transakcije implementaciju ograničen je na docuemnts unutar zbirke. Ako je zbirka particije, pohranjene procedure se izvode u opsegu transakcije ključ jedan particije. Svakog pohranjene procedure izvođenja zatim mora sadržavati particija ključa vrijednost koja odgovara opsega transakcije morate pokrenuti u odjeljku. Dodatne informacije potražite u članku [DocumentDB particija](documentdb-partition-data.md).

### <a name="commit-and-rollback"></a>Potvrđivanje i vraćanje
Transakcije Duboko i nativno integrirati u model programiranja za JavaScript DocumentDB korisnika. Unutar funkcije JavaScript, sve operacije se automatski prelomljeni pod jednom transakcijom. Ako JavaScript dovrši bez svaku iznimku, izvršenja su operacije u bazu podataka. Na snazi su naredbe "POČETKA TRANSAKCIJE" i "IZVRŠAVANJE TRANSAKCIJE" u relacijskim bazama podataka implicitno u DocumentDB.  
 
Ako postoji svaku iznimku koja se upisuje iz skriptu, izvođenja JavaScript DocumentDB, vratit će cijeli transakcije. Kao što je prikazano u primjeru u starijim prijavi iznimka jednako učinkovito "VRAĆANJE TRANSAKCIJE" u DocumentDB.
 
### <a name="data-consistency"></a>Dosljednost podataka
Pohranjene procedure i okidača uvijek se izvršava na primarni replike zbirke DocumentDB. Time se osigurava da čitanja iz unutar pohranjene procedure ponudu istaknuti dosljednost. Upita pomoću korisnički definirane funkcije koje će se izvršiti na primarni ili bilo koju sekundarnog replike, ali ne možemo biti sigurni da bi odgovarao razinu tražene dosljednost tako da odaberete odgovarajući replike.

## <a name="bounded-execution"></a>Bounded izvođenja
Sve operacije DocumentDB morate dovršiti u navedenom poslužitelju zahtjev za trajanje vremenskog ograničenja. Ovo ograničenje odnosi i na JavaScript funkcije (pohranjene procedure, okidača i korisnički definirane funkcije). Ako operacije ispunjen te vremensko ograničenje, transakcija je vraćen. JavaScript funkcije morate Završi unutar vremensko ograničenje ili implementirati nastavka temelji model za obradu/životopis izvršavanja.  

Da biste pojednostavnili razvoja pohranjene procedure i okidača za rukovanje vremenska ograničenja, sve u odjeljku zbirka objekta (za stvaranje, čitanje, Zamijeni i brisanje dokumenata i privitaka) vraćaju logička vrijednost koja označava je li će dovršiti postupak. Ako je vrijednost false, je znak da vremensko ograničenje uskoro će isteći i da morate postupak prelamanje se izvršavanje.  Operacije u redu čekanja prije prve operacije neprihvaćene spremište su zajamčiti da biste dovršili ako pohranjena procedura dovršava u vremenu, a red čekanja sve zahtjevi.  

JavaScript funkcije su bounded i na potrošnje resursa. DocumentDB rezervira propusnost po zbirci dodijeljenu veličinom računa baze podataka. Propusnost izraziti u kategorijama normaliziranu jedinica procesora, memorije i potrošnje IO naziva jedinice zahtjev ili RUs. Funkcija JavaScript potencijalno možete iskoristiti velik broj RUs unutar kratko vrijeme i mogla bi se stopa ograničena ako se dosegne ograničenje za zbirku. Resursa intenzivno pohranjene procedure možda također biti poslana u karantenu da biste bili sigurni raspoloživost postupaka jednostavne baze podataka.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Primjer: Skupno uvoz podataka u program za baze podataka
Slijedi primjer pohranjena procedura napisan skupno i uvoz dokumenata u zbirku. Imajte na umu kako pohranjena procedura rukuje bounded izvođenja tako da na Booleove vrijednosti vraća vrijednost iz createDocument, a potom koristi broj dokumenata koji su umetati u svakom poziva pohranjene procedure za praćenje i nastavili tijeku svim grupama.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
    
        // The count of imported docs, also used as current doc index.
        var count = 0;
    
        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");
    
        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }
    
        // Call the create API to create a document.
        tryCreate(docs[count], callback);
    
        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);
    
            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }
    
        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;
    
            // One more document has been inserted, increment the count.
            count++;
    
            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Okidača baze podataka
### <a name="database-pre-triggers"></a>Prije okidača baze podataka
DocumentDB nudi okidača koji izvršava ili operaciju na dokumentu je pokrenuo. Ako, na primjer, možete odrediti stara okidača kada stvarate dokument – ovog okidača stara će pokrenuti stvoriti dokument. Slijedi primjer kako koristiti stara okidača za provjeru valjanosti svojstva dokumenta koji je stvoren je:

    var validateDocumentContentsTrigger = {
        name: "validateDocumentContents",
        body: function validate() {
            var context = getContext();
            var request = context.getRequest();
    
            // document to be created in the current operation
            var documentToCreate = request.getBody();
    
            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }
    
            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


I odgovarajuće Node.js klijentsko Registracija šifru za okidača:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };
    
            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };
    
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Prije okidača ne smije sadržavati ulazne parametre. Zahtjev za objekt može se koristiti za rukovanje zahtjev poruka povezana sa postupak. Ovdje, stara okidača pokrenut s stvaranja dokumenta, a zahtjev za poruke s dokumentom će biti stvoren u JSON OSNOVNI oblik.   

Kada okidača Registrirane, korisnici mogu odrediti operacije koje se može pokrenuti s. Taj se okidač stvorena pomoću TriggerOperation.Create, što znači da sljedeće nije dopušteno.

    var options = { preTriggerInclude: "validateDocumentContents" };
    
    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });
    
    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Nakon okidača baze podataka
Nakon okidača, kao što je stara okidača pridružuju operaciju u dokumentu, a ne želite poduzeti ulazne parametre. Oni pokrenuti **Kada** završi postupak, a imate pristup odgovor poruku koja se šalje klijentu.   

Sljedećim primjerom popravaka okidača u akciji:

    var updateMetadataTrigger = {
        name: "updateMetadata",
        body: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            // document that was created
            var createdDocument = response.getBody();
    
            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";
     
            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';
                         
                         var metadataDocument = documents[0];
                         
                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                           
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


Okidača mogu biti registrirani kao što prikazuje sljedeći primjer.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };
    
            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };
        
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Ovog okidača upite za dokument metapodataka i usklađuje s pojedinostima o novostvorenu dokumenta.  

Jedna od stvari koje je važno je napomenuti da je **transakcijskih** izvršavanje okidača u DocumentDB. Taj se nakon okidač izvodi kao dio iste transakcije kao Stvaranje izvornog dokumenta. Stoga ako smo vratiti iznimku iz nakon okidača (izgovorite ako trenutno nije moguće ažurirati dokument metapodataka), cijeli transakcije neće uspjeti i vraćen. Stvorit će se bez dokumenta pa će se vratiti iznimku.  

##<a id="udf"></a>Korisnički definirane funkcije
Korisnički definirane funkcije (UDF-ove) koriste se za proširivanje gramatike jezika DocumentDB SQL upita i implementacija prilagođene poslovne logike. Ih možete pozvati samo iz unutar upita. Neće imati pristup Kontekstni objekt i namijenjeni su će se koristiti kao samo za računalnim JavaScript. Stoga UDF-ove mogu se izvoditi na sekundarnog replike DocumentDB servisa.  
 
Sljedeći primjer stvara UDF da biste izračunali dobit porez na temelju stope za različite dobit uglatim zagradama i koristi ga u upit da biste pronašli sve osobe koje plaćena više od 20 000 kuna poreza.

    var taxUdf = {
        name: "tax",
        body: function tax(income) {
    
            if(income == undefined) 
                throw 'no input';
    
            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


Na UDF naknadno mogu koristiti u upitima kao što su u sljedeći primjer:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);
    
            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>Upit integriran s jezika JavaScript API-JA
Osim izdavanja upita pomoću DocumentDB na SQL gramatike SDK poslužiteljsko omogućuje izvođenje optimizirana upita pomoću JavaScript sučelje fluent bez sve znanja SQL. Upit JavaScript API omogućuje programski sastavljanje upita prosljeđivanjem predikata funkcije u chainable funkcija poziva s sintaksa koji je poznatom built-ins polja i popularne JavaScript biblioteke kao što su lodash ECMAScript5 korisnika. Upiti su raščlaniti po runtime JavaScript izvršavanje učinkovito korištenje indeksi DocumentDB korisnika.

> [AZURE.NOTE]`__` (dvaput podvlaka) je pseudonim za `getContext().getCollection()`.
> <br/>
> Drugim riječima, možete koristiti `__` ili `getContext().getCollection()` da biste pristupili upita JavaScript API-JA.

Podržane funkcije obuhvaćaju sljedeće:
<ul>
<li>
<b>Chain().... vrijednost ([povratnog] [, mogućnosti])</b>
<ul>
<li>
Počinje value() ulančana poziva koji se prekinuti.
</li>
</ul>
</li>
<li>
<b>Filtar (predicateFunction [, mogućnosti] [, povratni])</b>
<ul>
<li>
Filtrira unos pomoću funkcije predikata koji vraća vrijednost true i false za filtriranje u/izvan unos dokumenata u skupu rezultata. Time se ponaša slično WHERE u SQL.
</li>
</ul>
</li>
<li>
<b>Karta (transformationFunction [, mogućnosti] [, povratni])</b>
<ul>
<li>
Primjenjuje projekcije dali transformaciju funkcija koje karte svake stavke unosa JavaScript objekt ili vrijednost. Time se ponaša slično kao u uvjetu SELECT u SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([propertyName] [, mogućnosti] [, povratni])</b>
<ul>
<li>
Ovo je prečac za mape koji se izdvaja vrijednost od jednog svojstva iz svake stavke unosa.
</li>
</ul>
</li>
<li>
<b>stopi ([isShallow] [, mogućnosti] [, povratni])</b>
<ul>
<li>
Spaja i poravnava polja iz svake unos stavke u jednom polju. Time se ponaša slično SelectMany u LINQ.
</li>
</ul>
</li>
<li>
<b>izbornik sortiranja ([predikata] [, mogućnosti] [, povratni])</b>
<ul>
<li>
Stvaranje novog skupa dokumenata tako da sortiranje na dokumente na ulazni dokument toka uzlaznim redoslijedom pomoću zadanog predikata. Time se ponaša slično uvjet ORDER BY u SQL.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([predikata] [, mogućnosti] [, povratni])</b>
<ul>
<li>
Stvaranje novog skupa dokumenata tako da sortiranje na dokumente na ulazni dokument toka silaznim redoslijedom pomoću zadanog predikata. Time se ponaša slično uvjet ORDER BY x DESC u SQL.
</li>
</ul>
</li>
</ul>


Kad se Uključi u predikata i/ili odabir funkcija sljedećih konstrukta JavaScript se automatski optimizirane pokrenuti izravno DocumentDB indeksi:

* Jednostavan operatora: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Literala, uključujući slovni objekta: {}
* povrat VAR

Sljedećih konstrukta JavaScript dobiti optimiziran za DocumentDB indeksi:

* Kontrolirati tijek (npr. if, dok)
* Funkcija poziva

Dodatne informacije potražite naše [JSDocs na strani poslužitelja](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Primjer: Pisanje pohranjena procedura korištenje upita za JavaScript API-JA

Sljedećim primjerom koda je primjera kako koristiti API upita JavaScript u kontekstu pohranjena procedura. Pohranjena procedura umeće dokumenta, daje ulazni parametar i usklađuje s metapodacima dokument, pomoću na `__.filter()` metoda s minSize, od maxSize i totalSize na temelju svojstvo veličina unos dokumenta.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>SQL list oglasi Javascript
Sljedeća tablica predstavlja različite SQL upite te u odgovarajućem JavaScript.

Kao što je sa SQL upitima dokumenta svojstvo tipke (npr. `doc.id`) se velika i mala slova.

<br/>
<table border="1" width="100%">
<colgroup>
<col span="1" style="width: 40%;">
<col span="1" style="width: 40%;">
<col span="1" style="width: 20%;">
</colgroup>
<tbody>
<tr>
<th>SQL</th>
<th>Upit JavaScript API-JA</th>
<th>Pojedinosti</th>
</tr>
<tr>
<td>
<pre>
Odaberite * iz dokumenata
</pre>
</td>
<td>
<pre>
__.Map(Function(doc) {povrata doc;});
</pre>
</td>
<td>Rezultati u svim dokumentima (paginated token nastavka) kao je.</td>
</tr>
<tr>
<td>
<pre>
Odaberite docs.id, docs.message kao poruka, docs.actions iz dokumenata
</pre>
</td>
<td>
<pre>
__.Map(Function(doc) {povratna {id: doc.id, poruka: doc.message, akcije: doc.actions};});
</pre>
</td>
<td>Projekata id, poruka (sukobima da biste poruka) i akciju s sve dokumente.</td>
</tr>
<tr>
<td>
<pre>
Odaberite * iz dokumenata na mjesto na koje docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.filter(Function(doc) {vratili doc.id === "X998_Y998;"});
</pre>
</td>
<td>Upiti za dokumente s na predikata: id = "X998_Y998".</td>
</tr>
<tr>
<td>
<pre>
Odaberite * iz dokumenata na KOJIMA ARRAY_CONTAINS(docs. Oznake, 123)
</pre>
</td>
<td>
<pre>
__.filter(Function(x) {vratili x.Tags & & x.Tags.indexOf(123) >-; 1});
</pre>
</td>
<td>Upiti za dokumente koji imaju svojstvo oznake i oznake je polja koja sadrži vrijednost 123.</td>
</tr>
<tr>
<td>
<pre>
Odaberite docs.id, docs.message kao poruka šalje dokumente gdje docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.Chain() .filter(function(doc) {vratili doc.id === "X998_Y998;"}) .map(function(doc) {vratili {id: doc.id, poruka: doc.message};}) .value();
</pre>
</td>
<td>Upiti za dokumente s predikata, id = "X998_Y998", a zatim projekata id i poruke (sukobima da biste poruka).</td>
</tr>
<tr>
<td>
<pre>
Oznaka odaberite vrijednost iz dokumenata na SPOJ oznaka u dokumente. Oznake ORDER BY docs._ts
</pre>
</td>
<td>
<pre>
__.Chain() .filter(function(doc) {povrata dokumenta. Oznaka & & Array.isArray (dokument. Oznake); {povrata doc._ts;}}) .sortBy(function(doc)) .pluck("Tags") .flatten() .value()
</pre>
</td>
<td>Filtre za dokumente koji sadrže svojstvo polja, oznake, a sortira dobivene dokumente prema _ts vremenske oznake sustava svojstva, a zatim projekata + poravnava oznake polja.</td>
</tr>
</tbody>
</table>

## <a name="runtime-support"></a>Podrška za vrijeme izvođenja
[Strani poslužitelja DocumentDB JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/) pruža podršku za većinu osnovna značajke jezika JavaScript kao standardizirani po [ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Sigurnost
JavaScript pohranjene procedure i okidača su u memoriji za testiranje tako da se efekti jedan skripte ne osipanje drugome bez prolaze kroz odvajanja transakcije snimku stanja na razini baze podataka. Izvođenje okruženja su okupljene, ali očistiti o kontekstu nakon svakog pokretanja. Dakle oni su zajamčeno sigurni od neželjenog popratne pojave međusobno.

### <a name="pre-compilation"></a>Prije sastavljanja
Pohranjene procedure, okidača i korisnički definiranih funkcija implicitno unaprijed kompilirane kod oblik bajt su da biste izbjegli sastavljanja trošak prilikom svakog skripte poziva. Na taj način instanci pohranjene procedure su brza i imate manje ostavlja manji trag pri.

## <a name="client-sdk-support"></a>Podrška za klijentski program SDK
Osim klijent [Node.js](documentdb-sdk-node.md) DocumentDB podržava [.NET](documentdb-sdk-dotnet.md), [Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/)i [Python SDK-ovi](documentdb-sdk-python.md). Pohranjene procedure, okidača UDF-ove mogu stvoriti i izvršava na bilo koji od tih SDK-ovi i. Sljedeći primjer pokazuje kako stvoriti i izvoditi pohranjena procedura pomoću klijentskog programa .NET. Imajte na umu kako su vrste .NET ušli u pohranjena procedura kao JSON i pročitati.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
            }"
    };
    
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


Ovaj primjer pokazuje kako koristiti [.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx) za stvaranje okidač stara i stvaranje dokumenta pomoću okidača omogućena. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };
    
    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


I u sljedećem primjeru pokazuje kako stvoriti korisnički definirane funkcije (UDF) i koristiti u [DocumentDB SQL upita](documentdb-sql-query.md).

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };
    
    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST API-JA
Sve DocumentDB operacije može izvoditi RESTful način. Pohranjene procedure, okidača i korisnički definirane funkcije mogu biti registrirani u odjeljku zbirka pomoću HTTP POST. Slijedi primjer kako registrirati pohranjena procedura:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    
    
    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


Pohranjena procedura je registrirana izvršavanjem zahtjev za objavu odnosu na URI dbs/testdb/colls/testColl/sprocs s tijelo koje sadrži pohranjena procedura da biste stvorili. Okidača i UDF-ove mogu biti slično registrirani slanjem objavu u odnosu na /triggers i /udfs redom.
Pohranjena procedura može zatim izvršiti slanjem zahtjeva za objavu na temelju njegove vezu resursa:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT
    
    
    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Ovdje, unos pohranjene procedure se prenosi u tijelu zahtjeva. Imajte na umu da se unos proslijeđena kao JSON polja ulaznih parametara. Pohranjena procedura uzima prvi unos kao dokument koji je tijelo odgovor. Odgovor ćemo dobiti je na sljedeći način:

    HTTP/1.1 200 OK
     
    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Okidača za razliku od pohranjene procedure nije moguće izravno izvršiti. Umjesto toga ih se provode u sklopu operacije na dokumentu. Ne možemo možete odrediti okidača za pokretanje sa zahtjevom za pomoću HTTP zaglavlja. Slijedi zahtjev za stvaranje dokumenta.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger
    
    
    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Ovdje stara okidača za pokretanje sa zahtjevom za navedeni su u zaglavlju x-ms-documentdb-pre-trigger-include. Način, sve nakon okidača ponudit će u zaglavlju x-ms-documentdb-post-trigger-include. Imajte na umu da i prije i poslije okidača možete navesti za zahtjev za dani.

## <a name="sample-code"></a>Ogledni kod

Još poslužiteljsko kod primjera (uključujući [masovnog brisanja](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js)i [Ažuriranje](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) pronaći ćete na našem [Github spremište](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Želite omogućiti zajedničko korištenje fenomenalna pohranjena procedura? Pošaljite nam zahtjeva istaknuti! 

## <a name="next-steps"></a>Daljnji koraci

Nakon što dodate jedan ili više pohranjene procedure, okidača i korisnički definirana funkcija stvorena, možete ih učitavanje i njihov prikaz u Portal Azure pomoću programa Explorer skripte. Dodatne informacije potražite u članku [Prikaz pohranjene procedure, okidača, i korisnički definirane funkcije pomoću programa Explorer za skripte DocumentDB](documentdb-view-scripts.md).

Možete pronaći sljedećim reference i resursima korisna u putu da biste saznali više o DocumentDB poslužiteljsko programiranje:

- [Azure DocumentDB SDK-ovi](https://msdn.microsoft.com/library/azure/dn781482.aspx)
- [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
- [JSON](http://www.json.org/) 
- [JavaScript ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- [JavaScript – JSON vrsta sustava](http://www.json.org/js.html) 
- [Proširivanje sigurne i prijenosni baze podataka](http://dl.acm.org/citation.cfm?id=276339) 
- [Servis usmjerena arhitektura baze podataka](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
- [Hostiranje .NET Runtime u Microsoft SQL server](http://dl.acm.org/citation.cfm?id=1007669)
