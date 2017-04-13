<properties
   pageTitle="Kako označavanje Linux virtualnog računala | Microsoft Azure"
   description="Informirajte se o označavanju virtualni stroj Linux stvorene u Azure pomoću modela implementacije Voditelj resursa."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Kako označavanje Linux virtualnog računala u Azure

U ovom se članku opisuju različiti načini označavanje Linux virtualnog računala u Azure kroz model implementacije Voditelj resursa. Oznake su parove korisnički definirane ključ/vrijednosti koje se može smjestiti izravno na resurs ili grupu resursa. Azure trenutno podržava 15 oznake po resursa i grupa resursa. Oznake može smjestiti na resursa u vrijeme stvaranja ili dodati postojeći resurs. Uzmite u obzir, za resursa stvorene putem model upravljanja resursima implementacije samo podržane su oznake.

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Označavanje u kombinaciji s Azure EŽA

Da biste započeli, [instalirate i konfigurirate EŽA Azure](../xplat-cli-azure-resource-manager.md) i provjerite jeste li u načinu Voditelj resursa (`azure config mode arm`).

Možete pogledati sva svojstva za zadani virtualni stroj, uključujući oznake, ta naredba:

        azure vm show -g MyResourceGroup -n MyTestVM

Da biste dodali novu oznaku VM kroz EŽA Azure, možete koristiti u `azure vm set` naredbe uz parametar oznaka **-t**:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Da biste uklonili sve oznake, možete koristiti parametar **– T** u u `azure vm set` naredbe.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Sad kad su primijenjene oznake na našem resurse Azure EŽA i Portal sustava, recimo pogledajte informacije o korištenju da biste vidjeli oznake na portalu za naplatu.

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Daljnji koraci

* Dodatne informacije o označavanju Azure resursa potražite u članku [Pregled resursima Azure][] i [Korištenje oznake da biste organizirali Azure resursa][].
* Da biste vidjeli kako oznake omogućuju upravljanje korištenje Azure resursa, potražite u članku [objašnjenje računa Azure][] i [dobiti uvida u svoje potrošnje resursa za Microsoft Azure][].





[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Pregled Azure Voditelj resursa]: ../azure-resource-manager/resource-group-overview.md
[Korištenje oznaka da biste organizirali resursi za Azure]: ../resource-group-using-tags.md
[Objašnjenje računa za Azure]: ../billing/billing-understand-your-bill.md
[Rast uvida u svoje potrošnje resursa za Microsoft Azure]: ../billing-usage-rate-card-overview.md
