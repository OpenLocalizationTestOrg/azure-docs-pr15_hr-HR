<properties
    pageTitle="Početak rada s Azure pretraživanja u NodeJS | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Voditi kroz stvaranje aplikacije za pretraživanje na glavnom računalu oblaka servisa za pretraživanje na Azure pomoću NodeJS kao programski jezik."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-nodejs"></a>Početak rada s Azure pretraživanja u NodeJS
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Saznajte kako stvoriti prilagođenu aplikaciju NodeJS pretraživanja koja koristi Azure traženje njegov pretraživanju. Pomoću ovog praktičnog vodiča koristi [Azure pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/dn798935.aspx) za sastavljanje objekte i postupaka koji se koriste u ovu vježbu.

Koristi [NodeJS](https://nodejs.org) i NPM, [Sublime 3 tekst](http://www.sublimetext.com/3)i komponente Windows PowerShell sustavu Windows 8.1 razviti i testiranje kod.

Da biste pokrenuli ovaj uzorak, morate imati servisa Azure pretraživanja koji se možete prijaviti na [Portal za Azure](https://portal.azure.com). Detaljne upute potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) .

## <a name="about-the-data"></a>O podacima

U ovom primjeru aplikacije koristi podatke iz u [Sjedinjenim Američkim Državama Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtrirano na na stanje sustava Rhode Otok da biste smanjili veličinu skupa podataka. Da biste sastavili aplikacije za pretraživanje koji vraća zgrade obližnjim objektima kao što su hospitals i škole, kao i geological značajke kao što su strujanja, jezera smrznu i summits ćemo koristiti te podatke.

U ovoj aplikaciji **DataIndexer** program stvara i učitava indeksu koji koriste konstrukta [Indeksiranje](https://msdn.microsoft.com/library/azure/dn798918.aspx) , dohvaćanje filtrirani skup podataka USGS iz javne baze podataka SQL Azure. Vjerodajnice i veze u programski kod daju se informacije za mrežni izvor podataka. Potrebno je bez daljnje konfiguracije.

> [AZURE.NOTE] Ne možemo primijeniti filtar na ovaj skup podataka da biste bili u odjeljku ograničenje 10 000 dokumenta od besplatnih cijene sloju. Ako koristite standardne sloju, to ograničenje se neće primijeniti. Detalje o kapacitet za svaki cijene sloju potražite u članku [ograničenja servisa za pretraživanje](search-limits-quotas-capacity.md).


<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Pronađite naziv servisa i api-ključ servisa Azure pretraživanja

Kada stvorite servis, vratite se na portal za dohvaćanje URL-a ili `api-key`. Veza sa servisa za pretraživanje morate imati i URL-a i `api-key` za provjeru autentičnosti poziv.

1. Prijava na [Portal za Azure](https://portal.azure.com).
2. Na traci Skok kliknite **servis za pretraživanje** da biste dobili popis svih servisa Azure pretraživanje dodjeli za vašu pretplatu.
3. Odaberite servis koji želite koristiti.
4. Na nadzornoj ploči za servis, vidjet ćete pločice za ključne informacije, kao i ikona ključa za pristup tipki administrator.

    ![][3]

5. Kopirajte URL servisa, ključa administrator i ključa upita. Morat ćete sva tri kasnije, prilikom dodavanja config.js datoteku.

## <a name="download-the-sample-files"></a>Preuzimanje oglednih datoteka

Koristite ili jednu od sljedeće postupke da biste preuzeli uzorka.

1. Idite na [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodeJSIndexerDemo).
2. Kliknite **Preuzmi poštanski broj**, spremite datoteku .zip i izdvojiti sve datoteke koje sadrži.

Sve izmjene sljedeće datoteke i pokretanje izvješća bit će protiv datoteka u ovu mapu.


## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a>Ažurirajte na config.js. URL servisa za pretraživanje i ključ za API-ja

Pomoću URL-a i api-tipki koju ste kopirali ranije, navedite URL, administrator ključ i ključa za upit u konfiguracijskoj datoteci.

Tipke za administratore dodijeliti potpunu kontrolu nad operacije servisa, uključujući stvaranje ili brisanje indeksa i učitavanje dokumenata. Nasuprot tome, upit tipke su samo za čitanje operacije najčešće koriste klijentske aplikacije koji se povezuju sa Azure pretraživanja.

U ovom primjeru smo obuhvaćaju tipku upita da biste lakše poboljšajte preporučeni je način korištenja tipku upita u klijentskim aplikacijama.

Sljedeće snimku zaslona prikazuje **config.js** otvorena u tekst editor, s odgovarajućim stavkama demarcated tako da možete vidjeti gdje da biste ažurirali datoteke s vrijednostima koje su valjane servisa za pretraživanje.

![][5]


## <a name="host-a-runtime-environment-for-the-sample"></a>Glavno računalo okruženje za izvođenje ogledne

Uzorak zahtijeva HTTP-poslužitelju, koje možete instalirati globalno pomoću npm.

Korištenje u prozoru ljuske PowerShell za sljedeće naredbe.

1. Dođite do mape koja sadrži datoteku **package.json** .
2. Vrsta `npm install`.
2. Vrsta `npm install -g http-server`.

## <a name="build-the-index-and-run-the-application"></a>Stvaranje indeksa i pokrenuti aplikaciju

1. Vrsta `npm run indexDocuments`.
2. Vrsta `npm run build`.
3. Vrsta `npm run start_server`.
4. Izravni preglednika na`http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>USGS podataka za pretraživanje

Skup podataka USGS sadrži zapise koje važne da biste na stanje sustava Rhode Otok. Ako kliknete **pretraživanja** na prazan pretragu, dobit ćete gornji 50 stavke koja je zadana postavka.

Unos pojam za pretraživanje steći tražilica nešto da biste prešli. Pokušajte unos regionalne imena. "Roger Williams" je prvi governor od Rhode Otok. Nakon što ga imenovane su brojne parkova, zgrade i škole.

![][9]

Nije moguće i isprobajte neki od ovih uvjeta:

- Pawtucket
- Pembroke
- goose + Zelenortski


## <a name="next-steps"></a>Daljnji koraci

Ovo je prvi vodič Azure pretraživanje na temelju NodeJS i USGS skupu podataka. S vremenom ćete možemo proširiti ovog praktičnog vodiča da bismo pokazali značajke dodatne pretraživanja možda želite koristiti u Prilagođena rješenja.

Ako već imate malo pozadinskih Azure pretraživanja, možete koristiti ovaj uzorak kao u springboard za pokušaja suggesters (Vrsta dovršena prije ili samodovršetka upita), filtri i slojevito navigacije. Možete poboljšati i nakon stranice s rezultatima pretraživanja dodavanjem broji i grupnog slanja promjena dokumente da bi korisnici mogli pomicali kroz rezultate.

Novi korisnik Azure pretraživanje? Preporučujemo da druge vodiče za razvoj razumijevanja možete stvoriti korištenja probne verzije. Posjetite naš [dokumentaciju stranice](https://azure.microsoft.com/documentation/services/search/) da biste pronašli dodatne resurse. Možete pogledati i veze u našem [videozapis i vodiča popis](search-video-demo-tutorial-list.md) da biste pristupili informacijama.

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png
