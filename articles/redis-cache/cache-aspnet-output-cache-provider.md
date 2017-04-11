<properties
    pageTitle="Predmemoriju ASP.NET izlazne predmemorije davatelja"
    description="Saznajte kako predmemoriju ASP.NET stranice izlazne predmemorije Redis Azure pomoću"
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
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>Davatelj usluga ASP.NET izlazne predmemorije za Azure Redis predmemorije

Davatelj za Redis izlazne predmemorije je mehanizam za pohranu postupak za izlazne predmemorije podatke. Podaci posebno za cijeli odgovore HTTP (stranica izlaznog predmemoriranja). Davatelj priključuje u novi izlazne predmemorije davatelja proširenja točke koji je uvedena u ASP.NET 4.

Da biste koristili Redis izlazne predmemorije davatelja usluge, konfigurirati predmemoriju i konfigurirate aplikacije ASP.NET pomoću paketa Redis izlazne predmemorije NuGet za davatelja usluga. Ova tema sadrži upute o konfiguriranju aplikacije da biste koristiti Redis izlazne predmemorije davatelja usluga. Dodatne informacije o stvaranju i konfiguracija instancu Azure Redis predmemorije potražite u članku [Stvaranje predmemoriju](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>Spremanje ASP.NET stranice Izlaz u predmemoriji

Da biste konfigurirali klijentske aplikacije u Visual Studio pomoću paketa Redis NuGet davatelja izlazne predmemorije, desnom tipkom miša kliknite projekt u **Pregledniku rješenja** , a zatim odaberite **Upravljanje NuGet paketa**.

![Azure Redis predmemorije upravljanje NuGet paketa](./media/cache-aspnet-output-cache-provider/redis-cache-manage-nuget-menu.png)

U tekstni okvir za pretraživanje upišite **RedisOutputCacheProvider** , odaberite ga u rezultatima i kliknite **Instaliraj**.

![Azure Redis predmemorije izlazne predmemorije davatelja](./media/cache-aspnet-output-cache-provider/redis-cache-page-output-provider.png)

Paket Redis NuGet davatelj izlazne predmemorije je u opsegu StackExchange.Redis.StrongName paketa. Ako je paket StackExchange.Redis.StrongName ne postoji u projektu će se instalirati. Imajte na umu osim StackExchange.Redis.StrongName paket istaknuti pod nazivom ima li i verzija za koje nisu-istaknuti – pod nazivom StackExchange.Redis. Ako projekt koristi StackExchange.Redis verzije koje nisu-istaknuti – pod nazivom, morate deinstalirati prije ili nakon instalacije paketa Redis NuGet davatelja izlazne predmemorije, u suprotnom koji će se imenovanja sukoba u projektu. Dodatne informacije o te pakete potražite u članku [Konfiguriranje .NET predmemorije klijente](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Paket NuGet preuzimanja i dodaje reference potreban skup te sljedeću sekciju u web.config datoteku koja sadrži potrebne konfiguracija za ASP.NET aplikacija za korištenje usluga za Redis izlazne predmemorije.

    <caching>
      <outputCachedefault Provider="MyRedisOutputCache">
        <providers>
          <!--
          <add name="MyRedisOutputCache"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
          -->
          <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

Komentirane se odjeljku nalaze Primjeri atributi i postavke uzorka za svaki atribut.

Konfiguriranje atribute s vrijednostima iz vaše plohu predmemorije na portalu Microsoft Azure i konfigurirajte ostale vrijednosti po želji. Upute o pristupanju svojstva predmemorije, potražite u članku [Konfiguriranje Redis postavke predmemorije](cache-configure.md#configure-redis-cache-settings).

-   **glavno računalo** – navedite svoje predmemorije krajnjoj točki.
-   **priključak** – koristi priključak na koji nisu SSL priključak ili SSL, s ovisno o postavkama ssl.
-   **accessKey** – korištenje primarni ili sekundarni ključ za predmemoriju.
-   **ssl** – true ako želite sigurne predmemorije/klijent komunikacije s ssl; suprotnom je false. Pripazite da odredite točne priključak.
    -   Priključak koji nisu SSL onemogućeno je prema zadanim postavkama za nove predmemorije. Navedite true za tu postavku da biste koristili priključak SSL. Dodatne informacije o omogućivanju priključka koji nisu SSL potražite u odjeljku [Pristup priključke](cache-configure.md#access-ports) u temi [Konfiguriraj predmemoriju](cache-configure.md) .
-   **databaseId** – navedeni baze podataka koja želite koristiti za izlazne podatke u predmemoriji. Ako nije naveden, koristi se zadana vrijednost 0.
-   **applicationName** – tipke spremaju se u redis kao <AppName>_<SessionId>_Data. Time se omogućuje više aplikacija da biste zajednički koristili isti ključa. Taj parametar nije obavezan, a ako ga ne nudi koristi zadanu vrijednost.
-   **connectionTimeoutInMilliseconds** – ta postavka omogućuje da biste nadjačali postavku connectTimeout u klijentu StackExchange.Redis. Ako nije naveden, koristi se po zadanim postavkama connectTimeout 5000. Dodatne informacije potražite u članku [StackExchange.Redis konfiguracije modela](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** – ta postavka omogućuje da biste nadjačali postavku syncTimeout u klijentu StackExchange.Redis. Ako nije naveden, koristi se po zadanim postavkama syncTimeout 1000. Dodatne informacije potražite u članku [StackExchange.Redis konfiguracije modela](http://go.microsoft.com/fwlink/?LinkId=398705).

Dodajte programa OutputCache uputa na svaku stranicu za koju želite predmemoriju izlaz.

    <%@ OutputCache Duration="60" VaryByParam="*" %>

U ovom primjeru predmemorirane stranice podaci će ostati u predmemoriji za 60 sekundi, a drugu verziju sustava stranici je predmemorirani za svaku kombinaciju parametar. Dodatne informacije o OutputCache smjernica potražite u članku [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).

Kada se izvode ove korake, aplikacija je konfiguriran za korištenje Redis izlazne predmemorije davatelja usluga.

## <a name="next-steps"></a>Daljnji koraci

Pogledajte [ASP.NET davatelj usluga stanja sesije za Azure Redis predmemoriju](cache-aspnet-session-state-provider.md).
