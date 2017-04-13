
<properties
   pageTitle="Stvaranje okruženja Linux dovršeno pomoću EŽA Azure | Microsoft Azure"
   description="Stvaranje prostora za pohranu, Linux VM, virtualne mreže i podmreže, raspoređivača opterećenja, na NIC, javnu IP i sigurnosne grupe mreže, sve od dna gore pomoću EŽA Azure."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-complete-linux-environment-by-using-the-azure-cli"></a>Stvaranje okruženja za dovršavanje Linux pomoću EŽA Azure

U ovom se članku bismo stvoriti jednostavan mreže s alatima raspoređivača opterećenja i par VMs koje su korisne za razvoj i jednostavan rad. Vodit ćemo kroz postupak naredba po naredba dok ne dobijete dva radi, sigurne Linux VMs na koji možete povezati s bilo kojeg mjesta na Internetu. Zatim možete premještati na složenije mreže i okruženja.

Usput, koje Saznajte više o hijerarhiji ovisnost model implementacije Voditelj resursa daje i o tome koliko power je sadrži. Kada vam se prikazuje kako se temelji na sustavu, koje možete ponovno stvaranje je mnogo brže pomoću [predložaka Azure Voditelj resursa](../resource-group-authoring-templates.md). Osim toga, nakon saznat ćete kako dijelove vaše okruženje usklađuju, stvaranje predložaka za automatizaciju ih postaje lakše.

Okruženje sadrži:

- Dva VMs unutar skupa dostupnost.
- Opterećenja pomoću pravila za ujednačavanje opterećenja na priključak 80.
- Mrežni sigurnosnih grupa (NSG) pravila radi zaštite vaše VM od neželjenih promet.

![Pregled osnovnih okruženje](./media/virtual-machines-linux-create-cli-complete/environment_overview.png)

Da biste stvorili okruženje za prilagođene, morate najnovije [Azure EŽA](../xplat-cli-install.md) u načinu Voditelj resursa (`azure config mode arm`). Morate JSON Raščlanjivanje alata. U ovom se primjeru koristi [jq](https://stedolan.github.io/jq/).

## <a name="quick-commands"></a>Brzi naredbe
Ako morate brzo obaviti zadatak, sljedeće sekcije logaritma naredbe da biste prenijeli na VM Azure. Detaljnije informacije i kontekst za svaki korak pronaći ćete u ostatku dokumenta, početni [ovdje](#detailed-walkthrough).

Provjerite jeste li povezani s [EŽA Azure](../xplat-cli-install.md) prijavljen i koristite način upravljanja resursima:

```bash
azure config mode arm
```

U sljedećim primjerima zamijenite primjer Nazivi parametara vlastitih vrijednosti. Nazivi parametara primjeru sadrže `myResourceGroup`, `mystorageaccount`, i `myVM`.

Stvorite grupu resursa. Sljedeći primjer stvara grupu resursa pod nazivom `myResourceGroup` u na `westeurope` mjesto:

```bash
azure group create -n myResourceGroup -l westeurope
```

Potvrda grupu resursa pomoću JSON analizatora:

```bash
azure group show myResourceGroup --json | jq '.'
```

Stvaranje računa za pohranu. Sljedeći primjer stvara za pohranu račun pod nazivom `mystorageaccount` (naziv računa spremišta mora biti jedinstvena, pa navedite vlastitu jedinstveni naziv):

```bash
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Provjerite račun za pohranu pomoću JSON analizatora:

```bash
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Stvaranje virtualne mreže. Sljedeći primjer stvara virtualne mreže pod nazivom `myVnet`:

```bash
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Stvaranje podmreži. Sljedeći primjer stvara podmreže pod nazivom `mySubnet`"

```bash
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Provjerite je li virtualne mreže i podmreže pomoću JSON analizatora:


```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Stvaranje javne IP. Sljedeći primjer stvara javnu IP pod nazivom `myPublicIP` s nazivom DNS `mypublicdns` (DNS naziv mora biti jedinstvena, pa navedite vlastitu jedinstveni naziv):

```bash
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Stvaranje raspoređivača opterećenja. Sljedeći primjer stvara raspoređivača opterećenja pod nazivom `myLoadBalancer`:

```bash
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Stvaranje pristupne Skupna IP za raspoređivača opterećenja i pridruživanje javnu IP. Sljedeći primjer stvara sučelja Skupna IP pod nazivom `mySubnetPool`:

```bash
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool 
```

Stvaranje skupna pozadinsku IP za raspoređivača opterećenja. Sljedeći primjer stvara na skupna pozadinsku IP pod nazivom `myBackEndPool`:

```bash
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Stvorite SSH ulaznog mreže adresu prevođenje (NAT) pravila za raspoređivača opterećenja. Sljedeći primjer stvara dva pravila raspoređivača opterećenja `myLoadBalancerRuleSSH1` i `myLoadBalancerRuleSSH2`:

```bash
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Stvaranje weba ulazna pravila NAT raspoređivača opterećenja. Sljedeći primjer stvara pravilo raspoređivača opterećenja pod nazivom`myLoadBalancerRuleWeb`

```bash
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Stvorite probni stanja za raspoređivača opterećenja. Sljedeći primjer stvara TCP probni pod nazivom `myHealthProbe`:

```bash
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Provjerite je li opterećenja, IP grupe i NAT pravila pomoću JSON analizatora:

```bash
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Stvaranje prve kartice mreža sučelja (NIC). Zamjena na `#####-###-###` sekcije koristeći Azure pretplate ID. Vaša pretplata ID je naznačeno u izlaz `jq` kada Provjera resursa koje stvarate. Možete pogledati i identifikacijskog Broja za pretplatu s `azure account list`. 

Sljedeći primjer stvara NIC pod nazivom `myNic1`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Stvaranje drugog NIC. Sljedeći primjer stvara NIC pod nazivom `myNic2`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Potvrda dvije mrežne kartice pomoću JSON analizatora:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Stvaranje sigurnosne grupe mreže. Sljedeći primjer stvara sigurnosnu grupu mreže pod nazivom `myNetworkSecurityGroup`:

```bash
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Dodajte dva ulazna pravila za mrežni sigurnosne grupe. Sljedeći primjer stvara dva pravila `myNetworkSecurityGroupRuleSSH` i `myNetworkSecurityGroupRuleHTTP`:

```bash
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Provjerite je li mrežni sigurnosne grupe i ulazna pravila pomoću JSON analizatora:

```bash
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Sigurnosne grupe mreže povezati dva mrežne kartice:

```bash
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Stvaranje skupa dostupnost. Sljedeći primjer stvara raspoloživost postavljanje imenovani `myAvailabilitySet`:

```bash
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

Stvaranje prve VM Linux. Sljedeći primjer stvara VM pod nazivom `myVM1`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Stvaranje drugog VM Linux. Sljedeći primjer stvara VM pod nazivom `myVM2`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Korištenje JSON analizatora da biste potvrdili da sve koja je ugrađena:

```bash
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Izvoz novo okruženje predložak da biste brzo ponovno stvoriti nove instance:

```bash
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Detaljni vodič
Detaljne korake koji slijede objašnjavaju što svaku naredbu radi kao izgradnje vaše okruženje. Koncepte su korisne prilikom stvaranja vlastitog prilagođenog okruženja za razvoj ili radni.

Provjerite jeste li povezani s [EŽA Azure](../xplat-cli-install.md) prijavljen i koristite način upravljanja resursima:

```bash
azure config mode arm
```

U sljedećim primjerima zamijenite primjer Nazivi parametara vlastitih vrijednosti. Nazivi parametara primjeru sadrže `myResourceGroup`, `mystorageaccount`, i `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Stvaranje grupe resursa, a zatim odaberite mjesta implementacije

Grupa Azure resursa su entiteti logičke implementacije koje sadrže podatke o konfiguraciji i metapodataka da biste omogućili logičke upravljanje implementacijama resursa. Sljedeći primjer stvara grupu resursa pod nazivom `myResourceGroup` u na `westeurope` mjesto:

```bash
azure group create --name myResourceGroup --location westeurope
```

Izlaz:

```bash                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Stvaranje računa za pohranu

Računi za pohranu potrebno za vaše VM diskova te za sve diskova dodatne podatke koje želite dodati. Stvaranje računa za pohranu gotovo odmah nakon stvaranja grupe resursa.

Ovdje se koristi u `azure storage account create` naredbe prosljeđivanje mjesto računa, grupa resursa koji određuje, a vrsta podrške za pohranu koji želite. Sljedeći primjer stvara za pohranu račun pod nazivom `mystorageaccount`:

```bash
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Izlaz:

```bash
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Da biste pregledali naš grupa resursa pomoću na `azure group show` naredbu, recimo pomoću alata [jq](https://stedolan.github.io/jq/) s na `--json` Azure EŽA mogućnost. (Možete koristiti **jsawk** ili bilo koje biblioteke jezik koji želite analizirati na JSON.)

```bash
azure group show myResourceGroup --json | jq '.'
```

Izlaz:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Da biste istražili račun za pohranu pomoću na EŽA, najprije morate postaviti naziva računa i tipki. Zamijenite naziv koji odaberete naziv računa za pohranu u sljedećem primjeru:

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Možete prikazati podatke za pohranu jednostavno:

```bash
azure storage container list
```

Izlaz:

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Stvaranje virtualne mreže i podmreže

Potom ćete morati stvoriti virtualne mreže radi u Azure i podmreže u kojem stvarate vaše VMs. Sljedeći primjer stvara virtualne mreže pod nazivom `myVnet` s na `192.168.0.0/16` adresa prefiks:

```bash
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16 
```

Izlaz:

```bash
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Ponovno, recimo koristite mogućnost – json `azure group show` i **jq** da biste vidjeli kako se stvara naš resursi. Sada je `storageAccounts` resursa, a `virtualNetworks` resursa.  

```bash
azure group show myResourceGroup --json | jq '.'
```

Izlaz:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Sada ćemo stvoriti podmreže u na `myVnet` virtualne mreže u kojima su implementiran na VMs. Koristimo na `azure network vnet subnet create` naredbe, zajedno s resursima smo ste već stvorili: u `myResourceGroup` grupa resursa i `myVnet` virtualne mreže. U sljedećem primjeru, dodat ćemo podmreže pod nazivom `mySubnet` s adresa predbroj od `192.168.1.0/24`:

```bash
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Izlaz:

```bash
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Budući da podmreži logično unutar virtualne mreže, potražite ćemo informacije podmreže naredbom malo drugačije. Naredba koristimo je `azure network vnet show`, ali ne možemo nastaviti da biste pregledali izlaz JSON pomoću **jq**.

```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Izlaz:

```bash
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address-pip"></a>Stvaranje javne IP adrese (TOČAKA)

Sada ćemo stvoriti javnu IP adresu (TOČAKA) koje ćemo dodijeliti raspoređivača opterećenja. Omogućuje povezivanje s vašeg VMs s interneta pomoću na `azure network public-ip create` naredbe. Budući da zadane adrese dinamički, ćemo stvoriti imenovane stavke za DNS u domeni **cloudapp.azure.com** pomoću na `--domain-name-label` mogućnost. Sljedeći primjer stvara javnu IP pod nazivom `myPublicIP` s nazivom DNS `mypublicdns`. Kao što je DNS naziv mora biti jedinstvena, unesite vlastiti jedinstveni naziv DNS:

```bash
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Izlaz:

```bash
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Na javnu IP adresu ujedno najviše razine resursa, da biste ga `azure group show`.

```bash
azure group show myResourceGroup --json | jq '.'
```

Izlaz:

```bash
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Možete ispitati dodatne detalje o resursa, uključujući na potpuno kvalificirani naziv domene (FQDN) poddomenu, pomoću potpunu `azure network public-ip show` naredbe. Javne IP adresa resurs je logički dodijeliti, ali određene adrese još nije dodijeljena. Da biste dobili IP adresu, namjeravate potrebno raspoređivača opterećenja koji smo još niste stvorili.

```bash
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Izlaz:

```bash
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Stvaranje raspoređivača opterećenja i IP grupe
Kada stvorite raspoređivača opterećenja, omogućuje vam da raspodijelite promet više VMs. Također nudi zalihosti u aplikaciji tako da pokrenete više VMs koji odgovora na zahtjeve za korisnika u slučaju održavanja ili podebljanom opterećenje. Sljedeći primjer stvara raspoređivača opterećenja pod nazivom `myLoadBalancer`:

```bash
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Izlaz:

```bash
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Naš opterećenja je prilično prazna, možemo stvoriti neke grupe IP. Želimo da biste stvorili dva IP grupe za naše opterećenja – jedan za sučelje i jedan za pozadinska. Javno vidljiv je sučelja Skupna IP. Preporučuje se i mjesto na koje ćemo dodijeliti TOČAKA koji smo stvorili ranije. Zatim koristimo skup pozadinske mjesto za naše VMs povezati. Na taj način promet možete tijeka kroz opterećenja da biste na VMs.

Najprije stvaranje naš sučelja Skupna IP. Sljedeći primjer stvara sučelja skup pod nazivom `myFrontEndPool`:

```bash
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool 
```

Izlaz:

```bash
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Imajte na umu kako koristi u `--public-ip-name` da biste prešli na prenesite na `myPublicIP` koji smo stvorili ranije. Dodjeljivanje na javnu IP adresu raspoređivača opterećenja omogućuje vam da dođete do vaše VMs putem Interneta.

Nakon toga stvaranje našem skupna drugi IP ovaj put za naše pozadinske promet. Sljedeći primjer stvara pozadinske skup pod nazivom `myBackEndPool`:

```bash
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Izlaz:

```bash
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Možemo vidjeti kako će se naš opterećenja način zatrebaju s `azure network lb show` i provjera JSON izlaz:

```bash
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Izlaz:

```bash
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Stvaranje pravila za NAT opterećenja
Da biste dobili promet slijedi kroz naš opterećenja, ne možemo potrebnih za stvaranje mrežne adrese prevođenje (NAT) pravila koja se navode dolazne i odlazne akcije. Možete navesti protokol za korištenje, a zatim mapiranje priključke u vanjskim interne priključke po želji. Za naše okruženje stvaranje neka pravila koja omogućuju SSH kroz naš opterećenja za naše VMs. Ne možemo postaviti TCP priključci 4222 i 4223 usmjeriti TCP priključak 22 na našem VMs (koje ćemo stvoriti kasnije). Sljedeći primjer stvara pravilo pod nazivom `myLoadBalancerRuleSSH1` da biste mapirali TCP priključak 4222 priključak 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Izlaz:

```bash
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Ponovite postupak za drugi NAT pravilo za SSH. Sljedeći primjer stvara pravilo pod nazivom `myLoadBalancerRuleSSH2` da biste mapirali TCP priključak 4223 priključak 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Pogledajmo nastaviti i stvorite pravilo NAT za priključak TCP 80 promet web hooking pravilo na našem IP grupe. Ako ne možemo priključiti pravilo Skupna IP umjesto pojedinačno, hooking gore pravilo naše VMs ćemo možete dodati ili ukloniti VMs iz Skupna IP. Raspoređivača opterećenja automatski prilagođava tijek promet. Sljedeći primjer stvara pravilo pod nazivom `myLoadBalancerRuleWeb` da biste mapirali TCP priključak 80 priključak 80:

```bash
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Izlaz:

```bash
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Stvaranje stanja probni opterećenja za učitavanje

Na stanje isprobati povremeno provjerava na VMs koje se nalaze iza naš raspoređivača opterećenja da biste bili sigurni ste operacijskom i odgovaranje na zahtjeve za onako kako su definirana. Ako nije, oni se uklanjaju iz postupak da biste bili sigurni da korisnici ne se usmjereni na ih. Možete definirati prilagođene provjere za probni stanja, zajedno s vremenskim razmacima i vremenskog ograničenja vrijednosti. Dodatne informacije o stanju probes potražite u članku [probes opterećenja](../load-balancer/load-balancer-custom-probe-overview.md). Sljedeći primjer stvara na TCP stanja Ispitani imenovani `myHealthProbe`:

```bash
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Izlaz:

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Ovdje, ne možemo navedeni intervala od 15 sekundi za naše provjere stanja. Ne možemo možete propustiti maksimalno četiri probes (jedne minute) prije raspoređivača opterećenja smatra da je na računalo koje hostira više ne funkcionira.

## <a name="verify-the-load-balancer"></a>Provjerite je li raspoređivača opterećenja
Sada je gotova konfiguracija raspoređivača opterećenja. Evo nekoliko koraka snimljene:

1. Prvo stvoriti raspoređivača opterećenja.
2. Zatim stvorili sučelja Skupna IP i dodijeliti javnu IP.
3. Stvorite Skupna IP pozadinske koje se mogu povezati VMs dalje.
4. Nakon toga stvoriti NAT pravila koja omogućuju SSH za VMs za upravljanje uz pravilo koje omogućuje TCP priključak 80 za naše web-aplikacije.
5. Na kraju dodaje probni stanja da biste povremeno na VMs. Ovaj probni stanja osigurava da korisnici ne pokušaju pristupiti VM koja se više ne funkcionira ili posluživanje sadržaj.

Pogledajmo kako raspoređivača opterećenja izgleda odmah:

```bash
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Izlaz:

```bash
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Stvaranje programa NIC za uporabu Linux VM

NIC-ovi programski dostupni su jer je moguća Primjena pravila za uporabu. Također može imati više od jedne. U nastavku `azure network nic create` naredba priključiti NIC na resurse za učitavanje pozadinske IP i povezati je s NAT pravilo tako da dopušta promet SSH.
 
Zamjena na `#####-###-###` sekcije koristeći Azure pretplate ID. Vaša pretplata ID je naznačeno u izlaz `jq` kada Provjera resursa koje stvarate. Možete pogledati i identifikacijskog Broja za pretplatu s `azure account list`. 

Sljedeći primjer stvara NIC pod nazivom `myNic1`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Izlaz:

```bash
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Možete vidjeti detalje tako izravno Provjera resurs. Pregledajte resursa pomoću na `azure network nic show` naredba:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Izlaz:

```bash
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Sada ćemo stvoriti drugi NIC ponovno hooking u našem skupna pozadinsku IP. Tim se pravilom NAT vrijeme drugi dopušta promet SSH. Sljedeći primjer stvara NIC pod nazivom `myNic2`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Stvaranje sigurnosne grupe mreže i pravila

Sada ćemo stvoriti sigurnosnu grupu mreže i ulazna pravila koja upravljaju pristup na NIC. Sigurnosne grupe mreže primjenjuje se na NIC ili podmreži. Definirajte pravila da biste odredili tijek prometa i vaše VMs. Sljedeći primjer stvara sigurnosnu grupu mreže pod nazivom `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Dodat ćemo ulazna pravila za NSG da dopušta unutarnje veze na priključak 22 (za podršku SSH). Sljedeći primjer stvara pravilo pod nazivom `myNetworkSecurityGroupRuleSSH` da biste omogućili TCP priključak 22:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Sada ćemo Dodavanje ulaznog pravila za NSG da dopušta unutarnje veze na priključak 80 (za podršku web promet). Sljedeći primjer stvara pravilo pod nazivom `myNetworkSecurityGroupRuleHTTP` da biste omogućili priključak TCP 80:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [AZURE.NOTE] Ulazna pravila je filtar za unutarnje mrežne veze. U ovom primjeru smo povezati s NSG NIC virtualne VMs, što znači da neki zahtjev za priključak 22 se prenosi putem NIC-a na našem VM. Dolazni pravilo o mrežne veze, a ne o krajnje, koji što bi o u klasični implementacijama. Da biste otvorili priključak, morate ostaviti na `--source-port-range` postavljeno na "\*" (Zadana vrijednost) da biste prihvatili dolazni zahtjevi **bilo koji** zahtijeva priključak. Priključci su obično dinamičke.

## <a name="bind-to-the-nic"></a>Povezivanje s NIC-a

Povezivanje s NSG na NIC-ovi. Moramo povezivanje naš NIC-ovi s naše mreže sigurnosne grupe. Pokrenite obje naredbe za oba naš NIC-ovi priključiti:

```bash
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```bash
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Stvaranje skupa dostupnosti
Dostupnost postavlja Podjela pomoć na VMs preko kvara domene i nadogradnje domene. Stvaranje raspoloživost postavljena za vaš VMs. Sljedeći primjer stvara raspoloživost postavljanje imenovani `myAvailabilitySet`:

```bash
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Domena kvara definirali grupiranje virtualnih računala zajedničko korištenje uobičajenih power izvora i mrežne skretnice. Prema zadanim postavkama virtualnih računala koja su konfigurirana u vašem skupu dostupnost razdjelnik svim do tri kvara domenama. Ideja je problem hardvera u neku od tih domena kvara utječe svaki VM sa sustavom aplikacije. Azure automatski distribuira VMs svim domenama kvara kada da ih stavite u skupu dostupnost.

Nadogradnja domene označavaju grupa virtualnim strojevima i temeljni fizičke hardver koji možete ponovno pokrenuti u isto vrijeme. Redoslijed kojim su nadogradnje domene sustava možda neće biti uzastopnih tijekom planiranog održavanja, ali je samo jednu nadogradnju sustava odjednom. Ponovno Azure automatski distribuira vaše VMs svim nadogradnje domenama prilikom upućivanja ih u web-mjesto sustava dostupnost.

Dodatne informacije o [upravljanju dostupnost VMs](./virtual-machines-linux-manage-availability.md).

## <a name="create-the-linux-vms"></a>Stvaranje Linux VMs

Prostor za pohranu i mrežne resurse za podršku Internet pristupiti VMs ste stvorili. Sada stvorite one VMs i sigurne ih pomoću ključa SSH koji nema lozinku. U ovom slučaju ne možemo ćete stvoriti Ubuntu VM koji se temelji na zadnjoj LTS. Ne možemo pronađite te podatke slike pomoću `azure vm image list`, kao što je opisano u [pronalaženju Azure VM slike](virtual-machines-linux-cli-ps-findimage.md).

Ne možemo odabrali sliku pomoću naredbe `azure vm image list westeurope canonical | grep LTS`. U ovom slučaju koristimo `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Za posljednje polje ćemo proslijediti `latest` tako da se u budućnosti da uvijek se koristiti najnoviju verziju. (Niz koristimo je `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

U ovom sljedeći je korak poznatih svakome tko ustanova već stvorila je ssh rsa javnim i privatnim ključ uparivanje na Linux ili Mac pomoću **rsa -t ssh-keygen-b 2048**. Ako nemate sve parove ključa certifikat vašem `~/.ssh` direktorija, možete ih stvoriti:

- Automatski pomoću na `azure vm create --generate-ssh-keys` mogućnost.
- Ručno, pomoću [upute da biste ih stvarati sami](virtual-machines-linux-mac-create-ssh-keys.md).

Umjesto toga možete upotrijebiti u `--admin-password` način za provjeru autentičnosti veze SSH nakon stvaranja na VM. Ta je metoda obično manje sigurna.

Ćemo stvoriti na VM tako da naš resurse i informacije s together u `azure vm create` naredba:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Izlaz:

```bash
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Možete povezati svoje VM odmah pomoću SSH ključeva zadani. Provjerite je li navesti s odgovarajućim priključkom jer smo si prolaska kroz raspoređivača opterećenja. (Za naše prvi VM ne možemo postaviti NAT pravilo da biste proslijedili priključak 4222 naš VM):

```bash
 ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Izlaz:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Nastaviti i stvorite na drugi VM na isti način:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

A sada možete koristiti u `azure vm show myResourceGroup myVM1` naredbu da biste pregledali ste stvorili. Sada koristite vaše VMs Ubuntu iza raspoređivača opterećenja servisu Azure koje možete se prijaviti u samo s vašeg SSH ključa par (jer su onemogućene lozinke). Možete instalirati nginx ili httpd, implementacija web-aplikacijama i vidjeti promet protok kroz opterećenja i na VMs.

```bash
azure vm show --resource-group myResourceGroup --name myVM1
```

Izlaz:

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>Izvoz u okruženju kao predloška
Sad kad ste ugrađena out ovo okruženje, što se događa ako želite stvoriti dodatne platforme s istim parametrima ili radnog okruženja koja odgovara ga? Voditelj resursa koristi JSON predloške koje definiraju sve parametre okruženju sustava. Upućivanjem ovaj predložak JSON izgradnje cijelu okruženja. Možete [ručno sastavljanje predložaka JSON](../resource-group-authoring-templates.md) i izvoz okruženje da biste stvorili predložak JSON umjesto vas:

```bash
azure group export --name myResourceGroup
```

Ta naredba stvara na `myResourceGroup.json` datoteke u trenutnom radnom direktoriju. Prilikom stvaranja okruženja iz ovog predloška, od vas će se Traži sve nazive resursa, zajedno s imenima opterećenja, sučelje mreže ili VMs. Popunjavanje te nazive u datoteku predloška tako da dodate u `-p` ili `--includeParameterDefaultValue` parametar u `azure group export` naredba koji je prikazan neke starije verzije. Uređivanje predloška JSON da biste odredili nazive resursa ili [stvoriti datoteku parameters.json](../resource-group-authoring-templates.md#parameters) koji određuje nazive resursa.

Da biste stvorili okruženju iz predloška:

```bash
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Možda ćete morati pročitajte [više o implementaciji iz predložaka sustava](../resource-group-template-deploy-cli.md). Saznajte kako postupno ažuriranje okruženja, korištenje parametara datoteke i pristupiti predložaka iz na jednom mjestu.

## <a name="next-steps"></a>Daljnji koraci

Sada ste spremni za početak rada s više mreža komponente i VMs. Možete koristiti okruženje za uzorak izgradnje aplikacije pomoću komponente core uvedene u nastavku.
