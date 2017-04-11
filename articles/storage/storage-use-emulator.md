<properties 
    pageTitle="Korištenje Emulator Azure prostora za pohranu za razvoj i testiranje | Microsoft Azure" 
    description="Emulator Azure prostora za pohranu pruža besplatnu lokalne platforme za razvoj i testiranje protiv Azure prostora za pohranu. Saznajte više o emulator pohrane, uključujući kako se provjerava zahtjeve, kako se povezati s emulator iz aplikacije i kako koristiti alat naredbenog retka." 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>
<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="tamram"/>

# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Korištenje Emulator Azure prostora za pohranu za razvoj i testiranje

## <a name="overview"></a>Pregled

Prostor za pohranu emulator Microsoft Azure pruža lokalnom okruženju u kojem se oponaša services blobova platforme Azure, reda čekanja i tablice u svrhu razvoj. Korištenje emulator prostora za pohranu, možete testirati vaše aplikacije u odnosu na lokalno, servise za pohranu bez stvaranje Azure pretplatu ili povećavanja sve troškove. Kada budete zadovoljni kako funkcionira vaše aplikacije u na emulator, možete prijeći pomoću računa Azure prostora za pohranu u oblaku.

> [AZURE.NOTE] Dostupna je za pohranu emulator kao dio sustava [Microsoft Azure SDK](https://azure.microsoft.com/downloads/). Možete instalirati i pohranu emulator pomoću [samostalne installer](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409). Da biste konfigurirali prostor za pohranu emulator, morate imati administratorske ovlasti na računalu.
> 
> Prostor za pohranu emulator trenutno se pokreće samo u sustavu Windows.
>  
> Imajte na umu da se podaci stvoren u verziji za pohranu emulator nisu zajamčiti bude dostupno i kada koristite neku drugu verziju. Ako vam je potrebna zadržava podataka long termina, preporučuje se spremanje podataka na račun za Azure prostora za pohranu umjesto emulator prostora za pohranu.

## <a name="how-the-storage-emulator-works"></a>Kako funkcionira emulator prostora za pohranu
 
Emulator prostora za pohranu koristi lokalnu instancu Microsoft SQL Server i lokalnom sustavu datoteka za emulaciju servisa Azure prostora za pohranu. Emulator prostora za pohranu po zadanom koristi baze podataka u programu Microsoft SQL Server 2012 Express LocalDB.  Možete odabrati da biste konfigurirali emulator prostora za pohranu za pristup lokalnu instancu programa SQL Server umjesto instancu LocalDB. Dodatne informacije potražite [početka i inicijalizaciju emulator prostora za pohranu](#start-and-initialize-the-storage-emulator) ispod.

Možete instalirati SQL Server Management Studio Express da biste upravljali LocalDB instalaciju. Prostor za pohranu emulator povezuje SQL Server ili LocalDB pomoću provjere autentičnosti sustava Windows. 

Postoje neke razlike u funkcijama između emulator prostora za pohranu i servise za pohranu Azure. Dodatne informacije o te razlike potražite u članku [razlike između emulator prostora za pohranu i spremište Azure](#differences-between-the-storage-emulator-and-azure-storage).

## <a name="authenticating-requests-against-the-storage-emulator"></a>Provjera autentičnosti zahtjeva protiv emulator prostora za pohranu

Baš kao i u slučaju Azure prostora za pohranu u oblaku, svaki zahtjev protiv emulator prostora za pohranu mora proći, osim ako je anonimni zahtjev. Možete provjeriti autentičnost zahtjeve protiv emulator prostora za pohranu pomoću provjere autentičnosti zajednički ključ ili zajednički pristup potpisom (SAS).

### <a name="authentication-with-shared-key-credentials"></a>Provjera autentičnosti pomoću vjerodajnica za zajedničko korištenje ključ

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Dodatne informacije o nizu za povezivanje potražite u članku [Konfiguriranje Azure prostora za pohranu nizu za povezivanje](storage-configure-connection-string.md). 

### <a name="authentication-with-a-shared-access-signature"></a>Provjera autentičnosti potpisom zajednički pristup 

Neke Azure prostora za pohranu klijenta biblioteke kao što je biblioteka Xamarin podržavaju samo provjera autentičnosti s token za potpis (SAS) zajednički pristup. Morat ćete stvoriti ovaj token SAS pomoću alata za ili aplikaciju koja podržava provjeru autentičnosti ključ za zajedničko korištenje. Jednostavan je način za stvaranje tokena SAS je Azure PowerShell:

1. Ako to već niste učinili, instalirajte Azure PowerShell. Preporučuje se koristiti najnoviju verziju cmdleta Azure PowerShell. Upute za instalaciju potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md#Install) .

2. Otvorite Azure PowerShell i izvršite sljedeće naredbe. Imajte na umu da biste zamijenili *naziv_računa* i *ACCOUNT_KEY ==* pomoću svojih vjerodajnica. *CONTAINER_NAME* zamijenite nazivom vašem odabiru.

        $context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="
        
        New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context
        
        $now = Get-Date 
        
        New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri

Dobivene potpisa zajednički pristup URI za novi spremnik mora biti sličnu ovoj:

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss

Potpis zajednički pristup stvorenih u ovom se primjeru vrijedi za jedan dan. Potpis daje puni pristup (odnosno čitanje, pisanje, brisanje, popis) blob-ova u spremniku.

Dodatne informacije o potpisima zajednički pristup potražite u članku [Korištenje zajednički pristup potpisa (SAS)](storage-dotnet-shared-access-signature-part-1.md).


## <a name="start-and-initialize-the-storage-emulator"></a>Pokretanje i pokretanje emulator prostora za pohranu

Da biste započeli emulator Azure prostora za pohranu, odaberite gumb Start ili pritisnite tipku s logotipom sustava Windows. Počnite pisati **Emulator Azure prostora za pohranu**i odaberite na emulator s popisa aplikacija sustava. 

Kada je pokrenut na emulator, vidjet ćete ikonu u području obavijesti na programskoj traci sustava Windows.

Kada se pokrene emulator prostora za pohranu, pojavit će se prozor naredbenog retka. Možete koristiti ovaj prozor naredbenog retka za pokretanje i zaustavljanje emulator prostora za pohranu kao i brisanje podataka se trenutni status, a pokrenuti na emulator. Dodatne informacije potražite u članku [Referenca alat za pohranu Emulator naredbenog retka](#storage-emulator-command-line-tool-reference).

Kada se Zatvori prozor naredbenog retka, emulator prostora za pohranu i dalje da biste pokrenuli. Da biste ponovno otvorili naredbeni redak, slijedite korake iznad kao da se početni emulator prostora za pohranu.

Prilikom prvog pokretanja emulator prostora za pohranu u okruženju lokalno spremište pokrenut umjesto vas. Pokretanje procesa stvara bazu podataka u LocalDB i pridržava HTTP priključke za svaki servis lokalno spremište. 

Prema zadanim postavkama imeniku Emulator C:\Program Files (x86) \Microsoft SDKs\Azure\Storage instaliran je emulator prostora za pohranu. 

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Pokretanje emulator prostora za pohranu za korištenje druge baze podataka SQL

Možete koristiti alat naredbenog retka za pohranu emulator Inicijalizacija emulator prostora za pohranu na instancu komponente SQL baze podataka koji nisu zadane instance LocalDB. Imajte na umu da morate koristiti alat naredbenog retka s administrativnim privilegijama za inicijalizaciju pozadinsku bazu podataka za pohranu emulator:

1. Kliknite gumb **Start** ili pritisnite tipku s logotipom **sustava Windows** . Počnite pisati `Azure Storage Emulator` , a zatim odaberite kad se pojavi da biste otvorili alat za pohranu emulator naredbenog retka.
2. U prozor naredbeni redak upišite sljedeću naredbu, gdje `<SQLServerInstance>` je naziv instance sustava SQL Server. Da biste koristili LocalDb navedite `(localdb)\v11.0` kao instancu sustava SQL Server.

        AzureStorageEmulator init /server <SQLServerInstance> 
    
    Možete koristiti i sljedeću naredbu, koji usmjerava emulator koristiti zadane instance sustava SQL Server:

        AzureStorageEmulator init /server .\\ 

    Ili, koristite sljedeću naredbu, koji se ponovno inicijalizira zadane instance LocalDB baze podataka:

        AzureStorageEmulator init /forceCreate 

Dodatne informacije o tim naredbama potražite u članku [Referenca alat za pohranu Emulator naredbenog retka](#storage-emulator-command-line-tool-reference).

## <a name="addressing-resources-in-the-storage-emulator"></a>Adresiranje resursa u emulator prostora za pohranu

Krajnje točke servisa za pohranu emulator se razlikuju od onih račun za Azure prostora za pohranu. Razlika je blokirale u zbog činjenice kojoj na lokalnom računalu izvođenje razlučivost naziv domene, pa krajnje točke za pohranu emulator zahtijevaju Lokalna adresa umjesto naziva domene.

Kada se adresa resursa u račun za Azure prostora za pohranu, možete koristiti sljedeće shemu, pri čemu je naziv računa dio URI naziv glavnog računala, a resursa koji se adresirana dio puta URI:

    <http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>

Na primjer, sljedeći URI je valjanu adresu za blob u račun za Azure prostora za pohranu:

    https://myaccount.blob.core.windows.net/mycontainer/myblob.txt

U emulator prostora za pohranu jer na lokalnom računalu izvoditi razlučivost naziv domene, naziv računa nije dio puta URI umjesto naziv glavnog računala. Resursa u emulator prostora za pohranu koristite shemu sljedeće:

    http://<local-machine-address>:<port>/<account-name>/<resource-path>

Ako, na primjer, sljedeće adrese možda koristi za pristup blob u emulator prostora za pohranu:

    http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt

Krajnje točke servisa za pohranu emulator su:

    Blob Service: http://127.0.0.1:10000/<account-name>/<resource-path>
    Queue Service: http://127.0.0.1:10001/<account-name>/<resource-path>
    Table Service: http://127.0.0.1:10002/<account-name>/<resource-path>

### <a name="addressing-the-account-secondary-with-ra-grs"></a>Adresiranja sekundarne s RA GRS računa

Početak verzijom 3.1, emulator račun za pohranu podržava pristup za čitanje zemlj suvišne replikacije (RA GRS). Za resursa za pohranu u oblaku i u lokalnom emulator, možete pristupiti sekundarne mjesto za dodavanje - sekundarni naziv računa. Ako, na primjer, sljedeće adrese možda koristi za pristup blob pomoću sekundarnog samo za čitanje u emulator prostora za pohranu:

    http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt 

> [AZURE.NOTE] Da biste postigli programski pristup sekundarnom s emulator prostora za pohranu koristite klijentska biblioteka za pohranu za .NET 3.2 ili novija verzija. Potražite u članku [Microsoft Azure prostora za pohranu klijenta biblioteka za .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) detalje.

## <a name="storage-emulator-command-line-tool-reference"></a>Prostor za pohranu emulator alata naredbenog retka reference

Uveden u verziji 3.0, kada pokrenete Emulator prostora za pohranu, prikazat će se prozor naredbenog retka skočnim. Koristite prozor naredbenog retka za pokretanje i zaustavljanje na emulator kao upita za status, a zatim druge radnje.

> [AZURE.NOTE] Ako imate Microsoft Azure izračunati emulator instaliran, palete sustava pojavit će se prilikom pokretanja Emulator prostora za pohranu. Desnom tipkom miša kliknite ikonu za prikaz izbornika, koji omogućuje grafički pokretanje i zaustavljanje Emulator prostora za pohranu.

### <a name="command-line-syntax"></a>Sintaksa naredbenog retka

    AzureStorageEmulator [start] [stop] [status] [clear] [init] [help]

### <a name="options"></a>Mogućnosti

Da biste vidjeli popis mogućnosti, upišite `/help` naredbeni redak.

| Mogućnost | Opis                                                    | Naredba                                                                                                 | Argumenti                                                                                                         |
|--------|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Pokretanje**  | Pokretanja emulator prostora za pohranu.                                | `AzureStorageEmulator start [-inprocess]`                                                                    | *-inprocess*: pokretanje u emulator u trenutnom procesu umjesto stvaranja novog postupak.                          |
| **Zaustavi**   | Zaustavlja emulator prostora za pohranu.                                    | `AzureStorageEmulator stop`                                                                                  |                                                                                                                   |
| **Status** | Ispisuje status emulator prostora za pohranu.                     | `AzureStorageEmulator status`                                                                                |                                                                                                                   |
| **Očisti**  | Čišćenje podataka u svim servisima naveden u naredbenom retku. | `AzureStorageEmulator clear [blob] [table] [queue] [all]                                                    `| *blob*: briše bloba podataka. <br/>*red čekanja*: čisti red podataka. <br/>*tablica*: čisti podatke u tablici. <br/>*sve*: uklanja sve podatke u svim servisima. |
| **Init**   | Izvodi jednokratno pokretanje da biste postavili na emulator.       | `AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate] [-inprocess]` | *-poslužitelja serverName\instanceName*: određuje na poslužitelju koji hostira instancu sustava SQL. <br/>*instanceName - sqlinstance*: određuje naziv instance sustava SQL će se koristiti u zadane instance poslužitelja. <br/>*-forcecreate*: nameće početak stvaranja baze podataka SQL, čak i ako već postoji. <br/>*-inprocess*: izvodi Inicijalizacija u trenutnom procesu umjesto množenja novi proces. Morate pokrenuti trenutnog procesa s dodatnim dozvolama za izvođenje Inicijalizacija.          |
                                                                                                                  
## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Razlike između emulator prostora za pohranu i spremište Azure

Budući da je prostora za pohranu emulator Emulirani okruženja koji se izvodi u lokalnu instancu SQL, postoje neke razlike funkcija u emulator i račun za Azure prostora za pohranu u oblaku:

- Emulator prostora za pohranu podržava samo jednog fixed računa i ključa poznati provjeru autentičnosti.

- Emulator prostora za pohranu nije skalabilni prostora za pohranu servisa, a ne podržava velik broj Istodobni klijente.

- Kao što je opisano u [adresiranja resursa za pohranu emulator](#addressing-resources-in-the-storage-emulator)Resursi su adresirana drugačije emulator prostora za pohranu i račun za Azure prostora za pohranu. Ova je razlika blokirale u zbog činjenice kojoj razlučivost naziv domene je dostupna u oblaku, ali ne i na lokalnom računalu.

- Početak verzijom 3.1, emulator račun za pohranu podržava pristup za čitanje zemlj suvišne replikacije (RA GRS). U emulator, svi računi imaju RA GRS omogućeno i između replike primarnih i sekundarnih nikad ne postoji neki kašnjenja. Operacije dobiti stat Blob servisa, dobiti stat reda čekanja servisa i dobiti stat tablice servisa podržani su na sekundarnom računa i uvijek će vratiti vrijednost u `LastSyncTime` elementa odgovora kao i trenutno vrijeme prema podlozi SQL baze podataka.

    Da biste postigli programski pristup sekundarnom s emulator prostora za pohranu koristite klijentska biblioteka za pohranu za .NET 3.2 ili novija verzija. Potražite u članku [Microsoft Azure prostora za pohranu klijenta biblioteka za .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) detalje.

- Datoteka servisa i krajnje točke servisa protokola za SMB su trenutno ne podržava u emulator prostora za pohranu.

- Prostor za pohranu emulator vraća pogreške VersionNotSupportedByEmulator (HTTP Šifra stanja 400 - Neispravan zahtjev) ako koristite verziju servise za pohranu koji još nije podržana verzija emulator koristite.

### <a name="differences-for-blob-storage"></a>Razlike u spremište blobova platforme 

Sa spremištem blobova u na emulator primjenjuju se sljedeće razlike:

- Prostor za pohranu emulator samo podržava blob veličine do 2 GB.

- Stavite Blob operacija mreži s blob koji postoji u emulator prostora za pohranu i ima aktivni Zakup, čak i ako Zakup ID-a nije naveden kao dio zahtjev. 

- Dodavanje Blob operacije nisu podržane u emulator. Pokušaj operacije na programa dodavanjem blob vraća pogreške FeatureNotSupportedByEmulator (HTTP Šifra stanja 400 - Neispravan zahtjev).

### <a name="differences-for-table-storage"></a>Razlike u spremište tablica 

Sa spremištem tablica u na emulator primjenjuju se sljedeće razlike:

- Svojstva datuma servis za tablice u prostor za pohranu emulator podržava samo raspon podržava SQL Server 2005 (*odnosno*ona jesu obavezna biti kasnije od siječnja 1, 1753). Svi datumi prije siječanj 1, 1753 mijenjaju se u tu vrijednost. Preciznost datuma ograničeno je na preciznosti SQL Server 2005, što znači da su datumi precizno 1/300th sekunde.

- Prostor za pohranu emulator podržava particija ključ i redak svojstvo ključa vrijednosti manje od 512 bajtova svaki. Uz to, ukupnu veličinu naziv računa, naziv tablice i nazivi svojstvo ključa zajedno ne može prelaziti 900 bajta.

- Ukupnu veličinu retka u tablici u prostor za pohranu emulator ograničeno je na manje od 1 MB.

- U emulator prostora za pohranu unesite svojstva podataka `Edm.Guid` ili `Edm.Binary` podržava samo na `Equal (eq)` i `NotEqual (ne)` operatori usporedbe u upitu filtriranje nizova.

### <a name="differences-for-queue-storage"></a>Razlike u redu čekanja za pohranu

Red čekanja za pohranu u na emulator odnose se nema razlike.

## <a name="storage-emulator-release-notes"></a>Prostor za pohranu emulator napomene

### <a name="version-45"></a>Verzija 4,5

- Ispraviti pogreške koje su uzrokovale pokretanje i instalaciju emulator prostora za pohranu za neće uspjeti ako preimenovana sigurnosnom bazu podataka.

### <a name="version-44"></a>Verzija 4.4

- Emulator prostora za pohranu sada podržava verzija 2015-12-11 servise za pohranu na Blob, reda čekanja i tablice krajnje točke servisa.

- Prostor za pohranu emulator smeća skup podataka blob sada je učinkovitiji pri dijeljenju s velikim brojem blob-ova.

- Ispraviti pogreške koje su uzrokovale spremnik ACL XML nastavlja se malo drugačije od servis za pohranu kako ga.

- Ispraviti pogreške koje ponekad događa max i min vrijednosti datuma i vremena da biste se prijavili u neispravne vremensku zonu.

### <a name="version-43"></a>Verzija 4,3

- Prostor za pohranu emulator sada podržava verzija 2015-07-08 servise za pohranu na Blob, reda čekanja i tablice krajnje točke servisa.

### <a name="version-42"></a>Verzija 4.2

- Emulator prostora za pohranu sada podržava verzija 2015 04 05 servise za pohranu na Blob, reda čekanja i tablice krajnje točke servisa.

### <a name="version-41"></a>Verzija 4.1

- Prostor za pohranu emulator sada podržava verzija 2015 02 21 servise za pohranu na Blob, reda čekanja i tablice krajnje točke servisa, osim nove značajke za dodavanje Blob. 

- Ako koristite verziju servise za pohranu koji još nije podržana u toj verziji programa u emulator emulator prostora za pohranu sada vraća smisleni poruka. Preporučujemo korištenje najnovije verzije sustava emulator. Ako naiđete na pogrešku VersionNotSupportedByEmulator (HTTP Šifra stanja 400 - Neispravan zahtjev), preuzmite najnoviju verziju emulator prostora za pohranu.

- Kojemu ispraviti pogrešku entitet podataka trkaći tablice uvjet uzrokovati biti netočan tijekom operacije Istodobni spajanja.

### <a name="version-40"></a>Verzije 4.0

- Izvršna emulator prostora za pohranu preimenovana je u *AzureStorageEmulator.exe*.

### <a name="version-32"></a>Verzija 3,2

- Emulator prostora za pohranu sada podržava verziju 2014. 02 14 servise za pohranu na Blob, reda čekanja i tablice krajnje točke servisa. Imajte na umu da krajnje točke servisa datoteka trenutno nisu podržani u emulator prostora za pohranu. Detalje o verziji 2014. 02 14 potražite u članku [Rad s verzijama za Azure servise za pohranu](https://msdn.microsoft.com/library/azure/dd894041.aspx) .

### <a name="version-31"></a>Verzijom 3.1

- Pristup za čitanje zemlj suvišne prostora za pohranu (RA GRS) sada podržava emulator prostora za pohranu. Početak stat Blob servisa, dobiti stat reda čekanja servisa i dobiti tablice servisa stat API-nisu podržane za sekundarne račun i uvijek vraća vrijednost elementa odgovor LastSyncTime kao i trenutno vrijeme prema podlozi SQL baze podataka. Da biste postigli programski pristup sekundarnom s emulator prostora za pohranu koristite klijentska biblioteka za pohranu za .NET 3.2 ili novija verzija. Potražite u članku Microsoft Azure prostora za pohranu klijentska biblioteka za .NET referencu pojedinosti.

### <a name="version-30"></a>Verzije 3.0

- Emulator Azure prostora za pohranu više poslane u paketu isti kao emulator računalnim.

- Grafičko korisničko sučelje za pohranu emulator ukinuta je korist skriptama naredbenim sučeljem redak. Detalje o naredbenim sučeljem redak potražite u članku referenca alata za za pohranu Emulator naredbenog retka. Grafičkog sučelja će se i dalje biti prisutne u verzije 3.0, no to se može pristupiti samo kad je instaliran Emulator izračunati tako da desnom tipkom miša na palete sustava i odaberete prikaz UI Emulator prostora za pohranu.

- Verzije 2013-08-15 servisa Azure prostora za pohranu sada u potpunosti podržane. (Prethodno ovu verziju je samo podržava pohranu Emulator verziju 2.2.1 pretpregleda.)
