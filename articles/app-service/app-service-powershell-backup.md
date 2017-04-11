<properties
    pageTitle="Pomoću komponente PowerShell sigurnosno kopiranje i vraćanje aplikacije servisa za aplikacije"
    description="Saznajte kako pomoću ljuske PowerShell za sigurnosno kopiranje i vraćanje aplikaciju u aplikacije servisa za Azure"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>Pomoću komponente PowerShell sigurnosno kopiranje i vraćanje aplikacije servisa za aplikacije

> [AZURE.SELECTOR]
- [PowerShell](app-service-powershell-backup.md)
- [REST API-JA](../app-service-web/websites-csm-backup.md)

Saznajte kako koristiti Azure PowerShell za sigurnosno kopiranje i vraćanje [aplikacije servisa za aplikacije](https://azure.microsoft.com/services/app-service/web/). Dodatne informacije o web sigurnosne kopije aplikacije, uključujući preduvjeti i ograničenja, potražite u članku [sigurnosno kopiranje web app u aplikacije servisa za Azure](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Preduvjeti
Da biste koristili ljuske PowerShell za upravljanje aplikacije sigurnosne kopije, potrebno je sljedeće:

- **A SAS URL** koji omogućuje čitanje i pristup za pisanje u spremniku Azure prostora za pohranu. Potražite u članku [objašnjenje SAS model](../storage/storage-dotnet-shared-access-signature-part-1.md) za objašnjenje SAS URL-ova. Primjeri Upravljanje prostorom za pohranu Azure pomoću komponente PowerShell potražite u članku [Korištenje Azure PowerShell s Azure prostora za pohranu](../storage/storage-powershell-guide-full.md) .
- **Niz za povezivanje baze podataka A** ako želite da sigurnosno kopiranje baze podataka zajedno s web-aplikaciju programa.

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Upute za generiranje URL-a SAS će se koristiti za sigurnosne kopije cmdleta za web app
URL-a SAS možete generirao PowerShell. Evo primjera kako da biste generirali onaj koji možete koristiti pomoću cmdleta koji se spominju u ovom članku.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Instaliranje modula Azure PowerShell 1.3.2 ili noviji

Upute za instalaciju i korištenje Azure PowerShell potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-install-configure.md) .

## <a name="create-a-backup"></a>Stvaranje sigurnosne kopije

Pomoću cmdleta New-AzureRmWebAppBackup da biste stvorili sigurnosnu kopiju web-aplikacijama.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Time se stvara sigurnosnu kopiju automatski generirani naziva. Ako želite dati naziv sigurnosne kopije, koristite BackupName neobavezan parametar.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

Da biste obuhvatili sigurnosnu kopiju baze podataka, najprije stvorite postavka sigurnosne kopije baze podataka pomoću cmdleta New-AzureRmWebAppDatabaseBackupSetting, a zatim navedite tu postavku u bazama podataka parametra cmdleta New-AzureRmWebAppBackup. Parametar baze podataka prihvaća polja postavki baze podataka, što omogućuje sigurnosno kopiranje više baza podataka.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Početak sigurnosnog kopiranja

Cmdlet Get-AzureRmWebAppBackupList vraća niz sve sigurnosne kopije za web-aplikacije. Morate navesti naziv web-aplikaciji i njezinu grupu resursa.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Da biste dobili određene sigurnosnu kopiju, koristite cmdlet Get-AzureRmWebAppBackup.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

U bilo kojem od cmdleti za sigurnosne kopije upravljanje pogodnost možete pipe i web-aplikacije objekt.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Zakazivanje automatskog sigurnosnog kopiranja

Možete planirati sigurnosne kopije dogoditi automatski u određenim intervalima. Da biste konfigurirali raspored za sigurnosnog kopiranja, koristite cmdlet uređivanje AzureRmWebAppBackupConfiguration. Ovaj cmdlet uzima nekoliko parametara:

- **Ime** - naziv web-aplikacije.
- **ResourceGroupName** - naziv grupe resursa koji sadrži web-aplikaciji.
- **Vremensko razdoblje** - neobavezno. Naziv vremensko razdoblje za web app.
- **StorageAccountUrl** - SAS URL za pohranu Azure spremnik koristi za spremanje sigurnosnih kopija.
- **FrequencyInterval** - numeričku vrijednost za koliko često treba napraviti sigurnosnih kopija. Moraju biti pozitivni cijeli broj.
- **FrequencyUnit** – jedinica vremena za koliko često treba napraviti sigurnosnih kopija. Mogućnosti su sat i dan.
- **RetentionPeriodInDays** - koliko je dana prije automatski brišu se spremiti automatsko sigurnosno kopiranje.
- **StartTime** - neobavezno. Vrijeme kad je automatsko sigurnosno kopiranje započeti. Sigurnosno kopiranje odmah započeti ako je vrijednost null. Mora biti s datumom i vremenom.
- **Baza podataka** – nije obavezno. Polje DatabaseBackupSettings za baze podataka za sigurnosno kopiranje.
- **KeepAtLeastOneBackup** - neobavezno promijenio parametar. Navedite ako jednu sigurnosnu kopiju uvijek će biti zadržane na računu za pohranu, bez obzira na to koliko je.

Slijedi primjer kako koristiti ovaj cmdlet.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Da biste trenutni raspored sigurnosnog kopiranja, koristite cmdlet Get-AzureRmWebAppBackupConfiguration. To može biti korisno za izmjenu raspored koji se već je konfiguriran.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Vraćanje web-aplikacijama iz sigurnosne kopije

Da biste vratili web-aplikacijama iz sigurnosne kopije, koristite cmdlet Vrati AzureRmWebAppBackup. Pomoću tog cmdleta najjednostavniji način je kanala sigurnosne kopije objekta dohvaćeni iz cmdlet Get-AzureRmWebAppBackup ili cmdlet Get-AzureRmWebAppBackupList.

Nakon što dodate sigurnosnu kopiju objekta, možete je pipe u cmdlet Vrati AzureRmWebAppBackup. Navedite parametar promjenu Prebriši da biste naznačili koju namjeravate prebrisati sadržaj web-aplikacije sadržaj sigurnosne kopije. Stvaranje sigurnosne kopije sadrži li baza podataka, kao i se vraćaju te baze podataka.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Slijedi primjer kako koristiti Vrati AzureRmWebAppBackup navođenjem sve parametre.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Brisanje sigurnosne kopije

Da biste izbrisali sigurnosnu kopiju, koristite cmdlet Ukloni AzureRmWebAppBackup. Time se sigurnosno kopiranje s računa za pohranu. Navedite naziv aplikacije, njegov grupa resursa i ID sigurnosne kopije koju želite izbrisati.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Sigurnosno kopiranje objekta možete pipe i u cmdlet Ukloni AzureRmWebAppBackup da biste je izbrisali.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
