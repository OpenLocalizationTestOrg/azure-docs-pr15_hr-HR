<properties
    pageTitle="Na disku priložiti u VM | Microsoft Azure"
    description="Prilaganje podatkovni disk na Windows virtualnog računala stvorene pomoću model klasični implementacije i inicijalizaciju."
    services="virtual-machines-windows, storage"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="cynthn"/>

# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Prilaganje podatkovni disk na Windows virtualnog računala stvorene pomoću model klasični implementacije

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ako želite koristiti na novom portalu potražite u članku [kako priložiti podatkovni disk na VM Windows Azure portalu](virtual-machines-windows-attach-disk-portal.md).

Ako vam je potrebna na disk dodatne podatke, možete priložiti prazan disk ili postojeći disk s podacima u VM. U oba slučaja u diskova su .vhd datoteke koje se nalaze u račun za Azure prostora za pohranu. U slučaju novi disk nakon priložiti diska, i morat ćete pokrenuti tako da bude spreman za korištenje Windows VM.

Dodatne informacije o diskova potražite u članku [o i VHDs za virtualnih računala](virtual-machines-windows-about-disks-vhds.md).


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>Pokretanje disk

1. Povezivanje s virtualnog računala. Upute potražite u članku [kako se prijaviti na virtualnog računala sa sustavom Windows Server][logon].

2. Kada se prijavite na virtualnog računala, otvorite **Upravitelj poslužitelja**. U lijevom oknu odaberite **datoteku i servise za pohranu**.

    ![Otvaranje upravitelja poslužitelja](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Proširenje izbornika i odaberite **diskova**.

4. Odjeljak **diskova** popise s diskova. U većini slučajeva, imat će disk 0, disk 1 i disk 2. Disk 0 disk operacijski sustav, disk 1 je privremeni diska i podatkovni disk samo priložiti u VM je disk 2. Novi disk podataka prikazat će se popis particija kao **Nepoznato**. Desnom tipkom miša kliknite disk, a zatim odaberite **pokretanje**.

5.  Dobit ćete obavijest da svi podaci se brišu kada je pokrenut na disku. Kliknite **da** da biste pogledajte upozorenje i pokretanje disk. Kada završi, na Partion će biti navedena kao **GPT**. Ponovno kliknite disk desnom tipkom miša, a zatim odaberite **Nova jedinica**.

6.  Dovršite čarobnjak pomoću zadanih vrijednosti. Kad je čarobnjak, u odjeljku **količine** popis novu jedinicu. Disk je sada mreži i spreman je za pohranu podataka.

    ![Količinsko uspješno pokrenuti](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] Veličina u VM određuje koliko diskova možete priložiti u nju. Detalje potražite u članku [veličine za virtualnih računala](virtual-machines-linux-sizes.md).

## <a name="additional-resources"></a>Dodatni resursi

[Upute za odvajanje disk s virtualnog računala za Windows](virtual-machines-windows-classic-detach-disk.md)

[O i VHDs za virtualnim strojevima](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md
