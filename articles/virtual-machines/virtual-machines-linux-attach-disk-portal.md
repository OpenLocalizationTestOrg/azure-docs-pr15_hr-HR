<properties
    pageTitle="Na disku podataka priložite Linux VM | Microsoft Azure"
    description="Kako priložiti novi ili postojeći podatkovni disk Linux VM na portalu Azure pomoću modela implementacije Voditelj resursa."
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
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Kako priložiti podatkovni disk Linux VM na portalu za Azure

U ovom se članku objašnjava dodavanje nove i postojeće diskova Linux virtualnog računala putem portala za Azure. Možete [priložiti podatkovni disk na VM Windows Azure portalu](virtual-machines-windows-attach-disk-portal.md). Prije nego što to učinite, pregledajte ove savjete:

- Veličina virtualnog računala određuje koliko podataka diskova možete priložiti. Detalje potražite u članku [veličine za virtualnih računala](virtual-machines-linux-sizes.md).
- Da biste koristili Premium prostora za pohranu, morat ćete DS-niz ili Oznaka njih virtualnog računala. Možete koristiti diskova s računa za pohranu Premium i standardnu s ove virtualnih računala. Prostor za pohranu Premium dostupna je u određenim regijama. Detalje potražite u članku [prostora za pohranu Premium: visokih performansi prostor za pohranu radnih opterećenja virtualnog računala Azure](../storage/storage-premium-storage.md).
- Diskova priložiti virtualnim strojevima su zapravo .vhd datoteke u račun za Azure prostora za pohranu. Detalje potražite u članku [o i VHDs za virtualnih računala](virtual-machines-linux-about-disks-vhds.md).
- Za novi disk, ne morate stvoriti je najprije jer Azure stvara ga, kada ga spojite.
- Za postojeće disk, datoteka .vhd mora biti dostupni u račun za Azure prostora za pohranu. Koristite .vhd koji je već postoji, ako je priložena drugom virtualnog računala ili prijenos vlastite .vhd datoteke u račun za pohranu.


[AZURE.INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## <a name="next-steps"></a>Daljnji koraci

Kada dodate na disku morate pripremite ih za korištenje. Potražite u članku u virtualnog računala operacijski sustav: "Kako: pokretanje podatkovni disk u Linux" u ovom [članku](virtual-machines-linux-classic-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux).
