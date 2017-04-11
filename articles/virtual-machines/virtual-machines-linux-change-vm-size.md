<properties
   pageTitle="Kako promijeniti veličinu Linux VM | Microsoft Azure"
   description="Kako se skaliranja prema gore i tako da promijenite veličinu VM skaliranje dolje virtualnog računala za Linux."
   services="virtual-machines-linux"
   documentationCenter="na"
   authors="mikewasson"
   manager="timlt"
   editor=""
   tags=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="mikewasson"/>


# <a name="how-to-resize-a-linux-vm"></a>Kako promijeniti veličinu Linux VM

## <a name="overview"></a>Pregled 

Nakon dodjele resursa virtualnog računala (VM), koje mogu mijenjati veličinu na VM prema gore ili prema dolje tako da promijenite [veličinu VM][vm-sizes]. U nekim slučajevima, morate najprije deallocate na VM. To se može dogoditi ako novu veličinu nije dostupan na klasteru hardveru na kojem je smješten u VM.

U ovom se članku objašnjava da biste promijenili veličinu Linux VM pomoću [Azure EŽA][azure-cli].

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Uvođenje klasičnog model.


## <a name="resize-a-linux-vm"></a>Promjena veličine Linux VM 

Da biste promijenili veličinu na VM, poduzeti sljedeće korake.

1. Pokrenite sljedeću naredbu EŽA. Ta se naredba popis veličina VM koji su dostupni na klaster hardver gdje se nalazi na VM.

    ```
    azure vm sizes -g <resource-group> --vm-name <vm-name>
    ```

2. Ako je naveden do željene veličine, pokrenite sljedeću naredbu da biste promijenili veličinu u VM.

    ```
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    ```

    Na VM će se pokrenuti tijekom postupka. Nakon ponovnog pokretanja, postojeće OS i diskova podataka će biti neispravno. Ništa na disku privremeni će se izgubiti.

    Korištenje na `--enable-boot-diagnostics` mogućnost omogućuje [Pokretanje dijagnostike][boot-diagnostics], da biste se prijavili sve pogreške vezane uz prilikom pokretanja.

3. U suprotnom do željene veličine nije naveden, pokrenite sljedeće naredbe deallocate VM, mu promijenili veličinu, a zatim ponovno pokrenite na VM.

    ```
    azure vm deallocate -g <resource-group> <vm-name>
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    azure vm start -g <resource-group> <vm-name>
    ```

   > [AZURE.WARNING] Također deallocating na VM izdaje dodijeljene u VM sve dinamičku IP adresu. Ne utječe na diskova OS i podatke.
   
## <a name="next-steps"></a>Daljnji koraci

Dodatni skalabilnost pokrenuti više instanci VM i Vremensko mjerilo. Dodatne informacije potražite u članku [automatski promijeniti omjer Linux strojeva u skupu na skaliranje virtualnog računala][scale-set]. 

<!-- links -->
   
[azure-cli]: ../xplat-cli-install.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]: virtual-machines-linux-sizes.md