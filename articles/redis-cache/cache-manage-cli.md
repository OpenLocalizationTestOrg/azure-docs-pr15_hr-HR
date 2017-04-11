<properties 
    pageTitle="Kako stvoriti i upravljanje Redis Azure predmemoriju pomoću sučelja Azure naredbenog retka (Azure EŽA) | Microsoft Azure" 
    description="Saznajte kako instalirati EŽA Azure na bilo kojoj platformi, kako je koristiti da biste se povezali s računom za Azure i kako stvoriti i upravljanje Redis predmemorije iz EŽA Azure." 
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

# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>Kako stvoriti i upravljanje Redis Azure predmemoriju pomoću sučelja Azure naredbenog retka (Azure EŽA)

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure EŽA](cache-manage-cli.md)

Azure EŽA sjajan je način da biste upravljali infrastruktura za Azure s bilo kojeg platforme. U ovom se članku objašnjava stvaranje i upravljanje vaše instance predmemorije Redis Azure pomoću EŽA Azure.

## <a name="prerequisites"></a>Preduvjeti

Da biste stvorili i upravljanje instance predmemorije Redis Azure pomoću EŽA Azure, morate poduzeti sljedeće korake.

-   Morate imati račun za Azure. Ako ga nemate, možete stvoriti [pomoću računa](https://azure.microsoft.com/pricing/free-trial/) u samo nekoliko sekundi dok.
-   [Instalacija Azure EŽA](../xplat-cli-install.md).
-   Povezivanje instalacije EŽA Azure s osobnog računa za Azure ili tvrtke ili obrazovne ustanove račun za Azure i prijavite se u pomoću Azure EŽA na `azure login` naredbe. Da biste bolje razumjeli razlike i odaberite, potražite u članku [Povezivanje Azure pretplatu s Azure sučelja naredbenog retka (Azure EŽA)](../xplat-cli-connect.md).
-   Prije pokretanja bilo koji od sljedećih naredbi, prebacite EŽA Azure upravljanja resursima u način rada tako da pokrenete u `azure config mode arm` naredbe. Dodatne informacije potražite u članku [Postavljanje načina upravljanja resursima Azure](../xplat-cli-azure-resource-manager.md#set-the-azure-resource-manager-mode).

## <a name="redis-cache-properties"></a>Redis predmemorije svojstva

Sljedeća svojstva se koriste prilikom stvaranja i ažuriranje instance Redis predmemorije.

| Svojstvo            | Promjena                                  | Opis                                                                                                                                                                                                                                          |
|---------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ime                | -n, – naziv                              | Naziv predmemorije Redis.                                                                                                                                                                                                                             |
| grupa resursa      | -g, – grupa resursa                    | Naziv grupe resursa.                                                                                                                                                                                                                          |
| mjesto            | -l, – mjesto                          | Mjesto da biste stvorili predmemoriju.                                                                                                                                                                                                                            |
| Veličina                | + z, – veličina                              | Veličina predmemorije Redis. Valjane vrijednosti: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]                                                                                                                                                                  |
| SKU                 | -x, – sku                               | Redis SKU-om. Mora biti jedan od: [osnovni standardne, Premium]                                                                                                                                                                                             |
| EnableNonSslPort    | -e, – Omogući-koje nisu-ssl-priključak               | Svojstvo EnableNonSslPort predmemoriju Redis. Dodavanje se oznaka ako želite omogućiti SSL priključak osobe koje nisu za predmemoriju                                                                                                                                    |
| Redis konfiguracija | -c, – redis konfiguracija               | Redis konfiguracije. Unesite niz JSON oblikovani konfiguracije ključeva i vrijednosti u nastavku. Oblik: "{" ":""," ":" "}"                                                                                                                                     |
| Redis konfiguracija | -f, – redis konfiguracijska datoteka          | Redis konfiguracije. Unesite put datoteke koja sadrži tipke za konfiguraciju i vrijednosti u nastavku. Oblik za unos datoteke: {"": "","": ""}                                                                                                                |
| Broj shard         | -r, – shard count                       | Broj Shards da biste stvorili na predmemoriju klaster Premium s Klasteriranje.                                                                                                                                                                               |
| Virtualne mreže     | -v, – virtualne mreže                   | Kada se nalaze predmemorije u na VNET određuje točno ARM resursa ID virtualne mreže za implementaciju redis predmemorije u. Primjer oblika: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Vrsta ključa            | -t, – ključ vrsta                          | Vrsta ključ za obnovu pretplate. Valjane vrijednosti: [primarnog, sekundarnog]                                                                                                                                                                                             |
| StaticIP            | -p, – statičnu ip < statičnu ip >             | Kada se nalaze predmemorije u na VNET određuje jedinstvenu IP adresu u podmreže predmemoriju. Ako nije naveden, jedna je odabrati umjesto vas podmreži.                                                                                                |
| Podmreže              | t, – podmreže<subnet>                    | Kada u kojem se nalazi predmemorije u na VNET, određuje naziv podmreže u koju želite uvesti u predmemoriju.                                                                                                                                                    |
| VirtualNetwork      | -v, – virtualne mreže < virtualne-mreže > | Kada se nalaze predmemorije u na VNET određuje točno ARM resursa ID virtualne mreže za implementaciju redis predmemorije u. Primjer oblika: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Pretplate        | -s, – pretplate                      | Identifikator pretplate.                                                                                                                                                                                                                         |

## <a name="see-all-redis-cache-commands"></a>Pogledajte sve naredbe Redis predmemorije

Da biste vidjeli sve naredbe Redis predmemorije i njihovim parametrima, koristite na `azure rediscache -h` naredbe.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Stvaranje Redis predmemorije

Da biste stvorili predmemoriju Redis, koristite sljedeću naredbu:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Dodatne informacije o ta naredba Pokreni u `azure rediscache create -h` naredbe.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Brisanje postojećeg predmemorija Redis

Da biste izbrisali predmemoriju Redis, koristite sljedeću naredbu:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Dodatne informacije o ta naredba Pokreni u `azure rediscache delete -h` naredbe.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Popis svih Redis predmemorije unutar pretplatu ili grupa resursa

Da biste dobili popis svih Redis predmemorije unutar pretplatu ili grupu resursa, koristite sljedeću naredbu:

    azure rediscache list [options]

Dodatne informacije o ta naredba Pokreni u `azure rediscache list -h` naredbe.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Prikaz svojstava postojećeg predmemorija Redis

Da biste prikazali svojstva postojećeg predmemorija Redis, koristite sljedeću naredbu:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Dodatne informacije o ta naredba Pokreni u `azure rediscache show -h` naredbe.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>
## <a name="change-settings-of-an-existing-redis-cache"></a>Promjena postavki postojećeg predmemorija Redis

Da biste promijenili postavke postojećeg predmemorija Redis, koristite sljedeću naredbu:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Dodatne informacije o ta naredba Pokreni u `azure rediscache set -h` naredbe.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>Obnavljanja ključa provjere autentičnosti za postojećeg predmemorija Redis

Prilikom obnavljanja ključa provjere autentičnosti za postojećeg predmemorija Redis, koristite sljedeću naredbu:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Odredite `Primary` ili `Secondary` za `key-type`.

Dodatne informacije o ta naredba Pokreni u `azure rediscache renew-key -h` naredbe.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Popis primarni i sekundarni tipke postojećeg predmemorija Redis

Da biste popis primarni i sekundarni tipke postojeće Redis predmemorije, koristite sljedeću naredbu:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Dodatne informacije o ta naredba Pokreni u `azure rediscache list-keys -h` naredbe.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
