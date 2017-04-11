<properties
    pageTitle="Postavljanje krajnje točke na klasični VM Linux | Microsoft Azure"
    description="Saznajte kako postaviti krajnje točke za Linux VM na portalu za Azure klasični dopustiti komunikaciju s Linux virtualnog računala u Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/13/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Upute za postavljanje krajnje točke na Linux klasični virtualne računalo u Azure

Sve Linux virtualnim strojevima koji ste stvorili u Azure pomoću modela uvođenje klasičnog možete automatski komunikaciju putem kanala privatna mreža s drugim virtualnim strojevima u istom servis u oblaku ili virtualne mreže. Međutim, računala na Internetu ili na drugim mrežama virtualne zahtijevaju krajnje točke da biste dolazni mrežni promet usmjerili virtualnog računala. U ovom se članku i dostupna je za [virtualnim strojevima sa sustavom Windows](virtual-machines-windows-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]U tom modelu implementaciju **Upravljanja resursima** krajnje točke konfigurirane pomoću **Mreže sigurnosnih grupa (NSGs)**. Dodatne informacije potražite u članku [otvaranju priključaka i krajnje točke] (virtualne-računalima-linux-nsg-quickstart.md).

Kada stvorite Linux virtualnog računala na portalu za Azure klasični, krajnje točke za sigurnu ljuske (SSH) obično automatski se stvara za vas. Prilikom stvaranja virtualnog računala ili naknadno potrebi možete konfigurirati dodatne krajnje točke.
 

[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Daljnji koraci

* Krajnja točka VM možete stvoriti i pomoću [sučelja Azure naredbenog retka](../virtual-machines-command-line-tools.md). Pokrenite naredbu **Stvaranje azure vm krajnjoj točki** .

* Ako ste stvorili virtualnog računala u modelu implementacije Voditelj resursa, možete koristiti EŽA Azure u načinu resursima za kontrolu promet usmjerili na VM [Stvaranje mreže sigurnosne grupe](../virtual-network/virtual-networks-create-nsg-arm-cli.md) .