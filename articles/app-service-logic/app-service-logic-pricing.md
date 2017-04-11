<properties 
    pageTitle="Logika aplikacije cijene modela | Microsoft Azure" 
    description="Detalje o cijene funkcioniranje u aplikacijama logike" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="logic-apps-pricing-model"></a>Logika aplikacija cijene modela

Azure logike aplikacije omogućuje skaliranja i izvršavanje Integracija tijekova rada u oblaku.  U nastavku su pojedinosti o naplata i cijene tarife za logike aplikacije.

## <a name="consumption-pricing"></a>Cijene potrošnje

Novostvoreni logike aplikacije pomoću plan potrošnje. Uz cijene modela potrošnje logike aplikacije samo platite koje koristite.  Logika aplikacija su ograničio vrijeme prilikom korištenja plan potrošnje.
Sve akcije izvršava u pokretanja instance aplikacija logike su s ograničenim prometom.

### <a name="what-are-action-executions"></a>Što su izvršavanja akciju?

Svaki korak u definiciju aplikacije logike je akcija.  To obuhvaća okidača, kontrolne tijek korake kao uvjeta, dosega, za svaku petlje do petlje, pozive poveznika i pozive na izvorni akcije.
Okidača su samo posebne akcije koje su namijenjene stvoriti novu instancu logike aplikacija kada dođe do određenog događaja.  Postoje broj različite postavke okidača koji može utjecati na prikaz kako aplikaciju logike je s ograničenim prometom.

-   **Provjere okidača** – pokretanje neprestano polls krajnje dok se ne primi poruku koja zadovoljava kriterije za stvaranje nove instance programa logike aplikacije.  Učestalost moguće je konfigurirati u okidača u dizajneru logike aplikacije.  Svaki zahtjev za ankete, čak i ako se neće stvoriti novu instancu u aplikacije logike će broje se kao programa izvođenja akcije.

-   **Pokretanje Webhook** – ovog okidača čeka klijent da biste je poslali zahtjev na određene krajnje točke.  Svaki zahtjev za poslane na krajnjoj točki webhook broji kao programa izvođenja akcije. Zahtjev i pokretanje HTTP Webhook su oba okidača webhook.

-   **Ponavljanje okidača** – ovog okidača će stvoriti novu instancu logike aplikacije na temelju intervala ponavljanja konfiguriran u okidača.  Na primjer, ponavljanje okidača moguće je konfigurirati za pokretanje svaki tri dana ili čak i svake minute.

Pokretanje izvršavanja moguće je vidjeti u plohu resursa logike aplikacije u dijelu okidača povijest.

Sve akcije koje su izvršava li su uspjelo ili nije uspjelo su s ograničenim prometom kao programa izvođenja akcije.  Akcije koje su preskočeno zbog uvjeta nije zadovoljen u tijeku i akcije koje niste izvršiti jer aplikaciju logike prekinut je prije dovršetka ne računaju se kao izvršavanja akcije.

Akcije izvršava u petlji broje po iteracije petlje.  Ako, na primjer, jednu akciju u na za svaki petlja iterating kroz popis 10 stavki će se broji kao broj stavki na popisu (10) množi broj akcija u petlji (1) plus jedan za pokretanje petlje, a koje, u ovom primjeru bio (10 * 1) + 1 = 11 izvršavanja akcije.

Logika aplikacijama koje su onemogućene ne smije sadržavati nove instance instancirati i stoga vrijeme koje su onemogućene će neće se naplatiti.  Moguće je mindful nakon onemogućivanja logike aplikacija to može potrajati malo vremena za slučajeve da biste postupno prije potpuno onemogućenom.

## <a name="app-service-plans"></a>Aplikacije servisa za tarife

Tarife za aplikaciju servisa više su potrebne za stvaranje logike aplikacije.  Možete referencirati na aplikaciju servisa Planiranje s postojećeg logike aplikacije.  Logika aplikacije prethodno stvorena pomoću Plan usluge za aplikaciju će i dalje ponašaju kao što je prije gdje, ovisno o plan odabrali, će dobiti ograničio vrijeme nakon broj dnevnih izvršavanja se premaše i će neće naplaćivati pomoću metar izvođenja akcije.

Tarife za aplikaciju servisa i njihovih dnevnih izvršavanja dopuštene akcije:

| |Slobodno/zajednički/Basic|Standardna|Premium|
|---|---|---|---|
|Akcija izvršavanja prema danu| 200|10 000|50.000|

### <a name="convert-from-consumption-to-app-service-plan-pricing"></a>Pretvaranje iz potrošnje aplikacije servisa planiranje cijene

Reference na aplikaciju servisa Plan za potrošnju logike aplikacije, možete ga jednostavno [pokrenuti na ispod skriptu PowerShell](https://github.com/logicappsio/ConsumptionToAppServicePlan).  Provjerite sadrži [alate za Azure PowerShell](https://github.com/Azure/azure-powershell) instaliran.

``` powershell
Param(
    [string] $AppService_RG = '<app-service-resource-group>',
    [string] $AppService_Name = '<app-service-name>',
    [string] $LogicApp_RG = '<logic-app-resource-group>',
    [string] $LogicApp_Name = '<logic-app-name>',
    [string] $subscriptionId = '<azure-subscription-id>'
)

Login-AzureRmAccount 
$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId
$appserviceplan = Get-AzureRmResource -ResourceType "Microsoft.Web/serverFarms" -ResourceGroupName $AppService_RG -ResourceName $AppService_Name
$logicapp = Get-AzureRmResource -ResourceType "Microsoft.Logic/workflows" -ResourceGroupName $LogicApp_RG -ResourceName $LogicApp_Name

$sku = @{
    "name" = $appservicePlan.Sku.tier;
    "plan" = @{
      "id" = $appserviceplan.ResourceId;
      "type" = "Microsoft.Web/ServerFarms";
      "name" = $appserviceplan.Name  
    }
}

$updatedProperties = $logicapp.Properties | Add-Member @{sku = $sku;} -PassThru

$updatedLA = Set-AzureRmResource -ResourceId $logicapp.ResourceId -Properties $updatedProperties -ApiVersion 2015-08-01-preview
```

### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Pretvorba iz aplikacije servisa planiranje cijene potrošnje

Da biste promijenili logike aplikacije koja ima u aplikaciju servisa planiranje pridružen potrošnje model uklonite vodič za planiranje aplikacije servisa u definiciji logike aplikacije.  To se može učiniti s pozivom cmdlet ljuske PowerShell:

`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`

## <a name="pricing"></a>Cijene

Cijene detalje potražite u članku [Logike aplikacije cijene](https://azure.microsoft.com/pricing/details/logic-apps/).

## <a name="next-steps"></a>Daljnji koraci

- [Pregled logike aplikacije][whatis]
- [Stvaranje prve logike aplikacije][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: app-service-logic-what-are-logic-apps.md
[create]: app-service-logic-create-a-logic-app.md

