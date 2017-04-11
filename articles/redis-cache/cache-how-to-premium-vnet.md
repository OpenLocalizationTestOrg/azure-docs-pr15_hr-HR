<properties 
    pageTitle="Upute za konfiguriranje podrške za virtualne mreže za Premium Azure Redis predmemoriju | Microsoft Azure" 
    description="Saznajte kako stvarati i upravljati virtualne mreže podrška za vaše instance Premium sloju predmemorije Redis Azure" 
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

# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Kako konfigurirati virtualne podrška za mrežu Premium Azure Redis predmemoriju
Azure Redis predmemorije ima različite predmemorije ponude koje omogućuju fleksibilnost u izbor veličina predmemorije i značajkama, uključujući nove sloju Premium.

Azure Redis predmemorije premium značajkama sloju obuhvaćaju Klasteriranje postojanost i podršku virtualne mreže (VNet). U VNet je privatna mreža u oblaku. Kada instance predmemorije Redis Azure je konfiguriran za korištenje na VNet, nije moguće javno adresirati i možete pristupiti samo iz virtualnim strojevima i aplikacijama u na VNet. U ovom se članku objašnjava kako konfigurirati virtualne mreže podrška za instancu za Azure Redis predmemorije premium.

>[AZURE.NOTE] Azure Redis predmemorije podržava oba klasični i VNets OKVIRA.

Informacije o drugim premium značajkama predmemorije, potražite u članku [Uvod u sloju Azure Redis predmemorije Premium](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Zašto VNet?
Uvođenje [Azure virtualne mreže (VNet)](https://azure.microsoft.com/services/virtual-network/) nudi bolja sigurnost i odvajanja Azure Redis predmemorije, kao i podmreže, pravilnici kontrole programa access i drugih značajki da biste dodatno ograničiti pristup Azure Redis predmemoriju.

## <a name="virtual-network-support"></a>Podrška za virtualne mreže
Podrška za virtualne mrežu (VNet) je konfiguriran na **Novu predmemoriju Redis** plohu tijekom stvaranja predmemoriju. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Kada odaberete premium cijene sloju, Integracija Azure Redis predmemorije VNet možete konfigurirati tako da odaberete VNet koji se nalazi u istom pretplate i mjesto kao predmemoriju. Da biste koristili novi VNet, prvo stvorite prema uputama u [stvorite virtualne mreže pomoću portala za Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) ili [virtualne mreže (klasični) pomoću portala za Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md) , a zatim se vratite na **Novu predmemoriju Redis** plohu za stvaranje i konfiguriranje predmemorije premium.

Da biste konfigurirali VNet za novu predmemoriju, kliknite **Virtualne mreže** na **Novu predmemoriju Redis** plohu pa odaberite željeni VNet s padajućeg popisa.

![Virtualne mreže][redis-cache-vnet]

Odaberite željeni podmreže s padajućeg popisa **podmreže** i navedite željeni **statičke IP adresa**. Ako koristite klasični VNet polje **statičke IP adresa** nije obavezan, a ako ništa nije naveden, jedna je odabran iz odabranog podmreže.

>[AZURE.IMPORTANT] Kada uvođenja programa Azure Redis predmemorije u OBLAK VNet predmemorije mora biti u namjenski podmreže koja sadrži ostali resursi osim instance predmemorije Redis Azure. Ako pokušaju za implementaciju sustava Azure Redis predmemorije da biste na ARM VNet da biste podmreži koja sadrži ostali resursi, uvođenje neće uspjeti.

![Virtualne mreže][redis-cache-vnet-ip]

>[AZURE.IMPORTANT] Prva četiri adresa u podmreži rezervirano i ne može se koristiti. Dodatne informacije potražite u članku [postoje li ograničenja korištenja IP adrese unutar te podmreže?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Nakon stvaranja predmemoriju konfiguraciju za na VNet možete pogledati tako da kliknete **Virtualne mreže** s plohu **Postavke** .

![Virtualne mreže][redis-cache-vnet-info]


Da biste povezali vašoj instanci Azure Redis predmemorije pri korištenju programa VNet, navedite naziv glavnog računala predmemorije u nizu za povezivanje kao što je prikazano u sljedećem primjeru.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Azure Redis predmemorije VNet najčešća Pitanja

Sljedeći popis sadrži odgovore na najčešća pitanja o Azure Redis predmemorije skaliranja.

-   [Što su neke uobičajene probleme konfiguraciji s Azure Redis predmemorije i VNets?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
-   [Mogu li koristiti VNets pomoću standardnih ili osnovni predmemorije?](#can-i-use-vnets-with-a-standard-or-basic-cache)
-   [Ne stvaranje Redis predmemorije Neuspjelo u nekim podmreže, ali ne i ostalima](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
-   [Funkcioniraju li sve značajke predmemorije kada hosting predmemorije u na VNET?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)


## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Što su neke uobičajene probleme konfiguraciji s Azure Redis predmemorije i VNets?

Kada Azure Redis predmemorije je smještena u VNet, koriste se priključke u tablici u nastavku. Ako su blokirane priključke, predmemoriju možda neće pravilno funkcionirati. Imate jedan ili više priključke blokirana je najčešće konfiguraciji problem kada koristite Azure Redis predmemorije u na VNet.

| Priključke     | Smjer        | Protokol za prijenos | Svrha                                                                           | Udaljena IP                           |
|-------------|------------------|--------------------|-----------------------------------------------------------------------------------|-------------------------------------|
| 80, 443     | Izlazni         | TCP                | Redis ovisnosti na Azure prostora za pohranu/PKI (Internet)                                | *                                   |
| 53          | Izlazni         | TCP/UDP            | Redis međuzavisnosti DNS (Internet na VNet)                                         | *                                   |
| 6379, 6380  | Dolazni          | TCP                | Komunikacija klijenta u Redis, Azure opterećenja                               | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 8443        | Dolazni/odlazni | TCP                | Detalji o implementaciji za Redis                                                   | VIRTUAL_NETWORK                     |
| 8500        | Dolazni          | TCP/UDP            | Azure za ujednačavanje opterećenja                                                              | AZURE_LOADBALANCER                  |
| 10221 10231 | Dolazni/odlazni | TCP                | Detalji o implementaciji za Redis (mogu ograničiti udaljene krajnjoj točki VIRTUAL_NETWORK) | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 13000 13999 | Dolazni          | TCP                | Komunikacija klijenta u Redis klastere, Azure opterećenja                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 15000 15999 | Dolazni          | TCP                | Komunikacija klijenta u Redis klastere, Azure opterećenja                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 16001       | Dolazni          | TCP/UDP            | Azure za ujednačavanje opterećenja                                                              | AZURE_LOADBALANCER                  |
| 20226       | Dolazni + izlaznog | TCP                | Detalji o implementaciji za klastere Redis                                          | VIRTUAL_NETWORK                     |


Postoje preduvjete za povezivanje s mreže predmemoriju Azure Redis koja možda neće biti prethodno zadovoljen u virtualne mreže. Azure Redis predmemorije zahtijeva sve sljedeće stavke da bi se ispravno funkcionirati kada se koristi unutar virtualne mreže.

-  Izlazni mrežne veze radi pohrane Azure krajnje točke diljem svijeta. To obuhvaća krajnje točke koja se nalazi u istom području kao instanca Azure Redis predmemorije kao i krajnje točke za pohranu nalaze u **drugim** Azure područja. Razrješavanje Azure krajnje točke za pohranu u odjeljku sljedeće DNS domene: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*i *file.core.windows.net*. 
-  Izlazni mrežne veze radi *ocsp.msocsp.com*, *mscrl.microsoft.com* i *crl.microsoft.com*. Ovo povezivanje potreban je za podršku funkcionalnosti SSL.
-  Konfiguracija DNS-a za virtualne mreže mora imati sposobnost Razrješavanje sve krajnje točke i domene koje se spominju u ranije točke. Osiguravanje valjani infrastrukture DNS je konfiguriran i održava za virtualne mreže mogu se podudarati te preduvjete za DNS.



### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Mogu li koristiti VNets pomoću standardnih ili osnovni predmemorije?

VNets se može koristiti samo s premium predmemorije.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Ne stvaranje Redis predmemorije Neuspjelo u nekim podmreže, ali ne i ostalima

Ako je Azure Redis predmemorije implementirate u OBLAK VNet, predmemoriju mora biti u namjenski podmreže koja sadrži nijedna vrsta resursa. Ako pokušaju za implementaciju sustava Azure Redis predmemorije u programa podmreži ARM VNet koja sadrži ostali resursi, uvođenje neće uspjeti. Postojećih resursa unutar podmreži morate izbrisati prije nego što možete stvoriti novu predmemoriju Redis.

Različite vrste resursa možete implementirati klasični VNet pod uvjetom da imate dovoljno IP adrese na disku.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Funkcioniraju li sve značajke predmemorije kada hosting predmemorije u na VNET?

Kada je predmemoriju dio na VNET, samo klijente u na VNET možete pristupiti predmemoriju. Zbog toga sljedeće značajke upravljanja predmemorije ne funkcioniraju u ovo vrijeme.

-   Redis konzole – jer Redis konzole koristi klijentski redis cli.exe hostirane na VMs koji nisu dio vaše VNET, ona ne može povezati s predmemoriju.


## <a name="use-expressroute-with-azure-redis-cache"></a>Korištenje ExpressRoute s Azure Redis predmemorije

Kupcima možete povezati je elektronička [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) njihove virtualne mrežne infrastrukture tako da biste Azure proširivanje njihove lokalne mreže. 

Prema zadanim postavkama novostvorenu elektronička ExpressRoute oglašava zadani smjer koji omogućuje izlaznog s Internetom. Uz tu konfiguraciju klijentske aplikacije su možete povezati s drugim Azure krajnje točke uključujući Azure Redis predmemoriju.

No uobičajenih konfiguracije klijenta je za definiranje vlastitih zadani smjer (0.0.0.0/0) koje navodi izlaznog internetski promet na umjesto tijek lokalnog. Ovaj tijek prometa invariably prijelomi povezivanje s Azure Redis predmemorije jer odlazni promet ili blokirani lokalnog ili NAT bi one neprepoznatljivog skup adrese koje više ne funkcioniraju s različitim Azure krajnje točke.

Rješenje je da biste definirali (neke) korisnički definirane usmjerava (UDRs) na podmreži koja sadrži predmemoriju Redis Azure. Na UDR definira usmjerava podmreže specifične koji će se na snazi umjesto zadani smjer.

Ako je to moguće, preporučuje se da biste koristili konfiguraciju sljedeće:

- Konfiguriranje ExpressRoute oglašava 0.0.0.0/0, a zatim po zadanom prisilno tunnels sve odlazni promet na lokalni.
- UDR primjenjuje se na podmreži koja sadrži Azure Redis predmemoriju definira 0.0.0.0/0 sljedeći put vrstom Internet (primjer to nije ono prema dolje u ovom članku).

Kombinirani efekt od ovih koraka je da razinu podmreže UDR ima prednost pred ExpressRoute prisilno tuneliranje, stoga osiguravanje izlaznog pristup Internetu iz predmemorije za Redis Azure.

Iako povezivanja s instancom Azure Redis predmemorije iz lokalnog aplikacije pomoću ExpressRoute nije scenarij uobičajenog korištenja zbog boljih performansi (za najbolje performanse Redis Azure predmemorije klijenti mora biti u području isti kao predmemoriju Redis Azure) u ovom scenariju izlazni mrežni put ne putovanja putem interne tvrtke proxyji niti može biti prisilno tunneled na lokalni. Na taj način mijenja učinkovitih NAT adresu izlazni mrežni promet iz predmemorije za Redis Azure. Promjena adrese NAT programa Azure Redis predmemorije izlazni mrežni promet na instancu uzrokuje povezivanje neuspjeha mnogim krajnje točke naveden. Rezultira nije uspjelo prilikom pokušaja stvaranja Azure Redis predmemoriju.

**Važno:**  Usmjerava definirano u na UDR **mora** biti dovoljno specifična za prednost pred bilo koji usmjerava objavio ExpressRoute konfiguracije. Sljedeći primjer koristi 0.0.0.0/0 širok raspon adresu, a kao potencijalno mogu nehotice nadjačati usmjeravanje oglasa pomoću konkretne rasponi adresa.

**Važno:**  Azure Redis predmemorije nije podržan ExpressRoute konfiguracije te **pogrešno unakrsno-oglašavanje usmjerava iz javne peering put do privatne peering put**. Konfiguracija ExpressRoute koji imaju javno peering konfiguriran, primit će smjera od Microsofta za veliki skup rasponi Microsoft Azure IP adresa. Ako su te rasponi adresa neispravno unakrsno-objavljeno privatne peering puta, rezultat je sve izlazne mrežnih paketa iz predmemorije Redis Azure instancu podmreže su neispravno prisilno-tunneled klijenta lokalnog mrežne infrastrukture. Ovaj tijek mreže prijelomi Azure Redis predmemoriju. Rješenje za ovaj problem je da biste prestali usmjerava unakrsno oglašavanje iz javne peering put privatne peering put.

Osnovne informacije na korisnički definirane usmjerava dostupan je na [Pregled](../virtual-network/virtual-networks-udr-overview.md). 

Dodatne informacije o ExpressRoute potražite u članku [ExpressRoute Tehnički pregled](../expressroute/expressroute-introduction.md)

## <a name="next-steps"></a>Daljnji koraci
Saznajte kako koristiti više značajki predmemorije premium.

-   [Uvod u sloju Premium predmemorije Redis Azure](cache-premium-tier-intro.md)





  
<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

