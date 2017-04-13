<properties
   pageTitle="Mrežni pregled resursa davatelja | Microsoft Azure"
   description="Dodatne informacije o novi davatelj mrežni resurs u Azure Voditelj resursa"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="network-resource-provider"></a>Davatelj mreže resursa
Potreba za underpinning u današnjeg uspješno tvrtke, je mogućnost omogućuje stvaranje i upravljanje velikim skaliranje mrežne umu aplikacije u agilno, fleksibilna, sigurne i kontrolirani način. Azure upravitelj resursa (ARM) omogućuje vam da biste stvorili takve aplikacije kao jedan skup resursa u grupe resursa. Takve resursa upravlja se putem različitih davatelji resursa u odjeljku OKVIRA.

Azure Voditelj resursa ovisi o davatelji drugi resurs za pristup resurse. Postoje tri glavna resursa davatelji: mreža, za pohranu i računalnim. Ovaj dokument govori o značajke i pogodnosti paketa resursa davatelj mreže uključujući:

- **Metapodaci** – možete dodati neke informacije resursima pomoću oznaka. Ove oznake se može koristiti za praćenje Upotreba resursa preko grupe resursa i pretplate.
- **Veću kontrolu nad mrežom** - mrežni Resursi su slobodnije povezano, a možete ih odrediti precizniji način. To znači da imate veću fleksibilnost u upravljanju mrežni resursi.
- **Brže konfiguracija** – jer labavo mrežni resursi, možete stvoriti i orkestrirali mrežni resursi paralelno. To je drastično smanjuje vrijeme konfiguraciju.
- **Kontrola pristupa temelji uloga** - RBAC pruža zadane uloge, uz određenu sigurnosnu opseg, osim što omogućuje stvaranje prilagođene uloge za sigurnu upravljanje.
- **Jednostavnije upravljanje i implementaciju** – je jednostavnije implementacije i upravljanje aplikacijama jer možete možete stvoriti na cijelu aplikaciju hrpu kao jedan skup resursa u grupu resursa. I brže za implementaciju jer možete implementirati unosom na teret za JSON predložak.
- **Prilagodbe brzog** - deklarativno stil predloške možete koristiti za omogućivanje prilagodbu kontrolirani i brzog uvođenja.
- **Prilagodba kontrolirani** - deklarativno stil predloške možete koristiti za omogućivanje prilagodbu kontrolirani i brzog uvođenja.
- **Upravljanje sučelja** - možete koristiti bilo koji od sljedećih sučelja za upravljanje resurse:
    - OSTALE temelji API-JA
    - PowerShell
    - .NET SDK
    - Node.JS SDK
    - Java SDK
    - Azure EŽA
    - Portal (pretpregled)
    - Jezik predloška ARM

## <a name="network-resources"></a>Mrežni resursi
Koje sada mogu upravljati mrežni resursi zasebno, umjesto da ih sve upravlja jedan računalnim resursa (virtualnog računala). Na taj način veći stupanj fleksibilnost i agility u složene i velike skaliranje infrastrukture u grupu resursa za sastavljanje poruke.

Konceptualni prikaz ogledne implementacije koje obuhvaćaju aplikacije za višestruki tiered raspoređene su ispod. Svaki resurs koji se prikaže, kao što su NIC-ovi, javnu IP adresa i VMs, upravlja zasebno.

![Mrežni resurs modela](./media/resource-groups-networking/Figure2.png)

Svaki resurs sadrži zajednički skup svojstava i postavite svoje pojedinačne svojstvo. Zajednička svojstva su:

|Svojstvo|Opis|Ogledne vrijednosti|
|---|---|---|
|**ime**|Naziv jedinstveni resursa. Svaka vrsta resursa ima vlastitu imenovanja ograničenja.|PIP01, VM01, NIC01|
|**mjesto**|Azure regije u kojoj se nalazi resurs|westus, eastus|
|**ID-a**|Jedinstveni identifikacijski URI temelji|/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP|

Možete provjeriti pojedinačne svojstva resursa u sljedećim odjeljcima.

[AZURE.INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Upravljanje sučelja
Možete upravljati pomoću sučelja za različite Azure mrežni resursi. U ovom dokumentu ne možemo će usredotočite se na tow te sučelja: REST API-JA i predlošci.

### <a name="rest-api"></a>REST API-JA
Kao što je rečeno ranije, mrežni resursi upravlja putem raznih sučelja, uključujući REST API-JA, .NET SDK, Node.JS SDK, Java SDK, PowerShell, EŽA, Azure Portal i predloške.

Rest API-JA u skladu sa specifikacija protokola HTTP 1.1. Općenito URI strukturu u API raspoređene su ispod:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

I parametara u vitičaste zagrade predstavljaju sljedeće elemente:

- **id pretplate** – identifikacijskog broja za Azure pretplate.
- **resurs-davatelja-prostor naziva** - prostor naziva za davatelja usluga koristi. Vrijednost za davatelja usluga za mrežni resurs je *Microsoft.Network*.
- **naziv regije** - naziv Azure regije

Na sljedeće načine HTTP su podržane kada upućivanje poziva na REST API-JA:

- **STAVITE** – koriste za stvaranje resursa navedene vrste, izmijenite svojstva resursa web-mjesta ili promijenite pridruživanja između resursi.
- **Početak** - koristi za dohvaćanje informacija o dodijeljenu resursa.
- **Brisanje** - koristi da biste izbrisali postojeći resurs.

Zahtjeva i odgovora u skladu sa JSON OSNOVNI oblik tereta. Dodatne informacije potražite u članku [Upravljanje API -ji resursa Azure](https://msdn.microsoft.com/library/azure/dn948464.aspx).

### <a name="arm-template-language"></a>Jezik predloška ARM
Uz upravljanje resursima imperatively (putem API-ji ili SDK), možete koristiti i stil deklarativno programiranje omogućuje stvaranje i upravljanje mrežnim resursima pomoću jezika predloška OKVIRA.

Ogledni prikaz predložak je navedene u nastavku –

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

Predložak je prvenstveno opis JSON resurse i vrijednosti instancu umetnutog putem parametara. Da biste stvorili virtualne mreže s 2 podmreže mogu se primjeru u nastavku.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Imate mogućnost prikazivanja vrijednosti parametara ručno prilikom korištenja predloška ili možete koristiti parametar datoteku. U sljedećem primjeru pokazuje mogući skup vrijednosti parametara koja će se koristiti s predloškom iznad:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


Glavnih prednosti korištenja predložaka su:

- Možete izraditi složene infrastrukture u grupu resursa u deklarativno stil. Djelovanje stvaranja resursa, uključujući upravljanje ovisnost upravlja OKVIRA.
- Infrastruktura moguće stvoriti kontrolirani način preko različite regije i unutar područja promjenom na parametara.
- Kraće vrijeme potencijalnog klijenta u stvaranje predložaka i vodoravnim out Infrastruktura potencijalne klijente deklarativno stil.

Ogledni predlošci potražite u članku [Predlošci Azure brzi početak rada](https://github.com/Azure/azure-quickstart-templates).

Dodatne informacije o jezik predloška OKVIRA potražite u članku [Jezik predloška resursima Azure](../resource-group-authoring-templates.md).

Primjer predloška iznad koristi virtualne mreže i resursima podmreži. Postoje drugi mrežni resursi možete koristiti navedene u nastavku:

### <a name="using-a-template"></a>Pomoću predloška

Možete implementirati uz servise za Azure iz predloška pomoću komponente PowerShell AzureCLI, ili izvođenjem klik za implementaciju s GitHub. Da biste implementirali servise iz predloška u GitHub, izvršiti sljedeće korake:

1. Otvaranje datoteke template3 GitHub. Na primjer, otvorite [virtualne mreža s dva podmreže](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Kliknite na **uvođenja za Azure**, a zatim se prijavite portalu Azure s vjerodajnice.
3. Provjerite je li predložak, a zatim kliknite **Spremi**.
4. Kliknite **Uređivanje parametara** , a zatim odaberite lokaciju, kao što su *Zapad SAD -a*za vnet i podmreže.
5. Ako je potrebno, promjena parametara **ADDRESSPREFIX** i **SUBNETPREFIX** , a zatim kliknite **u redu**.
6. Kliknite **Odaberite grupu resursa** , a zatim u grupi resursa koje želite dodati vnet i podmreže za. Osim toga, možete stvoriti novu grupu resursa tako da kliknete **ili stvorite novi**.
3. Kliknite **Stvori**. Obratite pozornost na to Popločaj prikazivanja **uvođenje predloška dodjele resursa**. Nakon dovršetka implementacijskih vidjet ćete nešto slično zaslon ispod.

![Uvođenje predloška uzorka](./media/resource-groups-networking/Figure6.png)


## <a name="next-steps"></a>Daljnji koraci

[Jezik predloška Azure Voditelj resursa](../resource-group-authoring-templates.md)

[Azure umrežavanje – često korišteni predlošci](https://github.com/Azure/azure-quickstart-templates)

[Izračunavanje davatelja resursa](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

[Pregled Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md)
