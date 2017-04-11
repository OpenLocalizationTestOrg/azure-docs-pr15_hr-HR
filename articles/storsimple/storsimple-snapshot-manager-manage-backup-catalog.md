<properties 
   pageTitle="Upravitelj snimku StorSimple katalog | Microsoft Azure"
   description="U članku se opisuje kako pomoću dodatka za BLOG StorSimple snimke upravitelja za prikaz i upravljanje katalog sigurnosne kopije."
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
   ms.date="04/26/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Korištenje upravitelja snimka StorSimple da biste upravljali katalog sigurnosne kopije

## <a name="overview"></a>Pregled

Primarni funkcija StorSimple snimke upravitelja je omogućuju stvaranje aplikacija dosljedan sigurnosne kopije StorSimple količine u obliku snimke. Snimke se navode u XML datoteku pod nazivom *katalog*. Katalog sigurnosne kopije organizira snimaka po jedinici grupi, a zatim po lokalne snimke ili oblak snimke. 

Pomoću ovog praktičnog vodiča opisuje kako možete koristiti čvor **Kataloga sigurnosnu kopiju** da biste dovršili sljedeće zadatke:

- Vraćanje jedinica 
- Kloniraj volumen ili grupe jedinica 
- Brisanje sigurnosne kopije 
- Oporavak datoteka
- Vraćanje baze podataka upravitelja Storsimple snimke

Katalog sigurnosne kopije možete vidjeti tako proširivanje čvor **Katalog** u oknu **opsega** i proširivanje grupe jedinica.

- Ako kliknete naziv grupe glasnoće, okno s **rezultatima** prikazuje broj lokalne snimke i dostupnih za grupu jedinica snimki oblaka. 

- Ako kliknete **Lokalne snimke** ili **Oblaka snimke**, okno s **rezultatima** prikazuje sljedeće informacije o svakom sigurnosne kopije snimke (ovisno o postavkama **Prikaz** ): 

    - **Naziv** – snimku snimljena vremena. 

    - **Vrsta** – to je li lokalne snimke ili snimku oblaka. 

    - **Vlasnik** – vlasnik sadržaja. 

    - **Dostupno** – li snimku trenutno nije dostupno. **True** pokazuje da je snimka dostupna je i može ih vratiti; **False** znači da je snimka više nije dostupna. 

    - **Uvezeno** – li uvezeni sigurnosnu kopiju. **True** ukazuje da sigurnosno kopiranje uvezeni iz servisa StorSimple Upravitelj trenutku uređaj nije konfiguriran u upravitelju StorSimple snimke; **False** znači da se ne uvoze, ali se stvorena StorSimple snimke Upravitelj. (Možete jednostavno prepoznati grupe sustava uvezene glasnoću jer se dodaje sufiks koji označavaju uređaj iz kojeg je uvezene grupi jedinica.)

    ![Katalog](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)

- Ako proširite **Lokalne snimke** ili **Oblaka snimke**, a zatim imenu pojedinačne snimke, okno s **rezultatima** prikazuje sljedeće informacije o snimke koju ste odabrali:

    - **Naziv** – glasnoće otkrije slovo. 

    - **Lokalni naziv** – naziv lokalni pogon (Ako je dostupna). 

    - **Uređaj** – naziv uređaja na kojem se nalazi glasnoću. 

    - **Dostupno** – li snimku trenutno nije dostupno. **True** pokazuje da je snimka dostupna je i može ih vratiti; **False** znači da je snimka više nije dostupna. 


## <a name="restore-a-volume"></a>Vraćanje jedinica

Koristite sljedeći postupak da biste vratili jedinice iz sigurnosne kopije.

#### <a name="prerequisites"></a>Preduvjeti

Ako već niste učinili, stvoriti glasnoću i glasnoću grupu, a zatim izbrišite glasnoću. Prema zadanim postavkama StorSimple snimke Upravitelj stvara sigurnosnu kopiju jedinice prije ako bude dovoljno je će se izbrisati. Ako slučajno izbrišete glasnoću ili ako je podatke potrebno šifriranjem iz bilo kojeg razloga te mjere opreza možete spriječiti gubitak podataka. 

Upravitelj snimka StorSimple prikazuje sljedeću poruku dok stvara precautionary sigurnosnu kopiju.

![Automatsko snimke poruke](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

>[AZURE.IMPORTANT]Nije moguće izbrisati jedinicu koja je dio grupe jedinica. Mogućnost Izbriši nije dostupna. <br>

#### <a name="to-restore-a-volume"></a>Da biste vratili jedinica

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini. 

2. U oknu **opseg** Proširite čvor **Kataloga sigurnosnog kopiranja** , proširivanje grupe glasnoće, a zatim kliknite **Lokalne snimke** ili **Oblak snimke**. Pojavit će se popis sigurnosne kopije snimke u oknu **rezultata** . 

3. Pronalaženje sigurnosne kopije koju želite vratiti, desnom tipkom miša, a zatim kliknite **Vrati**. 

    ![Vraćanje sigurnosne kopije kataloga](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 

4. Na stranici za potvrdu pregledajte pojedinosti, upišite **Potvrda**i zatim kliknite **u redu**. Upravitelj snimka StorSimple koristi sigurnosnu kopiju da biste vratili glasnoću. 

    ![Vraćanje poruke o potvrdi](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 

5. Možete nadzirati akciju vraćanja dok se izvodi. U oknu **opseg** Proširite čvor **poslova** , a zatim **pokretanje**. Detalji posla prikazuju se u oknu **rezultata** . Po dovršetku vraćanja posao pojedinosti posao prenose se na popisu **zadnje 24 sata** .

## <a name="clone-a-volume-or-volume-group"></a>Kloniraj volumen ili grupe jedinica

Koristite sljedeći postupak da biste stvorili njezinu (Kloniraj) glasnoću ili grupe jedinica.

#### <a name="to-clone-a-volume-or-volume-group"></a>Da biste Kloniraj volumen ili grupe jedinica

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** Proširite čvor **Kataloga sigurnosnog kopiranja** , proširivanje grupe jedinica pa kliknite **Oblaka snimke**. Pojavit će se popis sigurnosnih kopija u oknu **rezultata** .

3. Pronađite glasnoću ili glasnoću grupu koju želite Kloniraj, desnom tipkom miša kliknite glasnoću ili naziv grupe jedinica i kliknite **Kloniraj**. Pojavit će se dijaloški okvir **Kloniraj oblak snimke** .

    ![Kloniraj snimku oblaka](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Ispunite dijaloški okvir **Kloniraj oblaka snimku** na sljedeći način: 

    1. U tekstni okvir **naziv** upišite naziv za kloniranu jedinicu. Ovaj naziv pojavit će se čvor **količine** . 

    2. (Neobavezno) odaberite **pogon**, a zatim s padajućeg popisa odaberite slovo. 

    3. (Neobavezno) odaberite **Mapa (NTFS)**, upišite put do mape ili kliknite Pregledaj i odaberite mjesto za mapu. 

    4. Kliknite **Stvori**.

5. Po dovršetku postupka postupka kloniranja morate pokrenuti kloniranu glasnoću. Pokrenite Upravitelj poslužitelja, a zatim pokrenuli Disk Management. Detaljne upute potražite u članku [Postavljanje količine](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Kada je pokrenut glasnoću će se prikazati u odjeljku čvor **količine** u oknu **opseg** . Ako ne vidite glasnoće na popisu, osvježite popis jedinica (desnom tipkom miša kliknite čvor **količine** , a zatim kliknite **Osvježi**).

## <a name="delete-a-backup"></a>Brisanje sigurnosne kopije

Pomoću sljedećeg postupka možete izbrisati snimku iz sigurnosne kopije kataloga. 

>[AZURE.NOTE]Brisanje snimke stanja briše sigurnosne kopije podataka vezanih uz snimke. Međutim, postupak čišćenje podataka iz oblaka može potrajati neko vrijeme.<br>
 
#### <a name="to-delete-a-backup"></a>Da biste izbrisali sigurnosne kopije

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** Proširite čvor **Kataloga sigurnosnog kopiranja** , proširivanje grupe glasnoće, a zatim kliknite **Lokalne snimke** ili **Oblaka snimke**. Pojavit će se popis snimke u oknu **rezultata** . 

3. Desnom tipkom miša kliknite snimka koju želite izbrisati, a zatim kliknite **Izbriši**.

4. Kada se pojavi poruka o potvrdi, kliknite **u redu**. 

## <a name="recover-a-file"></a>Oporavak datoteka

Ako datoteku slučajno izbriše s jedinicu, datoteku možete oporaviti dohvaćanje snimku zaslona koja unaprijed datumi brisanja, pomoću snimku da biste stvorili Kloniraj jedinice, a kopiranja datoteke s kloniranu jedinice izvornu glasnoću.

#### <a name="prerequisites"></a>Preduvjeti

Prije početka, provjerite jeste li povezani s trenutnom sigurnosnu kopiju grupe za količinsko. Zatim izbrišite datoteke spremljene na jedan od količine u toj grupi jedinica. Na kraju, poduzmite sljedeće korake da biste vratili izbrisane datoteke iz sigurnosne kopije. 

#### <a name="to-recover-a-deleted-file"></a>Da biste oporavili izbrisane datoteke

1. Kliknite ikonu Upravitelj snimka StorSimple na radnoj površini. Pojavit će se prozor konzole Upravitelja StorSimple snimke. 

2. U oknu **opseg** Proširite čvor **Kataloga sigurnosnu kopiju** , a zatim Pregledaj da biste snimku zaslona koja sadrži izbrisane datoteke. Obično, trebali biste odabrati snimku zaslona koja je stvorena neposredno prije brisanja. 

3. Pronalaženje jedinicu koju želite Kloniraj, desnom tipkom miša, a zatim kliknite **Kloniraj**. Pojavit će se dijaloški okvir **Kloniraj oblaka snimku** .

    ![Kloniraj oblak snimke](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Ispunite dijaloški okvir **Kloniraj oblaka snimku** na sljedeći način: 

   1. U tekstni okvir **naziv** upišite naziv za kloniranu jedinicu. Ovaj naziv pojavit će se čvor **količine** . 

   2. (Neobavezno) Odaberite **pogon**, a zatim s padajućeg popisa odaberite slovo. 

   3. (Neobavezno) Odaberite **Mapu (NTFS)**, upišite put do mape ili kliknite **Pregledaj** i odaberite mjesto za mapu. 

   4. Kliknite **Stvori**. 

5. Po dovršetku postupka postupka kloniranja morate pokrenuti kloniranu glasnoću. Pokrenite Upravitelj poslužitelja i pokrenuli Disk Management. Detaljne upute potražite u članku [Postavljanje količine](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Kada je pokrenut glasnoću će se prikazati u odjeljku čvor **količine** u oknu **opseg** . 

    Ako ne vidite glasnoće na popisu, osvježite popis jedinica (desnom tipkom miša kliknite čvor **količine** , a zatim kliknite **Osvježi**).

6. Otvorite NTFS mapu koja sadrži kloniranu glasnoće, proširite čvor **količine** i otvorite kloniranu glasnoću. Pronađite datoteku koju želite oporaviti, a zatim kopirajte primarni jedinicu.

7. Nakon vratili datoteku, možete izbrisati NTFS mapu koja sadrži kloniranu glasnoću.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>Vraćanje baze podataka upravitelja StorSimple snimku

Trebali biste redovito sigurnosno kopirajte StorSimple snimke Upravitelj baze podataka na glavnom računalu. Ako je Izrada pojavi ili glavno računalo neće uspjeti zbog bilo kojeg razloga, pa je možete vratiti iz sigurnosne kopije. Stvaranje sigurnosne kopije baze podataka je ručnog procesa.

#### <a name="to-back-up-and-restore-the-database"></a>Sigurnosno kopiranje i vraćanje baze podataka

1. Zaustavljanje servisa za upravljanje Microsoft StorSimple:

    1. Pokrenite Upravitelj poslužitelja.

    2. Na nadzornoj ploči Upravitelj poslužitelja, na izborniku **Alati** odaberite **usluge**.

    3. U prozoru **usluge** odaberite **Microsoftov servis za upravljanje StorSimple**.

    4. U desnom oknu u odjeljku **Servis za upravljanje StorSimple Microsoft**, kliknite **Zaustavljanje servisa**.

2. Na glavnom računalu otvorite C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] Programskipodaci je skrivena mapa.
 
3. Pronalaženje datoteke XML kataloga, kopirajte datoteku i spremiti kopiju u sigurnom mjestu ili u oblaku. Glavno računalo ne uspije, koristite ove datoteke sigurnosne kopije za oporavak sigurnosne pravilnike koji ste stvorili u programu StorSimple snimku Manager.

    ![Azure StorSimple katalog sigurnosne kopije datoteke](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)

4. Ponovno pokretanje servisa za upravljanje Microsoft StorSimple: 

    1. Na nadzornoj ploči Upravitelj poslužitelja, na izborniku **Alati** odaberite **usluge**.
    
    2. U prozoru **usluge** odaberite **Microsoftov servis za upravljanje StorSimple**.

    3. U desnom oknu u odjeljku **Servis za upravljanje StorSimple Microsoft**, kliknite **ponovno pokrenite servis**.

5. Na glavnom računalu otvorite C:\ProgramData\Microsoft\StorSimple\BACatalog. 

6. Izbrišite XML datoteke kataloga i zamijeniti ga sigurnosne kopije verziju koju ste stvorili. 

7. Kliknite ikonu Upravitelj StorSimple brze snimke radne površine da biste pokrenuli StorSimple snimke Upravitelj. 

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o [korištenju StorSimple snimku Upravitelj administriranje StorSimple rješenje](storsimple-snapshot-manager-admin.md).
- Dodatne informacije o [StorSimple snimke upravitelja zadataka i tijekova rada](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).
