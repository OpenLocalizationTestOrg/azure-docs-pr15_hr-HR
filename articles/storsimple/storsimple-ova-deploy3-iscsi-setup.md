<properties 
   pageTitle="Instalacija poslužitelja iSCSI StorSimple virtualne polja | Microsoft Azure"
   description="U članku se opisuje kako izvršiti početnog postavljanja, registrirati iSCSI poslužitelja StorSimple i dovršite postavljanje uređaja."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/18/2016"
   ms.author="alkohli" />


# <a name="deploy-storsimple-virtual-array--set-up-your-virtual-device-as-an-iscsi-server"></a>Implementacija polja virtualne StorSimple – postavljanje virtualne uređaja kao iSCSI poslužitelj

![tijek postupka za postavljanje iSCSI](./media/storsimple-ova-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Pregled

Ovaj vodič za implementaciju primjenjuje se na Microsoft Azure StorSimple virtualne polja (poznat i kao virtualnog uređaja za lokalni StorSimple ili StorSimple virtualnog uređaja) izvodi ožujak 2016 Općenito dostupan (GA) izdanje. Pomoću ovog praktičnog vodiča opisuje izvođenje početnog postavljanja, registrirati iSCSI poslužitelja StorSimple, dovršili postavljanje uređaja i zatim stvaranje, dostupnosti, pokretanje i oblikovanje količine na poslužitelj za iSCSI virtualnog uređaja StorSimple. Informacije o postavljanju StorSimple u ovom članku primjenjuje se samo StorSimple virtualne polja. 

Postupci opisani u nastavku proći približno 30 minuta na 1 h da biste dovršili. Informacije objaviti u ovom članku odnose se samo StorSimple virtualne polja.

## <a name="setup-prerequisites"></a>Preduvjeti za instalaciju

Prije konfigurirati i postavljanje uređaja virtualne StorSimple, učinite sljedeće:

- Imate dodjeli virtualnog uređaja i s njom povezano kao što je opisano u [Implementacija StorSimple virtualne polju - dodjele virtualne polja u Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) ili [Uvođenje StorSimple virtualne polja - dodjele virtualne polja u VMware](storsimple-ova-deploy2-provision-vmware.md).

- Imate ključa za registraciju servisa StorSimple Upravitelj servisa koji ste stvorili za upravljanje StorSimple virtualne uređajima. Dodatne informacije potražite u članku **Korak 2: Registracija ključ servisa** u [Implementacija StorSimple virtualne polju – Priprema portalu](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key).

- Ako je drugi ili kasnije virtualne uređaj koji se registrirate za postojeći servis za Upravitelj StorSimple, imat ćete ključa za šifriranje podataka servisa. Ovaj ključ je generirana pri prvom uređaju uspješno je registriran na taj servis. Ako ste izgubili ovog ključa, potražite u članku **Početak ključa za šifriranje servisa podataka** koristi [Web korisničko Sučelje za administraciju sustava StorSimple virtualne polja](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Detaljnog postavljanja 

Pomoću sljedećih detaljne upute za postavljanje i konfiguriranje StorSimple virtualnog uređaja:

-  [Korak 1: Dovršavanje postavljanja korisničkog Sučelja lokalne web i registrirajte uređaj](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
-  [Korak 2: Dovršavanje postavljanja potrebna uređaja](#step-2-complete-the-required-device-setup)
-  [Korak 3: Dodavanje jedinica](#step-3-add-a-volume)
-  [Korak 4: Postavljanje, pokrenuti i oblikovati jedinica](#step-4-mount-initialize-and-format-a-volume)  

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Korak 1: Dovršavanje postavljanja korisničkog Sučelja lokalne web i registrirajte uređaj 

#### <a name="to-complete-the-setup-and-register-the-device"></a>Da biste dovršili postavljanje i registrirali uređaj

1. Otvorite prozor preglednika i povežite se s webom korisničkog Sučelja tako da upišete:

    `https://<ip-address of network interface>`

    Pomoću URL veze u prethodnom koraku. Prikazat će se pogreška koja obavještava da postoji problem sa sigurnosnim certifikatom web-mjesta. Kliknite **Nastavi da biste ovu web-stranicu**.

    ![Pogreška sigurnosnog certifikata](./media/storsimple-ova-deploy3-iscsi-setup/image3.png)

2. Prijavite se na webu UI virtualnog uređaja kao **StorSimpleAdmin**. Unesite lozinku administratora uređaja koji ste promijenili u koraku 3: pokretanje virtualnog uređaja u [Implementacija StorSimple virtualne polja - dodjele virtualnog uređaja u Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) i [Implementacija StorSimple virtualne polja - dodjele virtualnog uređaja u VMware](storsimple-ova-deploy2-provision-vmware.md).

    ![Stranica za prijavu](./media/storsimple-ova-deploy3-iscsi-setup/image4.png)

3. Će biti preusmjereni na stranicu **Početna stranica** . Ova stranica opisuje različite postavke možete konfigurirati i registrirati virtualnog uređaja sa servisom StorSimple Manager. Imajte na umu **mrežne postavke**, **Postavke proxyja Web**i **postavke vremena** nisu obavezni. Samo potrebne postavke su **Postavke uređaja** i **oblaka**.

    ![Početna stranica](./media/storsimple-ova-deploy3-iscsi-setup/image5.png)

4. Na stranici **postavke mreže** u odjeljku **mreža sučelja**podataka 0 će se automatski konfigurirati za vas. Svaki sučelje mreže postavljena prema zadanim postavkama automatski dohvatiti IP adresa (DHCP). Zbog toga IP adresu, podmreže i pristupnik dodijelit će se automatski (za IPv4 i IPv6).

    Prilikom planiranja za implementaciju uređaju kao poslužitelj iSCSI (za dodjelu resursa za pohranu bloka), preporučujemo da onemogućite mogućnost za **dohvaćanje IP adrese automatski** i konfiguriranje statičke IP adrese.

    ![Stranica postavke mreže](./media/storsimple-ova-deploy3-iscsi-setup/image6.png)

    Ako ste dodali više od jedne mreže sučelja tijekom dodjeljivanja uređaj, možete ih konfigurirati ovdje. Imajte na umu mrežno sučelje možete konfigurirati kao IPv4 samo ili kao IPv4 i IPv6. IPv6 ne podržava samo konfiguracije.

5. DNS poslužitelji potrebni su jer se koriste kada uređaj pokuša možete komunicirati s davateljima usluga za pohranu na cloud ili riješiti uređaju naziv ako je konfiguriran kao datotečnom poslužitelju. Na stranici **postavke mreže** , u odjeljku **DNS poslužitelji**:

    1. Primarnih i sekundarnih DNS poslužitelj će se automatski konfigurirati. Ako se odlučite za konfiguriranje statičke IP adrese, možete odrediti DNS poslužitelji. Za visoke dostupnosti, preporučujemo da konfigurirate primarne i sekundarne DNS poslužitelj.

    2. Kliknite **Primijeni**. To će se primijeniti i Provjerite valjanost postavki mreže.

6. Na stranici **Postavke uređaja** :

    1. Dodijelite jedinstveni **naziv** na uređaj. Ovaj naziv može sadržavati najviše 1-15 znakova i mogu sadržavati slova, brojeve i spojnice.

    2. Kliknite ikonu **iSCSSI poslužiteljem** ![iSCSI poslužitelja ikona](./media/storsimple-ova-deploy3-iscsi-setup/image7.png) za **vrste** uređaja koje stvarate. Poslužitelj za iSCSI će vam omogućiti za pohranu bloka dodjele resursa.

    3. Navedite želite li da se ovaj uređaj da biste se pridružili za domenu. Ako je vaš uređaj iSCSSI poslužiteljem, pridruživanje domene nije obavezno. Ako odlučite da ne uključuj iSCSSI poslužiteljem s domenom, kliknite **Primijeni**, pričekajte da se postavke primjenjivati, a zatim prijeđite na sljedeći korak.

        Ako se želite uključiti uređaj s domenom. Unesite **naziv domene**, a zatim kliknite **Primijeni**.

        > [AZURE.NOTE] Ako uključivanje iSCSSI poslužiteljem s domenom, provjerite je li vaše virtualne polja je u vlastitom organizacijsku jedinicu (OU) za Microsoft Azure Active Directory i bez objekti pravilnika grupe (GPO) primjenjuju se na njega.

    5. Pojavit će se dijaloški okvir. Unesite vjerodajnice za domenu u navedenom obliku. Kliknite ikonu provjeri ![Provjera ikona](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Provjerit će se vjerodajnice domene. Prikazat će se poruka o pogrešci ako nisu pravilne vjerodajnice.

        ![vjerodajnice](./media/storsimple-ova-deploy3-iscsi-setup/image8.png)

    6. Kliknite **Primijeni**. To će se primijeniti i Provjerite valjanost postavki uređaja.
 
7. (Nije obavezno) konfigurirajte web-proxy poslužitelj. Iako web-konfiguracija proxy nije obavezan, imajte na umu da ako koristite proxy poslužitelj za web, možete samo ga konfigurirati ovdje.

    ![Konfiguriranje web proxy poslužitelja](./media/storsimple-ova-deploy3-iscsi-setup/image9.png)

    Na stranici **Web proxy** :

    1. Navedite **web-URL proxy poslužitelja** u ovom obliku: *http://host-IP adresu* ili *FDQN:Port broj*. Imajte na umu da HTTPS URL-ova nisu podržane.

    2. Navedite **provjeru autentičnosti** kao **Osnovni** ili **ništa**.

    3. Ako koristite provjeru autentičnosti, također ćete morati navede **korisničko ime** i **lozinku**.

    4. Kliknite **Primijeni**. To će provjeriti i primijenite postavke proxyja konfigurirani web.
 
8. (Nije obavezno) konfigurirati postavke za svoj uređaj, kao što su vremenske zone i NTP poslužitelje primarnih i sekundarnih datuma i vremena. NTP poslužitelje potrebni su jer uređaj, morate sinkronizirati vrijeme tako da možete uspješnoj vaše oblaka davateljima usluga.

    ![Postavke vremena](./media/storsimple-ova-deploy3-iscsi-setup/image10.png)

    Na stranici **postavke vremena** :

    1. S padajućeg popisa odaberite **vremensku zonu** na temelju zemljopisnu lokaciju u kojem se uvodi uređaj. Zadanu vremensku zonu za svoj uređaj je pst datoteke. Uređaja koristit će se ovaj vremenske zone za sve operacije zakazano.

    2. Određivanje **primarnog NTP poslužitelja** za svoj uređaj ili prihvatite zadane vrijednosti time.windows.com. Provjerite dopušta li mrežom NTP promet prenesite iz vaše podatkovnog centra s Internetom.

    3. Po želji možete navesti **sekundarnog NTP poslužitelja** za svoj uređaj.

    4. Kliknite **Primijeni**. To će se provjeriti i primjenu postavki konfiguriranog vremena.

9. Konfiguriranje postavki oblaka za svoj uređaj. U ovom ćete koraku će dovršili konfiguraciju lokalnim uređajem, a zatim registrirali uređaj sa servisom za Upravitelj StorSimple.

    1. Unos **ključa za registraciju** koju ste dobili u **Korak 2: Registracija ključ servisa** u [Implementacija StorSimple virtualne polju – Priprema portalu](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key).

    2. Ako to ne prvi uređaj koji se registrirate za taj servis, morat ćete unijeti **ključa za šifriranje podataka servisa**. Ovaj ključ nužan je pomoću ključa Registracija servisa da biste registrirali dodatnih uređaja sa servisom StorSimple Manager. Dodatne informacije potražite u da biste [dobili ključa za šifriranje servisa podatke](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) na vašem lokalnom web-mjestu korisničkog Sučelja.

    3. Kliknite **Registracija**. To će ponovnim pokretanjem uređaja. Možda ćete morati pričekati 2-3 minuta prije uređaj uspješno registriran. Nakon ponovnog pokretanja uređaja koje odvest će stranica za prijavu.

       ![Registracija uređaja](./media/storsimple-ova-deploy3-iscsi-setup/image11.png)

10. Vratite se na portal Azure klasični. Na stranici **uređaja** , provjerite je li uređaj je uspješno povezan sa servisom traženjem status. Status uređaja mora biti **aktivna**.

    ![Stranica za uređaje](./media/storsimple-ova-deploy3-iscsi-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Korak 2: Dovršavanje postavljanja potrebna uređaja

Da biste dovršili konfiguraciju uređaja StorSimple uređaj, morate:

- Odaberite račun za pohranu za povezivanje s uređajem.

- Odaberite postavke šifriranja za podatke koji se šalju u oblaku.

Na portalu Azure klasični da biste dovršili postavljanje potrebna uređaju poduzeti sljedeće korake.

#### <a name="to-complete-the-minimum-device-setup"></a>Da biste dovršili postavljanje minimalne uređaju

1. Na stranici **uređaja** , odaberite uređaj koji ste upravo stvorili. Ovaj uređaj bi prikazivati kao **aktivna**. Kliknite strelicu pokraj naziva uređaja, a zatim kliknite **Brzi početak rada**.

    ![Stranica za uređaje](./media/storsimple-ova-deploy3-iscsi-setup/image13.png)

2. Kliknite **Postavljanje dovršeno uređaju** da biste pokrenuli čarobnjak za konfiguriranje uređaja.

    ![Konfiguriranje čarobnjak uređaja](./media/storsimple-ova-deploy3-iscsi-setup/image14.png)

3. U čarobnjaku za uređaj Konfiguriraj, na stranici **Osnovne postavke** učinite sljedeće:

   1. Odredite račun za pohranu koja će se koristiti s uređajem. U pretplatu, možete odabrati postojeći račun za pohranu s padajućeg popisa ili možete odrediti **Dodaj više** da biste odabrali račun neku drugu pretplatu.

   2. Definiranje postavke šifriranja za sve podatke na ostale koja će se slati u oblak. (StorSimple koristi AES 256 šifriranje). Da biste šifrirali podataka, odaberite potvrdni okvir **Omogući oblaka za pohranu šifriranje** . Unesite šifriranje prostora za pohranu oblaka koja sadrži 32 znaka. Ponovno je unesite ključ da biste je potvrdili.

   3. Kliknite ikonu provjeri ![Provjera ikona](./media/storsimple-ova-deploy3-iscsi-setup/image15.png).

    ![Osnovne postavke](./media/storsimple-ova-deploy3-iscsi-setup/image16.png)

    Ažurirat će se sada postavkama. Kada postavke uspješno su ažurirani, gumb postavljanje dovršeno uređaj neće biti dostupna. Vratit ćete stranicu za **Brzo pokretanje** uređaja.                                                        

>[AZURE.NOTE]Sve ostale postavke uređaja u bilo kojem trenutku možete izmijeniti tako da pristupite stranici **Konfiguracija** .

## <a name="step-3-add-a-volume"></a>Korak 3: Dodavanje jedinica

Na portalu Azure klasični da biste stvorili jedinicu poduzeti sljedeće korake.

#### <a name="to-create-a-volume"></a>Da biste stvorili jedinica

1. Na stranici **Brzi Start** uređaja kliknite **Dodaj jedinicu**. Time ćete glasnoću Čarobnjak za dodavanje.

2. U čarobnjaku za jedinicu, u odjeljku **Osnovne postavke**za dodavanje učinite sljedeće:

    1. Navedite jedinstveni naziv glasnoću. Naziv mora biti niz koji sadrži znakove 3 do 127.

    2. Unesite opis za jedinicu. Opis će olakšati prepoznavanje vlasnici glasnoću.

    3. Odaberite vrstu korištenje jedinice. Vrsta korištenja može biti **glasnoću Tiered** ili **lokalno prikvačene glasnoću.** (**Tiered glasnoću** je zadana postavka). Za radnih opterećenja koji zahtijevaju lokalne jamstva, malo latencies i noviji performanse, odaberite **lokalno prikvačene** **glasnoću**. Za sve druge podatke, odaberite **Tiered** **jedinicu**.

        Lokalno prikvačene glasnoću thickly dodjeli i osigurava primarni podataka u glasnoću ostaje na uređaju, a spill u oblak. Ako stvorite lokalno prikvačene glasnoće, uređaj će potražiti slobodnog prostora na lokalnom razine Dodjela količinu potrebnu veličinu. Stvaranje lokalno prikvačene glasnoću možda ćete morati ne prelaze postojeće podatke s uređaja u oblak, a možda dugo vrijeme potrebno za stvaranje glasnoću. Ukupno vrijeme ovisi o veličini dodijeljenu glasnoće, dostupna propusnost mreže i s podacima na uređaju.

        Tiered Glasnoća s druge strane thinly dodjeli i moguće brzo stvoriti. Prilikom stvaranja tiered glasnoću otprilike 10% prostora je dodjeli na lokalni sloju i 90% prostora dodjeli je u oblak. Ako, na primjer, ako dodjeli 1 TB glasnoće, 100 GB će se nalaziti u prostora na lokalnom i 900 GB će se koristiti u oblak kada razine podataka. To shodno podrazumijeva koji je ako vam ponestane na lokalne prostora na uređaju nije moguće Dodjela tiered zajedničko korištenje (jer 10% neće biti dostupna).

    4. Navedite dodijeljenu kapacitet glasnoću. Imajte na umu određenog kapaciteta mora biti manja od dostupan kapacitet. Ako stvarate tiered glasnoće, veličina mora biti između 500 GB i 5 Terabajta. Lokalno prikvačene opseg, navedite veličinu jedinice između 50 GB i 500 GB. Dostupan kapacitet koristite kao vodič za dodjelu resursa jedinicu. Ako je dostupan lokalne kapacitet 0 GB, a zatim koje se nije dopuštena Dodjela lokalno prikvačene ili tiered jedinice.

        ![Osnovne postavke](./media/storsimple-ova-deploy3-iscsi-setup/image17.png)

    5. Kliknite ikonu sa strelicom ![Ikona strelice](./media/storsimple-ova-deploy3-iscsi-setup/image18.png) Da biste prešli na sljedeću stranicu.

3. Na stranici **Dodatne postavke** dodali novi zapis pristup kontrole (ACR):

    1. Navedite **naziv** za svoje ACR.

    2. U odjeljku **iSCSI pokretač naziv**navedite iSCSI kvalificirani naziv (IQN) sustava Windows glavnog računala. Ako nemate na IQN, idite na [Dohvati dodatak A: IQN glavnog računala za Windows Server](#appendix-a-get-the-iqn-of-a-windows-server-host).

    3. Preporučujemo da omogućite zadane sigurnosne kopije tako da potvrdite okvir **Omogući zadane sigurnosne kopije za ovu jedinicu** . Zadane sigurnosne kopije će stvoriti pravila koja se izvršava na 22:30 svakog dana (vrijeme na uređaju) i stvara snimku oblaka jedinicu.

        ![Dodatne postavke](./media/storsimple-ova-deploy3-iscsi-setup/image19.png)

    4. Kliknite ikonu provjeri ![Provjera ikona](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Time ćete posao stvaranja glasnoću. Prikazat će se poruka tijeku otprilike ovako.

        ![poruka u tijeku](./media/storsimple-ova-deploy3-iscsi-setup/image20.png)

        Jedinice stvorit će se za navedeni postavke. Prema zadanim postavkama, nadzor i sigurnosno kopiranje će biti omogućen za jedinicu.

    5. Da biste potvrdili glasnoću uspješno je stvorena, idite na stranicu **količine** . Trebali biste vidjeti glasnoće na popisu.

        ![](./media/storsimple-ova-deploy3-iscsi-setup/image21.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Korak 4: Postavljanje, pokrenuti i oblikovati jedinica

Izvođenje sljedeće korake za postavljanje, pokrenuti i oblikovanje diskova StorSimple na glavnom računalu Windows Server.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Postavljanje, pokrenuti i oblikujte jedinica

1. Pokrenite Microsoft iSCSI pokretač.

2. U prozoru **iSCSI pokretač svojstva** na kartici **otkrivanje** kliknite **Otkrijte Portal**.

    ![Otkrijte portal](./media/storsimple-ova-deploy3-iscsi-setup/image22.png)

3. U dijaloškom okviru **Otkrivanje ciljnu Portal** navesti IP adresu sučelja iSCSI omogućeno mreže, a zatim **u redu**.

    ![IP adresa](./media/storsimple-ova-deploy3-iscsi-setup/image23.png)

4. U prozoru **iSCSI pokretač svojstva** na kartici **ciljnih web-mjesta** pronađite **Discovered ciljnih web-mjesta**. (Svakoj će biti otkriveni cilj.) Status uređaja prikazivati kao **Neaktivno**.

    ![Otkriven ciljnih web-mjesta](./media/storsimple-ova-deploy3-iscsi-setup/image24.png)

5. Odaberite ciljni uređaj, a zatim kliknite **Poveži**. Kada je uređaj povezan, status trebali biste promijeniti **povezan**. (Dodatne informacije o korištenju iSCSI pokretaču Microsoft potražite u članku [Instaliranje i konfiguriranje Microsoft iSCSI pokretač] [1].

    ![odaberite ciljni uređaj](./media/storsimple-ova-deploy3-iscsi-setup/image25.png)

6. Na Windows host pritisnite tipku s logotipom sustava Windows + X, a zatim kliknite **Pokreni**.

7. U dijaloški okvir **Pokreni** upišite **Diskmgmt.msc**. Kliknite **u redu**, a pojavit će se dijaloški okvir **Upravljanje Disk** . U desnom oknu jedinice vode na vašem računalu koje hostira.

8. U prozoru **Disk Management** postavljenu količine pojavit će se kao što je prikazano na sljedećoj slici. Desnom tipkom miša kliknite otkriven glasnoću (kliknite naziv diska), a zatim kliknite na **mreži**.

    ![Upravljanje disk](./media/storsimple-ova-deploy3-iscsi-setup/image26.png)

9. Desnom tipkom miša, a zatim odaberite **Inicijalizacija diska**.

    ![pokretanje disk 1](./media/storsimple-ova-deploy3-iscsi-setup/image27.png)

10. U dijaloškom okviru odaberite diskove Inicijalizacija, a zatim kliknite **u redu**.

    ![pokretanje diska 2](./media/storsimple-ova-deploy3-iscsi-setup/image28.png)

11. Pokreće se čarobnjak za novu jednostavnu jedinicu. Odaberite veličinu diska, a zatim kliknite **Dalje**.

    ![nove jedinice čarobnjak 1](./media/storsimple-ova-deploy3-iscsi-setup/image29.png)

12. Dodijeliti slovo jedinici, a zatim kliknite **Dalje**.

    ![nove jedinice čarobnjak 2](./media/storsimple-ova-deploy3-iscsi-setup/image30.png)

13. Unesite parametre da biste oblikovali. **Na poslužitelju Windows podržana je samo NTFS.** Postavite na Istočna Australija 64K. Unesite natpis za glasnoću. Je preporučena preporučenim načinom rada za ovaj naziv se podudara s nazivom glasnoću koje ste naveli na uređaju virtualne StorSimple. Kliknite **Dalje**.

    ![nove jedinice čarobnjak 3](./media/storsimple-ova-deploy3-iscsi-setup/image31.png)

14. Provjerite vrijednosti za jedinicu, a zatim kliknite **Završi**.

    ![nove jedinice čarobnjak 4](./media/storsimple-ova-deploy3-iscsi-setup/image32.png)

    Jedinice prikazat će se kao **Online** na stranici **Upravljanje Disk** .

    ![količine na Internetu](./media/storsimple-ova-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Daljnji koraci

Saznajte kako koristiti lokalne web korisničkog Sučelja za [administraciju sustava StorSimple virtualne polja](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>Dodatak A: dobiti IQN glavnog računala za Windows Server

Izvršite sljedeće korake da biste dobili iSCSI kvalificirani naziv (IQN) Windows glavnog računala sa sustavom Windows Server 2012.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>Da biste dobili IQN glavnog računala za Windows

1. Pokretanje iSCSI pokretaču Microsoft na Windows host.

2. U prozoru **iSCSI pokretač svojstva** na kartici **konfiguracije** odaberite i kopirajte niz iz polja **Naziv pokretač** .

    ![Svojstva pokretač iSCSI](./media/storsimple-ova-deploy3-iscsi-setup/image34.png)

2. Spremite ovaj niz.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



