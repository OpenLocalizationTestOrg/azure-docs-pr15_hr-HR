<properties
    pageTitle="Upute za stvaranje, upravljanje i brisanje računa za pohranu na portalu za Azure | Microsoft Azure"
    description="Stvaranje novog računa za pohranu, upravljanje tipke za pristup računu ili brisanje računa za pohranu na portalu za Azure. Informirajte se o računima standardnih ili premium prostora za pohranu."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# <a name="about-azure-storage-accounts"></a>O računima Azure za pohranu

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Pregled

Račun za Azure prostora za pohranu omogućuje jedinstveni prostor za naziv za pohranu i dohvaćanje objekte Azure pohrane podataka. Svi objekti na račun za pohranu naplatu zajedno kao grupu. Prema zadanim postavkama, na vašem računu podaci dostupni samo vama, vlasnik računa.

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Prostor za pohranu račun za naplatu

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [AZURE.NOTE] Prilikom stvaranja Azure virtualnog računala s računom za pohranu automatski se stvara umjesto vas na mjestu implementacije ako već nemate račun za pohranu na tom mjestu. Pa nije potrebno, slijedite upute za stvaranje računa za pohranu za vaše diskova virtualnog računala. Naziv računa spremišta će se temeljiti na nazivu virtualnog računala. Potražite u [dokumentaciji virtualnim računalima sustava Azure](https://azure.microsoft.com/documentation/services/virtual-machines/) više pojedinosti.

## <a name="storage-account-endpoints"></a>Krajnje točke račun za pohranu

Svaki objekt koje pohranjujete u prostor za pohranu Azure ima jedinstvene URL adrese. Naziv računa spremišta čini poddomene tu adresu. Kombinacija poddomene i naziv domene, što je specifične za svaku uslugu, čini *krajnja točka* za vaš račun za pohranu.

Ako, na primjer, ako vaš račun za pohranu zove *mystorageaccount*, zatim krajnje točke zadano za vaš račun za pohranu su:

- Bloba servisa: http://*mystorageaccount*. blob.core.windows.net

- Servis za tablice: http://*mystorageaccount*. table.core.windows.net

- Red čekanja servisa: http://*mystorageaccount*. queue.core.windows.net

- Datoteka servisa: http://*mystorageaccount*. file.core.windows.net

> [AZURE.NOTE] Račun za spremište blobova platforme samo izlaže Blob krajnju točku usluge.

URL za pristup objekta u račun za pohranu se temelji na kraj mjesto objekta u račun za pohranu krajnjoj. Na primjer, blob adresa možda ovaj oblik: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Možete konfigurirati i prilagođenog naziva domene za svoj račun za pohranu. Računi klasični prostora za pohranu potražite u članku [Konfiguriranje prilagođenu domenu naziv za svoje krajnju točku spremište blobova platforme](storage-custom-domain-name.md) detalje. Voditelj resursa za pohranu račune za tu mogućnost nisu dodani na [portal za Azure](https://portal.azure.com) još, ali možete je konfigurirati sa servisom PowerShell. Dodatne informacije potražite u članku cmdlet [Postavljanje AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) .  

## <a name="create-a-storage-account"></a>Stvaranje računa za pohranu

1. Prijavite se na [portal za Azure](https://portal.azure.com).

2. Na izborniku koncentrator odaberite **Novo** -> **podataka + prostor za pohranu** -> **računa za pohranu**.

3. Unesite naziv za vaš račun za pohranu. Detalje o kako koristiti naziv računa spremišta adrese objekte u Azure prostora za pohranu potražite u članku [krajnje točke račun za pohranu](#storage-account-endpoints) .

    > [AZURE.NOTE] Imena pohranu račun mora biti između 3 i 24 znakova i možda sadrži brojeve i samo mala slova.
    >  
    > Naziv računa spremišta mora biti jedinstvena u Azure. Portal za Azure će vas upozoriti ako odaberete naziv računa spremišta već se koristi.

4. Odredite model implementacije koja će se koristiti: **Voditelj resursa** ili **Klasični**. **Voditelj resursa** je model preporučene implementacije. Dodatne informacije potražite u članku [Implementacija upravljanja resursima za razumijevanje i klasični implementacije](../resource-manager-deployment-model.md).

    > [AZURE.NOTE] Računa spremišta blobova platforme moguće stvoriti samo pomoću model implementacije Voditelj resursa.

5. Odaberite vrstu računa za pohranu: **opće namjene** ili **spremište blobova platforme**. **Opće namjene** je zadana vrijednost.

    Ako je odabrana **opće namjene** , zatim navedite sloju performanse: **Standardno** ili **Premium**. Zadana je vrijednost **standardne**. Dodatne informacije o računi standardnih ili premium prostora za pohranu potražite u članku [Uvod u spremište na platformi Microsoft Azure](storage-introduction.md) i [prostora za pohranu Premium: visokih performansi prostor za pohranu radnih opterećenja virtualnog računala Azure](storage-premium-storage.md).

    Ako je odabrana **Blobova** , zatim navedite razina pristupa: **odlične**ili **Aktivno** . Zadano je **Aktivno**. U odjeljku [spremište blobova platforme Azure: zanimljivih i tipkovne razine](storage-blob-storage-tiers.md) više pojedinosti.

6. Odaberite mogućnost replikacije za račun za pohranu: **LRS**, **GRS**, **RA GRS**ili **ZRS**. Zadano je **RA GRS**. Dodatne informacije o mogućnostima replikacije Azure za pohranu potražite u članku [replikacijom Azure prostor za pohranu](storage-redundancy.md).

7. Odaberite pretplatu u koju želite stvoriti novi račun za pohranu.

8. Odredite novu grupu resursa ili odaberite postojeću grupu resursa. Dodatne informacije o grupama resursa potražite u članku [Pregled upravljanja resursima Azure](../azure-resource-manager/resource-group-overview.md).

9. Odaberite zemljopisnu lokaciju za vaš račun za pohranu. Dodatne informacije o servisima na koje su dostupne u koje regiji potražite u članku [Azure područja](https://azure.microsoft.com/regions/#services) .

10. Kliknite **Stvori** da biste stvorili račun za pohranu.

## <a name="manage-your-storage-account"></a>Upravljati računom za pohranu

### <a name="change-your-account-configuration"></a>Promjena konfiguracije računa

Kada stvorite račun za pohranu, možete izmijeniti svoju konfiguraciju, kao što su promjene mogućnosti replikacijom koristiti za račun i promjena razina pristupa za račun za spremište blobova platforme. [Portal za Azure](https://portal.azure.com)idite na račun servisa za pohranu, kliknite **sve postavke** , a zatim kliknite **konfiguracije** za prikaz i/ili promijenite konfiguracija računa.

> [AZURE.NOTE] Ovisno o performansama sloju koje ste odabrali prilikom stvaranja računa za pohranu, neke mogućnosti replikacije možda neće biti dostupne.

Promjena mogućnosti replikacije promijenit će se vaš cijene. Dodatne pojedinosti potražite u članku [cijene Azure prostora za pohranu](https://azure.microsoft.com/pricing/details/storage/) stranica.

Za račune spremište blobova platforme promjena razina pristupa mogu uzrokovati naknade za promjenu uz mijenjanje vaše cijene. Pročitajte članak [Blob prostora za pohranu računi – cijene i naplata](storage-blob-storage-tiers.md#pricing-and-billing) više pojedinosti.

### <a name="manage-your-storage-access-keys"></a>Upravljanje tipke za pristup za pohranu

Kada stvorite račun za pohranu, Azure stvara dva 512-bitni prostora za pohranu pristupnih tipki, koji se koriste za provjeru autentičnosti prilikom pristupa računu za pohranu. Unosom dvije tipke za pristup za pohranu Azure omogućuje Obnovi tipki bez prekida u funkcioniranju servisa za pohranu ili pristup tom servisu.

> [AZURE.NOTE] Preporučujemo da izbjegli tipke za pristup za pohranu za zajedničko korištenje s drugima. Dopustiti pristup resursima za pohranu bez dajete pristupnih tipki, možete koristiti *zajednički pristup potpis*. Potpis zajednički pristup omogućuje pristup resursa na vašem računu za interval koju ste definirali i s dozvolama koji navedete. Dodatne informacije potražite u članku [Korištenje zajednički pristup potpisa (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

#### <a name="view-and-copy-storage-access-keys"></a>Prikaz i kopiranje prostora za pohranu pristupnih tipki

[Portal za Azure](https://portal.azure.com)idite na račun servisa za pohranu, kliknite **sve postavke** , a zatim **tipke za pristup** za prikaz, kopiranje i Obnovi tipke za pristup računu. Plohu **Pristupnih tipki** obuhvaća i nizove unaprijed konfigurirana veze pomoću ključeva primarnih i sekundarnih koji možete kopirati za korištenje aplikacije.

#### <a name="regenerate-storage-access-keys"></a>Obnovi prostora za pohranu pristupnih tipki

Preporučujemo da promijenite tipke za pristup računu za pohranu povremeno radi zaštite vaše veze za pohranu. Dvije tipke za pristup dodjeljuju se tako da se veza s računom za pohranu možete održavati pomoću jedne tipkovnog dok Obnovi ostale pristupna tipka.

> [AZURE.WARNING] Ponovno stvaranje pristupnih tipki može utjecati na servisa Azure, kao i vlastite aplikacije koje ovise o računu za pohranu. Da biste koristili novi ključ mora se ažurirati sve klijente pomoću tipke programa access za pristup računu za pohranu.

**Media services** – ako imate media services koje ovise o računu za pohranu, morate ponovno sinkronizirate pristupnih tipki sa servisom za medijske sadržaje nakon Obnovi tipki.

**Aplikacija** – ako imate web-aplikacije ili servisa u oblaku koja se pomoću računa za pohranu, izgubit ćete veze ako Obnovi tipke, osim ako se poništiti ključeva.

**Prostor za pohranu programu Software Explorer** – ako koristite sve [aplikacije explorer prostora za pohranu](storage-explorers.md), vjerojatno morate da biste ažurirali ključa za pohranu koriste te aplikacije.

Ovo je postupak za rotiranje tipke za pristup za pohranu:

1. Ažurirajte niza veze u kodu aplikacije referentni sekundarne pristupni ključ računa za pohranu.

2. Obnovi primarni pristupni ključ za račun za pohranu. Na plohu **Pristupnih tipki** kliknite **Obnovi ključ1**, a zatim kliknite **da** da biste potvrdili da želite stvoriti novi ključ.

3. Ažuriranje veze nizova u svoj kod tako da upućuju na novi pristup primarni ključ.

4. Obnovi sekundarne tipkovni prečac na isti način.

## <a name="delete-a-storage-account"></a>Brisanje računa za pohranu

Da biste uklonili račun za pohranu koji više ne koristite, idite na račun za pohranu na [portal za Azure](https://portal.azure.com), a zatim kliknite **Izbriši**. Brisanje računa za pohranu briše sve račune, uključujući sve podatke na računu.

> [AZURE.WARNING] Nije moguće vratiti izbrisane prostora za pohranu račun ili dohvatiti sadržaj koji je sadržan prije brisanja. Obavezno sigurnosno kopirajte sve koje želite spremiti prije brisanja računa. To također se odnosi i za sve resurse u računa – nakon brisanja blob, tablice, reda čekanja ili datoteka se trajno izbrisati.

Da biste izbrisali račun za pohranu koji je povezan s Azure virtualnog računala, morate provjeriti da su izbrisane sve diskova virtualnog računala. Ako nehotice prvi put izbrišete vaše diskova virtualnog računala, zatim kada pokušate izbrisati s računom za pohranu, vidjet ćete poruka o pogrešci slična:

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Ako koristi račun za pohranu model implementacije klasični, možete ukloniti na disku virtualnog računala slijedeći ove korake za [Azure portal](https://manage.windowsazure.com):

1. Idite na [Klasični Azure portal](https://manage.windowsazure.com).
2. Idite na karticu virtualnih računala.
3. Kliknite karticu diskova.
4. Odaberite podatkovni disk, a zatim kliknite Izbriši Disk.
5. Da biste izbrisali slike na disku, idite na karticu slike i izbrisati sve slike pohranjene na računu.

Dodatne informacije potražite u [dokumentaciji Azure virtualnog računala](http://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="next-steps"></a>Daljnji koraci

- [Spremište blobova platforme Azure: Najbolju i aktivno razine](storage-blob-storage-tiers.md)
- [Azure replikacije prostora za pohranu](storage-redundancy.md)
- [Konfiguriranje nizove veze Azure prostora za pohranu](storage-configure-connection-string.md)
- [Prijenos podataka s AzCopy naredbenog retka uslužni](storage-use-azcopy.md)
- Posjetite [Blog tima za Azure prostora za pohranu](http://blogs.msdn.com/b/windowsazurestorage/).
