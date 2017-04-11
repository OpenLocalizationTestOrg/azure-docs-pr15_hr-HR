<properties
   pageTitle="Implementacija resursi Azure EŽA i predložak | Microsoft Azure"
   description="Koristite upravitelj resursa Azure i Azure EŽA resurs za Azure. Resursi su definirani u predlošku Voditelj resursa."
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

# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Implementacija resursa s resursima predloške i EŽA Azure

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure EŽA](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST API-JA](resource-group-template-deploy-rest.md)

U ovoj se temi objašnjava kako pomoću EŽA Azure s predlošcima resursima za implementaciju resurse za Azure.  

> [AZURE.TIP] Pomoć za ispravljanje pogrešaka pogreška tijekom uvođenja, pogledajte:
>
> - Otklanjanje poteškoća s [postupci implementacije prikaz s Azure EŽA](resource-manager-troubleshoot-deployments-cli.md) dodatne informacije o tome kako izvući podatke koji olakšava pogrešku
> - [Otklanjanje uobičajenih pogrešaka prilikom implementacije resurse za Azure s Azure Voditelj resursa](resource-manager-common-deployment-errors.md) da biste saznali kako riješiti uobičajenih pogrešaka implementacije

Predložak može biti lokalne datoteke ili vanjske datoteke koji je dostupan putem URI. Kada se predložak nalazi se u račun za pohranu, možete ograničiti pristup predloška i dati token za potpis (SAS) zajednički pristup tijekom implementacije.

## <a name="quick-steps-to-deployment"></a>Brzi koraci za implementaciju

U ovom se članku opisuju sve različite mogućnosti dostupne tijekom implementacije. Međutim, često potrebno samo dvije jednostavne naredbe. Da biste brzo početak rada s implementaciju, koristite sljedeće naredbe:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Da biste saznali više o mogućnostima implementacije koje možda bolje prilagođene za vašu situaciju, nastavite čitati u ovom se članku.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-azure-cli"></a>Implementacija s Azure EŽA

Ako niste prije koristili EŽA Azure s Voditelj resursa, pogledajte odjeljak [Korištenje EŽA Azure za Mac i Linux, Windows i upravljanje resursima Azure](xplat-cli-azure-resource-manager.md).

1. Prijavite se na račun za Azure. Nakon unošenja vjerodajnice, naredba vraća rezultat vašu prijavu.

        azure login
  
        ...
        info:    login command OK

2. Ako imate više pretplata, unesite id pretplate koju želite koristiti za implementaciju.

        azure account set <YourSubscriptionNameOrId>

3. Prelazak na modul Azure Voditelj resursa. Primite potvrdu novi način rada.

        azure config mode arm
   
        info:     New mode is arm

4. Ako nemate postojeću grupu resursa, stvorite grupu resursa. Navedite naziv grupe resursa i mjesto na koje su vam potrebne za rješenje. Morate upišete mjesto za grupu resursa jer grupa resursa pohranjuje metapodatke o resurse. Usklađenost razloga preporučujemo vam da biste odredili gdje je pohranjena te metapodataka. Općenito govoreći, preporučujemo da navedete na mjesto na kojem će se nalaziti Većina resurse. Korištenje na istom mjestu može pojednostavniti svoj predložak.

        azure group create -n ExampleResourceGroup -l "West US"

     Vraća se sažetak novu grupu resursa.
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Provjerite valjanost uvođenja prije izvršavanja tako da pokrenete naredbu **Provjera valjanosti predloška azure grupe** . Kada testiranje implementaciju, navesti parametra točno onako kako želite da se prilikom izvršavanja implementacije (prikazano u sljedećem koraku).

        azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup

5. Da biste implementirali resursa u grupu resursa, pokrenite sljedeću naredbu i navedite potrebne parametre. Parametri obuhvaćaju naziv za implementaciju sustava, naziv grupu resursa, put ili URL predloška i sve parametre potrebno za vašu situaciju. 
   
     Postoje sljedeće tri mogućnosti za dohvat vrijednosti parametara: 

     1. Korištenje parametara u istoj razini i lokalne predložak. Svaki je parametar u obliku: `"ParameterName": { "value": "ParameterValue" }`. Sljedeći primjer prikazuje parametre s prespojne znakove.

            azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Korištenje parametara u istoj razini i veza na predložak.

            azure group deployment create --template-uri <LinkToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Korištenje parametra datoteke. Informacije o datoteku predloška potražite u članku [parametar datoteke](#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Kada resurse imate uveden kroz jedan od tri načina iznad, vidjet ćete sažetak implementacije.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Da biste pokrenuli dovršeno implementaciju, postavite **način rada** u **Dovršeno**. Budite oprezni prilikom korištenja taj način kao što ste slučajno izbrisali resursa koji se ne nalaze u vašem predlošku.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Ako želite da biste se prijavili na dodatne informacije o implementaciji koji mogu olakšati otklanjanje pogrešaka implementaciju, koristite parametar **postavku za ispravljanje pogrešaka** . Možete navesti biti prijavljeni pomoću postupka implementacije sadržaja zahtjev, odgovor sadržaja ili oboje.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## <a name="deploy-template-from-storage-with-sas-token"></a>Uvođenje predloška iz spremišta s SAS tokena

Predloške možete dodati račun za pohranu i veze na njih tijekom implementacije s SAS token.

> [AZURE.IMPORTANT] Slijedeći korake u nastavku blob koja sadrži predložak je dostupan samo vlasnik računa. Kada stvorite SAS tokena za blob-om, blob-om je dostupan svakome s tom URI. Ako neki drugi korisnik intercepts URI, koji je mogu pristupiti predložak. Korištenje SAS token izvrstan je način od ograničavanje pristupa predloške, ali ne smiju povjerljive podatke kao što su lozinke izravno u predlošku.

### <a name="add-private-template-to-storage-account"></a>Dodavanje predloška privatni račun za pohranu

Postavljanje računa za pohranu za predloške sljedeće korake:

1. Stvorite grupu resursa.

        azure group create -n "ManageGroup" -l "westus"

2. Stvaranje računa za pohranu. Naziv računa spremišta mora biti jedinstvena preko Azure, pa pošaljite vlastiti naziv računa.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Postavljanje varijable za račun za pohranu i ključa.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Stvaranje spremnika. Dozvole postavljen je na **Isključeno** što znači spremnik je dostupan samo vlasnik.

        azure storage container create --container templates -p Off 
        
4. Dodavanje predloška u spremnik.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Navedite SAS token tijekom implementacije

Da biste implementirali privatne predloška u račun za pohranu, dohvaćanje SAS token i uključiti u URI za predložak.

1. Stvaranje tokena SAS s dozvolama za čitanje i neko vrijeme isteka ograničiti pristup. Postavite vrijeme isteka da biste omogućili dovoljno vremena za dovršenje uvođenje. Dohvaćanje cijelog URI predloška uključujući SAS token.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. Uvođenje predloška unosom URI koji uključuje SAS token.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Primjer SAS token pomoću povezane predložaka, potražite u članku [Korištenje predložaka povezane s Azure Voditelj resursa](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Daljnji koraci
- Primjer implementacija resursi putem klijenta biblioteke .NET, potražite u članku [uvođenja resursima pomoću .NET biblioteke i predložak](virtual-machines/virtual-machines-windows-csharp-template.md).
- Definiranje parametara u predložak, potražite u odjeljku [Predlošci na dokumentima](resource-group-authoring-templates.md#parameters).
- Upute za implementaciju rješenje u različitim okruženjima, potražite u članku [razvoj i testiranje okruženja u Microsoft Azure](solution-dev-test-environments.md).
- Pojedinosti o korištenju KeyVault referenca za prosljeđivanje sigurne vrijednosti, potražite u članku [proći sigurne vrijednosti tijekom implementacije](resource-manager-keyvault-parameter.md).

