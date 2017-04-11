<properties
   pageTitle="Upravljanje StorSimple uređaj kontrolera | Microsoft Azure"
   description="Saznajte kako zaustaviti, ponovno pokrenite, isključiti ili ponovno postavi vaš kontrolera StorSimple uređaja."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="manage-your-storsimple-device-controllers"></a>Upravljanje vaše kontrolera StorSimple uređaja

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča opisuju različite operacije koje se mogu izvršiti na vaše kontrolera StorSimple uređaja. Kontrolera uređaja StorSimple su kontrolera suvišnih (ravnopravnih članova) u konfiguraciji aktivno pasivni. U određeno vrijeme, samo jedan kontroler je aktivna i obrađuje sve na disku i mrežne operacije. S kontrolerom je u načinu rada za pasivni. Aktivni kontroler ne uspije, kontrolerom pasivni postaje aktivan automatski.

Pomoću ovog praktičnog vodiča sadrži detaljne upute za upravljanje kontrolera uređaja pomoću sustava:

- **Kontrolera** dijelu stranice za **Održavanje** u servisu StorSimple Manager
- Windows PowerShell za StorSimple.

Preporučujemo da upravljate kontrolera uređaja putem StorSimple Upravitelj servisa. Ako akciju mogu izvršiti samo pomoću komponente Windows PowerShell za StorSimple, vodič čini bilješku je.

Kad pročitate ovog praktičnog vodiča, će se moći:

- Pokrenite ili isključite uređaj kontroler StorSimple
- Isključite uređaj StorSimple
- Ponovno postavljanje StorSimple uređaja na tvorničke zadane postavke


## <a name="restart-or-shut-down-a-single-controller"></a>Pokrenite ili isključite jedan kontroler

Ponovno pokretanje kontroler ili isključivanja nije potrebna u sklopu operacije normalni sustava. Isključivanje operacije za jedan uređaj kontroler su uobičajeni samo u slučajevima u kojima nije uspjelo uređaj hardverska komponenta zahtijeva zamjenski. Ponovno pokretanje računala kontroler možda biti potrebno u situaciji u kojoj je utječu na performanse viškom memorije ili neispravan kontroler. Možda morate ponovno pokretanje kontroler nakon zamjena uspješno kontroler, ako želite omogućiti i testiranje kontrolerom zamijenjenu.

Ponovno pokretanje uređaja nije disruptive da biste povezanog Pokretači, ako je dostupna kontrolerom pasivni. Ako pasivni kontroler nije dostupna ili isključeno isključeno, a zatim ponovno pokrenuti kontrolerom aktivni može uzrokovati prekidu servisa i nedostupnost.

> [AZURE.IMPORTANT]

> - **Izvodi kontroler treba nikad fizički ukloniti kao rezultat u gubitak zalihosti i povećanu rizik isključiti.**

> - Sljedeći se postupak primjenjuje samo na StorSimple fizički uređaj. Informacije o tome da biste pokrenuli, zaustavili i ponovno pokrenite virtualnog uređaja potražite u članku [Rad s virtualnog uređaja](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).

Možete ponovno pokrenuti ili isključiti kontroler jedan uređaj pomoću portala Azure klasični StorSimple Upravitelj servisa ili komponente Windows PowerShell za StorSimple

Da biste upravljali kontrolera vaš uređaj s portala za Azure klasični, poduzeti sljedeće korake.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Ponovno pokrenite ili isključiti kontroler klasični portalu

1. Dođite do **uređaji > održavanja**.

1. Idite na **Stanje hardver** i provjerite je li status od oba kontrolera na uređaju **Zdravo**.

    ![Provjerite je li StorSimple uređaj kontrolera su dobar](./media/storsimple-manage-device-controller/IC766017.png)

1. Na dnu stranice za **Održavanje** kliknite **Upravljanje kontrolera**.

    ![Upravljanje kontrolera StorSimple uređaja](./media/storsimple-manage-device-controller/IC766018.png)</br>

    >[AZURE.NOTE] Ako ne vidite **Upravljanje kontrolera**, morate instalirati ažuriranja. Dodatne informacije potražite u članku [Ažuriranje uređaju StorSimple](storsimple-update-device.md).

1. U dijaloškom okviru **Promjena postavki kontroler** učinite sljedeće:
    1. S padajućeg popisa **Odaberite kontroler** odaberite kontroler koji želite upravljati. Mogućnosti su kontroler 0 i 1, kontroler. Ove kontrolera prepoznaju se i kao aktivna ni pasivni.

        >[AZURE.NOTE] Nije moguće upravljati kontroler Ako je dostupna ili je uključena je isključeno, a prikazat će se na padajućem popisu.

    2. S padajućeg popisa **Odaberite akcije** odaberite **ponovno pokrenite kontroler** ili **isključiti kontroler**.

        ![Ponovno pokrenite StorSimple uređaju pasivni kontroler](./media/storsimple-manage-device-controller/IC766020.png)
    3. Kliknite ikonu provjeri ![Ikonu Provjera](./media/storsimple-manage-device-controller/IC740895.png).

To će ponovno pokrenite ili isključiti kontrolerom. U tablici u nastavku navedene su pojedinosti što se događa ovisno o odabira koje ste unijeli u dijaloškom okviru **Promjena postavki kontroler** .  


|Odabir #|Ako se odlučite za...|To se događa.|
|---|---|---|
|1.|Ponovno pokrenite kontrolerom pasivni.|Posao stvorit će se da biste ponovno pokrenuli kontrolerom, a bit ćete obaviješteni nakon što zadatak uspješno stvorili. To će pokrenuti kontroler ponovno pokretanje računala. Možete nadzirati postupak ponovnog pokretanja tako da pristupite **servisa > nadzorne ploče > zapisnicima operacija** i zatim filtriranje prema parametara specifične za uslugu.|
|2.|Ponovno pokrenite kontrolerom aktivna.|Vidjet ćete sljedeću poruku: "ako pokrenete kontrolerom aktivna, uređaj neće uspjeti putem s kontrolerom pasivni. Želite da biste nastavili?" </br>Ako odlučite nastaviti s ovim postupkom daljnji koraci biti jednake onima koristi da biste ponovno pokrenuli kontrolerom pasivni (pogledajte članak odabir 1).|
|3.|Isključivanje kontrolerom pasivni.|Prikazat će se sljedeća poruka: "nakon dovršetka isključivanja, morat ćete automatske gumb power na upravljaču da biste ga uključili. Jeste li sigurni da želite isključiti ovaj kontroler?" </br>Ako odlučite nastaviti s ovim postupkom daljnji koraci biti jednake onima koristi da biste ponovno pokrenuli kontrolerom pasivni (pogledajte članak odabir 1).|
|4.|Isključivanje kontrolerom aktivna.|Prikazat će se sljedeća poruka: "nakon dovršetka isključivanja, morat ćete automatske gumb power na upravljaču da biste ga uključili. Jeste li sigurni da želite isključiti ovaj kontroler?" </br>Ako odlučite nastaviti s ovim postupkom daljnji koraci biti jednake onima koristi da biste ponovno pokrenuli kontrolerom pasivni (pogledajte članak odabir 1).|


#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Ponovno pokrenite ili isključiti kontroler u komponente Windows PowerShell za StorSimple
Izvođenje možete isključiti ili ponovno pokrenite jedan kontroler na uređaju StorSimple s portala za Azure klasični pomoću sljedećih koraka.


1. Pomoću serijskog konzole ili telnet sesije s udaljenog računala pristupiti uređaj. Povezivanje s kontroler 0 ili 1 kontroler tako da slijedite korake u [PuTTY koristi za povezivanje s konzole serijski uređaj](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

1. Na izborniku serijski konzole odaberite mogućnost 1, **prijavite se pomoću puni pristup**.

1. U poruci natpis zabilježite kontrolerom ste povezani s (kontroler 0 ili 1 kontroler) i je li to aktivnog ili kontrolerom pasivni (čekanja).
    - Da biste isključili jedan kontroler, kada se zatraži, upišite:

        `Stop-HcsController`

        To će se isključiti kontroler na koji ste spojeni. Ako prestanete s kontrolerom aktivna, pa ga neće uspjeti putem s kontrolerom pasivni prije nego što se isključi.
    - Da biste ponovno pokrenuli kontroler, kada se zatraži, upišite:

        `Restart-HcsController`

        To će se pokrenuti kontroler na koji ste spojeni. Ako pokrenete kontrolerom aktivna, ga neće uspjeti putem s kontrolerom pasivni prije ponovno pokretanje.


## <a name="shut-down-a-storsimple-device"></a>Isključite uređaj StorSimple

U ovom se odjeljku objašnjava kako isključiti tekući ili nije uspjelo StorSimple uređaj s udaljenog računala. Uređaj je isključen nakon isključivanja kontrolera uređaja. Uređaj isključivanja je gotovo kada je uređaj fizički premješten ili koristi se iz servisa.

> [AZURE.IMPORTANT] Prije nego što isključite uređaj, provjerite stanje komponenti za uređaj. Dođite do **uređaji > održavanje > Status hardver** i provjerite je li status LED svih komponenti zelena. Samo dobar uređaj će imati zeleni status. Ako vaš uređaj je u tijeku Zatvori da biste Zamijeni neispravan komponentu, vidjet ćete nije uspjelo (crveni) ili su smanjene status (žuto) za odgovarajući komponente.

#### <a name="to-shut-down-a-storsimple-device"></a>Da biste isključili StorSimple uređaja

1. Pomoću postupka [pokrenite ili isključiti kontroler](#restart-or-shut-down-a-single-controller) prepoznavanje i isključiti kontrolerom pasivni na uređaju. Ovaj postupak možete izvršiti na portalu za Azure klasični ili u ljusci Windows PowerShell za StorSimple.
2. Ponovite ovaj korak da biste isključili kontrolerom active.
3. Sada ćete pregled natrag ravnini uređaja. Nakon dvije kontrolera potpuno isključivanje, trebali biste status LEDs oba kontrolera titranje crvene boje. Ako vam je potrebna da biste isključili uređaj potpuno rješenje, zrcaliti parametri power Power i hlađenje Module (PCMs) na položaj ISKLJUČENO. To biste trebali isključiti uređaj.


<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>Ponovno postavljanje uređaja na tvorničke postavke.

> [AZURE.IMPORTANT] Ako morate vratiti uređaja na tvorničke zadanih postavki, obratite se Microsoft Support. Postupak opisan u nastavku moraju se koristiti samo u kombinaciji s Microsoft Support.

Ovaj postupak opisuje kako Vrati Microsoft Azure StorSimple uređaja na tvorničke postavke pomoću komponente Windows PowerShell za StorSimple.
Ponovno postavljanje uređaja uklanja sve podatke i postavke iz čitava grupa po zadanom.

Poduzmite sljedeće korake da biste ponovno postavili Microsoft Azure StorSimple uređaja na tvorničke zadane postavke:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Da biste ponovno postavili uređaj zadane postavke u ljusci Windows PowerShell za StorSimple

1. Pristup uređaju putem njegov serijski konzolu. Provjerite natpis poruku da biste bili sigurni da su povezani s kontrolerom aktivna.

1. Na izborniku serijski konzole odaberite mogućnost 1, **prijavite se pomoću puni pristup**.

1. U naredbeni redak upišite sljedeću naredbu da biste ponovno postavili čitava grupa, uklonite sve podatke, metapodataka i kontroler postavke:

    `Reset-HcsFactoryDefault`

    Da biste umjesto toga ponovno jedan kontroler, koristite cmdlet [Vrati HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) s na `-scope` parametar.)

    Sustav će ponovno više puta. Bit ćete obaviješteni kada ponovno je uspješno dovršena. Ovisno o modelu sustava, to može potrajati 45 60 minuta za uređaje sa sustavom 8100 i 60 90 minuta do 8600 da biste dovršili postupak.

    > [AZURE.TIP]

    > - Ako koristite Update 1.2 ili neke starije verzije koristite na `–SkipFirmwareVersionCheck` parametar da biste preskočili Provjera verzije opreme (u suprotnom vidjet ćete pogreške uslijed neodgovarajuće za firmver: tvorničke Vrati se ne može nastaviti zbog ne podudaraju u verzijama firmver. ).

    > - Ponovno postavljanje postupak tvorničke možda neće funkcionirati za StorSimple uređaje koji se izvode Update 1 ili 1.1 na portalu za državne ustanove i obavite zamjena uspješno jednostruko ili dvostruko kontroler (s zamjenski kontrolera otpremljene sa softverom prije ažuriranje 1). To se događa kada ponovno na tvorničke slike nastavlja se podaci o prisutnosti SHA1 datoteke na kontroler koji ne postoji za stara Update 1 softver. Ako vidite ovu tvorničke vratiti pogrešku, obratite se Microsoftovoj Support radi jednostavnijeg sljedeće korake. Taj se problem ne vide s kontrolera zamjenski otpremljene iz tvorničke Update 1 ili noviji softvera.


## <a name="questions-and-answers-about-managing-device-controllers"></a>Pitanja i odgovori o upravljanju kontrolera uređaja

U ovom ćete odjeljku smo zbrojiti neke od najčešćih pitanja o upravljanju kontrolera StorSimple uređaj.

**PITANJA.** Što se događa ako su oba na kontrolera na uređaju dobar, a uključena na, a I ponovno pokrenite ili isključiti kontrolerom aktivni?

**ODGOVORI.** Ako su oba na kontrolera na uređaju dobar, a uključena, zatražit će se za potvrdu. Možete odabrati da:

- **Ponovno pokrenite kontrolerom aktivni** – ćete biti obaviješteni da ponovno pokrenuti aktivni kontroler uzrokovat će uređaja nije uspjela tijekom s kontrolerom pasivni. Kontrolerom će se pokrenuti.

- **Isključivanje active kontroler** – ćete biti obaviješteni da isključuje aktivni kontroler rezultirat će isključiti. Također ćete automatske gumb power na uređaju da biste uključili kontrolerom.

**PITANJA.** Što se događa ako je kontrolerom pasivni na uređaju dostupne ili isključeno isključeno i I ponovno pokrenite ili isključiti kontrolerom aktivni?

**ODGOVORI.** Ako je kontrolerom pasivni na uređaju dostupne ili isključeno isključeno, a odlučite:

- **Ponovno pokrenite kontrolerom aktivni** – ćete biti obaviješteni da nastavite postupak rezultirat će privremene prekidu servisa i zatražit će se za potvrdu.

- **Isključivanje active kontroler** – ćete biti obaviješteni da nastavite postupak rezultirat će nedostupnost pa ćete morati automatske gumb power na jedan ili oba kontrolera da biste uključili željeni uređaj. Zatražit će se za potvrdu.

**PITANJA.** Kada ne kontroler ponovno pokretanje ili isključivanje ne uspije tijek?

**ODGOVORI.** Ponovno pokretanje ili isključivanje kontroler možda neće uspjeti ako:

- Ažuriranje za uređaj je u tijeku.

- U tijeku je kontroler ponovno pokretanje računala.

- U tijeku je kontroler zatvaranja.

**PITANJA.** Kako mogu vam utvrđivanje ako kontroler pokrene ili isključiti?

**ODGOVORI.** Možete provjeriti status kontroler na stranice za održavanje. Status kontroler označava hoće li kontroler pokrene ili isključiti. Osim toga, stranice upozorenja sadrži Informativna upozorenja ako kontrolerom je ponovno pokrenuti ili isključiti. U zapisnicima postupak i bilježe kontroler operacije ponovno pokretanje i zatvaranje. Dodatne informacije o zapisnicima postupak potražite [u zapisniku operacija](storsimple-service-dashboard.md#view-the-operations-logs).

**PITANJA.** Je li sve utjecaj na I/Os uslijed kontroler prebacivanje?

**ODGOVORI.** TCP veze između Pokretači i aktivni kontroler vratit će se kao rezultat kontroler prebacivanje, ali će se ponovo uspostaviti kada se postupak pretpostavlja da kontrolerom pasivni. Tijekom te operacije može biti Pauziraj privremeno (manje od 30 sekundi) u/i aktivnosti između Pokretači i uređaja.

**PITANJA.** Kako vratiti Moje kontroler za usluge kad je isključiti i ukloniti?

**ODGOVORI.** Da biste vratili kontroler servisa, potrebno je umetnuti u nosačima kao što je opisano u [Zamjena modula kontroler na uređaju StorSimple](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Daljnji koraci

- Ako naiđete na probleme s vašeg kontrolera StorSimple uređaja koje ne možete riješiti pomoću postupaka navedenih u ovom ćete praktičnom vodiču, [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md).

- Da biste saznali više o korištenju upravitelja StorSimple servisa, idite na članak [Korištenje StorSimple Upravitelj servisa za administraciju uređaju StorSimple](storsimple-manager-service-administration.md).
