<properties
    pageTitle="Alati zajednice za upravljanje servisom Azure Voditelj resursa Azure migracije"
    description="U ovom se članku katalozi alatima koji je dodijeljen Zajednica pomoći migracije IaaS resursa iz Upravljanje servisom Azure hrpu Azure Voditelj resursa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="singhkay"/>

# <a name="community-tools-for-azure-service-management-to-azure-resource-manager-migration"></a>Alati zajednice za upravljanje servisom Azure Voditelj resursa Azure migracije

U ovom se članku katalozi alatima koji je dodijeljen Zajednica pomoći migracije IaaS resursa iz Upravljanje servisom Azure snop Azure Voditelj resursa.

>[AZURE.NOTE]Ti Alati ne službeno podržava Microsoft Support. Stoga su otvorene izvorni termini na Github i Ispričavamo se zadovoljni da biste prihvatili PRs za rješenja ili dodatne scenarije. Da biste prijavili problem, značajku Github problema.
>
> Migracija s te alate, uzrokovat će nedostupnost za klasični virtualnog računala. Ako tražite platformu podržane migracije, posjetite 
>
>- [Platforme koje podržava migraciju resursa IaaS od klasičnog snop Voditelj resursa za Azure](./virtual-machines-windows-migration-classic-resource-manager.md)
>- [Tehnički precizno Dive na platformi podržane migracije od klasičnog za Azure Voditelj resursa](./virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
>- [Migriranje resursi IaaS od klasičnog za Azure Voditelj resursa pomoću komponente PowerShell Azure](./virtual-machines-windows-ps-migration-classic-resource-manager.md)

## <a name="asm2arm"></a>ASM2ARM

Ovo je modula skriptu PowerShell radi migracije na **jednom** virtualnog računala (VM) iz Upravljanje servisom Azure snop u stogu Voditelj resursa Azure. 

[Veza na dokumentaciju alata](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz

migAz se dodatna mogućnost da biste migrirali cjelovit skup resursa za Azure servis za upravljanje IaaS Azure resursima IaaS resursima. Migracija može dogoditi unutar iste pretplate ili između različitih pretplate i vrste pretplata (ex: CSP pretplate).

[Veza na dokumentaciju alata](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)