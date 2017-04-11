<properties 
   pageTitle="Konfiguriranje MPIO za svoj uređaj StorSimple | Microsoft Azure"
   description="U članku se opisuje kako konfigurirati Multipath/i (MPIO) za svoj uređaj StorSimple povezani na glavno računalo za pokretanje sustava Windows Server 2012 R2."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-for-your-storsimple-device"></a>Konfiguriranje/Multipath i za svoj uređaj StorSimple

Microsoft Ugrađena podrška za značajku Multipath/i (MPIO) u sustavu Windows Server za pomoć Sastavi iznimno dostupan, pogreške SAN konfiguracije. MPIO koristi komponente suvišnih fizički put – prilagodnika, kabeli i parametri – da biste stvorili logičke putova između poslužitelja i uređaj za pohranu. Ako postoji komponente pogreška, uzrok logičke put uvoza, multipathing logike koristi zamjenski put za/i tako da se aplikacija i dalje možete pristupati svojim podacima. Uz to ovisno o vašoj konfiguraciji MPIO možete i poboljšati performanse ponovno preko tih putove za ujednačavanje opterećenja. Dodatne informacije potražite u članku [Pregled MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO pregled i značajke").  

Najviša-dostupnosti rješenje StorSimple, MPIO trebali biste konfigurirati na uređaju StorSimple. Nakon instalacije MPIO na poslužiteljima glavno računalo izvodi Windows Server 2012 R2 poslužiteljima na kojima možete zatim tolerate vezu, mreži ili sučelje nije uspjelo. 

MPIO dodatna je značajka sustavu Windows Server i prema zadanim postavkama nije instaliran. Trebala biti instalirana kao značajka kroz Upravitelj poslužitelja. U ovoj se temi opisani koraci koje morate slijediti za instalaciju i korištenje značajke MPIO na glavnom računalu izvodi Windows Server 2012 R2, a povezani s StorSimple fizički uređaj.

>[AZURE.NOTE] **Ovaj postupak je primjenjivo StorSimple 8000 samo za serije. MPIO trenutno nije podržano na StorSimple virtualnog uređaja.**

Morat ćete slijedite ove korake da biste konfigurirali MPIO na uređaju StorSimple:

- Korak 1: Instalacija MPIO na glavnom računalu Windows Server

- Korak 2: Konfiguriranje MPIO za StorSimple jedinice

- Korak 3: Postavljanja StorSimple količine na glavnom računalu

- Korak 4: Konfiguriranje MPIO za visoke dostupnosti i uravnoteženje opterećenja

Svaki od gore navedeni koraci opisan u sljedećim odjeljcima.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Korak 1: Instalacija MPIO na glavnom računalu Windows Server

Da biste instalirali tu značajku na glavno računalo sustava Windows Server, dovršite sljedeći postupak.

#### <a name="to-install-mpio-on-the-host"></a>Da biste instalirali MPIO na glavnom računalu

1. Otvorite upravitelj poslužitelja na glavno računalo sustava Windows Server. Prema zadanim postavkama, Upravitelj poslužitelja se pokreće kada član zapisnike grupe administratora na računalu sa sustavom Windows Server 2012 R2 ili Windows Server 2012. Ako Upravitelj poslužitelja još nije otvoren, kliknite **Start > Upravitelj poslužitelja**.
![Upravitelj poslužitelja](./media/storsimple-configure-mpio-windows-server/IC740997.png)
2. Kliknite **Upravitelj poslužitelja > nadzorna ploča > Dodaj uloga i značajki**. Pokreće se čarobnjak za **Dodavanje uloge i značajke** .
![Dodavanje značajke čarobnjak 1 i uloge](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. U čarobnjaku za **Dodavanje uloga i značajki** učinite sljedeće:

    - Na stranici **prije nego što počnete** , kliknite **Dalje**.
    - Na stranici **Odabir vrste instalacije** prihvatite zadane **uloge ili značajku** instalacije. Kliknite **Dalje**. ![Dodavanje uloge i čarobnjak za značajke 2](./media/storsimple-configure-mpio-windows-server/IC740999.png)
    - Na stranici **Odabir odredišnom poslužitelju** odaberite **Odaberite poslužitelj iz resurse poslužitelja**. Poslužitelj za glavno računalo mora biti automatski otkriti. Kliknite **Dalje**.
    - Na stranici **Odaberite uloge poslužitelja** , kliknite **Dalje**.
    - Na stranici **Odaberite značajke** **odaberite/Multipath i**pa kliknite **Dalje**. ![Dodavanje uloge i čarobnjak za značajke 5](./media/storsimple-configure-mpio-windows-server/IC741000.png)
    - Na stranici **Potvrda instalacije odabire** potvrdili odabir, a zatim odaberite **automatski ako je potrebno ponovno pokrenuti odredišnom poslužitelju**, kao što je prikazano u nastavku. Kliknite **Instaliraj**. ![Dodavanje uloge i čarobnjak za značajke 8](./media/storsimple-configure-mpio-windows-server/IC741001.png)
    - Bit ćete obaviješteni nakon dovršetka instalacije. Kliknite **Zatvori** da biste zatvorili čarobnjak. ![Dodavanje uloge i čarobnjak za značajke 9](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Korak 2: Konfiguriranje MPIO za StorSimple jedinice

MPIO mora biti konfigurirana tako da odredite StorSimple količine. Da biste konfigurirali MPIO prepoznati StorSimple količine, poduzeti sljedeće korake.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>Da biste konfigurirali MPIO za StorSimple jedinice

1. Otvorite **MPIO konfiguracije**. Kliknite **Upravitelj poslužitelja > nadzorne ploče > Alati > MPIO**.

2. U dijaloškom okviru **Svojstva MPIO** odaberite karticu **Otkrijte više putova** .

3. Odaberite **Dodaj podrška za uređaje iSCSI**, a zatim kliknite **Dodaj**.  
![Svojstva MPIO otkriti više putova](./media/storsimple-configure-mpio-windows-server/IC741003.png)

4. Ponovno pokrenete poslužiteljem kada se to od vas zatraži.
5. U dijaloškom okviru **Svojstva MPIO** kliknite karticu **Uređaji MPIO** . Kliknite **Dodaj**.
    </br>![MPIO svojstva MPIO uređaja](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. U dijaloškom okviru **Dodavanje MPIO podrške** u odjeljku **ID hardvera uređaj**, unesite svoj uređaj serijski broj. Serijski broj uređaja možete dobiti pristup servisu StorSimple upravitelj i kretanje **uređaji > nadzorne ploče**. Serijski broj uređaja prikazuje se u desnom oknu **Brzo Glance** nadzorne ploče na uređaj.
    </br>![Dodavanje MPIO podrška](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Ponovno pokrenete poslužiteljem kada se to od vas zatraži.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Korak 3: Postavljanja StorSimple količine na glavnom računalu

Nakon MPIO je konfiguriran u sustavu Windows Server, volume(s) stvorena na uređaju StorSimple možete postaviti, a zatim možete iskoristiti prednost MPIO za zalihosti. Postavljanje jedinice poduzeti sljedeće korake.

#### <a name="to-mount-volumes-on-the-host"></a>Postavljanje količine na glavnom računalu

1. Otvorite prozor **iSCSI pokretač svojstva** na glavnom računalu Windows Server. Kliknite **Upravitelj poslužitelja > nadzorne ploče > Alati > iSCSI pokretač**.
2. U dijaloškom okviru **iSCSI pokretač svojstva** kliknite karticu otkrivanje, a zatim **Otkrijte Portal cilj**.
3. U dijaloškom okviru **Otkrivanje ciljnu Portal** učinite sljedeće:
    
    - Upišite IP adresu priključka podataka StorSimple uređaja (na primjer, unesite i podataka 0).
    - Kliknite **u redu** da biste se vratili u dijaloški okvir **iSCSI pokretač svojstva** .

    >[AZURE.IMPORTANT] **Ako koristite privatna mreža za iSCSI veze, unesite IP adresu priključak podataka koji je povezan s privatne mreže.**

4. Ponovite korake 2-3 za drugi mrežno sučelje (na primjer, 1 podataka) na uređaju. Imajte na umu sljedeće sučelja mora biti omogućen za iSCSI. Dodatne informacije o tome potražite u članku [sučelje mreže Izmijeni](storsimple-modify-device-config.md#modify-network-interfaces).
5. Odaberite karticu **ciljnih web-mjesta** u dijaloškom okviru **iSCSI pokretač svojstva** . Trebali biste vidjeti uređaju cilj StorSimple IQN u odjeljku **Otkrio ciljnih web-mjesta**.
 ![iSCSI kartica ciljnih web-mjesta za pokretač svojstva](./media/storsimple-configure-mpio-windows-server/IC741007.png)
6. Kliknite **Poveži** da biste uspostavili sesiju iSCSI s uređajem StorSimple. Pojavit će se dijaloški okvir **za povezivanje s cilj** .

7. U dijaloškom okviru **za povezivanje s ciljnom** odaberite potvrdni okvir **Omogući više puta** . Kliknite **Napredno**.

8. U dijaloškom okviru **Dodatne postavke** učinite sljedeće:                                       
    -    Na padajućem popisu **Lokalnog prilagodnika** odaberite **Microsoft iSCSI pokretač**.
    -    Na padajućem popisu **Pokretač IP** odaberite IP adresa glavnog računala.
    -    Na padajućem popisu IP **Ciljnu Portal** odaberite IP sučelje uređaja.
    -    Kliknite **u redu** da biste se vratili u dijaloški okvir **iSCSI pokretač svojstva** .

9. Kliknite **Svojstva**. U dijaloškom okviru **Svojstva** kliknite **Dodaj sesiju**.
10. U dijaloškom okviru **za povezivanje s ciljnom** odaberite potvrdni okvir **Omogući više puta** . Kliknite **Napredno**.
11. U dijaloškom okviru **Napredne postavke** :                                        
    -  Na padajućem popisu **lokalnog prilagodnika** odaberite Microsoft iSCSI pokretač.
    -  Na padajućem popisu **Pokretač IP** odaberite IP adresu koja odgovara glavnog računala. U ovom slučaju dva sučelja mrežom na uređaju se povezujete s jedne mreže sučelja na glavnom računalu. Zbog toga ovaj sučelje nije isti u kojemu se prvi sesiju.
    -  Na padajućem popisu **Ciljnu Portal IP** odaberite IP adresa za drugi sučelje podataka omogućeno na uređaju.
    -  Kliknite **u redu** da biste se vratili u dijaloški okvir svojstva pokretač iSCSI. Dodali ste druge sesije na odredište.

12. Otvorite odjeljak **Upravljanje računalom** tako da odete **Upravitelj poslužitelja > nadzorne ploče > Upravljanje računalom**. U lijevom oknu kliknite **prostora za pohranu > Upravljanje Disk**. Volume(s) stvorena na uređaju StorSimple koje su vidljive na ovom glavno računalo prikazat će se u odjeljku **Upravljanje Disk** kao novi diskove.

13. Pokretanje disk, a zatim stvorite novu jedinicu. Tijekom postupka oblikovanje odaberite veličinu bloka od 64 KB.
![Upravljanje disk](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. U odjeljku **Upravljanje Disk**, desnom tipkom miša kliknite **Disk** , a zatim odaberite **Svojstva**.
15. U modelu StorSimple ### **Svojstva uređaja na disku više puta** dijaloški okvir, kliknite karticu **MPIO** .
![StorSimple 8100 DeviceProp više puta Disk.](./media/storsimple-configure-mpio-windows-server/IC741009.png)

16. U odjeljku **Naziv DSM** kliknite **Detalji** i provjerite je li da se parametri postavljene na zadani parametri. Zadani Parametri su:

    - Put provjerite razdoblja = 30
    - Ponovite Count = 3
    - Ako je PDO uklanjanje razdoblja = 20
    - Interval ponovnog pokušaja = 1
    - Put provjerite je li omogućeno = poništen.


>[AZURE.NOTE] **Nemojte mijenjati zadane parametre.**

## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Korak 4: Konfiguriranje MPIO za visoke dostupnosti i uravnoteženje opterećenja

Za više puta na temelju visoke dostupnosti i opterećenja, više sesija mora ručno dodati deklarirati različite putovi dostupni. Ako, na primjer, ako je na računalo koje hostira dva sučelja povezani SAN i uređaj ima dva sučelja povezan s SAN, a zatim vam je potrebna četiri sesije s početnim put permutacija (samo dvije sesije bit će potrebno ako svaki PODACIMA interface sučelja glavnog računala na različitim IP podmreže i ne može se usmjeravati).

>[AZURE.IMPORTANT] **Preporučujemo da izradite 1 GbE i 10 sučelje GbE mreže. Ako koristite dva sučelja mrežom, oba sučelja mora biti jednake vrste.**

Sljedeći postupak opisuje kako dodati sesije kada se StorSimple uređaj s dva sučelja mreže povezani s glavnog računala s dva sučelja mreže.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>Konfiguriranje MPIO za visoke dostupnosti i uravnoteženje opterećenja

1. Izvođenje otkrivanje cilja: u dijaloškom okviru **iSCSI pokretač svojstva** na kartici **otkrivanje** kliknite **Otkrijte Portal**.
2. U dijaloškom okviru **za povezivanje s ciljnom** unesite IP adresa jedne mreže sučeljima uređaja.
3. Kliknite **u redu** da biste se vratili u dijaloški okvir **iSCSI pokretač svojstva** .

4. U dijaloškom okviru **iSCSI pokretač svojstva** odaberite karticu **ciljnih web-mjesta** , istaknite cilj otkriveni i kliknite **Poveži**. Pojavit će se dijaloški okvir **za povezivanje s cilj** .

5. U dijaloškom okviru **za povezivanje s cilj** :
    
    - Ostavite odabrani ciljani zadana postavka za **tu vezu Dodaj** na popis favorita ciljnih web-mjesta. To će postati uređaj automatski pokušati ponovno pokrenuli vezu prilikom svakog pokretanja ovog računala.
    - Odaberite potvrdni okvir **Omogući više puta** .
    - Kliknite **Napredno**.

6. U dijaloškom okviru **Napredne postavke** :
    - Na padajućem popisu **Lokalnog prilagodnika** odaberite **Microsoft iSCSI pokretač**.
    - Na padajućem popisu **Pokretač IP** odaberite IP adresa glavnog računala.
    - Na padajućem popisu **Ciljnu Portal IP** odaberite IP adresu sučelja podataka omogućeno na uređaju.
    - Kliknite **u redu** da biste se vratili u dijaloški okvir svojstva pokretač iSCSI.

7. Kliknite **Svojstva**, a zatim u dijaloškom okviru **Svojstva** kliknite **Dodaj sesiju**.

8. U dijaloškom okviru **za povezivanje s ciljnom** potvrdite okvir **Omogući više puta** , a zatim **Dodatno**.

9. U dijaloškom okviru **Napredne postavke** :
    1. Na padajućem popisu **lokalnog prilagodnika** odaberite **Microsoft iSCSI pokretač**.
    2. Na padajućem popisu **Pokretač IP** odaberite IP adresu koja odgovara drugi sučelja na glavnom računalu.
    3. Na padajućem popisu **Ciljnu Portal IP** odaberite IP adresa za drugi sučelje podataka omogućeno na uređaju.
    4. Kliknite **u redu** da biste se vratili u dijaloški okvir **iSCSI pokretač svojstva** . Sada ste dodali druge sesije cilj.

10. Ponovite korake od 8 10 da biste dodali dodatne sesije (putovi) cilj. Dva sučelja na glavnom računalu i dva na uređaju, možete dodati Ukupno četiri sesije.

11. Kad dodate željeni sesije (putovi), u dijaloškom okviru **iSCSI pokretač svojstva** , odaberite ciljnu datoteku, a zatim kliknite **Svojstva**. Na kartici sesije u dijaloškom okviru **Svojstva** , imajte na umu četiri sesiju identifikatora koji odgovaraju permutacija moguće put. Da biste poništili sesije, potvrdite okvir pokraj identifikatora sesija, a zatim **Prekini vezu**.

12. Da biste vidjeli uređaje koji se prikazuju unutar sesije, odaberite karticu **uređaji** . Da biste konfigurirali MPIO pravila za odabrani uređaj, kliknite **MPIO**. Pojavit će se dijaloški okvir **Detalji o uređaju** . Na kartici **MPIO** možete odabrati odgovarajući **Pravilo uravnoteženja opterećenja** postavke. Možete pogledati i vrsta puta **aktivna** ni **čekanja** .

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [korištenju upravitelja StorSimple servisa da biste izmijenili konfiguraciju StorSimple uređaja](storsimple-modify-device-config.md).
 
