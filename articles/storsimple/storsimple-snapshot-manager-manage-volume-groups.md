<properties 
   pageTitle="Upravitelj snimka StorSimple glasnoću grupe | Microsoft Azure"
   description="U članku se opisuje kako pomoću dodatka za BLOG StorSimple snimke upravitelja za stvaranje i upravljanje grupama za količinsko."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>Korištenje StorSimple snimke upravitelja za stvaranje i upravljanje grupama za glasnoću

## <a name="overview"></a>Pregled

Čvor **Glasnoću grupe** možete koristiti na oknu **opseg** glasnoću grupama dodijeliti količine, prikaz podataka o grupu jedinica, zakazivanje sigurnosne kopije i uređivanje grupe za količinsko. 

Jedinice grupe su grupe povezanih jedinicama koristi da bi bili kopija aplikacije dosljedni. Dodatne informacije potražite u članku [količine i glasnoću grupe](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) i [Integracija sa servisom Kopiraj sjene jedinica za Windows](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

>[AZURE.IMPORTANT] 
>
> * Sve jedinice u grupi jedinica mora potjecati iz usluga za jednu oblaka.
> 
> * Kada konfigurirate glasnoću grupe, miješati klaster zajednički količine (CSVs) i koje nisu-CSVs u istoj grupi jedinica. Upravitelj snimku StorSimple ne podržava CSVs i koje nisu CSVs u istom snimku.
 
![Čvor grupe jedinica](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Slika 1: StorSimple snimke Upravitelj glasnoću grupe čvor** 

Pomoću ovog praktičnog vodiča objašnjavaju kako koristiti upravitelj snimka StorSimple:

- Prikaz informacija o grupama za glasnoću 
- Stvaranje grupe za glasnoću
- Stvaranje sigurnosne kopije grupu jedinica
- Uređivanje grupe jedinica
- Brisanje grupe za glasnoću

Sve ove akcije dostupne su i u oknu **Akcije** .
 
## <a name="view-volume-groups"></a>Prikaz grupa jedinica

Ako kliknete čvor **Glasnoću grupe** , okno s **rezultatima** prikazuje sljedeće informacije o svake grupe glasnoću, ovisno o stupcu odabiri. (Stupci u oknu s **rezultatima** se može konfigurirati. Desnom tipkom miša kliknite čvor **količine** , odaberite **Prikaz**i zatim odaberite **Dodaj/Ukloni stupce**.)

Rezultati stupca | Opis 
:--------------|:------------ 
Ime           | **Naziv** stupca sadrži naziv grupe jedinica.
Aplikacija    | U stupcu **aplikacija** prikazuje broj VSS autorima trenutno instalirati i pokrenuti na glavnom računalu za Windows.
Odabrana       | **Odabrani** stupac prikazuje broj količine koje se nalaze u grupi jedinica. Vrijednost nula (0) pokazuje da nema aplikacije pridružen jedinice u grupi jedinica.
Uvoza       | U stupcu **uvezeno** prikazuje broj uvezene količine. Kada postavite na **True**, ovaj stupac pokazuje da je grupu jedinica uvezeni iz Azure klasični portal i je stvorena u StorSimple snimke Manager.
 
>[AZURE.NOTE] Upravitelj snimka StorSimple glasnoću grupe i prikazuju se na kartici **Sigurnosne kopije pravila** na portalu za Azure klasični.
 
## <a name="create-a-volume-group"></a>Stvaranje grupe za glasnoću

Koristite sljedeći postupak da biste stvorili grupu jedinica.

#### <a name="to-create-a-volume-group"></a>Da biste stvorili grupu jedinica

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini. 

2. U oknu **opseg** desnom tipkom miša kliknite **Glasnoću grupe**, a zatim **Stvori grupu jedinica**. 

    ![Stvaranje grupe za glasnoću](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
 
    Pojavit će se dijaloški okvir **Stvaranje grupe jedinica** . 

    ![Dijaloški okvir grupe jedinica stvaranje](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png) 

3.  Unesite sljedeće podatke: 

    1. U okvir **naziv** upišite jedinstveni naziv za novu grupu jedinica. 

    2. U okviru **aplikacije** odaberite aplikacije koje su pridružene jedinice koje će dodavanje u grupu jedinica. 

        Popisi za **aplikacije** okvir samo aplikacije koje koriste StorSimple količine i imaju VSS autorima omogućen za njih. VSS writer omogućena je samo ako su sve jedinicama u writer je upoznat StorSimple jedinicama. Ako aplikacija okvir je prazan, pa nema aplikacije koje su podržani VSS autorima i koristiti Azure StorSimple jedinicama instaliraju. (Trenutno Azure StorSimple podržava Microsoft Exchange i SQL Server.) Dodatne informacije o autorima VSS potražite u članku [integraciju sa servisom Kopiraj sjene jedinica za Windows](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

        Ako odaberete aplikaciju, sve jedinicama pridružena automatski odabrati. Nasuprot tome, ako odaberete količine pridružena određenoj aplikaciji, aplikacija je automatski odabran u okviru **aplikacije** . 

    3. U okviru **količine** odaberite StorSimple jedinice da biste dodali grupu jedinica. 

      - Možete uključiti količine s jednom ili više particija. (Više particija jedinica može biti dinamičkih diskova ili osnovnih diskova s više particija.) Jedna jedinica se smatra jedinicu koja sadrži više particija. Dakle, ako samo jedna particija dodali u grupu jedinica, sve ostale particije automatski se dodaju u toj grupi jedinica u isto vrijeme. Nakon što dodate više particija jedinice u grupu jedinica, i dalje glasnoće više particija tretirati kao jedna jedinica.

      - Možete stvoriti prazan glasnoću grupe tako da ne dodijelite sve jedinicama ih. 

      - Izradite klaster zajednički količine (CSVs) i koje nisu-CSVs u istoj grupi jedinica. Upravitelj snimku StorSimple ne podržava CSV količine i količine koje nisu CSV u istom snimku. 

4. Kliknite **u redu** da biste spremili grupi jedinica.

## <a name="back-up-a-volume-group"></a>Stvaranje sigurnosne kopije grupu jedinica

Pomoću sljedećeg postupka sigurnosnu grupu jedinica.

#### <a name="to-back-up-a-volume-group"></a>Sigurnosnu grupu jedinica

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** Proširite čvor **Glasnoću grupe** , desnom tipkom miša kliknite naziv grupe jedinica pa kliknite **Poduzeti sigurnosnu kopiju**. 

    ![Odmah sigurnosnu grupu jedinica](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)

3. U dijaloškom okviru **Iskoristite sigurnosno kopiranje** odaberite **Lokalni snimke** ili **Oblaka snimke**, a zatim **Stvori**. 

    ![Iskoristite sigurnosne kopije dijaloški okvir](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png) 

4. Da biste potvrdili da nije ostalo sigurnosnog kopiranja, proširite čvor **zadatke** , a zatim **pokretanje**. Stvaranje sigurnosne kopije mora biti naveden.

5. Da biste vidjeli dovršene snimke, proširite čvor **Katalog sigurnosne kopije** , proširite naziv grupe jedinica, a zatim kliknite **Lokalne snimke** ili **Oblaka snimke**. Stvaranje sigurnosne kopije će se prikazati ako je uspješno dovršena. 

## <a name="edit-a-volume-group"></a>Uređivanje grupe jedinica

Koristite sljedeći postupak da biste uredili grupu jedinica.

#### <a name="to-edit-a-volume-group"></a>Da biste uredili grupu jedinica

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** Proširite čvor **Glasnoću grupe** , desnom tipkom miša kliknite naziv grupe jedinica i kliknite **Uredi**. 

3. Pojavit će se dijaloški okvir **Stvaranje grupe jedinica **. Možete promijeniti **naziv**, **aplikacije**i **količine** stavke. 

4. Kliknite **u redu** da biste spremili promjene.

## <a name="delete-a-volume-group"></a>Brisanje grupe za glasnoću

Koristite sljedeći postupak da biste izbrisali grupu jedinica. 

>[AZURE.WARNING] Time ćete ukloniti i sve sigurnosne kopije pridružene grupi jedinica.

#### <a name="to-delete-a-volume-group"></a>Da biste izbrisali grupu jedinica

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini. 

2. U oknu **opseg** Proširite čvor **Glasnoću grupe** , desnom tipkom miša kliknite naziv grupe jedinica i zatim kliknite **Izbriši**. 

3. Pojavit će se dijaloški okvir **Brisanje grupa glasnoću** . Upišite **Potvrda** u tekstni okvir, a zatim kliknite **u redu**. 

    Grupi izbrisanih glasnoću nestaje s popisa u oknu s **rezultatima** i sve sigurnosne kopije koje su vezane uz tu grupu jedinica brišu se iz sigurnosne kopije kataloga.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [pomoću upravitelja snimka StorSimple za administriranje StorSimple rješenje](storsimple-snapshot-manager-admin.md).
- Saznajte kako [pomoću upravitelja snimka StorSimple za stvaranje i upravljanje sigurnosne kopije pravila](storsimple-snapshot-manager-manage-backup-policies.md).
