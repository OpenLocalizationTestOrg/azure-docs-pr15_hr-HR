<properties
   pageTitle="Stvorite identitet tvrtke ili obrazovne ustanove u AAD | Microsoft Azure"
   description="Saznajte kako stvoriti tvrtke ili obrazovne ustanove identiteta servisa Azure Active Directory za uporabu virtualnim strojevima sustava Windows."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-windows-vms"></a>Stvaranje tvrtke ili obrazovne ustanove identiteta servisa Azure Active Directory za uporabu VMs za Windows

Ako ste stvorili osobni račun za Azure ili ste osobne pretplate na MSDN i stvoriti račun za Azure da iskoristite prednost kredita MSDN Azure – koristi identiteta za *Microsoftov račun* da biste je stvorili. Mnoge sjajne značajke programa Azure – [Predlošci grupnih resurs](../azure-resource-manager/resource-group-overview.md) je jedan primjer--potreban račun tvrtke ili obrazovne ustanove (identitet upravlja Azure Active Directory) da biste radili. Možete slijediti upute u nastavku da biste stvorili Budući da Srećom, najbolje stvari o osobni račun za Azure je dolazi s zadanu domenu za Azure Active Directory koje možete koristiti da biste stvorili na novi račun tvrtke ili obrazovne ustanove koji koristite za Azure značajki koje zahtijevaju ga na novi račun tvrtke ili obrazovne ustanove.

Međutim, najnovije promjene omogućuju da biste upravljali pretplatom s bilo koje vrste račun za Azure pomoću na `azure login` interaktivna prijava metoda opisana [ovdje](../xplat-cli-connect.md). Pomoću tog mehanizam ili možete slijediti upute koje pratite. Možete i [stvoriti tvrtke ili obrazovne ustanove identiteta servisa Azure Active Directory za uporabu Linux VMs](virtual-machines-linux-create-aad-work-id.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]
