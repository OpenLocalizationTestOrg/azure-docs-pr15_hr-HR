<properties 
   pageTitle="Implementacija StorSimple snimke Upravitelj | Microsoft Azure"
   description="Saznajte kako preuzeti i instalirati snimke upravitelju StorSimple na BLOG dodatak za upravljanje značajkama zaštite i sigurnosno kopiranje StorSimple podataka."
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
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>Implementacija dodatka za BLOG Upravitelj StorSimple snimku

## <a name="overview"></a>Pregled

Snimka Upravitelj StorSimple je Microsoft Management Console (BLOG) dodatku koji olakšava zaštitu podataka i upravljanje sigurnosne kopije u okruženju sustava Microsoft Azure StorSimple. Pomoću upravitelja StorSimple snimke, možete upravljati Microsoft Azure StorSimple lokalnog i u oblaku za pohranu kao da je sustav potpuno integrirano prostora za pohranu, stoga pojednostavljivanje sigurnosnog kopiranja i vraćanja i smanjenju troškova. 

Pomoću ovog praktičnog vodiča opisuju preduvjeti za konfiguraciju, kao i postupke za instalaciju, uklanjanje i nadogradnje Upravitelj StorSimple snimke.

>[AZURE.NOTE] 
>
>- Upravitelj snimka StorSimple ne možete koristiti da biste upravljali Microsoft Azure StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualne uređaji).
>
>- Ako planirate instalirati StorSimple ažuriranju 2 na uređaju StorSimple, obavezno preuzmite najnoviju verziju programa StorSimple snimke Manager i instalirajte ga **prije instalacije StorSimple ažuriranju 2**. Najnoviju verziju programa StorSimple snimke Manager nije kompatibilna i radi sve objavljene verzije sustava Microsoft Azure StorSimple. Ako koristite prethodnu verziju StorSimple snimke upravitelja, morat ćete ažurirati (ne morate deinstalirati prethodnu verziju prije instalirati novu verziju).

## <a name="storsimple-snapshot-manager-installation"></a>Instalacija Upravitelj StorSimple snimke

Upravitelj snimka StorSimple moguće je instalirati na računalima sa sustavom Windows Server 2008 R2 SP1, Windows Server 2012 ili Windows Server 2012 R2 operacijski sustav. Na poslužiteljima koji se izvode Windows 2008 R2, morate i instalirajte Windows Server 2008 SP1 i Windows Management Framework 3.0. 

Prije instalacije ili nadogradnja dodatak za StorSimple snimke upravitelja za na Microsoft Management Console (BLOG), provjerite je li se ispravno konfigurirano Microsoft Azure StorSimple uređaja i glavnog računala poslužitelja. 

## <a name="configure-prerequisites"></a>Konfiguriranje preduvjeti

Sljedeći koraci pružaju više razine pregled zadataka konfiguracije da morate dovršiti prije instalacije snimku Upravitelj StorSimple. Dovršavanje konfiguracije Microsoft Azure StorSimple i informacije o postavljanju, uključujući sistemski preduvjeti i detaljne upute potražite u članku [uvođenja StorSimple uređaju lokalnog](storsimple-deployment-walkthrough.md).

>[AZURE.IMPORTANT]Prije početka, pregledajte [kontrolni popis za konfiguraciju implementacije](storsimple-deployment-walkthrough.md#deployment-configuration-checklist) i i [preduvjeti za implementaciju](storsimple-deployment-walkthrough.md#deployment-prerequisites) u [uvođenja lokalnog StorSimple uređaj](storsimple-deployment-walkthrough.md).
> <br>
 
### <a name="before-you-install-storsimple-snapshot-manager"></a>Prije instalacije Upravitelj StorSimple snimke

1. Otpakiravanje, postavljanje i priključite uređaj Microsoft Azure StorSimple kao što je opisano u [Instaliranje StorSimple 8100 uređaja](storsimple-8100-hardware-installation.md) ili ponovno [instalirati StorSimple 8600 uređaj](storsimple-8600-hardware-installation.md).

2. Provjerite je li se glavno računalo instaliran jedan od sljedećih operacijskih sustava:

    - Windows Server 2008 R2 (na poslužitelje s programom Windows 2008 R2, također morate instalirati Windows Server 2008 SP1 i Windows Management Framework 3.0)
    - Windows Server 2012.
    - Windows Server 2012 R2
 
    Za uređaj virtualne StorSimple, glavno računalo mora biti virtualnog računala za Microsoft Azure. 

3. Provjerite zadovoljavate li sve zahtjeve za konfiguraciju sustava Microsoft Azure StorSimple. Pojedinosti potražite [preduvjeti za implementaciju](storsimple-deployment-walkthrough.md#deployment-prerequisites).

4. Priključite uređaj na računalo koje hostira i izvođenje početne konfiguracije. Pojedinosti potražite [korake za implementaciju](storsimple-deployment-walkthrough.md#deployment-steps).

5. Stvaranje količine na uređaju, dodijeliti ih na glavno računalo i provjerite je li na računalo koje hostira možete postaviti te ih koristiti. Upravitelj snimka StorSimple podržava sljedeće vrste jedinicama: 

    - Osnovna jedinica
    - Jednostavne jedinice
    - Dinamičke jedinice
    - Zrcaljenje dinamičke jedinice (RAID 1)
    - Zajedničko korištenje klaster količine
 
    Informacije o stvaranju količine na uređaju StorSimple ili StorSimple virtualnog uređaja, idite na [korak 6: Stvaranje jedinice](storsimple-deployment-walkthrough.md#step-6-create-a-volume), u [uvođenja lokalnog StorSimple uređaj](storsimple-deployment-walkthrough.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Instaliranje novog voditelja StorSimple snimke

Prije instalacije Upravitelj StorSimple snimku, provjerite je li da na jedinicama stvorili na StorSimple uređaju ili StorSimple virtualnog uređaja postavljena, pokrenuti i oblikovana kao što je opisano u [Konfiguracija preduvjeti](#configure-prerequisites).

>[AZURE.IMPORTANT]
>
>- Za uređaj virtualne StorSimple, glavno računalo mora biti virtualnog računala za Microsoft Azure. 
>
>- Glavno računalo mora imati Windows 2008 R2, Windows Server 2012 ili Windows Server 2012 R2. Ako je vaš poslužitelj instaliran Windows Server 2008 R2, morate instalirati Windows Server 2008 SP1 i Windows Management Framework 3.0.
>
>- Možete se povezati s uređajem StorSimple snimke upravitelju, morate konfigurirati vezu s iSCSI s računala koje hostira StorSimple uređaj.

Slijedite ove korake da biste dovršili Osvježi instalaciju StorSimple snimku upravitelja. Ako instalirate nadogradnju, idite na [nadograditi ili ponovno instalirajte StorSimple snimke Manager](#upgrade-or-reinstall-storsimple-snapshot-manager).

- Korak 1: Instalacija Manager StorSimple snimke 
- Korak 2: Povezati StorSimple snimku Upravitelj uređaja 
- Korak 3: Provjerite stanje veze na uređaju 

###<a name="step-1-install-storsimple-snapshot-manager"></a>Korak 1: Instalacija Manager StorSimple snimke

Poduzmite sljedeće korake da biste instalirali Upravitelj StorSimple snimke.

#### <a name="to-install-storsimple-snapshot-manager"></a>Da biste instalirali Upravitelj StorSimple snimke

1. Preuzimanje softvera StorSimple snimke Manager (Idi na [Upravitelj StorSimple snimke](https://www.microsoft.com/download/details.aspx?id=44220) u Microsoft Download Center) i spremiti lokalno na glavnom računalu.

2. U Eksploreru za datoteke desnom tipkom miša kliknite mu komprimiranu mapu, a zatim kliknite **Izdvoji sve**.

3. U prozoru **izdvajanje Komprimirane (Zipped) mape** , u okviru **Odabir odredišta i izdvajanje datoteka** upišite ili kliknite Pregledaj put gdje biste željeli datoteka za izdvajanje. 

       >[AZURE.IMPORTANT] Upravitelj snimka StorSimple morate instalirati na pogon C:.
 
4. Potvrdite okvir **Prikaži izdvojenih datoteka po dovršetku** , a zatim kliknite **Izdvoji**.

    ![Izdvajanje datoteka dijaloškog okvira](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 

4. Po završetku izdvajanje otvorit će se odredišnu mapu. Dvokliknite ikonu postavljanje aplikacije koja će se pojaviti u odredišnu mapu.

5. Kada se pojavi poruka za **Postavljanje uspješan** , kliknite **Zatvori**. Trebali biste vidjeti ikonu Upravitelj snimka StorSimple na radnoj površini.

    ![ikona na radnoj površini](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>Korak 2: Povezati StorSimple snimke Upravitelj uređaja

Povezati StorSimple snimke Upravitelj uređaja StorSimple, poduzmite sljedeće korake.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>Da biste se povezali StorSimple snimke Upravitelj na uređaj

1. Kliknite ikonu Upravitelj snimka StorSimple na radnoj površini. Otvara se prozor Upravitelj StorSimple snimku. Prozor sadrži **opseg** okno, okno s **rezultatima** i **Udaljenost od središta** slajda. 

    ![Upravitelj snimka StorSimple korisničkog sučelja](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png) 

    - Okno **opseg** (u lijevom oknu) sadrži popis čvorove organizirane u strukturi stabla. Možete proširiti neke čvorove da biste odabrali ili prikazali određene podatke vezane uz taj čvor. Kliknite ikonu sa strelicom da biste proširili ili saželi čvor. Desnom tipkom miša kliknite stavku u oknu **opseg** da biste vidjeli popis dostupnih akcija za tu stavku. 

    - Okno s **rezultatima** (središnjeg okna) sadrži detaljno stanje podatke o čvor, prikaza ili podatke koji ste odabrali u oknu **opseg** .

    - U oknu **Akcije** navedeni operacije koje možete izvršiti na čvor, prikaza ili podatke koji ste odabrali u oknu **opseg** .

    Opis upravitelja snimka StorSimple korisničkog sučelja programa potražite u članku [Upravitelj snimka StorSimple korisničkog sučelja](storsimple-use-snapshot-manager.md).

2. U oknu **opseg** desnom tipkom miša kliknite čvor **uređaji** , a zatim **Konfiguracija uređaja**. Pojavit će se dijaloški okvir **Konfiguracija uređaja** .

    ![Konfiguriranje uređaja](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 

3. U okviru s popisom **uređaja** odaberite IP adresa uređaja Microsoft Azure StorSimple ili virtualnog uređaja. U tekstni okvir **Lozinka** upišite lozinku StorSimple snimke Manager koju ste stvorili za uređaj na portalu za Azure klasični. Kliknite **u redu**.

4. Upravitelj snimka StorSimple traži uređaj koji ste naveli. Ako je uređaj nije dostupan, Upravitelj snimka StorSimple dodaje veze. Možete [Provjerite stanje veze na uređaj](#to-verify-the-connection) da biste potvrdili uspješno dodati vezu.

    Ako uređaj nije dostupan zbog bilo kojeg razloga, Upravitelj snimka StorSimple vraća poruku o pogrešci. Kliknite **u redu** da biste zatvorili poruku o pogrešci, a zatim kliknite **Odustani** da biste zatvorili dijaloški okvir **Konfiguracija uređaja** .

5. Kada se povezuje s uređajem, StorSimple snimke Upravitelj uvozi svake grupe jedinica konfigurirana za taj uređaj, pod uvjetom da grupi glasnoću povezane sigurnosne kopije. Jedinica grupe koje imaju pridružene sigurnosnih kopija se ne uvoze. Uz to, sigurnosne pravilnike koje su stvorene za grupu jedinica se ne uvoze. Da biste vidjeli uvezene grupe, desnom tipkom miša kliknite čvor vrha do krajnjeg **Glasnoću grupe** u oknu **opseg** pa kliknite **Uključivanje i isključivanje uvesti grupe**.

### <a name="step-3-verify-the-connection-to-the-device"></a>Korak 3: Provjerite stanje veze na uređaju

Poduzmite sljedeće korake da biste provjerili povezani StorSimple snimke Upravitelj uređaja StorSimple.

#### <a name="to-verify-the-connection"></a>Da biste provjerili veze

1. U oknu **opseg** kliknite čvor **uređaji** .

    ![Stanje StorSimple snimke Upravitelj uređaja](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 

2. Provjerite okno s **rezultatima** : 

   - Ako zelenim pokazateljem pojavljuje se na ikoni uređaja i **dostupan** u stupcu **Status** , zatim uređaj povezan. 

   - Ako se crveni indikator pojavljuje se na ikoni uređaja i nije dostupan u stupcu **Status** , zatim željeni uređaj nije povezan. 

   - Ako se **Refreshing** u stupcu **Status** , StorSimple snimku Upravitelj je dohvaćanje glasnoću grupe i pridruženi sigurnosnog kopiranja za povezani uređaj.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Nadogradite ili ponovno instalirajte Manager StorSimple snimke

Upravitelj snimka StorSimple treba potpuno deinstalirati prije nadogradnje ili ponovno instalirati softver. 

Prije nego što ponovno instalirati Upravitelj StorSimple snimke, sigurnosnu kopiju postojeće StorSimple snimke Upravitelj baze podataka na glavnom računalu. Time informacije sigurnosne kopije pravila i konfiguraciju tako da se jednostavno možete vratiti podatke iz sigurnosne kopije.

Ako su nadogradnje ili ponovna instalacija Upravitelj StorSimple snimke, slijedite ove korake:

- Korak 1: Deinstalacija Upravitelj StorSimple snimke 
- Korak 2: Sigurnosnu kopiju baze podataka upravitelja StorSimple snimke 
- Korak 3: Ponovno instalirajte StorSimple snimke Manager i vraćanje baze podataka 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Korak 1: Deinstalirajte StorSimple snimku Manager

Poduzmite sljedeće korake da biste deinstalirali StorSimple snimku Manager.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>Da biste deinstalirali Upravitelj StorSimple snimke

1. Na glavnom računalu otvorite **Upravljačku ploču**, kliknite **programi**, a zatim kliknite **Programi i značajke**.

2. U lijevom oknu kliknite **Deinstaliranje ili promjena programa**.

3. Desnom tipkom miša kliknite **Upravitelj StorSimple snimke**, a zatim kliknite **Deinstaliraj**.

4. Time ćete StorSimple snimke Upravitelj instalacijski program. Kliknite **Postavljanje izmijeniti**, a zatim **Deinstaliraj**.

    >[AZURE.NOTE] Ako je bilo koji BLOG procesa koji se izvode u pozadini, kao što je upravitelj snimka StorSimple ili Disk Management, neće uspjeti Deinstalacija i primit ćete poruku da biste zatvorili sve instance BLOG prije nego se pokušate deinstalirati. Odaberite **automatski zatvorite programe i pokušajte ponovno pokrenuti ih nakon dovršetka postavljanja**, a zatim kliknite **u redu**.
 
5. Kada se Završi proces deinstalacije, pojavljuje se **Postavljanje uspješno** poruka. Kliknite **Zatvori**.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>Korak 2: Sigurnosnu kopiju baze podataka upravitelja StorSimple snimke

Stvorite i spremite kopiju baze podataka upravitelja StorSimple snimke, poduzmite sljedeće korake.

#### <a name="to-back-up-the-database"></a>Da biste sigurnosnu kopiju baze podataka

1. Zaustavljanje servisa za upravljanje Microsoft StorSimple:

   1. Pokrenite Upravitelj poslužitelja.

   2. Na nadzornoj ploči Upravitelj poslužitelja, na izborniku **Alati** odaberite **usluge**.

   3. Na stranici **servisa** odaberite **Microsoftov servis za upravljanje StorSimple**.

   4. U desnom oknu u odjeljku **Servis za upravljanje StorSimple Microsoft**, kliknite **Zaustavljanje servisa**.

        ![Zaustavljanje servisa StorSimple Manager](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)

2. Pronađite C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] Programskipodaci je skrivena mapa.

3. Pronalaženje datoteke XML kataloga, kopirajte datoteku i spremiti kopiju u sigurnom mjestu ili u oblaku.

    ![Datoteka StorSimple kataloga sigurnosnog kopiranja](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)

4. Ponovno pokretanje servisa za upravljanje Microsoft StorSimple: 

    1. Na nadzornoj ploči Upravitelj poslužitelja, na izborniku **Alati** odaberite **usluge**.

    2. Na stranici **servisa** odaberite **Microsoft StorSimple upravljanja uslug**e.

    3. U desnom oknu u odjeljku **Servis za upravljanje StorSimple Microsoft**, kliknite **ponovno pokrenite servis**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>Korak 3: Ponovno instalirajte StorSimple snimke Manager i vraćanje baze podataka

Da biste ponovno instalirali Upravitelj StorSimple snimke, slijedite korake u [instalirati novi Upravitelj snimka StorSimple](#install-a-new-storsimple-snapshot-manager). Vraćanje baze podataka StorSimple snimku Manager pa pomoću sljedećeg postupka.

#### <a name="to-restore-the-database"></a>Vraćanje baze podataka

1. Zaustavljanje servisa za upravljanje Microsoft StorSimple:

    1. Pokrenite Upravitelj poslužitelja.

    2. Na nadzornoj ploči Upravitelj poslužitelja, na izborniku **Alati** odaberite **usluge**.

    3. Na stranici **servisa** odaberite **Microsoftov servis za upravljanje StorSimple**.

    4. U desnom oknu u odjeljku **Servis za upravljanje StorSimple Microsoft**, kliknite **Zaustavljanje servisa**.

2. Pronađite C:\ProgramData\Microsoft\StorSimple\BACatalog. 

     >[AZURE.NOTE] Programskipodaci je skrivena mapa.

3. Izbrišite datoteke XML kataloga i zamijeniti s verzijom koju ste ranije spremili.

4. Ponovno pokretanje servisa za upravljanje Microsoft StorSimple: 

    1. Na nadzornoj ploči Upravitelj poslužitelja, na izborniku **Alati** odaberite **usluge**.

    2. Na stranici **servisa** odaberite **Microsoftov servis za upravljanje StorSimple**.

    3. U desnom oknu u odjeljku **Servis za upravljanje StorSimple Microsoft**, kliknite **ponovno pokrenite servis**.

## <a name="next-steps"></a>Daljnji koraci

- Da biste saznali više o upravitelju za StorSimple snimke, idite na [što je upravitelj StorSimple snimke?](storsimple-what-is-snapshot-manager.md).

- Da biste saznali više o korisničko sučelje upravitelja StorSimple snimke, otvorite [Upravitelj snimka StorSimple korisničkog sučelja](storsimple-use-snapshot-manager.md).

- Da biste saznali više o korištenju upravitelja StorSimple snimke, idite na [Korištenje StorSimple snimke upravitelja za administriranje StorSimple rješenje](storsimple-snapshot-manager-admin.md).
