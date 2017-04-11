<properties
    pageTitle="Otklanjanje poteškoća s brisanjem računi Azure prostora za pohranu, spremnika ili VHDs klasični implementacije | Microsoft Azure"
    description="Otklanjanje poteškoća s brisanjem računi Azure prostora za pohranu, spremnika ili VHDs klasični implementacije"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="tysonn"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="genli"/>

# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Otklanjanje poteškoća s brisanjem računi Azure prostora za pohranu, spremnika ili VHDs klasični implementacije

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Možda ćete primiti pogreške kada pokušate izbrisati račun za Azure prostora za pohranu, spremnik ili VHD web-mjesto [portala za Azure](https://portal.azure.com/) ili u okvir za [Azure klasični portal](https://manage.windowsazure.com/). Probleme može uzrokovati u sljedećim situacijama:

-   Kada izbrišete na VM, disk i VHD se neće automatski brišu. Koji bi mogli biti razlog pogrešku na brisanja računa za pohranu. Da bi mogli koristiti na disku postavljanja drugi VM smo nemojte brisati disk.

-   I dalje je Zakup na disku ili blob povezanog s diska.

Ako poteškoću Azure je adresirana ovog članka, posjetite forume o Azure na [MSDN i prelijevanje stogu](https://azure.microsoft.com/support/forums/). Možete objaviti problem na forume za ove ili u @AzureSupport na Twitteru. Osim toga, možete datoteka zahtjev za Azure podršku tako da odaberete **Pomoć** na web-mjestu za [podršku Azure](https://azure.microsoft.com/support/options/) .

## <a name="symptoms"></a>Simptomi

Sljedeći odjeljak sadrži popis uobičajenih pogrešaka koje se mogu pojaviti kada pokušate izbrisati računi Azure prostora za pohranu, spremnika ili VHDs.

### <a name="scenario-1-unable-to-delete-a-storage-account"></a>Scenarij 1: Nije moguće izbrisati s računom za pohranu

Prilikom idite na račun za pohranu u [ [Azure portal](https://portal.azure.com/) ili Azure klasični portal](https://manage.windowsazure.com/) i odaberite **Izbriši**, možda će se pojaviti sljedeća poruka o pogrešci:

*Račun za pohranu StorageAccountName sadrži VM slike. Provjerite je li te slike VM uklanjaju se prije brisanja taj račun za pohranu.*

Mogu se prikazati sljedeća pogreška:

**Portal na Azure**:

*Nije uspjelo brisanje računa za pohranu < vm--naziv računa za spremište->. Nije moguće izbrisati račun za pohranu < vm--naziv računa za spremište->: "račun za pohranu < vm--naziv računa za spremište-> sadrži neke aktivni slike i/ili diskove. Provjerite je li te slike i/ili diskove uklanjaju se prije brisanja taj račun za pohranu. ".*

**Klasični portal na Azure**:

*Prostor za pohranu račun < vm--naziv računa za spremište-> ima neki aktivni slike i/ili diskove, primjerice xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Provjerite je li to slike i/ili diskove uklanjaju se prije brisanja taj račun za pohranu.*

Ili

**Portal na Azure**:

Račun za pohranu *< vm--naziv računa za spremište-> sadrži 1 container(s) koji imaju aktivne slike i/ili artefakte disk. Provjerite je li te artefakte uklonjeni su iz spremišta slike prije brisanja taj račun za pohranu*.

**Klasični portal na Azure**:

Račun za slanje nije uspjelo prostora za pohranu *< vm--naziv računa za spremište-> sadrži 1 container(s) koji imaju aktivne slike i/ili artefakte disk. Provjerite je li te artefakte uklonjeni su iz spremišta slike prije brisanja taj račun za pohranu. Kada pokušate izbrisati račun za pohranu i postoje aktivan diskova pridružena, vidjet ćete poruku koja vas obavještava postoje aktivne diskova koje je potrebno izbrisati*.

### <a name="scenario-2-unable-to-delete-a-container"></a>Scenarij 2: Nije moguće izbrisati spremnik

Kada pokušate izbrisati spremnik za pohranu, možda će se prikazati sljedeća pogreška:

*Nije uspjelo brisanje spremnik za pohranu <container name>. Pogreška: "na spremnik trenutno postoji u Zakup, a ne Zakup ID naveden u zahtjevu za*.

### <a name="scenario-3-unable-to-delete-a-vhd"></a>Scenarij 3: Nije moguće izbrisati s VHD

Kada je izbrisati s VM, a zatim pokušate izbrisati s blob-ova za povezane VHDs, možda će se pojavljuje se sljedeća poruka:

*Nije uspjelo brisanje blob ' path/XXXXXX-XXXXXX-os-1447379084699.vhd'. Pogreška: "na blob-om trenutno postoji u Zakup, a ne Zakup ID naveden u zahtjevu za.*

## <a name="solution"></a>Rješenja
Da biste riješili najčešći problemi, pokušajte na sljedeći način:

### <a name="step-1-delete-any-os-disks-that-are-preventing-deletion-of-the-storage-account-container-or-vhd"></a>Korak 1: Brisanje sve OS diskova koji sprječavaju brisanja računa za pohranu, spremnik ili VHD

1. Prebacite se na [portal za Azure klasični](https://manage.windowsazure.com/).
2. Odaberite **VIRTUALNOG računala** > **DISKOVA**.

    ![Slika diskova na virtualnim strojevima Azure klasični portala.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)

3. Pronađite diskova koje su vezane uz račun za pohranu, spremnik ili VHD koji želite izbrisati. Kada potvrdite mjesto na disku, tražit će račun povezan prostora za pohranu, spremnik ili VHD.

    ![Slika koja prikazuje informacije o mjestu diskova Azure klasični portala](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)

4. Provjerite nema VM nalazi na polju **priložiti** od diskova, a zatim izbrišite na diskova.

    > [AZURE.NOTE] Ako na disku priključen na VM, nećete moći da biste je izbrisali. Diskova su odvojene iz izbrisane VM asinkrono. Može potrajati nekoliko minuta nakon brisanja na VM za ovo polje da biste očistili prema gore.

### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-the-storage-account-or-container"></a>Korak 2: Brisanje VM slikama koje onemogućuju brisanja računa za pohranu ili spremnik

1. Prebacite se na [portal za Azure klasični](https://manage.windowsazure.com/).
2. Odaberite **VIRTUALNOG računala** > **SLIKE**, a zatim izbrišite slike koje su vezane uz račun za pohranu, spremnik ili VHD.

    Nakon toga ponovo pokušajte izbrisati račun za pohranu, spremnik ili VHD.

> [AZURE.WARNING] Obavezno sigurnosno kopirajte sve koje želite spremiti prije brisanja računa. Nije moguće vratiti izbrisane prostora za pohranu račun ili dohvatiti sadržaj koji je sadržan prije brisanja. To također se odnosi i za sve resurse u računa: nakon brisanja VHD, blob, tablice, reda čekanja ili datoteka se trajno izbrisati. Provjerite je li resurs ne koristi.

## <a name="about-the-stopped-deallocated-status"></a>O statusu zaustavljen (deallocated)

VMs koji su stvoreni u modelu uvođenje klasičnog i zadržavaju koji će imati status **zaustavljen (deallocated)** na [portal za Azure](https://portal.azure.com/) ili [Azure klasični portal](https://manage.windowsazure.com/).

**Azure klasični portal**:

![Zaustavljen (Deallocated) stanja za VMs portala za Azure.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)


**Portal za azure**:

![Zaustavljen (deallocated) status VMs Azure klasični portala.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

Status "Zaustavljen (deallocated)" izdaje resurse računala, kao što su procesora, memorije i mreže. Diskova, no i dalje zadržavaju tako da možete brzo ponovno stvoriti u VM prema potrebi. Ove disketa pri vrhu VHDs koje se sigurnosno po Azure prostora za pohranu. Račun za pohranu sadrži ove VHDs, a na diskova uključeno leases te VHDs.

## <a name="next-steps"></a>Daljnji koraci

- [Brisanje računa za pohranu](storage-create-storage-account.md#delete-a-storage-account)
- [Kako se prelamaju zaključanih Zakup spremišta blobova platforme Microsoft Azure (PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
