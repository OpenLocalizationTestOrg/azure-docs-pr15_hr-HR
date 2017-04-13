<properties
   pageTitle="Access i sigurnost u Azure resursima predlošci | Microsoft Azure" 
   description="Praktični vodič DotNet Core Azure virtualnog računala"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="access-and-security-in-azure-resource-manager-templates"></a>Access i sigurnost u predlošcima Voditelj resursa za Azure

Aplikacije koje se nalaze u Azure vjerojatno moraju biti pristup putem Interneta ili VPN / Express usmjeravanje veze sa Azure. Pomoću aplikacije uzorka glazbu spremište na web-mjestu postane dostupna na Internetu s javnu IP adresa. Uz pristup putem uspostaviti veze s aplikacijom i pristup resursima virtualnog računala same treba zaštiti. Ovaj sigurnosti programa access prikazuje se s mrežom sigurnosne grupe. 

Ovaj dokument detalje o kako se aplikacije za glazbu pohrane zaštićene u oglednog predloška Azure Voditelj resursa. Istaknute su sve zavisnosti i jedinstvene konfiguracije. Za najbolje mogućnosti rada prije implementacije instance komponente rješenja Azure pretplate i rad uz predloška Azure Voditelj resursa. Dovršavanje predložak Ovdje možete pronaći – [Implementacije spremište glazbu na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).


## <a name="public-ip-address"></a>Javnu IP adresa

Za pristup javnim Azure resursa, može se koristiti javno resursa IP adresa. Javnu IP adresu možete konfigurirati statički ili dinamički IP adresa. Ako koristi se dinamički adresu, a virtualnog računala je zaustavio i deallocated, uklanja se adrese. Kada računalo ponovno pokreće, ga može dodijeliti različite javnu IP adresu. Da biste onemogućili promjenu IP adresu, može se koristiti Rezervirana IP adresa. 

Javna IP adresa možete dodati predlošku Voditelj resursa Azure pomoću Visual Studio novi resurs Čarobnjak za dodavanje ili tako da umetnete valjani JSON u predložak. 

Slijedite vezu da biste vidjeli JSON uzorak u predlošku Voditelj resursa – na [Javnu IP adresu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicipaddressName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "public-ip-front"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Javnu IP adresu može biti pridruženi virtualne mrežnog prilagodnika ili raspoređivača opterećenja. U ovom primjeru jer je web-mjesto spremišta glazbu rasporediti preko nekoliko virtualnim strojevima opterećenje na javnu IP adresu priložiti raspoređivača opterećenja.

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Pridruživanje opterećenja javnu IP adresa](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Javnu IP adresu kao vide na portalu Azure. Obratite pozornost na to da je na javnu IP adresu povezan raspoređivača opterećenja i ne virtualnog računala. Balancers opterećenje mreže detaljno opisane u sljedeći dokument ovaj niz.

![Javnu IP adresa](./media/virtual-machines-linux-dotnet-core/pubip.png)

Dodatne informacije o Azure javnu IP adrese, potražite u članku [IP adresa u Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Mrežni sigurnosne grupe

Kada access je uspostavljena Azure resursima, mora biti ograničeni pristup. Za Azure virtualnim strojevima sigurnog pristupa se postiže pomoću sigurnosne grupe mreže. Pomoću aplikacije uzorka spremište glazbu, sve virtualnog računala ograničen je pristup za sve osim za priključak 80 za pristup http, a priključak 22 SSH pristup. Mrežni sigurnosne grupe mogu se dodati predlošku Voditelj resursa Azure pomoću Visual Studio novi resurs Čarobnjak za dodavanje ili tako da umetnete valjani JSON u predložak.

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Mreže sigurnosne grupe](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).

```none
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('nsgfront')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "nsg-front"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
}
```

U ovom primjeru sigurnosne grupe mreže je povezati s objektom podmreže prijavljenom u virtualne mrežni resurs. 

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [mreže sigurnosne grupe pridruživanje virtualne mreže](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).


```none
"subnets": [
  {
    "name": "[variables('subnetName')]",
    "properties": {
      "addressPrefix": "10.0.0.0/24",
      "networkSecurityGroup": {
        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
      }
    }
  }
```

Evo kako sigurnosne grupe mreže izgleda na portalu Azure. Uočite da se NSG možete pridružiti podmreži i / ili mreži sučelja. U ovom slučaju na NSG je povezan podmreži. U ovoj konfiguraciji ulazna pravila primjenjuju se na sve virtualnim strojevima koji su povezani s podmreži.

![Mrežni sigurnosne grupe](./media/virtual-machines-linux-dotnet-core/nsg.png)

Detaljne informacije o sigurnosnim grupama s mrežom potražite u članku [što je mreža sigurnosne grupe]( https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Sljedeći korak

<hr>

[Korak 3 – dostupnosti i skaliranje u predlošcima Azure Voditelj resursa](./virtual-machines-linux-dotnet-core-4-availability-scale.md)
