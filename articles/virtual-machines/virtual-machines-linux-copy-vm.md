<properties
    pageTitle="Stvaranje kopije vaše VM Linux Azure | Microsoft Azure"
    description="Saznajte kako stvoriti kopiju vašeg Azure Linux virtualnog računala u modelu implementacije Voditelj resursa"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>

# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure"></a>Stvaranje kopije virtualni stroj Linux sustavom Azure


U ovom se članku objašnjava da biste stvorili kopiju Azure virtualnog računala (VM) radi Linux pomoću modela implementacije Voditelj resursa. Najprije kopiranje putem operacijski sustav i diskova podataka u novi spremnik, a zatim postavite mrežni resursi i stvaranje novog virtualnog računala.

Možete i [prijenos i stvaranje VM iz slike prilagođene disk](virtual-machines-linux-upload-vhd.md).


## <a name="before-you-begin"></a>Prije početka

Provjerite zadovoljavate sljedeće preduvjete prije nego što počnete korake:

- Imate [Azure EŽA] (.. / xplat eža install.md) preuzeli i instalirali na vašem računalu. 

- Potrebne neke informacije o vaše postojeće VM Azure Linux:

| Informacije o izvoru VM | Gdje ga nabaviti |
|------------|-----------------|
| Naziv VM | `azure vm list` |
| Naziv grupe resursa | `azure vm list` |
| Mjesto | `azure vm list` |
| Naziv računa spremišta | `azure storage account list -g <resourceGroup>` |
| Naziv spremnika | `azure storage container list -a <sourcestorageaccountname>` |
| Naziv izvorišne VM VHD datoteke | `azure storage blob list --container <containerName>` |



- Morat ćete izvršiti neke odabir o vašem novi VM:   <br> – Naziv spremnika   <br> Ime - VM   <br> Veličina - VM   <br> ime - vNet   <br> -Ime podmreže   <br> Ime - IP   <br> -NIC naziv
    

## <a name="login-and-set-your-subscription"></a>Prijava i postavljanje pretplate

1. Prijavite se na EŽA.
        
        azure login

2. Provjerite radite u načinu rada Voditelj resursa.
    
        azure config mode arm

3. Postavite odgovarajuće pretplate. Možete koristiti "račun za azure popis" da biste vidjeli sve svoje pretplate.

        azure account set <SubscriptionId>



## <a name="stop-the-vm"></a>Zaustavljanje na VM 

Zaustavljanje i deallocate izvor VM. Možete koristiti "azure vm popis" da biste dobili popis svih VMs u svoju pretplatu i njihovih resursa nazive grupa.
    
        azure vm stop <ResourceGroup> <VmName>
        azure vm deallocate <ResourceGroup> <VmName>




## <a name="copy-the-vhd"></a>Kopirajte na VHD


Možete kopirati u VHD iz spremišta izvora pomoću odredište na `azure storage blob copy start`. U ovom primjeru smo prolaze da biste kopirali u VHD isti račun za pohranu, ali neki drugi kontejner.

Da biste kopirali u VHD u drugi spremnik u isti račun za pohranu, upišite:

        azure storage blob copy start https://<sourceStorageAccountName>.blob.core.windows.net:8080/<sourceContainerName>/<SourceVHDFileName.vhd> <newcontainerName>
        

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Postavljanje virtualne mreže za svoje nove VM

Postavljanje virtualne mreže i NIC za svoje nove VM. 

    azure network vnet create <ResourceGroupName> <VnetName> -l <Location>

    azure network vnet subnet create -a <address.prefix.in.CIDR/format> <ResourceGroupName> <VnetName> <SubnetName>

    azure network public-ip create <ResourceGroupName> <IpName> -l <yourLocation>

    azure network nic create <ResourceGroupName> <NicName> -k <SubnetName> -m <VnetName> -p <IpName> -l <Location>


## <a name="create-the-new-vm"></a>Stvaranje nove VM 

Sada možete stvoriti na VM vaše prenesene virtualne disk [pomoću predloška za upravitelj resursa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) ili putem na EŽA navođenjem URI kopirane disk tako da upišete:

```bash
azure vm create -n <newVMName> -l "<location>" -g <resourceGroup> -f <newNicName> -z "<vmSize>" -d https://<storageAccountName>.blob.core.windows.net/<containerName/<fileName.vhd> -y Linux
```



## <a name="next-steps"></a>Daljnji koraci

Da biste saznali kako koristiti Azure EŽA da biste upravljali novi virtualnog računala, pogledajte [Azure EŽA naredbi za Voditelj resursa Azure](azure-cli-arm-commands.md).
