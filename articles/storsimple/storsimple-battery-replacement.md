<properties 
   pageTitle="Zamjena baterija na uređaju StorSimple | Microsoft Azure"
   description="U članku se opisuje kako ukloniti i zamjena teksta te održavanje modul sigurnosne kopije baterija na uređaju StorSimple."
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

# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>Zamjena modul sigurnosne kopije baterija na uređaju StorSimple

## <a name="overview"></a>Pregled

Primarni s prilozima Power i hlađenje modul (uo) na uređaju Microsoft Azure StorSimple ima paketa dodatne baterija. Ovaj paket sadrži power bi StorSimple uređaja možete spremiti podatke ako postoji gubitak struje za primarni s prilozima. Paket za baterija se nazivaju se *sigurnosne kopije baterija modul*. Modul za sigurnosno kopiranje baterija postoji samo za primarni s prilozima StorSimple uređaja (s prilozima EBOD sadržavati modula sigurnosne kopije baterija). 

Pomoću ovog praktičnog vodiča u članku se objašnjava kako:

- Uklanjanje modul sigurnosne kopije baterija 
- Instaliranje modula nove sigurnosne kopije baterija
- Održavanje modul sigurnosne kopije baterija

>[AZURE.IMPORTANT] Prije uklanjanja i zamjene modula sigurnosne kopije baterija, pregledajte informacije sigurnost u [Uvod u StorSimple hardver komponente zamjenski](storsimple-hardware-component-replacement.md).

## <a name="remove-the-backup-battery-module"></a>Uklanjanje modul sigurnosne kopije baterija

Modul za sigurnosno kopiranje baterija za svoj uređaj StorSimple je polje zamjenjivog jedinica. Prije nego što je instaliran na na uo, modul baterija će biti pohranjene u njegov izvorni pakiranju. Izvršite sljedeće korake da biste uklonili baterija sigurnosne kopije.

#### <a name="to-remove-the-backup-battery-module"></a>Da biste uklonili modul sigurnosne kopije baterija

1. Azure klasični portalu otvorite **uređaji** > **održavanja** > **Hardver Status**. U odjeljku **Komponente za zajedničko korištenje**, pogledajte status baterija.

2. Odredite uo uspjela baterija. Slika 1 prikazuje na stražnjoj strani StorSimple uređaja.

    ![Backplane moduli primarni s prilozima uređaja](./media/storsimple-battery-replacement/IC740994.png)

    **Slika 1** Ponovno od primarnog uređaja prikazuje uo i kontrolera moduli

  	|Oznaka|Opis|
  	|:----|:----------|
  	|1|UO 0|
  	|2|UO 1|
  	|3|Kontroler 0|
  	|4|Kontroler 1|

    Kao što je prikazano broj 3 u slika 2, nadzor pokazatelj LED na uo 0 koja odgovara **Baterija kvara** treba osvijetljen.

    ![Backplane uređaj uo nadzor pokazatelj LEDs](./media/storsimple-battery-replacement/IC740992.png)

    **Slika 2** Ponovno od uo prikazuje nadzora pokazatelj LEDs

  	|Oznaka|Opis|
  	|:---|:-----------|
  	|1|AC struje|
  	|2|Pogreška Lepeza|
  	|3|Kvara baterija|
  	|4|U REDU UO|
  	|5|Kontroler struje|
  	|6|Dobar baterija|

3. Da biste uklonili uo s neuspjelih baterija, slijedite korake u [Uklanjanje na uo](storsimple-power-cooling-module-replacement.md#remove-a-pcm).

4. S uo uklonjen, Podignite ručica za rotiranje na baterija modul prema gore kao što je naznačeno u sljedećoj je slici i izvukli najviše Ukloni baterija.

    ![Uklanjanjem baterija uo](./media/storsimple-battery-replacement/IC741019.png)

    **Slika 3** Uklanjanjem baterija na uo

5. Modul stavite u polje zamjenjivog jedinica pakiranje.

6. Vraćanje neispravan jedinica Microsoftu proper održavanje i obrađuju.

## <a name="install-a-new-backup-battery-module"></a>Instaliranje modula nove sigurnosne kopije baterija

Izvršite sljedeće korake da biste instalirali module baterija zamjenski uo u primarni s prilozima StorSimple uređaja.

#### <a name="to-install-the-battery-module"></a>Da biste instalirali module baterija

1. Modul za sigurnosno kopiranje baterija potvrdite odgovarajuće usmjerenje u na uo.

2. Pritisnite dolje pokazivača modula baterija da biste usidrili sjedala poveznik.

3. Zamjena uo u primarni s prilozima slijedeći upute u [Zamjena Power i hlađenje modul na uređaju StorSimple](storsimple-power-cooling-module-replacement.md).

4. Po dovršetku zamjena otvorite **uređaji** > **održavanja** > **Hardver Status** na portalu za Azure klasični. Provjerite stanje baterija da biste bili sigurni da je instalacija bila uspješna. Zelena status označava da baterija dobar.

## <a name="maintain-the-backup-battery-module"></a>Održavanje modul sigurnosne kopije baterija

Na uređaju StorSimple modula za sigurnosne kopije baterija prikazuje power s kontrolerom tijekom događaja za gubitak power. Omogućuje StorSimple uređaj da biste spremili ključnih podataka prije no što isključuje u kontrolirano. S dva potpuno naplaćena baterija u na PCMs, sustav možete rukovati dva uzastopna gubitka događaja.

Azure klasični portalu **Hardver Status** na stranice za **Održavanje** pokazuje je li baterija je neispravna ili na kraju od-stvarnih približava. Status baterija označena je **baterija na uo 0** ili **baterija na uo 1** u odjeljku **Komponente za zajedničko korištenje**. Ova stranica će prikazati stanje **DEGRADED** za kraj stvarnih Približavanje, a **nije uspjelo** za kraj stvarnih otvoriti. 

>[AZURE.NOTE] Kada je samo treba platiti baterija mogu prijaviti **nije uspjelo** .
 
Ako se pojavi **DEGRADED** stanje, preporučujemo sljedeće tečaja akcije:

- Sustav iskustvom nedavne power gubitka ili na baterija tijeku periodičku održavanja. Primijetit sustav za 12 sati prije nastavka.

    - Ako je stanje i dalje **DEGRADED** nakon 12 sati stalnu vezu na potenciju AC s kontrolera i PCMs pokrenut, zatim baterija treba zamijeniti. Imajte [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) za sigurnosne kopije baterija modula zamjena.

    - Ako stanje Pretvori u redu nakon 12 sati, baterija je radu i nužan samo naknadu za održavanje.

- Ako nije prijavljen je povezana gubitak struje i na uo uključen i povezan s struje, baterija treba zamijeniti. [Obratite se podršci Microsoft](storsimple-contact-microsoft-support.md) narudžbe modula za sigurnosne kopije baterija zamjena.

>[AZURE.IMPORTANT] Rashoda nije uspjelo baterija prema nacionalna i regional propisa. 

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [StorSimple hardvera komponente zamjenu](storsimple-hardware-component-replacement.md).
