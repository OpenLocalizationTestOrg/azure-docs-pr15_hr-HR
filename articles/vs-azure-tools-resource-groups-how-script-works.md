<properties
    pageTitle="Pregled grupa resursa Azure projekta implementacijsku skriptu | Microsoft Azure"
    description="U članku se opisuje kako funkcionira skriptu PowerShell u programu project za implementaciju Azure grupa resursa."
    services="visual-studio-online"
    documentationCenter="na"
    authors="tfitzmac"
    manager="timlt"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/26/2016"
    ms.author="tomfitz" />

# <a name="overview-of-the-azure-resource-group-project-deployment-script"></a>Pregled implementacijsku skriptu projekta Azure grupa resursa

Grupa resursa Azure implementacije projekata pomoći faze i implementacija datoteke i druge artefakte Azure. Kada stvorite projekta za implementaciju programa Azure upravljanja resursima u Visual Studio, skriptu PowerShell naziva **Uvođenja AzureResourceGroup.ps1** dodaje se projekt. Ova tema sadrži detalje o čemu služi Ova skripta i kako izvršiti unutar i izvan nje omogućiti Visual Studio.

## <a name="what-does-the-script-do"></a>Čemu služi skripte?

Uvođenje AzureResourceGroup.ps1 skripte ne dvije stvari koje su vam važne tijeka rada za implementaciju.

- Prijenos datoteka ni artefakte koja su potrebna za uvođenje predloška
- Uvođenje predloška

Prvi dio skripte prenese datoteka i artefakte radi implementacije i zadnji cmdlet u skripti zapravo uvođenje predloška. Na primjer, ako virtualnog računala nije potrebno je konfigurirati s skriptu, implementacijsku skriptu najprije sigurno prenosi skripte za konfiguraciju računa Azure prostora za pohranu. To postaje dostupan za Azure Voditelj resursa za konfiguriranje virtualnog računala prilikom dodjele resursa.

Jer je sve predložak implementacijama potrebna dodatna artefakte koje je potrebno prenijeti, vrednuje se parametar promjenu naziva *uploadArtifacts* . Ako sve artefakte potreban je moguće prenijeti, postavite parametar *uploadArtifacts* prilikom pozivanja skriptu. Imajte na umu da datoteke glavni predloška i parametara datoteku ne morate je moguće prenijeti. Samo druge datoteke, kao što su skripte konfiguracije ugniježđene predlošci implementaciju, a potrebno je moguće prenijeti datoteke aplikacije.

## <a name="detailed-script-description"></a>Opis detaljne skripte

Slijedi opis koje dijelove odaberite Nemoj skripte uvođenja AzureResourceGroup.ps1 Azure PowerShell.

>[AZURE.NOTE] To opisuje verzije 1.0 skripte uvođenja AzureResourceGroup.ps1.

1.  Deklariranje parametara potrebne Voditelj resursa Azure implementacije projekta. Neke parametre sadrže zadane vrijednosti koja su postavljena prilikom stvaranja projekta. Možete promijeniti te zadane vrijednosti u skripti i dodavanje vrijednosti parametara različite prije izvršavanje skriptu.

    ```
    Param(
      [string] [Parameter(Mandatory=$true)] $ResourceGroupLocation,
      [string] $ResourceGroupName = 'AzureResourceGroup1',
      [switch] $UploadArtifacts,
      [string] $StorageAccountName,
      [string] $StorageAccountResourceGroupName,
      [string] $StorageContainerName = $ResourceGroupName.ToLowerInvariant() + '-stageartifacts',
      [string] $TemplateFile = '..\Templates\azuredeploy.json',
      [string] $TemplateParametersFile = '..\Templates\azuredeploy.parameters.json',
      [string] $ArtifactStagingDirectory = '..\bin\Debug\staging',
      [string] $AzCopyPath = '..\Tools\AzCopy.exe',
      [string] $DSCSourceFolder = '..\DSC'
    )
    ```

  	|Parametar|Opis|
  	|---|---|
  	|$ResourceGroupLocation|Regija ili podataka centar mjesto za grupu resursa, kao što su **Zapad SAD -a** ili **istočnoazijski**.|
  	|$ResourceGroupName|Naziv grupe Azure resursa.|
  	|$UploadArtifacts|Binarni vrijednost koja označava je li artefakte nije potrebno je moguće prenijeti Azure iz sustava.|
  	|$StorageAccountName|Naziv računa Azure prostora za pohranu gdje se prenijeti na artefakte.|
  	|$StorageAccountResourceGroupName|Naziv grupe Azure resursa koja sadrži račun za pohranu.|
  	|$StorageContainerName|Naziv spremnika prostora za pohranu koji se koristi za prijenos artefakte.|
  	|$TemplateFile|Put do datoteke implementacije (`<app name>.json`) u projektu Azure grupu resursa.|
  	|$TemplateParametersFile|Put do datoteke parametara (`<app name>.parameters.json`) u projektu Azure grupu resursa.|
  	|$ArtifactStagingDirectory|Put na računalu na kojem artefakte lokalno prenose, uključujući korijenske mape skriptu PowerShell. Ovaj put može biti apsolutne ili odnosu skripte mjesto.|
  	|$AzCopyPath|Put gdje alat za AzCopy.exe kopira njegov .zip datoteke, uključujući korijenske mape skriptu PowerShell. Ovaj put može biti apsolutne ili odnosu skripte mjesto.|
  	|$DSCSourceFolder|Put do mape DSC (želji stanje konfiguracije) izvora, uključujući korijenske mape skriptu PowerShell. Ovaj put može biti apsolutne ili odnosu skripte mjesto. Potražite u članku [Uvod u proširenje Azure PowerShell DSC (želji stanje konfiguracije)](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx), ako je potrebno, dodatne informacije.|

1.  Provjerite je li artefakte nije potrebno je moguće prenijeti Azure. Ako nije, preskočite do koraka 11. U suprotnom poduzeti sljedeće korake.

1.  Pretvoriti sve varijable s relativni putovi apsolutnim putovima. Na primjer, kao što su promjena puta `..\Tools\AzCopy.exe` da biste `C:\YourFolder\Tools\AzCopy.exe`. Osim toga, pokretanje varijable *ArtifactsLocationName* i *ArtifactsLocationSasTokenName* na null. *ArtifactsLocation* i *SaSToken* možda parametara u predlošku. Ako su njihove vrijednosti null nakon čitanja u datoteci s parametrima, skripta generira vrijednosti za njih.

    Alati za Azure vrijednosti parametra *_artifactsLocation* i *_artifactsLocationSasToken* u predlošku upravljajte pomoću artefakte. Ako skriptu PowerShell pronalazi parametara s nazive, ali ne postoje vrijednosti parametra, skripta prenosi u artefakte i vraća odgovarajuće vrijednosti za te parametre. On zatim prosljeđuje ih cmdlet putem `@OptionsParameters`.

  	|Varijabla|Opis|
  	|---|---|
  	|ArtifactsLocationName|Put do gdje se nalaze Azure artefakte.|
  	|ArtifactsLocationSasTokenName|SAS (zajednički pristup potpisa) tokena naziv koji koriste skripta za provjeru autentičnosti Bus servisa. Dodatne informacije potražite u članku [Zajedničko korištenje programa Access potpis Authentication s Bus servisa](service-bus-shared-access-signature-authentication.md) .|

    ```
    if ($UploadArtifacts) {
    # Convert relative paths to absolute paths if needed
    $AzCopyPath = [System.IO.Path]::Combine($PSScriptRoot, $AzCopyPath)
    $ArtifactStagingDirectory = [System.IO.Path]::Combine($PSScriptRoot, $ArtifactStagingDirectory)
    $DSCSourceFolder = [System.IO.Path]::Combine($PSScriptRoot, $DSCSourceFolder)

    Set-Variable ArtifactsLocationName '_artifactsLocation' -Option ReadOnly
    Set-Variable ArtifactsLocationSasTokenName '_artifactsLocationSasToken' -Option ReadOnly

    $OptionalParameters.Add($ArtifactsLocationName, $null)
    $OptionalParameters.Add($ArtifactsLocationSasTokenName, $null)
    ```

1.  Ovaj odjeljak provjerava je li na <app name>. parameters.json datoteka (naziva se "datoteka parametara") ima nadređeni čvor naziva **parametrima** (u na `else` bloka). U suprotnom je nema nadređeni čvor. Neki oblik nije prihvatljiva.
    
    ```
    if ($JsonParameters -eq $null) {
            $JsonParameters = $JsonContent
        }
        else {
            $JsonParameters = $JsonContent.parameters
        }
    ```

1.  Iteracija putem zbirke JSON parametara. Ako je vrijednost parametra dodijeljeno *_artifactsLocation* ili *_artifactsLocationSasToken*, postavite varijable *$OptionalParameters* s te se vrijednosti. To sprječava skriptu slučajno prebrisivanje sve vrijednosti parametra pružate.

    ```
    $JsonParameters | Get-Member -Type NoteProperty | ForEach-Object {
        $ParameterValue = $JsonParameters | Select-Object -ExpandProperty $_.Name

        if ($_.Name -eq $ArtifactsLocationName -or $_.Name -eq $ArtifactsLocationSasTokenName) {
            $OptionalParameters[$_.Name] = $ParameterValue.value
        }
    }
    ```

1.  Pronađite ključ za račun za pohranu i kontekst resursa za pohranu račun za držite artefakte radi implementacije.

    ```
    $StorageAccountKey = (Get-AzureRMStorageAccountKey -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Key1

    $StorageAccountContext = (Get-AzureRmStorageAccount -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Context
    ```

1.  Ako koristite PowerShell DSC da biste konfigurirali virtualnog računala, proširenje DSC zahtijeva artefakte u jednom zip datoteku. Tako, stvorite .zip arhivska datoteka za konfiguraciju DSC. Da biste to učinili, provjerite postoji li $DSCSourceFolder. Ako postoji DSC konfiguracije, uklonite ga, a zatim stvorite novu komprimiranu datoteku pod nazivom dsc.zip.

    ```
    # Create DSC configuration archive
    if (Test-Path $DSCSourceFolder) {
    Add-Type -Assembly System.IO.Compression.FileSystem
        $ArchiveFile = Join-Path $ArtifactStagingDirectory "dsc.zip"
        Remove-Item -Path $ArchiveFile -ErrorAction SilentlyContinue
        [System.IO.Compression.ZipFile]::CreateFromDirectory($DSCSourceFolder, $ArchiveFile)
    }
    ```

1.  Ako nema put za Azure artefakte nije naveden u datoteci s parametrima, postavite put potražite skriptu PowerShell za korištenje prilikom prijenosa artefakte. Da biste to učinili, stvorite put pomoću kombinacije računa za pohranu krajnjoj točki put plus naziv spremnika za pohranu. Nakon toga ažurirati datoteku parametara s ovom novi put.

    ```
    # Generate the value for artifacts location if it is not provided in the parameter file
    $ArtifactsLocation = $OptionalParameters[$ArtifactsLocationName]
    if ($ArtifactsLocation -eq $null) {
        $ArtifactsLocation = $StorageAccountContext.BlobEndPoint + $StorageContainerName
        $OptionalParameters[$ArtifactsLocationName] = $ArtifactsLocation
    }
    ```

1.  Pomoću uslužni **AzCopy** (uvršteni su u mapi **Alati** implementacije projekta Azure grupa resursa) kopirajte sve datoteke iz lokalni put za pohranu padajuće online račun za Azure prostora za pohranu. Ovaj korak ne uspije, zatvorite skriptu jer implementacijskih vjerojatno neće uspjeti bez potrebne artefakte.

    ```
    # Use AzCopy to copy files from the local storage drop path to the storage account container
    & $AzCopyPath """$ArtifactStagingDirectory""", $ArtifactsLocation, "/DestKey:$StorageAccountKey", "/S", "/Y", "/Z:$env:LocalAppData\Microsoft\Azure\AzCopy\$ResourceGroupName"
    if ($LASTEXITCODE -ne 0) { return }
    ```

1.  Ako je token za SAS lokacije artefakte nije navedena u datoteci s parametrima stvoriti za privremene samo za čitanje pristup mreži spremnik za pohranu. Nakon toga prosljeđivanje tog SAS tokena na na cmdline kao programa "optionalParameter." Imajte na umu da sve parametre proslijeđen u cmdline će prednost pred vrijednosti navedene u datoteci parametara.

    ```
    # Generate the value for artifacts location SAS token if it is not provided in the parameter file
    $ArtifactsLocationSasToken = $OptionalParameters[$ArtifactsLocationSasTokenName]
    if ($ArtifactsLocationSasToken -eq $null) {
       # Create a SAS token for the storage container - this gives temporary read-only access to the container
       $ArtifactsLocationSasToken = New-AzureStorageContainerSASToken -Container $StorageContainerName -Context $StorageAccountContext -Permission r -ExpiryTime (Get-Date).AddHours(4)
       $ArtifactsLocationSasToken = ConvertTo-SecureString $ArtifactsLocationSasToken -AsPlainText -Force
       $OptionalParameters[$ArtifactsLocationSasTokenName] = $ArtifactsLocationSasToken
    }
    ```

1.  Ako još ne postoji i provjerite datoteku predloška i parametara za sve pogreške provjere valjanosti koja će spriječiti implementacije uspješnu, stvorite grupu resursa.

    ```
    # Create or update the resource group using the specified template file and template parameters file
    New-AzureRMResourceGroup -Name $ResourceGroupName -Location $ResourceGroupLocation -Verbose -Force -ErrorAction Stop

    Test-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterFile $TemplateParametersFile @OptionalParameters -ErrorAction Stop
    ```

1. Na kraju, uvođenje predloška. Kod stvara jedinstveni naziv za implementaciju pomoću vremenske oznake.

    ```
    New-AzureRMResourceGroupDeployment -Name ((Get-ChildItem $TemplateFile).BaseName + '-' + ((Get-Date).ToUniversalTime()).ToString('MMdd-HHmm')) `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterFile $TemplateParametersFile `
        @OptionalParameters `
        -Force -Verbose
    ```

## <a name="deploy-the-resource-group"></a>Implementacija grupa resursa

### <a name="to-deploy-the-resource-group-in-visual-studio"></a>Za implementaciju u grupu resursa u Visual Studio

1. Na izborniku prečacu projekta grupa resursa Azure odaberite **uvođenja** > **Novu implementaciju**.

    ![][0]

1. U dijaloškom okviru **uvođenja grupu resursa** ili odabrati postojeću grupu resursa u okvir padajućeg popisa za implementaciju ili odaberite ** &lt;Stvori novu... &gt;** Da biste stvorili novu grupu resursa.

    ![][1]

1. Ako se to od vas zatraži, unesite naziv grupe resursa i lokaciju u dijaloškom okviru **Stvaranje grupa resursa** , a zatim odaberite gumb **Stvori** .

    ![][2]

1. Odaberite gumb **Uređivanje parametara** da biste prikazali dijaloški okvir **Uređivanje parametar** , a zatim unesite bilo koji nedostaju vrijednosti parametara.

    ![][3]

    >[AZURE.NOTE] Ako zahtijeva parametre potrebna vrijednosti, u ovom dijaloškom okviru automatski se pojavljuje kada pokrenete.

    ![][4]

1. Kada završite unos vrijednosti parametra, odaberite gumb **Spremi** , a zatim gumb **uvođenja** .

    Pokreće implementacijsku skriptu (uvođenja AzureResourceGroup.ps1), a predložak, uz sve artefakte uvodi za Azure.

### <a name="to-deploy-the-resource-group-by-using-powershell"></a>Za implementaciju u grupu resursa pomoću komponente PowerShell

Ako želite pokrenuti skriptu bez korištenja Visual Studio implementacija naredbu i korisničko Sučelje na izborničkom prečacu skripte, odaberite **Otvori pomoću Očisti filtar**.

![][5]


## <a name="command-deployment-examples"></a>Naredba implementacija Primjeri

### <a name="deploy-using-default-values"></a>Implementacija pomoću zadanih vrijednosti

U ovom se primjeru pokazuje kako pokrenuti skriptu pomoću zadanih vrijednosti parametra. (Zato što parametar mjesto nema zadanu vrijednost, morate unijeti jednu.)

`.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus`

### <a name="deploy-overriding-the-default-values"></a>Implementacija nadjačavanje zadane vrijednosti

U ovom se primjeru pokazuje kako pokrenuti skriptu uvođenje predloška i parametre datoteke koje se razlikuju od zadane vrijednosti.

```
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus –TemplateFile ..\templates\AnotherTemplate.json –TemplateParametersFile ..\templates\AnotherTemplate.parameters.json
```

### <a name="deploy-using-uploadartifacts-for-staging"></a>Implementacija pomoću UploadArtifacts za pripremna

U ovom se primjeru pokazuje kako pokrenuti skriptu da biste prenijeli artefakte iz mape izdanje i implementirati predloške koji nisu zadani.

```
.\Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory ..\bin\release\staging
```

U ovom se primjeru pokazuje kako pokrenuti skriptu u zadatku Azure PowerShell u Visual Studio Online.

```
$(Build.StagingDirectory)/AzureResourceGroup1/Scripts/Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory $(Build.StagingDirectory)
```

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o Azure Voditelj resursa tako da pročitate [Pregled Azure Voditelj resursa](azure-resource-manager/resource-group-overview.md).

Više primjera o radu s Azure resursa grupne projekte potražite u članku [uvođenje i upravljanje resursima Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Deploy-and-manage-Azure-resources) iz povezivanje [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [pokazni videozapis](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Dodatne početak rada s HealthClinic.biz videozapis, potražite u članku [Početak rada za alate za razvojne inženjere za Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

[0]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy1c.png
[1]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy2bc.png
[2]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy3bc.png
[3]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy4bc.png
[4]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy5c.png
[5]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy6c.png
