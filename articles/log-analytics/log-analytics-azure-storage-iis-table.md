<properties
    pageTitle="Pomoću spremište blobova platforme za IIS i tablice za pohranu za događaje | Microsoft Azure"
    description="Prijava analitiku može čitati zapisnike za Azure servise koje pisanje Dijagnostika sa spremištem tablica ili IIS zapisnici mogli unijeti bloba prostora za pohranu."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="using-blob-storage-for-iis-and-table-storage-for-events"></a>Korištenje spremište blobova platforme za IIS i spremište tablica za događaje

Prijava analitiku možete pročitati u zapisnicima sljedeće servise koje pisanje Dijagnostika sa spremištem tablica ili IIS zapisnici mogli unijeti bloba prostora za pohranu:

+ Servis tkanina klaster (pretpregled)
+ Virtualnim strojevima
+ Web-tempiranja uloge

Prije nego što prijava analitiku možete prikupiti podatke za ove resurse, Azure Dijagnostika mora biti omogućen.

Kada su omogućeni Dijagnostika, možete koristiti portal za Azure ili PowerShell konfiguriranje prijava analitiku u prikupiti zapisnike.

Azure Dijagnostika je Azure kućni broj koji omogućuje prikupljanje dijagnostičkih podataka od uloge suradnika, uloga web ili virtualnog računala koji se izvodi u Azure. Podaci se pohranjuju u račun za Azure prostora za pohranu i prijava analitiku pa mogu prikupiti.

Za analizu zapisnika prikupljanje zapisnika te Azure Dijagnostika, zapisnike mora biti na sljedećim mjestima:

| Vrste zapisa | Vrsta resursa | Mjesto |
| -------- | ------------- | -------- |
| IIS zapisnika | Virtualnim strojevima <br> Uloge za web <br> Uloga suradnika | wad-iis-logfiles (blobova) |
| Syslog | Virtualnim strojevima | LinuxsyslogVer2v0 (tablice za pohranu) |
| Servis tkanina radu događaja | Čvorovi tkanina servisa | WADServiceFabricSystemEventTable |
| Servis tkanina pouzdanog glumca događaja | Čvorovi tkanina servisa | WADServiceFabricReliableActorEventTable |
| Servis tkanina pouzdanog servisa događaja | Čvorovi tkanina servisa | WADServiceFabricReliableServiceEventTable |
| Zapisnike događaja sustava Windows | Čvorovi tkanina servisa <br> Virtualnim strojevima <br> Uloge za web <br> Uloga suradnika | WADWindowsEventLogsTable (spremište tablica) |
| Za sustav Windows ETW zapisnika | Čvorovi tkanina servisa <br> Virtualnim strojevima <br> Uloge za web <br> Uloga suradnika | WADETWEventTable (spremište tablica) |

>[AZURE.NOTE] Zapisnici IIS s web-mjesta Azure trenutno nisu podržani.

Za virtualnim računalima imate mogućnost instalacije [agent prijava analitiku](log-analytics-azure-vm-extension.md) u virtualnog računala da biste omogućili dodatne uvide. Osim mogućnosti da biste analizirali IIS zapisnika i zapisnike događaja, možete poduzeti dodatne analize obuhvaća evidentiranja promjena konfiguracije, SQL procjenu i procjenu ažuriranja.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Omogućivanje Azure Dijagnostika u virtualnog računala za događaj i IIS prijave zbirke

Da biste omogućili Azure Dijagnostika u virtualnog računala za događaj i IIS prikupljanje zapisnika pomoću portala za Microsoft Azure pomoću sljedećeg postupka.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Da biste omogućili Azure Dijagnostika u virtualnog računala pomoću portala za Azure

1. Instalirajte VM Agent prilikom stvaranja virtualnog računala. Ako već postoji virtualnog računala, provjerite je li VM Agent već je instaliran.
    - Na portalu Azure dođite do virtualnog računala, odaberite **Neobavezno konfiguracija**, a zatim **Dijagnostika** i **Status** postavite **na**.

    Po dovršetku, na VM ima nastavak Azure Dijagnostika instaliran i pokrenut. Ovaj kućni broj je zadužen za prikupljanje podataka Dijagnostika.

2. Omogućite praćenje i konfiguriranje zapisivanja na postojeće VM događaja. Možete omogućiti Dijagnostika na razini VM. Da biste omogućili dijagnostika i konfiguriranje zapisivanja događaja, učinite sljedeće:
    1. Odaberite na VM.
    2. Kliknite **praćenje**.
    3. Kliknite **Dijagnostika**.
    4. Postavljanje **statusa** na **Uključeno**.
    5. Odaberite svaku Dijagnostika zapisnik koji želite prikupiti.
    7. Kliknite **u redu**.

Pomoću komponente PowerShell Azure preciznije navodite događaja koji se pišu sa spremištem Azure. Pogledajte [Prikupljanje podataka korištenjem Azure Dijagnostika napisali spremište tablica ili IIS zapisnici mogli unijeti bloba](log-analytics-azure-storage-json.md).


## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Omogućivanje Azure Dijagnostika u ulozi Web za IIS prikupljanje zapisnika i događaja

Pogledajte [Kako da biste omogućili dijagnostika u Oblaku](../cloud-services/cloud-services-dotnet-diagnostics.md) za općenite korake za omogućivanje Azure Dijagnostika. Upute u nastavku te podatke koristiti i prilagoditi za korištenje s zapisnika analize.

S Azure Dijagnostika omogućen:

- Zapisnici IIS spremaju se po zadanom s prenijeti u intervalu prijenos scheduledTransferPeriod podaci iz zapisnika.
- Prema zadanim postavkama ne prenose zapisnika događaja sustava Windows.

### <a name="to-enable-diagnostics"></a>Da biste omogućili dijagnostiku

Da biste omogućili zapisnike događaja sustava Windows ili da biste promijenili u scheduledTransferPeriod konfiguriranje Dijagnostika Azure pomoću konfiguracijska datoteka XML (diagnostics.wadcfg), kao što je prikazano u [4 korak: Stvaranje Dijagnostika konfiguracijskoj datoteci i instalirajte proširenje](../cloud-services/cloud-services-dotnet-diagnostics.md)

Sljedeći primjer konfiguracijska datoteka prikuplja IIS zapisnika i sve događaje iz zapisnika aplikacije i sustava:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Provjerite je li vaša ConfigurationSettings određuje je račun za pohranu, kao u sljedećem primjeru:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

Vrijednosti **AccountName** i **AccountKey** nalaze se na portalu za Azure u prostor za pohranu računa nadzorne ploče, u odjeljku Upravljanje pristupnih tipki. Protokol za niz za povezivanje mora biti **https**.

Nakon što ažurirane dijagnostičkih konfiguraciju primjenjuje se na servis u oblaku i pisanje Dijagnostika za pohranu Azure, zatim ste spremni za konfiguriranje zapisnika analize.


## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Prikupljanje zapisnika iz spremišta Azure pomoću portala za Azure

Portal za Azure možete koristiti za konfiguriranje zapisnika analize prikupljanje zapisnika za sljedeće Azure usluge:

+ Servis tkanina klastere
+ Virtualnim strojevima
+ Web-tempiranja uloge

Na portalu Azure idite na prijava analitiku radnog prostora i izvedite sljedeće zadatke:

1. Kliknite *Zapisnici o računima za pohranu*
2. Kliknite *Dodaj* zadatak
3. Odaberite račun za pohranu koji sadrži dijagnostički zapisnici
  - Taj račun može biti klasični prostora za pohranu račun ili račun Azure Voditelj resursa za pohranu
4. Odaberite vrstu podataka koje želite prikupljanje zapisnika za
  - Na raspolaganju su zapisnika IIS; Događaje; Syslog (Linux) Zapisnici ETW; Servis tkanina događaja
5. Vrijednost za izvor se automatski popuniti ovisno o vrsti podataka i ne može promijeniti
6. Kliknite u redu da biste spremili konfiguraciju

Ponovite korake 2-6 za vrste podataka koje želite evidencije analize da biste prikupili i račune dodatni prostor za pohranu.

U više od 30 minuta se moći vidjeti podatke s računa za pohranu u zapisnik analize. Prikazat će se samo podaci napisan za pohranu nakon primjene konfiguraciju. Prijava analitiku pročitajte postojećih podataka s računa za pohranu.

>[AZURE.NOTE] Portalu za provjeru valjanosti u račun za pohranu postoji izvorišnog web-mjesta ili ako se napisan nove podatke.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Omogućivanje Azure Dijagnostika u virtualnog računala za događaj i IIS prijave zbirke pomoću komponente PowerShell

Slijedite korake u [Konfiguriranje zapisnika analize indeksirati Azure dijagnostiku](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) da biste pročitali Azure Dijagnostika koji se pišu sa spremištem tablica pomoću komponente PowerShell.

Pomoću komponente PowerShell Azure preciznije navodite događaja koji se pišu sa spremištem Azure.
Dodatne informacije potražite u članku [Omogućavanje dijagnostike virtualnim računalima sustava u Azure](../virtual-machines-dotnet-diagnostics.md).

Možete omogućiti i ažuriranje Azure Dijagnostika pomoću sljedeće skripte komponente PowerShell.
Ovu skriptu možete koristiti i konfiguraciji prilagođene zapisivanje.
Izmjena skriptu da biste postavili račun za pohranu, naziv usluge i naziv virtualnog računala.
Skripta koristi cmdleta za klasični virtualnih računala.

Pregledajte sljedeće ogledne skripte, kopirajte je, izmijeniti prema potrebi, spremanje uzorka u obliku datoteke skriptu PowerShell i pokrenuti skriptu.

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Daljnji koraci

- [Korištenje JSON datoteka u spremište blobova platforme](log-analytics-azure-storage-json.md) za čitanje zapisnike Azure services taj unos Dijagnostika sa spremištem blobova u JSON OSNOVNI oblik.
- [Omogućivanje rješenja](log-analytics-add-solutions.md) za davanje uvid u podatke.
- [Korištenje upita za pretraživanje](log-analytics-log-searches.md) da biste analizirali podatke.
