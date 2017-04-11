
<properties 
   pageTitle="Bilješke o izdanju Azure SDK za .NET 2.7 i .NET 2.7.1" 
   description="Bilješke o izdanju Azure SDK za .NET 2.7 i .NET 2.7.1" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Bilješke o izdanju Azure SDK za .NET 2.7 i .NET 2.7.1

##<a name="overview"></a>Pregled

Ovaj dokument sadrži napomene u za Azure SDK za .NET 2.7 izdanje. 

Napomene u dokument i sadrži za Azure SDK za .NET 2.7.1 otpustite.

Azure SDK 2.7 podržano je samo u Visual Studio 2015 i Visual Studio 2013. [Azure SDK 2,6](https://azure.microsoft.com/downloads/) je zadnji podržane SDK za Visual Studio 2012.

Detaljne informacije o ovom izdanju potražite u članku [Azure SDK 2.7 objava objavljivati](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) i [objavljivanje objava Azure SDK 2.7.1](http://go.microsoft.com/fwlink/?LinkId=623850).

##<a name="azure-sdk-for-net-27"></a>Azure SDK za .NET 2.7

###<a name="sign-in-improvements-for-visual-studio-2015"></a>Prijavite se u poboljšanja za Visual Studio 2015.

Azure 2.7 SDK za Visual Studio 2015 podržava nove značajke upravljanja identiteta u Visual Studio 2015.  To obuhvaća podršku za račune pristupa Azure putem kontrolu pristupa temelji uloge, davatelji rješenja za oblaka, DreamSpark i druge vrste računa i pretplate.

Poboljšanja prijavu u sklopu Azure SDK 2.7 dostupni su samo u Visual Studio 2015. Podrška za Visual Studio 2013 je sve obuhvaćeno Azure SDK 2.7.1.


###<a name="mobile-sdk"></a>Mobilni SDK

Ažuriranje **Aplikacije Mobile** predložaka u skladu s vizualnim najnovijeg [paketa NuGet](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) i postavljanje postupak.

###<a name="service-bus"></a>Servis Bus 

Općenito popravci programskih pogrešaka i poboljšanja. Za pojedinosti o ažuriranjima te značajke, pogledajte napomene od najnovijih [NuGet Bus servisa](http://www.nuget.org/packages/WindowsAzure.ServiceBus/).

###<a name="hdinsight-tools"></a>Alati za HDInsight 

U ovom izdanju nastali sljedeća ažuriranja. Ta su ažuriranja u pretpregledu. Dodatne informacije potražite u članku [ovom blogu](http://go.microsoft.com/fwlink/?LinkId=619108).

- Grafikoni grozd za grozd na Tez poslove
- Puna podrška vrste Hive IntelliSense DML
- Svinja predložak podrške
- Predlošci oluja za Azure servise

####<a name="breaking-changes"></a>Najnovije promjene

- Stari **oluja** projekt mora biti nadograđena kada koristite ovu verziju alata. Dodatne informacije potražite u članku [ovom blogu](http://go.microsoft.com/fwlink/?LinkId=619108).
- Vizualni Web Studio Express više nisu podržane. Dodatne informacije potražite u članku [ovom blogu](http://go.microsoft.com/fwlink/?LinkId=619108).

###<a name="azure-app-service-tools"></a>Alati za aplikacije servisa za Azure

U ovom izdanju sljedeća ažuriranja su izvršene Extensions alata za Web. Dodatne informacije potražite u članku [ovom](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blogu. 

- Podrška za DreamSpark računi dodali
- Potpuna Azure Alati promjene podržava novi Azure resursa upravljanje API-ji
- Podrška za aplikacije servisa za Azure dodaje [Explorer oblaka](#cloud_explorer)

####<a name="known-issues"></a>Poznati problemi

Web App implementacije vremensko razdoblje čvorove ne prikazuju se u čvor slobodnih u programu Explorer poslužitelja, a web-aplikacije implementacije vremensko razdoblje podređene čvorove ne učitavaju se u odjeljku Explorer oblaka. Taj se problem riješen je i pripremiti za novo izdanje SDK. 


###<a name="cloud_explorer"></a>Oblak Explorer za Visual Studio 2015.

Azure SDK 2.7 sadrži Explorer oblaka za Visual Studio 2015 koji omogućuje prikaz Azure resursa, njihova svojstva i izvođenje akcija ključa za razvojne inženjere iz programa Visual Studio. 

Oblak explorer podržava sljedeće:

- Grupa resursa i resursa vrsta prikaza Azure resursa 
- Traženje resursa po imenu (dostupno u prikazu vrste resursa)
- Podrška za pretplate i resursi koji imaju ulogama temelji pristup kontrola (RBAC) primjenjuje 
- Integrirano ploča akcije koje prikazuju akcije za razvojne inženjere odnosila specifične za resurse. Na primjer: prilaganje udaljene ispravljanje pogrešaka za virtualnim strojevima stvorene pomoću stog resursima pogledati Dijagnostika podataka virtualnim strojevima itd.
- Integrirano ploča svojstva koje prikazuje svojstva za razvojne inženjere odnosila često potrebno tijekom razvojni i testiranje 
- Brzo prebacivanje račun koji želite koristiti prilikom prebrojavanje resursi (pomoću naredbe postavke na alatnoj traci) 
- Filtriranje pretplate za korištenje prilikom prebrojavanje resursi (pomoću naredbe postavke na alatnoj traci) 
- Precizno veze Azure portal za upravljanje resursa i grupa resursa 
 
 
###<a name="azure-resource-manager-tools"></a>Alati za Azure Voditelj resursa 

Alati za upravljanje resursima Azure ažurirane za rad s uloga temelji pristup kontrola (RBAC) i nove vrste pretplate.  Uključene te promjene je mogućnost korištenja nove račune za pohranu, osim klasični prostora za pohranu za pohranu artefakte tijekom implementacije.  

Ako koristite za grupu resursa Azure projekt iz prethodne verzije SDK SDK 2.7, novi implementacijsku skriptu potreban je za implementaciju pomoću novog računa za pohranu umjesto klasični prostora za pohranu.  Zatražit će se prije promjena u projekt da biste dodali novu skriptu.  Stari skripte će se preimenovati i ćete morati ručno izmijenite sve nove skripte.
 
 
###<a name="storage-explorer-tools"></a>Alati za pohranu Explorer 

- Podrška za prikaz dodavanje blob-ova. Dodatne informacije u [ovom blogu](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
- Podrška za prikaz račune za pohranu Premium putem poslužitelja Explorer. Poslužitelj Explorer samo prikazat će stranice blob-ova za račune za pohranu premium kao što su jedina podržana vrsta račune za premium prostora za pohranu.

###<a name="azure-data-factory-tools-for-visual-studio"></a>Azure Data tvorničke Tools za Visual Studio 

Uvod u **Azure Data tvorničke Tools** za Visual Studio. U nastavku su omogućeni značajke. Pročitajte dodatne informacije na [ovom blogu](http://go.microsoft.com/fwlink/?LinkId=617530) .

- **Stvaranje predloška koji se temelje**: Odaberite koristi cased koji se temelje predložaka, predložaka za premještanje podataka ili obradu podataka predložaka implementacije rješenja za integraciju programa završetka do kraja podataka i početak rada stjecanja brzo s tvorničke podataka. 
- **Integracija s preglednika rješenja za stvaranje i implementacija entiteti tvorničke podataka**: Stvaranje i implementaciju kanali i povezani entiteti Visual Studio projektima. 
- **Integracija s dijagrama prikaz za vizualni interakcije tijekom za izradu**: vizualno autor kanali i skupova podataka Word iz prikaza dijagrama. 
- **Integracija s programom explorer poslužitelja za pregledavanje i interakcije s već distribuiranih entiteti**: korištenje Explorer poslužitelja factories Pregledaj već ste implementirali podataka i odgovarajuće entiteti. Uvoz distribuiranih podataka tvorničke ili bilo koju entitet (kanal, servis za povezane skupove podataka) u projektu. 
- **Uređivanje JSON s Provjera valjanosti sheme i obogaćeni intellisense**: učinkovito konfigurirati i uređivanje dokumenata JSON entiteti tvorničke podataka s obogaćenim intellisense i sheme provjere valjanosti 
- **Objavljivanje na više okruženje**: objavljivanje autor stvaranje zasebnom config datoteka za svako okruženje kanali razvojni, testiranje ili grupa okruženje.
- **Svinja, grozd i .net na temelju podrška za obradu podataka**: podrška za Svinja i vrste Hive skripte u programu project tvorničke podataka. Podrška za pozivanju C# projekta za upravljanje .net aktivnosti.

##<a name="azure-sdk-for-net-271"></a>Azure SDK za .NET 2.7.1

Sljedeći odjeljak sadrži ažuriranja uvedene s Azure SDK za .NET 2.7.1 izdanja.
###<a name="hdinsight-tools"></a>Alati za HDInsight 

Detaljnije objašnjenje o ažuriranjima Alati servisa HDInsight potražite u članku [ovom blogu](http://go.microsoft.com/fwlink/?LinkId=623831).

- Prikaz Operator grozd posla (je nova značajka)

    Pomoću kojih se objašnjava upit grozd bolje, značajka vrste Hive prikaz Operator dodan. Da biste vidjeli sve operatore unutar vrh, dvaput pritisnite vrhovi grafikonu posao. Da biste pogledali dodatne informacije o određenom operatora, postavite pokazivač miša taj operator.
- Oznaka grozd pogreške (je nova značajka)

    Da biste omogućili prikaz gramatičke pogreške trenutačno, značajka vrste Hive pogreške oznake dodan. Osim toga, su Poboljšana poruke o pogreškama i sada možete vidjeti detaljne gramatičke pogreške trenutačno (do ovo izdanje morali slanje skriptu grozd klaster i pričekajte neko vrijeme prije nego prijeđete detalje o poruku o pogrešci).  
- Graph topologije oluja (je nova značajka)

    Vizualizacija važno je kada želite vidjeti ako topologiji funkcioniraju kako želite. U ovom izdanju dodali smo vizualizacije za oluja grafikona. Možete vizualizirati važne metriku za topologiji (na primjer, boju označava vremenske prognoze određene munje "zauzet" ili ne). Možete dvostruko pritisnuti munje/Spout da biste vidjeli dodatne detalje.

- Podrška za HDInsight klastere koje su stvorene na portalu za Azure (pogrešku fix)

    Visual Studio sada možete koristiti za prikaz i slanje zadatke za sve svoje HDInsight klastere bez obzira na to gdje su stvoreni klaster.

- Dodatnu podršku IntelliSense & brže metapodataka vrste Hive učitavanja (poboljšanje)

    Ne možemo poboljšani na IntelliSense dodavanjem više korisnika neslužbeni prijedloga. Ako, na primjer, pseudonim tablice možete sada također se predlagati u IntelliSense tako da možete jednostavnije pisanje upita. Osim toga, ne možemo poboljšani metapodataka grozd učitavanja tako da samo će trajati nekoliko sekundi da biste dobili popis svih baze podataka, tablica i stupaca na metastore grozd.

Detaljnije objašnjenje o ažuriranjima Alati servisa HDInsight potražite u članku [ovom blogu](http://go.microsoft.com/fwlink/?LinkId=623831).

###<a name="improvements-in-visual-studio-2013"></a>Poboljšanja u Visual Studio 2013

- Azure SDK 2.7.1 omogućuje Visual Studio 2013 da biste pristupili Azure računi i pretplate putem kontrolu pristupa temelji uloge, davatelji rješenja za oblak i Dreamspark.
- S Azure SDK 2.7.1 novi prozor alata oblaka Explorer sada dostupan je i u Visual Studio 2013.

###<a name="known-issues"></a>Poznati problemi

Instalacije SDK 2,6 Azure ili 2.7.1 za Visual Studio zajednice 2013 na u ne - engleski OS prikazat će se upozorenje na engleskom i engleskog resursima Visual Studio mogu se razlikovati. Ovo će se upozorenje može bez opasnosti nehotice. Samo će dogoditi ako na računalu ne sadrže prethodnu instalaciju sustava 2013 zajednice za Visual Studio i instalirate SDK na u ne - engleski OS. Upozorenje prikazuje se nakon jezični paket primjenjuje RTM resursi za Visual Studio, ali prije nego što se primjenjuje ažuriranjem 4. Zatvaranjem upozorenje da bi jezični paket koji će nastaviti i dovršiti primjene ažuriranje 4 verzija sadržaj paketa jezičnog.

Projekti LightSwitch nisu compatibile s ovom izdanju. Bit će taj se problem riješi s novo izdanje SDK.

##<a name="also-see"></a>Vidi također
[Azure SDK 2.7.1 objavu objava](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure objavu SDK 2.7 objava](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Podrška i njegovo povlačenje iz upotrebe informacije za Azure SDK za .NET i API-](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
