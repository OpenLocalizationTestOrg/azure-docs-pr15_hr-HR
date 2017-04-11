<properties 
   pageTitle="Zamjena kontroler StorSimple uređaja | Microsoft Azure"
   description="U članku se objašnjava kako ukloniti i zamijeniti jedan ili oba kontroler modula na uređaju StorSimple."
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
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Zamjena modula kontroler na uređaju StorSimple

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča objašnjava uklanjanje i zamjena jedan ili oba kontroler module u StorSimple uređaja. Opisano osnovnu logiku za zamjenu scenarije jednostrukih i dva kontroler.

>[AZURE.NOTE] Prije izvođenja zamjena kontroler, preporučujemo da uvijek ažuriranje opreme kontroler na najnoviju verziju.
>
>Da biste spriječili oštećenje uređaju StorSimple izbaciti kontrolerom dok se ne prikazuju li se na LEDs kao nešto od sljedećeg:
>
>- Sve svjetla su ISKLJUČENO.
>- LED 3 ![Zelena kvačica ikona](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), i ![unakrsno crvena ikona](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) su Bljeskave i LED 0 i LED 7 **Uključeno**.

Sljedeća tablica prikazuje zamjenski scenariji podržani kontroler.

|Velikim slovom|Zamjena scenarija|Primjenjivo postupak|
|:---|:-------------------|:-------------------|
|1|Jedan kontroler je u stanju nije uspio, s kontrolerom dobar i aktivno.|[Zamjena jednog kontroler](#replace-a-single-controller), koji opisuje [logike iza zamjena jedan kontroler](#single-controller-replacement-logic), kao i [Zamjena korake](#single-controller-replacement-steps).|
|2|Oba kontrolera nije uspjela i potrebni su zamjena. Na nosačima, diskova, s and.disk prilozima su dobar.|[Zamjena dva kontroler](#replace-both-controllers), koji opisuje [logike iza zamjena dva kontroler](#dual-controller-replacement-logic), kao i [Zamjena korake](#dual-controller-replacement-steps). |
|3|Zamjenjuju su kontrolera s istom uređaju ili s različitim uređajima. Nosačima, diskova i disk priloge su dobar.|Pojavit će se poruka upozorenja vremensko razdoblje ne podudaraju.|
|4|Nedostaje jedan kontroler, a ne uspije s kontrolerom.|[Zamjena slika zaslona dvostrukih kontroler](#replace-both-controllers), koji opisuje [logike iza zamjena dva kontroler](#dual-controller-replacement-logic), kao i [Zamjena korake](#dual-controller-replacement-steps).|
|5|Jedan ili oba kontrolera nije uspjela. Uređaj ne možete pristupiti putem komponente Windows PowerShell remoting ili serijski konzoli.|[Obratite se podršci Microsoft](storsimple-contact-microsoft-support.md) ručno kontroler zamjenski postupka.|
|6|Na kontrolera imaju različite Sastavi verzija, koja može biti krajnji rok za:<ul><li>Kontrolera imati verziju različitih programa.</li><li>Kontrolera imaju različite firmver verziju.</li></ul>|Ako verzija softvera kontroler razlikuju, logika zamjenski otkriva koji i ažuriranja verzija softvera na kontroleru zamjena.<br><br>Ako verzija firmver kontroler razlikuju se i starije verzije firmver **neće** automatski upgradeable poruku upozorenja pojavit će se na portalu za Azure klasični. Trebali biste Potraži ažuriranja i instalirajte ažuriranja opreme.</br></br>Ako firmver verzije kontroler razlikuju se i starije verzije firmver je automatski upgradeable, zamjenski logike kontroler će Odredi ovo, a nakon pokretanja kontrolerom firmver ažurirat će se automatski.|

Morate ukloniti modula kontroler Ako je nije uspio. Jedan ili oba kontroler module uspijeva, koji može uzrokovati jedan kontroler zamjena ili dvostruko kontroler zamjena. Zamjena postupke i logike iza ih, pogledajte sljedeće:

- [Zamjena jednog kontroler](#replace-a-single-controller)
- [Zamjena oba kontrolera](#replace-both-controllers)
- [Uklanjanje kontroler](#remove-a-controller)
- [Umetanje kontroler](#insert-a-controller)
- [Prepoznavanje kontrolerom aktivni na uređaju](#identify-the-active-controller-on-your-device)

>[AZURE.IMPORTANT] Prije uklanjanja i zamjene kontroler, pregledajte sigurnost informacije u [StorSimple hardver komponente zamjena](storsimple-hardware-component-replacement.md).

## <a name="replace-a-single-controller"></a>Zamjena jednog kontroler

Kada jedan od dva kontrolera na uređaju Microsoft Azure StorSimple nije uspjela, je neispravna ili nedostaje, morate zamijeniti jedan kontroler. 

### <a name="single-controller-replacement-logic"></a>Jedan kontroler zamjenski logike

U jedan kontroler zamjenu, najprije uklonite kontrolerom nije uspjelo. (Kontrolerom preostale uređaj nije aktivni kontroler.) Kada umetnete kontrolerom zamjenski pojavljuju sljedeće radnje:

1. Kontrolerom zamjenski odmah se pokreće komunikaciju s uređaja StorSimple.

2. Snimka virtualne tvrdog diska (VHD) za aktivni kontroler kopira se kontrolerom zamjena.

3. Snimka je izmijenjena tako da se prilikom pokretanja kontrolerom zamjena iz ovog VHD prepoznavanja kao čekanja kontroler.

4. Nakon dovršetka promjene kontrolerom zamjenski će se pokrenuti kao kontrolerom čekanja.

5. Kad se izvode oba kontrolera, klaster dolazi online.

### <a name="single-controller-replacement-steps"></a>Jedan kontroler zamjenski koracima

Ako jedan od kontrolera uređaja za Microsoft Azure StorSimple ne uspije, slijedite sljedeće korake. (S kontrolerom mora biti aktivna i pokrenuta. Ako oba kontrolera uspjeti ili nepravilno funkcionirati, prijeđite [dva kontroler zamjenski korake](#dual-controller-replacement-steps).)

>[AZURE.NOTE] To može potrajati 30 – 45 minuta za kontroler da biste ponovno i potpuno oporavak zamjenski postupak jedan kontroler. Ukupno vrijeme trajanja cijeli postupak, uključujući prilaganja kabeli, je više od dva sata.

#### <a name="to-remove-a-single-failed-controller-module"></a>Da biste uklonili jednu nije uspjela kontroler modula

1. Azure klasični portalu idite na servis StorSimple upravitelj, kliknite karticu **uređaji** , a zatim kliknite naziv uređaja na kojem želite nadzirati.

2. Idite na **Održavanje > Status hardver**. Stanje kontroler 0 ili 1 kontroler mora biti crvenu, što znači da se pogreška.

    >[AZURE.NOTE] Nije uspjelo kontroler u jednom kontroler zamjenski uvijek je čekanja kontroler.

3. Da biste pronašli modul nije uspjelo kontroler pomoću slika 1 i u sljedećoj tablici.  

    ![Backplane moduli primarni s prilozima uređaja](./media/storsimple-controller-replacement/IC740994.png)

    **Slika 1** Stražnja strana StorSimple uređaja

  	|Oznaka|Opis|
  	|:----|:----------|
  	|1|UO 0|
  	|2|UO 1|
  	|3|Kontroler 0|
  	|4|Kontroler 1|

4. Na kontrolerom nije uspjelo uklanjanje svih povezanih mrežni kabeli priključke podataka. Ako koristite 8600 modela, ukloniti i SAS kabela koji se povezuju s kontrolerom s kontrolerom EBOD.

5. Slijedite korake u [Uklanjanje kontroler](#remove-a-controller) da biste uklonili kontrolerom nije uspjelo. 

6. Instalirajte zamjenski tvorničke u istom vremensko razdoblje iz kojeg je uklonjena kontrolerom nije uspjelo. Time se pokreće zamjenski logike jedan kontroler. Dodatne informacije potražite u članku [jedan kontroler zamjenski logike](#single-controller-replacement-logic).

7. Dok zamjenski logike jedan kontroler nastavlja u pozadini, ponovno povezati s kabeli. Pobrinuti se da biste se povezali točno na isti način kao da ste povezani prije zamjena svi kabeli.

8. Nakon ponovnog pokretanja kontrolerom potvrdite **kontroler stanja** i u **skupine status** na portalu za Azure klasični provjerite je li kontrolerom u dobar stanje i je u stanju čekanja.

>[AZURE.NOTE] Ako su nadzor uređaja putem konzole za serijski, možda ćete vidjeti više ponovnog pokretanja dok je kontrolerom oporavak u postupku zamjena. Kada raspoređene su na izborniku serijski konzole, zatim znate zamjena dovršetka. Ako se ne prikazuje na izborniku unutar dva radno vrijeme pokretanje zamjenski kontroler, imajte [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md).

## <a name="replace-both-controllers"></a>Zamjena oba kontrolera

Kada obje kontrolera na uređaju Microsoft Azure StorSimple nije uspjela, su neispravna ili nedostaju, morate zamijeniti oba kontrolera. 

### <a name="dual-controller-replacement-logic"></a>Slika zaslona dvostrukih kontroler zamjenski logike

U dva kontroler zamjenu, uklonite i nije uspjelo kontrolera, a zatim umetnite zamjena. Dva zamjenski kontrolera su umetanju pojavljuju se sljedeće radnje:

1. Zamjena kontroler u vremensko razdoblje 0 provjerava sljedeće:
 
   1. Koristi se trenutne verzije opreme i softvera?

   2. Je li dio klaster?

   3. Je kontrolerom ravnopravnih članova pokrenut i je li grupirani?
                            
    Ako nijedna od ovih uvjeta nije true, kontrolerom traži najnovije dnevna sigurnosna kopija (koja se nalazi u **nonDOMstorage** na pogon S). Kontrolerom kopira najnovije snimke u VHD iz sigurnosne kopije.

2. Kontroler u vremensko razdoblje 0 pomoću snimku slika sam.

3. U međuvremenu, kontrolerom u vremensko razdoblje 1 čeka kontroler 0 da biste dovršili za obradu slika i pokrenuti.

4. Nakon pokretanja kontroler 0 kontroler 1 otkriva klaster stvorio kontroler 0, koji se pokreće zamjenski logike jedan kontroler. Dodatne informacije potražite u članku [jedan kontroler zamjenski logike](#single-controller-replacement-logic).

5. Nakon toga će se izvoditi oba kontrolera i klaster prosljeđivala online.

>[AZURE.IMPORTANT] Sljedeći zamjenski dva kontroler, nakon StorSimple uređaj konfiguriran, je ključna da se snimljene ručno sigurnosno kopiranje uređaja. Dnevni sigurnosne kopije konfiguracije uređaj se aktiviraju do nakon što ste proteklih od 24 sata. Rad s [Microsoftovoj službi za podršku](storsimple-contact-microsoft-support.md) da biste ručno sigurnosno kopiranje uređaja.

### <a name="dual-controller-replacement-steps"></a>Slika zaslona dvostrukih kontroler zamjenski korake

Ovaj tijek rada potreban je kada obje kontrolera uređaja za Microsoft Azure StorSimple nije uspjela. To se može dogoditi u podatkovnog centra u kojem nikakvo sustava prestane funkcionirati i zbog toga neće uspjeti oba kontrolera unutar kratkog vremenskog razdoblja. Ovisno o je li uređaj StorSimple uključili ili isključili i tome koristite programa 8600 ili modelu 8100, potreban je drugi skup koraka.

>[AZURE.IMPORTANT] To može potrajati 45 minuta na 1 sat za kontroler da biste ponovno i potpuno oporaviti iz dva kontroler zamjenski postupak. Ukupno vrijeme trajanja cijeli postupak, uključujući prilaganja kabeli, je otprilike 2.5 sati.

#### <a name="to-replace-both-controller-modules"></a>Da biste zamijenili oba kontroler moduli

1. Ako uređaj isključena, preskočite ovaj korak, a zatim prijeđite na sljedeći korak. Ako je uključena mogućnost uređaj, isključite uređaj.
                                        
    1. Ako koristite modelu 8600, isključite primarni s prilozima prvi, a zatim isključite s prilozima EBOD.

    2. Pričekajte dok se uređaj je isključiti u potpunosti. Isključivanje bit će LEDs nalaze na stražnjoj strani uređaj.

2. Uklonite sve mrežni kabeli koji su povezani s priključke podataka. Ako koristite 8600 modela, ukloniti i SAS kabela koji se primarni s prilozima povezati s prilozima EBOD.

3. Uklanjanje oba kontrolera StorSimple uređaja. Dodatne informacije potražite u članku [Uklanjanje kontroler](#remove-a-controller).

4. Najprije umetnite tvorničke zamjena za kontroler 0, a zatim umetnite kontroler 1. Dodatne informacije potražite u članku [Umetanje kontroler](#insert-a-controller). Time se pokreće zamjenski logike dva kontroler. Dodatne informacije potražite u članku [dva kontroler zamjenski logike](#dual-controller-replacement-logic).

5. Dok zamjenski logike kontroler nastavlja u pozadini, ponovno povezati s kabeli. Pobrinuti se da biste se povezali točno na isti način kao da ste povezani prije zamjena svi kabeli. Detaljne upute za model kabel vaš uređaj sekcije [Instaliranje StorSimple 8100 uređaja](storsimple-8100-hardware-installation.md) ili ponovno [instalirati uređaju StorSimple 8600](storsimple-8600-hardware-installation.md)potražite u članku.

6. Uključite StorSimple uređaja. Ako koristite modelu 8600:

    1. Provjerite je li s prilozima EBOD je uključeno prvi.

    2. Pričekajte da se izvodi s prilozima EBOD.

    3. Uključite primarni s prilozima.

    4. Kada prvog kontroler pokreće i dobar stanja, sustav će biti pokrenut.

    >[AZURE.NOTE] Ako su nadzor uređaja putem konzole za serijski, možda ćete vidjeti više ponovnog pokretanja dok je kontrolerom oporavak u postupku zamjena. Kad se otvori izbornik serijskih konzole, zatim znate zamjena dovršetka. Ako se na izborniku ne prikazuje unutar 2.5 radno vrijeme pokretanje zamjenski kontroler, imajte [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md).

## <a name="remove-a-controller"></a>Uklanjanje kontroler

Da biste uklonili neispravne kontroler modul s uređaja StorSimple, koristite sljedeći postupak.

>[AZURE.NOTE] Sljedeća ilustracija su za kontroler 0. Za kontroler 1, to bi moguće poništiti.

#### <a name="to-remove-a-controller-module"></a>Da biste uklonili modula kontroler

1. Grasp latch modul između palac i forefinger.

2. Lagano Umetanje palac i forefinger zajedno da biste predali latch kontroler.

    ![Otpustite kontroler latch](./media/storsimple-controller-replacement/IC741047.png)

    **Slika 2** Otpustite kontroler latch

2. Upotrijebili na latch bolji slajda kontrolerom iz na nosačima.

    ![Klizača kontroler iz nosačima](./media/storsimple-controller-replacement/IC741048.png)

    **Slika 3** Animiranog kontrolerom iz na nosačima

## <a name="insert-a-controller"></a>Umetanje kontroler

Pomoću sljedećeg postupka za instaliranje modula tvorničke navedena kontroleru nakon što uklonite neispravan modul s StorSimple uređaja.

#### <a name="to-install-a-controller-module"></a>Da biste instalirali modula kontroler

1. Provjerite postoji li oštećenje poveznika sučelja. Ako bilo koji od poveznik PIN-ove oštećen ili savijena, instalirajte modul.

2. Klizač modula kontroler u na nosačima dok se latch potpuno objavio. 

    ![Klizača kontroler u nosačima](./media/storsimple-controller-replacement/IC741053.png)

    **Slika 4** Klizača kontroler u na nosačima

3. S modul kontroler umetnuti započnite zatvaranja na latch tijekom nastavka da biste modul kontroler u na nosačima. Na latch će sudjelovati za vođenje kontrolerom u mjesto.

    ![Zatvaranje latch kontroler](./media/storsimple-controller-replacement/IC741054.png)

    **Slika 5** Zatvaranje latch kontroler

4. Završite kada na latch poravnava u mjesto. Na sada bi trebala biti LED **u redu** .  

    >[AZURE.NOTE] To može potrajati i do pet minuta za kontrolerom i LED da biste aktivirali.

5. Zamjena provjerite je li uspješan, na portalu za Azure klasični, idite na **uređajima** > **održavanja** > **Status hardver**i provjerite jesu li kontroler 0 i 1 kontroler dobar (status je zeleni).

## <a name="identify-the-active-controller-on-your-device"></a>Prepoznavanje kontrolerom aktivni na uređaju

U mnogim situacijama, kao što je uređaj prvo Registracija ili kontroler zamjenski, zbog kojih je da biste pronašli aktivni kontroler na StorSimple uređaju. Aktivni kontroler obrađuje sve disk firmver i rad s mrežom operacije. Da biste odredili kontrolerom active možete koristiti bilo koji od sljedećih načina:

- [Korištenje Azure klasični portal za prepoznavanje kontrolerom aktivni](#use-the-azure-classic-portal-to-identify-the-active-controller)

- [Korištenje komponente Windows PowerShell za StorSimple za prepoznavanje kontrolerom aktivni](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)

- [Provjera fizički uređaj da biste odredili kontrolerom aktivni](#check-the-physical-device-to-identify-the-active-controller)

Sljedeći je opisan svaki od tih postupaka.

### <a name="use-the-azure-classic-portal-to-identify-the-active-controller"></a>Korištenje Azure klasični portal za prepoznavanje kontrolerom aktivni

Azure klasični portalu pronađite **uređaje** > **održavanja**i pomaknite se do odjeljka **kontrolera** . Ovdje možete provjeriti koji kontroler je aktivna.

![Prepoznavanje aktivni kontroler Azure klasični portalu](./media/storsimple-controller-replacement/IC752072.png)

**Slika 6** Azure klasični portal prikazuje aktivni kontroler

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Korištenje komponente Windows PowerShell za StorSimple za prepoznavanje kontrolerom aktivni

Kada uređaj pristupiti putem konzole za serijski, raspoređene su poruke natpis. Natpis poruka sadrži informacije o osnovni uređaju kao što su model, naziv, verzija instaliranog softvera i status kontrolera pristupate. Na sljedećoj je slici prikazan primjer natpis poruke:

![Serijski natpis poruke](./media/storsimple-controller-replacement/IC741098.png)

**Slika 7** Natpis poruke s prikazom kontroler 0 kao aktivnu

Natpis poruka možete koristiti da biste utvrdili je li kontrolerom ste povezani s aktivnim ili pasivni.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Provjera fizički uređaj da biste odredili kontrolerom aktivni

Da biste odredili kontrolerom aktivni na uređaju, pronađite plavom LED iznad priključak podataka 5 na stražnjoj strani primarni s prilozima.

Ako je ovo LED titranje, kontrolerom je aktivna i s kontrolerom je u stanju čekanja. Pomoću sljedećih dijagrame i tablice kao programa Word.

![Uređaj primarni s prilozima backplane s dataports](./media/storsimple-controller-replacement/IC741055.png)

**Slika 8** Stražnja strana primarni s prilozima s podacima priključci i nadzor LEDs

|Oznaka|Opis|
|:----|:----------|
|1-6|PODATAKA 0 – 5 mrežni priključci|
|7|Plavi LED|


## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [StorSimple hardvera komponente zamjenu](storsimple-hardware-component-replacement.md).
