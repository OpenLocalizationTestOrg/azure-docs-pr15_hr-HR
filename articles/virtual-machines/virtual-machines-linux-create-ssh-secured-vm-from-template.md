<properties
    pageTitle="Stvaranje Linux VM pomoću predloška za Azure | Microsoft Azure"
    description="Stvaranje Linux VM na Azure pomoću predloška Azure Voditelj resursa."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="v-livech"/>

# <a name="create-a-linux-vm-using-an-azure-template"></a>Stvaranje Linux VM pomoću predloška za Azure

U ovom se članku objašnjava brzo uvođenje Linux virtualnog računala na Azure pomoću predloška Azure.  U članku zahtijeva:

- Azure račun ([dobiti besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure EŽA](../xplat-cli-install.md) prijavili `azure login`.

- način za Azure Voditelj resursa Azure EŽA _mora biti u_ `azure config mode arm`.

Možete brzo uvođenje predloška Linux VM pomoću [portala za Azure](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-command-summary"></a>Naredba brzi sažetak

```bash
azure group create \
-n myResourceGroup \
-l westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Detaljni vodič

Predlošci omogućuju vam da biste stvorili VMs na Azure s postavke koje želite prilagoditi vrijeme pokretanje, postavke kao što su korisnička imena i hostnames. Za ovaj članak ne možemo su pokretanje korištenja programa VM Ubuntu zajedno s mreže sigurnosne grupe (NSG) predloška Azure s priključak 22 Otvori za SSH.

Azure resursima Predlošci su JSON datoteka koje je moguće koristiti za jednostavne jednokratni zadatke poput pokretanje programa VM Ubuntu kao dovršen u ovom članku.  Azure predlošci mogu se koristiti i za sastavljanje složene Azure konfiguracije cijelu okruženja kao što su implementacije hrpu testiranja, razvojni ili radni.

## <a name="create-the-linux-vm"></a>Stvaranje Linux VM

Sljedeći primjer koda prikazuje kako poziva `azure group create` da biste stvorili grupu resursa i implementacija sustava VM SSH osigurani Linux u isto vrijeme pomoću [ovog predloška Azure Voditelj resursa](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Imajte na umu da u svoje primjeru, morate koristiti nazive koji su jedinstveni za vaše okruženje. U ovom se primjeru koristi `myResourceGroup` kao naziv grupe resursa i `myVM` nazivu VM.

```bash
azure group create \
--name myResourceGroup \
--location westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Izlaz trebao izgledati kao sljedećeg bloka izlaz:

```bash
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Taj se primjer implementiran VM pomoću na `--template-uri` parametar.  Možete preuzeti ili stvorite predložak lokalno i prenesite pomoću predloška na `--template-file` parametar s put do datoteke predloška kao argument. Azure EŽA traži parametara potrebnih predložak.

## <a name="next-steps"></a>Daljnji koraci

Pretražite [galeriju predložaka](https://azure.microsoft.com/documentation/templates/) da biste otkrili koje aplikacije okviri za implementaciju dalje.
