<properties
   pageTitle="Prikupljanje zapisnika korištenjem dijagnostike Azure | Microsoft Azure"
   description="U ovom se članku opisuje kako postaviti Azure Dijagnostika prikupljanje zapisnika iz servisa tkanina klaster izvodi u Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="ms-toddabel"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="toddabel"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Prikupljanje zapisnika korištenjem dijagnostike Azure

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Kada koristite programa tkanina servisa Azure klaster, je dobro prikupiti zapisnike iz sve čvorove na središnjem mjestu. Imate zapisnike središnjem mjestu pomaže vam analizirati i riješili probleme u svoj klaster ili probleme u aplikacije i servise u toj grupi.

Jedan da biste prenijeli i prikupljanje zapisnika tako da koriste nastavak Azure dijagnostiku koja prenosi zapisnika Azure prostora za pohranu. U zapisnicima nisu to korisno izravno u prostor za pohranu. No vanjski postupak možete koristiti za čitanje događaje iz spremišta i potvrdite proizvoda kao što je [Prijava analitiku](../log-analytics/log-analytics-service-fabric.md), [Elastic pretraživanje](service-fabric-diagnostic-how-to-use-elasticsearch.md)ili drugo rješenje zapisnika analize.

## <a name="prerequisites"></a>Preduvjeti
Koristit ćete te alate za izvođenje neke operacije u ovom dokumentu:

* [Azure dijagnostiku](../cloud-services/cloud-services-dotnet-diagnostics.md) (vezane uz servise u Oblaku Azure, ali je korisnih informacija i primjeri)
* [Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](../powershell-install-configure.md)
* [Azure resursima klijenta](https://github.com/projectkudu/ARMClient)
* [Predloška Azure Voditelj resursa](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)


## <a name="log-sources-that-you-might-want-to-collect"></a>Zapisnik izvori koje želite prikupiti
- **Servis tkanina zapisnika**: čuje iz platforme standardne događaj praćenje za sustav Windows (ETW) i EventSource kanalima. Zapisnici mogu biti jedan od nekoliko vrsta:
  - Radu događaja: zapisnicima operacija koja izvršava platformu tkanina servisa. Primjeri: Stvaranje aplikacijama i servisima, promjena stanja čvor i o nadogradnji.
  - [Pouzdan Glumci programming modela događaja](service-fabric-reliable-actors-diagnostics.md)
  - [Pouzdan Services programming modela događaja](service-fabric-reliable-services-diagnostics.md)
- **Događaji aplikacije**: događaji čuje iz koda na servisu i napisali pomoću navedenih u predložaka programa Visual Studio pomoćne klase EventSource. Dodatne informacije o pisanju zapisnika iz aplikacije, potražite u članku [monitora i dijagnosticiranje services u postavi za razvoj lokalnom stroju](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).


## <a name="deploy-the-diagnostics-extension"></a>Implementacija proširenje dijagnostiku
Prvi korak u prikupljanje zapisnika je za implementaciju proširenje Dijagnostika za svaku od VMs u skupini tkanina servisa. Proširenje Dijagnostika prikuplja zapisnika na svakom VM i prenosi ih na račun za pohranu koji navedete. Koraci razlikuju se malo ovisno o tome koristite li Azure portala i upravljanja resursima Azure. Upute i razlikuju se ovisno o tome implementacijskih je dio klaster stvaranja ili za klaster koji već postoji. Pogledajmo korake za svaki scenarij.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>Implementacija proširenje Dijagnostika kao dio stvaranja klaster putem portala sustava
Da biste implementirati za nastavak Dijagnostika VMs u klasteru kao dio stvaranja klaster, koristite ploče postavki dijagnostike prikazano na sljedećoj slici. Da biste omogućili pouzdanog Glumci ili pouzdanog Services događaj zbirka, provjerite je li Dijagnostika postavljen da biste **na** (Zadana postavka). Kada stvorite klaster, ne možete promijeniti postavke pomoću portala za.

![Azure postavki dijagnostike na portalu za stvaranje klaster](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Podrška za Azure tima *zahtijeva* podršku zapisnike da biste lakše razriješili sve zahtjeve za pomoć koje ste stvorili. Ti zapisnici su prikupljene u stvarnom vremenu, a spremaju se u jednom od računa za pohranu stvorene u grupu resursa. Dijagnostika postavki konfiguriranje događaja razini aplikacije. Ti događaji obuhvaćaju [Pouzdanog Glumci](service-fabric-reliable-actors-diagnostics.md) događaje, događaje [Pouzdanog servisa](service-fabric-reliable-services-diagnostics.md) , i neke događaje razinu sustava tkanina servisa za pohranu u Azure prostora za pohranu.

Proizvoda, primjerice [Elastic pretraživanje](service-fabric-diagnostic-how-to-use-elasticsearch.md) ili vlastite postupak možete dobiti događaje s računa za pohranu. Trenutno ne postoji način za filtriranje ili groom događaja koji se šalju u tablicu. Ako ne implementirate postupak da biste uklonili događaje iz tablice, tablice i dalje će rasti.

Kada stvarate klaster pomoću portala, preporučujemo da preuzmete na predlošku *prije nego što kliknete * *u redu*** da biste stvorili klaster. Dodatne informacije pogledajte da biste [postavili klaster tkanina servis pomoću predloška programa Azure Voditelj resursa](service-fabric-cluster-creation-via-arm.md). Morat ćete predložak da biste kasnije unijeli promjene jer ne možete napraviti izmjene pomoću portala za.

Predlošci s portala možete izvesti pomoću sljedećih koraka. Međutim, te predloške može biti teže koristiti jer možda imaju vrijednost null vrijednosti koje nedostaju potrebne informacije.

1. Otvorite grupu resursa.
2. Odaberite **Postavke** da biste prikazali na ploči postavke.
3. Odaberite **implementacijama** da biste prikazali na ploči povijest implementacije.
4. Odaberite implementaciju da biste prikazali detalje o implementacije.
5. Odaberite **Izvoz predložak** da biste prikazali na ploči za predložak.
6. Odaberite **spremite datoteku** da biste izvezli .zip datoteku koja sadrži predložak, parametar i PowerShell datoteke.

Kada izvezete datoteke, morate je promjenu. Uređivanje datoteke parameters.json i uklonite **adminPassword** element. Time će zatražiti lozinka prilikom pokretanja implementacijsku skriptu. Kada koristite implementacijsku skriptu, možda ćete morati riješiti vrijednosti null parametara.

Da biste koristili preuzeti predložak da biste ažurirali konfiguraciju:

1. Izdvajanje sadržaja u mapu na lokalnom računalu.
2. Sadržaj u skladu s vizualnim Nova konfiguracija mijenjati.
3. Pokretanje programa PowerShell i promijenite mapu u koju ste izdvojili sadržaj.
4. Pokrenite **deploy.ps1** i ispunite ID pretplate, naziv grupe resursa (koristite isti naziv da biste ažurirali konfiguraciju) i implementaciju jedinstveni naziv.


### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Implementacija proširenje Dijagnostika kao dio stvaranja klaster pomoću upravitelja resursa za Azure
Da biste stvorili klaster pomoću upravitelja resursa, morate dodati konfiguracije Dijagnostika JSON resursima predložak cijelog klaster prije stvaranja klaster. Primjer predloška pet VM klaster resursima dajemo Dijagnostika konfiguracije dodavanja kao dio naš uzoraka predložak Voditelj resursa. Vidite na tom mjestu u galeriji Azure uzorke: [pet čvor klaster s Voditelj resursa za dijagnostiku predložak uzorka](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Da biste vidjeli postavku Dijagnostika u predlošku Voditelj resursa, otvorite datoteku azuredeploy.json i potražiti **IaaSDiagnostics**. Da biste stvorili klaster pomoću ovog predloška, odaberite gumb **uvođenja za Azure** koja je dostupna na prethodnu vezu.

Osim toga, možete preuzeti uzorka Voditelj resursa, mijenjati i stvaranje klaster s predloškom izmijenjeni pomoću na `New-AzureRmResourceGroupDeployment` naredbe u prozoru za razmjenu Azure PowerShell. Pogledajte sljedeći kod za parametre koje proći u naredbi. Detaljne informacije o tome kako uvesti grupu resursa pomoću komponente PowerShell potražite u članku [uvođenja grupu resursa pomoću predloška Azure Voditelj resursa](../resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>Implementacija proširenje Dijagnostika postojeće klaster
Ako imate postojeću klaster koji nema Dijagnostika implementiran ili ako želite izmijeniti postojeću konfiguraciju, možete dodati ili ažurirati. Izmjena predloška Voditelj resursa koji se koristi za stvaranje postojeće klaster ili preuzimanje predloška s portala sustava, kao što je opisano ranije. Izmjena template.json datoteke, izvršite sljedeće zadatke.

Dodajte novi resurs za pohranu na predložak do odjeljka resursi.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Nakon toga dodajte parametre sekciji neposredno nakon računa definicije prostora za pohranu između `supportLogStorageAccountName` i `vmNodeType0Name`. Zamijenite na rezervirano mjesto teksta *naziv računa spremišta Ovdje dolazi* naziv računa za pohranu.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Nakon toga ažuriranje na `VirtualMachineProfile` dio datoteke template.json dodavanjem sljedeći kod unutar proširenja polja. Ne zaboravite da biste dodali točku sa zarezom na početak ili kraj, ovisno o tome gdje je umetnuti.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Nakon što izmijenite template.json datoteke kao što je opisano, ponovno objaviti predložak Voditelj resursa. Ako predložak izvoza, pokrenut datoteke deploy.ps1 republishes predložak. Nakon implementacije, provjerite je li **ProvisioningState** **uspješno**.


## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Ažuriranje Dijagnostika mogli ste prikupljati i prijenos zapisnika s novog kanala EventSource
Da biste ažurirali Dijagnostika prikupljanje zapisnika s novog kanala EventSource koji predstavljaju nove aplikacije koje ćete implementacije, izvođenje iste korake kao u [prethodnom odjeljku](#deploywadarm) za postavljanje Dijagnostika za postojeće klaster.

Ažuriranje na `EtwEventSourceProviderConfiguration` sekcija u datoteci template.json da biste dodali unose za ažuriranje novog kanala EventSource prije primjene konfiguraciju pomoću na `New-AzureRmResourceGroupDeployment` naredbe ljuske PowerShell. Naziv događaja izvora se definira kao dio kod u Visual Studio generira ServiceEventSource.cs datoteci.

Ako, na primjer, ako izvor događaja zove Moje Eventsource, dodajte sljedeći kod da biste postavili događaje iz moje Eventsource u tablicu pod nazivom MyDestinationTableName.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Da biste prikupili mjerača performansi i zapisnike događaja, izmijeniti predložak resursima pomoću primjerima u [Stvaranje virtualnog računala za Windows nadzora i Dijagnostika tako da pomoću predloška Azure Voditelj resursa](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md). Nakon toga ponovno objaviti predložak Voditelj resursa.

## <a name="next-steps"></a>Daljnji koraci
Detaljno objašnjenje koji su se događaji trebao bi izgledati pri otklanjanju poteškoća, potražite u članku dijagnostičkih događaja čuje [Pouzdanog Glumci](service-fabric-reliable-actors-diagnostics.md) i [Pouzdan Services](service-fabric-reliable-services-diagnostics.md).


## <a name="related-articles"></a>Povezani članci
* [Saznajte kako prikupljanje mjerača performansi i zapisnika korištenjem dijagnostike proširenje](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)
* [Servis Fabric rješenja u zapisnik Analytics](../log-analytics/log-analytics-service-fabric.md)
