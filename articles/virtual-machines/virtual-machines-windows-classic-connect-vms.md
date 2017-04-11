<properties
    pageTitle="Povezivanje Windows VMs u oblaku | Microsoft Azure"
    description="Povezivanje Windows virtualnim strojevima stvorene pomoću model klasični implementacije u oblaku Azure ili virtualne mreže."
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

# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Povezivanje Windows virtualnim strojevima stvorene pomoću modela klasični implementaciju sa servisom virtualne mreže ili oblaka

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Windows virtualnim strojevima stvorene pomoću model klasični implementacije se uvijek smještaju u oblaku. Servis u oblaku radi kao spremnik i omogućuje jedinstveni naziv javno DNS-a, javna IP adresa i skup krajnje točke za pristup virtualnog računala putem Interneta. Servis u oblaku može biti u virtualne mreže, no to nije preduvjet. Možete [povezati Linux virtualnim strojevima sa servisom virtualne mreže ili oblačić](virtual-machines-linux-classic-connect-vms.md).

Ako neki servis u oblaku nije virtualne mreže, naziva servis u oblaku *samostalne* . Virtualnih računala u oblaku samostalne samo mogli komunicirati sa drugim virtualnim strojevima pomoću drugih virtualne strojeva javno DNS naziva i promet prelazi putem Interneta. Ako je neki servis u oblaku u virtualne mreže virtualnim strojevima u tom servisu u oblaku mogli komunicirati sa svim virtualnim strojevima u virtualne mreže bez slanja sav promet putem Interneta.

Ako ste postavili virtualnim strojevima u istom samostalne oblaku, i dalje možete koristiti za ujednačavanje opterećenja i dostupnosti skupa. Detalje potražite u članku [virtualnim strojevima uravnoteženja](virtual-machines-windows-load-balance.md) opterećenja i [dostupnosti virtualnim strojevima upravljanje](virtual-machines-windows-manage-availability.md). Međutim, ne možete organizirati virtualnim strojevima na podmreže ili servis u oblaku samostalne povezati lokalne mreže. Evo jednog primjera:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Daljnji koraci

Kada stvorite virtualnog računala, preporučuje se u da biste [dodali na disku podataka](virtual-machines-windows-classic-attach-disk.md) servisa i radnih opterećenja je mjesto na koje želite spremiti podatke. 