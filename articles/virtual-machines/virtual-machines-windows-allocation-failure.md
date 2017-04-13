<properties
    pageTitle="Otklanjanje pogrešaka pri dodjeli Windows VM | Microsoft Azure"
    description="Otklanjanje pogrešaka pri dodjeli kada stvarate, ponovno pokrenite ili promjena veličine Windows VM servisu Azure"
    services="virtual-machines-windows, azure-resource-manager"
    documentationCenter=""
    authors="JiangChen79"
    manager="felixwu"
    editor=""
    tags="top-support-issue,azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/02/2016"
    ms.author="cjiang"/>

# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-windows-vms-in-azure"></a>Otklanjanje pogrešaka pri dodjeli kada stvarate, ponovno pokrenite ili promjena veličine VMs Windows servisu Azure

Kada stvorite na VM, ponovno pokrenite zaustavljen (deallocated) VMs ili promjena veličine na VM, Microsoft Azure dodjeljuje računalnim resurse za vašu pretplatu. Povremeno primate pogreške prilikom izvršavanja te operacije – čak i prije dođete do ograničenja Azure pretplate. U ovom se članku objašnjava uzroka neke od uobičajenih pogrešaka pri dodjeli i predlaže moguće olakšava. Informacije može biti korisna kada planirate implementaciju servisa. Možete i [Otklanjanje pogrešaka pri dodjeli kada stvarate, ponovno pokrenite, ili promjena veličine Linux VMs u Azure](virtual-machines-linux-allocation-failure.md).

[AZURE.INCLUDE [virtual-machines-common-allocation-failure](../../includes/virtual-machines-common-allocation-failure.md)]
