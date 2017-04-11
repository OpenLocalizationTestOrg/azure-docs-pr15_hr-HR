<properties 
    pageTitle="Uvoz i izvoz podataka u predmemoriji Redis Azure | Microsoft Azure" 
    description="Upute za uvoz i izvoz podataka i iz spremišta blobova s vašeg instancama Azure Redis predmemorije premium" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="import-and-export-data-in-azure-redis-cache"></a>Uvoz i izvoz podataka u predmemoriji Redis Azure

Uvoz/izvoz je Azure Redis predmemorije podataka upravljanja operaciju, koji omogućuje uvoz podataka u predmemoriji Redis Azure ili izvoz podataka iz predmemorije Redis Azure po uvozu i izvozu snimka Redis predmemorije baze podataka (RDB) od premium predmemorije blob stranice u račun za Azure prostora za pohranu. Uvoz/izvoz omogućuje migrirati između različitih instance predmemorije Redis Azure ili popunjavanje predmemorije s podacima prije korištenja.

Ovaj članak sadrži vodič za uvoz i izvoz podataka s Azure Redis predmemorije i navedeni odgovori na najčešća pitanja.

>[AZURE.IMPORTANT] Uvoz/izvoz se pretpregled i dostupna je samo za predmemorije [premium sloju](cache-premium-tier-intro.md) .

## <a name="import"></a>Uvoz

Uvoz može se koristiti da bi se prikazala Redis kompatibilne RDB datoteke s bilo kojeg Redis poslužitelja u oblak ni okruženju, uključujući Redis sustavom Linux, Windows ili bilo kojem davatelju usluga oblaku kao što su Amazon Web-servisima i drugim korisnicima. Uvoz podataka je jednostavan način da biste stvorili predmemoriju s unaprijed unesene podatke. Tijekom postupka uvoza Azure Redis predmemorije učitava RDB datoteke sa servisa za pohranu Azure u memorije i umeće tipki u predmemoriju.

>[AZURE.NOTE] Prije početka operacije uvoza, provjerite je li se prenose u BLOB-Ova stranica u Azure prostora za pohranu, u istom regija i pretplate kao vašoj instanci predmemorije Redis Azure Redis baze podataka (RDB) datoteke ili datoteke. Dodatne informacije potražite u članku [Početak rada s spremište blobova platforme Azure](../storage/storage-dotnet-how-to-use-blobs.md). Ako ste izvezli datoteku RDB pomoću značajke za [Azure Redis predmemorije izvoz](#export) , RDB datoteku već pohranjena u blob stranice i spremni za uvoz.

1. Da biste uvezli jedan ili više izvezene predmemorije blob-ova, [Pregledaj da biste predmemoriju](cache-configure.md#configure-redis-cache-settings) na portalu za Azure, a zatim kliknite **Uvoz podataka** iz plohu **Postavke** instance predmemorije.

    ![Uvoz podataka][cache-import-data]

2. Kliknite **Odaberite Blob(s)** , a zatim odaberite račun za pohranu koji sadrži podatke koje želite uvesti.

    ![Odaberite račun za pohranu][cache-import-choose-storage-account]

3. Kliknite spremnik koji sadrži podatke koje želite uvesti.

    ![Odaberite kontejner][cache-import-choose-container]

4. Odaberite blob jedan ili više polja da biste uvezli klikom na područje s lijeve strane blob naziv, a zatim **Odaberite**.

    ![Odaberite blob-ova][cache-import-choose-blobs]

5. Kliknite **Uvoz** da biste započeli postupak uvoza.

    >[AZURE.IMPORTANT] Tijekom postupka uvoza predmemorije nije dostupno za klijente predmemorije, a neki postojeći podaci u predmemoriji se briše.

    ![Uvoz][cache-import-blobs]

    Možete pratiti napredak operacije uvoza slijedeći obavijesti s portala za Azure ili pregledavanjem događaje u [nadzora prijave](cache-configure.md#support-amp-troubleshooting-settings).

    ![Tijek uvoza][cache-import-data-import-complete] 


## <a name="export"></a>Izvoz

Izvoz omogućuje vam da biste izvezli podatke pohranjene u predmemoriji Redis Azure da biste Redis kompatibilne datoteke RDB. Tu značajku možete koristiti za premještanje podataka iz jedne instance Azure Redis predmemorije u drugu ili na drugi poslužitelj Redis. Tijekom postupka izvoza, stvorit će se privremena datoteka na VM koji hostira instancu server Azure Redis predmemorije, a datoteka se prenosi račun zaduženog za pohranu. Kada se dovrši operaciju izvoza status uspjeha ili pogreške, privremene datoteke će se briše.

1. Izvoz sadržaja trenutne predmemoriju za pohranu, [Pronađite predmemoriju](cache-configure.md#configure-redis-cache-settings) na portalu za Azure i kliknite **Izvoz podataka** iz plohu **Postavke** instance predmemorije.

    ![Odaberite kontejner prostora za pohranu][cache-export-data-choose-storage-container]

2. Kliknite **Odaberite spremnik za pohranu** , a zatim odaberite željeni prostora za pohranu račun. Račun za pohranu mora biti u istom pretplate i regija kao predmemoriju.

    >[AZURE.IMPORTANT] Radi s blob polja stranice, koje podržava klasični i račune ARM prostora za pohranu, ali ne podržava [bloba račune za pohranu](../storage/storage-blob-storage-tiers.md#blob-storage-accounts) trenutno uvoz/izvoz.

    ![Račun za pohranu][cache-export-data-choose-account]

3. Odaberite željeni blob kontejner, a zatim kliknite **Odaberi**. Da biste koristili novi spremnik, kliknite **Dodaj spremnik** najprije dodajte, a zatim je odaberite s popisa.

    ![Odaberite kontejner prostora za pohranu][cache-export-data-container]

4. Upišite **prefiks Blob imena** , a zatim kliknite **Izvoz** da biste pokrenuli postupak izvoza. Prefiks naziva blob koristi se za prefiks nazive datoteka koje generira ovaj postupak izvoza.

    ![Izvoz][cache-export-data]

    Možete pratiti napredak operacije izvoza slijedeći obavijesti s portala za Azure ili pregledavanjem događaje u [nadzora prijave](cache-configure.md#support-amp-troubleshooting-settings).

    ![][cache-export-data-export-complete]

    Predmemorija ostaju dostupni za upotrebu tijekom postupka izvoza.


## <a name="importexport-faq"></a>Najčešća pitanja vezana uz uvoz/izvoz

Ova sekcija sadrži najčešća pitanja o značajci uvoz/izvoz.

-   [Razine cijene koje mogu koristiti uvoz/izvoz?](#what-pricing-tiers-can-use-importexport)
-   [Možete I uvesti podatke s bilo kojeg poslužitelja Redis?](#can-i-import-data-from-any-redis-server)
-   [Moje predmemorije bit će dostupni tijekom operacije uvoz/izvoz?](#will-my-cache-be-available-during-an-importexport-operation)
-   [Možete koristiti uvoz/izvoz s Redis klaster?](#can-i-use-importexport-with-redis-cluster)
-   [Kako uvoz/izvoz funkcionira s postavku prilagođene baze podataka?](#how-does-importexport-work-with-a-custom-databases-setting)
-   [Kako se uvoz/izvoz razlikuje od Redis postojanost?](#how-is-importexport-different-from-redis-persistence)
-   [Je li moguće automatizirati uvoz/izvoz pomoću programa PowerShell, EŽA ili drugim klijentskim programima za upravljanje?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
-   [Primio sam pogreške prekoračenja vremenskog ograničenja tijekom operacije Moje uvoz/izvoz. Što znači?](#i-received-a-timeout-error-during-my-importexport-operation.-what-does-it-mean)
-   [Dobio sam pogreške pri izvozu Moji podaci za spremište blobova platforme Azure. Šta se dogodilo?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage.-what-happened)


### <a name="what-pricing-tiers-can-use-importexport"></a>Razine cijene koje mogu koristiti uvoz/izvoz?

Uvoz/izvoz dostupna je samo u premium cijene sloju.

### <a name="can-i-import-data-from-any-redis-server"></a>Možete I uvesti podatke s bilo kojeg poslužitelja Redis?

Da, osim uvoza podataka izvezene iz predmemorije Redis Azure instance možete uvesti RDB datoteka s bilo kojeg poslužitelja Redis izvodi u oblak ni okruženju, kao što su Linux, Windows, ili davatelja usluga kao što su Amazon web-servisi u oblaku. Da biste to učinili, prijenos datoteke RDB željeni Redis poslužitelja u blob stranice u račun za Azure prostora za pohranu i uvezite ga u vašoj instanci Azure Redis predmemorije premium. Na primjer, trebali biste izvoz podataka iz predmemorije proizvodnje i uvezite ga u predmemoriji koristili kao dio pripremna okruženje za testiranje ili migracije. 

### <a name="will-my-cache-be-available-during-an-importexport-operation"></a>Moje predmemorije bit će dostupni tijekom operacije uvoz/izvoz?

-   **Izvoz** - predmemorije ostaju dostupne i možete nastaviti koristiti predmemoriju tijekom operacije izvoza.
-   **Uvoz** - predmemorije postaju nedostupne prilikom pokretanja operacije uvoza i postaju dostupne za korištenje nakon dovršetka postupka uvoza.


### <a name="can-i-use-importexport-with-redis-cluster"></a>Možete koristiti uvoz/izvoz s Redis klaster?

Da, i koje možete uvoz/izvoz između grupirani predmemorije i predmemoriju koje nisu grupirani. Nakon Redis klaster [podržava samo baze podataka 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), neće se uvesti podatke u bazama podataka koji nije 0. Prilikom uvoza podataka grupirani predmemorije, tipke su daljnja distribucija među shards klaster. 

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Kako uvoz/izvoz funkcionira s postavku prilagođene baze podataka?

Neke razine cijena imaju različite [ograničenja baze podataka](cache-configure.md#databases), tako da postoje neka razmatranja prilikom uvoza ako ste konfigurirali prilagođene vrijednosti za na `databases` postavljanje tijekom stvaranja predmemoriju.

-   Prilikom uvoza cijene sloj s na donjem `databases` ograničenje od sloju iz koje ste izvezli:
    -   Ako koristite zadani broj `databases` koji je 16 za sve razine cijene, podaci se gube.
    -   Ako koristite prilagođeni broj `databases` te pada unutar ograničenja za sloj na koje uvozite, bez podataka se gube.
    -   Ako izvezene podatke koje se nalaze podaci u bazi podataka koji prelazi granice novi sloju, podaci iz te veći baze podataka ne uvoze.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Kako se uvoz/izvoz razlikuje od Redis postojanost?

Azure Redis predmemorije postojanost omogućuje zadržava podatke pohranjene u Redis Azure za pohranu. Kada postojanost konfiguriran, Azure Redis predmemorije nastavi snimke predmemoriju Redis u binarnom obliku Redis disk na temelju konfigurirati učestalost sigurnosne kopije. Ako se dogodi do teškog oštećenja događaj koje onemogućuje primarni i replike predmemorije, predmemoriju podatke vratit će se automatski pomoću najnovije snimke. Dodatne informacije potražite u članku [kako konfigurirati postojanost podataka za Premium Redis predmemoriju Azure](cache-how-to-premium-persistence.md).

Uvoz / izvoz omogućuje prijenos podataka u ili izvoza iz predmemorije Redis Azure. Ga ne konfiguriranje sigurnosno kopiranje i vraćanje pomoću postojanost Redis.


### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Je li moguće automatizirati uvoz/izvoz pomoću programa PowerShell, EŽA ili drugim klijentskim programima za upravljanje?

Da, PowerShell upute potražite u članku [za Redis predmemorije za uvoz](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) i [Izvoz Redis predmemorije](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).



### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Primio sam pogreške prekoračenja vremenskog ograničenja tijekom operacije Moje uvoz/izvoz. Što znači?

Ako ostaju na plohu **Podaci za uvoz** ili **Izvoz podataka iz** više od 15 minuta prije pokretanja postupka, primit ćete pogrešku sličnu ovoj.

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Da biste riješili taj problem, pokretanje uvoza ili izvoza prije 15 minuta isteka.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Dobio sam pogreške pri izvozu Moji podaci za spremište blobova platforme Azure. Šta se dogodilo?

Uvoz/izvoz funkcionira samo s RDB datoteka pohranjenih kao BLOB-Ova stranica. Druge vrste blob nisu podržani trenutno, uključujući račune spremište blobova platforme s tipkovni i zanimljivih razine.


## <a name="next-steps"></a>Daljnji koraci
Saznajte kako koristiti više značajki predmemorije premium.

-   [Uvod u sloju Premium predmemorije Redis Azure](cache-premium-tier-intro.md)    

  
<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png








