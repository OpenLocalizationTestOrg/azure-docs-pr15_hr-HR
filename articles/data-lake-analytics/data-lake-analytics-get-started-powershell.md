<properties 
   pageTitle="Početak rada s Azure podataka Lake analize pomoću komponente PowerShell Azure | Azure" 
   description="Saznajte kako pomoću ljuske PowerShell Azure stvaranje računa spremišta Lake podataka, stvorite posao analize Lake podataka koji se koristi U SQL i slanje posao. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/21/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Praktični vodič: početak rada s Azure podataka Lake analize pomoću komponente PowerShell Azure

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Saznajte kako pomoću ljuske PowerShell Azure stvoriti račune Lake analize podataka za Azure, definirati analize podataka Lake zadacima [U SQL](data-lake-analytics-u-sql-get-started.md)i slanje zadacima podataka Lake analitički račune. Dodatne informacije o analize Lake podataka potražite u članku [Pregled Azure podataka Lake analize](data-lake-analytics-overview.md).

U ovom ćete praktičnom vodiču će razviti zadatak koji se čita karticu datoteku (TSV) vrijednosti odvojenih i pretvara ga u datoteku razdvojene zarezom (CSV). Da biste došli do isti praktičnom vodiču pomoću drugih alata za podržane, klikom na kartice pri vrhu stranice u ovom se odjeljku.

##<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
- **Radne stanice s Azure PowerShell**. Saznajte [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).
    
##<a name="create-data-lake-analytics-account"></a>Stvaranje računa analize podataka Lake

Morate imati račun analize podataka Lake prije pokretanja sve zadatke. Da biste stvorili analize podataka Lake račun, morate navesti sljedeće:

- **Grupa resursa Azure**: A podataka Lake analize računa moraju se stvoriti u skupini Azure resursa. [Voditelj resursa Azure](../azure-resource-manager/resource-group-overview.md) omogućuje rad s resursima u aplikaciji kao grupu. Možete implementirati, ažuriranje i brisanje svi resursi aplikacije u jednom, usklađenih postupak.  

    Da biste numeriranje grupa resursa u svoju pretplatu:
    
        Get-AzureRmResourceGroup
    
    Da biste stvorili novu grupu resursa:

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Naziv računa analize podataka Lake**
- **Lokacija**: jedan od središta Azure podataka koje podržava analize podataka Lake.
- **Zadani podataka Lake računa**: svaki račun analize podataka Lake ima zadani račun Lake podataka.

    Da biste stvorili novi račun Lake podataka:

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] Naziv računa Lake podataka mora sadržavati samo mala slova i brojeva.



**Da biste stvorili analize podataka Lake račun**

1. Otvaranje Očisti filtar iz vaše radne stanice sustava Windows.
2. Pokrenite sljedeću skriptu:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"
        
        Write-Host "Create a resource group ..." -ForegroundColor Green
        New-AzureRmResourceGroup `
            -Name  $resourceGroupName `
            -Location $location
        
        Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeStoreName `
            -Location $location 
        
        Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
        New-AzureRmDataLakeAnalyticsAccount `
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName
        
        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

##<a name="upload-data-to-data-lake"></a>Prijenos podataka Lake podataka

U ovom ćete praktičnom vodiču obradit će nekim zapisnicima pretraživanja.  U zapisniku pretraživanja može se spremiti u spremište podataka Lake ili spremište blobova platforme Azure. 

Ogledna datoteka zapisnika pretraživanja kopirani javno spremnik blobova platforme Azure. Koristite sljedeću skriptu komponente PowerShell da biste datoteku preuzeli na vaše radne stanice, a zatim Prenesite datoteku na zadani spremišta podataka Lake račun vašeg računa analize podataka Lake.

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

Sljedeću skriptu komponente PowerShell pokazuje kako zadani naziv spremišta podataka Lake analize podataka Lake računa:


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

>[AZURE.NOTE] Portal za Azure nudi korisničko sučelje da biste kopirali oglednim podatkovnim datotekama zadanog računa za spremište Lake podataka. Upute potražite u članku [Početak rada s analize Lake podataka za Azure pomoću portala za Azure](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account).

Spremište blobova platforme Azure možete pristupiti i analize podataka Lake.  Prijenos podataka u spremište blobova platforme Azure, potražite u članku [Korištenje Azure PowerShell s Azure prostora za pohranu](../storage/storage-powershell-guide-full.md).

##<a name="submit-data-lake-analytics-jobs"></a>Slanje podataka Lake Analitika poslova

Poslovi analize podataka Lake zapisuju U SQL jeziku. Dodatne informacije o U SQL potražite u članku [Uvod U SQL jezika](data-lake-analytics-u-sql-get-started.md) i [U SQL jezične preporuke](http://go.microsoft.com/fwlink/?LinkId=691348).

**Da biste stvorili skriptu analize podataka Lake posla**

- Stvaranje tekstne datoteke pomoću sljedeće skripte U SQL i spremite tekstnu datoteku vaše radne stanice:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Ova skripta U SQL čita izvornu datoteku podataka pomoću **Extractors.Tsv()**i stvara u csv datoteku pomoću **Outputters.Csv()**. 
    
    Nemojte mijenjati dva puta osim ako kopirate izvornu datoteku na drugo mjesto.  Ako ne postoji analize podataka Lake će stvoriti Izlazna datoteka.
    
    To je jednostavnije koristiti relativni putovi za datoteke spremljene u zadanom podataka Lake računi. Možete koristiti i apsolutni putovi.  Na primjer 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Koristite apsolutne putova da biste pristupili datotekama u povezane poslovne subjekte prostora za pohranu.  Vidjet ćete da sintaksa za datoteke spremljene u povezani poslovni subjekt za pohranu Azure je:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob kontejner s javnim blob-ova ili dozvole za pristup javnim spremnika trenutno nisu podržani.    
    
    
**Da biste poslali posao**

1. Otvaranje Očisti filtar iz vaše radne stanice sustava Windows.
2. Pokrenite sljedeću skriptu:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
        
        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    Datoteka skripte U SQL u skripti je pohranjena na c:\tutorials\data-lake-analytics\copyFile.usql. Ažurirajte put datoteke sukladno tome.
 
Nakon završetka posla, koristite sljedeće Cmdlete da biste datoteke i preuzimanje datoteke:
    
    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Vidi također

- Da biste vidjeli iste praktičnom vodiču pomoću drugih alata, kliknite karticu Birači pri vrhu stranice.
- Da biste vidjeli složeniji upit, potražite u članku [zapisnika analiza web-mjesta pomoću Azure podataka Lake analize](data-lake-analytics-analyze-weblogs.md).
- Prvi koraci u razvoju aplikacija U SQL, potražite u članku [razviti U – SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Da biste saznali U SQL, potražite u članku [Početak rada s jezikom Azure podataka Lake analize U-SQL](data-lake-analytics-u-sql-get-started.md).
- Upravljanje zadacima, potražite u članku [Upravljanje Azure podataka Lake analize pomoću portala za Azure](data-lake-analytics-manage-use-portal.md).
- Da biste dobili pregled analize podataka Lake, potražite u članku [Pregled Azure podataka Lake analize](data-lake-analytics-overview.md).

