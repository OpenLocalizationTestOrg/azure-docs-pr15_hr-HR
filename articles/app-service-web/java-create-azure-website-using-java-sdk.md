<properties 
    pageTitle="Stvaranje web-aplikacijama u Azure aplikacije servisa za korištenje Azure SDK za Java" 
    description="Saznajte kako stvoriti web-aplikacijama na Azure aplikacije servisa za programski pomoću Azure SDK za Java." 
    tags="azure-classic-portal"
    services="app-service\web" 
    documentationCenter="Java" 
    authors="donntrenton" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="02/25/2016" 
    ms.author="v-donntr"/>


# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Stvaranje web-aplikacijama u Azure aplikacije servisa za korištenje Azure SDK za Java

<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>Pregled

Ovaj vodič pokazuje kako stvoriti Azure SDK-a za aplikaciju Java koji stvara web-aplikacijama u [Aplikacije servisa za Azure][], a zatim implementacija aplikacije na njega. Sastoji se od dva dijela:

- Dio 1 pokazuje kako izraditi Java aplikacije koji stvara web-aplikacijama.
- 2.dio pokazuje kako stvoriti jednostavan JSP "Pozdrav svijeta" aplikacije, a zatim pomoću programa FTP klijent za implementaciju kod za aplikacije servisa.


## <a name="prerequisites"></a>Preduvjeti

### <a name="software-installations"></a>Instalacija softvera

Kod AzureWebDemo aplikacije u ovom članku napisan pomoću Azure Java SDK 0.7.0, koje možete instalirati pomoću [Installer platformu Web][] (WebPI). Osim toga, provjerite je li koristiti najnoviju verziju programa [Azure komplet alata za Eclipse][]. Nakon što instalirate SDK, ažuriranje zavisnosti u projektu Eclipse tako da pokrenete **Ažuriranje kazala** u **Spremištima Maven**, a zatim ponovno dodati najnoviju verziju svaki paket u prozoru **ovisnosti** . Verzija instaliranog programa u Eclipse možete provjeriti tako da kliknete **Pomoć > Detalji o instalaciji**; imat ćete barem sljedeće verzije:

- Paket za Microsoft Azure biblioteke Java 0.7.0.20150309
- Eclipse IDE za razvojne inženjere EE Java 4.4.2.20150219


### <a name="create-and-configure-cloud-resources-in-azure"></a>Stvaranje i konfiguriranje oblaka resursa u Azure

Prije nego počnete postupak, morate imati pretplatu Azure active i postavili zadanu Active Directory (AD) na Azure.


### <a name="create-an-active-directory-ad-in-azure"></a>Stvaranje Active Directory (AD) u Azure

Ako već nemate sustava Active Directory (AD) u sklopu Azure pretplate, prijavite se u [Azure klasični portal][] pomoću Microsoftova računa. Ako imate više pretplata, kliknite **pretplate** i odaberite zadani imenik za pretplatu koju želite koristiti za taj projekt. Zatim kliknite **Primijeni** da biste se prebacili na taj prikaz pretplate.

1. Na izborniku slijeva odaberite **Servisa Active Directory** . **Kliknite Novo > direktorija > Stvaranje prilagođenog**.

2. **Dodavanje direktorija**, odaberite **Stvaranje novog direktorija**.

3. U odjeljak **naziv**unesite naziv direktorija.

4. U **domeni**, unesite naziv domene. To je naziv osnovni domene koje je uključeno po zadanom direktorija; sadrži obrazac `<domain_name>.onmicrosoft.com`. Možete nazvati ga na temelju naziv imenika ili neki drugi naziv domene koji ste vlasnik. Naknadno možete dodati neki drugi naziv domene vaša tvrtka ili ustanova već koristi.

5. U **državi ili regiji**, odaberite svoje regionalne postavke.

Dodatne informacije o AD potražite u članku [što je u imeniku Azure AD][]?


### <a name="create-a-management-certificate-for-azure"></a>Stvaranje certifikata za upravljanje za Azure

Azure SDK Java koristi upravljanje certifikata za provjeru s Azure pretplate. To su X.509 v3 certifikate koju koristite za provjeru klijentska aplikacija koja koristi upravljanje API servisa djelovanje ime vlasnik pretplate za upravljanje resursima za pretplatu.

Kod u ovom postupku koristi se samopotpisani certifikat za provjeru s Azure. Ovaj postupak, morate Stvaranje certifikata pa prenesite [Azure klasični portal][] prije toga. To obuhvaća sljedeće korake:

- Generiranje PFX datoteke koji predstavlja klijentski certifikat i spremite je na lokalno.
- Generiranje upravljanje certifikat (CER datoteka) iz datoteke PFX.
- Prijenos datoteke CER u pretplatu za Azure.
- Datoteku možete pretvoriti PFX u JKS, jer Java koristi taj oblik za provjeru autentičnosti pomoću certifikata.
- Unesite kod provjere autentičnosti aplikacije koje se odnosi na lokalni JKS datoteku.

Kada dovršite ovaj postupak, certifikat CER će se nalaziti u pretplatu za Azure i certifikat JKS će se nalaziti na lokalni disk. Dodatne informacije o upravljanje certifikata potražite u članku [stvoriti i prenijeti upravljanje certifikat za Azure][].


#### <a name="create-a-certificate"></a>Stvaranje certifikata

Da biste stvorili vlastiti samopotpisani certifikat, otvorite konzolu operacijskog sustava i izvršite sljedeće naredbe.

> **Bilješke:**  Računalo na kojem pokretanja naredbe mora imati JDK instaliran. Osim toga, put do na keytool ovisi o mjestu instalirati na JDK. Dodatne informacije potražite u članku [ključ i alat za upravljanje certifikat (keytool)][] u Java dokumenata na mreži.

Da biste stvorili .pfx datoteka:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

Da biste stvorili .cer datoteka:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

pri čemu je:

- `<java-install-dir>`je put do direktoriju u kojem je instaliran Java.
- `<keystore-id>`unos identifikator keystore (na primjer, `AzureRemoteAccess`).
- `<cert-store-dir>`put direktoriju u kojem želite pohraniti certifikata (na primjer `C:/Certificates`).
- `<cert-file-name>`Naziv datoteka certifikata (na primjer `AzureWebDemoCert`).
- `<password>`Lozinka odlučite zaštititi potvrdi; mora biti najmanje 6 znakova. Možete unijeti bez lozinke, iako to ne preporučuje.
- `<dname>`Razlikovni naziv X.500 biti povezane s pseudonim pa se koristi kao polja izdavač i predmet u samopotpisani certifikat.

Dodatne informacije potražite u članku [Stvaranje i prijenos upravljanje certifikat za Azure][].


#### <a name="upload-the-certificate"></a>Prijenos certifikata

Da biste prenijeli samopotpisani certifikat za Azure, idite na stranicu **Postavke** na portalu klasični, a zatim kliknite karticu **Upravljanje potvrde** . Kliknite **Prijenos** pri dnu stranice, a zatim otvorite mjesto datoteke CER koju ste stvorili.


#### <a name="convert-the-pfx-file-into-jks"></a>PFX datoteku možete pretvoriti u JKS

U sustavu Windows naredbeni redak (pokrenut kao administrator), CD-a u direktorij koja sadrži certifikate i pokrenite sljedeću naredbu, gdje `<java-install-dir>` je direktoriju u kojem ste instalirali Java na vašem računalu:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Kada se to od vas zatraži, unesite lozinku za keystore odredište; To će biti lozinke za JKS datoteku.

2. Kada se to od vas zatraži, unesite lozinku za keystore izvora; To je lozinka koju ste naveli za datoteku PFX.

Dvije lozinke ne morate biti isti. Možete unijeti bez lozinke, iako to ne preporučuje.


## <a name="build-a-web-app-creation-application"></a>Stvaranje aplikacije za stvaranje web-aplikacije

### <a name="create-the-eclipse-workspace-and-maven-project"></a>Stvaranje radnog prostora Eclipse i Maven projekta

U ovom odjeljku stvorite web-mjesto radnog prostora i u okvir za Maven project web app stvaranje aplikacije, pod nazivom AzureWebDemo.

1. Stvorite novi projekt Maven. Kliknite **Datoteka > novo > projekta Maven**. U **Novi projekt Maven**, odaberite **Stvaranje jednostavne projekta** i **Korištenje zadano mjesto za radni prostor**.

2. Na drugoj stranici **Novi projekt Maven**navedite sljedeće:

    - ID grupe:`com.<username>.azure.webdemo`
    - ID artefakt: AzureWebDemo
    - Verzija: 0.0.1-SNAPSHOT
    - Pakiranje: posudu
    - Naziv: AzureWebDemo

    Kliknite **Završi**.

3. Otvorite datoteku pom.xml novi projekt u Project Explorer. Odaberite karticu **ovisnosti** . Ovo je novi projekt, bez paketa navedene su još.

4. Otvorite prikaz Maven spremišta. **Prozora kliknite > Pokaži prikaz > druge > Maven > Maven spremištima** i kliknite **u redu**. Prikaz **Maven spremištima** pojavit će se pri dnu zaslona u IDE.

5. Otvaranje **Globalni spremišta**, desnom tipkom miša kliknite **središnje** spremište i odaberite **Ponovno stvaranje indeksa**.

    ![][1]
    
    Ovaj korak može potrajati nekoliko minuta ovisno o brzini veze. Kada ponovno indeks, trebali biste vidjeti paketi za Microsoft Azure u **središnjem** spremniku Maven.

6. U **ovisnosti**, kliknite **Dodaj**. U **Grupi Enter ID-a...** unesite `azure-management`. Odaberite paketi za osnovnu upravljanje i upravljanje aplikacije servisa web-aplikacije:

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

    > **Bilješke:** Ako ažurirate ovisnosti nakon novo izdanje verziju ćete morati ponovno dodajete svaku ovisnosti na ovom popisu.
    > Nakon što kliknite **Dodaj** i odaberite svaku ovisnost, prikazuje se s broj na popisu **ovisnosti** nove verzije.

Kliknite **u redu**. Azure paketa pojaviti na popisu **ovisnosti** .


### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>Pisanja koda programskog jezika Java da biste stvorili web-aplikacijama tako da nazovete Azure SDK

Nakon toga unesite kod koji poziva API-ji u Azure SDK za Java da biste stvorili web-aplikaciji aplikacije servisa.

1. Stvaranje klase jezika Java sadrži točke kod glavni unos. U programu Project Explorer desnom tipkom miša kliknite čvor projekta i odaberite **Novo > klase**.

2. U **Novi klase jezika Java**naziv klase `WebCreator` i potvrdite okvir **javno statični glavne Poništi** . Odabiri trebao izgledati ovako:

    ![][2]

3. Kliknite **Završi**. Pojavit će se WebCreator.java datoteke u programu Project Explorer.


### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Pozivanje Azure API-JA za stvaranje web-aplikacije programa aplikacije servisa


#### <a name="add-necessary-imports"></a>Dodajte potrebne uvozi

U WebCreator.java, dodajte sljedeće uvozi; ove se uvozi omogućuje pristup klase u biblioteke upravljanja za troše Azure API-ji:

    // General imports
    import java.net.URI;
    import java.util.ArrayList;
    
    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;
    
    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;
    
    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;
    
    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-the-main-entry-point-class"></a>Definiranje točke predmete glavni unos

Budući da je svrha AzureWebDemo aplikaciju za stvaranje web-aplikacije programa aplikacije servisa, naziv glavnog klase za ovu aplikaciju `WebAppCreator`. Klase nudi kod točke glavni unos koji poziva upravljanje API usluge za Azure da biste stvorili web-aplikaciji.

Dodajte sljedeće definicija parametara za web-aplikacije i webspace. Morat ćete dobivenih vlastite Azure pretplate ID i certifikata.

    public class WebAppCreator {
    
        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";
    
        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

pri čemu je:

- `<subscription-id>`je ID Azure pretplate u kojoj želite stvoriti resurs.
- `<certificate-store-path>`je put i naziv datoteke JKS datoteku u direktoriju spremište lokalne certifikata. Na primjer, `C:/Certificates/CertificateName.jks` za Linux i `C:\Certificates\CertificateName.jks` za Windows.
- `<certificate-password>`To je lozinka koje ste naveli prilikom stvaranja JKS certifikata.
- `webAppName`može biti bilo koje ime koje ste odabrali; Ovaj postupak koristi naziv `WebDemoWebApp`. Punim nazivom domene je na `webAppName` s na `domainName` dodan, pa u tom slučaju cijelog domena je `webdemowebapp.azurewebsites.net`.
- `domainName`mora biti navedena kao što je prikazano gore.
- `webSpaceName`mora biti jedna od vrijednosti definirano u predmete [WebSpaceNames][] .
- `appServicePlanName`mora biti navedena kao što je prikazano gore.

> **Bilješke:** Svaki put kada pokrenete ovu aplikaciju, morate promijeniti vrijednost `webAppName` i `appServicePlanName` (ili brisanje web-aplikaciji portala za Azure) prije pokretanja aplikacije. U suprotnom, izvođenja neće uspjeti jer već postoji isti resurs na Azure.


#### <a name="define-the-web-creation-method"></a>Definiranje način stvaranja web

Nakon toga definirati način da biste stvorili web-aplikaciji. Ta metoda `createWebApp`, navodi parametre web-aplikacije i u webspace. Također stvara i konfigurira klijent za upravljanje web-aplikacije servisa za aplikaciju koja je definirana [WebSiteManagementClient][] objekt. Klijent za upravljanje je ključ za stvaranje web-aplikacije. Pruža RESTful web-usluge koje omogućuju aplikacija za upravljanje web-aplikacije (izvođenje operacije kao što su što stvaranje, ažuriranje i brisanje) tako da nazovete podršku za upravljanje servisom API-JA.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

Kod će izlaz status HTTP odgovor koji označava uspjelo ili nije, a ako ne uspije, će izlaz naziv stvoreno web-aplikacije.


#### <a name="define-the-main-method"></a>Definiranje metodu main()

Navedite šifru načina main() koja poziva createWebApp() da biste stvorili web-aplikaciji.

Na kraju, poziva `createWebApp` iz `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Pokrenite aplikaciju i provjerite je li stvaranja web app

Da biste potvrdili da se pokreće aplikaciju, kliknite **Pokreni > Pokreni**. Kada aplikacija dovrši izvođenje, trebali biste vidjeti sljedeće Izlaz na konzoli Eclipse:

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

Prijavite se na portal sustava Azure klasični, a zatim kliknite **Web-aplikacije**. Nova web-aplikaciji prikazivati na popisu web-aplikacije za nekoliko minuta.


## <a name="deploying-an-application-to-the-web-app"></a>Implementacija aplikacije na web-aplikaciji

Nakon što ste pokrenuli AzureWebDemo i stvara novu web-aplikaciju, prijavite se na portal sustava klasični, kliknite **Web-aplikacije**pa odaberite **WebDemoWebApp** na popisu **Web-aplikacije** . U web-aplikaciji stranice nadzorne ploče, kliknite **Pregledaj** (ili klikom na URL `webdemowebapp.azurewebsites.net`) da biste prešli na nju. Vidjet ćete stranicu prazno rezervirano mjesto jer nema sadržaja je objavljen na web-aplikaciji još.

Zatim će stvoriti aplikaciju "Pozdrav svijeta" i implementacija web-aplikaciji.


### <a name="create-a-jsp-hello-world-application"></a>Stvaranje aplikacije JSP pozdrav svijeta

#### <a name="create-the-application"></a>Stvaranje aplikacije

Da bi se Demonstracija za implementaciju aplikacije na webu, u nastavku prikazuje kako stvoriti jednostavan "Pozdrav svijeta" Java aplikacije i prijenos na web-aplikaciji servisa aplikacija stvorenog u aplikaciji.

1. Kliknite **Datoteka > novo > projekta dinamički Web**. Nazovite ga `JSPHello`. Ne morate promijeniti ostale postavke u dijaloškom okviru. Kliknite **Završi**.

    ![][3]

2. U programu Project Explorer proširenje projekta **JSPHello** , desnom tipkom miša kliknite **WebContent**, a zatim kliknite **Novo > datoteke JSP**. U dijaloškom okviru Nova datoteka JSP naziv nove datoteke `index.jsp`. Kliknite **Dalje**.

3. U dijaloškom okviru **Odabir predloška JSP** odaberite **Novu datoteku JSP (html)** , a zatim kliknite **Završi**.

4. U index.jsp, dodajte sljedeći kod u na `<head>` i `<body>` označavanje sekcija:

        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
    
        <body>
          Hello, the time is <%= date %> 
        </body>


#### <a name="run-the-hello-world-application-in-localhost"></a>Pokrenite aplikaciju pozdrav svijeta u localhost

Prije nego što pokrenete ovu aplikaciju, morate konfigurirati nekoliko svojstava.

1. Desnom tipkom miša kliknite **JSPHello** projekta, a zatim odaberite **Svojstva**.

2. U dijaloškom okviru **Svojstva** : odaberite **Java sastavljanje put**, odaberite karticu **narudžbe i izvoz** , potvrdite **JRE biblioteka sustava**, a zatim kliknite **gore** da biste je premjestili na vrh popisa.

    ![][4]

3. U dijaloškom okviru **Svojstva** : odaberite **Namijenjeno Runtimes** , a zatim kliknite **Novo**.

4. U dijaloškom okviru **Novo okruženje za izvođenje poslužitelja** odaberite poslužitelj kao što su **Apache Tomcat v7.0** , a zatim kliknite **Dalje**. U dijaloškom okviru **Tomcat poslužitelja** postavite **naziv** `Apache Tomcat v7.0`, i postavite **Tomcat Instalacijski imenik** direktoriju u kojem je instaliran verzije Tomcat poslužitelja koji želite koristiti.

    ![][5]

    Kliknite **Završi**.

5. Zatim vratili na stranicu **Namijenjeno Runtimes** dijaloškog okvira **Svojstva** . Odaberite **Apache Tomcat v7.0**, a zatim kliknite **u redu**.

    ![][6]

6. Na izborniku Eclipse **pokretanje** kliknite **Pokreni**. U dijaloškom okviru **Pokreni kao** odaberite **Pokreni na poslužitelju**. U dijaloškom okviru **pokretanje na poslužitelju** odaberite **Tomcat v7.0 poslužitelja**:

    ![][7]

    Kliknite **Završi**.

7. Kada se pokrene aplikaciju, trebali biste vidjeti stranici **JSPHello** pojavljuju se u prozoru localhost u Eclipse (`http://localhost:8080/JSPHello/`), prikazuje se sljedeća poruka:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="export-the-application-as-a-war"></a>Izvoz aplikacije u obliku na WAR

Izvoz datoteke projekta web kao web-arhive (WAR) datoteke tako da se može uvesti web-aplikaciji. Sljedeće datoteke projekta web nalaze se u mapi WebContent:

    META-INF
    WEB-INF
    index.jsp

1. Desnom tipkom miša kliknite mapu WebContent, a zatim odaberite **Izvoz**.

2. U dijaloškom okviru **Izvoz odaberite** kliknite **Web > WAR** datoteku, a zatim kliknite **Dalje**.

3. U dijaloškom okviru **Izvoz WAR** odaberite src direktorija u trenutni projekt, a obuhvaćaju naziv datoteke WAR na kraju. Ako, na primjer:

    `<project-path>/JSPHello/src/JSPHello.war`

Dodatne informacije o implementaciji WAR datoteke, potražite u članku [Dodavanje Java aplikacije na web-aplikacije za Azure aplikacije servisa](web-sites-java-add-app.md).


### <a name="deploying-the-hello-world-application-using-ftp"></a>Implementacija aplikacije svijeta pozdrav pomoću FTP

Odaberite proizvođača FTP klijent za objavljivanje aplikacija. U ovom postupku opisano dvije mogućnosti: konzole Kudu ugrađenu Azure; i FileZilla, alat za popularne s jednostavan, grafičkog Sučelja.

> **Bilješke:** Komplet alata za Azure za Eclipse podržava implementaciju web-mjesto za pohranu računa i u okvir za servise u oblaku, ali trenutno ne podržava implementaciju na web-aplikacije. Možete implementirati račune za pohranu i pomoću programa Project za implementaciju Azure, kao što je opisano u [Stvaranje pozdrav svijeta aplikacije za Azure u Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx)servise u oblaku, ali ne na web-aplikacije. Koristiti druge metode kao što su FTP ili GitHub za prijenos datoteka na web-aplikaciju.

> **Bilješke:** Ne preporučujemo korištenje FTP iz naredbenog retka sustava Windows (naredbenog retka FTP.EXE uslužni koji se isporučuje sa sustavom Windows). Klijenti FTP koje koristi active FTP, kao što su FTP.EXE, često se neće raditi putem vatrozida. Aktivni FTP određuje internu koji se temelji na LAN adresu, na koji FTP poslužitelj vjerojatno neće se povezati.

Dodatne informacije o implementaciji sustava aplikacije servisa za web-aplikaciju pomoću FTP potražite u sljedećim temama:

- [Implementacija pomoću uslužni program za FTP](web-sites-deploy.md)


#### <a name="set-up-deployment-credentials"></a>Postavljanje vjerodajnica za implementaciju

Provjerite je li pokrenete aplikaciju **AzureWebDemo** da biste stvorili web-aplikacijama. Datoteka će se prenijeti na ovom mjestu.

1. Prijavite se u klasične portal i kliknite **Web-aplikacije**. Provjerite je li **WebDemoWebApp** pojavljuje se na popisu web-aplikacije pa provjerite je li pokrenut. Kliknite **WebDemoWebApp** da biste otvorili njezinu stranicu **nadzorne ploče** .

2. Na stranici **nadzorne ploče** u odjeljku **Brzi Glance**, kliknite **Postavljanje vjerodajnica za implementaciju** (Ako već imate vjerodajnice za implementaciju, to čita **ponovno postavljanje vjerodajnica za implementaciju**).

    Vjerodajnice za implementaciju su povezani s Microsoftovim računom. Morate navesti korisničko ime i lozinku koje koristite za implementaciju pomoću brojka i FTP. Možete koristiti te vjerodajnice za implementaciju bilo koju aplikaciju za web u sve Azure pretplate povezan s Microsoftova računa. Unesite brojka i FTP vjerodajnice za implementaciju u dijaloškom okviru i bilježiti korisničko ime i lozinku za buduću upotrebu.


#### <a name="get-ftp-connection-information"></a>Dohvati podatke o vezi FTP

Da biste koristili FTP za implementaciju datoteka aplikacije na novostvorenom web-aplikaciju, morate dobiti informacije o vezi. Da biste dobili informacije o vezi na dva načina. Način je posjetite stranicu **nadzorne ploče** za web-aplikaciju; drugi način je da biste preuzeli web aplikacije objavljivanje profila. Objavljivanje profil je XML datoteku koja sadrži podatke kao što su FTP glavno računalo naziv i vjerodajnice za prijavu za web apps na servisu Azure aplikacije. Koristite ovaj korisničko ime i lozinku za implementaciju bilo koju aplikaciju za web u sve pretplate povezanu s računom za Azure, ne samo ova.

Da biste dobili informacije o povezivanju FTP iz web-aplikaciji plohu [Azure Portal][]:

1. U odjeljku **Osnove**, pronađite i kopirajte **FTP naziv glavnog računala**. To je slično kao URI `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.

2. U odjeljku **Osnove**, pronađite i kopirajte **FTP/implementacije korisničko ime**. To će imati obrazac *webappname\deployment username*; na primjer `WebDemoWebApp\deployer77`.

Da biste dobili informacije o povezivanju FTP iz profila za objavljivanje:

1. U web-aplikaciji plohu, kliknite **Dohvati objavljivanje profila**. To će preuzimanje datoteke .publishsettings na lokalni disk.

2. Otvorite datoteku .publishsettings u XML uređivaču ili uređivač teksta i pronaći u `<publishProfile>` koja sadrži element `publishMethod="FTP"`. Trebao bi izgledati ovako:

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. Imajte na umu da web-aplikaciji `publishProfile` postavke karte postavke FileZilla Upravitelj web-mjesta na sljedeći način:

- `publishUrl`je isti kao **glavno računalo FTP**vrijednost postavljenu na **glavno računalo**.
- `publishMethod="FTP"`označava **protokol** skupa **FTP - File Transfer Protocol**i **šifriranje** **koristi običan FTP**.
- `userName`i `userPWD` su tipke za stvarni korisničko ime i lozinku vrijednosti koje ste naveli kada ponovno postavljanje vjerodajnica za implementaciju. `userName`jednak **implementaciju / FTP korisnika**. Oni mapirajte **korisnika** i **lozinke** u FileZilla.
- `ftpPassiveMode="True"`znači da FTP mjesto koristi pasivni FTP prijenos; na kartici **Postavke za prijenos** odaberite **pasivni** .


#### <a name="configure-the-web-app-to-host-a-java-application"></a>Konfiguriranje web-aplikaciju za hostiranje Java aplikacije

Prije nego što objavite aplikaciju, morate promijeniti neke postavke konfiguraciju tako da se web-aplikaciju možete hostirati Java aplikacije.

1. Na portalu klasični idite na stranica **nadzorne ploče** na web-aplikaciju i kliknite **Konfiguriraj**. Na stranici **Konfiguracija** navedite sljedeće postavke.

2. U **verziji jezika Java** zadana vrijednost je **izvan**; Odaberite verziju jezika Java vaše aplikacije ciljnih web-mjesta; na primjer 1.7.0_51. Kada to učinite, provjerite upućuje li **spremnik Web** postavljena na verziju Tomcat poslužitelja.

3. U **Zadani dokumenata**, dodajte index.jsp i premjestite na vrh popisa. (Zadana datoteka za web-aplikacije je hostingstart.html).

4. Kliknite **Spremi**.


#### <a name="publish-your-application-using-kudu"></a>Objavljivanje aplikacija pomoću Kudu

Jedan od načina za objavljivanje aplikacija je konzole za ispravljanje pogrešaka Kudu ugrađenu Azure. Kudu poznato je da se stabilan i dosljedni s web-aplikacije servisa za aplikacije i Tomcat poslužitelja. Pristup konzole za web-aplikaciji pregledavanjem URL obrasca za sljedeće:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. U ovom postupku konzole za Kudu nalazi se na sljedeći URL; Idite na ovom mjestu:

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. Na gornjoj izborniku odaberite **konzole za ispravljanje pogrešaka > CMD**.

3. U naredbeni redak konzole dođite do `/site/wwwroot` (ili kliknite `site`, zatim `wwwroot` u prikazu direktorija pri vrhu stranice):

    `cd /site/wwwroot`

4. Nakon što odredite **Java verziju**, Tomcat server potrebno stvoriti direktorij webapps. U naredbeni redak konzole idite do webapps:

    `mkdir webapps`

    `cd webapps`

5. Povucite JSPHello.war iz `<project-path>/JSPHello/src/` i ispustite ga u prikazu direktorija Kudu pod `/site/wwwroot/webapps`. Ne povucite ga u područje "Povucite ovdje da biste prenijeli i zip" jer Tomcat će je raspakiraj.

  ![][8]

Pri prvom JSPHello.war pojavit će se u području direktorija sam:

  ![][9]

U kratko vrijeme (vjerojatno manje od pet minuta) poslužitelja Tomcat će raspakiraj datoteku WAR u raspakirane imenik za JSPHello. Kliknite KORIJENSKI direktorij da biste vidjeli li index.jsp raspakirana i kopirati postoji. Ako je tako, idite natrag u direktoriju webapps da biste vidjeli je li raspakirane direktorija JSPHello stvorena. Ako ne vidite te stavke, pričekajte i ponovite.

  ![][10]


#### <a name="publish-your-application-using-filezilla-optional"></a>Objavljivanje aplikacija pomoću FileZilla (neobavezno)

Neki drugi alat koji koristite za objavljivanje aplikacija je FileZilla, popularne FTP klijent proizvođača s jednostavan, grafičkog Sučelja. Možete preuzeti i instalirati FileZilla iz [http://filezilla-project.org/](http://filezilla-project.org/) ako već nemate ga. Dodatne informacije o korištenju klijentskog programa potražite u [dokumentaciji FileZilla](https://wiki.filezilla-project.org/Documentation) i u ovom blogu na [klijenti FTP - 4 dijela: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. U FileZilla, kliknite **Datoteka > Upravitelj web-mjesta**.
2. U dijaloškom okviru **Upravitelj web-mjesta** kliknite **Novo web-mjesto**. Nova prazna FTP mjesto će se pojaviti **Odaberite stavke** koje možete unijeti naziv upita. U ovom postupku nazovite ga `AzureWebDemo-FTP`.

    Na kartici **Općenito** navedite sljedeće postavke:
    - **Glavnog računala:** Unesite **Naziv glavnog računala FTP** koju ste kopirali na nadzornoj ploči.
    - **Priključka:** (Ostavite to prazno, kao što je to je pasivni prijenos i poslužitelj određuje priključak za korištenje)
    - **Protokol:** Protokol za prijenos datoteka FTP
    - **Šifriranja:** Koristi običan FTP
    - **Vrsta za prijavu:** Normalno
    - **Korisnika:** Unesite implementaciju / FTP korisnika koji ste kopirali iz na nadzornoj ploči. To je potpuna FTP korisničko ime, koja ima obrasca *webappname\username*.
    - **Lozinke:** Unesite lozinku koju ste naveli prilikom postavljanja vjerodajnice za implementaciju.

    Na kartici **Postavke za prijenos** odaberite **pasivni**.

3. Kliknite **Poveži**. Ako uspješan, konzole FileZilla, prikazat će se na `Status: Connected` poruku, a problem s `LIST` naredba popis sadržaja direktorija.

4. Na ploči s **lokalnom** web-mjesta, odaberite izvorišni direktorij u kojem se nalazi datoteka JSPHello.war; put bit će otprilike ovako:

    `<project-path>/JSPHello/src/`

5. Na ploči **udaljene** web-mjesta, odaberite odredišnu mapu. Će uvesti WAR datoteku na `webapps` imeničkog u odjeljku korijenskog web-aplikaciji. Dođite do `/site/wwwroot`, desnom tipkom miša kliknite `wwwroot`, a zatim odaberite **Stvori direktorija**. Naziv imenika `webapps` , a zatim unesite taj imenik.

6. Prijenos JSPHello.war za `/site/wwwroot/webapps`. Odaberite JSPHello.war na popisu **lokalne** datoteke, desnom tipkom miša kliknite ga, a zatim **Prijenos**. Prikazat će vam se prikazala u `/site/wwwroot/webapps`.

7. Kada kopirate JSPHello.war direktorij webapps, Tomcat Server automatski se otpakiravanje (raspakiraj) datoteke u datoteci WAR. Iako Tomcat poslužitelja započinje odmah Raspakiravanje, može potrajati dugu vrijeme (vjerojatno sati) datoteka koja se pojavljuju u FTP klijent.


#### <a name="run-the-hello-world-application-on-the-web-app"></a>Pokrenite aplikaciju pozdrav svijetu na web-aplikaciji

1. Nakon prijenosa datoteka WAR i provjeriti Tomcat poslužitelj je stvorio je raspakirane `JSPHello` direktorija, a zatim Pregledaj da biste `http://webdemowebapp.azurewebsites.net/JSPHello` li pokrenuti aplikaciju.

    > **Bilješke:** Ako kliknete **potražite** na portalu klasični, mogla bi vam se zadanu web-stranicu koja kaže "ovu web-aplikaciju Java temelji uspješno je stvorena." Možda ćete morati osvježiti web-stranicu da biste pregledavali izlaz aplikacije umjesto zadanu web-stranicu.

2. Kada se pokrene aplikaciju, trebali biste vidjeti na web-stranicu s sljedeće:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="clean-up-azure-resources"></a>Čišćenje Azure resursi

Ovaj postupak stvara web-aplikaciju programa aplikacije servisa. Dok god postoji će naplatiti za resurs. Ako planirate nastaviti koristiti web-aplikaciju za testiranje ili razvoju, razmislite o zaustavljanje ili izbrišete. Web-aplikacije koje je zaustavljena i dalje plaćati small trošak, ali ga ponovno pokrenite u bilo kojem trenutku. Brisanje web-aplikacijama briše sve podatke koje ste prenijeli na njega.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

  [1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
  [2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
  [3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
  [4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
  [5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
  [6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
  [7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
  [8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
  [9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
  [10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png
 

[Aplikacije servisa za Azure]: http://go.microsoft.com/fwlink/?LinkId=529714
[Instalacijski program platformu za web]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure komplet alata za Eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure klasični portal]: https://manage.windowsazure.com
[Što je u imeniku Azure AD]: http://technet.microsoft.com/library/jj573650.aspx
[Stvoriti i prenijeti upravljanje certifikat za Azure]: ../cloud-services/cloud-services-certs-create.md
[Ključ i alat za upravljanje certifikat (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Portal za Azure]: https://portal.azure.com
