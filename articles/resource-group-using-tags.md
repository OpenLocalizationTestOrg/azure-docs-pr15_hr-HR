<properties
    pageTitle="Korištenje oznaka da biste organizirali Azure resurse | Microsoft Azure"
    description="U članku se objašnjava Primjena oznake da biste organizirali resursi za naplatu i upravljanje njima."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="AzurePortal"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="tomfitz"/>


# <a name="using-tags-to-organize-your-azure-resources"></a>Korištenje oznaka da biste organizirali Azure resursi

Voditelj resursa omogućuje vam da biste logično organizirati resurse primjenom oznaka. Oznake se sastojati od parove ključa vrijednosti koje označavaju resursi sa svojstvima koje sami definirate. Da biste označili resurse kao pripadaju istu kategoriju, Primjena istu oznaku tih resursa.

Kada pogledate resursi određenu oznaku, vidjet ćete resursa iz svih grupa resursa. Niste ograničen na samo resursa u istoj grupi resursa koji omogućuje da biste organizirali resursa na način koji ne ovisi o odnosi implementacije. Oznaka može biti korisno kada želite organizirati resurse za naplatu i upravljanje.

Svakoj oznaci dodate resurs ili grupu resursa automatski se dodaje na razini pretplate taksonomiju. Možete i prethodno unijeti taksonomije za vašu pretplatu s naziva oznaka i vrijednosti koje želite koristiti kao što su resursi označene u budućnosti.

Svaki resurs ili grupa resursa može sadržavati najviše 15 oznake. Naziv oznake ograničeno je na 512 znakova, a vrijednost oznake ograničeno je na dulje od 256 znakova.

> [AZURE.NOTE] Oznake možete primijeniti samo na resurse koji podržavaju operacije Voditelj resursa. Ako ste stvorili virtualnog računala, virtualne mreže ili prostora za pohranu kroz model klasični implementacije (kao što su putem portala za klasični), ne možete primijeniti oznake resursa. Podrška za označavanje, ponovno implementirate ove resurse putem upravitelja resursa. Ostali resursi podrške za označavanje.

## <a name="templates"></a>Predlošci

Da biste označili resursa tijekom implementacije, jednostavno dodajte **oznake** element resursa implementacije i navedite naziv oznake i vrijednost. Naziv oznake i vrijednost ne morate unaprijed postoji u svoju pretplatu. Možete unijeti 15 oznake za svaki resurs.

Sljedeći primjer prikazuje prostora za pohranu račun s oznakom.

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

Trenutno resursima ne podržava obrade objekta za oznaku nazive i vrijednosti. Umjesto toga, prenesite objekta za oznaku vrijednosti, ali i dalje Navedite nazive oznaku, kao što je prikazano u sljedećem primjeru.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "tagvalues": {
          "type": "object",
          "defaultValue": {
            "dept": "Finance",
            "project": "Test"
          }
        }
      },
      "resources": [
      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "examplestorage",
        "tags": {
          "dept": "[parameters('tagvalues').dept]",
          "project": "[parameters('tagvalues').project]"
        },
        "location": "[resourceGroup().location]",
        "properties": {
          "accountType": "Standard_LRS"
        }
      }]
    }


## <a name="portal"></a>Portal

[AZURE.INCLUDE [resource-manager-tag-resource](../includes/resource-manager-tag-resources.md)]

## <a name="powershell"></a>PowerShell

[AZURE.INCLUDE [resource-manager-tag-resources](../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure EŽA

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="rest-api"></a>REST API-JA

Portal i PowerShell koristiti [REST API -JA Voditelj resursa](https://msdn.microsoft.com/library/azure/dn848368.aspx) u pozadini. Ako vam je potrebna integrirati označavanje u drugom okruženju, možete se oznaka s DOHVATI na identifikator resursa i ažurirati skup oznaka s pozivom ZAKRPU.


## <a name="tags-and-billing"></a>Oznake i naplata

Za podržane usluge, možete koristiti oznake da biste grupirali podatke za naplatu. Ako, na primjer, [virtualnim strojevima integriran s Azure Voditelj resursa](./virtual-machines/virtual-machines-windows-compare-deployment-models.md) omogućuju definiranje i primijenite oznake da biste organizirali naplate upotrebe virtualnim računalima. Ako koristite više VMs za različite tvrtke ili ustanove, možete koristiti oznake za korištenje grupa putem centra za trošak.  
Oznake možete koristiti i da biste kategorizirali troškove po okruženje za izvođenje; Primjerice, naplate korištenje VMs koji se izvodi u radnom okruženju.

Može dohvatiti podatke o oznake na [Korištenje resursa Azure i RateCard API -ji](billing-usage-rate-card-overview.md) ili korištenje datoteku vrijednosti odvojenih zarezom (CSV). Preuzmite datoteku za korištenje s [portala za Azure računi](https://account.windowsazure.com/) ili [EA portal](https://ea.azure.com). Dodatne informacije o programskom pristupu podatke o naplati potražite u članku [rast uvida u svoje potrošnje resursa za Microsoft Azure](billing-usage-rate-card-overview.md). Operacije REST API-JA, potražite u članku [Azure naplata REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Kada preuzmete korištenje CSV usluga koji podržavaju oznaka s naplatom oznake se pojavljuju u stupcu **oznake** . Dodatne informacije potražite u članku [objašnjenje računa za Microsoft Azure](billing/billing-understand-your-bill.md).

![U odjeljku oznake u naplata](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Daljnji koraci

- Možete primijeniti ograničenja i konvencije preko pretplatu prilagođenih pravila. Pravilo koje ste definirali nije potreban je da se svi resursi imati vrijednost za određenu oznaku. Dodatne informacije potražite u članku [Korištenje pravilnika za upravljanje resursima i nadzor pristupa](resource-manager-policy.md).
- Uvod u korištenje Azure PowerShell pri implementaciji resursa, potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](./powershell-azure-resource-manager.md).
- Uvod u korištenje Azure EŽA pri implementaciji resursa, potražite u članku [Korištenje EŽA Azure za Mac i Linux, Windows i upravljanje resursima Azure](./xplat-cli-azure-resource-manager.md).
- Uvod u korištenje portala, potražite [pomoću portala za Azure upravljanja Azure resursi](./azure-portal/resource-group-portal.md)  
