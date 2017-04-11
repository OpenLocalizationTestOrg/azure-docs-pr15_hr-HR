<properties
   pageTitle="Razvoj s više regijama DocumentDB | Microsoft Azure"
   description="Saznajte kako pristupiti podataka u više područja iz Azure DocumentDB, potpuno upravljanih NoSQL servis za baze podataka."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="kipandya"/>
   
# <a name="developing-with-multi-region-documentdb-accounts"></a>Razvoj s računima DocumentDB regije

> [AZURE.NOTE] Globalni distribucija baze podataka DocumentDB Općenito dostupan je i automatski omogućena za sve novostvorenu DocumentDB račune. Radimo da biste omogućili globalni raspodjele na sve postojeće račune, ali u privremeni, ako želite da se globalni raspodjele omogućena na svoj račun, [obratite se podršci](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) i smo ćete omogućite je za vas sada.

Da biste iskoristili [globalni raspodjele](documentdb-distribute-data-globally.md), klijentske aplikacije možete odrediti na popisu uređeni preferenca područja koja će se koristiti za izvođenje operacija dokumenta. To možete učiniti tako da postavite pravila veze. Ovisno o računu Azure DocumentDB, trenutnu dostupnost regionalne i popis preferenca naveden, većina optimalnih krajnjoj točki će biti odabran po SDK za izvođenje pisanje i operacija čitanja. 

Ovaj popis preferenca navedena je kada Inicijalizacija veza putem klijentskog programa DocumentDB SDK-ovi. U SDK-ovi prihvatiti neobavezan parametar "PreferredLocations" odnosno uređeni popis Azure područja.

SDK automatski Pošalji sve upisivanje trenutni pisanje regija. 

Sve čitanja poslat će se prvi dostupan područja na popisu PreferredLocations. Ako zahtjev ne uspije, klijent će uspjeti prema dolje na popisu da biste sljedeće područje i tako dalje. 

Klijent SDK-ovi će pokušati samo za čitanje iz područja navedeno u PreferredLocations. Stoga, na primjer, ako račun baze podataka nije dostupna u tri područja, ali klijent samo Odredi dva područja koje nisu pisanje PreferredLocations, pa nema čitanja će posluživanje iz područja za unos, čak i ako prebacivanje.

Aplikaciju možete provjeriti trenutni krajnja točka za pisanje i čitati krajnjoj točki odabrao SDK potvrđivanjem dva svojstva WriteEndpoint i ReadEndpoint, dostupne u verziji SDK 1.8 i iznad. 

Ako je svojstvo PreferredLocations nije postavljeno, sve zahtjeve za će posluživanje iz trenutno područje za unos. 


## <a name="net-sdk"></a>.NET SDK
SDK koristi se bez promjene kod. U ovom slučaju SDK automatski usmjerava oba čitanja i zapisuje trenutno područje za unos. 

U verziji 1.8 i kasnije .NET SDK parametar ConnectionPolicy za Graditelj DocumentClient ima svojstvo naziva Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Ovo je svojstvo vrsta zbirke `<string>` i mora sadržavati popis imena regija. Vrijednosti niza oblikovani su po stupcu naziv regije na [Područja Azure]  [ regions] stranica, bez razmaka ispred ili iza prvi i zadnji znak odnosno.

Trenutni unos i čitanje krajnje točke dostupne su u DocumentClient.WriteEndpoint i DocumentClient.ReadEndpoint redom.

> [AZURE.NOTE] URL-ovi za krajnjih točaka ne treba smatrati kao long-lived konstante. Servis može ažurirati te bilo kojem trenutku. SDK automatski rukuje tu promjenu.

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

    //Setting read region selection preference 
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB 
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, jezika JavaScript i Python SDK-ovi
SDK koristi se bez promjene kod. U ovom slučaju SDK će automatski izravno čitanja i pisanja s trenutnom pisanje regija. 

U verziji 1.8 i noviji od svakog SDK parametar ConnectionPolicy za Graditelj DocumentClient novo svojstvo naziva DocumentClient.ConnectionPolicy.PreferredLocations. To je parametar je polja nizova koja vas vodi popis imena regija. Imena su oblikovani po stupcu naziv regije u [Područja Azure]  [ regions] stranice. Unaprijed definirane konstante možete koristiti i u objektu praktičnost AzureDocuments.Regions

Trenutni unos i čitanje krajnje točke dostupne su u DocumentClient.getWriteEndpoint i DocumentClient.getReadEndpoint redom.

> [AZURE.NOTE] URL-ovi za krajnjih točaka ne treba smatrati kao long-lived konstante. Servis može ažurirati te bilo kojem trenutku. SDK će automatski obrađivati tu promjenu.

U nastavku je primjer koda za NodeJS/Javascript. Python i Java će slijedite isti uzorak.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();
    
    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];
    
    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>OSTALE 
Kada račun baze podataka sadrži nisu dostupne u više područja, klijenti upit možete poslati njezinu dostupnost izvođenjem zahtjevom GET na sljedeće URI.

    https://{databaseaccount}.documents.azure.com/

Servis će vratiti popis regije i njihove odgovarajuće DocumentDB krajnjoj točki ji za na replike. Trenutno područje za unos će biti naznačen u odgovoru. Klijent možete odabrati odgovarajući krajnja točka za daljnje sve zahtjeve za REST API-JA na sljedeći način.

Primjer odgovor

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


-   Sve STAVI, objavu i brisanje zahtjeva za morate otići na navedeni pisanje URI
-   Sve se i zahtjeve samo za čitanje (na primjer upiti) možda idite na sve krajnje točke po izboru klijenta

Pisanje zahtjevi za samo za čitanje područja neće uspjeti koda pogreške HTTP 403 ("zabranjeno").

Ako područje za unos Promijeni nakon faza početne otkrivanje klijenta, kasnije zapisivanja na prethodno područje za unos neće uspjeti koda pogreške HTTP 403 ("zabranjeno"). Klijent trebali biste DOBITI popisom regija ponovno da biste dobili ažurirani pisanje regija.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o distributing podaci globalno DocumentDB u sljedećim člancima:

- [Distribucija podataka globalno s DocumentDB](documentdb-distribute-data-globally.md)
- [Dosljednost razine](documentdb-consistency-levels.md)
- [Funkcioniranje propusnost s više područja](documentdb-manage.md#how-throughput-works-with-multiple-regions)
- [Dodavanje područja pomoću portala za Azure](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/ 
