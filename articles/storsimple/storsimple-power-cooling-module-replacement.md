<properties 
   pageTitle="Zamjena uo na uređaju StorSimple | Microsoft Azure"
   description="U članku se objašnjava uklanjanje i zamjena Power i hlađenje modul (uo) na uređaju StorSimple"
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Zamjena Power i hlađenje modul na uređaju StorSimple

## <a name="overview"></a>Pregled

Power i hlađenje modul (uo) uređaja za Microsoft Azure StorSimple sastoji se od u napajanje i hlađenje sporta koji se upravlja putem primarne i EBOD priloge. Postoji samo jedan model uo koji je potvrđena za svaku s prilozima. Primarni s prilozima je potvrđena za 764 uo W, a zatim s prilozima EBOD je potvrđena za 580 uo W. Iako se razlikuju PCMs za primarni s prilozima i s prilozima EBOD zamjenski postupak je jednak.

Pomoću ovog praktičnog vodiča u članku se objašnjava kako:

- Uklanjanje programa uo
- Instalacija zamjena uo

>[AZURE.IMPORTANT] Prije uklanjanja i zamjene na uo, pregledajte sigurnost informacije u [StorSimple hardver komponente zamjena](storsimple-hardware-component-replacement.md).

## <a name="before-you-replace-a-pcm"></a>Prije nego što zamijenite na uo

Imajte na umu sljedeće važne probleme s prije nego što zamijenite vaše uo:

- Napajanjem od na uo ne uspije, ostavite neispravan modula instaliran, ali ukloniti kabel power. Na Lepeza će i dalje power primanje u s prilozima i nastavite im omogućuju proper hlađenje. Ako na Lepeza neuspješan, u uo treba zamijeniti odmah.

- Prije uklanjanja na uo prekid power na uo tako da isključite glavnom (pri čemu je postoje) ili fizički uklanjanjem kabel power. To omogućuje upozorenje sa sustavom je li power zatvaranja Predstojeći.

- Provjerite je li u uo radi operacija nastavak sustava prije zamjene neispravan uo. Neispravni uo morate zamijeniti potpuno radu uo čim.

- Zamjena modul uo traje tek nekoliko minuta, ali morate dovršiti u roku od uklanjanje nije uspjelo uo da biste spriječili pregrijavanja 10 minuta.

- Imajte na umu da zamjena 764 W uo module otpremljene iz na tvorničke sadržavati sigurnosnu kopiju baterija modul. Morat ćete baterija uklanjanje vaša neispravan uo i umetnite ga u modul zamjenski prije izvođenja zamjena. Dodatne informacije potražite u članku kako [ukloniti, a zatim Umetni sigurnosne kopije baterija modul](storsimple-battery-replacement.md).


## <a name="remove-a-pcm"></a>Uklanjanje programa uo

Kada ste spremni za uklanjanje Power i hlađenje modul (uo) uređaja sa sustavom Microsoft Azure StorSimple, slijedite ove upute.

>[AZURE.NOTE] Prije uklanjanja vaše uo, provjerite imate li točan zamjenski (764 W za primarni s prilozima) ili 580 W za s prilozima EBOD.

#### <a name="to-remove-a-pcm"></a>Da biste uklonili s uo

1. Azure klasični portalu kliknite **uređaji** > **održavanja** > **Hardver Status**. Provjerite stanje komponenti za uo u odjeljku **Komponente za zajedničko korištenje** da biste odredili koji nije uspjela uo:

     - Ako nije uspjela napajanjem uo 0, status **Power navesti u uo 0** bit će crvena.

     - Ako nije uspjela napajanjem uo 1, status **Power navesti u 1 uo** bit će crvena.

     - Ako nije uspjela Lepeza u uo 1, status **hlađenje 0 za uo 0** ili **1 hlađenje za uo 0** će se crvena.

2. Pronađite neuspjelih uo na stražnjoj strani primarni s prilozima. Ako su pokrenuti s 8600 modelom, odredite primarni s prilozima tako da pogledate prikazane u oknu za prednju LED zaslonu sustava jedinica identifikacijskog broja. Zadani ID jedinice prikazuje se na primarni s prilozima je **00**, dok je zadani ID jedinice prikazuje na s prilozima EBOD **01**. Sljedeći dijagram i tablici objašnjavaju Prednja ploča njegovu LED zaslonu.

    ![ID sustava na prednju OPS ploča](./media/storsimple-power-cooling-module-replacement/IC740991.png)

     **Slika 1** Prednja ploča uređaja  

  	|Oznaka|Opis|
  	|:---|:-----------|
  	|1|Isključi zvuk|
  	|2|Power sustava|
  	|3|Modul kvara|
  	|4|Logička kvara|
  	|5|Prikaz ID jedinica|

3. Nadzor pokazatelj LEDs nalaze na stražnjoj strani primarni s prilozima se može koristiti i za prepoznavanje neispravan uo. Pogledajte sljedeći dijagram i tablice da biste razumjeli kako koristiti u LEDs da biste pronašli neispravan uo. Na primjer, ako je osvijetljen LED odgovara **Neće uspjeti Lepeza** , na Lepeza nije uspjelo. Isto tako, ako je osvijetljen LED odgovara **AC Neuspjelo** , napajanjem nije uspjelo. 

    ![Backplane uređaj uo nadzora pokazatelja LEDs](./media/storsimple-power-cooling-module-replacement/IC740992.png)

     **Slika 2** Stražnja strana uo pokazateljem LEDs

  	|Oznaka|Opis|
  	|:---|:-----------|
  	|1|AC struje|
  	|2|Pogreška Lepeza|
  	|3|Kvara baterija|
  	|4|U REDU UO|
  	|5|Kontroler struje|
  	|6|Dobar baterija|

4. Pogledajte na sljedećem su dijagramu stražnje StorSimple uređaj da biste pronašli uo modul nije uspjelo. Uo 0 je na lijevoj strani, a uo 1 na desnoj strani. U tablici koja slijedi objašnjeno module.

     ![Backplane moduli primarni s prilozima uređaja](./media/storsimple-power-cooling-module-replacement/IC740994.png)

     **Slika 3** Stražnja strana uređaj s moduli 

  	|Oznaka|Opis|
  	|:---|:-----------|
  	|1|UO 0|
  	|2|UO 1|
  	|3|Kontroler 0|
  	|4|Kontroler 1|

5. Isključivanje neispravan uo i prekid veze opskrbu kabel power. Sada možete ukloniti na uo.

6. Grasp na latch i strani ručicu uo između palac i forefinger, a zatim ih zajedno da biste otvorili ručice za umetanje.

    ![Otvaranje uo ručicu](./media/storsimple-power-cooling-module-replacement/IC740995.png)

    **Slika 4** Otvaranje ručicu uo

7. Ga ručicu i uklonite na uo.

    ![Uklanjanje uređaja uo](./media/storsimple-power-cooling-module-replacement/IC740996.png)

    **Slika 5** Uklanjanje na uo

## <a name="install-a-replacement-pcm"></a>Instalacija zamjena uo

Slijedite ove upute za instalaciju na uo StorSimple uređaja. Provjerite je li koji ste umetnuli modula za sigurnosne kopije baterija prije instalacije zamjenski uo (odnosi se na 764 W PCMs samo). Dodatne informacije potražite u članku kako [ukloniti, a zatim Umetni sigurnosne kopije baterija modul](storsimple-battery-replacement.md).

#### <a name="to-install-a-pcm"></a>Da biste instalirali na uo

1. Provjerite je li točan zamjenski uo za ovaj s prilozima. Primarni s prilozima mora 764 uo W, a zatim s prilozima EBOD mora 580 uo W. Ne treba pokušati koristiti 580 uo W u primarni s prilozima ili 764 uo W u s prilozima EBOD. Sljedeća slika prikazuje gdje da biste odredili podatke na oznaku koja je ponuđenima na uo.

    ![Natpis uo uređaja](./media/storsimple-power-cooling-module-replacement/IC740973.png)

    **Slika 6** Uo natpisa

2. Provjerite ima li oštećenje s prilozima, plaćanja posebnu pozornost obratite poveznika. 
                                        
    >[AZURE.NOTE] **Instalirajte modul ako su savijena sve poveznik PIN-ove.**

3. Pomoću držača uo Otvori položaju slajdova modul u na s prilozima.

    ![Instaliranje uređaja uo](./media/storsimple-power-cooling-module-replacement/IC740975.png)

    **Slika 7** Instaliranje sustava uo

4. Ručno zatvorite ručicu uo. Klik trebao čuti kao engages latch ručicu. 
                                        
    >[AZURE.NOTE] Da biste bili sigurni da imate aktivan poveznik PIN-ove, koje možete lagano tug ručice bez otpustite na latch. Ako na uo slajdovi odgovor, znači da je zatvoren u latch prije poveznika aktivan.

5. Povezivanje kabeli power izvor napajanja i na uo.

6. Sigurne na naprezanja bales obliku reljefa. 

7. Uključivanje u uo.

8. Provjerite je li bio uspješan zamjena: Azure klasični portalu StorSimple Upravitelj servisa pronađite **uređaje** > **održavanja** > **Hardver Status**. U odjeljku **Komponente za zajedničko korištenje**statusa na uo mora biti zelena. 
                                        
    >[AZURE.NOTE] Može potrajati nekoliko minuta za zamjenu uo potpuno Inicijalizacija.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [StorSimple hardvera komponente zamjenu](storsimple-hardware-component-replacement.md).
