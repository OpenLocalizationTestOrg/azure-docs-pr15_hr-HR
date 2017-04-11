<properties
    pageTitle="Migriranja resursi IaaS od klasičnog Azure resursa upravitelju pomoću Azure EŽA | Microsoft Azure"
    description="U ovom se članku objašnjavaju migracije podržane platforme resursa od klasičnog Azure resursa upravitelju pomoću EŽA Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Popis svakodnevnih zadataka IaaS resursi klasični Azure resursa upravitelju pomoću EŽA Azure

Sljedeći vam koraci prikazuju kako pomoću naredbe Azure sučelje naredbenog retka (EŽA) da biste migrirali infrastrukture kao resurse servisa (IaaS) iz modela uvođenje klasičnog implementacije model upravljanja resursima Azure. U članku zahtijeva [Azure EŽA](../xplat-cli-install.md).

>[AZURE.NOTE] Sve operacije u nastavku opisane su idempotent. Ako imate problema s koji nije nepodržane značajke ili pogreške u konfiguraciji, preporučujemo da ponovno pokušajte Priprema, prekinuti ili izvršavanje operacija. Platforme će pa pokušajte ponovno akciju.

## <a name="step-1-prepare-for-migration"></a>Korak 1: Priprema za migraciju

Slijedi popis nekoliko najboljih postupaka koji preporučujemo da se prilikom donošenja migracije IaaS resursa iz klasični upravitelju resursa:

- Pročitajte [popis nepodržane konfiguracije ili značajke](virtual-machines-windows-migration-classic-resource-manager.md). Ako imate virtualnim strojevima koji koristi nepodržane konfiguracije ili značajke, preporučujemo da Pričekajte podršku/konfiguracije značajki da biste se objaviti. Umjesto toga, možete ukloniti tu značajku ili premjestiti iz tog konfiguracije da biste omogućili migracije ako najbolje odgovara vašim potrebama.
-   Ako ste automatski skripte koje danas implementacija infrastrukturu i aplikacije, pokušajte da biste stvorili sličan postavljanje test pomoću tih skripte za migraciju. Osim toga, možete postaviti okruženja uzorka pomoću portala za Azure.

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Korak 2: Postavite svoju pretplatu i registrirati davatelja usluga

Za migraciju scenarije, morate postaviti okruženja za oba klasični i resursima. [Instalacija EŽA Azure](../xplat-cli-install.md) i [Odaberite svoju pretplatu](../xplat-cli-connect.md).

Prijava na račun.
    
    azure login

Odaberite pretplatu u Azure pomoću sljedeće naredbe.

    azure account set "<azure-subscription-name>"

>[AZURE.NOTE] Registracija je neko vrijeme korak, ali je potrebno učiniti jedanput prije migracije. Bez registracije vidjet ćete sljedeću poruku o pogrešci 

>   *BadRequest: Pretplate nije registriran za migraciju.* 

Registrirajte se s davateljem resursa migracije pomoću sljedeće naredbe. Imajte na umu da u nekim slučajevima, ta se naredba ističe. Međutim, Registracija neće biti uspješan.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Pričekajte pet minuta za registraciju da biste završili. Pomoću sljedeće naredbe možete provjeriti status odobrenje. Provjerite je li RegistrationState `Registered` prije nastavka.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Sada prijeđite EŽA da biste na `asm` načinu rada.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Korak 3: Provjerite je li dovoljno jezgri Azure resursima virtualnog računala u području Azure trenutni implementacije ili VNET

Za ovaj korak morat ćete da biste prešli na `arm` načinu rada. To se pomoću sljedeće naredbe.

```
azure config mode arm
```

Koristite sljedeću naredbu EŽA da biste provjerili tekući iznos jezgri imate u Azure Voditelj resursa. Dodatne informacije o kvotama core potražite u članku [ograničenja i Voditelj resursa za Azure](../articles/azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Kada završite potvrditi taj korak, možete prijeći natrag `asm` načinu rada.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Korak 4: Mogućnost 1 – migrirati virtualnim računalima sustava u oblaku 

Dohvaćanje popisa servise u oblaku pomoću sljedeću naredbu, a zatim odaberite servisa u oblaku koju želite migrirati. Imajte na umu da ako VMs u servis u oblaku u virtualne mreže ili ako imaju uloge web-tempiranja, dobit ćete poruku o pogrešci.

    azure service list

Pokrenite sljedeću naredbu da biste naziv implementacije u oblaku s opširno izlaz. U većini slučajeva, naziv implementacije je isti kao naziv usluge oblaka.

    azure service show <serviceName> -vv

Priprema virtualnih računala u oblaku za migraciju. Imate dvije mogućnosti za odabir.

Ako želite migrirati u VMs platformu stvorili virtualne mrežom, koristite sljedeću naredbu.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Ako želite migrirati postojeću virtualne mreže u modelu implementaciju upravljanja resursima, koristite sljedeću naredbu.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> subnetName <vnetName>

Nakon Priprema postupak ne uspije, možete pregledati opširno Izlaz na početak migracije stanje u VMs i provjerite jesu li se u na `Prepared` stanje.

    azure vm show <vmName> -vv

Provjerite konfiguraciju pripremljeni resursa pomoću EŽA ili Azure portal. Ako niste spremni za migraciju i želite da biste se vratili na staru stanje, koristite sljedeću naredbu.

    azure service deployment abort-migration <serviceName> <deploymentName>

Ako ste zadovoljni izgledom pripremljeni konfiguraciju, možete kretanje naprijed i primjenu resursa pomoću sljedeće naredbe.

    azure service deployment commit-migration <serviceName> <deploymentName>


    
## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Korak 4: Mogućnost 2 – migrirati virtualnim strojevima u virtualne mreže

Odaberite virtualne mreže koju želite migrirati. Imajte na umu da ako virtualne mreže sadrži uloge web-tempiranja ili VMs s nepodržane konfiguracije, dobit ćete poruku pogreške provjere valjanosti.

Dohvaćanje virtualne mreže u pretplatu pomoću sljedeće naredbe.

    azure network vnet list
    
Izlaz izgledat će otprilike ovako:

![Snimka zaslona naredbenog retka s nazivom cijelu virtualne mreže istaknut.](./media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

U primjeru iznad **virtualNetworkName** je cijeli naziv **"Grupe classicubuntu16 classicubuntu16"**.

Priprema virtualne mreže po izboru za migraciju pomoću sljedeće naredbe.

    azure network vnet prepare-migration <virtualNetworkName>

Provjerite konfiguraciju za pripremljeni virtualnim strojevima pomoću EŽA ili Azure portal. Ako niste spremni za migraciju i želite da biste se vratili na staru stanje, koristite sljedeću naredbu.

    azure network vnet abort-migration <virtualNetworkName>

Ako ste zadovoljni izgledom pripremljeni konfiguraciju, možete kretanje naprijed i primjenu resursa pomoću sljedeće naredbe.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Korak 5: Migracija na račun za pohranu

Kada završite migracije virtualnim strojevima, preporučujemo da migrirati račun za pohranu.

Priprema za pohranu računa za migraciju pomoću sljedeće naredbe

    azure storage account prepare-migration <storageAccountName>

Provjerite konfiguraciju računa pripremljeni prostora za pohranu pomoću EŽA ili Azure portal. Ako niste spremni za migraciju i želite da biste se vratili na staru stanje, koristite sljedeću naredbu.

    azure storage account abort-migration <storageAccountName>

Ako ste zadovoljni izgledom pripremljeni konfiguraciju, možete kretanje naprijed i primjenu resursa pomoću sljedeće naredbe.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Daljnji koraci

- [Podržane platforme migracije IaaS resursa od klasičnog upravitelju resursa](virtual-machines-windows-migration-classic-resource-manager.md)
- [Tehnički precizno dive na platformi podržane migracije od klasičnog upravitelju resursa](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
