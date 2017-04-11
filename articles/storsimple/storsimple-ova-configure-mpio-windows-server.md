<properties 
   pageTitle="Konfiguriranje MPIO na vašem računalu koje hostira StorSimple virtualne polja | Microsoft Azure"
   description="U članku se opisuje kako konfigurirati Multipath/i (MPIO) za vaše StorSimple virtualne polja povezani na glavno računalo za pokretanje sustava Windows Server 2012 R2."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>Konfiguracije/Multipath i u sustavu Windows Server glavno računalo za polja virtualne StorSimple

## <a name="overview"></a>Pregled

U ovom se članku opisuje kako instalirati značajku Multipath/i (MPIO) na vašem računalu koje hostira Windows Server, Primjena određene konfiguracijske postavke samo za StorSimple jedinicama, a zatim potvrdite MPIO za StorSimple jedinicama. Postupak pretpostavlja vaše StorSimple 1200 virtualne polja s dva sučelja mreže povezano glavno računalo za Windows Server s dva sučelja mreže. Informacije koje se nalaze u ovom članku odnosi samo na virtualne polja. Informacije o StorSimple 8000 niz uređaja, idite na [Konfiguriranje MPIO za StorSimple glavnog računala](storsimple-configure-mpio-windows-server.md). 

Značajka MPIO sustava Windows Server omogućuje sastavljanje iznimno dostupan prostor za pohranu pogreške konfiguracije. MPIO koristi komponente suvišnih fizički put – prilagodnika, kabela i parametri – da biste stvorili logičke putova između poslužitelja i uređaj za pohranu. Ako postoji komponente pogreška, uzrok logičke put uvoza, multipathing logike koristi zamjenski put za/i tako da se aplikacija i dalje možete pristupati svojim podacima. Uz to ovisno o vašoj konfiguraciji MPIO možete i poboljšati performanse ponovno preko tih putove za ujednačavanje opterećenja. Dodatne informacije potražite u članku [Pregled MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO pregled i značajke").  

Najviša-dostupnosti rješenje StorSimple, konfiguracije MPIO u sustavu Windows Server domaćini povezan s vašeg StorSimple 1200 virtualne polja (poznat i kao lokalne virtualne uređaj). Poslužitelji glavno računalo pa možete tolerate veze, mrežu ili sučelje nije uspjelo. 

Morat ćete slijedite ove korake da biste konfigurirali MPIO: 

- Preduvjeti za konfiguraciju

- Korak 1: Instalacija MPIO na glavnom računalu Windows Server

- Korak 2: Konfiguriranje MPIO za StorSimple jedinice

- Korak 3: Postavljanja StorSimple količine na glavnom računalu

Svaki od gore navedeni koraci opisan u sljedećim odjeljcima.


## <a name="prerequisites"></a>Preduvjeti

U ovom se odjeljku detalje o konfiguraciji preduvjeti za glavno računalo za Windows Server i vaše virtualne polja.

### <a name="on-windows-server-host"></a>Na računalu koje hostira Windows Server

-  Pripazite da sustava Windows Server glavno računalo ima 2 sučelje mreže omogućena.


### <a name="on-storsimple-virtual-array"></a>Na StorSimple virtualne polja

- Virtualna polja mora biti konfigurirana kao iSCSSI poslužiteljem. Dodatne informacije potražite u članku [Postavljanje virtualne polja kao iSCSI poslužitelj](storsimple-ova-deploy3-iscsi-setup.md). Sučelje mreže mora biti omogućen na polja.   

- Sučelje mreže na vašem virtualne polja mora biti dostupan iz glavnog računala za Windows Server.

- Jedan ili više količine trebalo stvoriti na vašem StorSimple virtualne polja. Dodatne informacije potražite u članku [Dodavanje jedinice](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume) na vašem StorSimple 1200 virtualne polja. U ovom postupku koju smo stvorili 3 količine (2 lokalno prikvačene i 1 tiered jedinice kao što je prikazano ispod) na virtualne polja.
    
    ![mpio0](./media/storsimple-ova-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Hardverska konfiguracija za StorSimple virtualne polja

Na slici u nastavku prikazuje hardverske konfiguracije za visoke dostupnosti i ujednačavanje opterećenja multipathing za glavno računalo za Windows Server i koristiti u ovom postupku StorSimple virtualne polja.  

![MPIO hardverska konfiguracija](./media/storsimple-ova-configure-mpio-windows-server/1200hardwareconfig.png)

Kao što je prikazano na prethodnoj slici:

- Vaše StorSimple virtualne je argument polje dodjeli na Hyper-V jedan čvor aktivni uređaj konfiguriran kao iSCSSI poslužiteljem.

- Dva sučelja virtualne mreže omogućena na vašem polja. Na lokalnom web-mjestu korisničkog Sučelja sustava 1200 virtualne polja, provjerite je li dva sučelja mreže omogućeni tako da odete **Mrežne postavke** kao što je prikazano u nastavku:

    ![Omogućena na 1200 sučelje mreže](./media/storsimple-ova-configure-mpio-windows-server/mpio9.png)
    
    Imajte na umu IPv4 adrese sučelja omogućeni mrežni (Ethernet, 2 Ethernet po zadanom) i spremite za kasnije korištenje na glavnom računalu.

- Na računalu koje hostira sustava Windows Server omogućene su dva sučelja mreže. Ako su povezani sučelja za glavno računalo i polja na istoj podmreži, zatim pojavit će se 4 putova dostupna. Ovo je predmet u ovaj postupak. Međutim, ako su svaki mrežno sučelje sučelje polja i glavnog računala na različite IP podmreže (a ne usmjeravati), zatim samo 2 putova bit će dostupni.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Korak 1: Instalacija MPIO na glavnom računalu Windows Server

MPIO dodatna je značajka sustavu Windows Server i prema zadanim postavkama nije instaliran. Trebala biti instalirana kao značajka kroz Upravitelj poslužitelja. Da biste instalirali tu značajku na glavno računalo sustava Windows Server, dovršite sljedeći postupak.

[AZURE.INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]


## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Korak 2: Konfiguriranje MPIO za StorSimple jedinice

MPIO mora biti konfigurirana tako da odredite StorSimple količine. Da biste konfigurirali MPIO prepoznati StorSimple količine, poduzeti sljedeće korake.

[AZURE.INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Korak 3: Postavljanja StorSimple količine na glavnom računalu

Nakon MPIO je konfiguriran u sustavu Windows Server, volume(s) stvoreno StorSimple polja možete postaviti, a zatim možete iskoristiti prednost MPIO za zalihosti. Postavljanje jedinice poduzeti sljedeće korake.

#### <a name="to-mount-volumes-on-the-host"></a>Postavljanje količine na glavnom računalu

1. Otvorite prozor **iSCSI pokretač svojstva** na glavnom računalu Windows Server. Kliknite **Upravitelj poslužitelja > nadzorne ploče > Alati > iSCSI pokretač**.
2. U dijaloškom okviru **iSCSI pokretač svojstva** kliknite karticu otkrivanje, a zatim **Otkrijte Portal cilj**.
3. U dijaloškom okviru **Otkrivanje ciljnu Portal** učinite sljedeće:
    
    - Upišite IP adresu prvi omogućeni mrežni sučelja na vašem StorSimple virtualne polja. Prema zadanim postavkama, to bi **Ethernet**. 
    - Kliknite **u redu** da biste se vratili u dijaloški okvir **iSCSI pokretač svojstva** .

    >[AZURE.IMPORTANT] **Ako koristite privatna mreža za iSCSI veze, unesite IP adresu priključak podataka koji je povezan s privatne mreže.**

4. Ponovite korake 2-3 za drugi mrežno sučelje (na primjer, Ethernet 2) na vašem polja. 

5. Odaberite karticu **ciljnih web-mjesta** u dijaloškom okviru **iSCSI pokretač svojstva** . Za virtualne polje, trebali biste vidjeti svaka površina glasnoću kao odredište u odjeljku **Otkrio ciljnih web-mjesta**. U ovom slučaju tri ciljnih web-mjesta (odgovara tri jedinice) želite otkriti.

    ![mpio1](./media/storsimple-ova-configure-mpio-windows-server/mpio1.png)

6. Kliknite **Poveži** da biste uspostavili sesiju iSCSI s vašeg StorSimple polja. Pojavit će se dijaloški okvir **za povezivanje s cilj** . Odaberite potvrdni okvir **Omogući više puta** . Kliknite **Napredno**.

    ![mpio2](./media/storsimple-ova-configure-mpio-windows-server/mpio2.png)

8. U dijaloškom okviru **Dodatne postavke** učinite sljedeće:                                       
    -    Na padajućem popisu **Lokalnog prilagodnika** odaberite **Microsoft iSCSI pokretač**.
    -    Na padajućem popisu **Pokretač IP** odaberite IP adresa glavnog računala.
    -    Na padajućem popisu IP **Ciljnu Portal** odaberite IP sučelja polja.
    -    Kliknite **u redu** da biste se vratili u dijaloški okvir **iSCSI pokretač svojstva** .

    ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

9. Kliknite **Svojstva**. 

    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)
10. U dijaloškom okviru **Svojstva** kliknite **Dodaj sesiju**.

    ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)

10. U dijaloškom okviru **za povezivanje s ciljnom** odaberite potvrdni okvir **Omogući više puta** . Kliknite **Napredno**.
11. U dijaloškom okviru **Napredne postavke** :                                        
    -  Na padajućem popisu **lokalnog prilagodnika** odaberite Microsoft iSCSI pokretač.
    -  Na padajućem popisu **Pokretač IP** odaberite IP adresu koja odgovara glavnog računala. U ovom slučaju povezujete dva sučelja mreže na polja sučelja jedne mreže na glavnom računalu. Zbog toga ovaj sučelje nije isti u kojemu se prvi sesiju.
    -  Na padajućem popisu **Ciljnu Portal IP** odaberite IP adresa za drugi sučelje podataka omogućena na polja.
    -  Kliknite **u redu** da biste se vratili u dijaloški okvir svojstva pokretač iSCSI. Dodali ste druge sesije na odredište.

        ![mpio11](./media/storsimple-ova-configure-mpio-windows-server/mpio11.png)

    - Kad dodate željeni sesije (putovi), u dijaloškom okviru **iSCSI pokretač svojstva** , odaberite ciljnu datoteku, a zatim kliknite **Svojstva**. Na kartici sesije u dijaloškom okviru **Svojstva** , imajte na umu četiri sesiju identifikatorima koji odgovaraju permutacija moguće put. Da biste poništili sesije, potvrdite okvir pokraj identifikatora sesija, a zatim **Prekini vezu**.
 
    - Da biste vidjeli uređaje koji se prikazuju unutar sesije, odaberite karticu **uređaji** . Da biste konfigurirali MPIO pravila za odabrani uređaj, kliknite **MPIO**. Na **
    -  Detalji o** pojavit će se dijaloški okvir. Na na **MPIO** kartica, možete odabrati odgovarajuće **pravilo uravnoteženja opterećenja** postavke. Možete pogledati u **aktivna** ili **upišite put čekanja **.

10. Ponovite korake od 8-11 da biste dodali dodatne sesije (putovi) cilj. Dva sučelja na glavnom računalu i dva na virtualne polja, možete dodati Ukupno četiri sesije za svaki cilj. 

    ![mpio14](./media/storsimple-ova-configure-mpio-windows-server/mpio14.png)

11. Morat ćete ponovite te korake za svaku jedinicu (površine kao cilj).

    ![mpio15](./media/storsimple-ova-configure-mpio-windows-server/mpio15.png)

12. Otvorite odjeljak **Upravljanje računalom** tako da odete **Upravitelj poslužitelja > nadzorne ploče > Upravljanje računalom**. U lijevom oknu kliknite **prostora za pohranu > Upravljanje Disk**. Volume(s) stvoreno polja virtualne StorSimple koje su vidljive na ovom glavno računalo prikazat će se u odjeljku **Upravljanje Disk** kao novi diskove.

13. Pokretanje disk, a zatim stvorite novu jedinicu. Tijekom postupka oblikovanje odaberite veličina jedinice za dodjelu (Istočna Australija) od 64 KB. Ponovite postupak za sve raspoložive jedinice.

    ![Upravljanje disk](./media/storsimple-ova-configure-mpio-windows-server/mpio20.png)

14. U odjeljku **Upravljanje Disk**, desnom tipkom miša kliknite **Disk** , a zatim odaberite **Svojstva**.

15. U dijaloškom okviru **Svojstva uređaja na disku više puta** pritisnite karticu **MPIO** .

    ![Svojstva diska](./media/storsimple-ova-configure-mpio-windows-server/mpio21.png)

16. U odjeljku **Naziv DSM** kliknite **Detalji** i provjerite je li da se parametri postavljene na zadani parametri. Zadani Parametri su:

    - Put provjerite razdoblja = 30
    - Ponovite Count = 3
    - Ako je PDO uklanjanje razdoblja = 20
    - Interval ponovnog pokušaja = 1
    - Put provjerite je li omogućeno = poništen.

    >[AZURE.NOTE] **Nemojte mijenjati zadane parametre.**


## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [korištenju upravitelja StorSimple servisa za administraciju sustava StorSimple virtualne polja](storsimple-ova-manager-service-administration.md).
 
