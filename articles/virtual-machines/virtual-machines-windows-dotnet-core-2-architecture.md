<properties
   pageTitle="Implementacija izračunati resursi Azure resursima predlošci | Microsoft Azure"
   description="Praktični vodič DotNet Core Azure virtualnog računala"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-architecture-with-azure-resource-manager-templates"></a>Arhitektura aplikacije s predlošcima Voditelj resursa za Azure

Prilikom razvoja implementacije sustava Azure Voditelj resursa za izračun preduvjeti potrebno je mapirati Azure resurse i servisima. Ako aplikacija sastoji se od nekoliko http krajnje točke, baze podataka i podataka predmemoriranje servisa, Azure resursa koja hostiraju svaki od ovih komponenti mora biti rationalized. Ako, primjerice, glazbu trgovine aplikacija uzorka obuhvaća web-aplikacije koje se hostira na virtualnog računala i baze podataka SQL, koji se nalazi u bazi podataka Azure SQL. 

Ovaj dokument detalje o konfiguraciji računalnim resursi spremište glazbe u oglednog predloška Azure Voditelj resursa. Istaknute su sve zavisnosti i jedinstvene konfiguracije. Za najbolje mogućnosti rada prije implementacije instance komponente rješenja Azure pretplate i rad uz predloška Azure Voditelj resursa. Dovršavanje predložak Ovdje možete pronaći – [Glazbu implementacije trgovine u sustavu Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="virtual-machine"></a>Virtualnog računala

Aplikacije za glazbu pohrane obuhvaća web-aplikacije koje korisnici možete pregledavati i kupiti glazbu. Dok je nekoliko Azure servise koje možete hostirati web-aplikacije, na primjer, koristi se virtualnog računala. Pomoću ovog predloška glazbu spremište uzorka, je implementiran virtualnog računala, web-poslužitelj instalirati i web-mjesta spremišta glazbu instalacije i konfiguracije. Radi u ovom se članku detaljnije je samo implementacije virtualnog računala. Konfiguriranje web-poslužitelj i računala je detaljan kasnije članka.

Virtualnog računala mogu se dodati predlošku pomoću čarobnjaka za Visual Studio dodajte novi resurs ili tako da umetnete valjani JSON u predlošku implementacije. Kada implementirate virtualnog računala, potrebne su i nekoliko povezanih resursa. Ako koristite Visual Studio da biste stvorili predložak, te resurse stvaraju se za vas. Ako ručno izgradnje predložak, te resurse morate umetnuti i konfiguracije.

Slijedite vezu da biste vidjeli JSON uzorak u predlošku Voditelj resursa – [JSON virtualnog računala](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).

```none
{
  {
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
  ........<truncated>  
}
```

Kada implementiran, svojstva virtualnog računala mogu vidjeti na portalu za Azure.

![Virtualnog računala](./media/virtual-machines-windows-dotnet-core/vm-win.png)

## <a name="storage-account"></a>Račun za pohranu

Računi za pohranu imaju brojne mogućnosti pohrane i mogućnosti. Za kontekst Azure virtualnog računala s računom za pohranu sadrži virtualne tvrdog diska virtualnog računala i sve diskova dodatne podatke. Uzorak glazbu spremište sadrži jedan račun za pohranu na čuvanje virtualne tvrdi disk svaki virtualnog računala implementacije. 

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Račun za pohranu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

Račun za pohranu je povezati s virtualnog računala unutar deklariranje predložak resursima virtualnog računala. 

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Pridruživanje virtualnog računala i račun za pohranu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).

```none
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

Nakon implementacije, računa za pohranu moguće je prikazati na portalu za Azure.

![Račun za pohranu](./media/virtual-machines-windows-dotnet-core/storacct-win.png)

Klikom na u kontejner blob račun za pohranu, virtualne tvrdi disk datoteke za svaki virtualnog računala u uveden s predloškom mogu vidjeti.

![Virtualna tvrdog diska](./media/virtual-machines-windows-dotnet-core/vhd-win.png)

Dodatne informacije o Azure prostora za pohranu potražite u [dokumentaciji za Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Virtualne mreže

Ako je virtualnog računala potreban Interna s mrežom kao što je mogućnost za komunikaciju s drugim virtualnim strojevima i Azure resursa, potreban je Azure virtualne mreže.  Virtualne mreže ne pristupačni virtualnog računala putem Interneta. Javno povezivanje zahtijeva javnu IP adresu koja je detaljan kasnije u ovoj seriji.

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [virtualne mreže i podmreže](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
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
    ]
  }
}
```

Na portalu Azure virtualne mreže izgleda kao na sljedećoj slici. Obratite pozornost na to da su sve virtualnim strojevima implementiran s predloškom priložene virtualne mreže.

![Virtualne mreže](./media/virtual-machines-windows-dotnet-core/vnet-win.png)

## <a name="network-interface"></a>Mrežno sučelje

 Mrežno sučelje povezuje virtualnog računala virtualne mreže, a preciznije podmreže koji sadrži definirani u virtualne mreže. 
 
 Slijedite vezu da biste vidjeli JSON uzorak u predlošku Voditelj resursa – [Sučelje mreže](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).
 
```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceName'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'RDP-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Svaki virtualnog računala resursa obuhvaća mrežni profil. Mrežno sučelje povezan je s virtualnog računala u ovaj profil.  

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Virtualnog računala mrežni profil](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).


```none
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

Na portalu Azure mrežnog sučelja izgleda kao na sljedećoj slici. Interna IP adresa i pridruživanje virtualnog računala mogu vidjeti na mrežnom resursu sučelja.

![Mrežno sučelje](./media/virtual-machines-windows-dotnet-core/nic-win.png)

Dodatne informacije o Azure virtualne mreže potražite u [dokumentaciji za Azure virtualne mreže](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Baze podataka Azure SQL

Osim virtualnog računala hostirati web-mjesto spremišta glazbu, baze podataka SQL Azure je implementirana za hostiranje glazbu spremište bazu podataka. Prednost korištenja baze podataka SQL Azure ovdje je da drugi skup virtualnim strojevima nije potrebna, a promjenom veličine i dostupnost se temelji na servis.

Baze podataka Azure SQL možete dodati pomoću Visual Studio dodajte novi resurs čarobnjak, ili tako da umetnete valjani JSON u predložak. SQL Server resursa obuhvaća korisničko ime i lozinku koju je dodijeljena administratorska prava na instancu sustava SQL. Osim toga, dodaje SQL vatrozid resurs. Prema zadanim postavkama aplikacije smješten u Azure se možete povezati s instancom SQL. Da biste omogućili vanjski program takvo programa SQL Server Management studio za povezivanje s instancom SQL, vatrozid potrebno je konfigurirati. Radi pokazni videozapis glazbu spremište Zadana konfiguracija je u redu. 

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Baze podataka SQL Azure](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).


```none
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicstoresqlName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('adminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicstoresqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Prikaz SQL server i MusicStore bazu podataka kao što se vidi na portalu za Azure.

![SQL Server](./media/virtual-machines-windows-dotnet-core/sql-win.png)

Dodatne informacije o implementaciji baze podataka SQL Azure, potražite u [dokumentaciji za baze podataka SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Sljedeći korak

<hr>

[Korak 2 - pristupa i sigurnosti u predlošcima Azure Voditelj resursa](./virtual-machines-windows-dotnet-core-3-access-security.md)
