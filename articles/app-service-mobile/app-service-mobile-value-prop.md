<properties
    pageTitle="Što su mobilne aplikacije"
    description="Saznajte koje prednosti ne aplikacije servisa za stavljanje mobilne aplikacije enterprise."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="getting-started"> </a>Što je aplikacija Mobile?

Aplikacije servisa za Azure je potpuno upravljanih [platforme kao Service](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) koja se nudi za razvojne inženjere za profesionalni koji objedinjuje bogatog skupa mogućnosti web-mjestu, mobilne i scenarija za integraciju. *Mobilne aplikacije* u *Aplikacije servisa za Azure* nude platforme za razvoj iznimno prilagodljivi, globalno dostupne mobilne aplikacije za Enterprise razvojnim inženjerima i Integrators sustav koji objedinjuje bogatog skupa mogućnosti mobilne programerima.

![Mobilne aplikacije](./media/app-service-mobile-value-prop/overview.png)

##<a name="why-mobile-apps"></a>Zašto mobilne aplikacije?
*Mobilne aplikacije* u *Aplikacije servisa za Azure* nudi platforme za razvoj iznimno prilagodljivi, globalno dostupne mobilne aplikacije za Enterprise razvojnim inženjerima i Integrators sustav koji objedinjuje bogatog skupa mogućnosti mobilne programerima. Pomoću mobilne aplikacije možete učiniti sljedeće:

- **Sastavljanje nativnih i unakrsni platformu aplikacije** – hoće li se stvara nativni iOS, Android i Windows aplikacije ili Xamarin ili Cordova (Phonegap) različite platforme aplikacije, možete iskoristiti aplikacije servisa pomoću nativnog SDK-ovi.
- **Povezivanje s velikim sustavima** – pomoću mobilne aplikacije možete dodati tvrtke prijavu na u minutama i povezivanje s enterprise lokalnih ili resursa u oblaku.
- **Stvaranje izvanmrežne spremna aplikacije za sinkronizaciju podataka** – da bi vaš mobilni snage produktivni, stvaranje aplikacije koji funkcioniraju izvanmrežno web-mjesta i pomoću mobilne aplikacije prilikom sinkronizacije podataka u pozadini povezivanje postoji u bilo kojem od izvora podataka tvrtke ili SaaS API-ji.
- **Automatske obavijesti da biste milijune u sekundama** - sudjelovati s klijentima putem izravnih automatske obavijesti na bilo kojem uređaju prilagoditi svojim potrebama šalje kada je vrijeme desno.

## <a name="mobile-app-features"></a>Značajke aplikacije za mobilne uređaje
Sljedeće značajke su važne oblaka omogućeno mobilne razvoj:

- **Provjera autentičnosti i autorizacije** – odaberite s popisa ikad rast davatelji identiteta, uključujući Azure Active Directory za provjeru enterprise plus društvenih davatelji kao što su Facebook, Google, Twitter i Microsoftov Account.  Azure mobilne aplikacije sadrži na servis OAuth 2.0 za svaki davatelja usluga.  Možete integrirati i SDK za davatelja identiteta za određene funkcije za davatelja usluga.

  Saznajte više o naše [značajke provjere autentičnosti].

- **Pristup podacima** – Azure mobilne aplikacije sadrži prikladna za mobilne OData v3 izvor podataka povezan s SQL Azure ili je lokalnog sustava SQL Server.  Taj servis mogu se temeljiti na Framework entitet, što omogućuje jednostavno integrirati s drugim NoSQL i davatelji podataka SQL, uključujući [Spremište tablica Azure], MongoDB, [DocumentDB] i SaaS API davatelji kao što su Office 365 i Salesforce.com.
- **Izvanmrežna sinkronizacija** - naše klijent SDK-ovi stvaranjem jednostavno izraditi robusne i odredište mobilne aplikacije koje rade s izvanmrežne podatke je postavili možete automatski sinkronizira s pozadinskom podatke, uključujući podršku za rješavanje sukoba.

  Saznajte više o naše [značajke podataka].

- **Automatske obavijesti** - naše klijent SDK-ovi jednostavno integrirati s mogućnostima Registracija koncentratora obavijesti Azure, što omogućuje slanje automatskih obavijesti milijune korisnika istodobno.

  Saznajte više o naše [automatske obavijesti značajke].

- **Klijent SDK-ovi** - dajemo potpunog skupa klijent SDK-ovi koji pokrivaju nativni razvoj ([iOS], [Android] i [Windows]), više platformi razvoj ([Xamarin za iOS i Android], [Xamarin obrazaca]) i razvoj aplikacija za hibridno ([Apache Cordova]).  Svaki klijent SDK nije dostupno u sklopu MIT licenca i Otvori izvor.

## <a name="azure-app-service-features"></a>Značajke aplikacije servisa za Azure.
Sljedeće značajke platformu obično korisne su za mobilne radnog mjesta.

- **Automatsko skaliranje** – aplikacije servisa omogućuje vam brzo skaliranje gore ili Smanji učiniti sve dolazne učitavanja klijenta. Ručno odaberite broj i veličinu VMs ili postaviti automatsko skaliranje za promjenu veličine mobilne aplikacije pozadinskog sustava na temelju učitavanja ili raspored.

  Saznajte više o [Automatska promjena veličine].

- **Pripremna okruženja** – aplikacije servisa za jer se može pokrenuti više verzija web-mjesta, što omogućuje izvođenje A / B testiranja, testirajte u radnog kao dio veći plan DevOps i lokalno pripremna nove pozadinske.

  Saznajte više o [pripremna okruženja].

- **Neprekinuti implementacije** – aplikacije servisa možete integrirati s uobičajeni sustavi IO, što vam omogućuje da automatski Implementirajte novu verziju sustava pozadinskog pritiskom granu IO sustava.

  Saznajte više o [mogućnostima implementacije].

- **Virtualna umrežavanje** - aplikacije servisa možete se povezati s lokalnog resursa virtualne veze na mreži, ExpressRoute ili hibridnog.

  Saznajte [više o hibridnim veze], [virtualne mreže]i [ExpressRoute].

- **Izolirani / namjenski okruženja** – aplikacije servisa za mogu se izvoditi u potpunosti Izolirani i namjenski okruženja za sigurno pokretanje aplikacije servisa za Azure aplikacije na visok mjerilo.  To idealna je za radnih opterećenja aplikacije koje je obavezna vrlo visoka mjerilo, odvajanja ili sigurne mrežni pristup.

  Saznajte više o [Okruženja aplikacije servisa].

## <a name="getting-started"></a>Početak rada ##
Za početak rada s aplikacijama Mobile, slijedite Praktični vodič za [Početak rada] .  To će pokrivaju osnove proizvodnje mobilne pozadinskog sustava i klijentski po izboru, a zatim integriranje provjeru autentičnosti, izvanmrežnu sinkronizaciju i slanje obavijesti.  Možete pratiti Praktični vodič za [Početak rada] nekoliko puta – jednom za svaki klijentske aplikacije.

Dodatne informacije o aplikacijama Mobile Azure pregledajte naš [tečaj karta].
Dodatne informacije o platforme Azure aplikacije servisa potražite u članku [Aplikacije servisa za Azure].

>[AZURE.NOTE]Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](https://tryappservice.azure.com/?appServiceName=mobile), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

<!-- URLs. -->
[Migrate your Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Aplikacije servisa za Azure]: ../app-service/app-service-value-prop-what-is.md
[Početak rada]: app-service-mobile-ios-get-started.md
[Spremište tablica platforme Azure]: ../storage/storage-getting-started-guide.md
[DocumentDB]: ../documentdb/documentdb-get-started.md
[značajke provjere autentičnosti]: ./app-service-mobile-auth.md
[značajke podataka]: ./app-service-mobile-offline-data-sync.md
[automatske obavijesti značajke]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin za iOS i Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin obrasci]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[Automatska promjena veličine]: ../app-service-web/web-sites-scale.md
[pripremna okruženja]: ../app-service-web/web-sites-staged-publishing.md
[Mogućnosti implementacije]: ../app-service-web/web-sites-deploy.md
[hibridno veze]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[virtualne mreže]: ../app-service-web/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service-web/app-service-app-service-environment-network-configuration-expressroute.md
[Aplikacije servisa za okruženja]: ../app-service-web/app-service-app-service-environment-intro.md
[Vodič za karte]: https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/
