<properties
   pageTitle="Implementacija resursi PowerShell i predložak | Microsoft Azure"
   description="Koristite upravitelj resursa Azure i Azure PowerShell resurs za Azure. Resursi su definirani u predlošku Voditelj resursa."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Implementacija resursa s resursima predloške i Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure EŽA](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST API-JA](resource-group-template-deploy-rest.md)

U ovoj se temi objašnjava kako pomoću komponente PowerShell Azure s predlošcima Voditelj resursa za implementaciju resurse za Azure.  

> [AZURE.TIP] Pomoć za ispravljanje pogrešaka pogreška tijekom uvođenja, pogledajte:
>
> - Otklanjanje poteškoća s [postupci implementacije prikaz s Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md) dodatne informacije o tome kako izvući podatke koji olakšava pogrešku
> - [Otklanjanje uobičajenih pogrešaka prilikom implementacije resurse za Azure s Azure Voditelj resursa](resource-manager-common-deployment-errors.md) da biste saznali kako riješiti uobičajenih pogrešaka implementacije

Predložak može biti lokalne datoteke ili vanjske datoteke koji je dostupan putem URI. Kada se predložak nalazi se u račun za pohranu, možete ograničiti pristup predloška i dati token za potpis (SAS) zajednički pristup tijekom implementacije.

## <a name="quick-steps-to-deployment"></a>Brzi koraci za implementaciju

U ovom se članku opisuju sve različite mogućnosti dostupne tijekom implementacije. Međutim, često potrebno samo dvije jednostavne naredbe. Da biste brzo početak rada s implementaciju, koristite sljedeće naredbe:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Da biste saznali više o mogućnostima implementacije koje možda bolje prilagođene za vašu situaciju, nastavite čitati u ovom se članku.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-powershell"></a>Implementacija sa servisom PowerShell

1. Prijavite se na račun za Azure.

        Add-AzureRmAccount

     Vraća se sažetak računa.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Ako imate više pretplata, unesite ID pretplate koju želite koristiti za implementaciju s naredbom **Skup AzureRmContext** . 

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. Obično pri implementaciji novi predložak želite stvoriti grupu resursa sadrži resurse. Ako imate postojeću grupu resursa koje želite uvesti, možete preskočiti ovaj korak i koristiti tu grupu resursa. 

     Da biste stvorili grupu resursa, navedite naziv i mjesto za grupu resursa. Morate upišete mjesto za grupu resursa jer grupa resursa pohranjuje metapodatke o resurse. Usklađenost razloga preporučujemo vam da biste odredili gdje je pohranjena te metapodataka. Općenito govoreći, preporučujemo da navedete na mjesto na kojem će se nalaziti Većina resurse. Korištenje na istom mjestu može pojednostavniti svoj predložak.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
     Vraća se sažetak novu grupu resursa.
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
             Actions  NotActions
             =======  ==========
             *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

4. Prije izvođenja implementaciju sustava, možete provjeriti valjanost postavki uvođenja. Cmdlet **Test AzureRmResourceGroupDeployment** omogućuje vam da biste pronašli probleme prije stvaranja stvarni resursi. Sljedeći primjer prikazuje način da biste provjerili valjanost implementacije.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Da biste implementirali resursa u grupu resursa, pokrenite naredbu **Novo AzureRmResourceGroupDeployment** i navedite potrebne parametre. Parametri obuhvaćaju naziv za implementaciju sustava, naziv grupu resursa, put ili URL-a za predložak koji ste stvorili i sve parametre potrebno za vašu situaciju. Ako parametar **načina rada** nije naveden, koristi se zadana vrijednost **rastuća** . Da biste pokrenuli dovršeno implementaciju, postavite **način rada** u **Dovršeno**. Budite oprezni prilikom korištenja dovršeno načina kako ste slučajno izbrisali resursa koji se ne nalaze u vašem predlošku.

     Da biste implementirali lokalne predložak, koristite parametar **TemplateFile** :

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Da biste implementirali vanjskih predložak, koristite parametar **TemplateUri** :

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     Imate sljedeće mogućnosti za dohvat vrijednosti parametara: 
   
     1. Korištenje parametara u istoj razini.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. Koristite parametar objekt.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Korištenje lokalnog parametra datoteke. Informacije o datoteku predloška potražite u članku [parametar datoteke](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Korištenje vanjskih parametar datoteke. Informacije o datoteku predloška potražite u članku [parametar datoteke](#parameter-file). 

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Kada koristite datotečni vanjskih parametar, ne možete prenijeti druge vrijednosti ili retku ili iz lokalne datoteke. Dodatne informacije potražite u članku [Prednost parametar](#parameter-precendence).

     Nakon što ste uveden u člancima, vidjet ćete sažetak implementacije.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Ako predložak sadrži parametar s istim nazivom kao jedan od parametara u naredbu komponente PowerShell, morat ćete navesti vrijednost za taj parametar. Parametar u predlošku neće sadržavati stavku postfix **FromTemplate**. Na primjer, parametar pod nazivom **ResourceGroupName** u predložak u sukobu s parametrom **ResourceGroupName** u cmdleta [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) . Morat ćete navesti vrijednost za **ResourceGroupNameFromTemplate**. Općenito govoreći, izbjegavajte ovaj zbrku ne imenovanje parametara s istim nazivom kao parametar koji se koristi za implementaciju operacije.

6. Ako želite da biste se prijavili na dodatne informacije o implementaciji koji mogu olakšati otklanjanje pogrešaka implementaciju, koristite parametar **DeploymentDebugLogLevel** . Možete navesti biti prijavljeni pomoću postupka implementacije sadržaja zahtjev, odgovor sadržaja ili oboje.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     Dodatne informacije o korištenju pogrešaka sadržaja za otklanjanje poteškoća s implementacijama potražite u članku [implementacijama za grupu resursa otklanjanje poteškoća sa servisom PowerShell Azure](resource-manager-troubleshoot-deployments-powershell.md).

## <a name="deploy-template-from-storage-with-sas-token"></a>Uvođenje predloška iz spremišta s SAS tokena

Predloške možete dodati račun za pohranu i veze na njih tijekom implementacije s SAS token.

> [AZURE.IMPORTANT] Slijedeći korake u nastavku blob koja sadrži predložak je dostupan samo vlasnik računa. Kada stvorite SAS tokena za blob-om, blob-om je dostupan svakome s tom URI. Ako neki drugi korisnik intercepts URI, koji je mogu pristupiti predložak. Korištenje SAS token izvrstan je način od ograničavanje pristupa predloške, ali ne smiju povjerljive podatke kao što su lozinke izravno u predlošku.

### <a name="add-private-template-to-storage-account"></a>Dodavanje predloška privatni račun za pohranu

Postavljanje računa za pohranu za predloške sljedeće korake:

1. Stvorite grupu resursa.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Stvaranje računa za pohranu. Naziv računa spremišta mora biti jedinstvena preko Azure, pa pošaljite vlastiti naziv računa.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. Postavljanje računa za trenutni prostor za pohranu.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Stvaranje spremnika. Dozvole postavljen je na **Isključeno** što znači spremnik je dostupan samo vlasnik.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. Dodavanje predloška u spremnik.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Navedite SAS token tijekom implementacije

Da biste implementirali privatne predloška u račun za pohranu, dohvaćanje SAS token i uključiti u URI za predložak.

1. Ako ste promijenili trenutni račun za pohranu, postavite trenutni račun za pohranu na onu koja sadrži predloške.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Stvaranje tokena SAS s dozvolama za čitanje i neko vrijeme isteka ograničiti pristup. Dohvaćanje cijelog URI predloška uključujući SAS token.

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. Uvođenje predloška unosom URI koji uključuje SAS token.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Primjer SAS token pomoću povezane predložaka, potražite u članku [Korištenje predložaka povezane s Azure Voditelj resursa](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="parameter-precedence"></a>Prednost parametra

Možete koristiti web-mjesto u istoj razini parametara i u okvir za lokalni parametar datoteke u isti postupak implementacije. Na primjer, možete odrediti neke vrijednosti u datoteci parametara lokalno i dodati druge vrijednosti u istoj razini tijekom implementacije. Ako unesete vrijednosti parametra u datoteku parametara lokalno i u istoj razini, vrijednost u istoj razini prednost.

Međutim, ne možete koristiti parametara u ravnini s datotekom vanjskih parametar. Kada navedete parametar datoteke u parametru **TemplateParameterUri** , zanemaruju se sve parametre u istoj razini. Morate navesti sve vrijednosti parametra u vanjskoj datoteci. Ako predložak sadrži povjerljive vrijednost koja nije moguće dodati u datoteci parametar, dodajte tu vrijednost za ključne sigurnog i referencu ključa sigurnog u datoteci vanjskih parametar ili dinamički sadrže sve parametar vrijednosti u istoj razini.

Pojedinosti o korištenju KeyVault referenca za prosljeđivanje sigurne vrijednosti, potražite u članku [proći sigurne vrijednosti tijekom implementacije](resource-manager-keyvault-parameter.md).

## <a name="next-steps"></a>Daljnji koraci
- Primjer implementacija resursi putem klijenta biblioteke .NET, potražite u članku [uvođenja resursima pomoću .NET biblioteke i predložak](virtual-machines/virtual-machines-windows-csharp-template.md).
- Definiranje parametara u predložak, potražite u odjeljku [Predlošci na dokumentima](resource-group-authoring-templates.md#parameters).
- Upute za implementaciju rješenje u različitim okruženjima, potražite u članku [razvoj i testiranje okruženja u Microsoft Azure](solution-dev-test-environments.md).

