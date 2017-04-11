<properties
    pageTitle="Povezivanje Linux VMs u oblaku | Microsoft Azure"
    description="Povezivanje Linux virtualnim strojevima stvorene pomoću model klasični implementacije u oblaku Azure ili virtualne mreže."
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
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="connect-linux-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Povezivanje Linux virtualnim strojevima stvorene pomoću modela klasični implementaciju sa servisom virtualne mreže ili oblaka

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Linux virtualnim strojevima stvorene pomoću modela uvođenje klasičnog se uvijek smještaju u oblaku. Servis u oblaku radi kao spremnik i omogućuje jedinstveni naziv javno DNS-a, javna IP adresa i skup krajnje točke za pristup virtualnog računala putem Interneta. Servis u oblaku može biti u virtualne mreže, no to nije preduvjet. Možete [povezati virtualnim računalima sustava Windows s uslugom virtualne mreže ili oblačić](virtual-machines-windows-classic-connect-vms.md).

Ako neki servis u oblaku nije virtualne mreže, naziva servis u oblaku *samostalne* . Virtualnih računala u oblaku samostalne samo mogli komunicirati sa drugim virtualnim strojevima pomoću drugih virtualne strojeva javno DNS naziva i promet prelazi putem Interneta. Ako je neki servis u oblaku u virtualne mreže virtualnim strojevima u tom servisu u oblaku mogli komunicirati sa svim virtualnim strojevima u virtualne mreže bez slanja sav promet putem Interneta.

Ako ste postavili virtualnim strojevima u istom samostalne oblaku, i dalje možete koristiti za ujednačavanje opterećenja i dostupnosti skupa. Detalje potražite u članku [virtualnim strojevima uravnoteženja](virtual-machines-linux-load-balance.md) opterećenja i [dostupnosti virtualnim strojevima upravljanje](virtual-machines-linux-manage-availability.md). Međutim, ne možete organizirati virtualnim strojevima na podmreže ili servis u oblaku samostalne povezati lokalne mreže. Evo jednog primjera:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Daljnji koraci

Kada stvorite virtualnog računala, preporučuje se u da biste [dodali na disku podataka](virtual-machines-linux-classic-attach-disk.md) servisa i radnih opterećenja je mjesto na koje želite spremiti podatke. 



