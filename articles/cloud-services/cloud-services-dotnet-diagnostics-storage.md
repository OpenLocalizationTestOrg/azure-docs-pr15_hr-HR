<properties
    pageTitle="Pohrana i prikaz dijagnostičkih podataka Azure prostora za pohranu | Microsoft Azure"
    description="Preuzimanje podataka Azure Dijagnostika u prostor za pohranu Azure te prikazati"
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="cloud-services"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="robb" />

# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Pohrana i prikaz dijagnostičkih podataka u spremište Azure

Dijagnostičkih podataka ne sprema se trajno osim ako transfer emulator prostora za pohranu za Microsoft Azure ili Azure prostora za pohranu. Jednom u prostor za pohranu, mogu vidjeti s jednim od nekoliko dostupno alata.

## <a name="specify-a-storage-account"></a>Navedite račun za pohranu

Navedite račun za pohranu koji želite koristiti u datoteci ServiceConfiguration.cscfg. Informacije o računu se definira kao niz za povezivanje u postavke konfiguracije. Sljedeći primjer prikazuje niz za povezivanje na zadani stvaranja novog projekta u Oblaku u Visual Studio:


```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Možete promijeniti ovaj niz za povezivanje za unos informacija o računu za račun za Azure prostora za pohranu.

Ovisno o vrsti dijagnostičkih podataka koji se prikupljaju, Azure Dijagnostika koristi servis Blob ili servis tablice. Sljedeća tablica prikazuje izvori podataka koje su dosljedan i njihov oblik.

|Izvor podataka|Oblik spremanja|
|---|---|
|Azure zapisnika|Tablica|
|IIS 7.0 zapisnika|Blob|
|Azure zapisnika infrastrukture za dijagnostiku|Tablica|
|Nije uspjelo zapisnika zahtjev za praćenje|Blob|
|Zapisnike događaja sustava Windows|Tablica|
|Mjerača performansi|Tablica|
|Ispisi rušenje|Blob|
|Prilagođeni zapisivanje|Blob|

## <a name="transfer-diagnostic-data"></a>Prijenos dijagnostičkih podataka

SDK 2,5 i novijim verzijama zahtjev za prijenos dijagnostičkih podataka može doći do konfiguracijskoj datoteci. Dijagnostičkih podataka možete prenijeti u određenim intervalima kao što je navedeno u konfiguraciji.

Za SDK 2.4 ili prethodnu možete zatražiti da biste prenijeli dijagnostičkih podataka putem datoteku konfiguracije kao programski. Programatski pristup omogućuje vam prijenosa na zahtjev.


>[AZURE.IMPORTANT] Kada prenosite dijagnostičkih podataka na račun za Azure prostora za pohranu, plaćati troškova za resurse za pohranu koji koristi dijagnostičkih podataka.

## <a name="store-diagnostic-data"></a>Spremanje dijagnostičkih podataka

Podaci iz zapisnika je pohranjena u spremište blobova ili tablice s nazivima sljedeće:

**Tablica**

- **WadLogsTable** – zapisnici pisan kod korištenjem ga slušatelj praćenje.

- Mijenja se **WADDiagnosticInfrastructureLogsTable** - dijagnostičkih monitora i konfiguraciji.

- **WADDirectoriesTable** – direktorije koji nadzire dijagnostičkih monitor.  To obuhvaća IIS zapisnike, IIS nije uspjela zahtjev zapisnika i prilagođeni direktorija.  Mjesta datoteke zapisnika blob je naveden u polju spremnik a ime blob-om u polju RelativePath.  Polje AbsolutePath označava mjesto i naziv datoteke, kao što je postojali Azure virtualnog računala.

- **WADPerformanceCountersTable** – mjerača performansi.

- **WADWindowsEventLogsTable** – zapisnika događaja sustava Windows.

**Blob-ova**

- **wad kontrola spremnik** – (samo za SDK 2.4 ili prethodnu) sadrži XML konfiguracijska datoteka koje određuje Azure Dijagnostika.

- **wad iis failedreqlogfiles** – sadrži podatke iz IIS nije uspjela zahtjev zapisnike.

- **wad iis logfiles** – sadrži informacije o IIS zapisnike.

- **"Prilagođeno"** – spremniku prilagođene ovisno o konfiguriranju direktorije koji nadzire dijagnostičkih monitor.  Naziv ovaj spremnik blob će biti naveden u WADDirectoriesTable.

## <a name="tools-to-view-diagnostic-data"></a>Alati za prikaz dijagnostičkih podataka
Nekoliko alata dostupnih za prikaz podataka kada se prenose u spremište. Ako, na primjer:

- Poslužitelj Explorer u Visual Studio – ako ste instalirali Azure alata za Microsoft Visual Studio, koristite čvor Azure prostora za pohranu u programu Explorer Server da biste pogledali blob samo za čitanje i tablica s podacima o poslovnim subjektima Azure prostora za pohranu. Može prikazati podatke s lokalno spremište emulator računa, a i s računa za pohranu koji ste stvorili za Azure. Dodatne informacije potražite u članku [pregledavanja i upravljanje resursa za pohranu pomoću programa Explorer poslužitelja](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).

- [Microsoft Azure prostora za pohranu Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostalnu aplikaciju koja omogućuje jednostavno rad s podacima Azure prostora za pohranu u sustavu Windows, OSX i Linux.

- [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) obuhvaća Upravitelj Dijagnostika Azure koji omogućuje prikaz, preuzmite i upravljanje Dijagnostika podatke prikupljene putem aplikacije koje rade na Azure.


## <a name="next-steps"></a>Daljnji koraci

[Praćenje tijeka u aplikaciji za servise u Oblaku s Dijagnostika Azure](cloud-services-dotnet-diagnostics-trace-flow.md)
