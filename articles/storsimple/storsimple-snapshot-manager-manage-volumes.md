<properties 
   pageTitle="Upravitelj snimka StorSimple i količine | Microsoft Azure"
   description="U članku se opisuje kako pomoću dodatka za BLOG StorSimple snimke upravitelja za prikaz i upravljanje količine i konfigurirati sigurnosne kopije."
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

# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>Prikaz i upravljanje količine pomoću upravitelja StorSimple snimku

## <a name="overview"></a>Pregled

Koristite čvor Upravitelj snimka StorSimple **jedinicama** (u oknu **opseg** ) da biste odabrali količine i prikaz podataka o njima. Jedinice su navedene kao pogona koji odgovaraju jedinicama postavljen tako da na glavno računalo. Čvor **količine** prikazuje vrste jedinica koje podržava StorSimple, uključujući količine otkrio pomoću iSCSI i uređaja i lokalne jedinice. 

Dodatne informacije o podržanim količine otvorite [podrške za više vrsta glasnoću](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Popis jedinica u okno s rezultatima](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

Čvor **količine** omogućuje ponovni pregled i brisanje količine nakon StorSimple snimku Upravitelj otkrije ih. 

Pomoću ovog praktičnog vodiča objašnjava postupak možete dostupnosti, pokretanje, i oblikovati količine te koristite Upravitelj snimka StorSimple:

- Prikaz informacija o jedinicama 
- Brisanje količine
- Ponovni pregled količine 
- Konfiguriranje osnovne jedinice i sigurnosno kopiranje
- Konfiguriranje dinamičkih zrcaljene jedinice i sigurnosno kopiranje

>[AZURE.NOTE] Sve akcije čvor **glasnoću** dostupne su i u oknu **Akcije** .
 
## <a name="mount-volumes"></a>Postavljanje količine

Pomoću sljedećeg postupka postavljanja, pokrenuti i oblikovati StorSimple jedinicama. Ovaj postupak koristi upravljanje Disk, sustav utility za upravljanje tvrdom disku i odgovarajuće jedinice ili particije. Dodatne informacije o upravljanju disku otvorite [Upravljanje Disk](https://technet.microsoft.com/library/cc770943.aspx) na web-mjestu Microsoft TechNet.

#### <a name="to-mount-volumes"></a>Postavljanje količine

1. Na glavnom računalu, pokrenite iSCSI pokretaču Microsoft.

2. Navedite jednu od sučelja IP adrese kao cilj portal ili otkrivanje IP adresa i povezati s uređajem. Kada je uređaj povezan, jedinica bit će dostupne sa sustavom Windows. Dodatne informacije o korištenju iSCSI pokretaču Microsoft, prijeđite na odjeljak "Povezivanje s odredišnim uređajem iSCSI" u [Instaliranje i konfiguriranje Microsoft iSCSI pokretač][1].

3. Da biste pokrenuli Disk Management, koristite neku od sljedećih mogućnosti:

    - U okvir **Pokreni** upišite Diskmgmt.msc.

    - Pokrenite Upravitelj poslužitelja, proširite čvor **prostora za pohranu** , a zatim odaberite **Disk Management**.

    - Pokretanje **Administrativni alati**, proširite čvor **Upravljanje računalom** , a zatim **Upravljanje Disk**. 

    >[AZURE.NOTE] Da biste pokrenuli Disk Management morate koristiti administratorske ovlasti.
 
4. Izvršite na volume(s) mreži:

   1. U odjeljku Upravljanje Disk, desnom tipkom miša kliknite bilo koje jedinice označene **izvanmrežno**.

   2. Kliknite **Ponovna aktivacija Disk**. Na disku trebale bi biti označene **Online** kada je aktivirati disk.

5. Pokretanje na volume(s):

   1. Desnom tipkom miša kliknite otkriven količine.

   2. Na izborniku odaberite **Inicijalizacija diska**.

   3. U dijaloškom okviru **Pokretanje diska** odaberite diskova koji želite pokrenuti, a zatim **u redu**.

6. Oblikovanje jednostavnim jedinicama:

   1. Desnom tipkom miša kliknite jedinicu koju želite oblikovati.

   2. Na izborniku odaberite **Nova jednostavna jedinica**.

   3. Da biste oblikovali koristiti Čarobnjak za nova jednostavna jedinica:

      - Određivanje veličine jedinice.
      - Navedite slovo.
      - Odaberite datotečnog sustava NTFS.
      - Odredite veličinu jedinice za dodjelu 64 KB.
      - Izvođenje brzo oblikovanje.

7. Oblik s više particija količine. Upute za prijeđite na odjeljak, "Particije i jedinice" [Implementaciju upravljanja disku](https://msdn.microsoft.com/library/dd163556.aspx)in.

## <a name="view-information-about-your-volumes"></a>Prikaz informacija o vašem količine

Da biste vidjeli informacije o lokalnom i količine Azure StorSimple pomoću sljedećeg postupka.

#### <a name="to-view-volume-information"></a>Da biste pregledali informacije glasnoće

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini. 

2. U oknu **opseg** kliknite čvor **količine** . Pojavit će se popis lokalne i postavljenu jedinice, uključujući sve Azure StorSimple jedinice u oknu **rezultata** . Stupci u oknu s **rezultatima** su konfigurirati. (Desnom tipkom miša kliknite čvor **jedinicama** odaberite **Prikaz**, a zatim odaberite **Dodaj/Ukloni stupce**.)

    ![Konfiguriranje stupaca](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)

    Rezultati stupca | Opis 
    :--------------|:-------------
    Ime           | **Naziv** stupca sadrži slovo dodijeljene svakoj otkriven jedinici.
    Uređaj         | **Uređaj** stupac sadrži IP adresa uređaja povezanih s glavnim računalom.
    Naziv glasnoću uređaja | Stupac **Naziv glasnoću uređaja** sadrži naziv Glasnoća uređaja na kojem pripada Odabrana jedinica. To je naziv glasnoću definiran na portalu Azure klasični za tu određenu jedinicu.
    Pristup putova   | Stupac **Putova Access** prikazuje put pristupa na jedinicu. Ovo je pogon slovo ili postavljanja točke na kojoj je dostupan na glavnom računalu glasnoću.
 
## <a name="delete-a-volume"></a>Brisanje jedinica

Da biste izbrisali jedinice iz StorSimple snimku upravitelja, koristite sljedeći postupak.

>[AZURE.NOTE] Ako je dio bilo koje grupe jedinica nećete moći izbrisati jedinicu. (Mogućnost Izbriši nije dostupna za jedinice koje su članovi grupe jedinice). Morate izbrisati grupu čitave jedinice da biste izbrisali.


#### <a name="to-delete-a-volume"></a>Da biste izbrisali jedinica

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** kliknite čvor **količine** . 

3. U oknu s **rezultatima** desnom tipkom miša kliknite jedinicu koju želite izbrisati.

4. Na izborniku kliknite **Izbriši**. 

    ![Brisanje jedinica](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 

5. Pojavit će se dijaloški okvir **Izbriši jedinicu** . U tekstni okvir upišite **Potvrdi** , a zatim kliknite **u redu**.

6. Prema zadanim postavkama StorSimple snimke Upravitelj stvara sigurnosnu kopiju jedinice prije brisanja. Te mjere opreza možete zaštititi od gubitka podataka ako je slučajni. Upravitelj StorSimple snimke prikazuje tijek poruke **Automatskog snimke** dok sigurnosno glasnoću. 

    ![Automatsko snimke poruke](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Ponovni pregled količine

Da biste ponovo skenirali količine povezanih StorSimple snimke upravitelju pomoću sljedećeg postupka.

#### <a name="to-rescan-the-volumes"></a>Da biste ponovo skenirali jedinica

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** **količine**desnom tipkom miša, a zatim **ponovo Pretraži količine**.

    ![Ponovni pregled količine](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
 
    Ovaj postupak sinkronizira s StorSimple snimku Upravitelj popis jedinica. Sve promjene, kao što su nove jedinice ili izbrisane količine odrazit će se u rezultatima.

## <a name="configure-and-back-up-a-basic-volume"></a>Konfiguriranje i sigurnosno kopiranje osnovne jedinice

Koristite sljedeći postupak da biste konfigurirali sigurnosne kopije osnovne jedinice i zatim odmah pokrenuti sigurnosnu kopiju ili stvaranje pravila za zakazano sigurnosno kopiranje.

### <a name="prerequisites"></a>Preduvjeti

Prije nego što počnete:

- Provjerite je li se ispravno konfigurirano StorSimple uređaja i glavnog računala. Dodatne informacije, idite na [uvođenja StorSimple uređaju lokalnog](storsimple-deployment-walkthrough-u2.md).

- Instalacija i podešavanje upravitelja StorSimple snimke. Dodatne informacije, otvorite [Implementacija Upravitelj StorSimple snimke](storsimple-snapshot-manager-deployment.md).

#### <a name="to-configure-backup-of-a-basic-volume"></a>Da biste konfigurirali sigurnosne kopije osnovne jedinice

1. Stvaranje osnovne jedinice na uređaju StorSimple.

2. Postavljanje, pokrenuti i formatirali kao što je opisano u [dostupnosti količine](#mount-volumes). 

3. Kliknite ikonu Upravitelj snimka StorSimple na radnoj površini. Otvara se prozor Upravitelj StorSimple snimke. 

4. U oknu **opseg** desnom tipkom miša kliknite čvor **količine** , a zatim odaberite **Ponovni pregled količine**. Po završetku pregleda popis jedinica prikazivati u oknu **rezultata** . 

5. U oknu s **rezultatima** desnom tipkom miša kliknite jedinicu, a zatim **Stvori grupu jedinica**. 

    ![Stvaranje grupe za glasnoću](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 

6. U dijaloškom okviru **Stvaranje grupe za količinsko** upišite naziv grupe jedinica, dodijeliti količine i kliknite **u redu**.

7. U oknu **opseg** Proširite čvor **Grupe jedinica** . Nova grupa jedinica prikazivati u odjeljku čvor **Grupe jedinica** . 

8. Desnom tipkom miša kliknite naziv grupe jedinica.

    - Da biste započeli interaktivne (zahtjev) sigurnosno kopiranje, kliknite **Preuzimanje sigurnosne kopije**. 

    - Da biste zakazali automatske sigurnosnog kopiranja, kliknite **Stvaranje sigurnosne kopije pravila**. Na stranici **Općenito** odaberite grupu za količinsko s popisa. Na stranici **raspored** unesite detalje raspored. Kada završite, kliknite **u redu**. 

9. Da biste potvrdili sigurnosno kopiranje pokrene, proširite čvor **poslova** u oknu **opseg** , a zatim čvor **pokrenut** . Popis zadataka u tijeku pojavljuje se u oknu **rezultata** . 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Konfiguriranje i sigurnosno kopiranje dinamički zrcaljene jedinice

Izvršite sljedeće korake da biste konfigurirali sigurnosne kopije dinamičkih zrcaljene jedinice:

- Korak 1: Upravljanje Disk korištenje da biste stvorili dinamičnu zrcaljene jedinice. 

- Korak 2: Korištenje StorSimple snimke upravitelja da biste konfigurirali sigurnosne kopije.

### <a name="prerequisites"></a>Preduvjeti

Prije nego što počnete:

- Provjerite je li se ispravno konfigurirano StorSimple uređaja i glavnog računala. Dodatne informacije, idite na [uvođenja StorSimple uređaju lokalnog](storsimple-deployment-walkthrough-u2.md).

- Instalacija i podešavanje upravitelja StorSimple snimke. Dodatne informacije, otvorite [Implementacija Upravitelj StorSimple snimke](storsimple-snapshot-manager-deployment.md).

- Konfiguriranje dvije jedinice na uređaju StorSimple. (U primjerima dostupne jedinice su **Disk 1** i **2 na disku**.) 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>Korak 1: Korištenje Disk Management da biste stvorili dinamičnu zrcaljene jedinice

Upravljanje disk je sustav utility za upravljanje tvrdom disku i količine ili particije koje sadrže. Dodatne informacije o upravljanju disku otvorite [Upravljanje Disk](https://technet.microsoft.com/library/cc770943.aspx) na web-mjestu Microsoft TechNet.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Da biste stvorili dinamičnu zrcaljene jedinice

1. Da biste pokrenuli Disk Management, koristite neku od sljedećih mogućnosti: 

   - Otvorite okvir **Pokreni** upišite **Diskmgmt.msc**i pritisnite Enter.

   - Pokrenite Upravitelj poslužitelja, proširite čvor **prostora za pohranu** , a zatim odaberite **Disk Management**. 

   - Pokretanje **Administrativni alati**, proširite čvor **Upravljanje računalom** , a zatim **Upravljanje Disk**. 

2. Provjerite jeste li povezani s dvije jedinice dostupnih na uređaju StorSimple. (U ovom primjeru dostupne jedinice su **Disk 1** i **2 na disku**.) 

3. U prozoru Disk Management u desnom stupcu donjeg okna, desnom tipkom miša kliknite **1 na disku** , a zatim odaberite **Nova zrcaljene jedinica**. 

    ![Novi zrcaljene jedinice](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 

4. Na stranici čarobnjaka **Nova zrcaljene jedinica** kliknite **Dalje**.

5. Na stranici **Odaberite diskova** odaberite **Disk 2** u oknu **Odabrana** , kliknite **Dodaj**i zatim kliknite **Dalje**. 

6. Na stranici **Dodjela pogon slovo ili put** prihvatite zadane postavke, a zatim kliknite **Dalje**. 

7. Na stranici **Formatiranje jedinice** u okviru **Veličina jedinice za dodjelu** odaberite **64 K**. Potvrdite okvir **Izvedi brzo formatiranje** , a zatim kliknite **Dalje**. 

8. Na stranici **dovršavanje Nova jedinica zrcaljene** pregledajte postavke, a zatim **Završi**. 

9. Pojavljuje se poruka da biste naznačili osnovni disk bit će pretvoreni u dinamički disk. Kliknite **da**.

    ![Dinamički disk poruka](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 

10. U odjeljku Upravljanje Disk, provjerite je li na disku 1 i 2 na disku prikazuju kao dinamičke zrcaljene jedinice. (**Dinamički** prikazivati u stupcu stanje i boja trake kapaciteta promijenite u crvenu, koji označava zrcaljene jedinice.) 

    ![Na disku upravljanje zrcaljene dinamičkih diskova](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 
 
### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>Korak 2: Korištenje StorSimple snimku upravitelja da biste konfigurirali sigurnosne kopije

Pomoću sljedećeg postupka konfiguriranje dinamičkih zrcaljene jedinice i zatim odmah pokrenuti sigurnosnu kopiju ili stvaranje pravila za zakazano sigurnosno kopiranje.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Da biste konfigurirali sigurnosne kopije dinamičkih zrcaljene jedinice

1. Kliknite ikonu Upravitelj snimka StorSimple na radnoj površini. Otvara se prozor Upravitelj StorSimple snimke. 

2. U oknu **opseg** desnom tipkom miša kliknite čvor **količine** , a zatim odaberite **Ponovni pregled količine**. Po završetku pregleda popis jedinica prikazivati u oknu **rezultata** . Dinamični zrcaljene jedinice nalazi se kao jedna jedinica. 

3. U oknu s **rezultatima** desnom tipkom miša kliknite dinamički zrcaljene jedinice, a zatim **Stvori grupu jedinica**. 

4. U dijaloškom okviru **Stvaranje grupe jedinica** upišite naziv grupe jedinica, dinamički zrcaljene jedinice dodijeliti ovu grupu pa kliknite **u redu**. 

5. U oknu **opseg** Proširite čvor **Grupe jedinica** . Nova grupa jedinica prikazivati u odjeljku čvor **Grupe jedinica** . 

6. Desnom tipkom miša kliknite naziv grupe jedinica. 

    - Da biste započeli interaktivne (zahtjev) sigurnosno kopiranje, kliknite **Preuzimanje sigurnosne kopije**. 

    - Da biste zakazali automatske sigurnosnog kopiranja, kliknite **Stvaranje sigurnosne kopije pravila**. Na stranici **Općenito** odaberite grupu za količinsko s popisa. Na stranici **raspored** unesite detalje raspored. Kada završite, kliknite **u redu**. 

7. Sigurnosno kopiranje možete nadzirati dok se izvodi. U oknu **opseg** Proširite čvor **zadatke** , a zatim **pokrenut**, Detalji posla prikazuju se u oknu **rezultata** . Po završetku sigurnosno kopiranje detalje prenose se na popis posla **posljednje 24** sata. 

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [pomoću upravitelja snimka StorSimple za administriranje StorSimple rješenje](storsimple-snapshot-manager-admin.md).
- Saznajte kako [pomoću upravitelja snimka StorSimple za stvaranje i upravljanje grupama jedinica](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
