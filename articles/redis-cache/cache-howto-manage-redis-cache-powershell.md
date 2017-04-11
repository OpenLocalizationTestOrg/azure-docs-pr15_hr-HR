<properties
    pageTitle="Upravljanje Azure Redis predmemorije s Azure PowerShell | Microsoft Azure"
    description="Saznajte kako obavljanje zadataka administracije predmemoriju Azure Redis pomoću komponente PowerShell Azure."
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
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Upravljanje Azure Redis predmemorije s Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure EŽA](cache-manage-cli.md)

U ovoj se temi prikazano kako da biste izvršili uobičajene zadatke kao što su stvaranje, ažuriranje, i skaliranje vaše instance Azure Redis predmemorije, kako Obnovi pristupnih tipki i upute za prikaz podataka o vašem predmemorije. Popis svih Azure Redis predmemorije cmdleta ljuske PowerShell potražite u članku [cmdleti za Azure Redis predmemoriju](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)][Klasični modela](../resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Preduvjeti

Ako ste već instalirali Azure PowerShell, morate imati Azure PowerShell verzije 1.0.0 ili noviji. Možete provjeriti verziju Azure PowerShell koji ste instalirali tu naredbu u naredbenom retku Azure PowerShell.

    Get-Module azure | format-table version

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

Najprije morate se prijaviti Azure pomoću ove naredbe.

    Login-AzureRmAccount

Navedite adresu e-pošte te račun za Azure lozinku u dijalogu za prijavu u Microsoft Azure.

Nakon toga ako imate više pretplata Azure, morate postaviti Azure pretplatu. Da biste vidjeli popis svoje trenutne pretplate, pokrenite sljedeću naredbu.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Da biste odredili pretplatu, pokrenite sljedeću naredbu. U sljedećem primjeru je naziv pretplate `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Da biste koristili Windows PowerShell s Azure Voditelj resursa, potrebne su vam sljedeće:

- Komponente Windows PowerShell verzije 3.0 ili 4.0. Da biste pronašli verziju sustava Windows PowerShell, upišite:`$PSVersionTable` i provjerite je li vrijednost `PSVersion` 3.0 ili 4.0. Da biste instalirali kompatibilna verzija, potražite u članku [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) ili [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Da biste dobili detaljnu pomoć za sve cmdlet potražite u ovom ćete praktičnom vodiču, koristite cmdlet Get-pomoć.

    Get-Help <cmdlet-name> -Detailed

Na primjer, da biste dobili pomoć za na `New-AzureRmRedisCache` cmdlet, a zatim vrstu:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-azure-government-cloud-or-azure-china-cloud"></a>Kako se povezati sa Azure Government Cloud ili Azure Kina oblaka

Po zadanom se Azure okruženje je `AzureCloud`, koji predstavlja instancu globalni Azure oblaka. Da biste povezali neku drugu instancu, koristite na `Add-AzureRmAccount` naredbe s na `-Environment` ili -`EnvironmentName` Skretnice naredbenog retka s željeni okruženje ili naziv okruženju.

Da biste vidjeli popis dostupnih okruženja, pokrenite na `Get-AzureRmEnvironment` cmdlet.

### <a name="to-connect-to-the-azure-government-cloud"></a>Da biste se povezali s oblakom državne Azure

Da biste se povezali s oblakom državne Azure, koristite neku od sljedećih naredbi.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

ili

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Da biste stvorili predmemoriju u Oblaku državne Azure, koristite neku od sljedećih mjesta.

-   USGov Virginia
-   USGov Iowa

Dodatne informacije o Azure Government Cloud potražite u članku [Microsoft Azure državne](https://azure.microsoft.com/features/gov/) i [Vodič za razvojne inženjere državne Microsoft Azure](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Da biste se povezali s oblakom Kina Azure

Da biste se povezali s oblakom Kina Azure, koristite neku od sljedećih naredbi.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

ili

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Da biste stvorili predmemoriju u Kini oblaka Azure, koristite neku od sljedećih mjesta.

-   Kina Istok
-   Sjeverna Kina

Dodatne informacije o Kina oblaka Azure potražite u članku [AzureChinaCloud za Azure kojim upravlja 21vianet u Kini](http://www.windowsazure.cn/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Svojstva koja se koriste za Azure Redis PowerShell predmemorije

Sljedeća tablica sadrži svojstva i opise najčešće korištenih parametre pri stvaranju pravila i upravljanju vaše instance predmemorije Redis Azure pomoću komponente PowerShell Azure.

| Parametar          | Opis                                                                                                                                                                                                        | Zadani  |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Ime               | Naziv predmemorije                                                                                                                                                                                                  |          |
| Mjesto           | Mjesto predmemorije                                                                                                                                                                                              |          |
| ResourceGroupName  | Naziv grupe resursa u kojoj želite stvoriti predmemoriju                                                                                                                                                                   |          |
| Veličina               | Veličina predmemorije. Valjane su vrijednosti: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB                                                                     | 1GB      |
| ShardCount         | Broj shards da biste stvorili prilikom stvaranja predmemoriju premium s Klasteriranje omogućena. Valjane vrijednosti su: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10                                                                                                      |          |
| SKU                | Određuje SKU predmemoriju. Valjane su vrijednosti: osnovni standardne, Premium                                                                                                                                         | Standardna |
| RedisConfiguration | Određuje Redis konfiguracijske postavke. Pojedinosti svake postavke potražite u tablici u nastavku [RedisConfiguration svojstva](#redisconfiguration-properties) . |          |
| EnableNonSslPort   | Označava je li omogućen priključka koji nisu SSL.                                                                                                                                                                     | FALSE    |
| MaxMemoryPolicy    | Izostavljen je taj parametar – umjesto toga koristite RedisConfiguration.                                                                                                                                              |          |
| StaticIP           | Kada se hostira predmemorije u na VNET određuje jedinstvenu IP adresu u podmreže predmemoriju. Ako nije naveden, jedna je odabrati umjesto vas podmreži.                                                                                                                     |          |
| Podmreže             | Kada u kojem se nalazi predmemorije u na VNET, određuje naziv podmreže u koju želite uvesti u predmemoriju.                                                                                                                  |          |
| VirtualNetwork     | Kada se nalaze predmemorije u na VNET određuje ID resursa VNET u koju želite uvesti u predmemoriju.                                                                                                             |          |
| KeyType            | Određuje koji tipkovni prečac da biste Obnovi kada obnovom pristupnih tipki. Valjane su vrijednosti: primarnog, sekundarnog |  |                                                                                                                                                                                                              |          |


### <a name="redisconfiguration-properties"></a>Svojstva RedisConfiguration

| Svojstvo                      | Opis                                                                                                          | Cijene razine       |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------|
| RDB sigurnosno kopiranje-omogućeni            | Hoće li biti omogućen [Redis postojanost podataka](cache-how-to-premium-persistence.md)                                     | Samo Premium        |
| RDB-prostora za pohranu--niz za povezivanje | Niz za povezivanje s računom za pohranu za [Redis postojanost podataka](cache-how-to-premium-persistence.md)       | Samo Premium        |
| RDB sigurnosne kopije učestalost          | Sigurnosno kopiranje učestalost za [Redis postojanost podataka](cache-how-to-premium-persistence.md)                               | Samo Premium        |
| maxmemory rezervirana            | Konfigurira [memorije rezervirane](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) -cache procesima | Standardni i Premium |
| maxmemory pravila              | Konfigurira [eviction pravila](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) za predmemoriju           | Sve razine cijena   |
| obavijestite-keyspace-događaja        | Konfigurira [keyspace obavijesti](cache-configure.md#keyspace-notifications-advanced-settings)                     | Standardni i Premium |
| Raspršivanje max-ziplist-stavke      | Konfigurira [optimizacija memorije](http://redis.io/topics/memory-optimization) za vrste small prikupljanje podataka          | Standardni i Premium |
| Raspršivanje max-ziplist-vrijednosti        | Konfigurira [optimizacija memorije](http://redis.io/topics/memory-optimization) za vrste small prikupljanje podataka          | Standardni i Premium |
| Postavljanje max-intset-stavke        | Konfigurira [optimizacija memorije](http://redis.io/topics/memory-optimization) za vrste small prikupljanje podataka          | Standardni i Premium |
| zset max-ziplist-stavke      | Konfigurira [optimizacija memorije](http://redis.io/topics/memory-optimization) za vrste small prikupljanje podataka          | Standardni i Premium |
| zset max-ziplist-vrijednosti        | Konfigurira [optimizacija memorije](http://redis.io/topics/memory-optimization) za vrste small prikupljanje podataka          | Standardni i Premium |
| Baza podataka                     | Konfigurira broj baza podataka. Ovo svojstvo se može konfigurirati samo kod stvaranja predmemoriju.                          | Standardni i Premium |

## <a name="to-create-a-redis-cache"></a>Da biste stvorili predmemoriju Redis

Nove instance predmemorije Redis Azure stvaraju se pomoću cmdleta [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

>[AZURE.IMPORTANT] Prvi put stvorite Redis predmemorije u pretplatu pomoću portala za Azure portalu registrira u `Microsoft.Cache` prostor naziva za tu pretplatu. Ako pokušate stvoriti prvi Redis predmemorije u pretplatu pomoću komponente PowerShell, prvo morate registrirati taj prostor naziva pomoću sljedeće naredbe; u suprotnom cmdleta kao što su `New-AzureRmRedisCache` i `Get-AzureRmRedisCache` neće uspjeti.
>
>`Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`

Da biste vidjeli popis dostupnih parametara i njihovi opisi za `New-AzureRmRedisCache`, pokrenite sljedeću naredbu.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed
    
    NAME
        New-AzureRmRedisCache
    
    SYNOPSIS
        Creates a new redis cache.
    
    
    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]
    
    
    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.
    
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to create.
    
        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.
    
        -Location <String>
            Location in which to create the redis cache.
    
        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}
    
        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Da biste stvorili predmemoriju zadani parametri, pokrenite sljedeću naredbu.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, a `Location` su potrebne parametre, ali ostale neobavezni su i imaju zadane vrijednosti. Izvodi prethodne naredbe stvara standardne SKU Azure Redis predmemorije instance s navedeni naziv, mjesto i grupa resursa koja je 1 GB u veličinu priključka koji nisu SSL onemogućene.

Da biste stvorili premium predmemorije, odredite veličinu P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), ili P4 (53 GB - 530 GB). Da biste omogućili Klasteriranje, navedite shard broj uz pomoć u `ShardCount` parametar. Sljedeći primjer stvara predmemoriju P1 premium s 3 shards. Predmemoriju premium P1 6 GB veličina, a jer smo navedena tri shards ukupnu veličinu 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Da biste odredili vrijednosti u `RedisConfiguration` parametar, prije i nakon vrijednosti unutar `{}` kao ključa vrijednosti parove kao što su `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Sljedeći primjer stvara standardne predmemorije 1 GB s `allkeys-random` maxmemory pravila keyspace obavijesti o i konfiguriran pomoću `KEA`. Dodatne informacije potražite u članku [Keyspace obavijesti (Napredne postavke)](cache-configure.md#keyspace-notifications-advanced-settings) i [Maxmemory pravila i maxmemory rezervirane](cache-configure.md#maxmemory-policy-and-maxmemory-reserved).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>
## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Da biste konfigurirali postavku tijekom stvaranja predmemorije baze podataka

Na `databases` postavke koje se mogu konfigurirati samo tijekom stvaranja predmemoriju. Sljedeći primjer stvara premium P3 predmemorije (26 GB) s 48 bazama podataka pomoću cmdleta [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Dodatne informacije o na `databases` svojstvo, [zadane Azure Redis predmemorije konfiguraciju poslužitelja](cache-configure.md#default-redis-server-configuration)u odjeljku. Dodatne informacije o stvaranju predmemoriju pomoću cmdleta [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) potražite u članku u prethodnom odjeljku [Da biste stvorili predmemoriju Redis](#to-create-a-redis-cache) .

## <a name="to-update-a-redis-cache"></a>Da biste ažurirali Redis predmemorije

Azure Redis predmemorije instanci su ažurirani pomoću cmdleta [Skup AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) .

Da biste vidjeli popis dostupnih parametara i njihovi opisi za `Set-AzureRmRedisCache`, pokrenite sljedeću naredbu.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed
    
    NAME
        Set-AzureRmRedisCache
    
    SYNOPSIS
        Set redis cache updatable parameters.
    
    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]
    
    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to update.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Na `Set-AzureRmRedisCache` cmdlet može se koristiti za ažuriranje svojstava kao što su `Size`, `Sku`, `EnableNonSslPort`, i `RedisConfiguration` vrijednosti. 

Sljedeća naredba ažurira pravilniku maxmemory predmemoriju Redis pod nazivom myCache.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>
## <a name="to-scale-a-redis-cache"></a>Da biste skalirali Redis predmemorije

`Set-AzureRmRedisCache`možete koristiti za promjenu veličine predmemorije za Azure Redis instancu kada u `Size`, `Sku`, ili `ShardCount` izmjene svojstava. 

>[AZURE.NOTE]Promjena veličine predmemorije pomoću komponente PowerShell podložni o iste ograničenja i smjernice kao skaliranje predmemorije s portala za Azure. Mogu se mijenjati veličinu za različite cijene sloj s sljedeća ograničenja.
>
>-  Ne promijeni iz veći cijene sloju na donjem cijene sloju.
>    -    Nije moguće skaliranje od **Premium** predmemorije prema dolje do u **standardni** ili **Osnovni** predmemorije.
>    -    Nije moguće skaliranje iz **standardnog** predmemorije prema dolje do **Osnovni** predmemoriju.
>-  Možete skaliranje **Osnovni** predmemorije **standardne** predmemoriju, ali ne možete promijeniti veličinu u isto vrijeme. Ako vam je potrebna različitih veličina, to možete učiniti sljedeće skaliranja operacije do željene veličine.
>-  Nije moguće skaliranje iz **osnovne** predmemorije izravno u predmemoriju **Premium** . Mora skaliranje od **Osnovno** za **standardnu** skaliranja odjednom, a zatim iz **standardnog** na **Premium** u operaciji kasnije skaliranja.
>-  Nije moguće Skaliranje s većom prema dolje da biste na **C0 (250 MB)** veličina.
>
>Dodatne informacije potražite u članku [Skaliranje Azure Redis predmemoriju](cache-how-to-scale.md).

Sljedeći primjer prikazuje način za promjenu veličine predmemorije pod nazivom `myCache` 2.5 GB predmemoriju. Imajte na umu da ova naredba radi kao osnovni ili standardni predmemoriju.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Kada se izda tu naredbu, vraća se status predmemorije (slično zovete `Get-AzureRmRedisCache`). Imajte na umu da se `ProvisioningState` je `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB
    
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Nakon dovršetka postupka skaliranja, u `ProvisioningState` mijenja `Succeeded`. Ako trebate napraviti sljedeće skaliranja operacije, kao što su promjena iz osnovne Standard i promjena veličine, morate pričekati prethodne operacije dovršetka ili primite poruku o pogrešci slična sljedećoj.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Da biste saznali o Redis predmemorije

Može dohvatiti podatke o predmemoriju pomoću cmdleta [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) .

Da biste vidjeli popis dostupnih parametara i njihovi opisi za `Get-AzureRmRedisCache`, pokrenite sljedeću naredbu.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed
    
    NAME
        Get-AzureRmRedisCache
    
    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.
    
    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.
    
        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.
    
        If no parameters are given than it will return details about all caches the current subscription.
    
    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Da biste vratili informacije o svim predmemorije u trenutne pretplate, pokrenite `Get-AzureRmRedisCache` bez parametre.

    Get-AzureRmRedisCache

Da biste vratili informacije o svim predmemorije u određene grupe resursa, pokrenite `Get-AzureRmRedisCache` s na `ResourceGroupName` parametar.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Da biste vratili informacije o određenim predmemorije, pokrenite `Get-AzureRmRedisCache` s na `Name` parametar koji sadrži naziv predmemorije, i `ResourceGroupName` parametar s grupa resursa koje sadrže taj predmemoriju.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Da biste dohvatili tipkovni prečaci za Redis predmemorije

Da biste dohvatili tipkovni prečaci za predmemorije, koristite cmdlet [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) .

Da biste vidjeli popis dostupnih parametara i njihovi opisi za `Get-AzureRmRedisCacheKey`, pokrenite sljedeću naredbu.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed
    
    NAME
        Get-AzureRmRedisCacheKey
    
    SYNOPSIS
        Gets the accesskeys for the specified redis cache.
    
    
    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Da biste dohvatili tipke za predmemorije, poziva na `Get-AzureRmRedisCacheKey` cmdlet i prenesite naziv predmemorije naziv grupe resursa koja sadrži predmemoriju.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Da biste Obnovi tipkovni prečaci za Redis predmemoriju

Da biste Obnovi tipkovni prečaci za predmemoriju pomoću cmdleta [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) .

Da biste vidjeli popis dostupnih parametara i njihovi opisi za `New-AzureRmRedisCacheKey`, pokrenite sljedeću naredbu.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed
    
    NAME
        New-AzureRmRedisCacheKey
    
    SYNOPSIS
        Regenerates the access key of a redis cache.
    
    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.
    
        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    
Da biste Obnovi primarni ili sekundarnu tipku predmemorije, nazovite na `New-AzureRmRedisCacheKey` cmdlet proći u nazivu, grupa resursa i navesti `Primary` ili `Secondary` za na `KeyType` parametar. U sljedećem primjeru regenerated je sekundarne tipkovni prečac za predmemoriju.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary
    
    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Da biste izbrisali Redis predmemorije

Da biste izbrisali Redis predmemorije, koristite cmdlet [Ukloni AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) .

Da biste vidjeli popis dostupnih parametara i njihovi opisi za `Remove-AzureRmRedisCache`, pokrenite sljedeću naredbu.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed
    
    NAME
        Remove-AzureRmRedisCache
    
    SYNOPSIS
        Remove redis cache if exists.
    
    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>
    
    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.
    
        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.
    
        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.
    
        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

U sljedećem primjeru pod nazivom predmemoriju `myCache` nestaje.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Da biste uvezli Redis predmemorije

Podatke možete uvesti pomoću programa Azure Redis predmemorije instancu na `Import-AzureRmRedisCache` cmdlet.

>[AZURE.IMPORTANT] Uvoz/izvoz dostupna je samo za predmemorije [premium sloju](cache-premium-tier-intro.md) . Dodatne informacije o uvoz/izvoz potražite u članku [Uvoz i izvoz podataka u predmemoriji Redis Azure](cache-how-to-import-export-data.md).

Da biste vidjeli popis dostupnih parametara i njihovi opisi za `Import-AzureRmRedisCache`, pokrenite sljedeću naredbu.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed
    
    NAME
        Import-AzureRmRedisCache
    
    SYNOPSIS
        Import data from blobs to Azure Redis Cache.
    
    
    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.
    
        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Sljedeća naredba Uvozi podatke iz blob određen SAS uri u predmemoriji Redis Azure.


    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Da biste izvezli Redis predmemorije

Podatke možete izvesti pomoću programa Azure Redis predmemorije instancu na `Export-AzureRmRedisCache` cmdlet.

>[AZURE.IMPORTANT] Uvoz/izvoz dostupna je samo za predmemorije [premium sloju](cache-premium-tier-intro.md) . Dodatne informacije o uvoz/izvoz potražite u članku [Uvoz i izvoz podataka u predmemoriji Redis Azure](cache-how-to-import-export-data.md).

Da biste vidjeli popis dostupnih parametara i njihovi opisi za `Export-AzureRmRedisCache`, pokrenite sljedeću naredbu.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed
    
    NAME
        Export-AzureRmRedisCache
    
    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.
    
    
    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Prefix <String>
            Prefix to use for blob names.
    
        -Container <String>
            SAS url of container where data should be exported.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Sljedeća naredba izvoz podataka iz predmemorije Redis Azure instance u spremniku određen SAS uri.


        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Da biste ponovno Redis predmemorije

Ponovno korištenje predmemorije Redis Azure instancu na `Reset-AzureRmRedisCache` cmdlet.

>[AZURE.IMPORTANT] Ponovno pokrenite računalo dostupna je samo za predmemorije [premium sloju](cache-premium-tier-intro.md) . Dodatne informacije o ponovnog pokretanja predmemorije, potražite u članku [administriranje predmemorije - ponovno](cache-administration.md#reboot).

Da biste vidjeli popis dostupnih parametara i njihovi opisi za `Reset-AzureRmRedisCache`, pokrenite sljedeću naredbu.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed
    
    NAME
        Reset-AzureRmRedisCache
    
    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.
    
    
    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".
    
        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.
    
        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.
    
        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Sljedeća naredba izvršen oba čvorove navedeni predmemoriju.

    
        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force
    

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o korištenju programa Windows PowerShell s Azure potražite u sljedećim resursima:

- [Azure dokumentaciju cmdlet Redis predmemorije na MSDN-u](https://msdn.microsoft.com/library/azure/mt634513.aspx)
- [Cmdleti za upravljanje resursima Azure](http://go.microsoft.com/fwlink/?LinkID=394765): Saznajte kako koristiti s cmdletima u modulu AzureResourceManager.
- [Grupa pomoću resursa za upravljanje Azure resurse](../resource-group-template-deploy-portal.md): Saznajte kako stvoriti i upravljanje grupama resursa na portalu za Azure.
- [Azure blog](http://blogs.msdn.com/windowsazure): dodatne informacije o novim značajkama programa Azure.
- [Blog komponente Windows PowerShell](http://blogs.msdn.com/powershell): dodatne informacije o novim značajkama programa Windows PowerShell.
- ["Hej, skriptiranje Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): dohvaćanje stvarnog života savjete i trikove iz zajednice komponente Windows PowerShell.
