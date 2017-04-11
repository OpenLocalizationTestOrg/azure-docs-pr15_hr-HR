<properties
    pageTitle="Pretplate na Microsoft Azure i ograničenja servisa, kvota i ograničenjima"
    description="Daje popis uobičajenih Azure pretplate i ograničenja servisa, kvota i ograničenja. To obuhvaća informacije o tome kako povećati ograničenja uz maksimalne vrijednosti."
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>

# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure pretplate i ograničenja servisa, kvota i ograničenjima

## <a name="overview"></a>Pregled

Ovaj dokument određuje neke od najčešćih ograničenja Microsoft Azure. To trenutno obuhvaća sve Azure servise. S vremenom ta ograničenja bit će proširen i ažurirati da prekrije od platforme.

Posjetite [Azure cijene pregled](https://azure.microsoft.com/pricing/) da biste saznali više o Azure cijene. Postoji, procjenu troškove pomoću [Cijene Kalkulator](https://azure.microsoft.com/pricing/calculator/) ili potražite na stranici Detalji o cijene za servis (na primjer, [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).

> [AZURE.NOTE] Ako želite podići ograničenje iznad **Zadano ograničenje**, možete ga [otvorite zahtjev za podršku mrežna služba bez troškova](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Ograničenja ne može biti potenciju iznad **Maksimalno ograničenje** vrijednosti u tablicama u nastavku. Ako nema stupca **Maksimalno ograničenje** , od navedenog resursa ne sadrži prilagodljivim ograničenja.

## <a name="limits-and-the-azure-resource-manager"></a>Ograničenja i Azure Voditelj resursa

Sada je moguće kombinirati više Azure resursa u jednu grupu resursa Azure. Kada koristite grupe resursa, ograničenja koja jednom su globalni postaju upravlja regionalne razine s Voditelj resursa Azure. Dodatne informacije o grupama resursa Azure potražite u članku [Pregled upravljanja resursima Azure](azure-resource-manager/resource-group-overview.md).

U nastavku ograničenja novu tablicu je dodan u skladu s vizualnim sve razlike u ograničenja prilikom korištenja Voditelj resursa Azure. Ako, na primjer, postoji tablice za **Pretplatu ograničenja** i tablice **Pretplate ograničenja - Azure Voditelj resursa** . Kada je ograničenje se primjenjuje na oba scenarija, samo prikazane u prvoj tablici. Osim ako nije drukčije naznačeno, ograničenja su globalne preko sva područja.

> [AZURE.NOTE] Važno da biste istaknuli kvote za resurse u grupama resursa Azure su po regije-dostupno za pretplatu, a nisu po – pretplate, kao što su Upravljanje kvotama servis je. Pogledajmo pomoću core kvote kao primjer. Ako vam je potrebna za upućivanje zahtjeva za povećanje kvota s podrškom za jezgri, morate odlučiti koliko jezgri koji želite koristiti u koje regijama i zatim izvršili određene zahtjev za grupu resursa Azure Jezgra kvote za iznose i regije koje želite. Stoga, ako je potrebno koristiti 30 jezgri u Europi Zapad da biste pokrenuli aplikaciju postoji; trebali biste izričito zatražite 30 jezgri u Europi Zapad. Ali nećete imati core kvote produljili u druga regija--Zapad Europe će imati 30 core kvote.
<!-- -->
Zbog toga možda korisni treba uzeti u obzir pri odlučivanju što kvote za grupu resursa Azure moraju biti za svoje radno opterećenje u bilo kojem određenu regiju i zatražite tu količinu na svakom području u koje namjeravate implementacije. Potražite u članku [otklanjanje problema s implementacije](./resource-manager-common-deployment-errors.md) dodatnu pomoć otkrivanje svoje trenutne kvote za određena područja.

## <a name="service-specific-limits"></a>Ograničenja specifične za servis

- [Active Directory](#active-directory-limits)
- [Upravljanje API-JA](#api-management-limits)
- [Aplikacije servisa](#app-service-limits)
- [Pristupnik za aplikaciju](#application-gateway-limits)
- [Aplikacija uvida](#application-insights-limits)
- [Automatizacija](#automation-limits)
- [Azure Redis predmemorije](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Sigurnosno kopiranje](#backup-limits)
- [Grupe](#batch-limits)
- [BizTalk Services](#biztalk-services-limits)
- [CDN](#cdn-limits)
- [Servisi u oblaku](#cloud-services-limits)
- [Tvorničke podataka](#data-factory-limits)
- [Analitički Lake podataka](#data-lake-analytics-limits)
- [DNS-A](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Događaj koncentratora](#event-hubs-limits)
- [IoT koncentratora](#iot-hub-limits)
- [Ključni zbirke ključeva](#key-vault-limits)
- [Media Services.](#media-services-limits)
- [Mobilni radnje](#mobile-engagement-limits)
- [Mobilne usluge](#mobile-services-limits)
- [Nadzor](#monitoring-limits)
- [Višestruka provjera autentičnosti](#multi-factor-authentication)
- [Povezivanje s mrežom](#networking-limits)
- [Servis koncentrator za obavijesti](#notification-hub-service-limits)
- [Radu uvida](#operational-insights-limits)
- [Grupa resursa](#resource-group-limits)
- [Raspored](#scheduler-limits)
- [Pretraživanje](#search-limits)
- [Servis Bus](#service-bus-limits)
- [Oporavak web-mjesta](#site-recovery-limits)
- [SQL baze podataka](#sql-database-limits)
- [Prostor za pohranu](#storage-limits)
- [StorSimple sustava](#storsimple-system-limits)
- [Strujanje Analytics](#stream-analytics-limits)
- [Pretplate](#subscription-limits)
- [Upravitelj promet](#traffic-manager-limits)
- [Virtualnim strojevima](#virtual-machines-limits)
- [Skupovi skaliranje virtualnog računala](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>Ograničenja za pretplatu
#### <a name="subscription-limits"></a>Ograničenja za pretplatu
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Ograničenja za pretplatu - Voditelj resursa za Azure

Sljedeća ograničenja primjenjuju se prilikom korištenja Azure Voditelj resursa i grupa resursa za Azure. Ograničenja koje su se promijenile s Voditelj resursa Azure ne navode se ispod. Pogledajte u prethodnoj tablici ta ograničenja.

Informacije o rukovanje ograničenja zahtjeva za Voditelj resursa, potražite u članku [zahtjeva za ograničavanje Voditelj resursa](resource-manager-request-limits.md).

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>Ograničenja grupa resursa

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Ograničenja virtualnim strojevima
#### <a name="virtual-machine-limits"></a>Ograničenja virtualnog računala
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>Ograničenja virtualnim strojevima - Voditelj resursa za Azure

Sljedeća ograničenja primjenjuju se prilikom korištenja Azure Voditelj resursa i grupa resursa za Azure. Ograničenja koje su se promijenile s Voditelj resursa Azure ne navode se ispod. Pogledajte u prethodnoj tablici ta ograničenja.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Ograničenja skupove skaliranje virtualnog računala

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>Ograničenja za povezivanje s mrežom

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Ograničenja za povezivanje s mrežom
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Pristupnik za aplikaciju ograničenjima

[AZURE.INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>Upravitelj promet ograničenja

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>Ograničenja DNS-a

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Ograničenja prostora za pohranu

Dodatne informacije o ograničenjima prostora za pohranu računa potražite u članku [skalabilnost Azure prostora za pohranu i ciljeve](../articles/storage/storage-scalability-targets.md).

#### <a name="storage-service-limits"></a>Ograničenja prostora za pohranu servisa

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>Ograničenja na disku virtualnog računala

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Dodatne informacije potražite u odjeljku [Veličina virtualnog računala](../articles/virtual-machines/virtual-machines-linux-sizes.md) .

**Računa standardnu prostora za pohranu**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Računi za pohranu Premium**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Ograničenja prostora za pohranu davatelja resursa

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>Ograničenja za servise u oblaku

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>Ograničenja servisa aplikacija
Sljedeća ograničenja aplikacije servisa za obuhvaćaju ograničenja za web-aplikacije, mobilne aplikacije, API aplikacije i logike aplikacije.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Ograničenja za raspored

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Ograničenja za obradu

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>Ograničenja servisa BizTalk
Sljedeća tablica prikazuje ograničenja za Azure Biztalk servise.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>Ograničenja DocumentDB

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Kvota navedene s [može se prilagoditi tako da se obratite podršci za Azure](./documentdb/documentdb-increase-limits.md)zvjezdica (*).

### <a name="mobile-engagement-limits"></a>Ograničenja mobilne radnje

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>Ograničenja pretraživanja

Cijene razine odredite kapacitetu i ograničenja servisa za pretraživanje. Razine obuhvaćaju sljedeće:

- Servis za više klijentu *slobodno* , zajednički koristiti s drugim Azure pretplatnike namijenjene procjenu i small razvoj projekata.
- *Osnovni* nudi namjenski računalno resursi za radnih opterećenja radnog pri manje mjerilo s najviše tri replike za radnih opterećenja iznimno dostupna upita.
- *Standardna (S1, S2, S3, Visoko gustoće S3)* je veće radnih opterećenja radnog. Više razina postoje standardne sloju tako da odaberete konfiguracija resursa za nekim konkretnim scenarijima.

**Ograničenja po pretplati**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Ograničenja po servisa za pretraživanje**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Precizniji informacije o drugim ograničenjima, uključujući veličinu dokumenta, upiti po sekundi, tipke, zahtjeve i odgovore, potražite u članku [ograničenja servisa Azure pretraživanja](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Ograničenja Media Services

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN ograničenja

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Ograničenja mobilne usluge

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>Ograničenja za nadzor

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Ograničenja servisa koncentrator obavijesti

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Ograničenja koncentratora događaja

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Ograničenja servisa Bus

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>Ograničenja IoT koncentratora

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Ograničenja tvorničke podataka

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Ograničenja Lake analize podataka
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>Strujanje analize ograničenja
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Ograničenjima servisa Active Directory

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Azure RemoteApp ograničenja

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>Ograničenja StorSimple sustava

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>Ograničenja radu uvida

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Sigurnosno kopiranje ograničenja

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Ograničenja za oporavak web-mjesta

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Ograničenja uvida aplikacije

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>Ograničenja za upravljanje API-JA

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure Redis predmemorije ograničenja

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Ograničenja sigurnog ključ

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Višestruka provjera autentičnosti
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Automatizacija ograničenja
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>Ograničenja SQL baze podataka

Ograničenja baze podataka SQL potražite u članku [Ograničenja resursa za SQL baze podataka](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Vidi također

[Razumijevanje Azure ograničenja i povećava](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtualnog računala i veličine servisa oblaka za Azure](virtual-machines/virtual-machines-linux-sizes.md)

[Veličina za servise u Oblaku](cloud-services/cloud-services-sizes-specs.md)
