<properties 
    pageTitle="Kraj do kraja rješavanje problema pomoću Azure metrike pohrane i zapisivanje, AzCopy i alata za analizu poruke | Microsoft Azure" 
    description="Vodič takvi kraj do kraja otklanjanje poteškoća s analize Azure prostora za pohranu, AzCopy i Microsoftov analizator poruke" 
    services="storage" 
    documentationCenter="dotnet" 
    authors="robinsh" 
    manager="carmonm"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/03/2016" 
    ms.author="robinsh"/>

# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Otklanjanje poteškoća završetka do kraja pomoću Azure metrike pohrane i zapisivanje, AzCopy i alata za analizu poruke 

[AZURE.INCLUDE [storage-selector-portal-e2e-troubleshooting](../../includes/storage-selector-portal-e2e-troubleshooting.md)]

## <a name="overview"></a>Pregled

Dijagnosticiranje i otklanjanje poteškoća je ključa znanja i vještine za stvaranje i klijentske aplikacije s spremište na platformi Microsoft Azure za podršku. Zbog raspodijeljeno prirode Azure aplikacije dijagnosticiranje i otklanjanje pogrešaka i probleme s performansama možda složenije od u tradicionalni okruženja.

U ovom ćete praktičnom vodiču smo će demonstrirati prepoznavanje klijent određene pogreške koje se može utjecati na performanse i rješavanje problema te pogreške od kraja do kraja pomoću alata za koje ste dobili od Microsofta i za pohranu Azure, da biste optimizirali klijentske aplikacije. 

Pomoću ovog praktičnog vodiča pruža stjecanja istraživanje scenarija otklanjanje poteškoća do kraja do kraja. Detaljnije konceptualni vodič za otklanjanje poteškoća s aplikacijama Azure prostora za pohranu, u odjeljku [Monitor, dijagnosticiranje, i otklanjanje poteškoća s spremište na platformi Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md). 

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Alati za otklanjanje poteškoća s aplikacijama za pohranu Azure

Da biste otklonili poteškoće s klijentskim aplikacijama pomoću spremište na platformi Microsoft Azure, koristite kombinaciju Alati da biste odredili kada problem došlo je do i što može biti uzrok problema. Ti Alati obuhvaćaju sljedeće:

- **Analitički azure prostora za pohranu**. [Azure prostora za pohranu Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) nudi metriku i za pohranu Azure zapisivanje.
    - **Prostor za pohranu metrika** prati metriku transakcije i kapacitet metriku za vaš račun za pohranu. Korištenje metriku, možete odrediti kako se vaše aplikacije izvršava prema raznih različite mjere. Dodatne informacije o vrstama metriku pratiti Analytics za pohranu potražite u članku [Prostora za pohranu analize metriku tablice shemu](http://msdn.microsoft.com/library/azure/hh343264.aspx) . 

    - **Prijava za pohranu** prijavi svaki zahtjev za Azure prostora za pohranu servisa da biste se prijavili na strani poslužitelja. Zapisnik prati detaljne podatke za svaki zahtjev, uključujući operacije obavljene, status postupak i Latencija informacije. Dodatne informacije o podacima zahtjeva i odgovora Analytics za pohranu napisan u zapisnicima potražite u članku [Spremanje analize zapisnika oblikovanje](http://msdn.microsoft.com/library/azure/hh343259.aspx) .

> [AZURE.NOTE] Prostor za pohranu računi s vrstom replikacije od zona suvišne prostora za pohranu (ZRS) nemaju metriku ili mogućnost bilježenje omogućeno trenutno. 

- **Azure klasični Portal**. Možete konfigurirati metriku i zapisivanje za vaš račun za pohranu za [Azure klasični Portal](https://manage.windowsazure.com). Možete prikaz grafikona i grafova koji pokazuju kako se izvršava aplikacije s vremenom i konfiguriranje upozorenja koja vas obavještava ako aplikacija drugačije nego je očekivano navedeni metrike izvodi. 
    
    Dodatne informacije o konfiguriranju nadzor na portalu klasičnih Azure potražite u članku [monitora na račun za pohranu na portalu za Azure](storage-monitor-storage-account.md) .

- **AzCopy**. Zapisnici poslužitelja za pohranu Azure spremaju se kao BLOB-ova, da biste mogli koristiti AzCopy da biste kopirali zapisnika blob-ova lokalni direktorij za analizu pomoću alata za analizu Microsoft poruke. Potražite u članku [prijenos podataka s Utility za naredbenog retka AzCopy](storage-use-azcopy.md) dodatne informacije o AzCopy.

- **Alat za analizu poruka Microsoft**. Alat za analizu poruke alat je koji troši datoteke zapisnika i prikazuje podaci iz zapisnika u vizualnom obliku koji olakšava filtriranje, pretraživanje i podaci iz zapisnika grupe u korisne skupove koje možete koristiti da biste analizirali pogrešaka i probleme s performansama. Dodatne informacije o alata za analizu poruke potražite u članku [Poruke alata za analizu operacijski vodič za Microsoft](http://technet.microsoft.com/library/jj649776.aspx) .

## <a name="about-the-sample-scenario"></a>O uzorak scenarija

Za ovaj vodič smo ćete pregledajte scenarij gdje se malo postotka uspjeh stopu za aplikaciju koja se poziva Azure prostora za pohranu metrika pohrane Azure označava. Metriku stopa niskog postotka uspjeh (prikazan kao **PercentSuccess** na portalu klasičnih Azure i u tablicama metriku) prati operacije koje uspjela, ali koje vratili HTTP kod stanja veće od 299. Na popisu datoteke zapisnika poslužiteljsko prostora za pohranu te operacije bilježe se sa statusom transakcije **ClientOtherErrors**. Dodatne pojedinosti o metriku niskog postotka uspjeh potražite u članku [metriku prikaz malo PercentSuccess ili analize zapisnika stavke imaju operacije sa statusom transakcije ClientOtherErrors](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure prostora za pohranu operacije može vratiti HTTP kodovima stanja veći od 299 kao dio njihove normalni funkcionalnosti. No te pogreške u nekim slučajevima upućuju možda moći optimiziranje klijentska aplikacija za bolje performanse. 

U ovom scenariju smo ćete uzmite u obzir u malom postotka uspješnosti se ništa ispod 100%. Možete metričkim razinu, no prema svojim potrebama. Preporučujemo da tijekom testiranja aplikacije, uspostavljate osnovne odstupanje za vaše metriku ključnog učinka. Na primjer, mogu odrediti utemeljen na broju testiranja, da bi aplikacija trebali imati na dosljedan postotka uspješnosti 90% ili 85%. Ako vaši podaci metriku pokazuje da je aplikacija deviating iz taj broj, a zatim istražiti što uzrokuje povećanja. 

Za naše uzorak scenarija nakon što smo uspostavite nalazi li se stopa metriku postotka uspjeh ispod 100%, ne možemo će provjeriti zapisnike da biste pronašli pogreške koje povezivanje metriku i pomoću njih možete otkriti što uzrokuje donjem postotka uspješnosti. Ne možemo ćete pogledajte izričito pogreške u rasponu od 400. Zatim ćemo ćete pobliže Istražite 404 pogreške (nema).

### <a name="some-causes-of-400-range-errors"></a>Nekoliko razloga 400 raspon pogrešaka

Primjere u nastavku prikazuje navedeni neki 400 raspon pogrešaka zahtjeve od spremište blobova platforme Azure i njihovi mogući uzroci. Bilo koji od te pogreške, kao i pogreške u rasponu od 300 i 500 raspon mogu pridonijeti niskog postotka uspješnosti. 

Imajte na umu da donje popise far from dovršeno. Pogledajte [Stanje i šifre pogreške](http://msdn.microsoft.com/library/azure/dd179382.aspx) detalje o općenite pogreške Azure prostora za pohranu i pogreške specifične za svaki od servise za pohranu.

**Primjeri status kod 404 (nema)**

Pojavljuje se kada operacija čitanja protiv spremnik ili blob nije uspjela zbog blob ili spremnik nije pronađen.

- Pojavljuje se ako spremnik ili blob je izbrisao drugi klijent prije zahtjev. 
- Pojavljuje se ako koristite API poziva koji stvara spremnik ili blob nakon provjerava postoji li. API-ji CreateIfNotExists upućivanje poziva GLAVE prvi put da biste provjerili postojanje parametra spremnik ili blob; Ako ne postoji, vraća se o pogrešci 404, a zatim drugi STAVI poziv Napravi pisanje container ili blob.

**Status kod 409 Primjeri (Sukob)**

- Pojavljuje se ako koristite API za stvaranje da biste stvorili novi spremnik ili blob, bez prvo provjerava postojanje i spremnik ili blob s tim nazivom već postoji. 
- Događa prilikom brisanja spremnik i pokušate stvoriti novi spremnik s istim nazivom prije dovršetka postupak brisanja.
- Pojavljuje se ako navedete u Zakup spremnik ili blob i već postoji u Zakup prezentacija.
 
**Šifra stanja 412 (preduvjet nije uspjelo) Primjeri**

- Pojavljuje se kada nije zadovoljen uvjet naveden uvjetno zaglavlja.
- Pojavljuje se kada navedeni ID Zakup odgovara ID-u Zakup spremnik ili blob.

## <a name="generate-log-files-for-analysis"></a>Generiranje zapisničke datoteke za analizu

U ovom ćete praktičnom vodiču ćemo pomoću alata za analizu poruke da biste radili s tri različite vrste datoteka zapisnika, iako može odabrati da biste radili s nešto od sljedećeg:

- U **zapisniku**, koja je stvorena prilikom omogućivanja zapisivanja Azure prostora za pohranu. U zapisniku sadrži podatke o svaka operacija naziva u odnosu na jedan od servise za pohranu Azure - blob, reda čekanja, tablice i datoteke. U zapisniku upućuje na to koju operaciju stranice naziva se i koje Šifra stanja je vraćena, kao i druge detalje o zahtjeva i odgovora.
- **.NET klijent zapisnika**, koji je stvorio Kada omogućite zapisivanje na klijentskoj strani iz aplikacije .NET. Zapisnik klijenta sadrži detaljne informacije o kako klijent Priprema zahtjev i prima i obrađuje odgovor. 
- **Zapisnik praćenja mreže HTTP**, koji prikuplja podatke na HTTP/HTTPS zahtjeva i odgovora podataka, uključujući za operacije na temelju Azure prostora za pohranu. U ovom ćete praktičnom vodiču smo ćete generirati mrežnog prometa putem alata za analizu poruke.

### <a name="configure-server-side-logging-and-metrics"></a>Konfiguriranje poslužiteljsko zapisivanje i mjerenja

Najprije ćemo morat ćete konfigurirati zapisivanje Azure prostora za pohranu i metrike tako da se imamo podatke iz klijentska aplikacija za analizu. Zapisivanje i metrike možete konfigurirati na razne načine - putem [Klasične Portal Azure](https://manage.windowsazure.com)pomoću komponente PowerShell ili programski. Potražite u članku [Omogućivanje metrike pohrane i prikaz podataka metriku](http://msdn.microsoft.com/library/azure/dn782843.aspx) i [Omogućivanje prijave za pohranu i pristup podaci iz zapisnika](http://msdn.microsoft.com/library/azure/dn782840.aspx) detalje o konfiguriranju zapisivanje i metrike.

**Putem portala za Azure klasičnih**

Da biste konfigurirali zapisnika i metriku za vaš račun za pohranu pomoću portala, slijedite upute na [Monitor na račun za pohranu na portalu za Azure](storage-monitor-storage-account.md).

> [AZURE.NOTE] Nije moguće postaviti minute mjernih podataka pomoću portala za klasični Azure. No preporučujemo da ih postaviti za potrebe ovog praktičnog vodiča i istražuje probleme s performansama s aplikacijom. Možete postaviti minute metriku pomoću komponente PowerShell kao što je prikazano u nastavku ili programski ili putem portala za klasični Azure.
>
> Imajte na umu portalu klasični Azure ne može prikazati minute metrika, svaki sat metriku. 

**PowerShell**

Početak rada sa servisom PowerShell za Azure, potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

1. Pomoću cmdleta [Dodaj AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) da biste dodali Azure korisnički račun u prozor PowerShell:

    ```
    Add-AzureAccount
    ```

2. U prozoru za **prijavu na Microsoft Azure** upišite adresu e-pošte i lozinku povezanu s računom. Azure potvrđuje sprema vjerodajnicu informacije i zatvara prozor.
3. Postaviti zadani račun za pohranu na račun za pohranu koji koristite za vodič izvršavanjem sljedeće naredbe u prozoru PowerShell:

    ```
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount' 
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName 
    ```

4. Omogućite zapisivanje spremišta blobova platforme servisa: 
 
    ```
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0 
    ```
5. Omogućivanje metrike pohrane za servis Blob pazeći da biste postavili **- MetricsType** na `Minute`:

    ```
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0 
    ```

### <a name="configure-net-client-side-logging"></a>Konfiguriranje .NET zapisivanje na klijentskoj strani

Da biste konfigurirali zapisivanje na klijentskoj strani za .NET aplikaciju, omogućite .NET Dijagnostika u datoteci konfiguracije aplikacije (web.config ili app.config). Dodatne informacije potražite u [zapisivanja na klijentskoj strani s bibliotekom .NET prostora za pohranu klijenta](http://msdn.microsoft.com/library/azure/dn782839.aspx) i [klijentsko zapisivanje s Microsoft Azure prostora za pohranu SDK Java](http://msdn.microsoft.com/library/azure/dn782844.aspx) .

Zapisnik klijentsko sadrži detaljne informacije o kako klijent Priprema zahtjev i prima i obrađuje odgovor.

Klijentska biblioteka za pohranu pohranjeni podaci iz zapisnika klijentsko u naveli mjesto u datoteci konfiguracije aplikacije (web.config ili app.config). 

### <a name="collect-a-network-trace"></a>Prikupljanje mrežnog prometa

Alat za analizu poruka možete koristiti za prikupljanje programa HTTP/HTTPS mrežnog prometa dok se izvodi klijentsku aplikaciju. Alat za analizu poruke na pozadinska koristi [Fiddler](http://www.telerik.com/fiddler) . Preporučujemo da prije prikupljanje mrežnog prometa, morate konfigurirati Fiddler da biste zabilježili šifrirana promet HTTPS:

1. Instalirajte [Fiddler](http://www.telerik.com/download/fiddler).
2. Pokrenite Fiddler.
2. Odaberite Alati **| Mogućnosti fiddler**.
3. U dijaloškom okviru Mogućnosti bili sigurni da **Snimiti povezuje HTTPS** i **Dešifriranje promet HTTPS** odabrane, kao što je prikazano u nastavku.

![Konfiguracija mogućnosti Fiddler](./media/storage-e2e-troubleshooting-classic-portal/fiddler-options-1.png)

Praktični vodič, prikupljanje i prvo spremiti mrežnog prometa u alat za analizu poruku, a zatim stvaranje sesiju analizu analizirati praćenja i zapisnike. Da biste prikupili mrežnog prometa u alat za analizu poruke:

1. Alat za analizu poruku, odaberite **datoteku | Brzi praćenje | Nešifrirani HTTPS**.
2. Praćenja će započeti odmah. Odaberite **Zaustavi** da biste prekinuli praćenja tako da se ne možemo možete konfigurirati da biste samo promet za pohranu za praćenje.
3. Odaberite **Uređivanje** da biste uredili praćenje sesiju.
4. Odaberite vezu **Konfiguriranje** desno od davatelja ETW **Microsoft Pef WebProxy** .
5. U dijaloškom okviru **Dodatne postavke** kliknite karticu **davatelja usluga** .
6. U polje **Naziv glavnog računala filtar** navedite svoje prostora za pohranu krajnje točke, odvojene razmacima. Na primjer, možete odrediti na krajnje točke na sljedeći način; Promjena `storagesample` naziv računa spremišta:
    
    ``` 
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net 
    ```

7. Zatvorite dijaloški okvir, a zatim kliknite da biste započeli prikupljanje praćenja pomoću filtra za naziv glavnog računala na mjestu, **ponovno pokrenite** tako da samo za pohranu Azure mrežni promet uvrštava u rezultatu praćenja.

>[AZURE.NOTE] Nakon što dovršite prikupljanje mrežnog prometa, preporučujemo da se vratite postavke koje možda ste promijenili u Fiddler dešifriranje HTTPS promet. U dijaloškom okviru Mogućnosti Fiddler poništite potvrdne okvire **Snimiti povezuje HTTPS** i **Dešifriranje HTTPS promet** .

Pogledajte odjeljak [Korištenje značajke za praćenje mreže](http://technet.microsoft.com/library/jj674819.aspx) na mreži Technet više pojedinosti.

## <a name="review-metrics-data-in-the-azure-classic-portal"></a>Pregled podataka o metriku na portalu klasičnih Azure

Kada aplikacija pokrenuta vremensko razdoblje, možete pregledati metriku grafikoni koji se pojavljuju u Azure klasični portalu pridržavajte se kako je bio izvođenje na servisu. Najprije ćemo dodati metriku **Uspjeh postotak** praćenja stranicu:

1. Nadzorne ploče za vaš račun za pohranu za [Azure klasični Portal](https://manage.windowsazure.com), a zatim odaberite **Monitor** za prikaz nadzora stranice.
2. Kliknite **Dodavanje metriku** da bi se prikazao dijaloški okvir **Odabir metriku** .
3. Pomicanje potražite grupi **Uspjeh postotak** proširili, pa odaberite **zbrajanja**, kao što je prikazano na slici u nastavku. Ova metrika objedinjuje podatke o postocima uspjeh iz sve operacije Blob.

![Odaberite mjerenja](./media/storage-e2e-troubleshooting-classic-portal/choose-metrics-portal-1.png)

Na portalu klasični Azure sada vidjet ćete **Uspjeh postotak** u praćenju grafikona, zajedno s drugim metriku možda ste dodali (do šest mogu prikazati na grafikonu odjednom). Na slici u nastavku, vidjet ćete da je postotak uspješnosti donekle odgovarali ispod 100%, što je scenarij smo sljedeće će istražiti analizom zapisnike u analizator poruka:

![Grafikon metriku portalu](./media/storage-e2e-troubleshooting-classic-portal/portal-metrics-chart-1.png)

Dodatne informacije o dodavanju metriku za nadzor stranice potražite u članku [Kako: dodavanje metriku tablicu metriku](storage-monitor-storage-account.md#how-to-add-metrics-to-the-metrics-table).

> [AZURE.NOTE] To može potrajati neko vrijeme za podatke metriku da se pojavi na portalu klasičnih Azure Kada omogućite metrike pohrane. Ovo je jer se svaki sat metriku prethodne h ne prikazuju na portalu klasičnih Azure dok isteka trenutnog sata. Osim toga, minute metriku se ne prikazuju na portalu klasični Azure. Stoga zavisno o Kada omogućite metriku, može proći do dva sata da biste vidjeli podatke metriku.

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>Korištenje AzCopy da biste kopirali zapisnici poslužitelja lokalni direktorij

Azure zapisivanja u spremište podaci iz zapisnika poslužitelja za blob-ova, tijekom mjerenja zapisuju u tablice. Zapisnik blob-ova dostupne u dobro poznatog `$logs` spremnik za vaš račun za pohranu. Zapisnik blob-ova imenovane su hijerarhijski godinu, mjesec, dan i sata, tako da možete lako pronaći raspon vremena koju želite istražiti. Na primjer, u `storagesample` je račun, spremnik za zapisnika blob-ova za 01/02/2015, od 8-9 am `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. Pojedinačne blob-ova u ovom spremniku imenovane su sekvencijalno, počevši od `000000.log`.

Možete koristiti alat naredbenog retka AzCopy da biste preuzeli te datoteke zapisnika poslužiteljsko na mjesto na temelju odabira na lokalnom računalu. Ako, na primjer, koristite sljedeću naredbu da biste preuzeli datoteke zapisnika za blob operacije koje snimljene mjesto siječanj 2, 2015 u mapu `C:\Temp\Logs\Server`; Zamjena `<storageaccountname>` s nazivom vašeg računa za pohranu i `<storageaccountkey>` pomoću svojeg ključa za pristup računu:

    AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V

AzCopy dostupna je za preuzimanje na stranici za [Preuzimanja Azure](https://azure.microsoft.com/downloads/) . Detalje o korištenju AzCopy potražite u članku [prijenos podataka s Utility za naredbenog retka AzCopy](storage-use-azcopy.md).

Dodatne informacije o zapisnicima preuzimanje poslužiteljsko potražite u članku [Preuzimanje prostora za pohranu zapisivanje podaci iz zapisnika](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata). 

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Koristite Microsoftov analizator poruku da biste analizirali podatke iz zapisnika

Microsoftov analizator poruka je alat za snimanje, prikaz i analiza protokol poruke promet, događaji i druge poruke sustava ili aplikacije u scenarijima za otklanjanje poteškoća i dijagnostički. Alat za analizu poruke omogućuje učitavanje, zbrajanja i analiza podataka iz zapisnika i spremljene datoteke praćenja. Dodatne informacije o alata za analizu poruka potražite u članku [Poruka alata za analizu operacijski vodič za Microsoft](http://technet.microsoft.com/library/jj649776.aspx).

Alat za analizu poruka sadrži resursi za pohranu Azure koje olakšavaju analizu poslužitelj, klijenta i zapisnika mreže. U ovom se odjeljku smo ćete navode upute za korištenje tih alata da biste riješili problem uspjeha niskog postotka u zapisnicima prostora za pohranu.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>Preuzimanje i instaliranje alata za analizu poruke i resursi za pohranu za Azure

1. Preuzimanje [Alata za analizu poruke](http://www.microsoft.com/download/details.aspx?id=44226) iz Microsoft Download Center i pokrenite instalacijski program.
2. Pokretanje alata za analizu poruke.
3. Na izborniku **Alati** odaberite **Upravitelj resursa**. U dijaloškom okviru **Upravitelj resursa** odaberite **preuzimanja**, a zatim filtrirati **Azure prostora za pohranu**. Vidjet ćete imovine Azure prostora za pohranu, kao što je prikazano na slici u nastavku.
4. Kliknite **Sinkroniziraj sve stavke prikaza** da biste instalirali spremišta sredstvima Azure. Dostupna sredstva obuhvaćaju sljedeće:
    - **Azure prostora za pohranu boja pravila:** Azure pravila za pohranu boja omogućuju definiranje posebne filtre pomoću kojih boju, tekst i stilova fontova da biste istaknuli poruke koje sadrže određene podatke u rezultatima praćenja.
    - **Azure prostora za pohranu grafikoni:** Azure prostora za pohranu grafikoni su unaprijed definiranih grafikona grafikona podaci iz zapisnika poslužitelja. Imajte na umu da da biste koristili za pohranu Azure grafikoni trenutno, možda samo učitate u zapisniku u rešetki analize.
    - **Parsers azure prostora za pohranu:** Parsers prostora za pohranu Azure raščlaniti Azure prostora za pohranu klijenta, poslužitelja i HTTP zapisnika da bi ih prikazati u rešetki analize.
    - **Azure prostora za pohranu filtri:** Azure filtre za pohranu su unaprijed definirane kriterije koje možete koristiti za dohvaćanje podataka u rešetki analize.
    - **Azure prostora za pohranu prikaz izgleda:** Azure prostora za pohranu prikaz izgleda su unaprijed definirane izglede stupaca i grupama u rešetki analize.
4. Ponovno pokrenite alat za analizu poruke nakon instalacije imovinu.

![Upravitelj resursa za poruku alata za analizu](./media/storage-e2e-troubleshooting-classic-portal/mma-start-page-1.png)

> [AZURE.NOTE] Instalacija svih resursi za pohranu Azure prikazani u svrhu ovog praktičnog vodiča.

### <a name="import-your-log-files-into-message-analyzer"></a>Uvoz datoteke zapisnika programa u alat za analizu poruke

Sve datoteke spremljene zapisnika (poslužiteljsko klijentsko te mreže) možete uvesti u jednu sesiju u Microsoftov analizator poruke za analizu.

1. Na izborniku **datoteka** u Microsoftov analizator poruke kliknite **Novu sesiju**, a zatim **Prazna sesiju**. U dijaloškom okviru **Nova sesija** unesite naziv za analizu sesiju. U oknu s **Detaljima sesiju** kliknite gumb **datoteka** . 
1. Da biste učitali mrežnih podataka praćenje generira alata za analizu poruku, kliknite **Dodaj datoteke**, pronađite mjesto na kojem ste spremili datoteku .matp iz sesiju praćenje web, odaberite .matp datoteku i kliknite **Otvori**. 
1. Da biste učitali podaci iz zapisnika na strani poslužitelja, kliknite **Dodaj datoteke**, pronađite mjesto na kojem ste preuzeli na strani poslužitelja zapisnici, odaberite zapisničke datoteke za vremenskog razdoblja koje želite analizirati i kliknite **Otvori**. Zatim u oknu s **Detaljima sesiju** postavite **Tekst zapisnika konfiguracije** padajućeg izbornika za svaku datoteku zapisnika poslužiteljsko na **AzureStorageLog** da biste bili sigurni da Microsoftov analizator poruke možete analizirati datoteke zapisnika pravilno.
1. Da biste učitali podatke zapisnika klijentsko, kliknite **Dodaj datoteke**, pronađite mjesto na koje ste spremili klijentski se zapisnici, odaberite zapisničke datoteke koje želite analizirati i kliknite **Otvori**. Zatim u oknu s **Detaljima sesiju** postavite **Tekst zapisnika konfiguracije** padajućeg izbornika za svaku datoteku zapisnika klijentsko na **AzureStorageClientDotNetV4** da biste bili sigurni da Microsoftov analizator poruke možete analizirati datoteke zapisnika pravilno.
1. Kliknite **Start** u dijaloškom okviru **Nova sesija** za učitavanje i analizirati podatke. Podaci zapisnika prikazuju se u rešetki poruka alata za analizu Analysis.

Na slici u nastavku prikazuje sesiju primjer s poslužiteljem, klijenta i datoteka zapisnika praćenja mreže.

![Konfiguriranje sesiju poruka alata za analizu](./media/storage-e2e-troubleshooting-classic-portal/configure-mma-session-1.png)

Imajte na umu alata za analizu poruke učitava datoteke zapisnika u memorije. Ako imate veliki skup podaci iz zapisnika, želite filtrirati da biste dobili najbolje performanse iz alata za analizu poruke.

Prvo, odrediti vremenski okvir koji vas zanima pregleda, a zadržati ovu vremensko razdoblje moguću. U mnogim slučajevima želite pregledati razdoblje minuta ili sati na većini. Uvoz najmanji skup zapisnika koji možete ne odgovara vašim potrebama.

Ako i dalje imate veliku količinu podataka zapisnika, pa preporučujemo vam da biste odredili sesiju filtar da biste filtrirali podatke iz zapisnika prije nego što ga učitati. U okviru **Sesiju filtar** odaberite gumb **biblioteke** da biste odabrali unaprijed definirane filtar. Ako, na primjer, odabrati **globalni vrijeme filtar kojeg** filtre za pohranu Azure filtar na vremenski interval. Zatim možete urediti kriterija filtra da biste odredili početni i završni vremenske oznake u intervalu koji želite pogledati. Možete filtrirati i prema Šifra statusa; Ako, na primjer, možete odabrati da biste učitali samo stavke evidencije gdje je Šifra stanja 404.

Dodatne informacije o uvozu podataka u zapisniku na Microsoftov analizator poruka potražite u članku [Dohvaćanje podataka iz poruka](http://technet.microsoft.com/library/dn772437.aspx) na mreži TechNet.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>Povezivanje podataka iz datoteke zapisnika pomoću zahtjev ID klijenta

Klijentska biblioteka za pohranu Azure automatski generira ID za zahtjev za jedinstveni klijenta za svaki zahtjev. Ta vrijednost napisan zapisnika klijent, u zapisniku i praćenje mrežnog prometa da biste mogli koristiti za povezivanje podataka preko tri zapisnika unutar alata za analizu poruke. U odjeljku [ID klijenta zahtjev](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) za dodatne informacije o zahtjevu za klijent ID-a.

U odjeljcima opisuju kako koristiti prikaze unaprijed konfigurirana i prilagođeni izgled za povezivanje i grupiranje podataka na temelju zahtjev ID klijenta

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Odaberite raspored prikaza da biste prikazali u rešetki analizu

Resursi za pohranu za analizu poruke obuhvaćaju Azure prostora za pohranu prikaz rasporeda, koje su unaprijed konfigurirana prikazima koje možete koristiti za prikaz podataka pomoću korisne grupiranja i stupce u različitim scenarijima. Stvaranje prilagođenog prikaza rasporeda i ih spremiti za ponovno korištenje. 

Na slici u nastavku prikazuje izbornik **Prikaz izgleda** , dostupna tako da odaberete **Raspored prikaza** vrpce alatne trake. Prikaz rasporeda za pohranu Azure grupiraju se u čvor **Azure prostora za pohranu** na izborniku. Možete tražiti `Azure Storage` u okvir za pretraživanje da biste filtrirali Azure prostora za pohranu prikaz samo izgleda. Možete odabrati i zvjezdica uz prikaz rasporeda za dodavanje u favorite i prikazuje pri vrhu izbornika.

![Izbornik Prikaz izgleda](./media/storage-e2e-troubleshooting-classic-portal/view-layout-menu.png)

Počinje sa, odaberite **Grouped ClientRequestID i Module**. Ovaj prikaz grupa izgleda prvi put prijavite podaci iz zapisnika za sva tri po ID klijenta zahtjev, a zatim izvorišna datoteka zapisnika (ili **Modul** u poruci analizator). S tom prikazu možete kroz razine naniže u pojedinom klijentu zahtjev za ID-a i vidjeti podatke iz svih datoteka tri zapisnika za taj klijent zahtjev ID-a.

Slika niže prikazano je ovaj prikaz izgleda primjenjuje se na ogledne podatke zapisnika, s podskup stupaca koji se prikazuju. Vidjet ćete da za ID za zahtjev za određeni klijenta, rešetki analiza prikazuje podatke iz zapisnika za klijenta, zapisniku i praćenje mrežnog prometa.

![Raspored prikaza Azure prostora za pohranu](./media/storage-e2e-troubleshooting-classic-portal/view-layout-client-request-id-module.png)

>[AZURE.NOTE] Različite zapisničke datoteke imaju različite stupce tako da se kada se u rešetki analizu prikaz podataka iz više datoteka zapisnika, neki stupci možda sadrži podatke za dani redak. Na primjer, na gornjoj slici, reci zapisnika klijent ne prikazuj sve podatke za **vremensku oznaku**, **TimeElapsed**, **izvor**i **odredište** stupaca, jer ti stupci ne postoje u zapisniku klijent, ali postoji u mrežnog prometa. Isto tako, stupac **Vremenska oznaka** prikazuje vremenske oznake podataka iz zapisnika poslužitelja, ali nema podaci se prikazuju za stupce **TimeElapsed**, **izvorišnoj**i **odredišnoj** , koji nisu dio u zapisniku. 

Uz prikaz rasporeda za pohranu Azure možete definirati i Spremi prikaz izgleda. Možete odabrati druge željena polja za grupiranje podataka i spremanje grupiranja kao dio kao i prilagođeni izgled. 

### <a name="apply-color-rules-to-the-analysis-grid"></a>Primijeni pravila boja u rešetku analizu

Resursi za pohranu uvrštavati pravila boje koje se nude vizualnim znači da biste odredili različite vrste pogrešaka u rešetki analize. Pravila unaprijed definirane boja primijeniti HTTP pogreške tako da se prikažu samo za poslužitelj zapisnika i mrežne praćenja.

Da biste primijenili boju pravila, odaberite **Boju pravila** s vrpce alatne trake. Vidjet ćete pravila boja Azure prostora za pohranu na izborniku. Praktični vodič, odaberite **Klijenta pogreške (kôd statusa između 400 i 499)**, kao što je prikazano na slici u nastavku.

![Raspored prikaza Azure prostora za pohranu](./media/storage-e2e-troubleshooting-classic-portal/color-rules-menu.png)

Osim pomoću pravila boja Azure prostora za pohranu, možete definirati i spremanje pravila boja.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>Grupiranje i filtar podaci iz zapisnika da biste pronašli 400 raspon pogreške

Zatim ćemo ćete grupirati i filtriranje podataka zapisnika da biste pronašli sve pogreške u rasponu od 400.

1. Pronađite stupac **kôd statusa** u rešetki analizu, desnom tipkom miša kliknite zaglavlje stupca pa odaberite **grupu**.
2. Sljedeći, grupa **ClientRequestId** stupac. Vidjet ćete da je podatke u rešetki analizu sada organiziran Šifra stanja i drugi klijent zahtjev za ID-a.
1. Ako još nije prikazan, prikazuje se prozor alata filtra prikaza. Na vrpci alatnoj traci odaberite **Alat za Windows**, zatim **Filtar prikaza**.
2. Da biste filtrirali podatke da biste prikazali samo 400 raspon pogreške, dodajte sljedeće kriterije filtriranja u prozor **Filtar prikaza** , a zatim kliknite **Primijeni**:

        (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)

Na slici u nastavku prikazuje rezultate u ovom grupiranja i filtriranja. Proširivanje polje **ClientRequestID** ispod grupiranja za Šifra stanja 409, na primjer, prikazuje operaciju koja je rezultirala kod stanja.

![Raspored prikaza Azure prostora za pohranu](./media/storage-e2e-troubleshooting-classic-portal/400-range-errors1.png)

Nakon što primijenite filtar, vidjet ćete da ukupnih redaka iz zapisnika za klijenta, kao klijent zapisnika uključiti stupac **kôd statusa** . Počinje sa, smo ćete pregledajte poslužitelj i zapisnika praćenja mreže da biste pronašli 404 pogreške, a zatim ćemo ćete se vratite zapisnika klijenta za ispitivanje operacije na klijentu koji su doveli ih.

>[AZURE.NOTE] Možete filtrirati prema stupcu **kôd statusa** i i dalje prikazuju podatke iz svih tri zapisnike, uključujući zapisnika klijent ako Dodavanje izraza filtra koja sadrži unose zapisnika gdje je null Šifra stanja. Sastavljanje izraz za filtriranje, koristite:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code> 
>
> Filtar vraća sve retke iz klijentskog programa zapisnika i samo retke iz zapisnika poslužitelj i prijaviti se HTTP kojima je Šifra stanja veća od 400. Ako zatvarate u prikaz izgled grupiranih prema ID klijenta zahtjev i modul, možete pretraživati ili se pomaknite do stavke evidencije da biste pronašli one koje predstavljaju sve tri zapisnika.   

### <a name="filter-log-data-to-find-404-errors"></a>Podaci iz zapisnika filtar da biste pronašli 404 pogreške

Resursi za pohranu obuhvaćaju unaprijed definirane filtri koje možete koristiti da biste suzili podaci iz zapisnika za pronalaženje pogrešaka ili trendova koju tražite. Nakon toga ćete primijeniti dva filtra unaprijed definirane: onaj koji filtrira na poslužitelj i zapisnika praćenja mreže 404 pogrešaka i onaj koji filtrira podatke na određeni vremenski raspon.

1. Ako još nije prikazan, prikazuje se prozor alata filtra prikaza. Na vrpci alatnoj traci odaberite **Alat za Windows**, zatim **Filtar prikaza**.
2. U prozoru filtra prikaza odaberite **biblioteku**i potražite na `Azure Storage` da biste pronašli prostora za pohranu Azure filtre. Odaberite filtar za **404 (nema) poruke u svim zapisnika**.
3. Ponovno prikazali izbornik **biblioteke** i pronađite i odaberite **Globalni filtar vremena**.
4. Uređivanje vremenske oznake prikazani u filtru na raspon koji želite pregledati. To će pomoći da biste suzili raspon podataka radi analize.
5. Filtar prikazivati slične primjeru u nastavku. Kliknite **Primijeni** da biste primijenili filtar u rešetku analize.

        ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And 
        (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)

![Raspored prikaza Azure prostora za pohranu](./media/storage-e2e-troubleshooting-classic-portal/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Analiziranje podataka zapisnika

Sad kad ste grupirati i filtrirati podatke, možete provjeriti pojedinosti pojedinačne zahtjeve koji je generirao 404 pogreške. U trenutni raspored prikaza grupiranja podataka po ID zahtjev za klijenta, zatim po izvoru zapisnika. Jer smo filtrirate na zahtjeve za kojima polje kôd statusa sadrži 404, ćemo vidjeti samo poslužitelj i mrežnih podataka za praćenje, ne podataka zapisnika klijenta.

Na slici u nastavku prikazuje zahtjeva za određene gdje se Blob operacija dalo na 404 jer ne postoji blob-om. Imajte na umu da neke stupce uklonjeni su iz standardni prikaz da bi se prikazali relevantne podatke.

![Filtrirani poslužitelja i zapisnika praćenja mreže](./media/storage-e2e-troubleshooting-classic-portal/server-filtered-404-error.png)

Nakon toga ne možemo taj zahtjev ID klijenta ćete povezivanje s podacima zapisnika klijent da biste vidjeli akcije koje je klijent poduzimanja kada se pogreška dogodilo. Možete prikazati novog prikaza rešetke analizu za ovu sesiju da se podaci zapisnika klijenta, koji se otvara u drugoj kartici:

1. Najprije vrijednost polja **ClientRequestId** kopiranje u međuspremnik. To možete učiniti tako da odaberete neki redak, pronalaženje polje **ClientRequestId** , desnom tipkom miša na vrijednost podataka, a zatim odaberite **Kopiraj 'ClientRequestId'**. 
1. Na vrpci alatnoj traci odaberite **Novi preglednika**, a zatim odaberite **Analizu rešetke** za otvaranje na novoj kartici. Na novoj kartici prikazuje sve podatke u datoteke zapisnika programa, bez grupiranja, filtriranja ili boja pravila. 
2. Na vrpci alatnoj traci odaberite **Raspored prikaza**, a zatim odaberite **Sve stupce .NET klijenta** u odjeljku **Azure prostora za pohranu** . Taj prikaz izgled prikazuje podatke iz klijentskog programa zapisnika, kao i zapisnika praćenja poslužitelj i mreže. Po zadanom su sortirani **MessageNumber** stupac.
3. Nakon toga pretraživanje zapisnika klijenta za zahtjev za ID klijenta Na vrpci alatnoj traci odaberite **Traži poruke**, a zatim navedite Prilagođeni filtar na zahtjev za ID klijenta u polje **Traži** . Pomoću ove sintakse za filtriranje, pri određivanju svoj ID klijenta zahtjev:

        *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"

Alat za analizu poruke pronalazi i odabir prve stavke zapisnika gdje kriterij pretraživanja podudara zahtjev ID klijenta U zapisniku klijentskog postoje nekoliko stavki za svaki zahtjev za ID klijenta, stoga želite grupirati prema polju **ClientRequestId** da biste olakšali da biste ih vidjeli sve zajedno. Na slici u nastavku prikazuje sve poruke u zapisniku klijenta za navedeni klijent zahtjev za ID-a. 

![Prikazivanje zapisnika klijent 404 pogreške](./media/storage-e2e-troubleshooting-classic-portal/client-log-analysis-grid1.png)

Pomoću podaci prikazani u prikazu rasporeda u ove dvije kartice, možete analizirati podatke zahtjev da biste odredili što može uzrokovati pogrešku. Možete pogledati i zahtjeve ispred ova da biste vidjeli Ako prethodni događaj možda imaju koji vode o pogrešci 404. Na primjer, možete pregledati stavke evidencije klijent ispred taj klijent zahtjev ID da biste odredili hoće li se blob-om izbrisan ili Pogreška zbog klijentska aplikacija pozivanje CreateIfNotExists API-JA na spremnik ili blob. U zapisniku klijenta s blob adresa možete pronaći u polje **opisa** poslužitelj i zapisnika praćenja mreže, ti se podaci prikazuju u polju **Sažetak** .

Kada znate adresu blob u kojem su dale o pogrešci 404, možete ispitati Dodatno. Tražite li stavke evidencije za druge poruke pridružene operacije na istom blob-om, možete provjeriti li klijent prethodno izbrisali entitet. 

## <a name="analyze-other-types-of-storage-errors"></a>Analiza druge vrste pogrešaka za pohranu

Sad kad ste upoznati s korištenjem alata za analizu poruku da biste analizirali podatke iz zapisnika, možete analizirati druge vrste pogrešaka u prikazu izgleda, pravila za boju i pretraživanja i filtriranja. Tablicama u nastavku navedeni neki problemi koji se mogu pojaviti i kriterija filtra možete koristiti da biste pronašli ih. Dodatne informacije o izgradnje filtri i alata za analizu poruke filtriranje jezik, potražite u članku [Filtriranje podataka poruke](http://technet.microsoft.com/library/jj819365.aspx).

|    Da biste istražili...                                                                                               |    Korištenje izraza filtra...                                                                                                                                                                                                                                        |    Izraz primjenjuje se na zapisnika (klijent, Server, a zatim mrežom, sve)    |
|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
|    Neočekivana kašnjenja u isporuku poruke na u redu čekanja                                                              |    AzureStorageClientDotNetV4.Description sadrži "Retrying operacija nije uspjela."                                                                                                                                                                                |    Klijent                                                        |
|    HTTP povećava PercentThrottlingError                                                                       |    HTTP. Response.StatusCode == 500 & #124; & #124; HTTP. Response.StatusCode == 503                                                                                                                                                                                          |    Mreža                                                       |
|    Povećavanje u PercentTimeoutError                                                                               |    HTTP. Response.StatusCode == 500                                                                                                                                                                                                                             |    Mreža                                                       |
|    Povećavanje u PercentTimeoutError (sve)                                                                         |    * Kôd statusa == 500                                                                                                                                                                                                                                          |    Sve                                                           |
|    Povećavanje u PercentNetworkError                                                                               |    AzureStorageClientDotNetV4.EventLogEntry.Level < 2                                                                                                                                                                                                          |    Klijent                                                        |
|    HTTP 403 (nije dozvoljeno) poruke                                                                                 |    HTTP. Response.StatusCode == 403                                                                                                                                                                                                                             |    Mreža                                                       |
|    HTTP 404 (nije pronađeno) poruke                                                                                 |    HTTP. Response.StatusCode == 404                                                                                                                                                                                                                             |    Mreža                                                       |
|    404 (sve)                                                                                                     |    * Kôd statusa == 404                                                                                                                                                                                                                                          |    Sve                                                           |
|    Zajedničko korištenje problem autorizacije pristup potpis (SAS)                                                             |    AzureStorageLog.RequestStatus == "SASAuthorizationError"                                                                                                                                                                                                     |    Mreža                                                       |
|    HTTP 409 (Sukob) poruke                                                                                  |    HTTP. Response.StatusCode == 409                                                                                                                                                                                                                             |    Mreža                                                       |
|    409 (sve)                                                                                                     |    * Kôd statusa == 409                                                                                                                                                                                                                                          |    Sve                                                           |
|    Niska PercentSuccess ili analize stavke evidencije ste operacije sa statusom transakcije ClientOtherErrors    |    AzureStorageLog.RequestStatus == "ClientOtherError"                                                                                                                                                                                                         |    Poslužitelj                                                        |
|    Nagle upozorenje                                                                                               |    ((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1,5)) i (AzureStorageLog.RequestPacketSize < 1460) i (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200)        |    Poslužitelj                                                        |
|    Raspon vremena u zapisnicima poslužitelja i mreže                                                                    |    #Vremenska oznaka > = 2014.-10-20T16:36:38 i #Timestamp < = 2014.-10-20T16:36:39                                                                                                                                                                                     |    Poslužitelj, a zatim na mreži                                               |
|    Raspon vremena u zapisnicima poslužitelja                                                                                |    AzureStorageLog.Timestamp > = 2014.-10-20T16:36:38 i AzureStorageLog.Timestamp < = 2014.-10-20T16:36:39                                                                                                                                                     |    Poslužitelj                                                        |


## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o otklanjanju poteškoća scenarijima kraj do kraja Azure pohrane u, potražite u sljedećim resursima:

- [Praćenje, dijagnosticiranje i otklanjanje poteškoća s spremište na platformi Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md)
- [Prostor za pohranu Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx)
- [Praćenje računa za pohranu na portalu za Azure](storage-monitor-storage-account.md)
- [Prijenos podataka s pomoćnog programa naredbenog retka AzCopy](storage-use-azcopy.md)
- [Vodič za operacijski Microsoft poruka alata za analizu](http://technet.microsoft.com/library/jj649776.aspx)
 
 
