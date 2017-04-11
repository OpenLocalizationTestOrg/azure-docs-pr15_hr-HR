<properties
   pageTitle="Početak rada s Azure obradu PowerShell | Microsoft Azure"
   description="Početak kratkog uvoda Azure PowerShell cmdleti možete koristiti za upravljanje servisom Azure grupe"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/20/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>Početak rada s cmdleta ljuske PowerShell za obradu Azure
Pomoću cmdleta ljuske PowerShell za obradu Azure možete izvesti i skripte mnoge zadatke izvršavanje s API-ji grupe, portala za Azure i sučelje naredbenog retka Azure (EŽA). Ovo je kratkog uvoda cmdleta možete koristiti za upravljanje računima grupe i rad s resursa grupe kao što su grupe, zadatke i zadatke.

Popis svih cmdleti za grupe i sintaksa detaljne cmdlet potražite u članku [Referenca cmdlet Azure grupe](https://msdn.microsoft.com/library/azure/mt125957.aspx).

U ovom se članku temelji se na cmdleta u Azure PowerShell verzije 3.0.0. Preporučujemo da ažurirate Azure PowerShell često da biste iskoristili servisnih ažuriranja i poboljšanja.

## <a name="prerequisites"></a>Preduvjeti

Izvođenje sljedeće postupke za upravljanje resursa u grupe pomoću komponente PowerShell Azure.

* [Instaliranje i konfiguriranje Azure PowerShell](../powershell-install-configure.md)

* Pokrenite cmdlet za **Prijavu AzureRmAccount** za povezivanje s pretplate (grupe za Azure cmdleti za dostavu u modulu Voditelj resursa za Azure):

    `Login-AzureRmAccount`

* **Registriranje kod davatelja naziva grupe**. Ovaj postupak samo potrebno izvesti **jedanput po pretplati**.

    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Upravljanje grupe za račune i ključevi

### <a name="create-a-batch-account"></a>Stvaranje grupe za račun

**Novo AzureRmBatchAccount** stvara račun za obradu u grupi navedenog resursa. Ako već nemate grupu resursa, stvorite ga tako da pokrenete cmdleta [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) . Navedite jednu od Azure regija u parametru **mjesto** , kao što su "Središnje sad". Ako, na primjer:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Zatim stvorite račun grupe u grupi resursa koji određuje naziv računa u <*naziv_računa*> i mjesto i naziv grupu resursa. Stvaranje računa grupe može potrajati neko vrijeme da biste dovršili. Ako, na primjer:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [AZURE.NOTE] Naziv računa za obradu mora biti jedinstven u području Azure za grupu resursa, sadrže između 3 i 24 znakova i koristiti isključivo mala slova i brojeva.

### <a name="get-account-access-keys"></a>Početak tipke za pristup računu
**Get-AzureRmBatchAccountKeys** prikazuje povezani s računom za obradu Azure pristupnih tipki. Na primjer, pokrenite sljedeće da biste dobili primarnih i sekundarnih ključeva računa koji ste stvorili.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Stvaranje novog ključa za access
**Novo AzureRmBatchAccountKey** stvara novi ključ primarnog ili sekundarni računa za račun za Azure grupe. Na primjer, da biste generirali nove primarni ključ za račun grupe, upišite:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [AZURE.NOTE] Da biste generirali novog ključa sekundarne, navedite "Sekundarne" za parametar **KeyType** . Morate Obnovi primarnih i sekundarnih ključeva zasebno.

### <a name="delete-a-batch-account"></a>Brisanje grupe za račun
**Uklanjanje AzureRmBatchAccount** Brisanje grupe za račun. Ako, na primjer:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Kada se to od vas zatraži, potvrdite da želite ukloniti račun. Uklanjanje računa može potrajati neko vrijeme da biste dovršili.

## <a name="create-a-batchaccountcontext-object"></a>Stvaranje BatchAccountContext objekta

Za provjeru autentičnosti pomoću cmdleta ljuske PowerShell za obradu kada stvaranje i upravljanje obradu grupe, zadatke, zadatke i ostale resurse, stvorite objekt BatchAccountContext za pohranu naziv računa i ključevi:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Objekt BatchAccountContext ulaze u cmdletima koji koristite parametar **BatchContext** .

> [AZURE.NOTE] Po zadanom koristi se s računom primarni ključ za provjeru autentičnosti, ali možete odabrati izričito ključ za korištenje promjenom svojstava **KeyInUse** BatchAccountContext objekt: `$context.KeyInUse = "Secondary"`.

## <a name="create-and-modify-batch-resources"></a>Stvaranje i izmjena grupe za resurse
Pomoću cmdleta kao što je **Novo AzureBatchPool**, **Nova AzureBatchJob**i **Novo AzureBatchTask** da biste stvorili resursa u odjeljku grupe za račun. Postoje odgovarajući **Get -** i **Postavljanje -** cmdleta za ažuriranje svojstava postojećih resursa i **Ukloni -** Cmdlete da biste uklonili resursa u odjeljku grupe za račun.

Prilikom korištenja mnoge od tih cmdleta, osim prosljeđivanje BatchContext objekt, morate stvoriti ili proslijedite objekte koji sadrže postavke detaljne resursa, kao što je prikazano u sljedećem primjeru. Pogledajte detaljnu pomoć za svaki cmdlet dodatni primjere.

### <a name="create-a-batch-pool"></a>Stvaranje grupe za grupu

Kada stvarate ili ažuriranje grupe za grupu, odaberite konfiguracija servisa za oblak ili konfiguracija virtualnog računala za operacijski sustav na čvorove računalnim (pogledajte [Pregled značajke grupe](batch-api-basics.md#pool)). Izbor određuje hoće li vaša čvorove računalnim su digitalno obrađen s jednim od [Azure goste OS izdavanje](../cloud-services/cloud-services-guestos-update-matrix.md#releases) ili s jednom podržani Linux ili Windows VM slike iz trgovine Windows Azure.

Kada pokrenete **Novi AzureBatchPool**, prenesite postavke operacijski sustav PSCloudServiceConfiguration ili PSVirtualMachineConfiguration objekta. Ako, na primjer, sljedeći cmdlet stvara novi skup grupe s veličine malih računalnim čvorove u Konfiguracija servisa oblaka digitalno obrađen najnoviju verziju operacijskog sustava grupe 3 (Windows Server 2012). Ovdje, **CloudServiceConfiguration** parametar određuje varijabla *$configuration* kao objekt PSCloudServiceConfiguration. Parametar **BatchContext** određuje prethodno definirane varijable *$context* kao objekt BatchAccountContext.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Formule autoscaling određuje cilj broj računalnim čvorove u novu grupu. U ovom slučaju, formula je jednostavno **$TargetDedicated = 4**, koji označava broj računalnim čvorove u je najviše 4.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Upit za grupe, zadatke, zadatke i druge detalje

Pomoću cmdleta **Get-AzureBatchPool**, **Get-AzureBatchJob**i **Get-AzureBatchTask** upit za entiteti stvoreni u odjeljku grupe za račun.

### <a name="query-for-data"></a>Upit za podatke

Na primjer, koristite **Get-AzureBatchPools** da biste pronašli svoje grupe. Prema zadanim postavkama ovaj upita za sve grupe na vašem računu, pod pretpostavkom da ste već spremljena BatchAccountContext objekta u *$context*:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Korištenje programa OData filtra

Možete navesti filtar za OData pomoću parametar **filtra** da biste pronašli samo objekte koji vas zanima. Na primjer, možete pronaći sve grupe pomoću ID-ova počevši od "myPool":

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Ta metoda nije fleksibilne, kao što su korištenje "Gdje objekt" u lokalnom kanal. Međutim, upit će poslati na servis za obradu izravno tako da sva filtriranja se događa na strani poslužitelja spremanja propusnosti internetske veze.

### <a name="use-the-id-parameter"></a>Koristite parametar ID-a

Alternative filtar za OData je koristiti parametar **ID-a** . Upit za određeni skup s ID-a "myPool":

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

Parametar **ID-a** podržava samo pretraživanja cijelog id, ne zamjenske znakove ili filtri OData stil.

### <a name="use-the-maxcount-parameter"></a>Koristite parametar MaxCount

Prema zadanim postavkama, svaki cmdlet vraća najviše 1000 objekte. Ako ne dođete do to ograničenje, suzite filtar da bi se prikazala natrag manje objekata ili eksplicitno postavljene najviše pomoću **MaxCount** parametra. Ako, na primjer:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Da biste uklonili gornja granica, postavite **MaxCount** 0 ili manje.

### <a name="use-the-powershell-pipeline"></a>Korištenje ljuske PowerShell kanal

Cmdleti za grupe omogućuje korištenje ljuske PowerShell kanal za slanje podataka između cmdleta. To ima isti učinak kao i određivanje parametar, ali olakšava rad s više entiteti.

Na primjer, traženje i prikaz svih zadataka na vašem računu:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Ponovno pokretanje (potrebno je ponovno) svaki računalnim čvor u zajedničko područje:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Upravljanje aplikacijama paketa

Pakete omogućuje pojednostavljeni implementacija aplikacije da biste čvorove računalnim u svoje grupe. Pomoću cmdleta ljuske PowerShell za obradu prijenos i upravljanje paketa aplikacije na vašem računu grupe, a Implementirajte verzije paketa za izračunavanje čvorove.

**Stvaranje** aplikacije:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Dodavanje** paketa aplikacije:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Postavili **zadanu verziju** aplikacije:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Popis** paketa aplikacije

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Brisanje** Aplikacijski paket

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Brisanje** aplikacije

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

>[AZURE.NOTE] Sve verzije paketa aplikacije za aplikacije sustava morate izbrisati prije brisanja aplikacije. Primit ćete "Sukoba" Pogreška ako pokušate izbrisati aplikaciju koja je trenutno paketa aplikacije.

### <a name="deploy-an-application-package"></a>Implementacija Aplikacijski paket

Možete navesti paketa aplikacije za implementaciju prilikom stvaranja zajedničko područje. Kada odredite paket vrijeme stvaranja grupe aplikacija, kao skup spojeva čvor je implementirana svaki čvor. Paketi i uvode se kada se čvor sustava ili reimaged.

Odredite na `-ApplicationPackageReference` mogućnosti prilikom stvaranja grupe aplikacija za implementaciju paketa aplikacije za čvorove na resurse kao što su uključiti u grupu. Najprije stvorite objekt **PSApplicationPackageReference** i konfiguriranje verzijom aplikacije ID-a i paket koji želite uvesti čvorove računalnim na resurse:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Sada stvorite na resurse i navedite objekt za pregled paketa kao argument za na `ApplicationPackageReferences` mogućnost:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

Dodatne informacije o paketa aplikacije možete pronaći u [Implementacija aplikacije s paketa aplikacije Azure grupe](batch-application-packages.md).

>[AZURE.IMPORTANT] Na račun za obradu da biste koristili aplikaciju paketa morate [vezu račun za Azure prostora za pohranu](#linked-storage-account-autostorage) .

### <a name="update-a-pools-application-packages"></a>Ažuriranje paketa aplikacije u grupu

Da biste ažurirali aplikacije dodijeljene postojeću grupu, najprije stvoriti PSApplicationPackageReference objekta sa željenim svojstvima (Id i paket verzija programa):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Nakon toga dobiti na resurse iz serije, izbrisati sve postojeće pakete, dodavanje naš novi paket reference i ažurirajte servis za obradu nove postavke grupe aplikacija:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Sada ste ažurirali svojstva na resurse u servisu grupe. Zapravo uvođenje novog paketa aplikacije za izračunavanje čvorove u, međutim, morate ponovno pokrenuti ili reimage te čvorove. Ponovno pokrenuti svaki čvor u grupu s sljedeću naredbu:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

>[AZURE.TIP] Možete implementirati više paketa aplikacije za čvorove računalnim u zajedničko područje. Ako želite *dodati* Aplikacijski paket umjesto zamjene trenutno distribuiranih paketa, preskočite na `$pool.ApplicationPackageReferences.Clear()` redak iznad.

## <a name="next-steps"></a>Daljnji koraci

* Detaljne cmdlet sintaksa i primjeri, potražite u članku [grupe za Azure cmdlet referenca](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Dodatne informacije o aplikacijama i paketa aplikacije u seriji potražite u članku [Implementacija aplikacije s paketa aplikacije Azure grupe](batch-application-packages.md).
