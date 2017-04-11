<properties 
    pageTitle="Stanje sesije s Azure Redis predmemorije u aplikacije servisa za Azure" 
    description="Saznajte kako pomoću servisa Azure predmemorije podržava ASP.NET sesije stanje predmemoriranje." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="none"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="06/27/2016" 
    ms.author="riande"/>


# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Stanje sesije s Azure Redis predmemorije u aplikacije servisa za Azure


U ovoj se temi objašnjava kako pomoću servisa Azure Redis predmemoriju za stanje sesije.

Ako web-aplikaciju programa ASP.NET koristi stanje sesije, morat ćete konfigurirati vanjskih sesiju stanje davatelj internetskih usluga (servisom predmemorije Redis ili davatelj usluge SQL Server sesiju stanje). Ako koristite stanje sesije i ne koristite vanjskog davatelja usluga, bit će ograničeni na jednu instancu web-aplikacije. Predmemorija servis Redis je najbrži i najjednostavniji da biste omogućili.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>Stvaranje predmemoriju
Slijedite [ove upute](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) da biste stvorili predmemoriju.

##<a id="configureproject"></a>Dodavanje paketa RedisSessionStateProvider NuGet na web-aplikaciju
Instalacija na NuGet `RedisSessionStateProvider` paketa.  Koristite sljedeću naredbu da biste instalirali iz konzola upravitelja paketa (**Alati** > **Upravitelj paketa NuGet** > **Upravitelj paketa konzola**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
Da biste instalirali iz **alata** > **Upravitelj paketa NuGet** > **Upravljanje NugGet paketa rješenja**, okvir za pretraživanje `RedisSessionStateProvider`.

Dodatne informacije potražite u članku [NuGet RedisSessionStateProvider stranica](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) i [Konfiguriranje predmemorije klijenta](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

##<a id="configurewebconfig"></a>Izmjena Web.Config datoteke
Osim učiniti skupa reference za predmemoriju, paket NuGet dodaje graničnik unose u datoteci *web.config* . 

1. Otvorite *web.config* i pronađite u **sessionState** element.

1. Unesite vrijednosti za `host`, `accessKey`, `port` (priključak SSL mora biti 6380), a postavite `SSL` da biste `true`. Te vrijednosti možete dobivenog plohu [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) za vaše instancu predmemoriju. Dodatne informacije potražite u članku [Povezivanje u predmemoriju](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Imajte na umu priključka koji nisu SSL onemogućen zadano za nove predmemorije. Dodatne informacije o omogućivanju priključka koji nisu SSL potražite u odjeljku [Pristup priključke](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) u temi [Konfiguriranje predmemorije u predmemoriji Redis Azure](https://msdn.microsoft.com/library/azure/dn793612.aspx) . Sljedeće oznake prikazuje promjene u datoteci *web.config* , posebice promjene *priključak*, *glavno računalo*, a zatim accessKey*, a *ssl *.

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a>Korištenje Session objekt u kodu
U posljednjem koraku je da biste počeli koristiti Session objekt u ASP.NET kod. Dodavanje objekata stanje sesije pomoću metode amortizacije **Session.Add** . Ova metoda koristi parovima ključnih vrijednosti da biste pohranili stavke u predmemoriji stanje sesije.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Sljedeći kod dohvaća vrijednost iz stanja sesije.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

Predmemoriju Redis predmemorije objekte možete koristiti i u web-aplikaciji. Dodatne informacije potražite u članku [MVC film aplikaciju pomoću Azure Redis predmemorije 15 minuta](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
Dodatne informacije o načinu korištenja stanje ASP.NET sesije potražite u članku [Pregled stanja za ASP.NET sesije][].

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="whats-changed"></a>Što se promijenilo
* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)

  *Po [Obogaćenog Anderson](https://twitter.com/RickAndMSFT)*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [Pregled stanja ASP.NET sesije]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 
