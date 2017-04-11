<properties 
   pageTitle="Upravitelj snimka StorSimple sigurnosne kopije pravila | Microsoft Azure"
   description="U članku se opisuje kako koristiti dodatak za BLOG StorSimple snimke Upravitelj za stvaranje i upravljanje sigurnosne kopije pravila koja određuju zakazano sigurnosno kopiranje."
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
   ms.date="05/12/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>Korištenje StorSimple snimke upravitelja za stvaranje i upravljanje sigurnosne kopije pravila

## <a name="overview"></a>Pregled

Sigurnosne kopije pravila stvara raspored za sigurnosno kopiranje podataka glasnoću lokalno i u oblaku. Kada stvorite sigurnosne kopije pravila, možete odrediti i pravilnika o zadržavanju. (Najviše 64 snimke možete zadržati.) Dodatne informacije o pravilima za sigurnosne kopije potražite u članku [vrste sigurnosnu kopiju](storsimple-what-is-snapshot-manager.md#backup-type) u [StorSimple 8000 niz: rješenje oblaka hibridnog](storsimple-overview.md).

Pomoću ovog praktičnog vodiča u članku se objašnjava kako:

- Stvaranje sigurnosne kopije pravila 
- Uređivanje sigurnosne kopije pravila 
- Brisanje sigurnosne kopije pravila 

## <a name="create-a-backup-policy"></a>Stvaranje sigurnosne kopije pravila

Pomoću sljedećeg postupka za stvaranje novog pravila za sigurnosno kopiranje.

#### <a name="to-create-a-backup-policy"></a>Stvaranje sigurnosne kopije pravila

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** desnom tipkom miša kliknite **Sigurnosno kopiranje pravila**pa kliknite **Stvori sigurnosnu kopiju pravila**.

    ![Stvaranje sigurnosne kopije pravila](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    Pojavit će se dijaloški okvir **Stvori pravilo** . 

    ![Stvaranje pravila – Kartica Općenito.](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3. Na kartici **Općenito** ispunite sljedeće informacije:

   1. U tekstni okvir **naziv** upišite naziv pravila.

   2. U tekstni okvir **grupe jedinica** upišite naziv grupe jedinica pridružene pravila.

   3. Odaberite **lokalnu snimke** ili **oblaka snimke**.

   4. Odaberite broj snimke stanja da biste zadržali. Ako odaberete **sve**64 snimke bit će zadržati (maksimalno). 

4. Kliknite karticu **raspored** .

    ![Stvaranje pravila - zakazivanje](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5. Na kartici **raspored** unesite sljedeće podatke: 

   1. Kliknite potvrdni okvir **Omogući** da biste zakazali sljedeće sigurnosno kopiranje.

   2. U odjeljku **Postavke**odaberite **jednu sesiju**, **Dnevni**, **Tjedni**ili **mjesečni**. 

   3. U tekstni okvir **pokretanje** kliknite ikonu kalendara, a zatim odaberite datum početka.

   4. U odjeljku **Dodatne postavke**možete postaviti neobavezno ponavljanja rasporede i završni datum.

   5. Kliknite **u redu**.

Kada stvorite sigurnosne kopije pravila, u oknu **rezultata** prikazuje sljedeće informacije:

- **Naziv** – naziv sigurnosne kopije pravila.

- **Vrsta** – lokalne snimke ili oblaka snimke.

- **Grupa jedinica** – grupi jedinica pridružene pravila.

- **Zadržavanje** – broj snimaka zadržavaju; Maksimalna je 64.

- **Stvoreno** – datum koji je stvoren ovo pravilo.

- **Omogućeno** – pravilnik je li trenutno na snazi: **True** upućuje na to da ga na snazi; **False** znači da je nije na snazi. 

## <a name="edit-a-backup-policy"></a>Uređivanje sigurnosne kopije pravila

Da biste uredili postojeću sigurnosne kopije pravila pomoću sljedećeg postupka.

#### <a name="to-edit-a-backup-policy"></a>Da biste uredili sigurnosne kopije pravila

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini. 

2. U oknu **opseg** kliknite čvor **Sigurnosne kopije pravila** . Sigurnosne kopije pravila pojavljuju se u oknu s **rezultatima** . 

3. Desnom tipkom miša kliknite pravilo koje želite urediti, a zatim kliknite **Uredi**. 

    ![Uređivanje sigurnosne kopije pravila](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png) 

4. Kada se prikaže prozor za **Stvaranje pravila** , unesite promjene, a zatim kliknite **u redu**. 

## <a name="delete-a-backup-policy"></a>Brisanje sigurnosne kopije pravila

Pomoću sljedećeg postupka možete izbrisati sigurnosne kopije pravila.

#### <a name="to-delete-a-backup-policy"></a>Da biste izbrisali sigurnosne kopije pravila

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini. 

2. U oknu **opseg** kliknite čvor **Sigurnosne kopije pravila** . Sigurnosne kopije pravila pojavljuju se u oknu s **rezultatima** . 

3. Desnom tipkom miša kliknite sigurnosno kopiranje pravilo koje želite izbrisati, a zatim kliknite **Izbriši**.

4. Kada se pojavi poruka o potvrdi, kliknite **da**.

    ![Brisanje sigurnosne kopije pravila potvrdu](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [pomoću upravitelja snimka StorSimple za administriranje StorSimple rješenje](storsimple-snapshot-manager-admin.md).
- Saznajte kako [pomoću upravitelja snimka StorSimple za prikaz i upravljanje zadacima sigurnosne kopije](storsimple-snapshot-manager-manage-backup-jobs.md).
