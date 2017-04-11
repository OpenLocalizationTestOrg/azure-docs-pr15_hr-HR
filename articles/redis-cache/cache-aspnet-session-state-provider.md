<properties
    pageTitle="Stanje sesije ASP.NET predmemoriju davatelja | Microsoft Azure"
    description="Saznajte kako spremiti stanje ASP.NET sesije pomoću predmemorije Redis Azure"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/01/2016"
    ms.author="sdanie" />

# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>Stanje ASP.NET sesije davatelj usluga za Azure Redis predmemorije

Azure Redis predmemorije omogućuje davatelja stanje sesije koje možete koristiti za spremanje stanje sesije u predmemoriji umjesto u memoriji ili u bazi podataka sustava SQL Server. Da biste koristili predmemoriranja davatelja stanje sesije, najprije konfigurirati predmemoriju i konfiguriranje ASP.NET aplikacija za predmemoriju pomoću paketa Redis NuGet stanje predmemorije sesiju.

Nije često praktično u aplikaciji stvarnog života oblaku da biste izbjegli spremanje neki oblik stanja za sesiju korisnika, ali neke pristupa utjecati na performanse i skalabilnost više od ostalih. Ako imate za pohranu stanje, najbolje je rješenje da biste zadržali količinu stanje small i spremite ga na Kolačići. Ako koji nije izvedivo, sljedeće najbolje je rješenje da biste koristili stanje ASP.NET sesije davatelju usluge za raspodijeljeno, u memoriji predmemoriju. Najgori rješenje iz performanse i skalabilnost aspekta je korištenje baze podataka sigurnosno davatelja stanje sesije. Ova tema sadrži upute o korištenju davatelj stanje ASP.NET sesije za Azure Redis predmemoriju. Informacije o drugim mogućnostima stanje sesije potražite u odjeljku [Mogućnosti stanja ASP.NET sesije](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-the-cache"></a>Spremanje stanja ASP.NET sesije u predmemoriji

Da biste konfigurirali klijentske aplikacije u Visual Studio pomoću paketa Redis NuGet stanje predmemorije sesiju, desnom tipkom miša kliknite projekt u **Pregledniku rješenja** , a zatim odaberite **Upravljanje NuGet paketa**.

![Azure Redis predmemorije upravljanje NuGet paketa](./media/cache-aspnet-session-state-provider/redis-cache-manage-nuget-menu.png)

U tekstni okvir za pretraživanje upišite **RedisSessionStateProvider** , odaberite ga u rezultatima i kliknite **Instaliraj**.

>[AZURE.IMPORTANT] Ako koristite značajku klasteriranja od premium sloju, morate koristiti [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 ili noviji ili iznimka izbačena. Ovo je promjena otpornosti; Dodatne informacije potražite u članku [v2.0.0 prekidanje promjena pojedinosti](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

![Stanje sesije za Azure Redis predmemorije davatelja](./media/cache-aspnet-session-state-provider/redis-cache-session-state-provider.png)

Paket Redis NuGet davatelja sesiju stanje je u opsegu StackExchange.Redis.StrongName paketa. Ako je paket StackExchange.Redis.StrongName ne postoji u projektu će se instalirati. Imajte na umu osim StackExchange.Redis.StrongName paket istaknuti pod nazivom ima li i verzija za koje nisu-istaknuti – pod nazivom StackExchange.Redis. Ako projekt koristi StackExchange.Redis verzije koje nisu-istaknuti – pod nazivom, morate deinstalirati prije ili nakon instalacije paketa Redis NuGet davatelja sesiju stanje, u suprotnom koji će se imenovanja sukoba u projektu. Dodatne informacije o te pakete potražite u članku [Konfiguriranje .NET predmemorije klijente](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Paket NuGet preuzima i dodaje potrebne skupa reference i dodaje sljedeće dodaje sljedeću sekciju u web.config datoteku koja sadrži potrebne konfiguracija za ASP.NET aplikacija za korištenje Redis predmemorije sesiju stanje davatelja usluga.

    <sessionState mode="Custom" customProvider="MySessionStateStore">
        <providers>
        <!--
        <add name="MySessionStateStore"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            throwOnError = "true" [true|false]
            retryTimeoutInMilliseconds = "0" [number]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
        />
        -->
        <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
    </sessionState>

Komentirane se odjeljku nalaze Primjeri atributi i postavke uzorka za svaki atribut.

Konfiguriranje atribute s vrijednostima iz vaše plohu predmemorije na portalu Microsoft Azure i konfigurirajte ostale vrijednosti po želji. Upute o pristupanju svojstva predmemorije, potražite u članku [Konfiguriranje Redis postavke predmemorije](cache-configure.md#configure-redis-cache-settings).

-   **glavno računalo** – navedite svoje predmemorije krajnjoj točki.
-   **priključak** – koristi priključak na koji nisu SSL priključak ili SSL, s ovisno o postavkama ssl.
-   **accessKey** – korištenje primarni ili sekundarni ključ za predmemoriju.
-   **ssl** – true ako želite sigurne predmemorije/klijent komunikacije s ssl; suprotnom je false. Pripazite da odredite točne priključak.
    -   Priključak koji nisu SSL onemogućeno je prema zadanim postavkama za nove predmemorije. Navedite true za tu postavku da biste koristili priključak SSL. Dodatne informacije o omogućivanju priključka koji nisu SSL potražite u odjeljku [Pristup priključke](cache-configure.md#access-ports) u temi [Konfiguriraj predmemoriju](cache-configure.md) .
-   **throwOnError** – true ako želite da se iznimka izbačena u slučaju pogreške ili false ako želite da se operacije uvoza tihu. Potvrđivanjem statične svojstvo Microsoft.Web.Redis.RedisSessionStateProvider.LastException možete potražiti pogreške. Zadana je vrijednost true.
-   **retryTimeoutInMilliseconds** – operacije koje se neće se pokušati interval, naveden u milisekundama. Prvi pokušajte ponovno pojavljuje nakon 20 milisekundama, a zatim ponovne pokušaje pojaviti sekundi dok se ne retryTimeoutInMilliseconds intervala isteka. Odmah nakon intervala, postupak je ponoviti konačni jedanput. Ako se i dalje ne uspije, u Izbačena je ponovno pozivatelju, ovisno o postavci throwOnError. Zadana vrijednost je 0, što znači da nema ponovne pokušaje.
-   **databaseId** – određuje predmemoriju baze podataka koja želite koristiti za izlazne podatke. Ako nije naveden, koristi se zadana vrijednost 0.
-   **applicationName** – tipke spremaju se u redis kao `{<Application Name>_<Session ID>}_Data`. Time se omogućuje više aplikacija da biste zajednički koristili isti ključa. Taj parametar nije obavezan, a ako ga ne nudi koristi zadanu vrijednost.
-   **connectionTimeoutInMilliseconds** – ta postavka omogućuje da biste nadjačali postavku connectTimeout u klijentu StackExchange.Redis. Ako nije naveden, koristi se po zadanim postavkama connectTimeout 5000. Dodatne informacije potražite u članku [StackExchange.Redis konfiguracije modela](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** – ta postavka omogućuje da biste nadjačali postavku syncTimeout u klijentu StackExchange.Redis. Ako nije naveden, koristi se po zadanim postavkama syncTimeout 1000. Dodatne informacije potražite u članku [StackExchange.Redis konfiguracije modela](http://go.microsoft.com/fwlink/?LinkId=398705).

Dodatne informacije o tim svojstvima potražite u članku izvorne objava objavu bloga [Objavljivanje ASP.NET sesije stanje](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)davatelja za Redis.

Nemojte zaboraviti komentare pogledajte i standardne InProc sesiju stanje davatelja sekciju u vašem web.config.

    <!-- <sessionState mode="InProc"
         customProvider="DefaultSessionProvider">
         <providers>
            <add name="DefaultSessionProvider"
                  type="System.Web.Providers.DefaultSessionStateProvider,
                        System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                        PublicKeyToken=31bf3856ad364e35"
                  connectionStringName="DefaultConnection" />
          </providers>
    </sessionState> -->

Kada se izvode ove korake, aplikacija je konfiguriran za korištenje Redis predmemorije sesiju stanje davatelja usluga. Kada koristite stanje sesije u aplikaciji, će se spremati instance predmemorije Redis Azure.

>[AZURE.NOTE] Imajte na umu da podatke pohranjene u predmemoriji mora biti serializable, za razliku od podataka koje se mogu pohraniti u zadanom sesiju u memoriji ASP.NET stanja davatelja usluga. Kada se koristi sesiju stanje davatelj usluga za Redis, provjerite jesu li vrste podataka koje se pohranjuju u stanje sesije serializable.

## <a name="aspnet-session-state-options"></a>Stanje ASP.NET sesije mogućnosti

- U stanje sesije memorije davatelja – ovaj davatelj pohranjuje stanje sesije u memoriji. Prednost korištenja ovog davatelja je jednostavno i brzo. No ne možete skaliranje Web aplikacije ako koristite davatelja memorije jer ne distribuirati.

- Stanje sustava SQL Server sesije davatelja – ovaj davatelj pohranjuje stanje sesije u sustavu Sql Server. Koristite ovaj davatelj ako želite zadržava stanje sesije u stalnih prostor za pohranu. Mogu mijenjati veličinu Web App, no pomoću Sql Server za sesiju će imati utjecaj na performanse na web-aplikaciju programa.

- Raspodijeljeno memorije sesije stanje davatelj usluge uključivanja u kao što su Redis predmemorije sesiju stanje davatelja - ovog davatelja daje najbolje oba svijetu. Web-aplikaciju programa može imati jednostavni, brza i skalabilni davatelja stanje sesije. No morate zadržati u obzir ovog davatelja pohranjuje stanje sesije u predmemoriju te aplikacije sadrži snimanja u obzir sve značajke povezan kad razgovor web Distributed u predmemoriji kao što su tranzitne mrežnih pogrešaka. Praktični savjeti o korištenju predmemorije potražite u članku [smjernice Međuspremanje](../best-practices-caching.md) iz Microsoft Patterns & prakse [Azure oblaka aplikacije dizajna i smjernice implementaciju](https://github.com/mspnp/azure-guidance).

Dodatne informacije o stanju sesije i druge najbolje prakse potražite u članku [Web razvoj najbolje prakse (sastavni stvarnog života oblaka web -aplikacije s Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Daljnji koraci

Pogledajte [ASP.NET izlazne predmemorije davatelja za Azure Redis predmemoriju](cache-aspnet-output-cache-provider.md).
