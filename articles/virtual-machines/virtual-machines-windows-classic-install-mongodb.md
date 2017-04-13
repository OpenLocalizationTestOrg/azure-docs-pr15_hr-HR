<properties
    pageTitle="Stvarna MongoDB veličina VM | Microsoft Azure"
    description="Saznajte kako instalirati MongoDB na programa Azure VM stvorene pomoću klasične implementacije modelu koji se izvodi Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

#<a name="install-mongodb-on-a-windows-vm"></a>Stvarna MongoDB veličina VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Instaliranje i konfiguriranje MongoDB pomoću modela implementacije Voditelj resursa, pročitajte [Ovaj članak](virtual-machines-windows-classic-install-mongodb.md).

[MongoDB] [ MongoDB] je popularne Otvori izvor, visokih performansi NoSQL baza podataka. U ovom se članku će vas voditi kroz stvaranje Windows Server virtualni stroj (VM) pomoću [portala za Azure klasični][AzurePortal]. Zatim stvorite i priložite podatkovni disk VM prije instaliranje i konfiguriranje MongoDB. Ako imate postojeću VM u Azure koje želite koristiti, možete se odmah baciti na [Instaliranje i konfiguriranje MongoDB](#install-and-run-mongodb-on-the-virtual-machine).


## <a name="create-a-virtual-machine-running-windows-server"></a>Stvaranje virtualnog računala sa sustavom Windows Server

Slijedite ove upute za stvaranje virtualnog računala.

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE] Dodavanje krajnje točke za MongoDB prilikom stvaranja virtualnog računala i konfigurirajte ga na sljedeći način: naziv kao **Mongo**, **TCP** upotrijebili protokol i postavite na javne i privatne priključke **27017**.

## <a name="attach-a-data-disk"></a>Prilaganje podatkovni disk
Omogućuje pohranu za virtualnog računala priložiti podatkovni disk, a zatim ga pokrenuti tako da ga možete koristiti za Windows. Ako već imate podatkovni disk, možete priložiti toj postojeći disk ili možete priložiti prazan disk.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

Upute za pokretanje disk, potražite "Kako: pokretanje podatkovni disk u sustavu Windows Server" u [kako priložiti podatkovni disk na virtualnog računala za Windows](virtual-machines-windows-classic-attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Instalirajte i pokrenite MongoDB na virtualnog računala

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Sažetak
U ovom ćete praktičnom vodiču naučili kako stvoriti virtualnog računala sa sustavom Windows Server, daljinski s njim povezati i priložite podatkovni disk.  I saznali kako instalirati i konfigurirati MongoDB na virtualnog računala utemeljen na sustavu Windows. Sada možete pristupiti MongoDB na virtualnog računala utemeljen na sustavu Windows slijedeći dodatne teme u [dokumentaciji MongoDB][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com
