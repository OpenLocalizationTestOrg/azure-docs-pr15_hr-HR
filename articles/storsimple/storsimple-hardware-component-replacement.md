<properties 
   pageTitle="StorSimple hardver komponente zamjenski | Microsoft Azure"
   description="U članku se opisuje sigurno zamijeniti PCMs, baterija, moduli kontroler, EBOD kontrolera, diskovnih pogona, i nosačima StorSimple uređaja."
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
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="storsimple-hardware-component-replacement"></a>Zamjena za komponentu StorSimple hardvera

## <a name="overview"></a>Pregled

Vodiči za zamjenu komponente hardver opisuju hardverske komponente Microsoft Azure StorSimple 8000 niz uređaja i korake potrebne da biste uklonili i zamijeniti ih. U ovom se članku opisuje ikone sigurnost, nudi pokazivača na detaljne vodiče i navedene su zamjenjivog komponente.

>[AZURE.IMPORTANT] Prije nego što pokušate uklonite ili zamijenite sve komponente StorSimple, provjerite je li da pregledate [konvencije ikona sigurnost](#safety-icon-conventions) i druge [mjere opreza sigurnost](storsimple-safety.md).
 
### <a name="safety-icon-conventions"></a>Sigurnost ikona konvencije

U sljedećoj tablici opisane sigurnost ikone koje se koriste u ovim praktičnim vodičima. Obratite pozornost Zatvori da biste te ikone sigurnost kao proći kroz korake da biste uklonili i zamijenili komponente uređaja.

| Ikona | Tekst | Dodatne informacije |
|:---- |:---- |:-----------|
|![Ikona upozorenja](./media/storsimple-hardware-component-replacement/Warning.png)| **OPASNOST!** | Označava opasne što, ako ne izbjegava rezultirat će death ili ozbiljne ozljede. Ova riječ signal ograničeno je na najviše ekstremne situacije.|
|![Ikona upozorenja](./media/storsimple-hardware-component-replacement/Warning.png)| **UPOZORENJE!** | Označava opasne što, ako ne izbjegava može rezultirati death ili ozbiljne ozljede.|
|![Ikona upozorenje](./media/storsimple-hardware-component-replacement/Caution.png)| **OPREZ!** |Označava opasne što, ako ne izbjegava može rezultirati manji ili moderiranje ozljede.|
|![Ikona obavijesti](./media/storsimple-hardware-component-replacement/NoticeIcon.png)| **NAPOMENA:** | Pokazuje informacije koje se smatraju važne, ali ne rizik vezane uz.|
|![Ikona električne shock](./media/storsimple-hardware-component-replacement/Electric.png) | **Rizik električne Shock** | Označava visoke napona.|
|![Ikona podebljanom Debljina](./media/storsimple-hardware-component-replacement/Weight.png)| **Velike Debljina**| |
|![Ikona serviceable dijelovi bez korisnika](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png)| **Nema dijelova Serviceable korisnika** | Pristup osim ako se ispravno obučavanju članova.|
|![Ikona upute za čitanje](./media/storsimple-hardware-component-replacement/ReadInstructions.png)|**Najprije pročitajte upute za sve**| |
|![Savjet rizik ikona](./media/storsimple-hardware-component-replacement/TipHazard.png)|**Savjet rizik**| |

### <a name="before-you-begin"></a>Prije početka

Upoznajte se s sigurnost informacije o koristi u ovom ćete praktičnom vodiču uređaja i sigurnost ikone. Idite na [sigurno instalirati i raditi uređaju StorSimple](storsimple-safety.md) potpune informacije. Ne zaboravite da biste pregledali [mjere opreza sigurnost](storsimple-safety.md#handling-precautions) prije obraditi uređaju StorSimple. 

Prije nego se pokušate da biste zamijenili komponente, razmotrite sljedeće informacije.

![Ikona upozorenja](./media/storsimple-hardware-component-replacement/Warning.png) ![ikona električne Shock](./media/storsimple-hardware-component-replacement/Electric.png) **upozorenje!** 

- Uzemljite ispravno pomoću Elektrostatički rashodovanja ili antistatic Communicator prilikom rukovanja moduli i komponente StorSimple uređaja.

- Dodirnite bilo koje krugove. Korištenje navedenom ručice i vodilice tijekom obrade komponente koje možda izložen krugove.

![Ikona upozorenja](./media/storsimple-hardware-component-replacement/Warning.png) ![obratite pozornost na ikonu](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **obavijest:**

Kad zamijenite modula **NIKAD ostavite prazan servis u stražnja na s prilozima**. Nabavite zamjenski ili prazna modul prije uklanjanja dio problema.

## <a name="hardware-component-replacement-procedures"></a>Hardverski komponente zamjenski postupaka

Niz uređaju StorSimple 8000 sastoji se od nekoliko moduli u primarni i/ili EBOD priloge. Na 8100 sadrži jedan s prilozima primarni, dok je u 8600 uređaj s dva prilozima s primarni s prilozima i programa EBOD s prilozima.

Glavni hardverske komponente uređaja u su navedene u tablicama u nastavku. Kliknite vezu u stupcu **zamjenski postupak** da biste prešli na povezane Praktični vodič.

|Komponente|# Internetska prezentacija|Modul za dodatak?|Zamjena postupak
|:---------|:--------|:--------------|:---------------------|
| Nosačima|1|ne|[Zamjena nosačima na uređaju StorSimple](storsimple-chassis-replacement.md) |
|Primarni kontrolera|2|Da| [Zamjena modula kontroler na uređaju StorSimple](storsimple-controller-replacement.md) |
|764W Power i hlađenje Module (PCMs)|2|Da| [Zamjena Power i hlađenje modul na uređaju StorSimple](storsimple-power-cooling-module-replacement.md) |
|Sigurnosno kopiranje baterija|2|Da| [Zamjena modula za sigurnosne kopije baterija na uređaju StorSimple](storsimple-battery-replacement.md) |
|Diskovnih pogona|12|Da| [Zamjena pogon diska na uređaju StorSimple](storsimple-disk-drive-replacement.md) |

**Tablica 1** Hardverske komponente u primarni s prilozima

Primarni s prilozima i s prilozima EBOD razlikuju se u njihove/i moduli. Uz to, na PCMs imaju različite wattage. PCMs u primarni s prilozima su 764 W, dok su onima iz s prilozima EBOD 580 W. PCMs u primarni s prilozima sadržavati i sigurnosne kopije baterija modul.

|Komponente|# Internetska prezentacija|Modul za dodatak?| Zamjena postupak
|:---------|:--------|:--------------|:---------------------|
|Nosačima|1|ne| [Zamjena nosačima na uređaju StorSimple](storsimple-chassis-replacement.md) |
|EBOD kontrolera|2|Da| [Zamjena programa EBOD kontroler na uređaju StorSimple](storsimple-ebod-controller-replacement.md) |
|580W Power i hlađenje Module (PCMs)|2|Da| [Zamjena Power i hlađenje modul na uređaju StorSimple](storsimple-power-cooling-module-replacement.md) |
|Diskovnih pogona|12|Da| [Zamjena pogon diska na uređaju StorSimple](storsimple-disk-drive-replacement.md) |

**Tablica 2** Hardverski komponente s prilozima EBOD

U sljedećim prednje i stražnjeg dijagrama istaknute su moduli na uređaju. Ove dijagrame možete koristiti da biste odredili mjesto različite moduli zamjena je li potrebna. Prednja dijagram prikazuje diskovnih pogona i stražnjeg dijagrama s prilozima EBOD i prikaz primarnog s prilozima dodatak module.

![Frontplane uređaj s diskovnih pogona](./media/storsimple-hardware-component-replacement/IC741028.png)

**Slika 1** Ispred uređaja

|Oznaka|Opis|
|:----|:----------|
|0 - 11|Diskovnih pogona (ukupno 12)|

Primarni s prilozima i s prilozima EBOD imati moduli prijenosni disk. Na nosačima nalaze dvanaest 3.5" diskovnih pogona raspoređeni u obliku 3, 4.

![Backplane moduli primarni s prilozima uređaja](./media/storsimple-hardware-component-replacement/IC740994.png)

**Slika 2** Stražnja strana primarni s prilozima

|Oznaka|Opis|
|:----|:----------|
|1|UO 0|
|2|UO 1|
|3|Kontroler 0|
|4|Kontroler 1|

![Backplane uređaj EBOD s prilozima moduli](./media/storsimple-hardware-component-replacement/IC769599.png)

**Slika 3** Stražnja strana s prilozima EBOD

|Oznaka|Opis|
|:----|:----------|
|1|UO 0|
|2|UO 1|
|3|EBOD kontroler 0|
|4|EBOD kontroler 1|

## <a name="field-replaceable-units"></a>Polje zamjenjivog jedinice

Za uređaj StorSimple dostupne su sljedeće polje zamjenjivog jedinice (FRUs):

- Nosačima (uključujući ploču integrirani operacije)

- 764 UO AC W

- 580 UO AC W

- Tvrdi disk s modul prijenosni disk

- Modul za kontroler

- Modul kontroler EBOD

- Modul za sigurnosno kopiranje baterija

- Postavljanje rail komplet za bicikle

Imajte redoslijed bilo koji od ovih jedinica zamjenski [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) .

## <a name="next-steps"></a>Daljnji koraci

Pregledajte sve [Sigurnost informacije](storsimple-safety.md) prije nego se pokušate da biste zamijenili hardverska komponenta StorSimple.
