<properties
    pageTitle="Izvoz predloška Azure Voditelj resursa | Microsoft Azure"
    description="Korištenje Azure resursa upravljanje da biste izvezli predloška iz postojeće grupe resursa."
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
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="tomfitz"/>

# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Izvoz predložak Voditelj resursa Azure iz postojećih resursa

Voditelj resursa omogućuje vam da biste izvezli predloška Voditelj resursa iz postojećih resursa u svoju pretplatu. Možete koristiti taj stvoreni predložak dodatne informacije o sintaksi predložak ili da biste automatizirali ponovno uvođenje vašeg rješenja prema potrebi.

Važno Imajte na umu da postoje dva načina da biste izvezli predložak je:

- Možete izvesti stvarni predložak koji ste koristili za implementaciju. Izvezeni predložak sadrži sve parametre i varijable točno onako kako se prikazivala u izvornom predlošku. Taj se način je korisno kada ste implementiran resursi putem portala sustava. Sada želite li vidjeti kako stvoriti predložak da biste stvorili tih resursa.
- Možete izvesti predložak koji predstavlja trenutno stanje grupu resursa. Izvezeni predložak se ne temelji na bilo koji predložak koji ste koristili za implementaciju. Umjesto toga, stvara predložak koji je snimke u grupu resursa. Izvezeni predložak ima mnogo programiranih vrijednosti i vjerojatno ne proizvoljan broj parametre pri definiranju bi obično. Taj se način je korisno kada ste izmijenili grupa resursa putem portala sustava i skripte. Sada ćete morati snimiti grupu resursa kao predložak.

Ova tema prikazuje oba pristupa.

U ovom ćete praktičnom vodiču, prijavite se na portal za Azure, stvorite račun za pohranu i Izvezi predložak za taj račun za pohranu. Dodajte virtualne mreže da biste izmijenili grupu resursa. Na kraju, izvezite novog predloška koji predstavlja njegovu trenutnom stanju. Iako ovaj članak usredotočuje se na pojednostavnjeni infrastrukture, možete koristiti isti se koraci da biste izvezli predloška za složenije rješenja.

## <a name="create-a-storage-account"></a>Stvaranje računa za pohranu

1. [Portal za Azure](https://portal.azure.com), odaberite **Novo** > **prostora za pohranu** > **računa za pohranu**.

      ![Stvaranje spremišta](./media/resource-manager-export-template/create-storage.png)

2. Stvaranje računa za pohranu s nazivom **prostora za pohranu**, svoje inicijale i datum. Naziv računa spremišta mora biti jedinstvena preko Azure. Ako već se koristi naziv, vidjet ćete poruku o pogrešci koja navodi se koristi naziv. Pokušajte varijacija. Za grupu resursa, stvorite novu grupu resursa i nazovite ih **ExportGroup**. Možete koristiti zadane vrijednosti za druga svojstva. Odaberite **Stvori**.

      ![Unesite vrijednosti za pohranu](./media/resource-manager-export-template/provide-storage-values.png)

Uvođenje može potrajati nekoliko minuta. Po završetku implementacijskih pretplate sadrži račun za pohranu.

## <a name="view-a-template-from-deployment-history"></a>Prikaz predloška iz povijesti implementacije

1. Idite na plohu grupu resursa za novu grupu resursa. Imajte na umu da u plohu prikazuje rezultat posljednjeg implementacije. Odaberite tu vezu.

      ![plohu grupa resursa](./media/resource-manager-export-template/resource-group-blade.png)

2. Pogledajte povijest implementacijama za grupu. U slučaju, na plohu vjerojatno popisuje samo jedan implementacije. Odaberite ovaj implementacije.

     ![zadnji implementacije](./media/resource-manager-export-template/last-deployment.png)

3. Na plohu prikazuje sažetak implementacije. Sažetak obuhvaćaju status uvođenje i operacije i vrijednosti koju ste naveli za parametre. Da biste pogledali predložak koji ste koristili za implementaciju, odaberite **predložak prikaza**.

     ![Prikaz sažetka za implementaciju](./media/resource-manager-export-template/deployment-summary.png)

4. Voditelj resursa dohvaća sljedećih šest datoteka za vas:

   1. **Predložak** – predložak koji definira infrastrukture za rješenje. Prilikom stvaranja računa za pohranu putem portala sustava resursima koristi predložak za implementaciju ga i spremiti taj predložak za buduću upotrebu.
   2. **Parametri** - parametar datoteka koje možete koristiti za prosljeđivanje vrijednosti tijekom implementacije. Sadrži vrijednosti koje ste naveli tijekom prvog uvođenja, ali neke od ovih vrijednosti možete promijeniti kada ponovno implementirate predložak.
   3. **EŽA** - programa Azure command-line sučelja (EŽA) skriptna datoteka koje možete koristiti za uvođenje predloška.
   4. **PowerShell** – programa Azure PowerShell skriptna datoteka koje možete koristiti za uvođenje predloška.
   5. **.NET** - A .NET predmete koje možete koristiti za uvođenje predloška.
   6. **Ruby** - A Ruby predmete koje možete koristiti za uvođenje predloška.

     Datoteke su dostupne putem veze preko na plohu. Po zadanom se plohu prikazuje predložak.

       ![predloška prikaza](./media/resource-manager-export-template/view-template.png)

     Pogledajmo pozornost određeni predložak. Predložak trebao bi izgledati slično:

        {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "parametara": {"naziv": {"Vrsta": "Niz"}, "accountType": {"Vrsta": "Niz"}, "mjesto": {"Vrsta": "Niz"}, "encryptionEnabled": {"defaultValue": false, "Vrsta": "Booleovom"}}, "resursi": [{"Vrsta": "Microsoft.Storage/storageAccounts", "sku": {"naziv": "[parameters('accountType')]"}, "Vrsta": "Prostor za pohranu", "naziv": "[parameters('name')]", "apiVersion": "2016-01-01", "mjesto": "[parameters('location')]", "Svojstva": {"šifriranje": {"usluge": {"blob": {"nije omogućeno": "[parameters('encryptionEnabled')]"}}, "keySource": "Microsoft.Storage"}}}]}
 
Ovaj predložak je stvarni predložak koji se koristi za stvaranje računa za pohranu. Obratite pozornost na to sadrži parametre koje omogućuju implementaciju različite vrste računa za pohranu. Dodatne informacije o strukturi predloška potražite u članku [Upravitelj resursa za Azure za izradu predložaka](resource-group-authoring-templates.md). Popis svih funkcije možete koristiti u predlošku, potražite u članku [funkcije predloška Azure Voditelj resursa](resource-group-template-functions.md).


## <a name="add-a-virtual-network"></a>Dodavanje virtualne mreže

Predložak koji ste preuzeli u prethodnom odjeljku predstavlja infrastrukture za taj izvorne implementacije. Međutim, ona će uzima sve promjene koje napravite nakon uvođenje.
Da biste ilustrirali taj problem recimo dodavanjem virtualne mreže putem portala sustava izmijeniti grupa resursa.

1. U grupi plohu resursa odaberite **Dodaj**.

      ![Dodavanje resursa](./media/resource-manager-export-template/add-resource.png)

2. Odaberite **mrežni virtualne** dostupnih resursa.

      ![Odaberite virtualne mreže](./media/resource-manager-export-template/select-vnet.png)

2. Imenujte virtualne mreže **VNET**i koristiti zadane vrijednosti za druga svojstva. Odaberite **Stvori**.

      ![Postavi upozorenje](./media/resource-manager-export-template/create-vnet.png)

3. Nakon virtualne mreže uspješno je postavila u grupu resursa, pogledajte ponovno povijest implementacije. Sada vidite dva implementacije. Ako ne vidite drugi implementaciju, možda morati zatvoriti vaše plohu grupu resursa i ponovno je otvorite. Odaberite novija implementacije.

      ![Povijest implementacije](./media/resource-manager-export-template/deployment-history.png)

4. Prikaz predložak za taj implementacije. Obratite pozornost na to da definira virtualne mreže. Ne uključuje račun za pohranu ranije implementiran. Više ne morate predložak koji predstavlja resursa u grupu resursa.

## <a name="export-the-template-from-resource-group"></a>Izvoz predloška iz grupa resursa

Da biste dobili trenutno stanje grupu resursa, izvezite predložak koji prikazuje snimke u grupu resursa.  

> [AZURE.NOTE] Nije moguće izvesti predloška za grupu resursa koji ima više od 200 resursa.

1. Da biste pogledali predložak za grupu resursa, odaberite **skripte za automatizaciju**.

      ![Izvoz grupa resursa](./media/resource-manager-export-template/export-resource-group.png)

     Podržavaju sve vrste resursa funkcije predložak za izvoz. Ako grupu resursa sadrži samo račun za pohranu i virtualne mreže prikazani u ovom članku, nećete vidjeti pogrešku. Međutim, ako ste stvorili druge vrste resursa, vidjet ćete pogrešku s porukom da postoji problem s izvoz. Saznajte kako rukovati te probleme u odjeljku [otklanjanje problema izvoz](#fix-export-issues) .

      

2. Ponovno vidite šest datoteke koje možete koristiti da biste ponovno implementirate rješenje, no ovaj put predložak je malo razlikovati. Ovaj predložak sadrži samo dva parametra: jednu za naziv računa za pohranu i jednu za naziv virtualne mreže.

        "parameters": {
          "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
          },
          "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
          }
        },

     Voditelj resursa nije dohvatiti predlošci koje ste koristili tijekom implementacije. Umjesto toga generira novog predloška koji se temelji na trenutnoj konfiguraciji resursa. Ako, na primjer, predložak postavlja prostora za pohranu računa mjesto i replikacije vrijednost:

        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Imate nekoliko mogućnosti za nastavak rada pomoću ovog predloška. Možete preuzeti predložak i raditi na njoj lokalno uređivaču JSON. Ili možete spremiti predložak u biblioteci, a zatim na njoj raditi putem portala sustava.

     Ako ste upoznati s uređivaču JSON kao što su [Dodavanje veze za VANJSKIH kod](resource-manager-vs-code.md) ili [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), možda radije preuzimanje predloška lokalno i pomoću tog uređivača. Ako niste postavili pomoću uređivača JSON, možda radije uređivanje predloška putem portala sustava. Ostatak u ovoj se temi pretpostavlja da ste spremili predložak u biblioteku servisa na portalu. Međutim, unesete istu sintaksu promjene predložak li lokalno rad s uređivaču JSON ili putem portala sustava.

     Da biste radili lokalno, odaberite **Preuzimanje**.

      ![Preuzimanje predloška](./media/resource-manager-export-template/download-template.png)

     Da biste se putem portala sustava, odaberite **Dodaj u biblioteku**.

      ![Dodaj u biblioteku](./media/resource-manager-export-template/add-to-library.png)

     Prilikom dodavanja predloška u biblioteku, predlošku dajte naziv i opis. Zatim odaberite **Spremi**.

     ![Postavljanje predloška vrijednosti](./media/resource-manager-export-template/set-template-values.png)

4. Da biste pogledali predložak spremljen u biblioteci, odaberite **nove servise**, upišite **predložaka** za filtriranje rezultata, odaberite **Predlošci**.

      ![traženje predložaka](./media/resource-manager-export-template/find-templates.png)

5. Odaberite predložak pod nazivom koje ste spremili.

      ![Odaberite predložak](./media/resource-manager-export-template/select-library-template.png)

## <a name="customize-the-template"></a>Prilagodba predloška

Izvezeni predložak dobro funkcionira ako želite stvoriti isti račun za pohranu i virtualne mreže za svaku implementacije. Međutim, Voditelj resursa pruža mogućnosti tako da možete implementirati predloške uz znatno veću fleksibilnost. Na primjer, tijekom uvođenja, možda ćete da biste odredili vrstu računa za pohranu da biste stvorili ili vrijednosti koje valja koristiti za prefiks adresu virtualne mreže i predbroj.

U ovom ćete odjeljku dodate parametara izvezene predložak tako da se pa je možete koristiti predložak ako pokrenete ove resurse u drugim okruženja. Dodavanje neke značajke u predložak da biste smanjili vjerojatnost od nailazi na pogreške prilikom uvođenje predloška. Više ne trebate pogoditi jedinstveni naziv za vaš račun za pohranu. Umjesto toga, predložak stvara jedinstveni naziv. Ograničavanje vrijednosti koje se mogu navesti za vrstu računa za pohranu za valjane mogućnosti.

1. Odaberite **Uređivanje** da biste prilagodili predložak.

     ![Prikaz predložaka](./media/resource-manager-export-template/show-template.png)

1. Odaberite predložak.

     ![Uređivanje predloška](./media/resource-manager-export-template/edit-template.png)

1. Da biste mogli proći vrijednosti koje želite navesti tijekom implementacije, zamijenite odjeljku **parametara** novih definicija parametar. Obratite pozornost na to vrijednosti **allowedValues** za **storageAccount_accountType**. Ako slučajno unesete nevažeću vrijednost, tu pogrešku prepoznat će se prije pokretanja uvođenje. Primijetite da su koja omogućuje samo prefiks za naziv računa za pohranu i prefiks je ograničeno na 11 znakova. Kada ograničite prefiks koji će se 11 znakova, provjerite puni naziv premašuje najveći broj znakova za račun za pohranu. Prefiks omogućuje vam da biste primijenili na konvencija imenovanja računi za pohranu. Vidjet ćete kako stvoriti jedinstveni naziv u sljedećem koraku.

        "parameters": {
          "storageAccount_prefix": {
            "type": "string",
            "maxLength": 11
          },
          "storageAccount_accountType": {
            "defaultValue": "Standard_RAGRS",
            "type": "string",
            "allowedValues": [
              "Standard_LRS",
              "Standard_ZRS",
              "Standard_GRS",
              "Standard_RAGRS",
              "Premium_LRS"
            ]
          },
          "virtualNetwork_name": {
            "type": "string"
          },
          "addressPrefix": {
            "defaultValue": "10.0.0.0/16",
            "type": "string"
          },
          "subnetName": {
            "defaultValue": "subnet-1",
            "type": "string"
          },
          "subnetAddressPrefix": {
            "defaultValue": "10.0.0.0/24",
            "type": "string"
          }
        },

2. U odjeljku **varijable** predloška trenutno je prazno. U odjeljku **varijable** stvorite vrijednosti koje Pojednostavnite vidjet ćete da sintaksa za ostale predloška. U ovom se odjeljku zamijenite novu definiciju varijabli. Varijabla **storageAccount_name** spaja prefiks parametar jedinstveni niz koji se generira na temelju identifikator grupu resursa. Više ne trebate pogoditi jedinstveni naziv kada pružanje vrijednosti parametra.

        "variables": {
          "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
        },

3. Da biste koristili parametara i varijabla definicije resursa, zamijenite sekciji **Resursi** nove definicije resursa. Obavijest da malo promijenjen u definicije resursa koji nije vrijednost koja vam je dodijeljen svojstva resursa. Svojstva su jednaki svojstva izvezene predloška. Jednostavno dodjeljujete svojstva za vrijednosti parametara umjesto programiranih vrijednosti. Lokacija resursa postavljena za korištenje na isto mjesto kao grupu resursa kroz izraz **.location resourceGroup ()** . Varijabla koja ste stvorili za naziv računa spremišta poziva putem izraz **varijabli** .

        "resources": [
          {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetwork_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "properties": {
              "addressSpace": {
                "addressPrefixes": [
                  "[parameters('addressPrefix')]"
                ]
              },
              "subnets": [
                {
                  "name": "[parameters('subnetName')]",
                  "properties": {
                    "addressPrefix": "[parameters('subnetAddressPrefix')]"
                  }
                }
              ]
            },
            "dependsOn": []
          },
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccount_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {},
            "properties": {
                "accountType": "[parameters('storageAccount_accountType')]"
            },
            "dependsOn": []
          }
        ]

4. Odaberite **u redu** kad završite uređivanje predloška.

5. Odaberite **Spremi** da biste spremili promjene na predlošku.

     ![Spremanje predloška](./media/resource-manager-export-template/save-template.png)

6. Da biste implementirali ažurirane predloška, odaberite **Implementiraj**.

     ![uvođenje predloška](./media/resource-manager-export-template/deploy-template.png)

7. Unos vrijednosti parametra, a zatim odaberite Nova grupa resursa za resurse za implementaciju.

## <a name="update-the-downloaded-parameters-file"></a>Ažuriranje parametara preuzete datoteke

Ako se radi o preuzete datoteke (umjesto portala biblioteke), potrebno je ažuriranje parametar preuzete datoteke. Više ne odgovara parametara u predlošku. Ne morate koristiti parametar datoteku, ali možete pojednostavniti postupak kada ponovno implementirate okruženju. Koristite zadane vrijednosti koji su definirani u predložak za mnoge parametre tako da datoteku s parametrima potrebno samo dvije vrijednosti.

Zamijenite sadržaj parameters.json datoteka:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

Ažurirani parametar datoteka sadrži vrijednosti samo za parametre koje ste zadanu vrijednost. Kada želite da se vrijednost koja se razlikuje od zadane vrijednosti, unesite vrijednosti za drugi parametri.

## <a name="fix-export-issues"></a>Otklanjanje problema vezanih uz izvoza

Podržavaju sve vrste resursa funkcije predložak za izvoz. Voditelj resursa posebno izvesti neke vrste resursa da biste spriječili će povjerljive podatke. Ako, na primjer, ako imate niz za povezivanje u datoteci config web-mjesta, vjerojatno ne želite izričito prikazana u izvezeni predložak. Možete pristupiti putem riješili problem dodavanjem ručno nedostaje resursa u predlošku.

> [AZURE.NOTE] Samo naiđete na probleme izvoz kada se izvoze iz grupe resursa, a ne od povijesti implementacije. Zadnji uvođenja točno predstavljaju trenutno stanje radne grupe resursa, izvozi predloška iz povijesti implementacije umjesto iz grupe resursa. Izvesti samo iz grupe resursa kada ste unijeli promjene u grupi resursa koji su definirani u jedan predložak.

Ako, na primjer, Ako izvozite predloška za grupu resursa koji sadrži web-aplikacije, baze podataka SQL i niz za povezivanje u config web-mjesta, prikazat će se sljedeća poruka:

![Prikaži pogreške](./media/resource-manager-export-template/show-error.png)

Ako odaberete poruku prikazuje točno koje vrste resursa nisu izvezeni. 
     
![Prikaži pogreške](./media/resource-manager-export-template/show-error-details.png)

Ova tema prikazuje uobičajena rješenja.

### <a name="connection-string"></a>Niz za povezivanje

U resursa web-mjesta dodajte definiciju niz za povezivanje s bazom podataka:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "type": "config",
      "name": "connectionstrings",
      "dependsOn": [
          "[concat('Microsoft.Web/Sites/', parameters('<site-name>'))]"
      ],
      "properties": {
          "DefaultConnection": {
            "value": "[concat('Data Source=tcp:', reference(concat('Microsoft.Sql/servers/', parameters('<database-server-name>'))).fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('<database-name>'), ';User Id=', parameters('<admin-login>'), '@', parameters('<database-server-name>'), ';Password=', parameters('<admin-password>'), ';')]",
              "type": "SQLServer"
          }
      }
    }
  ]
}
```    

### <a name="web-site-extension"></a>Proširenje web-mjesta

U resursa web-mjesta, dodati definiciju kod da biste instalirali:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "name": "MSDeploy",
      "type": "extensions",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('<site-name>'))]"
      ],
      "properties": {
        "packageUri": "[concat(parameters('<artifacts-location>'), '/', parameters('<package-folder>'), '/', parameters('<package-file-name>'), parameters('<sas-token>'))]",
        "dbType": "None",
        "connectionString": "",
        "setParameters": {
          "IIS Web Application Name": "[parameters('<site-name>')]"
        }
      }
    }
  ]
}
```

### <a name="virtual-machine-extension"></a>Proširenje virtualnog računala

Primjeri proširenja virtualnog računala potražite u članku [Azure Windows VM proširenje konfiguracije uzorka](./virtual-machines/virtual-machines-windows-extensions-configuration-samples.md).

### <a name="virtual-network-gateway"></a>Pristupnik virtualne mreže

Dodajte vrsta resursa za pristupnik virtualne mreže.

```
{
  "type": "Microsoft.Network/virtualNetworkGateways",
  "name": "[parameters('<gateway-name>')]",
  "apiVersion": "2015-06-15",
  "location": "[resourceGroup().location]",
  "properties": {
    "gatewayType": "[parameters('<gateway-type>')]",
    "ipConfigurations": [
      {
        "name": "default",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('<vnet-name>'), parameters('<new-subnet-name>'))]"
          },
          "publicIpAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('<new-public-ip-address-Name>'))]"
          }
        }
      }
    ],
    "enableBgp": false,
    "vpnType": "[parameters('<vpn-type>')]"
  },
  "dependsOn": [
    "Microsoft.Network/virtualNetworks/codegroup4/subnets/GatewaySubnet",
    "[concat('Microsoft.Network/publicIPAddresses/', parameters('<new-public-ip-address-Name>'))]"
  ]
},
```

### <a name="local-network-gateway"></a>Pristupnik za lokalnu mrežu

Dodajte vrsta resursa za pristupnik za lokalnu mrežu.

```
{
    "type": "Microsoft.Network/localNetworkGateways",
    "name": "[parameters('<local-network-gateway-name>')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "localNetworkAddressSpace": {
        "addressPrefixes": "[parameters('<address-prefixes>')]"
      }
    }
}
```

### <a name="connection"></a>Veza

Dodavanje veze vrste resursa.

```
{
    "apiVersion": "2015-06-15",
    "name": "[parameters('<connection-name>')]",
    "type": "Microsoft.Network/connections",
    "location": "[resourceGroup().location]",
    "properties": {
        "virtualNetworkGateway1": {
        "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', parameters('<gateway-name>'))]"
      },
      "localNetworkGateway2": {
        "id": "[resourceId('Microsoft.Network/localNetworkGateways', parameters('<local-gateway-name>'))]"
      },
      "connectionType": "IPsec",
      "routingWeight": 10,
      "sharedKey": "[parameters('<shared-key>')]"
    }
},
```


## <a name="next-steps"></a>Daljnji koraci

Čestitamo! Saznali ste kako izvesti predloška iz resursa koji ste stvorili na portalu.

- Možete implementirati predloška kroz [PowerShell](resource-group-template-deploy.md), [Azure EŽA](resource-group-template-deploy-cli.md)ili [REST API -JA](resource-group-template-deploy-rest.md).
- Da biste vidjeli kako izvesti predloška putem komponente PowerShell potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](powershell-azure-resource-manager.md).
- Da biste vidjeli kako izvesti predloška kroz EŽA Azure, potražite u članku [Korištenje EŽA Azure za Mac i Linux, Windows s Azure Voditelj resursa](xplat-cli-azure-resource-manager.md).
