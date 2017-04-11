<properties
    pageTitle="Što su VM skaliranje postavlja? | Microsoft Azure"
    description="Saznajte više o VM skaliranje skupove."
    keywords="Postavlja virtualnog računala skaliranje Linux virtualnog računala" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/24/2016"
    ms.author="gatneil"/>

# <a name="what-are-virtual-machine-scale-sets"></a>Što su virtualnog računala skaliranje postavlja?

Skupovi skaliranje virtualnog računala omogućuju upravljanje više VMs u skupu. Visoke razine skaliranje skup ima sljedeće argumente za i protiv:

Profesionalce:

1. Visoke dostupnosti. Svaki skup skaliranje stavlja njegov VMs u dostupnost postavljanje 5 kvara domene (FDs) i 5 ažuriranje domene (UDs) koja osiguraj (za dodatne informacije o FDs i UDs, potražite [dostupnost VM](./virtual-machines-linux-manage-availability.md)). 
2. Jednostavna Integracija s Azure opterećenja i aplikacije pristupnika.
3. Jednostavna Integracija s automatsko skaliranje Azure.
4. Pojednostavnjeni implementaciju, upravljanje i čišćenja od VMs.
5. Podržava uobičajene Windows i Linux flavors, kao i prilagođene slike.

Protiv:

1. Ne možete priložiti podataka diskova VM instance u skupu mjerilo. Umjesto toga, morate koristiti blobova, Azure datoteke, Azure tablice ili druge rješenja za pohranu.

## <a name="quick-create-using-azure-cli"></a>Brzo stvaranje pomoću EŽA Azure

[AZURE.INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## <a name="next-steps"></a>Daljnji koraci

Opće informacije potražite u članku [glavni odredišna stranica za skupove mjerilo](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

Više dokumentacije, pogledajte [stranicu glavni dokumentaciju za skaliranje postavlja](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Na primjer resursima predložaka korištenje skupova skaliranje potražit "vmss" u [repo github Azure predložaka za brzi početak rada](https://github.com/Azure/azure-quickstart-templates).

