<properties
    pageTitle="Postavljanje krajnje točke na klasični VM Windows | Microsoft Azure"
    description="Saznajte kako postaviti krajnje točke za VM Windows Azure klasični portalu dopustiti komunikaciju s virtualnog računala za Windows u Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Upute za postavljanje krajnje točke na klasični virtualnog računala sustava Windows u Azure


Svi prozori virtualnim strojevima koji ste stvorili u Azure pomoću modela uvođenje klasičnog može se automatski komunikaciju privatnog kanala mreže s drugim virtualnim strojevima u istom servis u oblaku ili virtualne mreže. Međutim, računala na Internetu ili na drugim mrežama virtualne zahtijevaju krajnje točke da biste dolazni mrežni promet usmjerili virtualnog računala. U ovom se članku i dostupna je za [virtualnim strojevima Linux](virtual-machines-linux-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]U tom modelu implementaciju **Upravljanja resursima** krajnje točke konfigurirane pomoću **Mreže sigurnosnih grupa (NSGs)**. Dodatne informacije potražite u članku [Dopusti vanjski pristup vašem VM pomoću portala za Azure] (virtual-machines-windows-nsg-quickstart-portal.md).

Kada stvorite virtualnog računala za Windows Azure klasični portalu, uobičajeni krajnje točke poput onih udaljene radne površine i Windows PowerShell Remoting obično automatski se stvaraju za vas. Prilikom stvaranja virtualnog računala ili naknadno potrebi možete konfigurirati dodatne krajnje točke.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Daljnji koraci

* Pomoću cmdleta za Azure PowerShell da biste postavili krajnjoj točki VM, potražite u članku [Dodavanje AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).

* Pomoću cmdleta za Azure PowerShell za upravljanje ACL na krajnje, potražite u članku [Upravljanje kontrola pristupa popise (ACL) za krajnje točke pomoću komponente PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

* Ako ste stvorili virtualnog računala u modelu implementacije Voditelj resursa, Azure PowerShell možete koristiti za [Stvaranje mreže sigurnosne grupe](../virtual-network/virtual-networks-create-nsg-arm-ps.md) kontrola promet usmjerili na VM.
