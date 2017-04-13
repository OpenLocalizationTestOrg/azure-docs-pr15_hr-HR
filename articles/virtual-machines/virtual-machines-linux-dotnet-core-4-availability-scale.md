<properties
   pageTitle="Dostupnost i skaliranje u predlošcima Azure resursima | Microsoft Azure"
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

# <a name="availability-and-scale-in-azure-resource-manager-templates"></a>Dostupnost i promjena veličine u predlošcima Voditelj resursa za Azure

Dostupnost i skaliranje pogledajte vrijeme aktivnosti i mogućnost da bi odgovarao zahtjev. Ako aplikacija mora biti gore % 99.9 vremena, ga mora imati programa arhitektura koja omogućuje višestrukih Istodobni računalnim resursa. Ako, primjerice, umjesto jednog web-mjesta, konfiguraciji s više razine dostupnosti sadrži više instanci isto mjesto s tehnologije ispred ih za ujednačavanje. U ovoj konfiguraciji jedna instanca aplikacije se može preuzeti prema dolje za održavanje, dok preostalih i dalje funkcionirati. Skaliranje s druge strane se odnosi na aplikacije mogućnost koji će vam poslužiti zahtjev. S opterećenjem Balansirani aplikacija, dodavanjem ili uklanjanjem instance s na resurse omogućuje aplikaciju za promjenu veličine da bi odgovarao zahtjev.

Ovaj dokument detalje o konfiguraciji uzorka implementacije glazbu spremište za dostupnosti i promjena veličine. Istaknute su sve zavisnosti i jedinstvene konfiguracije. Za najbolje mogućnosti rada prije implementacije instance komponente rješenja Azure pretplate i rad uz predloška Azure Voditelj resursa. Dovršavanje predložak Ovdje možete pronaći – [Implementacije spremište glazbu na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="availability-set"></a>Postavljanje dostupnosti

Dostupnost postavljanje logično obuhvaća virtualnim računalima sustava Azure fizičke hosts i druge infrastructural komponente kao što su power zalihama i fizičke mrežni hardver. Dostupnost skupovima osigurati da tijekom održavanja, pogreška uređaja ili drugi put, ne i svih virtualnim strojevima su effected. Dostupnost postavljanje možete dodati predlošku Voditelj resursa Azure pomoću Visual Studio novi resurs Čarobnjak za dodavanje ili umetanje valjani JSON u predložak.

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Postavljanje dostupnosti](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "avalibility-set"
  },
  "properties": {
    "platformUpdateDomainCount": 5,
    "platformFaultDomainCount": 3
  }
}
```

Dostupnost postavljanje deklarirati kao svojstvo resursa virtualnog računala. 

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Postavljanje dostupnosti pridruživanje virtualnog računala](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).


```none
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
Dostupnost postavite kako pogledati s portala za Azure. Ovdje su detaljne svakom virtualnog računala i detalje o konfiguraciji.

![Postavljanje dostupnosti](./media/virtual-machines-linux-dotnet-core/aset.png)

Detaljne informacije o dostupnosti skupa potražite u članku [Upravljanje raspoloživost virtualnih računala](./virtual-machines-linux-manage-availability.md). 

## <a name="network-load-balancer"></a>Mrežni opterećenja

Dok skupa dostupnost nudi odstupanje kvara aplikacije, raspoređivača opterećenja stavlja više instanci aplikacije jedne mreže adresu. Više instanci aplikacije mogu nalaziti na mnogo virtualnim strojevima svaki od njih povezan s raspoređivača opterećenja. Kako pristupiti aplikaciji raspoređivača opterećenja usmjerava dolazni zahtjev preko priložene članova. Raspoređivača opterećenja možete dodati pomoću Visual Studio novi resurs Čarobnjak za dodavanje ili tako da umetnete pravilno oblikovani JSON resursa u predlošku Azure Voditelj resursa.

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Opterećenja mreže](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-front"
  },
  ........<truncated>
}
```

Budući da se primjer aplikacije je izložen putem Interneta s javnu IP adresu, tu adresu povezan je s raspoređivača opterećenja. 

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [mrežom opterećenja pridruživanje javnu IP adresa](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).

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

Na portalu Azure pregled raspoređivača opterećenja mreže prikazuje pridruživanje na javnu IP adresu.

![Mrežni opterećenja](./media/virtual-machines-linux-dotnet-core/nlb.png)

## <a name="load-balancer-rule"></a>Pravilo raspoređivača učitavanja

Prilikom korištenja raspoređivača opterećenja konfigurirane su pravila koja određuju kako se promet raspoređen na svrhu resursi. Spremište glazba aplikacijom uzorka promet stigne na priključak 80 na javnu IP adresu i distribuira preko priključak 80 sve virtualnih računala. 

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Pravilo raspoređivača opterećenja](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).


```none
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

Prikaz pravila raspoređivača opterećenja mreže s portala sustava.

![Pravilo za raspoređivača opterećenja mreže](./media/virtual-machines-linux-dotnet-core/lbrule.png)

## <a name="load-balancer-probe"></a>Probni raspoređivača učitavanja

Raspoređivača opterećenja treba i praćenje svaki virtualnog računala tako da se zahtjevi za poslužena su samo za pokretanje sustava. U ovom nadzor odvija po konstante probing unaprijed definiranih. Uvođenje spremište glazba je konfiguriran za isprobati priključak 80 na sve uključene virtualnim računalima. 

Slijedite vezu da biste vidjeli JSON uzorak u predlošku Voditelj resursa – [Isprobati raspoređivača opterećenja](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).


```none
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

Raspoređivača opterećenja probni vidjeti na portalu Azure.

![Probni raspoređivača opterećenja za mrežu](./media/virtual-machines-linux-dotnet-core/lbprobe.png)

## <a name="inbound-nat-rules"></a>Ulazna pravila NAT

Prilikom korištenja raspoređivača opterećenja pravila morati staviti na mjesto na kojem se omogućuju koje nisu učitavanja Balansirani pristup svaki virtualnog računala. Ako, primjerice, prilikom stvaranja veze s SSH sa svakom virtualnog računala, tog prometa mora biti rasporediti opterećenje, radije konfigurirati unaprijed odredio put. unaprijed odredio putova konfigurirane pomoću programa ulazna pravila NAT resursa. Koristite ovaj resurs, dolazni komunikacije može se mapirati pojedinačne virtualnih računala. 

S aplikacijom glazbu spremište priključak počevši od 5000 mapirani priključak 22 na svakom virtualnog računala za pristup SSH. Na `copyindex()` funkcija koristi da biste povećali ulazni priključak tako da drugi virtualnog računala prima dolazne priključak 5001, treći 5002 i tako dalje.

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Ulazna pravila NAT](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270). 

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'SSH-VM', copyIndex())]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 22,
    "enableFloatingIP": false
  }
}
```

Primjer ulazna pravila NAT kao što se vidi na portalu za Azure. Pravilo SSH NAT stvara se za svaki virtualnog računala implementacije.

![Ulazna pravila NAT](./media/virtual-machines-linux-dotnet-core/natrule.png)

Detaljne informacije o opterećenja Azure mreže potražite u članku [opterećenja za servise Azure infrastrukture](./virtual-machines-linux-load-balance.md).

## <a name="deploy-multiple-vms"></a>Implementacija više VMs

Na kraju, za postavljanje dostupnosti ili opterećenja učinkovito funkcioniranje, više virtualnim strojevima potrebni su. Više VMs može uvesti pomoću funkcije Kopiraj predloška Azure Voditelj resursa. Pomoću funkcije Kopiraj nije potrebno definirati ograničen broj virtualnim strojevima, radije tu vrijednost može se dinamički pružati trenutku implementacije. Funkcija kopiranja troši broj instanci da biste stvorili i držačima za implementaciju proper broj virtualnim strojevima i pridružene resurse.

U predlošku glazbu spremište uzorka parametar definiran koja vas vodi u je broj instanci. Taj broj je koristiti u cijeloj predloška prilikom stvaranja virtualnim strojevima i povezani resursi.

```none
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
}
```

Resurs virtualnog računala petlje Kopiraj izražen je naziv i broj instanci parametar određuje broj primjeraka rezultata.

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Funkcija kopiranja virtualnog računala](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300). 


```none
"apiVersion": "2015-06-15",
"type": "Microsoft.Compute/virtualMachines",
"name": "[concat(variables('vmName'),copyindex())]",
"location": "[resourceGroup().location]",
"copy": {
  "name": "virtualMachineLoop",
  "count": "[parameters('numberOfInstances')]"
}
```

Trenutni iteracije funkcije Kopiraj možete pristupiti s na `copyIndex()` (opis funkcije). Vrijednost funkcije index Kopiraj se može koristiti za imenovanje virtualnim računalima i ostale resurse. Na primjer, ako su dvije instance programa virtualnog računala implementiran, koje su im potrebne različite nazive. Na `copyIndex()` funkcija može se koristiti kao dio naziva virtualnog računala da biste stvorili jedinstveni naziv. Primjer u `copyindex()` funkciju koja se koristi za davanje naziva namjene moguće je vidjeti u resursa virtualnog računala. Ovdje je naziv računala na Ulančavanje na `vmName` parametar i `copyIndex()` (opis funkcije). 

Slijedite vezu da biste vidjeli uzorka JSON unutar predložak Voditelj resursa – [Kopiraj Index (funkcija)](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319). 


```none
"osProfile": {
  "computerName": "[concat(parameters('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "linuxConfiguration": {
    "disablePasswordAuthentication": "true",
    "ssh": {
      "publicKeys": [
        {
          "path": "[variables('sshKeyPath')]",
          "keyData": "[parameters('sshKeyData')]"
        }
      ]
    }
  }
}
```

Na `copyIndex` se koristi funkcija nekoliko puta oglednog predloška glazbu trgovine. Resurse i funkcije korištenja `copyIndex` obuhvaćaju ništa specifične za instancu virtualnog računala kao što su pravila raspoređivača opterećenja, mrežno sučelje i bilo koji ovisi o funkcijama. 

Dodatne informacije o funkcija kopiranja potražite u članku [Stvaranje više instanci resursa u Azure Voditelj resursa](../resource-group-create-multiple.md).

## <a name="next-step"></a>Sljedeći korak

<hr>

[Korak 4 - implementaciju aplikacije s predlošcima Azure Voditelj resursa](./virtual-machines-linux-dotnet-core-5-app-deployment.md)