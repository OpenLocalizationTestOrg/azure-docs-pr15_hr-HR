<properties
    pageTitle="Uvod u prostor za pohranu | Microsoft Azure"
    description="Pregled prostora za pohranu Azure na tvrtke Microsoft online podataka prostor za pohranu u oblaku. Saznajte kako koristiti najbolje rješenje dostupna oblaka za pohranu u aplikacije."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="introduction-to-microsoft-azure-storage"></a>Uvod u Microsoft Azure prostora za pohranu

## <a name="overview"></a>Pregled

Azure prostora za pohranu je rješenje za pohranu oblaka za Moderna aplikacije koje se temelje na rok trajanja, dostupnost i skalabilnost potrebama svoje kupce. Tako da pročitate u ovom članku, razvojnim inženjerima, IT profesionalce i poslovna odluka proizvođači dodatne informacije o:

- Što je Azure prostora za pohranu i kako može potrajati prednost je u oblak, mobilne, poslužitelj i aplikacije za stolna računala
- Koje vrste podataka možete spremiti pomoću servisa Azure prostora za pohranu: bloba podataka (objekt), NoSQL podataka u tablici reda čekanja poruka i datoteka na zajedničko korištenje.
- Kako se upravlja pristupa podacima u spremište Azure
- Kako Azure prostora za pohranu podataka postane durable putem zalihosti ni replikacije
- Gdje Dalje da biste sastavili prvi aplikacija za pohranu za Azure

Brz početak rada s Azure pohranom brzo, potražite u članku [Početak rada s Azure prostor za pohranu u pet minuta](storage-getting-started-guide.md).

Detalje o alatima za biblioteke i ostale resurse za rad s Azure prostora za pohranu potražite u članku [Sljedeće korake](#next-steps) navedene.

## <a name="what-is-azure-storage"></a>Što je prostor za pohranu Azure?

Izračunavanje omogućuje novih scenarija za aplikacije koje je obavezna skalabilni, durable i vrlo dostupan prostor za pohranu za svoje podatke – što je točno Zašto razvio Microsoft Azure prostora za pohranu u oblaku. Osim što za razvojne inženjere za izgradnju veliki aplikacija podržava nove scenarije, za pohranu Azure omogućuje foundation prostora za pohranu za Azure virtualnim strojevima, daljnje testament za njegov odličnim.

Azure prostora za pohranu je massively skalabilni, pa možete spremiti i proces stotine terabajta podataka koje podržava scenarije velikih skupova podataka potrebnih znanstvenom, financijske analize i multimedijskih aplikacija. Ili koji vam omogućuje pohranu manjih količina podatke potrebne za web-mjesto male tvrtke. Kad god se nalaziti vašim potrebama, plaćate samo za pohranjujete podatke. Azure prostora za pohranu trenutno sprema tens trillions jedinstveni korisnički objekata i u rukuje milijune za u sekundi.

Azure prostora za pohranu je elastic, da biste mogli dizajn aplikacije za veću publiku globalni i skaliranje tim aplikacijama po potrebi – uključujući količinu podataka koji su pohranjeni i broj zahtjeva u odnosu na ga. Plaćate samo za koje koristite, a samo kada je koristite.

Azure prostora za pohranu koristi sustav za automatsko particija koja automatski učitavanja-salda podataka koji se temelje na promet. To znači da kao na zahtjeve na vaše aplikacije Povećaj Azure prostora za pohranu automatski dodjeljuje odgovarajuće resurse da bi odgovarao ih.

Azure prostora za pohranu se može pristupiti iz bilo gdje u svijetu, bilo koje vrste aplikacija, hoće li se izvodi u oblaku, na radnoj površini, lokalnog poslužitelja ili na mobilni ili tablet uređaja. Možete koristiti Azure prostora za pohranu u mobilnom scenarijima gdje aplikacija pohranjuje podskup podataka na uređaju i sinkronizira s potpunog skupa podataka pohranjenih u oblaku.

Azure prostora za pohranu podržava klijente pomoću raznih skup operacijskim sustavima (uključujući Windows i Linux) i raznih programskog jezika (uključujući .NET, Java, Node.js, Python, fonetski zapis, PHP i C++ i mobilne programskog jezika) za praktičan razvoj. Azure prostora za pohranu i izlaže izvor podataka putem jednostavne REST API-ji, koje su dostupne za bilo kojeg instaliranog slanja i primanja podataka putem HTTP/HTTPS klijenta.

Prostor za pohranu Azure Premium nudi visokih performansi, najniža Latencija disk podrška za/i intenzivno radnih opterećenja izvodi na virtualnim strojevima sa sustavom Azure. Azure Premium prostor za pohranu, možete priložiti više diskova trajnog podataka virtualnog računala i konfigurirati ih tako da odgovaraju potrebama performansi. Svaki podatkovni disk je sigurnosno tako da na SSD disk u spremište Premium Azure Maksimalna/i performanse. U odjeljku [prostora za pohranu Premium: visokih performansi prostor za pohranu radnih opterećenja Azure virtualnog računala](storage-premium-storage.md) više pojedinosti.

## <a name="introducing-the-azure-storage-services"></a>Uvođenje servisa Azure prostora za pohranu

Azure prostora za pohranu pruža sljedeće četiri usluge: bloba prostora za pohranu, spremište tablica, reda čekanja za pohranu i pohranu datoteka.

- Spremište blobova platforme pohranjuje objekt nestrukturirane podatke. Blob može biti bilo koje vrste teksta ili binarne podatke, kao što su dokument, medijske datoteke ili program za instalaciju aplikacije. Spremište blobova platforme naziva se i spremište objekata.
- Spremište tablica pohranjuje strukturirane skupova podataka. Spremište tablica je izvor podataka ključ atributa NoSQL, koja omogućuje brz razvoj i brzog pristupa velike količine podataka.
- Red čekanja za pohranu nudi pouzdanog poruke za obradu tijeka rada i za komunikaciju između komponenti servise u oblaku.
- Pohrani nudi zajedničkog prostora za pohranu za naslijeđene aplikacije koje se koriste standard SMB protokol. Azure virtualnim strojevima i servise u oblaku možete omogućiti zajedničko korištenje podataka iz datoteke putem aplikacije komponente putem postavljenu dionice, a lokalni aplikacije može pristupiti podacima datoteke u zajedničko korištenje putem servisa datoteke REST API-JA.

Račun za Azure prostora za pohranu je sigurna račun koji omogućuje vam pristup servisima u Azure prostora za pohranu. Račun za pohranu omogućuje jedinstveni prostor za naziv resursa za pohranu. Na donjoj slici prikazano odnose između resursa Azure prostora za pohranu u prostor za pohranu računa:

![Resursi za Azure prostora za pohranu](./media/storage-introduction/storage-concepts.png)

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>Spremište blobova platforme

Korisnici s velikim količinama podataka nestrukturirane objekta u oblak, blobova nudi učinkovit i skalabilni rješenja. Spremište blobova platforme možete koristiti za pohranu sadržaja kao što su:

- Dokumenti
- Društvenih podataka kao što je fotografija, videozapisa, glazba i blogova
- Sigurnosne kopije datoteke, računala, baza podataka i uređaja
- Slike i tekst za web-aplikacije
- Konfiguracija podataka za aplikacije oblaka
- Velikih skupova podataka, kao što su zapisnika i drugih velikih skupova podataka

Svaki blob je organizirane u spremniku. Spremnici nuditi koristan način da biste dodijelili sigurnosnih pravilnika grupe objekata. Prostor za pohranu račun može sadržavati bilo koji broj spremnika i spremnik može sadržavati bilo koji broj blob-ova, najviše 500 TB kapaciteta ograničenje prostora za pohranu računa.  

Bloba prostora za pohranu nudi tri vrste blob-ova, blokirati blob-ova, dodati blob-ova i blob-Ova stranica (diskova).

- Blokiranje blob-ova su optimizirani za strujanje i spremanje oblaka objekata i su dobar izbor za spremanje dokumenata, medijske datoteke, a zatim sigurnosno kopiranje itd.
- Dodavanje blob-ova su slične bloka blob-ova, ali su optimizirani za dodavanje operacije. U upit s dodavanjem blob se može ažurirati samo dodavanjem novi blok do kraja. Dodavanje blob-ova su dobar izbor za scenarije kao što su prijave, gdje nove podatke treba upisati samo na kraju blob-om.
- Blob-Ova stranica su optimizirani za predstavljanje IaaS diskova i podršku slučajni piše, a može biti do 1 TB po veličini. Na mreži Azure virtualnog računala priložiti IaaS disk je VHD pohranjenih kao blob stranice.

Za vrlo velikih skupova podataka odabirati ograničenja mreže prijenos ili preuzimanje podataka sa spremištem blobova putem nerealne žičani možete isporuka tvrdi disk Microsoftu za uvoz i izvoz podataka izravno iz podatkovnog centra. U odjeljku [Korištenje uvoz/izvoz servisa Microsoft Azure za prijenos podataka da biste bloba prostora za pohranu](storage-import-export-service.md).

## <a name="table-storage"></a>Spremište tablica

Moderna aplikacije često potražnje služi za pohranu podataka s veći skalabilnost i fleksibilnost od prethodna generations softverski preduvjeti. Spremište tablica nudi iznimno dostupan, massively skalabilni prostor za pohranu, tako da se aplikacija može automatski promijeniti omjer da bi odgovarao zahtjev za korisnika. Spremište tablica je Microsoftov NoSQL ključ/atribut pohrane – ima schemaless dizajna, čime razlikuje od tradicionalni relacijske baze podataka. S trgovinom u schemaless podataka jednostavno je prilagoditi podataka kao potrebama vaše evolve aplikacije. Spremište tablica je jednostavno je za korištenje, tako da je razvojni inženjeri možete brzo stvoriti aplikacije. Pristup podacima je brz i učinkovit za sve vrste aplikacije.  Spremište tablica je obično znatno nižem položaju trošak od tradicionalni SQL za slične količine podataka.

Spremište tablica je pohrana ključ atribut, što znači da se svaka vrijednost u tablici pohranjen pod nazivom upisani svojstvo. Naziv svojstva može se koristiti za filtriranje i određenim kriterijima odabira. Zbirka svojstava i njihove vrijednosti čine entitet. Budući da je spremište tablica schemaless, dva entiteti u istoj tablici mogu sadržavati različite skupove svojstva, a ta svojstva mogu biti različitih vrsta.

Spremište tablica možete koristiti za pohranu fleksibilne skupova podataka, kao što su korisnički podaci za web-aplikacije, adresare, informacije o uređaju i neku drugu vrstu metapodataka koje je potrebno za uslugu.  Bilo koji broj entiteti mogu pohraniti u tablici, a pohranu račun može sadržavati bilo koji broj tablice, do kapaciteta ograničenje prostora za pohranu računa.

Kao što je blob-ova i redove razvojni inženjeri možete upravljati i access tablica protokoli za pohranu pomoću standardnih REST, no spremište tablica podržava i podskup protokola OData pojednostavljivanje Napredne mogućnosti za slanje upita i omogućavanja JSON i AtomPub (XML temelji) oblici.

Za današnji internetske aplikacije NoSQL baze podataka kao što je spremište tablica nude popularne zamjena za tradicionalni relacijske baze podataka.

## <a name="queue-storage"></a>Red čekanja za pohranu

U dizajniranja aplikacije za skaliranje aplikacije komponente se često samostalne, tako da se mogu mijenjati veličinu neovisno. Red čekanja za pohranu nudi pouzdan razmjenu rješenje za asinkronog komunikaciju između aplikacija komponente, li ih koristite u oblaku, na radnoj površini, lokalnog poslužitelja ili na mobilnom uređaju. Red čekanja za pohranu podržava i upravljanje asinkronog zadataka te izgradnju postupak tijekova rada.

Prostor za pohranu račun može sadržavati bilo koji broj redova. Red čekanja mogu sadržavati bilo koji broj poruke do kapaciteta ograničenje prostora za pohranu računa. Pojedinačne poruke može biti najviše od 64 KB.

## <a name="file-storage"></a>Pohrani

Azure pohrani nudi oblaku SMB zajedničke datoteke, tako da možete migrirati naslijeđene aplikacije koje se temelje na mjestima sa zajedničkim datotekama za Azure brzo i bez skup pisanja. S Azure pohrani, aplikacija u Azure virtualnih računala ili servisa u oblaku možete dostupnosti zajedničko korištenje datoteka u oblaku, baš kao i u aplikaciji za stolna računala mounts uobičajeni SMB Omogući zajedničko korištenje. Bilo koji broj aplikacije komponente možete, a zatim dostupnosti i istovremeno pristupiti na zajedničkom mjestu prostora za pohranu datoteka.

Budući da Omogući zajedničko korištenje prostora za pohranu datoteka je standardni SMB zajedničke datoteke, aplikacije koje rade u Azure može pristupiti podacima u odjeljku zajedničko korištenje putem datoteke moglo API-ji/i. Razvojni inženjeri stoga na raspolaganju svoje postojeće kod i znanja i vještine za migriranje postojeće aplikacije. INFORMATIČKI profesionalci mogu pomoću cmdleta ljuske PowerShell možete stvarati, postavljanje i Upravljanje zajedničkim datotekama za pohranu kao dio Administracija Azure aplikacije.

Pohrani kao što su drugih servisa Azure prostora za pohranu, izlaže REST API-JA za pristup podacima u zajedničke mape. Na lokaciji aplikacije možete nazvati pohrani REST API-JA za pristup podacima u zajedničko korištenje datoteka. Na taj način tvrtki možete odabrati migriranje neke naslijeđene aplikacije za Azure i nastaviti s izvođenjem drugima iz svoje tvrtke ili ustanove. Imajte na umu da Postavljanje zajedničke datoteke moguće je samo za aplikacije koji se izvodi u Azure; Lokalni aplikacije mogu pristupati samo zajedničko korištenje datoteka putem REST API-JA.

Raspodijeljeno aplikacije možete koristiti za pohranu datoteka za pohranu i zajedničko korištenje podataka korisna aplikacije i razvoj i Alati za testiranja. Ako, na primjer, aplikacije mogu pohranjivati konfiguracije i dijagnostičkih podataka kao što su zapisnike, metrike i pad ako ste u datoteci za pohranu zajednički koristiti tako da su dostupni za više virtualnim strojevima ili uloge. Razvojni inženjeri i administratori mogu pohraniti uslužni programi koji su potrebni za sastavljanje i upravljanje aplikacije u zajedničku prostora za pohranu koji je dostupan za sve komponente, umjesto instalacije u svaku virtualnog računala ili uloga instance.

## <a name="access-to-blob-table-queue-and-file-resources"></a>Pristup Blob, tablice, reda čekanja i datoteka resursa

Prema zadanim postavkama, samo vlasnik računa za pohranu možete pristupiti resursa u račun za pohranu. Sigurnost vaših podataka, svaki zahtjev za resurse na vašem računu u odnosu na mora proći. Provjera autentičnosti ovisi o modela zajedničke ključ. Blob-ova moguće je konfigurirati za podršku anonimna provjera autentičnosti.

Račun za pohranu se dodjeljuje dva privatne pristupnih tipki na stvaranje koji se koriste za provjeru autentičnosti. Dvije tipke osigurava da aplikacija postaje dostupan kada redovito Obnovi tipki kao uobičajeno upravljanja ključem sigurnost.

Ako je potrebno da biste korisnicima omogućili kontrolirati pristup resursa za pohranu, a zatim možete stvoriti potpis zajednički pristup. Zajednički pristup potpis (SAS) je token koji se mogu pridruživati URL-a koji omogućuje ovlaštenog pristup resursu za pohranu. Svi koji sadrži sadrže token pristup resursu pokazuje na s dozvolama određuje, vremensko razdoblje da je valjan. Počevši od verzije 2015 04 05, za pohranu Azure podržava dvije vrste potpisa zajednički pristup: SAS servisa i računa SAS.

Servis SAS Delegati pristup resursu u samo jednom servise za pohranu: servis Blob, reda čekanja, tablice ili datoteke.

Račun SAS Delegati pristup resursa u jednu ili više servise za pohranu. Možete dodijeliti pristup razini usluge operacije koje nisu dostupne sa servisom SAS. Možete i delegirati pristup za čitanje, pisanje i brisanje operacije blob spremnika, tablice, redovima i zajedničko korištenje datoteka koji nisu dopušteni sa servisom SAS.

Na kraju, možete odrediti spremnika i njegova blob-ova ili određene blob su dostupne za javni pristup. Kada označite kao spremnik ili blob je javna, svi mogli pročitati anonimno; potreban je provjere autentičnosti.  Javni spremnika i blob-ova korisne su za će se Resursi kao što su mediji i dokumente koji se nalaze na web-mjesta.  Da biste smanjili latenciju mreže za globalnu publiku, možete predmemoriju blob podataka koje koriste web-mjesta s Azure CDN.

Dodatne informacije o potpisima zajednički pristup potražite u članku [Korištenje zajednički pristup potpisa (SAS)](storage-dotnet-shared-access-signature-part-1.md) . Dodatne informacije o siguran pristup računu za pohranu u odjeljku [Upravljanje anonimni pristup za čitanje spremnika i blob-ova](storage-manage-access-to-resources.md) i [provjere autentičnosti za Azure servise za pohranu](https://msdn.microsoft.com/library/azure/dd179428.aspx) .

## <a name="replication-for-durability-and-high-availability"></a>Replikacija rok trajanja i visoke dostupnosti

Podatke na računu sustava Microsoft Azure prostora za pohranu uvijek replicirati da bi se rok trajanja i visoke dostupnosti. Replikacija kopira podatke, u centru za iste podatke ili na drugi podatkovnog centra, ovisno o tome koju mogućnost replikacije odaberete. Replikacija štiti podatke, a zadržava aplikacije gore-vremenom slučaju tranzitne hardverske pogreške. Ako vaši podaci je replicirati na drugom podatkovnog centra, koje i štiti podatke na temelju Katastrofalna pogreška u primarnom mjestu.

Replikacija osigurava ispunjava li vaš račun za pohranu [Razini usluge ugovor (SLA) za pohranu](https://azure.microsoft.com/support/legal/sla/storage/) čak i in the face of pogreške. Potražite u članku jamčiti SLA informacije o Azure prostora za pohranu za rok trajanja i dostupnost. 

Kada stvorite račun za pohranu, možete odabrati jednu od sljedećih mogućnosti replikacije:  

- **Lokalno suvišnih prostor za pohranu (LRS).** Lokalno suvišnih prostora za pohranu održava tri kopije vaših podataka. LRS je replicirati triput unutar jednog podatkovnog centra u jednom području. LRS štiti podatke iz normalni hardverske pogreške, ali ne zbog pogreške u centru za jedan podatkovni.  

    LRS je ponuđen uz popust. Maksimalni rok trajanja, preporučujemo da koristite zemlj suvišnih prostor za pohranu, opisane ispod.


- **Zone suvišne prostora za pohranu (ZRS).** Zone suvišnih prostora za pohranu održava tri kopije vaših podataka. ZRS replicirati triput preko dvije do tri funkcije unutar jedno područje ili preko dva područja koja omogućuje veći rok trajanja od LRS. ZRS osigurava podataka durable unutar jedno područje.  

    ZRS pruža više razine rok trajanja od LRS; Međutim, maksimalna rok trajanja preporučujemo da koristite zemlj suvišnih prostor za pohranu, opisanim.  

    > [AZURE.NOTE] ZRS trenutno je dostupna samo za blokiranje blob-ova, a samo je podržana za verzije 2014. 02 14 i noviji.
    >
    > Kada ste stvorili račun za pohranu i odabrali ZRS, ne možete koristiti neku drugu vrstu replikacije, pretvoriti ili obrnuto.

- **Zemlj suvišnih prostora za pohranu (GRS)**. GRS održava šest kopije vaših podataka. GRS, podataka je replicirati triput unutar primarni područja i i ponavlja se triput u sekundarni regiji stotine milja izvan primarni regiju, koja omogućuje najvišu razinu rok trajanja. U slučaju pogreške pri primarni regija, za pohranu Azure će prebacivanje s područjem sekundarne. GRS osigurava podataka durable u dva zasebna područja.

    Informacije o primarnih i sekundarnih uparivanja po regiji potražite u članku [Azure područja](https://azure.microsoft.com/regions/).

- **Pristup za čitanje zemlj suvišnih prostora za pohranu (RA GRS)**. Pristup za čitanje zemlj suvišnih prostora za pohranu replicira podataka na sekundarnom zemljopisni, a omogućuje pristup čitanju, pristup podacima sekundarne mjestu. Pristup za čitanje zemlj suvišne prostora za pohranu omogućuje pristup vašim podacima iz primarnih ili sekundarne mjesto za slučaj da jedno mjesto postane nedostupan. Prostor za pohranu zemlj suvišnih pristup za čitanje je zadana mogućnost za vaš račun za pohranu po zadanom kad ga stvorite. 

    > [AZURE.IMPORTANT] Možete promijeniti kako podataka je replicirati nakon stvaranja računa za pohranu, osim ako ne navedete ZRS prilikom stvaranja računa. Međutim, imajte na umu da mogu uzrokovati o prijenosu dodatne podatke o jednokratne trošak ako se prebacite iz LRS GRS ili RA GRS.

Dodatne informacije o mogućnostima replikacije prostora za pohranu potražite u članku [replikacije Azure prostora za pohranu](storage-redundancy.md) .

Informacije o cijenama za replikaciju račun za pohranu, potražite u članku [Cijene za Azure prostora za pohranu](https://azure.microsoft.com/pricing/details/storage/). Dodatne informacije o servisima na koje su dostupne na svakom području potražite u članku [Azure područja](https://azure.microsoft.com/regions/#services) .

Arhitektonski detalje o rok trajanja s Azure prostora za pohranu potražite u članku [SOSP papira - Azure prostora za pohranu: odgovora iznimno dostupne za pohranu u Oblaku s Strong dosljednost](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).


## <a name="transferring-data-to-and-from-azure-storage"></a>Prijenos podataka i iz Azure prostora za pohranu

Pomoćnog programa naredbenog retka AzCopy možete koristiti da biste kopirali blob, datoteke i podatke iz tablice u račun za pohranu ili putem računa za pohranu. Dodatne informacije potražite [prenijeti podatke s AzCopy naredbenog retka uslužni](storage-use-azcopy.md) .

AzCopy ugrađeni pri vrhu [Azure podataka premještanje biblioteke](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)koja je trenutno dostupno u pretpregledu.

Servis za uvoz/izvoz Azure omogućuje blob podatke za uvoz i izvoz blob podataka s računa za pohranu putem tvrdi disk na disku šalje u Centar za Azure podataka. Dodatne informacije o servisu za uvoz/izvoz potražite u članku [korištenje usluge Microsoft Azure uvoz/izvoz prijenos podataka sa spremištem blobova](storage-import-export-service.md).

## <a name="pricing"></a>Cijene

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>API-ji za pohranu, biblioteke i Alati

Azure resursa za pohranu možete pristupiti bilo koji jezik koji olakšavaju zahtjeva za HTTP/HTTPS. Uz to, za pohranu Azure nudi programiranje biblioteke za nekoliko Popularni jezika. Te biblioteke Pojednostavnite mnogo aspektima rada sa spremištem Azure rukovanje detalje kao što je slika i asinkronog poziva, grupnog slanja promjena postupci upravljanja iznimku, automatsko ponovne pokušaje, radu ponašanje tako naprijed. Biblioteka su trenutno dostupne za sljedeće jezike i platforme s drugim korisnicima u kanalu:

### <a name="azure-storage-data-services"></a>Azure prostora za pohranu podataka usluge

- [Servise za pohranu REST API-JA](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Prostor za pohranu klijenta biblioteka za .NET, Windows Phone i Windows Runtime](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Prostor za pohranu klijenta biblioteka za C++](https://github.com/Azure/azure-storage-cpp)
- [Prostor za pohranu klijenta biblioteke za Java/Android](/develop/java/)
- [Prostor za pohranu klijenta biblioteka za Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [Prostor za pohranu klijenta biblioteka za PHP](/develop/php/)
- [Prostor za pohranu klijenta biblioteka za Ruby](/develop/ruby/)
- [Prostor za pohranu klijenta biblioteka za Python](/develop/python/)
- [Cmdleti za pohranu za PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### <a name="azure-storage-management-services"></a>Azure prostora za pohranu Management Services

- [Prostor za pohranu resursa davatelja REST API Reference](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [Biblioteka davatelj usluga za pohranu resursa klijenta za .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [Cmdleti za davatelja resursa za pohranu za PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [Prostor za pohranu servisa upravljanje REST API-JA (Classic)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Premještanje servise za Azure pohranu podataka

- [Prostor za pohranu uvoz/izvoz servisa REST API-JA](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [Prostor za pohranu podataka premještanja klijentska biblioteka za .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Alati i uslužni programi

- [Explorer Azure prostora za pohranu](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
- [Azure prostora za pohranu klijentskih alata](storage-explorers.md)
- [Alati i Azure SDK-ovi](https://azure.microsoft.com/tools/)
- [Emulator Azure prostora za pohranu](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure PowerShell](../powershell-install-configure.md)
- [AzCopy pomoćnog programa naredbenog retka](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o Azure prostora za pohranu, proučite ove resurse:

### <a name="documentation"></a>Dokumentacija

- [Dokumentacija Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/)

### <a name="for-administrators"></a>Za administratore

- [Azure PowerShell pomoću Azure prostora za pohranu](storage-powershell-guide-full.md)
- [Azure EŽA pomoću Azure prostora za pohranu](storage-azure-cli.md)

### <a name="for-net-developers"></a>Za razvojne inženjere za .NET

- [Početak rada s Azure blobova pomoću .NET](storage-dotnet-how-to-use-blobs.md)
- [Početak rada sa spremištem tablica Azure pomoću .NET](storage-dotnet-how-to-use-tables.md)
- [Početak rada s pohranu reda čekanja Azure pomoću .NET](storage-dotnet-how-to-use-queues.md)
- [Početak rada s Azure pohrani u sustavu Windows](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Za razvojne inženjere Java/Android

- [Upute za korištenje spremišta blobova iz Java](storage-java-how-to-use-blob-storage.md)
- [Upute za korištenje spremišta tablica iz Java](storage-java-how-to-use-table-storage.md)
- [Kako koristiti reda čekanja za pohranu iz Java](storage-java-how-to-use-queue-storage.md)
- [Kako koristiti za pohranu datoteka iz Java](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Za razvojne inženjere Node.js

- [Upute za korištenje spremišta blobova s Node.js](storage-nodejs-how-to-use-blob-storage.md)
- [Upute za korištenje spremišta tablica iz Node.js](storage-nodejs-how-to-use-table-storage.md)
- [Kako koristiti reda čekanja za pohranu iz Node.js](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>Za razvojne inženjere i

- [Upute za korištenje spremišta blobova iz PHP](storage-php-how-to-use-blobs.md)
- [Upute za korištenje spremišta tablica iz PHP](storage-php-how-to-use-table-storage.md)
- [Kako koristiti reda čekanja za pohranu iz PHP](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Za razvojne inženjere za Ruby

- [Upute za korištenje spremišta blobova iz Ruby](storage-ruby-how-to-use-blob-storage.md)
- [Upute za korištenje spremišta tablica iz Ruby](storage-ruby-how-to-use-table-storage.md)
- [Kako koristiti reda čekanja za pohranu iz Ruby](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Za razvojne inženjere Python

- [Upute za korištenje spremišta blobova iz Python](storage-python-how-to-use-blob-storage.md)
- [Upute za korištenje spremišta tablica iz Python](storage-python-how-to-use-table-storage.md)
- [Kako koristiti reda čekanja za pohranu iz Python](storage-python-how-to-use-queue-storage.md)
- [Kako koristiti za pohranu datoteka iz Python](storage-python-how-to-use-file-storage.md)
