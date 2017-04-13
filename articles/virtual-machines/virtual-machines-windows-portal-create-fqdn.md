<properties
   pageTitle="Stvaranje FQDN za na VM Azure portalu | Microsoft Azure"
   description="Saznajte kako stvoriti potpuno kvalificirani naziv domene ili FQDN za upravitelj resursa koji se temelje virtualnog računala na portalu za Azure."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Stvaranje potpuno kvalificirani naziv domene na portalu za Azure
Kada stvorite virtualnog računala (VM) [Azure portal](https://portal.azure.com) pomoću modela implementacije Voditelj resursa, automatski se stvara javnu IP resurs virtualnog računala. Koristite ovu IP adresu za daljinski pristup na VM. Iako portal stvoriti na [potpuno kvalificirani naziv domene](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)ili FQDN, po zadanom, možete ga stvoriti nakon stvaranja na VM. U ovom se članku objašnjava korake da biste stvorili DNS naziv ili FQDN.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Sada se možete povezati daljinski da biste VM pomoću DNS naziv kao što su za Remote Desktop Protocol (RDP).

## <a name="next-steps"></a>Daljnji koraci
Sad kad vaše VM ima naziv javnu IP-a i DNS- a, možete implementirati uobičajenih okviri aplikacije i servise kao što je IIS i SQL ili u sustavu SharePoint.

Također možete potražite dodatne informacije o [korištenju upravitelja resursa](../azure-resource-manager/resource-group-overview.md) stvara sustava Azure implementacijama savjete za.