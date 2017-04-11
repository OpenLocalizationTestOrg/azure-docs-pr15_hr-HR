<properties
    pageTitle="Implementacija web-aplikacijama MSDeploy pomoću naziv glavnog računala i ssl certifikat"
    description="Implementacija web-aplikacijama pomoću MSDeploy i postaviti prilagođeni naziv glavnog računala i SSL certifikat pomoću predloška Azure Voditelj resursa"
    services="app-service\web"
    manager="erikre"
    documentationCenter=""
    authors="jodehavi"
    />

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="john.dehavilland"/>

# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Implementacija web app s MSDeploy, prilagođeni naziv glavnog računala i SSL certifikat

Ovaj vodič vodi kroz stvaranje implementacije sustava Završi do kraja za web-aplikaciju programa Azure, korištenje MSDeploy kao i dodavanje prilagođeni naziv glavnog računala i SSL certifikat na ARM predložak.

Dodatne informacije o stvaranju predložaka potražite u članku [Za izradu predložaka Azure Voditelj resursa](../resource-group-authoring-templates.md).

###<a name="create-sample-application"></a>Stvaranje aplikacije uzorka

Će implementacija ASP.NET web-aplikacije. Prvi je korak da biste stvorili jednostavan web-aplikacije (ili mogli odabrati da biste koristili postojeći – u tom se slučaju možete preskočiti ovaj korak).

Otvorite Visual Studio 2015 i odaberite Datoteka > novi projekt. U dijaloškom okviru koji će se prikazati odaberite Web > ASP.NET web-aplikacije. U odjeljku predlošci odaberite Web, a zatim MVC predložak. Odaberite _Promijeni vrstu provjere autentičnosti_ za _Provjeru autentičnosti bez_. Ovo je primjer aplikacije bi sadržavati jednostavno.

Sada će imati osnovni ASP.Net web-aplikacijama spreman za korištenje u sklopu postupka implementacije.

###<a name="create-msdeploy-package"></a>Stvaranje paketa MSDeploy

Sljedeći je korak stvaranje paketa za implementaciju web-aplikaciju za Azure. Da biste to učinili, spremite projekt i u naredbenom retku pokrenite na sljedeći način:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

To će stvoriti zipane paketa u mapi PackageLocation. Aplikacija je sada da biste se uvesti koji se sada možete izgradnje predložak Voditelj resursa Azure da to učinite.

###<a name="create-arm-template"></a>Stvaranje predloška ARM
Najprije Započnimo s Osnovni predložak OKVIRA koji će stvoriti web-aplikacije i plan hosting (Imajte na umu da parametara i varijable nisu prikazani za ispušteno da bude kraće).

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

Nakon toga će morati izmijeniti web-aplikacije resursa da bi ugniježđene MSDeploy resursa. To će vam omogućiti referenca paket ste ranije stvorili i recite Azure Voditelj resursa za korištenje MSDeploy za implementaciju paketa za Azure WebApp. Na sljedećoj je slici prikazan resurs Microsoft.Web/sites s ugniježđenim MSDeploy resursa:

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

Sada uočit ćete resursa MSDeploy potrebno je svojstvo **packageUri** koja je definirana na sljedeći način:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

**PackageUri** vodi na zip paketa za uri račun za pohranu koji pokazuje na račun za pohranu koju ćete prenijeti. Voditelj resursa Azure će pod utjecajem [Zajednički pristup potpisa](../storage/storage-dotnet-shared-access-signature-part-1.md) padajući paket lokalno s računa za pohranu kada uvođenje predloška. Ovaj postupak će biti automatski putem komponente PowerShell skriptu koja će se prenijeti paket i poziva API upravljanje Azure da biste stvorili tipki obavezne, a one u predložak prosljeđivanje kao parametara (*_artifactsLocation* i *_artifactsLocationSasToken*). Morat ćete definiranje parametara za mapu, a zatim naziv paketa se prenosi u kontejneru za pohranu.

Dalje morate dodati u drugi ugniježđene resurs za postavljanje povezivanja naziv glavnog računala odražava prilagođenu domenu. Najprije morate da biste bili sigurni da ste vlasnik glavnog računala i postavili ste ga za potvrdu po Azure da ste njezin vlasnik – potražite u članku [Konfiguriranje prilagođenog naziva domene na servisu Azure aplikacije](web-sites-custom-domain-name.md). Nakon toga predložak u odjeljku resursa Microsoft.Web/sites možete dodati na sljedeći način:

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

Na kraju morate dodati druge gornje razine resursa, Microsoft.Web/certificates. Ovaj resurs će sadržavati SSL certifikat i će postoje na istoj razini kao web-aplikacije i hostinga plan.

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

Morat ćete imati valjani SSL certifikat da biste postavili ovaj resurs. Nakon što dodate taj valjani certifikat morate izdvojiti bajtova pfx kao niz base64. Jedna od mogućnosti da biste izdvojili to je pomoću sljedeće naredbe ljuske PowerShell:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Nije moguće zatim Proslijedite ovo kao parametar predlošku implementacije OKVIRA.

Sada je spremna predložak OKVIRA.

###<a name="deploy-template"></a>Uvođenje predloška

Konačni korake su dio to sve zajedno u potpuni završetka do kraja implementacije. Da biste olakšali implementacije omogućuje korištenje skriptu PowerShell **Uvođenja AzureResourceGroup.ps1** koja se dodaje prilikom stvaranja projekta programa Azure grupa resursa u Visual Studio će vam pomoći u prijenos sve artefakte obavezno u predlošku. Zahtijeva ste stvorili za pohranu račun koji želite koristiti na vrijeme. U ovom primjeru sam stvorio račun za zajednički prostor za pohranu za package.zip prijenos. Skripta će pod utjecajem AzCopy prenijeti paket na račun za pohranu. Prenesite na lokaciji artefakt mapu, a skriptu će automatski prijenos svih datoteka u taj imenik spremnik imenovani prostora za pohranu. Nakon pozivanja uvođenja AzureResourceGroup.ps1 ćete morati ažurirati povezivanja SSL da biste mapirali prilagođeni naziv glavnog računala s SSL certifikata.

Sljedeće komponente PowerShell prikazuje potpuni implementacije poziva na uvođenja-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

Sada aplikacije mora imati uveden i trebali biste moći otvoriti pomoću preglednika putem https://www.yourcustomdomain.com
