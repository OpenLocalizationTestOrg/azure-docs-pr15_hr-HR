<properties 
   pageTitle="Zamjena kontroler StorSimple EBOD | Microsoft Azure"
   description="U članku se objašnjava kako ukloniti i zamijeniti jedan ili oba EBOD kontrolera na uređaju StorSimple 8600."
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

# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Zamjena programa EBOD kontroler na uređaju StorSimple

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča objašnjava kako zamijeniti neispravan EBOD kontroler modula na uređaju Microsoft Azure StorSimple. Da biste zamijenili u modulu kontroler EBOD, morate:

- Uklanjanje neispravne kontroler EBOD
- Instaliranje nove EBOD kontroler

Prije nego što počnete Imajte na umu sljedeće informacije:

- U svim Neiskorišteni slobodnih morate umetnuti prazne EBOD module. Na s prilozima će ispravno cool ako je razdoblje ostaje otvorena.

- Kontrolerom EBOD je moguće-swappable i mogu ukloniti ili zamijeniti. Nije uspjelo modula uklonite sve dok ne dobijete zamjena. Kada pokrenete zamjenski postupak, morate je dovrši u 10 minuta.

>[AZURE.IMPORTANT] Prije nego što pokušate uklonite ili zamijenite sve komponente StorSimple, provjerite je li da pregledate [konvencije ikona sigurnost](storsimple-safety.md#safety-icon-conventions) i druge [mjere opreza sigurnost](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>Uklanjanje programa EBOD kontroler

Prije zamjene neuspjelih EBOD kontroler modul StorSimple uređaja, provjerite je li druge EBOD modul kontroler aktivna i pokrenuta. Sljedeći postupak i tablice objašnjavaju kako ukloniti kontroler modul EBOD.

#### <a name="to-remove-an-ebod-module"></a>Da biste uklonili modul za EBOD

1. Otvorite portal sustava Azure klasični.

2. Dođite do **uređaje** > **održavanja** > **Status hardver**i provjerite je li status LED za aktivni modul kontroler EBOD zeleni i LED za modul nije uspjelo EBOD kontroler se crvena.

3. Pronađite nije uspjelo EBOD kontroler modul at the back of uređaj.

4. Uklanjanje kabela koji se povezuju kontroler modul EBOD s kontrolerom prije poduzimanja modul EBOD iz sustava.

5. Zabilježite točno priključak SAS modula kontroler EBOD koji je povezan s kontrolerom. Će se morati vratiti sustav tu konfiguraciju nakon što zamijenite modul EBOD. 

    >[AZURE.NOTE] Obično, to će biti A priključak, koja je označena kao **glavno računalo u** sljedeći dijagram.

    ![Backplane EBOD kontroler](./media/storsimple-ebod-controller-replacement/IC741049.png)

     **Slika 1** Stražnja strana EBOD modul

  	|Oznaka|Opis|
  	|:----|:----------|
  	|1|Kvara LED|
  	|2|Power LED|
  	|3|SAS poveznika|
  	|4|SAS LEDs|
  	|5|Serijski priključci tvorničke samo za korištenje|
  	|6|Priključka (glavno računalo u)|
  	|7|Priključak B (glavno računalo izgleda)|
  	|8|C priključak (samo tvorničke koristite)|

## <a name="install-a-new-ebod-controller"></a>Instaliranje nove EBOD kontroler

Sljedeći postupak i tablice objašnjavaju kako se instalira u modulu kontroler EBOD StorSimple uređaja.

#### <a name="to-install-an-ebod-controller"></a>Da biste instalirali na kontroler EBOD

1. Provjerite koji uređaj EBOD oštećenja, posebice za sučelja. Instalirajte novi kontroler EBOD ako su savijena sve PIN-ove.

2. S latches Otvori položaju, slajd modul u na s prilozima dok sudjelovati u latches.

    ![Instaliranje EBOD kontroler](./media/storsimple-ebod-controller-replacement/IC741050.png)

    **Slika 2**  Instaliranje modula kontroler EBOD

3. Zatvorite u latch. Kao što je na latch engages treba postavite zvučni signal.

    ![Otpustite EBOD latch](./media/storsimple-ebod-controller-replacement/IC741047.png)

    **Slika 3**  Zatvaranje latch modul EBOD

4. Ponovno povezati s kabeli. Koristite točan konfiguracija koja se nalazila prije zamjena. Pogledajte sljedeći dijagram i detalje o povezivanju s kabeli u tablici.

    ![Kabela uređaju 4U za power](./media/storsimple-ebod-controller-replacement/IC770723.png)

    **Slika 4**. Ponovno povezivanje kabela

  	|Oznaka|Opis|
  	|:----|:----------|
  	|1|Primarni s prilozima|
  	|2|UO 0|
  	|3|UO 1|
  	|4|Kontroler 0|
  	|5|Kontroler 1|
  	|6|EBOD kontroler 0|
  	|7|EBOD kontroler 1|
  	|8|S EBOD prilozima|
  	|9|Jedinice Power distribuciju.|

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [StorSimple hardvera komponente zamjenu](storsimple-hardware-component-replacement.md).
