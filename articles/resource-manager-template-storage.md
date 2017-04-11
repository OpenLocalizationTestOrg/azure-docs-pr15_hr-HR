<properties
   pageTitle="Predložak Voditelj resursa za pohranu | Microsoft Azure"
   description="Prikazuje shemi resursima za implementaciju račune za pohranu putem predloška."
   services="azure-resource-manager,storage"
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

# <a name="storage-account-template-schema"></a>Shemu predloška račun za pohranu

Stvara račun za pohranu.

## <a name="schema-format"></a>Oblikovanje sheme

Stvaranje računa za pohranu, dodajte Sljedeća shema do odjeljka resursi predloška.

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## <a name="values"></a>Vrijednosti

Tablice u nastavku opisuju vrijednosti morate postaviti u shemi.

| Ime | Vrijednost |
| ---- | ---- |
| Vrsta | Redni broj<br />Obavezno<br />**Microsoft.Storage/storageAccounts**<br /><br />Vrsta resursa, da biste stvorili. |
| apiVersion | Redni broj<br />Obavezno<br />**2015 06 15** ili **2015-05-01 – pregled**<br /><br />Verziju API će se koristiti za stvaranje resursa. | 
| ime | Niz<br />Obavezno<br />Između 3 i 24 znakova, samo brojeve i mala slova.<br /><br />Naziv računa spremišta da biste stvorili. Naziv mora biti jedinstvena u svim Azure. Preporučujemo da koristite funkciju [uniqueString](resource-group-template-functions.md#uniquestring) s vašeg konvencija imenovanja kao što je prikazano u primjeru u nastavku. |
| mjesto | Niz<br />Obavezno<br />Regija koja podržava račune za pohranu. Da biste utvrdili valjani područja, potražite u članku [podržane područja](resource-manager-supported-services.md#supported-regions).<br /><br />Područje za hostiranje na račun za pohranu. |
| Svojstva | Objekt<br />Obavezno<br />[Svojstva objekta](#properties)<br /><br />Objekt koji navodi vrstu računa za pohranu da biste stvorili. |

<a id="properties" />
### <a name="properties-object"></a>Svojstva objekta

| Ime | Vrijednost |
| ---- | ---- | 
| accountType | Niz<br />Obavezno<br />**Standard_LRS**, **Standard_ZRS**, **Standard_GRS**, **Standard_RAGRS**ili **Premium_LRS**<br /><br />Vrsta računa za pohranu. Dopušteno vrijednosti koje odgovaraju standardne lokalno suvišnih, standardne suvišnih Zone, standardni Redundant zemlj., standardni zemlj. – Redundant pristup za čitanje i Premium lokalno suvišnih. Informacije o tim vrstama računa potražite u članku [replikacije Azure prostora za pohranu](./storage/storage-redundancy.md ). |

    
## <a name="examples"></a>Primjeri

U sljedećem primjeru uvodi standardne lokalno suvišnih prostora za pohranu računa s jedinstveni naziv na temelju id grupe resursa.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                "location": "[resourceGroup().location]",
                "properties": 
            {
                    "accountType": "Standard_LRS"
            }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Predlošci za brzi početak rada

Postoje mnoge brzi početak rada predloške koji sadrže s računom za pohranu. Sljedeći predlošci objašnjavaju neke uobičajene scenarije:

- [Stvaranje računa standardnu prostora za pohranu](https://azure.microsoft.com/documentation/templates/101-storage-account-create)
- [Jednostavan implementacije VM sustava Windows](https://azure.microsoft.com/documentation/templates/101-vm-simple-windows)
- [Jednostavan implementaciju sustava VM Linux](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux)
- [Stvaranje profila CDN, CDN krajnje s računom za pohranu kao izvor](https://azure.microsoft.com/documentation/templates/201-cdn-with-storage-account)
- [Stvaranje visoke farme sustava SharePoint Availabilty s 9 VMs korištenje DSC nastavka Powershell](https://azure.microsoft.com/documentation/templates/sharepoint-server-farm-ha)
- [Jednostavan implementacije na 5 čvor sigurne servisa tkanina klaster s WAD omogućeno](https://azure.microsoft.com/documentation/templates/service-fabric-secure-cluster-5-node-1-nodetype-wad)
- [Stvaranje virtualnog računala iz slike sustava Windows s 4 diskova prazan podataka](https://azure.microsoft.com/documentation/templates/101-vm-multiple-data-disk)


## <a name="next-steps"></a>Daljnji koraci

- Opće informacije o prostora za pohranu potražite u članku [Uvod u spremište na platformi Microsoft Azure](./storage/storage-introduction.md).
- Na primjer predložaka koji koriste novi prostor za pohranu račun s virtualnog računala potražite u članku [uvođenja jednostavan Linux VM](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) ili [uvođenja jednostavne VM Windows](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/).
