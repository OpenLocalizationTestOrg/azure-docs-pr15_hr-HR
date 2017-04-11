<properties
   pageTitle="Vodič za upravljanje resursima predložak | Microsoft Azure"
   description="Korak po korak vodič predloška upravitelj resursa dodjeljivanje osnovni arhitektura Azure IaaS."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   
# <a name="resource-manager-template-walkthrough"></a>Vodič za upravljanje resursima predloška

Jedna od prvog pitanja prilikom stvaranja predloška jest "kako pokrenuti?". Jedan počnite od praznog predloška, slijedite osnovna struktura opisane u [članku Stvaranje predloška](resource-group-authoring-templates.md#template-format)i dodavanje resurse i odgovarajuće parametre i varijabli. Dobar bi da biste pokrenuli putem [Galerija brzi početak rada](https://github.com/Azure/azure-quickstart-templates) i potražite slične scenariji onu koju pokušavate stvoriti. Možete spajati nekoliko predložaka ili uređivanje postojeće kako bi odgovarao određene scenariju. 

Pogledajmo na uobičajeni infrastrukture:

* Dva virtualnim strojevima koristiti isti prostor za pohranu račun su u istom postavljanje dostupnosti, a zatim na istoj podmreži virtualne mreže.
* U jedan NIC i VM IP adresa za svaki virtualnog računala.
* Opterećenja s u pravilo na priključak 80 za ujednačavanje opterećenja

![Arhitektura](./media/resource-group-overview/arm_arch.png)

U ovoj se temi vodit će vas kroz korake Stvaranje predloška resursima za taj infrastrukture. Konačni predloška stvorite se temelji na predlošku brzi početak rada naziva [2 VMs u raspoređivača opterećenja i pravila za ujednačavanje opterećenja](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/).

No, koja je mnogo napraviti odjednom, možemo najprije stvorite račun za pohranu i implementirajte ga. Nakon što ste usvojili stvaranje računa za pohranu, će dodati druge resurse i ponovno uvođenje predloška da biste dovršili Infrastruktura.

>[AZURE.NOTE] Možete koristiti bilo koju vrstu uređivač prilikom stvaranja predloška. Visual Studio nudi alate koji Pojednostavnite razvoj predložak, ali ne morate Visual Studio za dovršetak ovog praktičnog vodiča. Praktični vodič na stvaranje implementacija Web App i SQL baze podataka pomoću programa Visual Studio, potražite u članku [Stvaranje i implementacija grupe Azure resursa kroz Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). 

## <a name="create-the-resource-manager-template"></a>Stvaranje predloška Voditelj resursa

Predložak je JSON datoteku koja definira sve resurse će implementacija. Također omogućuje da definirate parametre koje su navedene tijekom uvođenja, varijabli koje konstruirana iz drugih vrijednosti i izrazi i izlaza iz uvođenje. 

Započnimo s predloškom najjednostavniji:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Spremanje datoteka u obliku **azuredeploy.json** (Imajte na umu da predložak može imati bilo koji naziv koji želite, samo taj it mora biti json datoteka).

## <a name="create-a-storage-account"></a>Stvaranje računa za pohranu
Unutar odjeljka **Resursi** dodajte objekt koji definira račun za pohranu, kao što je prikazano u nastavku. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

Koje možda se pitate li se te svojstva i vrijednosti odakle potječu. Svojstva **Vrsta**, **naziv**, **apiVersion**i **mjesto** su standardne elemente koji su dostupni za sve vrste resursa. Koje dodatne informacije o uobičajene elemente na [resurse](resource-group-authoring-templates.md#resources). **naziv** je postavljeno na vrijednost parametra koji proslijedite u tijekom uvođenja i **mjesto** kao mjesto koristi grupu resursa. Ne možemo ćete pogledajte kako odrediti **vrstu** i **apiVersion** u sljedećim odjeljcima.

U odjeljku **Svojstva** sadrži sva svojstva koja su jedinstvena vrsti određeni resurs. Vrijednosti koju navedete u ovom odjeljku točno odgovara operacija STAVLJANJA u REST API-JA za stvaranje vrste resursa. Prilikom stvaranja računa za pohranu, morate navesti **accountType**. Obratite pozornost na to u [REST API -JA za stvaranje računa za pohranu](https://msdn.microsoft.com/library/azure/mt163564.aspx) odjeljku Svojstva OSTATAK postupka sadrži i svojstva u programu **accountType** i su navedenih dopušteno vrijednosti. U ovom primjeru vrsta računa postavljena na **Standard_LRS**, ali nije moguće navesti nekoj drugoj vrijednosti ili dopušta prenesite vrstu računa kao parametar.

Sada ćemo prešli u odjeljku **parametre** i potražite u članku kako definirati naziv računa za pohranu. Dodatne informacije o korištenju parametara pri [parametara](resource-group-authoring-templates.md#parameters). 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
Ovdje ste definirali parametar vrste niz koji sadrži naziv računa za pohranu. Vrijednost za ovaj parametar objavit ćemo zajedno tijekom implementacije predloška.

## <a name="deploying-the-template"></a>Uvođenje predloška
Imamo puno predloška za stvaranje novog računa za pohranu. Kako opozvati, predložak je spremljena u datoteci **azuredeploy.json** :

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

Postoje prilično nekoliko načina za uvođenje predloška, kao što je vidljivo u [članku implementacija resursa](resource-group-template-deploy.md). Uvođenje predloška pomoću komponente PowerShell Azure, koristite:

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Ili za uvođenje predloška pomoću EŽA Azure, koristite:

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

Sada ste vlasnik Sretan računa za pohranu!

Da biste dodali svi resursi koji su potrebni za implementaciju arhitektura opisane u početka ovog praktičnog vodiča će se na sljedeće korake. Dodat će ove resurse u isti predložak koji ste radili.

## <a name="availability-set"></a>Postavljanje dostupnosti
Nakon definiciju za račun za pohranu, dodajte je availably postavljene za virtualnim računalima. U ovom slučaju nema dodatnih svojstava obavezna, pa je njegova definicija relativno jednostavno. U slučaju da želite definirati ažuriranje domene count i kvara domene broj vrijednosti potražite u članku [REST API -JA za stvaranje dostupnost postavljanje](https://msdn.microsoft.com/library/azure/mt163607.aspx) sekcije cijelog svojstva.

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

Obratite pozornost na to da se **naziv** postavljen tako da vrijednost varijable. Za ovaj predložak naziv skupa dostupnost potreban je u nekoliko različitih mjesta. Predložak možete održavati jednostavnije po jednom definiranje tu vrijednost i njihovo korištenje u više mjesta.

Vrijednost koju navedete za **vrstu** sadrži davatelja resursa i vrsta resursa. Za skupove dostupnost davatelja resursa je **Microsoft.Compute** i resursa vrsta je **availabilitySets**. Na popisu dostupnih resursa davatelja možete dobiti pokretanjem sljedeće naredbe ljuske PowerShell:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Ili ako koristite Azure EŽA, pokrenite sljedeću naredbu:
```
    azure provider list
```
Given da se u ovoj temi stvarate s računima za pohranu, virtualnim strojevima i virtualne mreže, radite s:

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Da biste vidjeli vrste resursa za određeni davatelj usluga, pokrenite sljedeću naredbu komponente PowerShell:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Ili EŽA Azure sljedeću naredbu će se vratiti u dostupne vrste JSON OSNOVNI oblik i spremiti datoteku.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

Trebali biste vidjeti **availabilitySets** kao jedna vrsta unutar **Microsoft.Compute**. Puni naziv vrste je **Microsoft.Compute/availabilitySets**. Možete odrediti naziv vrste resursa za bilo koju od resursa u predlošku koji.

## <a name="public-ip"></a>Javnu IP
Definiranje javnu IP adresa. Ponovno pogledati [REST API -JA za javnu IP adrese](https://msdn.microsoft.com/library/azure/mt163590.aspx) za svojstva da biste postavili.

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

Način dodjele postavljen za **dinamičku** , ali nije moguće postaviti na vrijednost potrebno ili postavite da biste prihvatili vrijednosti parametra. Omogućeno je korisnicima predloška za prosljeđivanje vrijednost za oznaku naziv domene.

Sada Pogledajmo kako odrediti **apiVersion**. Vrijednost koju navedete jednostavno odgovara verziji REST API-JA koji želite koristiti za stvaranje resursa. Tako, možete pogledati u dokumentaciji REST API-JA za tu vrstu resursa. Ili pokrenite sljedeću naredbu komponente PowerShell za određenu vrstu.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Koja vraća sljedeće vrijednosti:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

Da biste vidjeli verzije API-JA s EŽA Azure, pokrenite isti **Prikaz azure davatelja** naredbu prethodno prikazano.

Prilikom stvaranja novog predloška, odaberite najnoviju verziju API-JA.

## <a name="virtual-network-and-subnet"></a>Virtualne mreže i podmreže
Stvaranje virtualne mreže s jednog podmreže. Pogledajte [REST API -JA za virtualne mreže](https://msdn.microsoft.com/library/azure/mt163661.aspx) za sva svojstva da biste postavili.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## <a name="load-balancer"></a>Opterećenja
Sada će stvoriti vanjski dostupnog raspoređivača opterećenja. Budući da ova opterećenja koristi na javnu IP adresu, morate postaviti ovisnosti na javnu IP adresu u odjeljku **dependsOn** . To znači da raspoređivača opterećenja će ne implementiraju na odgovarajući dok na javnu IP adresu Završi implementacija. Bez definiranja ovaj ovisnost, primit ćete pogrešku jer resursima će pokušati implementirati resursi paralelno i će pokušati da biste postavili raspoređivača opterećenja na javnu IP adresa koji još ne postoji. 

Skupna adresa za pozadinskog, nekoliko ulazna pravila NAT RDP u na VMs i pravila za ujednačavanje opterećenja će stvoriti i s tcp probni na priključak 80 u definiciji ovaj resurs. Odjava [REST API -JA za opterećenja](https://msdn.microsoft.com/library/azure/mt163574.aspx) za sva svojstva.

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## <a name="network-interface"></a>Mrežno sučelje
Morate stvoriti 2 sučelje mreže jedan za svaki VM. Umjesto da biste uključili duplicirane stavke za sučelje mreže, [funkcija copyIndex()](resource-group-create-multiple.md) možete koristiti za ponavljanje petlje Kopiraj (naziva se nicLoop) i stvaranje broja sučelja mreže kako je definirano u na `numberOfInstances` varijabli. Mrežno sučelje ovisi o stvaranje virtualne mreže i raspoređivača opterećenja. Da biste konfigurirali skupna adresa za raspoređivača opterećenja i ulazna pravila NAT koristi podmreže definiran u stvaranje virtualne mreže i id raspoređivača opterećenja.
Pogledajte [REST API -JA za mrežni sučelja](https://msdn.microsoft.com/library/azure/mt163668.aspx) za sva svojstva.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## <a name="virtual-machine"></a>Virtualnog računala
2 virtualnim strojevima će stvoriti pomoću funkcije copyIndex(), kao i u stvaranje [sučelje mreže](#network-interface).
Stvaranje VM ovisi o računu za pohranu mreže sučelje i dostupnosti skupa. U ovom VM stvorit će se iz trgovine slike, kako je definirano u na `storageProfile` svojstvo - `imageReference` koristi se za definiranje publisher slike, ponuda, sku i verziju. Na kraju, dijagnostičkih profila je konfiguriran za omogućivanje Dijagnostika za na VM. 

Da biste pronašli odgovarajući svojstva sliku trgovine, slijedite članci [Odaberite Linux virtualnog računala slike](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) ili [slike virtualnog računala za Windows](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md) .

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] Slika objavljuje **3 strana dobavljačima**, morat ćete navesti drugi svojstvo s nazivom `plan`. Primjer pronaći ćete u [Ovaj predložak](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) u galeriji brzi početak rada. 

Završili ste definiranje resursa u predlošku.

## <a name="parameters"></a>Parametri

U odjeljku parametara definirati vrijednosti koje možete navesti kada uvođenje predloška. Samo definiranje parametara za vrijednosti koje mislite da moraju biti različiti tijekom implementacije. Možete unijeti zadanu vrijednost za parametar koji se koristi ako ga ne koristiti tijekom implementacije. Možete definirati i dopuštena vrijednost kao što je prikazano za parametar **imageSKU** .

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## <a name="variables"></a>Varijable

U odjeljku varijable možete definirati vrijednosti koje se koriste u više mjesta u predlošku ili vrijednosti koje su konstruirana iz drugih izraza ili varijabli. Varijable se često koriste da biste pojednostavnili vidjet ćete da sintaksa predloška.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

Dovršite predložak! Možete usporediti predloška protiv cijeli predložak u [galeriji brzi početak rada](https://github.com/Azure/azure-quickstart-templates) u odjeljku [2 VMs s učitati opterećenja i Učitavanje predloška pravila opterećenja](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules). Predložak može se malo razlikovati ovisno o korištenju brojevi različitih verzija. 

Ponovno možete uvesti predložak koristeći istu naredbi koje se koriste prilikom implementacije račun za pohranu. Ne trebate izbrisati račun za pohranu prije no što ponovno implementirate jer resursima će preskakati ponovno stvaranje resursa koji već postoji, a niste promijenili.

## <a name="next-steps"></a>Daljnji koraci

- [Azure resursima predložak Visualizer (ARMViz)](http://armviz.io/#/) je sjajan alat vizualizirati ARM predlošci, kao što je može postati prevelika da biste shvatili samo čitanju json datoteku.
- Dodatne informacije o strukturi predloška potražite u članku [Upravitelj resursa za Azure za izradu predložaka](resource-group-authoring-templates.md).
- Da biste saznali više o implementaciji predložak, potražite u članku [uvođenja grupu resursa pomoću predloška Azure Voditelj resursa](resource-group-template-deploy.md)
