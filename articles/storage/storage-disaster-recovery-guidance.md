<properties
    pageTitle="Što učiniti u slučaju programa Azure prostora za pohranu prekida | Microsoft Azure"
    description="Što učiniti u slučaju do prekida Azure prostora za pohranu"
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Što učiniti ako je nestalo Azure prostora za pohranu

Microsoft, radimo konačnog da biste bili sigurni da su uvijek dostupne naših usluga. Ponekad navodi izvan naš utjecaj kontrola nam na načine koji uzrokovati kvarove neplanirano servisa u jednu ili više područja. Da biste lakše rukovati tih rijetko pojava, dajemo sljedeće više razine smjernice za servise za pohranu Azure.

## <a name="how-to-prepare"></a>Kako pripremiti 

Ključne je važnosti za svaki klijenta za pripremu vlastite Izrada plan za oporavak. Trud se može oporaviti iz spremišta nedostupnosti obično uključuje operacije osoblje i automatiziranog postupke da bi se ponovno aktivirati aplikacija u funkcionira stanju. Pogledajte Azure dokumentaciju ispod da biste sastavili vlastiti plan za oporavak Izrada:

-   [Izrada oporavak i visoke dostupnosti za Azure aplikacije](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)

-   [Tehnički vodič Azure otpornost](../resiliency/resiliency-technical-guidance.md)

-   [Servisa Azure oporavak web-mjesta](https://azure.microsoft.com/services/site-recovery/)

-   [Azure replikacijom prostora za pohranu](storage-redundancy.md)

-   [Servisa Azure sigurnosne kopije](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Kako prepoznati 

Preporučeni način da biste odredili stanje servisa Azure je da biste se pretplatili na [Nadzorna ploča za stanje servisa Azure](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Što učiniti ako je nestalo prostora za pohranu

Ako jedan ili više prostora za pohranu servisa privremeno nedostupan na jednu ili više područja, postoje dvije mogućnosti za koje treba uzeti u obzir. Ako po volji odmah ostvarite pristup vašim podacima, razmotrite mogućnost 2.

### <a name="option-1-wait-for-recovery"></a>Mogućnost 1: Čekanja za oporavak

U ovom slučaju ne poduzmete na svoj dio potreban je. Radimo održavate da biste vratili dostupnost servisa Azure. Možete nadzirati status servisa u [Nadzorna ploča za stanje servisa Azure](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>Mogućnost 2: Kopiranje podataka iz sekundarnog

Ako se odlučite [za pohranu zemlj suvišnih pristup za čitanje (RA GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (preporučeno) za račune za pohranu, morat ćete pristup za čitanje podataka iz sekundarne područja. Možete koristiti alate kao što su [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)i [Premještanje podataka Azure biblioteke](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) u neki drugi račun za pohranu u unimpacted regiji kopirate podatke iz sekundarne područja, a zatim pokažite aplikacija za taj račun za pohranu za čitanje i pisanje dostupnost.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Što mogu očekivati ako se dogodi prebacivanje za pohranu

Ako ste odabrali [zemlj suvišnih prostora za pohranu (GRS)](storage-redundancy.md#geo-redundant-storage) ili [pristup za čitanje zemlj suvišnih prostora za pohranu (RA GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (preporučeno), za pohranu Azure će zadržati podatke durable u dva područja (primarnih i sekundarnih). Azure prostora za pohranu u obje regije neprestano održava više replike podataka.

Kada regionalne Izrada utječe na vaš primarni regija, ne možemo će prvi put pokušate vratiti uslugu u tom području. Ovisno o prirode na Izrada i njegov impacts u nekim rijetko prilike smo možda neće biti moći vratiti primarni regija. U tom trenutku smo izvedite prebacivanje zemlj.. Replikacija regije-podataka je proces asinkronog koji može obuhvaćati stanke, pa je moguće promjene koje još je replicirati na sekundarnom područja mogu se izgubiti. Upit možete poslati ["vrijeme zadnje sinkronizacije" vašeg računa za pohranu](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) da biste vidjeli detalje o statusu replikacijom.

Dvije točke vezane uz zemlj prebacivanje sučelje za pohranu:

-   Samo se pokrenuti zemlj prebacivanje za pohranu od tima za pohranu Azure – je potrebna akcija klijenta.

-   Postojeće točke servisa za pohranu za blob-ova, tablice, redovima i datoteke će ostati nakon prebacivanje; unos DNS ćete morati ažurirati da biste se prebacili primarni regija sekundarne regiju.

-   Prije i tijekom prebacivanje zemlj., nećete moći pristupiti pisanje na račun servisa za pohranu zbog utjecaj na Izrada, ali i dalje vidite iz sekundarne ako vaš račun za pohranu konfiguriran kao RA GRS.

-   Kada je dovršena prebacivanje zemlj, a promjene DNS preneseni, čitanje i pisanje pristup računu za pohranu će se nastaviti. Upit možete poslati ["zemlj prebacivanje posljednjeg" vašeg računa za pohranu](https://msdn.microsoft.com/library/azure/ee460802.aspx) da biste vidjeli dodatne detalje.

-   Nakon prebacivanje, računa za pohranu će biti potpuno funkcionalna, ali u "su smanjene" status, kao što je zapravo nalazi u regiji samostalne s nije moguće zemlj replikacijom. U ublažavanju ovog rizika ćemo će vratiti izvorne primarni regija i učinite na failback zemlj da biste vratili u izvorno stanje. Ako je izvorni primarni regija nepopravljiva, ne možemo će dodijeliti drugoj regiji sekundarne.
Dodatne informacije o infrastrukture replikacije prostora za pohranu Azure zemlj., pogledajte članak na blogu tima za pohranu o [zalihosti mogućnosti i RA GRS](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

##<a name="best-practices-for-protecting-your-data"></a>Najbolje prakse za zaštitu podataka

Postoje neke preporučene postupke za redovito sigurnosno kopiranje podataka za pohranu.

-   Diskova VM – pomoću [servisa Azure sigurnosne kopije](https://azure.microsoft.com/services/backup/) sigurnosno kopiranje diskova VM koristi Azure virtualnih računala.

-   Blokiranje blob-ova – stvaranje na [snimke](https://msdn.microsoft.com/library/azure/hh488361.aspx) svake blob blok ili Kopiraj blob polja za pohranu za neki drugi račun u drugoj regiji pomoću [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)ili [Premještanje podataka Azure biblioteke](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).

-   Tablica – pomoću [AzCopy](storage-use-azcopy.md) izvezite podatke u tablici u neki drugi račun za pohranu u drugoj regiji.

-   Datoteke – koriste [AzCopy](storage-use-azcopy.md) ili [Azure PowerShell](storage-powershell-guide-full.md) da biste kopirali datotekama s drugim računom za pohranu u drugoj regiji.
