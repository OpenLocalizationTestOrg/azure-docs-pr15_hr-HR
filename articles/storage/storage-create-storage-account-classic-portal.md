<properties
    pageTitle="Upute za stvaranje, upravljanje i brisanje računa za pohranu na portalu klasični Azure | Microsoft Azure"
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


# <a name="about-azure-storage-accounts"></a>O računima Azure prostora za pohranu

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Pregled

Račun za Azure prostora za pohranu omogućuje vam pristup servisima za pohranu Azure blobova platforme Azure, reda čekanja, tablice i datoteke. Račun za pohranu omogućuje jedinstveni prostor za naziv za objekte Azure pohrane podataka. Prema zadanim postavkama, na vašem računu podaci dostupni samo vama vlasnika računa.

Postoje dvije vrste računa za pohranu:

- Standardni prostora za pohranu računa obuhvaća Blob, tablice, reda čekanja i datoteke za pohranu.
- Račun za pohranu premium trenutno podržava samo diskova Azure virtualnog računala. Potražite u članku [prostora za pohranu Premium: visokih performansi prostor za pohranu radnih opterećenja virtualnog računala Azure](storage-premium-storage.md) detaljnije pregled Premium prostora za pohranu.

## <a name="storage-account-billing"></a>Prostor za pohranu račun za naplatu

Se naplatiti za korištenje spremišta Azure koji se temelji na vašem računu za pohranu. Troškovi spremanja temelje se na četiri čimbenika: kapacitet pohrane, replikacije shemu, transakcije prostora za pohranu i izlazne podatke.

- Kapacitet pohrane se odnosi na koliki dio vašeg dodjele računa za pohranu ste pohranili podatke. Trošak jednostavno omogućuje pohrana podataka određuje koliko podataka spremanje i kako je replicirati.
- Replikacija određuje koliko kopije vaših podataka održavaju odjednom, a koje mjestima.
- Transakcije pogledajte sve čitanja i pisanja operacije Azure prostora za pohranu.
- Izlazne podatke se odnosi na podatak koji se prenose iz Azure regija. Kada aplikacije koja se izvodi u području iste podatke na računu za pohranu pristupati tu aplikaciju je li servis u oblaku ili neke druge vrste aplikacije, vam se naplatiti za izlazne podatke. (Za Azure servise koje možete poduzeti korake da biste grupirali podatke i usluge u istom centre za podataka da biste uklonili naknade za izlazne podatke.)  

Stranica za [Cijene za pohranu Azure](https://azure.microsoft.com/pricing/details/storage) sadrži detaljne podatke o cijenama za kapacitet pohrane, replikacijom i transakcije. Stranicu s [Detaljima cijene za prijenos podataka](https://azure.microsoft.com/pricing/details/data-transfers/) sadrži detaljne podatke o cijenama za izlazne podatke.

Detalje o prostora za pohranu računa kapacitetu i performanse ciljnih web-mjesta, potražite u članku [skalabilnost Azure prostora za pohranu i ciljeve](storage-scalability-targets.md).

> [AZURE.NOTE] Prilikom stvaranja Azure virtualnog računala s računom za pohranu automatski se stvara za mjesto implementaciju ako već nemate račun za pohranu na tom mjestu. Pa nije potrebno, slijedite upute za stvaranje računa za pohranu za vaše diskova virtualnog računala. Naziv računa spremišta će se temeljiti na nazivu virtualnog računala. Potražite u [dokumentaciji virtualnim računalima sustava Azure](https://azure.microsoft.com/documentation/services/virtual-machines/) više pojedinosti.

## <a name="create-a-storage-account"></a>Stvaranje računa za pohranu

1. Prijava na [Portal za Azure klasični](https://manage.windowsazure.com).

2. Na programskoj traci pri dnu stranice kliknite **Novo** . Odaberite **Podatkovni servisi** | **prostora za pohranu**, a zatim kliknite **Brzo stvaranje**.

    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. U **URL-a**unesite naziv računa za pohranu.

    > [AZURE.NOTE] Imena pohranu račun mora biti između 3 i 24 znakova i možda sadrži brojeve i samo mala slova.
    >  
    > Naziv računa spremišta mora biti jedinstvena u Azure. Klasični Portal za Azure će vas upozoriti ako odaberete naziv računa spremišta već preuzeli.

    Detalje o kako koristiti naziv računa spremišta adrese objekte u Azure prostora za pohranu potražite u članku [krajnje točke za pohranu račun](#storage-account-endpoints) ispod.

4. U **Grupi mjesto/afinitet**, odaberite mjesto za vaš račun za pohranu koji je blizu koje ili klijentima. Ako podatke na računu za pohranu pristupa s drugih servisa Azure, kao što su Azure virtualnog računala ili servis u oblaku, preporučujemo vam da biste odabrali grupu afinitet s popisa da biste grupirali računa za pohranu u centru za iste podatke s drugih servisa Azure koju koristite da biste poboljšali performanse i manji trošak.

    Imajte na umu da morate odaberite grupu afinitet stvaranja računa za pohranu. Postojeći račun nije moguće premjestiti u grupu afinitet. Dodatne informacije o grupama afinitet potražite u članku [mjesto servisa za suautorstvo s grupom afinitet](#service-co-location-with-an-affinity-group) ispod.

    >[AZURE.IMPORTANT] Da biste utvrdili koja mjesta dostupnih za vašu pretplatu, možete nazvati operacija [popis svih proizvođača resursa](https://msdn.microsoft.com/library/azure/dn790524.aspx) . Na popisu davatelje PowerShell [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx)poziva. Iz .NET, upotrijebite metodu " [popis](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) " klase ProviderOperationsExtensions.
    >
    >Uz to, potražite u članku [Azure područja](https://azure.microsoft.com/regions/#services) dodatne informacije o servisima na koje su dostupne u koje regiji.


5. Ako imate više pretplata Azure, prikazuje se polje **pretplate** . U **pretplatu**, unesite Azure pretplatu u koju želite koristiti za pohranu račun s.

6. U **replikacije**, odaberite željenu razinu replikacije za vaš račun za pohranu. Mogućnost preporučena replikacije je zemlj suvišnih replikacije u kojemu se nalazi Maksimalna rok trajanja za vaše podatke. Dodatne informacije o mogućnostima replikacije Azure prostora za pohranu potražite u članku [replikacije Azure prostora za pohranu](storage-redundancy.md).

6. Kliknite **Stvori račun za pohranu**.

    Može potrajati nekoliko minuta da biste stvorili račun za pohranu. Da biste provjerili status, možete nadzirati obavijesti pri dnu portalu klasični Azure. Nakon stvaranja računa za pohranu na novi račun za pohranu stanjem **mreži** i spreman za korištenje.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### <a name="storage-account-endpoints"></a>Krajnje točke račun za pohranu

Svaki objekt koje pohranjujete u prostor za pohranu Azure ima jedinstvene URL adrese. Naziv računa spremišta čini poddomene tu adresu. Kombinacija poddomene i naziv domene, što je specifične za svaku uslugu, čini *krajnja točka* za vaš račun za pohranu.

Ako, na primjer, ako vaš račun za pohranu zove *mystorageaccount*, zatim krajnje točke zadano za vaš račun za pohranu su:

- Bloba servisa: http://*mystorageaccount*. blob.core.windows.net

- Servis za tablice: http://*mystorageaccount*. table.core.windows.net

- Red čekanja servisa: http://*mystorageaccount*. queue.core.windows.net

- Datoteka servisa: http://*mystorageaccount*. file.core.windows.net

Vidjet ćete krajnjih točaka za vaš račun za pohranu na nadzornoj ploči za pohranu za [Azure klasični Portal](https://manage.windowsazure.com) kada je stvorena s računom.

URL za pristup objekta u račun za pohranu ugrađen dodavanjem mjesto objekta u račun za pohranu za krajnju točku. Na primjer, blob adresa možda ovaj oblik: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Možete konfigurirati i prilagođenog naziva domene za svoj račun za pohranu. Detalje potražite u članku [Konfiguriranje prilagođenog naziva domene za vaše krajnju točku spremište blobova platforme](storage-custom-domain-name.md) .

### <a name="service-co-location-with-an-affinity-group"></a>Mjesto servisa za suautorstvo s grupom afinitet

*Grupa afinitet* je zemljopisna grupiranja servisa Azure i VMs sa svojim računom Azure prostora za pohranu. Grupe sustava afinitet možete poboljšati performanse servisa pronalaženjem radnih opterećenja računalu u centru za iste podatke ili blizu ciljne publike korisnika. Osim toga, naknade za naplatu nastaje za izlazne prilikom pristupa s drugih servisa za koja je dio iste grupe afinitet podataka na račun za pohranu.

> [AZURE.NOTE]  Da biste stvorili grupu afinitet, otvorite područje <b>Postavke</b> [Azure klasični Portal](https://manage.windowsazure.com), kliknite <b>afinitet grupe</b>i pa kliknite <b>Dodaj grupu afinitet</b> ili gumb <b>Dodaj</b> . Stvaranje i upravljanje grupama afinitet pomoću upravljanja API-JA servisa Azure. Dodatne informacije potražite u članku <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">operacije afinitet grupe</a> .

## <a name="view-copy-and-regenerate-storage-access-keys"></a>Prikaz, kopiranje i Obnovi prostora za pohranu pristupnih tipki

Kada stvorite račun za pohranu, Azure stvara dva 512-bitni prostora za pohranu pristupnih tipki, koji se koriste za provjeru autentičnosti prilikom pristupa računu za pohranu. Unosom dvije tipke za pristup za pohranu Azure omogućuje Obnovi tipki bez prekida u funkcioniranju servisa za pohranu ili pristup tom servisu.

> [AZURE.NOTE] Preporučujemo da izbjegli tipke za pristup za pohranu za zajedničko korištenje s drugima. Dopustiti pristup resursima za pohranu bez dajete pristupnih tipki, možete koristiti *zajednički pristup potpis*. Potpis zajednički pristup omogućuje pristup resursu na vašem računu interval koje sami definirate i s dozvolama koje ste odredili. Dodatne informacije potražite u članku [Korištenje zajednički pristup potpisa (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

[Klasični Portal Azure](https://manage.windowsazure.com)tipkama **Upravljanje** na nadzornoj ploči ili stranici **prostora za pohranu** za prikaz, kopiranje i Obnovi pristupnih tipki za pohranu koji se koriste za pristup servisima Blob, tablice i red.

### <a name="copy-a-storage-access-key"></a>Kopiranje tipka za pristup za pohranu  

**Upravljanje tipki** možete koristiti da biste kopirali tipka za pristup za pohranu za korištenje u nizu za povezivanje. Niz za povezivanje potreban je naziv računa za pohranu i ključ za korištenje u provjeru autentičnosti. Dodatne informacije o konfiguriranju nizove veze da biste pristupili servisa Azure prostora za pohranu, potražite u članku [Konfiguriranje Azure prostora za pohranu nizu za povezivanje](storage-configure-connection-string.md).

1. [Klasični Portal Azure](https://manage.windowsazure.com)kliknite **prostora za pohranu**, a zatim naziv računa spremišta da biste otvorili na nadzornoj ploči.

2. Kliknite **Upravljanje tipke**.

    Otvorit će se **Upravljati pristupnih tipki** .

    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. Da biste kopirali tipka za pristup za pohranu, odaberite ključa tekst. Desnom tipkom miša, a zatim kliknite **Kopiraj**.

### <a name="regenerate-storage-access-keys"></a>Obnovi prostora za pohranu pristupnih tipki
Preporučujemo da promijenite tipke za pristup računu za pohranu povremeno radi zaštite vaše veze za pohranu. Dvije tipke za pristup dodjeljuju se tako da se veza s računom za pohranu možete održavati pomoću jednog tipkovnog dok Obnovi ostale pristupna tipka.

> [AZURE.WARNING] Ponovno stvaranje pristupnih tipki može utjecati na servisa Azure, kao i vlastite aplikacije koje ovise o računu za pohranu. Da biste koristili novi ključ mora se ažurirati sve klijente pomoću tipke programa access za pristup računu za pohranu.

**Media services** – ako imate media services koje ovise o računu za pohranu, morate ponovno sinkronizirate pristupnih tipki sa servisom za medijske sadržaje nakon Obnovi tipki.

**Aplikacija** – ako imate web-aplikacije ili servisa u oblaku koja se pomoću računa za pohranu, izgubit ćete veze ako Obnovi tipke, osim ako se poništiti ključeva. 

**Prostor za pohranu programu Software Explorer** – ako koristite sve [aplikacije explorer prostora za pohranu](storage-explorers.md), vjerojatno morate da biste ažurirali ključa za pohranu koriste te aplikacije.

Ovo je postupak za rotiranje tipke za pristup za pohranu:

1. Ažurirajte niza veze u kodu aplikacije referentni sekundarne pristupni ključ računa za pohranu.

2. Obnovi primarni pristupni ključ za račun za pohranu. [Klasični Portal Azure](https://manage.windowsazure.com)s na nadzornoj ploči ili stranici **Konfiguracija** kliknite **Upravljanje tipke**. Kliknite **Obnovi** u odjeljku primarni tipkovni prečac, a zatim kliknite **da** da biste potvrdili da želite stvoriti novi ključ.

3. Ažuriranje veze nizova u svoj kod tako da upućuju na novi pristup primarni ključ.

4. Obnovi sekundarne tipkovni prečac.

## <a name="delete-a-storage-account"></a>Brisanje računa za pohranu

Da biste uklonili račun za pohranu koji više ne koristite, koristite **Brisanje** na nadzornoj ploči ili stranici **Konfiguracija** . **Brisanje** briše cijelu pohranu računa, uključujući blob-ova, tablice i redova u računa.

> [AZURE.WARNING] Nije moguće vratiti izbrisane prostora za pohranu račun ili dohvatiti sadržaj koji je sadržan prije brisanja. Obavezno sigurnosno kopirajte sve koje želite spremiti prije brisanja računa. To također se odnosi i za sve resurse u računa – nakon brisanja blob, tablice, reda čekanja ili datoteka se trajno izbrisati.
>
> Ako vaš račun za pohranu sadrži VHD datoteka za Azure virtualnog računala, zatim morate izbrisati željene slike i diskova koji koriste te datoteke VHD prije brisanja računa za pohranu. Prvo Zaustavi virtualnog računala ako je pokrenut, a zatim izbrišite. Da biste izbrisali diskova, idite na karticu **diskova** i izbrisati sve diskova. Da biste izbrisali slike, idite na karticu **slike** i izbrisati sve slike pohranjene na računu.

1. [Klasični Portal Azure](https://manage.windowsazure.com)kliknite **prostora za pohranu**.

2. Kliknite bilo gdje na stavci računa za pohranu osim naziv, a zatim kliknite **Izbriši**.

     – Ili –

    Kliknite naziv računa spremišta da biste otvorili nadzornu ploču, a zatim kliknite **Izbriši**.

3. Kliknite **da** da biste potvrdili da želite izbrisati račun za pohranu.

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o Azure za pohranu potražite u [dokumentaciji Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/).
- Posjetite [Blog tima za Azure prostora za pohranu](http://blogs.msdn.com/b/windowsazurestorage/).
- [Prijenos podataka s AzCopy naredbenog retka uslužni](storage-use-azcopy.md)
