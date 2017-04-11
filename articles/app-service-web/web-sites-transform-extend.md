<properties
    pageTitle="Azure aplikacije servisa za web-aplikacije napredne konfiguracijske i proširenja"
    description="Koristiti XML dokument Transformation(XDT) deklaracije Pretvorba ApplicationHost.config datoteke u web-aplikaciju programa aplikacije servisa za Azure i da biste dodali privatne proširenja da biste omogućili Administracija prilagođene akcije."
    authors="cephalin"
    writer="cephalin"
    editor="mollybos"
    manager="wpickett"
    services="app-service"
    documentationCenter=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Azure aplikacije servisa za web-aplikacije napredne konfiguracijske i proširenja

Pomoću deklaracija [XML pretvorbe dokumenata](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) možete transformirati [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) datoteka u web-aplikaciji u servisu Azure aplikacije. Deklaracija XDT možete koristiti i da biste dodali privatne proširenja da biste omogućili prilagođene web app Administracija akcije. Ovaj članak sadrži nastavkom aplikaciju za web i Upravitelj uzorka koji omogućuje upravljanje postavkama i putem web-sučelja.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a id="transform"></a>Napredna konfiguracija kroz ApplicationHost.config
Platforme aplikacije servisa za pruža fleksibilnost i kontrolu za web-aplikacije konfiguracija. Iako standardne IIS ApplicationHost.config konfiguracijska datoteka nije dostupna za izravno uređivanje u aplikaciji servisa, platforma podržava deklarativno model pretvorbe ApplicationHost.config koji se temelji na XML dokument transformaciju (XDT).

Odražava funkcionalnost pretvorbe, stvorite datoteku ApplicationHost.xdt s XDT sadržaja i mjesto u odjeljku korijenskog web-mjesta (d:\home\site) [Kudu konzolu](https://github.com/projectkudu/kudu/wiki/Kudu-console). Možda ćete morati ponovno pokrenuti web-aplikaciju da bi promjene stupile na snagu.

Sljedeća Ogledna applicationHost.xdt pokazuje kako da biste dodali novi prilagođeni okruženje tjednog prikaza kalendara u web-aplikacije koje koristi PHP 5.4.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
                <fastCgi>
                    <application>
                        <environmentVariables>
                                <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
                        </environmentVariables>
                    </application>
                </fastCgi>
        </system.webServer>
    </configuration>


Datoteka zapisnika s pretvorbe stanja i detalje o dostupna iz korijena FTP u odjeljku LogFiles\Transform.

Dodatne primjere potražite u članku [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Napomena**<br />
Elementi na popisu module u odjeljku `system.webServer` ne može ukloniti ili ponovno naručiti, ali dodatke na popis mogućih.


##<a id="extend"></a>Proširivanje web-aplikaciju programa

###<a id="overview"></a>Pregled privatne proširenja web-aplikacije

Aplikacije servisa za podržava web app proširenja kao točku proširenja za administracijske akcije. Zapravo, neke značajke aplikacije servisa za platformu primjenjuju se kao unaprijed instalirana proširenja. Dok je instalirana platformu proširenja nije moguće mijenjati, možete stvoriti i konfigurirati privatne proširenja za vlastitu web-aplikacije. Ta je funkcija i ovisi XDT deklaracije. Ključni koraci za stvaranje nastavkom privatnih web app su sljedeće:

1. Web-aplikacije proširenje **sadržaja**: stvaranje bilo kojeg web-aplikacija podržava aplikacije servisa
2. Web-aplikacije proširenje **deklariranje**: Stvaranje ApplicationHost.xdt datoteke
3. Web-aplikacije nastavak **implementacije**: postavite sadržaja u mapi SiteExtensions u odjeljku`root`

Put odnosu put aplikacije koji je naveden u datoteci ApplicationHost.xdt moraju pokazivati interne veze za web-aplikacije. Promjene u datoteku ApplicationHost.xdt zahtijeva koš za smeće web app.

**Napomena**: dodatne informacije o te ključne elemente dostupna je na [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Detaljne primjer uključen za ilustraciju korake za stvaranje i omogućivanje nastavkom privatnih web app. Izvorni kod za upravitelj i primjer u kojem se nalazi iza mogu se preuzeti sa [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

###<a id="SiteSample"></a>Web app proširenje primjer: PHP Manager

Upravitelj PHP je nastavkom web app koja administratorima web app da biste pogledali i konfigurirati svoje postavke PHP pomoću web-sučelja umjesto da da biste izmijenili PHP .ini datoteka izravno. Zajedničke datoteke konfiguracije za PHP obuhvaćaju datoteke php.ini koja se nalazi u odjeljku programske datoteke i. user.ini datoteke koja se nalazi u korijenskoj mapi web-aplikacije. Budući da je datoteka php.ini izravno uređivati na platformi aplikacije servisa, proširenje PHP Upravitelj koristi s. user.ini datoteku da biste primijenili promjene postavki.

####<a id="PHPwebapp"></a>Upravitelj i web-aplikacije

Na početnoj stranici upravitelj i implementaciju je:

![TransformSitePHPUI][TransformSitePHPUI]

Kao što vidite, nastavkom web app je baš kao i obične web-aplikacije, ali pomoću dodatnih ApplicationHost.xdt datoteke smještene u korijenskoj mapi web-aplikacije (Dodatne informacije o datoteci ApplicationHost.xdt dostupne su u sljedećem odjeljku ovog članka).

Proširenje PHP Upravitelj stvorena pomoću predloška za Visual Studio ASP.NET MVC 4 web-aplikacije. Sljedeći prikaz u pregledniku rješenja prikazuje strukturu upravitelj i nastavak.

![TransformSiteSolEx][TransformSiteSolEx]

Je jedini posebni logiku koja su potrebna za datoteke/i da biste naznačili gdje se nalazi wwwroot direktorija web-aplikacije. Kao što prikazuje sljedeći primjer koda, okruženje varijable "KUĆNA" označava put do korijenske web-aplikacije i wwwroot put može biti konstruirana dodavanjem "site\wwwroot":

    /// <summary>
    /// Gives the location of the .user.ini file, even if one doesn't exist yet
    /// </summary>
    private static string GetUserSettingsFilePath()
    {
            var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
            if (rootPath == null)
            {
                rootPath = System.IO.Path.GetTempPath(); // For testing purposes
            };
            var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
            return userSettingsFile;
    }


Nakon što ste put direktorija, operacije/i obična datoteka možete koristiti za čitanje i pisanje datoteke.

Jednu točku Oprez s datotečnim nastavcima web app interpretira rukovanje interne veze.  Ako imate sve veze u HTML datoteke koje daju apsolutnim putovima Interna vezama na web-aplikaciju programa, morate osigurati te veze su prefiksom nazivu korijensko kućni broj. To je potrebno jer korijenske mape za vaš kućni broj trenutno "/`[your-extension-name]`/" umjesto se samo "/", pa sve interne veze morate ažurirati sukladno tome. Na primjer, pretpostavimo kod sadržava vezu za sljedeće:

`"<a href="/Home/Settings">PHP Settings</a>"`

Kada je veza dio nastavkom web app, vezu mora biti u sljedećem obliku:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Da biste zaobišli taj zahtjev nešto od sljedećeg pomoću samo relativni putovi unutar web-aplikacije ili, u slučaju aplikacije ASP.NET pomoću na `@Html.ActionLink` način na koji je za vas stvara odgovarajuće veze.

####<a id="XDT"></a>Datoteka applicationHost.xdt

Kod za vaš nastavak web app dolazi u odjeljku %HOME%\SiteExtensions\[vaše-proširenje-name]. Ne možemo ćete nazovite to korijenski kućni broj.  

Da biste registrirali pozivni broj web app s datotekom applicationHost.config, morate postavite datoteku s nazivom ApplicationHost.xdt u korijenskoj mapi kućni broj. Sadržaj datoteke ApplicationHost.xdt mora biti na sljedeći način:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.applicationHost>
                <sites>
                    <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
                        <!-- NOTE: Add your extension name in the application paths below -->
                        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
                        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
                            <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
                        </application>
                    </site>
                </sites>
        </system.applicationHost>
    </configuration>

Naziva odaberite naziv proširenje moraju imati isti naziv kao korijenske mape kućni broj.

To utječe dodavanja novog puta aplikacije da biste na `system.applicationHost` popis web-mjesta u odjeljku IO web-mjesta. Web-mjesta IO je web-mjesta administracije krajnju točku s vjerodajnicama određenog programa access. Sadrži URL `https://[your-site-name].scm.azurewebsites.net`.  

    <system.applicationHost>
        ...
        <site name="~1[your-website]" id="1716402716">
                <bindings>
                    <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
                    <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
                </bindings>
                <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
                <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
                <logFile logSiteId="false" />
                <application path="/" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
                </application>
                <!-- Note the custom changes that go here -->
                <application path="/[your-extension-name]" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
                </application>
            </site>
    </sites>
      ...
    </system.applicationHost>

###<a id="deploy"></a>Proširenje implementaciju aplikacije za web

Da biste instalirali web app pozivni broj, možete koristiti FTP da biste kopirali sve datoteke web-aplikaciji u `\SiteExtensions\[your-extension-name]` mape web-aplikacije koje želite instalirati datotečni nastavak.  Ne zaboravite kopirajte ApplicationHost.xdt datoteku na to mjesto. Ponovno pokrenite aplikaciju web da biste omogućili datotečni nastavak.

Trebali biste moći vidjeti vaš web app nastavak na:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Imajte na umu da se URL izgleda baš kao URL za web-aplikacije, osim što se koristi HTTPS i sadrži ".scm".

Nije moguće onemogućiti sve privatne (nije instalirana) datotečne nastavke za web-aplikaciju programa tijekom razvoja i istrage dodavanjem programa postavki aplikacije s ključem `WEBSITE_PRIVATE_EXTENSIONS` i vrijednost `0`.

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="whats-changed"></a>Što se promijenilo
* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png
 
