<properties
    pageTitle="Upravljanje istodobnosti u spremište Microsoft Azure"
    description="Kako upravljati istodobnosti za servise Blob, reda čekanja, tablice i datoteke"
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jahogg"/>

# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Upravljanje istodobnosti u spremište Microsoft Azure

## <a name="overview"></a>Pregled

Moderna internetske temelji aplikacije obično imaju više korisnika, prikaz i ažuriranje podataka istodobno. Potreban je razvojnim inženjerima pažljivo razmišljati o omogućavaju predvidljivi sučelje njihove krajnjim korisnicima osobito scenariji kojima više korisnika možete ažurirati iste podatke. Postoje tri glavna podatkovna istodobnosti strategije razvojnim inženjerima će obično razmislite o:  


1.  Optimistična Procjena istodobnosti – ažuriranje će kao dio njegov ažuriranje provjerite ako podatke od promijenjena aplikacija aplikacije izvođenje zadnje pročitajte tih podataka. Ako, na primjer, radi li dva korisnika koji pregledavaju wiki stranice ažuriranje na istoj stranici pa platformu wiki osigurati drugi ažuriranje prebrisivanje prvog ažuriranja – i oba korisnici razumijevanje je li njihove ažuriranje uspjelo ili nije. U ovom strategije najčešće koristi u web-aplikacijama.
2.  Pesimistična Procjena istodobnosti – aplikacije pogled za izvođenje ažuriranje se zaključati na onemogućuje druge korisnike da ažuriranje podataka dok je objavio zaključavanje objekt. Na primjer, u scenariju matrice/podređeni podataka replikacije gdje će se samo glavni izvršiti ažuriranja matricu će obično držite zaključavanje dulje vrijeme na podacima da biste bili sigurni da nitko drugi možete ažurirati.
3.  Zadnji ima prednost writer – za pristup koja omogućuje sve operacije ažuriranja za nastavak bez provjere ako neke druge aplikacije sadrži ažurirani podataka aplikacije najprije pročitati podatke. Ovaj strategije (ili nedostatak formalno strategije) obično se koriste gdje će se podaci particije na način da postoji bez vjerojatnost da više korisnika pristupiti s istim podacima. Može biti korisno gdje se obrađuju strujanja short-lived podataka.  

Ovaj članak sadrži pregled načina platforme Azure prostora za pohranu pojednostavljuje razvoj unosom prve klase podrška za sva tri te strategije istodobnosti.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure prostora za pohranu – pojednostavljuje razvoj oblaka
Servis za pohranu Azure podržava sve tri strategije, iako isticanjem u sposobnost nudi puna podrška za optimistična Procjena i pesimistična Procjena istodobnosti jer je dizajniran obuhvaćaju istaknuti dosljednost modela koji se jamčiti koji servis za pohranu pamti podataka umetanje ili postupak ažuriranja sve daljnje pristupa da biste podatke će vidjeti najnovije ažuriranje. Platforme za pohranu koji koriste modelu usmjerenog dosljednost imati kašnjenja između kada pisanje provodi od korisnika i ažurirane podatke mogu vidjeti drugim korisnicima tako complicating razvoja klijentske aplikacije da bi se sprječavanje nedosljednosti iz utjecaja krajnjim korisnicima.  

Osim odabira stvaranje strategije odgovarajuće istodobnosti razvojnim inženjerima mora biti svjesni kako izolira spremišta platforme promjene – osobito promjene na isti objekt preko transakcije. Servis za Azure prostora za pohranu koristi odvajanja snimke omogućuje čitanje operacije da se dogodi čine istovremeno operacije pisanja unutar jedna particija. Za razliku od drugih razine odvajanja snimke odvajanja jamčiti da sve čitanja potražite u članku dosljedan snimku podataka čak i kada su ažuriranja pojavljivanja – zapravo vraćanjem zadnje vrijednosti izvršenja prilikom ažuriranja transakcije obrade.  

## <a name="managing-concurrency-in-blob-storage"></a>Upravljanje istodobnosti u spremište blobova platforme
Možete odabrati da biste koristili neki optimistična Procjena ili pesimistična Procjena istodobnosti modela za upravljanje pristupom blob-ova i spremnika u servisu blob. Ako izričito ne navedete u strategije posljednje zapisivanja ima prednost je zadana vrijednost.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Optimistična Procjena istodobnosti blob-ova i spremnika  

Servis za pohranu dodjeljuje identifikatora svaki objekt pohranjen. Ovaj identifikator ažurira se svaki put kada operaciju ažuriranja se izvodi na objekt. Identifikator se vraćaju u klijentu kao dio je HTTP GET odgovor pomoću zaglavlja e-oznake (entitet oznaka) koji je definiran unutar HTTP protokola. Korisnika koji se izvodi ažuriranje na takav objekt možete poslati u izvornom e-oznake zajedno s uvjetnim zaglavlje da biste bili sigurni da ažuriranje samo pojaviti ako su ispunjeni određenim uvjetima – u ovom slučaju uvjet je zaglavlje "If-Match" koji su potrebni za pohranu servisa da biste bili sigurni da je vrijednost e-oznake naveden u zahtjevu za ažuriranje jednak je pohranjen u servis za pohranu.  

Struktura postupak je na sljedeći način:  

1.  Dohvaćanje blob iz servis za pohranu, odgovor sadrži vrijednost HTTP e-oznake zaglavlja koja služi za identifikaciju trenutnu verziju objekt u servis za pohranu.
2.  Kada ažurirate blob-om, uvrstite jedna vrijednost koji ste primili u koraku 1 u zaglavlju **If-Match** uvjetnog zahtjeva za slanje servis.
3.  Servis uspoređuje vrijednost e-oznake u zahtjev s trenutnu vrijednost e-oznake blob-om.
4.  Ako je trenutna vrijednost e-oznake blob-om neku drugu verziju od e-oznake u zaglavlju **If-Match** uvjetno u zahtjev, servis vraća 412 pogreške klijent. To označava klijentu drugi proces je ažurirao blob-om jer ga dohvatili klijent.
5.  Ako je trenutna vrijednost e-oznake blob-om iste verzije kao e-oznake u zaglavlju **If-Match** uvjetno u zahtjev, servis izvodi traženi postupak i ažurira trenutnu vrijednost e-oznake blob-om za prikaz u kojem je stvorena nova verzija.  

U sljedećim C# isječak (pomoću biblioteke za pohranu klijenta 4.2.0) prikazuje jednostavan primjer kako se izgraditi **AccessCondition If-Match** na temelju jedna vrijednost na kojoj se pristupa iz svojstava blob koji je prethodno dohvatiti ili umetnuti. Zatim koristi **AccessCondition** objekta kada je ažuriranje blob-om: objekt **AccessCondition** dodaje zaglavlje **If-Match** zahtjev. Ako drugi proces ažurirao blob-om, servis blob vraća poruku stanja 412 HTTP (preduvjet nije uspio). Možete preuzeti ovdje punu ogledni: [Upravljanje istodobnosti pomoću Azure prostora za pohranu](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

    // Retrieve the ETag from the newly created blob
    // Etag is already populated as UploadText should cause a PUT Blob call
    // to storage blob service which returns the etag in response.
    string orignalETag = blockBlob.Properties.ETag;

    // This code simulates an update by a third party.
    string helloText = "Blob updated by a third party.";

    // No etag, provided so orignal blob is overwritten (thus generating a new etag)
    blockBlob.UploadText(helloText);
    Console.WriteLine("Blob updated. Updated ETag = {0}",
    blockBlob.Properties.ETag);

    // Now try to update the blob using the orignal ETag provided when the blob was created
    try
    {
        Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
        blockBlob.UploadText(helloText,accessCondition:
        AccessCondition.GenerateIfMatchCondition(orignalETag));
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        {
            Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
            // TODO: client can decide on how it wants to handle the 3rd party updated content.
        }
        else
            throw;
    }  

Servis za pohranu i sadrži podršku za dodatne uvjetno zaglavlja kao što su **If-Modified-od**, **If-nepromijenjenu-od** i **If-ništa – Match** kao i kombinacije sadržaja. Dodatne informacije potražite u članku [Određivanje uvjetno zaglavlja Blob terenu](http://msdn.microsoft.com/library/azure/dd179371.aspx) na MSDN-u.  

U sljedećoj su tablici navedene operacije spremnik koja prihvaća uvjetno zaglavlja kao što su **If-Match** u zahtjevu za i vratiti jedna vrijednost u odgovoru.  

| Postupak                | Vraća spremnik jedna vrijednost | Prihvaća uvjetno zaglavlja |
|:-------------------------|:-----------------------------|:----------------------------|
| Stvaranje kontejnera         | Da                          | ne                          |
| Dobiti spremnik svojstva | Da                          | ne                          |
| Početak spremnik metapodataka   | Da                          | ne                          |
| Postavljanje spremnik metapodataka   | Da                          | Da                         |
| Početak spremnik ACL-a        | Da                          | ne                          |
| Postavljanje spremnik ACL-a        | Da                          | Da (*)                     |
| Brisanje spremnik         | ne                           | Da                         |
| Spremnik Zakup          | Da                          | Da                         |
| Popis blob-ova               | ne                           | ne                          |

(*) Dozvole definira SetContainerACL predmemoriraju, a zatim ažuriranjima te dozvole snimite 30 sekundi proširiti tijekom razdoblja ažuriranja ne jamči da budu dosljedni.  

U sljedećoj su tablici navedene operacije blob koja prihvaća uvjetno zaglavlja kao što su **If-Match** u zahtjevu za i vratiti jedna vrijednost u odgovoru.

| Postupak           | Vraća jedna vrijednost | Prihvaća uvjetno zaglavlja           |
|:--------------------|:-------------------|:--------------------------------------|
| Stavite blobova platforme            | Da                | Da                                   |
| Početak blobova platforme            | Da                | Da                                   |
| Dobiti Blob svojstva | Da                | Da                                   |
| Postavljanje svojstava Blob | Da                | Da                                   |
| Početak Blob metapodataka   | Da                | Da                                   |
| Postavljanje Blob metapodataka   | Da                | Da                                   |
| Zakup Blob (*)      | Da                | Da                                   |
| Snimka Blob       | Da                | Da                                   |
| Kopiranje blobova platforme           | Da                | Da (za izvorišne i odredišne blob) |
| Prekid Blob Kopiraj     | ne                 | ne                                    |
| Brisanje blobova platforme         | ne                 | Da                                   |
| Stavite bloka           | ne                 | ne                                    |
| Postavljanje popisa blokova      | Da                | Da                                   |
| Dohvaćanje popisa blokova      | Da                | ne                                    |
| Postavljanje stranice            | Da                | Da                                   |
| Dohvaćanje raspon stranica     | Da                | Da                                   |


(*) Zakup Blob promijenite e-oznake na blob.  

### <a name="pessimistic-concurrency-for-blobs"></a>Pesimistična Procjena istodobnosti za blob-ova
Da biste zaključali blob za isključivo korištenje nabavljate [Zakup](http://msdn.microsoft.com/library/azure/ee691972.aspx) na njemu. Kada nabavljate u Zakup, navedite koliko potreban vam je u Zakup: to može biti za između 15 do 60 sekundi ili beskonačno koja iznosi za zaključavanje. Možete obnoviti konačne Zakup da biste je proširili, a sve Zakup možete i otpustite kada završite s njim. Servis blob automatski izdaje konačne leases kada isteknu.  

Leases Omogućivanje strategije različite sinkronizacije planirate podržavati, uključujući isključivo pisanje / Zajedničko pisanje čitanja, isključujući te dvije vrijednosti / isključivo čitanje i pisanje zajedničkih / isključivo čitati. Gdje je postoji u Zakup servis za pohranu nameće isključivo zapisivanje (staviti, postavljanje i brisanje operacije) no osiguravanje exclusivity za čitanje operacije potrebno programiranje da biste bili sigurni da sve klijentske aplikacije koristiti Zakup ID-a i taj klijent samo jedan po jedan sadrži ID-a za valjane Zakup Operacije koje uključuju rezultat ID Zakup zajedničko čitanja za čitanje.  

U sljedećim C# isječak prikazuje primjer pri dohvaćanju isključivo Zakup 30 sekundi na blob, ažuriranje sadržaja blob-om, a zatim otpustite u Zakup. Ako već postoji valjani Zakup na blob-om kada pokušate Nabavite novu Zakup, servis blob vraća i rezultat status "HTTP (409) sukoba". Isječak u nastavku koristi objekta **AccessCondition** za Enkapsulacija informacije Zakup kada će se zahtjev za ažuriranje blob u servis za pohranu.  Možete preuzeti ovdje punu ogledni: [Upravljanje istodobnosti pomoću Azure prostora za pohranu](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    // Acquire lease for 15 seconds
    string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
    Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

    // Update blob using lease. This operation will succeed
    const string helloText = "Blob updated";
    var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
    blockBlob.UploadText(helloText, accessCondition: accessCondition);
    Console.WriteLine("Blob updated using an exclusive lease");

    //Simulate third party update to blob without lease
    try
    {
        // Below operation will fail as no valid lease provided
        Console.WriteLine("Trying to update blob without valid lease");
        blockBlob.UploadText("Update without lease, will fail");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
            Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
        else
            throw;
    }  

Ako pokušate postupak pisanja na leased blob bez prolaska ID Zakup, zahtjev ne uspijeva uz 412 poruku o pogrešci. Imajte na umu da ako u Zakup istječe prije pozivanja metodu **UploadText** , ali i dalje proći ID Zakup, zahtjev i ne uspijeva uz poruku o pogrešci **412** . Dodatne informacije o upravljanju Zakup isteka vremena i Zakup ID-a potražite u dokumentaciji za OSTALE [Zakup Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx) .  

Sljedeće postupke blob omogućuju upravljanje pesimistična Procjena istodobnosti leases:  


-   Stavite blobova platforme
-   Početak blobova platforme
-   Dobiti Blob svojstva
-   Postavljanje svojstava Blob
-   Početak Blob metapodataka
-   Postavljanje Blob metapodataka
-   Brisanje blobova platforme
-   Stavite bloka
-   Postavljanje popisa blokova
-   Dohvaćanje popisa blokova
-   Postavljanje stranice
-   Dohvaćanje raspon stranica
-   Snimka Blob - Zakup ID neobavezno ako postoji u Zakup
-   Kopiranje Blob - Zakup ID-a potreban nalazi li se u Zakup na odredište blob-om
-   Prekid Kopiraj Blob - Zakup ID-a potreban nalazi li se na odredište blob beskonačno Zakup
-   Zakup Blob  

### <a name="pessimistic-concurrency-for-containers"></a>Pesimistična Procjena istodobnosti za spremnika
Leases na spremnika omogućiti istu strategije sinkronizacije planirate podržavati na blob-ova (isključivo pisanje / Zajedničko pisanje čitanja, isključujući te dvije vrijednosti / isključivo čitanje i pisanje zajedničkih / isključivo čitati) no za razliku od blob-ova servis za pohranu samo nameće exclusivity na operacija brisanja. Da biste izbrisali kontejner s aktivni Zakup, klijenta mora sadržavati Identifikator aktivni Zakup sa zahtjevom za brisanje. Svih ostalih operacija spremnik uspjeti na leased spremnik bez ID Zakup u tom slučaju su zajedničke operacije. Ako potreban je exclusivity ažuriranja (stavi ili skup) ili operacije čitanja zatim razvojnim inženjerima potrebno je provjeriti sve klijente pomoću Zakup ID-a i koje samo jednog klijenta po ima valjan Zakup ID.  

Sljedeće postupke spremnik omogućuju upravljanje pesimistična Procjena istodobnosti leases:  

-   Brisanje spremnik
-   Dobiti spremnik svojstva
-   Početak spremnik metapodataka
-   Postavljanje spremnik metapodataka
-   Početak spremnik ACL-a
-   Postavljanje spremnik ACL-a
-   Spremnik Zakup  

Dodatne informacije potražite u člancima:  

- [Određivanje uvjetnog zaglavlja za Blob terenu](http://msdn.microsoft.com/library/azure/dd179371.aspx)
- [Spremnik Zakup](http://msdn.microsoft.com/library/azure/jj159103.aspx)
- [Zakup Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Upravljanje istodobnosti u servisu tablice
Servis tablice koristi optimistična Procjena istodobnosti provjerava kao zadano ponašanje kada radite s entiteti, za razliku od servisa blob kojem morate izričito odabrati da biste izvršili optimistična Procjena istodobnosti provjere. Razlika između servisa tablice i blob je da možete samo upravljati istodobnosti ponašanja entiteti dok sa servisom blob možete upravljati istodobnosti spremnika i blob-ova.  

Da biste koristili optimistična Procjena istodobnosti i provjerite je li drugi proces promijenjena entitet od dohvaćen iz tablice servisa za pohranu, možete koristiti jedna vrijednost kada tablice usluga vraća entitet. Struktura postupak je na sljedeći način:  

1.  Dohvaćanje entitet iz tablice servisa za pohranu, odgovor sadrži jedna vrijednost koja služi za identifikaciju trenutačnom identifikatoru pridružene entitet u servis za pohranu.
2.  Kada ažurirate entitet, uvrstite jedna vrijednost koji ste primili u koraku 1 u zaglavlju obavezno **If-Match** zahtjeva za slanje servis.
3.  Servis uspoređuje vrijednost e-oznake u zahtjev s trenutnu vrijednost e-oznake entitet.
4.  Ako se razlikuje od e-oznake u zaglavlju obavezno **If-Match** u zahtjevu za trenutnu vrijednost e-oznake entitet, servis vraća 412 pogreške klijentu. To označava klijentu drugi proces je ažurirao entitet jer ga dohvatili klijent.
5.  Ako trenutna vrijednost e-oznake entitet jednak e-oznake u zaglavlju obavezno **If-Match** u zahtjevu za ili zaglavlje **If-Match** sadrži zamjenski znak (*), servis izvodi traženi postupak i ažurira trenutnu vrijednost e-oznake entitet da bi se prikazala da je ažuriran.  

Imajte na umu da za razliku od servisa blob servis tablice zahtijeva klijent da biste dodali zaglavlje **If-Match** u zahtjeva za ažuriranje. Međutim, moguće je da biste nametnuli programa unconditional ažurirati (posljednji writer ima prednost strategije), a zatim zaobišli istodobnosti provjere Ako klijent postavlja zaglavlje **If-Match** zamjenski znak (*) u zahtjevu za.  

U sljedećim C# isječak prikazuje entitet klijenta koji ste prethodno stvorili ili ili dohvatiti pojavljuju njihove adrese e-pošte ažuriraju. Na početni umetnuti ili dohvatiti operacija trgovine jedna vrijednost u objektu klijenta i jer uzorka koristi iste instancu objekta kada se izvršava postupak zamjene, automatski šalje jedna vrijednost natrag na servis tablice, Omogućivanje servisa da biste provjerili kršenja istodobnosti. Ako drugi proces ažurirao entitet u spremište tablica, servis vraća poruku stanja 412 HTTP (preduvjet nije uspio).  Možete preuzeti ovdje punu ogledni: [Upravljanje istodobnosti pomoću Azure prostora za pohranu](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    try
    {
        customer.Email = "updatedEmail@contoso.org";
        TableOperation replaceCustomer = TableOperation.Replace(customer);
        customerTable.Execute(replaceCustomer);
        Console.WriteLine("Replace operation succeeded.");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == 412)
            Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
        else
            throw;
    }  

Da biste izričito onemogućili potvrdite istodobnosti, postavite svojstvo **e-oznake** **zaposlenika** objekta koji želite "*" prije izvršavanje operacija Zamijeni.  

    customer.ETag = "*";  

U sljedećoj su tablici navedene kako koristiti entitet Postupci tablice e-oznake vrijednosti:

| Postupak                | Vraća jedna vrijednost | Potreban je If-Match zahtjev za zaglavlja |
|:-------------------------|:-------------------|:---------------------------------|
| Entiteti upita           | Da                | ne                               |
| Umetanje entitet            | Da                | ne                               |
| Ažuriranje entitet            | Da                | Da                              |
| Spajanje entitet             | Da                | Da                              |
| Brisanje entitet            | ne                 | Da                              |
| Umetanje ili zamjena entitet | Da                | ne                               |
| Umetanje i spajanje entitet   | Da                | ne                               |

Imajte na umu da operacije **Insert ili zamjena entitet** , a **Umetanje ili spajanje entitet** činiti *ne* izvršiti bilo koju provjeru istodobnosti jer ne šalju jedna vrijednost na servis za tablice.  

Općenito govoreći razvojnim inženjerima pomoću tablice potrebno je za optimistična Procjena istodobnosti prilikom razvoja skalabilni aplikacije. Ako pesimistična Procjena zaključavanje nije potrebna, jedan pristup razvojnim inženjerima može potrajati kada je pristup tablice dodijeliti određeni blobova platforme za svaku tablicu i pokušajte da biste preuzeli u Zakup blob-om prije radi u tablici. Taj se način zahtijevaju aplikacije da biste bili sigurni da svi putovi za pristup podacima nabaviti Zakup prije no što radi u tablici. Možete primijetiti i vrijeme minimalne Zakup je to 15 sekundi koji su potrebni pažljivo razmotriti za skalabilnost.  

Dodatne informacije potražite u člancima:  

- [Operacije na entiteti](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Upravljanje istodobnosti u servisu reda čekanja
Jedan scenarij u koje istodobnosti je problem u servisu reda čekanja je koje su više klijenata primanje poruka iz reda. Kada se poruke dohvaća iz reda čekanja, odgovor sadrži poruku i pop potvrda vrijednost, koji je potreban da biste izbrisali poruku. Poruku nije automatski brišu iz reda čekanja, ali nakon što je omogućeno dohvatili, nije vidljiv u drugim klijentima za razdoblje navedeno parametrom visibilitytimeout. Da biste izbrisali poruku nakon bude obrađen, a prije vremena naveden u TimeNextVisible element odgovor, koji se izračunava na temelju vrijednosti parametra visibilitytimeout očekuje klijentskog programa dohvaća podatke u poruku. Vrijednost visibilitytimeout dodaje se vrijeme dohvaća poruku interpolacijom određuje vrijednost TimeNextVisible.  

Usluga reda čekanja nema podrške za optimistična Procjena ili pesimistična Procjena istodobnosti i za klijente razlog obrada poruka dohvaća iz reda potrebno je provjeriti na idempotent način obrade poruka. Zadnji strategije ima prednost writer koristi se za ažuriranje operacija poput SetQueueServiceProperties, SetQueueMetaData, SetQueueACL i UpdateMessage.  

Dodatne informacije potražite u člancima:  

- [Red čekanja servisa REST API-JA](http://msdn.microsoft.com/library/azure/dd179363.aspx)
- [Se poruke](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Upravljanje istodobnosti u servisu datoteka
Servis za datoteka može se pristupiti pomoću dvije različite protokol krajnje točke – SMB i POSTAVITE. Servis REST nema podrške za optimistična Procjena zaključavanje ili pesimistična Procjena zaključavanje i sva ažuriranja slijedit će posljednje strategije ima prednost writer. Klijenti za SMB dostupnosti zajedničke datoteke na raspolaganju su datoteke sustava zaključavanja mehanizme za upravljanje pristup zajedničkim datotekama – uključujući za bicikle pesimistična Procjena zaključavanje. Određuje kada se datoteka otvara SMB klijent, pristup datoteci i zajedničko korištenje načinu rada. Postavljanje mogućnosti za pristup datoteci "Pisanje" ili "Čitanje/pisanje" uz način rada za zajedničko korištenje datoteka s "Nema" rezultirat će datoteke koja se zaključao SMB klijent dok je zatvorite datoteku. Ako je OSTATAK postupka pokušava na datoteci koju SMB klijent je datoteku zaključao servisa REST će vratiti Šifra stanja 409 (Sukob) kod pogreške SharingViolation.  

Kada se datoteka za brisanje SMB klijent otvara označava datoteku kao čekaju brisanje do druge SMB klijent zatvoreni Otvori ručice za tu datoteku. Dok je datoteka označena čekaju brisanje, sve OSTALE operacije na toj datoteci će vratiti Šifra stanja 409 (Sukob) kod pogreške SMBDeletePending. Šifra stanja 404 (nije pronađeno) je vratio Budući da je da SMB klijent za uklanjanje zastavice na čekanju za brisanje prije zatvaranja datoteku. Drugim riječima, Šifra stanja 404 (nije pronađeno) samo očekuje kada datoteku uklonjena. Imajte na umu da dok su datoteke u SMB na čekanju Izbriši stanje, ga ne uvrstiti u rezultatima popis datoteka. Imajte na umu i operacije OSTALE brisanje datoteka i OSTALE brisanje direktorija predaju atomically, a neće rezultirati stanje brisanje na čekanju.  

Dodatne informacije potražite u člancima:  

- [Upravljanje datoteka zaključavanja](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Sažetak i daljnji koraci
Servis za spremište na platformi Microsoft Azure dizajniran potrebama najčešće složene online aplikacije bez prisilno razvojnim programerima ugroziti ili rethink pretpostavke ključa dizajna kao što su dosljednosti istodobnosti i podatke koji su srednjim da je potrebno dodijeliti.  

Gotovi ogledni aplikacije navedeni u ovom blogu:  

- [Upravljanje istodobnosti pomoću Azure prostora za pohranu – probna aplikacija](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Dodatne informacije o Azure prostora za pohranu potražite u članku:  

- [Microsoft Azure prostora za pohranu Početna stranica](https://azure.microsoft.com/services/storage/)
- [Uvod u Azure prostora za pohranu](storage-introduction.md)
- Prostor za pohranu za početak rada za [Blob](storage-dotnet-how-to-use-blobs.md), [tablice](storage-dotnet-how-to-use-tables.md), [redovima](storage-dotnet-how-to-use-queues.md)i [datoteka](storage-dotnet-how-to-use-files.md)
- Prostor za pohranu arhitektura – [Azure prostora za pohranu: na vrlo dostupan prostor za pohranu u Oblaku s istaknuti dosljednost](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
