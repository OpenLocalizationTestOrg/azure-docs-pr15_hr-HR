<properties
    pageTitle="Početak rada s Azure pretraživanja u Java | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Upute za stvaranje aplikacija za pretraživanje glavnom računalu oblak na Azure pomoću Java kao programski jezik."
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

# <a name="get-started-with-azure-search-in-java"></a>Početak rada s Azure pretraživanja u Java
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Saznajte kako stvoriti prilagođenu aplikaciju Java pretraživanja koja koristi Azure traženje njegov pretraživanju. Pomoću ovog praktičnog vodiča koristi [Azure pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/dn798935.aspx) za sastavljanje objekte i postupaka koji se koriste u ovu vježbu.

Da biste pokrenuli ovaj uzorak, morate imati servisa Azure pretraživanja koji se možete prijaviti na [Portal za Azure](https://portal.azure.com). Detaljne upute potražite u članku [Stvaranje servis za pretraživanje Azure na portalu](search-create-service-portal.md) .

Sastavljanje i testiranje ovaj primjer koristi sljedeće programe:

- [Eclipse IDE za razvojne inženjere EE Java](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Obavezno preuzmite EE verziju. Neki od koraka za potvrdu potreban je značajka koja se nalazi se samo u ovom izdanju.

- [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a>O podacima

U ovom primjeru aplikacije koristi podatke iz u [Sjedinjenim Američkim Državama Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtrirano na na stanje sustava Rhode Otok da biste smanjili veličinu skupa podataka. Da biste sastavili aplikacije za pretraživanje koji vraća zgrade obližnjim objektima kao što su hospitals i škole, kao i geological značajke kao što su strujanja, jezera smrznu i summits ćemo koristiti te podatke.

U ovoj aplikaciji **SearchServlet.java** program stvara i učitava indeksu koji koriste konstrukta [Indeksiranje](https://msdn.microsoft.com/library/azure/dn798918.aspx) , dohvaćanje filtrirani skup podataka USGS iz javne baze podataka SQL Azure. Unaprijed definirane vjerodajnice i podatke za povezivanje s izvorom podataka online isporučuju se programski kod. Pomoću pristupa podacima, potrebno je bez daljnje konfiguracije.

> [AZURE.NOTE] Ne možemo primijeniti filtar na ovaj skup podataka da biste bili u odjeljku ograničenje 10 000 dokumenta od besplatnih cijene sloju. Ako koristite standardne sloju, to ograničenje primijeniti, a možete izmijeniti kod da biste koristili veći skup podataka. Detalje o kapacitet za svaki cijene sloju potražite u članku [ograničenja i ograničenjima](search-limits-quotas-capacity.md).

## <a name="about-the-program-files"></a>O datotekama programa

Sljedeći popis sadrži opis datoteke koje se odnose na ovaj uzorak.

- Search.jsp: Nudi korisničkog sučelja
- SearchServlet.java: Pruža metode (slično kontroler u MVC)
- SearchServiceClient.java: Zahtjevi za ručice HTTP
- SearchServiceHelper.java: Pomoćnik za klasu koja nudi statične metode
- Document.Java: Nudi podatkovnog modela
- Config.Properties: postavlja URL servisa za pretraživanje i ključ za API-ja
- Pom.XML: Maven ovisnosti

<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Pronađite naziv servisa i api-ključ servisa Azure pretraživanja

Sve pozive REST API-JA u Azure pretraživanja zahtijevaju Navedite URL servisa i api-ključ. 

1. Prijava na [Portal za Azure](https://portal.azure.com).
2. Na traci Skok kliknite **servis za pretraživanje** da biste dobili popis svih servisa Azure pretraživanje dodjeli za vašu pretplatu.
3. Odaberite servis koji želite koristiti.
4. Na nadzornoj ploči za servis, vidjet ćete pločice za ključne informacije, kao i ikona ključa za pristup tipki administrator.

    ![][3]

5. Kopirajte URL servisa i ključa za administratore. Morate ih kasnije, prilikom dodavanja **config.properties** datoteku.

## <a name="download-the-sample-files"></a>Preuzimanje oglednih datoteka

1. Idite na [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) na Github.

2. Kliknite **Preuzimanje poštanski broj**, spremite datoteku .zip na disku, a izdvojiti sve datoteke koje sadrži. Razmislite o izdvajanju datoteka u radni prostor Java da biste ga kasnije lakše pronaći projekta.

3. Ogledne datoteke su samo za čitanje. Desnom tipkom miša kliknite Svojstva mape, a zatim poništite atribut samo za čitanje.

Sve izmjene sljedeće datoteke i pokretanje izvješća bit će protiv datoteka u ovu mapu.  

## <a name="import-project"></a>Uvoz projekta

1. U Eclipse, odaberite **datoteka** > **Uvoz** > **Općenito** > **Postojeće projekte u radni prostor**.

    ![][4]

2. U **korijenskom direktoriju odaberite**, pronađite mapu koja sadrži ogledne datoteke. Odaberite mapu koja sadrži mapu .project. Projekt prikazivati na popisu **projekata** kao odabrane stavke.

    ![][12]

3. Kliknite **Završi**.

4. Prikaz i uređivanje datoteka pomoću **Programa Project Explorer** . Ako to još nije otvoren, kliknite **prozor** > **Pokaži prikaz** > **Project Explorer** ili korištenje prečac da biste ga otvorili.

## <a name="configure-the-service-url-and-api-key"></a>Konfiguriranje URL servisa i ključ za API-ja

1. U programu **Project Explorer**, dvokliknite **config.properties** da biste uredili postavke konfiguracije koja sadrži naziv poslužitelja i ključ za API-ja.

2. Pogledajte korake u ovom članku, gdje pronaći URL servisa i api-ključ [Azure Portal](https://portal.azure.com)da biste dobili vrijednosti sada će se unijeti u **config.properties**.

3. U **config.properties**zamijenite "Ključ za API-ja" api ključ za uslugu. Nakon toga naziv usluge (prvi komponenta http://servicename.search.windows.net URL-a) zamjenjuje "naziv usluge" u istu datoteku.

    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Konfiguriranje okruženja projekta, sastavljanje i izvođenja

1. U Eclipse u Project Explorer desnom tipkom miša kliknite projekt > **Svojstva** > **Pozornici projekta**.

2. Odaberite **modul za dinamičku Web**, **Java**i **JavaScript**.

    ![][6]

3. Kliknite **Primijeni**.

4. **Prozor**odabir > **Preference** > **Server** > **Runtime okruženja** > **dodatnoj.**.

5. Proširite Apache i odaberite verziju Apache Tomcat server već instaliran. Na našem sustavu ne možemo instalirati verzije 8.

    ![][7]

6. Na sljedećoj stranici navedite Instalacijski imenik Tomcat. Na računalu sa sustavom Windows to vjerojatno će C:\Program Files\Apache softver Foundation\Tomcat *verziju*.

6. Kliknite **Završi**.

7. **Prozor**odabir > **Preference** > **Java** > **instaliran JREs** > **Dodavanje**.

8. **Dodavanje JRE**odaberite **Standardnu VM**.

10. Kliknite **Dalje**.

11. U definiciji JRE, u JRE home, kliknite **imenika**.

12. Dođite do **Programske datoteke** > **Java** i odaberite na JDK ste prethodno instalirali. Važno je da na JDK kao odaberite na JRE.

13. U instaliran JREs odaberite **JDK**. Postavke trebala bi izgledati slično sljedeće snimku zaslona.

    ![][9]

14. Po želji odaberite **prozor** > **Web-pregledniku** > **Internet Explorer** da biste otvorili aplikaciju u prozoru za razmjenu vanjskih preglednika. Pomoću vanjske preglednika daje bolje iskustvo aplikaciju za Web.

    ![][8]

Sada ste izvršili zadatke konfiguracije. Nakon toga će vam Sastavljanje i pokrenite projekt.

## <a name="build-the-project"></a>Stvaranje projekta

1. U programu Project Explorer desnom tipkom miša kliknite naziv projekta i odaberite **Pokreni kao** > **Maven sastavljanje …** da biste konfigurirali projekta.

    ![][10]

8. Uređivanje konfiguraciji, u ciljevima, upišite "clean instalirati", a zatim **Pokreni**.

Poruke o statusu su Izlaz u prozor konzole. Trebali biste vidjeti SASTAVLJANJE USPJEH koji označava projekta ugrađen bez pogrešaka.

## <a name="run-the-app"></a>Pokrenite aplikaciju

U posljednjem koraku će pokrenuti aplikaciju u okruženju izvođenja lokalni poslužitelj.

Ako još niste naveli okruženje za izvođenje poslužitelja u Eclipse, morat ćete najprije.

1. U programu Project Explorer proširite **WebContent**.

5. Desnom tipkom miša kliknite **Search.jsp** > **Pokreni kao** > **pokrenuti na poslužitelju**. Odaberite poslužitelj za Apache Tomcat, a zatim kliknite **Pokreni**.

> [AZURE.TIP] Ako ste koristili koji nisu zadani prostor za pohranu u projekt, morat ćete izmijeniti **Pokrenuti konfiguracije** da postavite pokazivač na mjesto projekta da biste izbjegli pogreške pokretanja na poslužitelju. U programu Project Explorer desnom tipkom miša kliknite **Search.jsp** > **Pokreni kao** > **Pokretanje konfiguracije**. Odaberite poslužitelj za Apache Tomcat. Kliknite **argumente**. Kliknite **radni prostor** ili **Datotečnog sustava** da biste postavili mapu koja sadrži projekt.

Kada pokrenete aplikaciju, trebali biste vidjeti prozoru preglednika, koja omogućuje okvir za pretraživanje za unos uvjeta.

Pričekajte otprilike jedan minuta prije nego što kliknete **pretraživanja** da bi se dobilo vrijeme servisa da biste stvorili i učitavanje indeksa. Ako nailazite na pogrešku HTTP 404, jednostavno ćete morati pričekati malo više prije ponovnog pokušaja.

## <a name="search-on-usgs-data"></a>USGS podataka za pretraživanje

Skup podataka USGS sadrži zapise koje važne da biste na stanje sustava Rhode Otok. Ako kliknete **pretraživanja** na prazan pretragu, dobit ćete gornji 50 stavke koja je zadana postavka.

Unos pojam za pretraživanje steći tražilica nešto da biste prešli. Pokušajte unos regionalne imena. "Roger Williams" je prvi governor od Rhode Otok. Nakon što ga imenovane su brojne parkova, zgrade i škole.

![][11]

Nije moguće i isprobajte neki od ovih uvjeta:

- Pawtucket
- Pembroke
- goose + Zelenortski

## <a name="next-steps"></a>Daljnji koraci

Ovo je prvi vodič Azure pretraživanje na temelju Java i USGS skupu podataka. S vremenom ćete možemo proširiti ovog praktičnog vodiča da bismo pokazali značajke dodatne pretraživanja možda želite koristiti u Prilagođena rješenja.

Ako već imate malo pozadinskih Azure pretraživanja, možete koristiti ovaj uzorak kao u springboard za daljnje početak možda augmenting [stranice za pretraživanje](search-pagination-page-layout.md), ili implementacijom [slojevito navigacije](search-faceted-navigation.md). Možete poboljšati i nakon stranice s rezultatima pretraživanja dodavanjem broji i grupnog slanja promjena dokumente da bi korisnici mogli pomicali kroz rezultate.

Novi korisnik Azure pretraživanje? Preporučujemo da druge vodiče za razvoj razumijevanja možete stvoriti korištenja probne verzije. Posjetite naš [dokumentaciju stranice](https://azure.microsoft.com/documentation/services/search/) da biste pronašli dodatne resurse. Možete pogledati i veze u našem [videozapis i vodiča popis](search-video-demo-tutorial-list.md) da biste pristupili informacijama.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
