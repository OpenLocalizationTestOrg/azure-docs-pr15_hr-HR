<properties 
    pageTitle="Zaključavanje resursi s Voditelj resursa | Microsoft Azure" 
    description="Onemogućavanje korisnika u ažuriranje i brisanje određenih resursi primjenom ograničenja za sve korisnike i uloge." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tomfitz"/>

# <a name="lock-resources-with-azure-resource-manager"></a>Zaključavanje resursi s Azure Voditelj resursa

Kao administrator, morate zaključati na pretplatu, grupa resursa ili resursa da biste drugim korisnicima u tvrtki ili ustanovi iz slučajno izbrišete ili izmjena ključnih resursi. Možete postaviti razinu Zaključaj **CanNotDelete** ili **samo za čitanje**. 

- **CanNotDelete** znači ovlašteni korisnici i dalje mogu čitati i izmjena resursa, ali ih nije moguće izbrisati. 
- **Samo za čitanje** označava ovlašteni korisnici mogu čitati iz resursa, ali ne možete izbrisati ili izvršiti nijednu akciju. Dozvola resurs je ograničena ulozi **čitač** . 

Primjena **samo za čitanje** može uzrokovati neočekivane rezultate jer neke operacije koje čini se kao što je pročitati operacije zahtijevaju dodatne akcije. Na primjer, smještanje zaključati **samo za čitanje** na račun za pohranu onemogućuje sve korisnike popis tipki. Na popisu operacija tipke se upravlja zahtjeva za objavu jer vraćeni tipke su dostupne za pisanje operacija. Drugi, primjerice, postavite zaključati **samo za čitanje** na programa resursa aplikacije servisa za onemogućuje Explorer poslužitelja za Visual Studio prikazuju datoteka resursa zato što taj interakcije zahtijeva pristup za pisanje.

Za razliku od kontrola pristupa na temelju uloga pomoću upravljanja zaključavanja ograničenja primjenjuju se na sve korisnike i uloge. Dodatne informacije o postavljanju dozvola za korisnika i uloga, potražite u članku [Utemeljeno na ulogama Azure kontrola pristupa](./active-directory/role-based-access-control-configure.md).

Kada primijenite zaključavanja na nadređeni opseg, sve podređene resurse nasljeđuju iste lock. Čak i resursima dodate poslije nasljeđuju zaključavanje od nadređenog. Većina ograničenja lokot u nasljeđivanje ima prednost.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Tko može stvaranje ili brisanje zaključavanja u tvrtki ili ustanovi

Stvaranje ili brisanje upravljanje zaključavanja, morate imati pristup **Microsoft.Authorization/\* ** ili **Microsoft.Authorization/locks/\* ** akcije. Ugrađeni uloga samo **vlasnik** i **Korisnički pristup administratora** odobravaju tih akcija.

## <a name="creating-a-lock-through-the-portal"></a>Stvaranje zaključavanja putem portala

[AZURE.INCLUDE [resource-manager-lock-resources](../includes/resource-manager-lock-resources.md)]

## <a name="creating-a-lock-in-a-template"></a>Stvaranje zaključati u predložak

Sljedeći primjer prikazuje predložak koji stvara zaključati na račun za pohranu. Račun za pohranu po kojem želite primijeniti zaključavanje prikazuje se kao parametar. Zaključavanje naziv se stvara spajanjem naziv resursa s **/Microsoft.Authorization/** i zaključavanje, u ovom slučaju **myLock**.

Vrsta navedena je za vrstu resursa. Za pohranu, postaviti vrstu za "Microsoft.Storage/storageaccounts/providers/locks".

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="creating-a-lock-with-rest-api"></a>Stvaranje zaključati pomoću REST API-JA

Možete zaključati distribuiranih resursi [REST API -JA za upravljanje zaključavanja](https://msdn.microsoft.com/library/azure/mt204563.aspx). REST API-JA omogućuje vam stvaranje i brisanje zaključavanja i Dohvaćanje informacija o postojeće zaključavanja.

Da biste stvorili zaključati, pokrenite:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Opseg može biti pretplatu, grupa resursa ili resurs. Naziv lock je ono što želite pozvati zaključavanje. Da biste postigli api-verzija, koristite **2015 01 01**.

U zahtjevu za obuhvaćaju JSON objekt koji navodi svojstva za zaključavanje.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

Primjeri, potražite u članku [REST API -JA za upravljanje zaključavanja](https://msdn.microsoft.com/library/azure/mt204563.aspx).

## <a name="creating-a-lock-with-azure-powershell"></a>Stvaranje zaključati pomoću komponente PowerShell Azure

Možete zaključati distribuiranih resursi Azure PowerShell pomoću **Novo AzureRmResourceLock** kao što je prikazano u sljedećem primjeru.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

Azure PowerShell nudi drugih naredbi za rad zaključavanja, kao što je **Skup AzureRmResourceLock** da biste ažurirali zaključati i **Uklanjanje AzureRmResourceLock** da biste izbrisali zaključati.

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o radu s zaključavanja resursa potražite u članku [Lock dolje resursi vaše Azure](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
- Da biste saznali više o logično organiziranje resursa, potražite u članku [Korištenje oznake da biste organizirali resursi](resource-group-using-tags.md)
- Da biste promijenili koje grupa resursa resursa koji se nalazi u, potražite u članku [Premještanje resurse za novu grupu resursa](resource-group-move-resources.md)
- Možete primijeniti ograničenja i konvencije preko pretplatu prilagođenih pravila. Dodatne informacije potražite u članku [Korištenje pravilnika za upravljanje resursima i nadzor pristupa](resource-manager-policy.md).
