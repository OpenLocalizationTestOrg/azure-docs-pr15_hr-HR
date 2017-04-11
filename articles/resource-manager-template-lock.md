<properties
   pageTitle="Voditelj resursa predložak za resursa zaključavanja | Microsoft Azure"
   description="Prikazuje shemi resursima za implementaciju zaključavanja resursa pomoću predloška."
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="resource-locks-template-schema"></a>Shemu predloška zaključavanja resursa

Stvara se zaključati resursa i njezinih podređenih resursi.

## <a name="schema-format"></a>Oblikovanje sheme

Da biste stvorili zaključati, dodajte Sljedeća shema sekciju resursi predloška.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "level": enum,
            "notes": string
        }
    }



## <a name="values"></a>Vrijednosti

Tablice u nastavku opisuju vrijednosti morate postaviti u shemi.

| Ime | Obavezno | Opis |
| ---- | -------- | ----------- |
| Vrsta | Da | Vrsta resursa, da biste stvorili.<br /><br />Za resurse:<br />**{prostora za naziv} / {type} / davatelji/zaključavanja**<br /><br/>Grupa resursa:<br />**Microsoft.Authorization/locks** |
| apiVersion | Da | Verziju API će se koristiti za stvaranje resursa.<br /><br />Korištenje:<br />**2015 01 01**<br /><br /> |
| ime | Da | Vrijednost koja određuje resursa koje želite zaključati i naziv za zaključavanje. Može biti do 64 znaka i ne smije sadržavati % <>; &,?, ili bilo koju kontrolne znakove.<br /><br />Za resurse:<br />**{resource}/Microsoft.Authorization/{lockname}**<br /><br />Grupa resursa:<br />**{lockname}** |
| dependsOn | ne | Popis odvojenih zarezom resursa imena ili resursa odredišnih stilova.<br /><br />Skup resursa to zaključavanje ovisi o. Ako je resursa su zaključavanje implementiran u isti predložak, uključiti taj naziv resursa u taj element da biste bili sigurni resursa najprije implementiran. | 
| Svojstva | Da | Objekt koji označava vrstu lock i napomene vezane uz zaključavanje.<br /><br />U odjeljku [Svojstva objekta](#properties-object). |  

### <a name="properties-object"></a>Svojstva objekta

| Ime | Obavezno | Opis |
| ---- | -------- | ----------- |
| Razina   | Da | Vrsta lock da biste primijenili opseg.<br /><br />**CannotDelete** – korisnici mogu mijenjati resursa, ali ne i izbrisati.<br />**Samo za čitanje** – korisnici mogu čitati iz resursa, ali ne možete izbrisati ili izvršiti nijednu akciju. |
| bilješke   | ne | Opis lokota. Može biti do 512 znakova. |


## <a name="how-to-use-the-lock-resource"></a>Upute za korištenje resursa lock

Ovaj resurs dodati predlošku da biste spriječili navedeni akcije na resurs. Zaključavanje primjenjuje se na sve korisnike i grupe.

Stvaranje ili brisanje upravljanje zaključavanja, morate imati pristup **Microsoft.Authorization/** * ili * *Microsoft.Authorization/locks/* ** Akcije. Ugrađeni ulogama, samo **vlasnika** i **korisnički pristup Administrator ** odobravaju tih akcija. Informacije o kontrola pristupa na temelju uloga potražite u članku [Utemeljeno na ulogama Azure kontrola pristupa](./active-directory/role-based-access-control-configure.md).

Zaključavanje primjenjuje se na navedenog resursa i sve podređene resursi.

Zaključavanje pomoću naredbe ljuske PowerShell **Ukloni AzureRmResourceLock** ili [operacija brisanja](https://msdn.microsoft.com/library/azure/mt204562.aspx) od REST API-JA možete ukloniti.

## <a name="examples"></a>Primjeri

U sljedećem primjeru ne Izbriši lock primjenjuje se na web-aplikacijama.

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
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                },
            },
            {
                "type": "Microsoft.Web/sites/providers/locks",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
             }
        ],
        "outputs": {}
    }

U sljedećem primjeru primjenjuje lokot ne Izbriši grupu resursa.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Authorization/locks",
                "apiVersion": "2015-01-01",
                "name": "MyGroupLock",
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
            }
        ],
        "outputs": {}
    }

## <a name="next-steps"></a>Daljnji koraci

- Informacije o strukturi predloška potražite u članku [Upravitelj resursa za Azure za izradu predložaka](resource-group-authoring-templates.md).
- Dodatne informacije o zaključavanja potražite u članku [Lock resursi s Azure Voditelj resursa](resource-group-lock-resources.md).
