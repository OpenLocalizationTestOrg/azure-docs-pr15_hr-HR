<properties
   pageTitle="Voditelj resursa predložak za povezivanje resursi | Microsoft Azure"
   description="Prikazuje shemi resursima za implementaciju veza između povezani resursi pomoću predloška."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="resource-links-template-schema"></a>Shemu predloška veze resursa

Stvaranje veze između dva resursa. Veza primjenjuje se na resursa naziva izvora resursa. Drugi resursa u vezi zove resursa cilj.

## <a name="schema-format"></a>Oblikovanje sheme

Da biste stvorili vezu, dodajte Sljedeća shema do odjeljka resursi predloška.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## <a name="values"></a>Vrijednosti

Tablice u nastavku opisuju vrijednosti morate postaviti u shemi.

| Ime | Vrijednost |
| ---- | ---- |
| Vrsta | Redni broj<br />Obavezno<br />**{prostora za naziv} / {type} / davatelji/veze**<br /><br />Vrsta resursa, da biste stvorili. {Prostora za naziv} i {unesite} vrijednosti se odnositi na davatelja prostor naziva i resursa vrstu izvora resursa. |
| apiVersion | Redni broj<br />Obavezno<br />**2015 01 01**<br /><br />Verziju API će se koristiti za stvaranje resursa. |  
| ime | Niz<br />Obavezno<br />**{resouce}/Microsoft.Resources/{linkname}**<br /> 64 znaka, i ne smije sadržavati % <>; &,?, ili bilo koju kontrolne znakove.<br /><br />A vrijednost te određuje i naziv izvora resursa i naziv za vezu. |
| dependsOn | Polja<br />Neobavezno<br />Popis odvojenih zarezom resursa imena ili resursa odredišnih stilova.<br /><br />Skup resursa ovisi o tu vezu. Ako su resursi povezujete implementiran u isti predložak, uključite nazive resursa u taj element da biste bili sigurni da se najprije implementiran. | 
| Svojstva | Objekt<br />Obavezno<br />[Svojstva objekta](#properties)<br /><br />Objekt koji služi za identifikaciju resursa koje želite povezati, a napomene vezane uz vezu. |  

<a id="properties" />
### <a name="properties-object"></a>Svojstva objekta

| Ime | Vrijednost |
| ------- | ---- |
| targetId | Niz<br />Obavezno<br />**{id resursa}**<br /><br />Identifikator resursa cilj da biste se povezali. |
| bilješke | Niz<br />Neobavezno<br />do 512 znakova<br /><br />Opis lokota. |


## <a name="how-to-use-the-link-resource"></a>Kako koristiti veza resursu

Primjena vezu između dva resursa kada resurse ovisnosti koje se nastavljaju se nakon implementacije. Na primjer, aplikacije možda povezivanje s bazom podataka u grupi različitih resursa. Možete definirati tu ovisnost tako da stvorite vezu iz aplikacije u bazu podataka. Veza omogućuju dokumenta odnos između dva resursa. Noviju verziju, vi ili netko drugi u tvrtki ili ustanovi možete poslati upit resursa za veze da biste otkrili kako resursa s ostalim resursima.

Sve povezane resurse mora pripadati iste pretplate. Svaki resurs može povezati s 50 ostali resursi. Ako bilo koji od povezani Resursi su izbrisali ili premjestili, vlasnik vezu morate očistiti preostale vezu.

Da biste radili s vezama kroz OSTALE, potražite u članku [Povezani resursi](https://msdn.microsoft.com/library/azure/mt238499.aspx).

Da biste vidjeli sve veze za pretplatu, koristite sljedeću naredbu Azure PowerShell. Možete unijeti drugi parametri da biste ograničili rezultate.

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -ResourceGroupName <YourResourceGroupName>

## <a name="examples"></a>Primjeri

U sljedećem primjeru odnosi se samo za čitanje zaključavanja web App.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
            {
                "type": "Microsoft.Web/sites/providers/links",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
                    "notes": "This web site uses the storage account to store user information."
                }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Predlošci za brzi početak rada

Sljedeće predloške za brzi početak rada implementirati resursi s vezom.

- [Upozorenje red s aplikacijom logike](https://azure.microsoft.com/documentation/templates/201-alert-to-queue-with-logic-app)
- [Upozorenje na prazan hod s aplikacijom logike](https://azure.microsoft.com/documentation/templates/201-alert-to-slack-with-logic-app)
- [Dodjela resursa za aplikaciju API-JA s postojećeg pristupnika](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-existing)
- [Dodjela resursa za aplikaciju API-JA s novog pristupnika](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-new)
- [Stvaranje aplikacije logike plus API aplikacije pomoću predloška](https://azure.microsoft.com/documentation/templates/201-logic-app-api-app-create)
- [Logika aplikacije koja se šalje tekstnu poruku kada se aktivira se upozorenja](https://azure.microsoft.com/documentation/templates/201-alert-to-text-message-with-logic-app)


## <a name="next-steps"></a>Daljnji koraci

- Informacije o strukturi predloška potražite u članku [Upravitelj resursa za Azure za izradu predložaka](resource-group-authoring-templates.md).
