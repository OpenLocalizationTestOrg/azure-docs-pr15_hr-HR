<properties
   pageTitle="Korištenje nastavka Azure Docker VM | Microsoft Azure"
   description="Saznajte kako koristiti proširenje Docker VM brzo i sigurno uvođenje okruženju Docker u Azure pomoću predložaka Voditelj resursa."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/25/2016"
   ms.author="iainfou"/>

# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Stvaranje okruženja Docker u Azure pomoću Docker VM datotečni nastavak
Docker je upravljanje popularne spremnik i slike platformu koja omogućuje da brzo radite s spremnika Linux (i kao i Windows). U Azure, postoje različite načine možete implementirati Docker prema svojim potrebama. U ovom se članku usredotočuje se na pomoću proširenje Docker VM i Voditelj resursa Azure predložaka. 

Dodatne informacije o u različitim načinima, uključujući korištenje Docker računala i servisa Azure spremnik Services potražite u sljedećim člancima:

- Da biste brzo predložak aplikacije, možete stvoriti jednu Docker glavno računalo pomoću [Docker računala](./virtual-machines-linux-docker-machine.md).
- Za veće, više stabilan okruženja, možete koristiti proširenje Azure Docker VM koji podržava [Docker sastavite](https://docs.docker.com/compose/overview/) da biste generirali dosljedan spremnik implementacije. U ovom se članku Detalji o korištenje nastavka Azure Docker VM.
- Da biste sastavili proizvodnje – Priprema, skalabilni okruženja kojima ćete pronaći dodatne alate za planiranje i upravljanje njima, možete implementirati [Docker Swarm klaster Azure spremnik Services](../container-service/container-service-deployment.md).


## <a name="azure-docker-vm-extension-overview"></a>Pregled proširenja za Azure Docker VM
Proširenje Azure Docker VM instalira i konfigurira Docker daemon, Docker klijenta i Docker sastavite u Linux virtualnog računala (VM). Pomoću proširenje Azure Docker VM imate više kontrola i značajke od jednostavno pomoću Docker računalu ili stvaranje Docker glavnog računala. Dodatne značajke, kao što su [Docker sastavite](https://docs.docker.com/compose/overview/), provjerite nastavak Azure Docker VM najprikladniji za Robusniji programiranje ili radnog okruženja.

Azure resursima predlošci definiraju cijelu strukturu vaše okruženje. Predlošci omogućuju vam omogućuje stvaranje i konfiguriranje resurse kao što su voditelju Docker VMs, za pohranu, na temelju uloga pristup kontrolama (RBAC) i dijagnostici. Ponovne upotrebe te predloške da biste stvorili dodatne implementacijama dosljedan način. Dodatne informacije o Voditelj resursa Azure i predložaka potražite u članku [Pregled upravitelja resursa](../azure-resource-manager/resource-group-overview.md). 


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Uvođenje predloška s nastavkom Azure Docker VM
Pogledajmo pomoću postojećeg predloška brzi početak rada da biste stvorili programa Ubuntu VM koja koristi datotečni nastavak Azure Docker VM instaliranja i konfiguriranja Docker glavno računalo. Možete pogledati u predložak: [jednostavne implementaciju sustava VM Ubuntu s Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Potreban vam je [Najnovija Azure EŽA](../xplat-cli-install.md) instaliran i prijavljeni pomoću MOD upravitelja resursa na sljedeći način:

```
azure config mode arm
```

Uvođenje predloška pomoću EŽA Azure, pri određivanju predložak URI. Sljedeći primjer stvara grupu resursa pod nazivom `myResourceGroup` u na `WestUS` mjesto. Koristite vlastiti naziv grupe resursa i mjesto na sljedeći način:

```
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Odgovoriti na upute za naziv računa za pohranu, korisničko ime i lozinku i navedite naziv DNS-a. Rezultat je slično kao u sljedećem primjeru:

```
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: ops
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicip
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK

```

Vraća Azure EŽA upit nakon samo nekoliko sekundi, ali Docker usluge-a i dalje stvorili i konfigurirao Azure Docker VM nastavak. U svega nekoliko minuta za implementaciju da biste završili. Možete pogledati detalje o korištenju status Docker glavnog računala na `azure vm show` naredbe.

Sljedeći primjer provjerava status VM pod nazivom `myDockerVM` (zadani naziv predloška - nemojte mijenjati naziv) u grupi resursa pod nazivom `myResourceGroup`. Unesite naziv grupe resursa koji ste stvorili u prethodnom koraku:

```bash
azure vm show -g myResourceGroup -n myDockerVM
```

Izlaz iz na `azure vm show` naredba je slično kao u sljedećem primjeru:

```
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicip.westus.cloudapp.azure.com]
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

Pri vrhu Izlaz, pogledajte u `ProvisioningState` od na VM. Kada prikazat će se `Succeeded`, uvođenje je završio, pa ćete moći SSH da biste na VM.

Pri kraju Izlaz, `FQDN` prikazuje na potpuno kvalificirani naziv domene vaše Docker glavnog računala. U ovom FQDN je ono što vi koristite SSH Docker glavno računalo u preostale korake.


## <a name="deploy-your-first-nginx-container"></a>Implementacija sustava prvome spremniku nginx
Dovršetku implementaciju, SSH nove Docker glavno računalo s lokalnog računala. Unesite svoje korisničko ime i FQDN na sljedeći način:

```bash
ssh ops@mypublicip.westus.cloudapp.azure.com
```

Kada prijavljen na glavno računalo za Docker, recimo pokrenite programa nginx spremnik:

```
sudo docker run -d -p 80:80 nginx
```

Rezultat je slično kao u sljedećem primjeru kao što je slika nginx se preuzima i kontejnera s radom:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Provjera stanja spremnika koji se izvode na vašem računalu koje hostira Docker na sljedeći način:

```
sudo docker ps
```

Rezultat je slično kao u sljedećem primjeru, pokazuje da spremnik nginx je pokrenut TCP priključci 80 i 443 i prosljeđivanje:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Da biste vidjeli svoje spremnika u akciji, otvorite web-preglednik i unesite naziv FQDN vaše Docker glavnog računala:

![Tekući ngnix spremnik](./media/virtual-machines-linux-dockerextension/nginxrunning.png)


## <a name="azure-docker-vm-extension-template-reference"></a>Azure Docker VM proširenje predložak reference
U prethodnom primjeru koristi postojećeg predloška brzi početak rada. Možete uvesti i nastavak Azure Docker VM s resursima predloške. Da biste to učinili, dodajte sljedeće predloške Voditelj resursa koji definira na `vmName` od vašeg VM komponenta:

```
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Možete pronaći detaljnije vodič o korištenju predložaka Voditelj resursa tako da pročitate [Pregled Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md).


## <a name="next-steps"></a>Daljnji koraci
Možda želite [konfigurirati daemon Docker TCP priključak](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), razumijevanje [Docker sigurnosti](https://docs.docker.com/engine/security/security/)ili uvođenje spremnika pomoću [Docker sastavite](https://docs.docker.com/compose/overview/). Dodatne informacije o proširenje VM Azure Docker sam potražite u članku [GitHub projekta](https://github.com/Azure/azure-docker-extension/).

Pročitajte više o dodatne mogućnosti implementacije Docker za Azure:

- [Pomoću računala Docker upravljački program za Azure](./virtual-machines-linux-docker-machine.md)  
- [Početak rada s web-mjesto Docker i u okvir za sastavljanje definiranje i izvođenje više spremniku na Azure virtualnog računala](virtual-machines-linux-docker-compose-quickstart.md).
- [Implementacija programa servisa Azure spremnik klaster](../container-service/container-service-deployment.md)
