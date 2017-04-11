<properties 
   pageTitle="Uključivanje/isključivanje uređaju StorSimple | Microsoft Azure"
   description="U članku se objašnjava kako uključiti na novi uređaj StorSimple, uključite uređaj koji je isključiti ili prekida napajanja i isključivanje izvodi uređaja."
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
   ms.date="08/23/2016"
   ms.author="alkohli" />

# <a name="turn-your-storsimple-device-on-or-off"></a>Uključivanje/isključivanje StorSimple uređaj 

## <a name="overview"></a>Pregled

Isključite uređaj Microsoft Azure StorSimple nije potrebna u sklopu operacije normalni sustava. Međutim, možda ćete morati uključite novi uređaj ili uređaj koji je trebalo biti zatvoren. Općenito govoreći, u zatvaranja potreban je u slučajevima u kojima morate zamijenite neuspjelih hardver, fizički premještanje jedinica ili preuzeti na uređaj iz servisa. Pomoću ovog praktičnog vodiča opisuje postupak potrebne za uključivanje i isključivanje uređaju StorSimple u različitim scenarijima.

U sljedećoj su tablici navedeni različitim scenarijima za uključivanje i isključivanje uređaju StorSimple i navode veze na odgovarajućem postupku.

|Scenarij|Referentne teme|
|:-------|:---------------|
|Uključivanje na novi uređaj|[Uključivanje na novi uređaj](#turn-on-a-new-device)<ul><li>[Novi uređaj s samo primarni s prilozima](#new-device-with-primary-enclosure-only)</li><li>[Novi uređaj s EBOD s prilozima](#new-device-with-ebod-enclosure)</li></ul>|
|Uključite uređaj nakon zatvaranja|[Uključite uređaj nakon zatvaranja](#turn-on-a-device-after-shutdown)<ul><li>[Uređaj s samo primarni s prilozima](#device-with-primary-enclosure-only)</li><li>[Uređaj s EBOD s prilozima](#device-with-ebod-enclosure)</li></ul>|
|Uključite uređaj nakon gubitka napajanja|[Uključite uređaj nakon gubitka napajanja](#turn-on-a-device-after-a-power-loss)<ul><li>[Uređaj s samo primarni s prilozima](#8100)</li><li>[Uređaj s EBOD s prilozima](#8600)</li></ul>|
|Uključite uređaj nakon primarni s prilozima i EBOD prekida veze|[Uključite uređaj nakon primarnih i prekida veze s prilozima EBOD](#turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost)|
|Isključivanje izvodi uređaja|[Isključivanje izvodi uređaja](#turn-off-a-running-device)<ul><li>[Uređaj s samo primarni s prilozima](#8100a)</li><li>[Uređaj s EBOD s prilozima](#8600a)</li></ul>|

## <a name="turn-on-a-new-device"></a>Uključivanje na novi uređaj

Koraci za uključivanje StorSimple uređaj prvo razlikuju se ovisno o tome je li uređaj na 8100 ili s 8600 modelom. Na 8100 sadrži jedan s prilozima primarni, dok je u 8600 lišće-s prilozima uređaj s primarni s prilozima i programa EBOD s prilozima. U sljedećim odjeljcima možete pronaći detaljne upute za obje modela.

- [Novi uređaj s samo primarni s prilozima](#new-device-with-primary-enclosure-only)

- [Novi uređaj s EBOD s prilozima](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Novi uređaj s samo primarni s prilozima

Model StorSimple 8100 je jedan s prilozima uređaj. Vaš uređaj sadrži suvišnih Power i hlađenje Module (PCMs). Oba PCMs mora biti instaliran i povezan s različitim power izvora da biste bili sigurni visoke dostupnosti.

Izvršite sljedeće korake da biste kabelski uređaja radi power.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

>[AZURE.NOTE]Za potpuni uređaja kabeli upute otvorite da biste [instalirali StorSimple 8100 uređaj](storsimple-8100-hardware-installation.md). Provjerite je li točno slijedite upute.

### <a name="new-device-with-ebod-enclosure"></a>Novi uređaj s EBOD s prilozima

Model StorSimple 8600 sadrži primarni s prilozima i programa EBOD s prilozima. Potreban je jedinice da biste se zajedno cabled za povezivanje serijski priložene SCSI (SAS) i power.

Kada postavljate ovaj uređaj prvi put, poduzeti korake za SAS kabeli prvi put, a zatim dovršite korake za power kabeli.

[AZURE.INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

>[AZURE.NOTE]Za potpuni uređaja kabeli upute otvorite da biste [instalirali StorSimple 8600 uređaj](storsimple-8600-hardware-installation.md). Provjerite je li točno slijedite upute.

## <a name="turn-on-a-device-after-shutdown"></a>Uključite uređaj nakon zatvaranja

Koraci za uključivanje StorSimple uređaj nakon što ga je isključen razlikuju se ovisno o tome je li uređaj na 8100 ili modelu 8600. Na 8100 sadrži jedan s prilozima primarni, dok je u 8600 dvostrukog-s prilozima uređaju s primarni s prilozima i programa EBOD s prilozima.

- [Uređaj s samo primarni s prilozima](#device-with-primary-enclosure-only)

- [Uređaj s EBOD s prilozima](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Uređaj s samo primarni s prilozima

Nakon isključivanja, koristite sljedeći postupak da biste uključili StorSimple uređaj s primarni s prilozima i bez EBOD s prilozima.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Da biste uključili uređaj s u primarni s prilozima samo

1. Provjerite je li power prelazi na oba Power i hlađenje Module (PCMs) se na položaj ISKLJUČENO. Ako parametri nisu položaj ISKLJUČENO, zrcaljenje na položaj ISKLJUČENO i pričekajte da svjetla da biste se.

2. Uključite uređaj tako da zrcaljenje power prelazi na oba PCMs u položaj Uključeno. Uređaj mora uključiti.

3. Provjerite sljedeće da biste provjerili je li uređaj potpuno na:

    1. LEDs u redu u oba uo moduli se zelena.

    2. Status LEDs oba kontrolera su pune zelene boje.

    3. Titranje je plavom LED na jednom od kontrolera koji označava kontrolerom aktivan.

    Ako neki od ovih uvjeta nije zadovoljen, zatim vaš uređaj nije dobar. Imajte [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Uređaj s EBOD s prilozima

Nakon isključivanja, koristite sljedeći postupak da biste uključili StorSimple uređaj s primarni s prilozima i programa EBOD s prilozima. Točno onako kao što je opisano u nizu obavljanje svakog koraka. Pogreška da biste to učinili može rezultirati gubitka podataka.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Da biste uključili uređaj s primarne i programa EBOD s prilozima

1. Provjerite je li povezani s prilozima EBOD primarni s prilozima. Dodatne informacije potražite u članku [Instaliranje StorSimple 8600 uređaja](storsimple-8600-hardware-installation.md).

2. Provjerite je li Power hlađenje Module (PCMs) na EBOD i primarni priloge jesu i na položaj ISKLJUČENO. Ako parametri nisu položaj ISKLJUČENO, zrcaljenje na položaj ISKLJUČENO i pričekajte da svjetla da biste se.

3. Uključite s prilozima EBOD prvog prema zrcaljenje power prelazi na oba PCMs u položaj Uključeno. LEDs uo mora biti zelena. Zelena kontroler EBOD LED na jedinicu pokazuje na s prilozima EBOD.

4. Uključite primarni s prilozima prema zrcaljenje power prelazi na oba PCMs u položaj Uključeno. Cijelog sustava sada bi trebala biti na.

5. Provjerite jesu li SAS LEDs zelenoj boji, koji jamči veze između s prilozima EBOD i primarne s prilozima dobro.

## <a name="turn-on-a-device-after-a-power-loss"></a>Uključite uređaj nakon gubitka napajanja

Bez napajanja ili prekid može isključiti StorSimple uređaja. Bez napajanja se može dogoditi na jedan od zalihama power ili oba power zalihama. Koraci za oporavak razlikuju se ovisno o tome je li uređaj s modelom 8100 ili 8600. Na 8100 sadrži jedan s prilozima primarni, dok je u 8600 lišće-s prilozima uređaj s primarni s prilozima i programa EBOD s prilozima. U ovom se odjeljku opisuje postupak oporavka za svaki scenarij.

- [Uređaj s samo primarni s prilozima](#8100)

- [Uređaj s EBOD s prilozima](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Uređaj s samo primarni s prilozima<a name="8100">

U sustavu možete nastaviti njegov normalnu aktivnost ako postoji gubitkom napajanja na neki od njezinih power zalihama. Međutim, da biste bili sigurni visoke dostupnosti uređaja, vratiti power napajanjem čim.

Ako postoji bez napajanja ili prekid power na oba power zalihama, sustav će se isključiti ravnomjerno i kontrolirati način. Kada je vraćen na potenciju, sustav će automatski uključite.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Uređaj s EBOD s prilozima<a name="8600">

#### <a name="power-loss-on-one-power-supply"></a>Navedite gubitka napajanja na jedan power

U sustavu možete nastaviti njegov normalni operacija ako postoji power gubitka na neki od njezinih power zalihama na primarni s prilozima ili s prilozima EBOD. Međutim, da biste bili sigurni visoke dostupnosti uređaja, vratite power napajanjem čim.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Power gubitka na oba power zalihama na primarni i EBOD priloge

Ako postoji bez napajanja i prekid power na oba power zalihama, s prilozima EBOD će se isključiti odmah i primarni s prilozima će se isključiti ravnomjerno i kontrolirati način. Kada je vraćen power, na uređaj će se automatski pokrenuti.

Ako na potenciju je isključen ručno, provedite sljedeće korake da biste vratili power sustav.

1. Uključite s prilozima EBOD.

2. Nakon što s prilozima EBOD uključen, uključite primarni s prilozima.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Power gubitka na oba power zalihama na EBOD s prilozima

Prilikom postavljanja sustava kabeli osigurati da na EBOD nikad priključen samostalno na zasebnom PDU. Ako EBOD i primarni s prilozima neće funkcionirati u isto vrijeme, sustav će vratiti.

Ako samo s prilozima EBOD ne uspijeva u oba zalihama napajanja, sustav će ne automatski oporavak. Poduzmite sljedeće korake da biste uključili sustav i vraćanje dobar stanje:

1. Ako je uključena mogućnost primarna s prilozima, isključuje Power i hlađenje modula (PCMs).

2. Pričekajte nekoliko minuta za sustav da biste isključili.

3. Uključite s prilozima EBOD.

4. Nakon što s prilozima EBOD uključen, uključite primarni s prilozima.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Uključite uređaj nakon primarnih i prekida veze s prilozima EBOD

Ako se prekine između kontrolerom čekanja i odgovarajuće kontroler EBOD veza, uređaj i dalje raditi. Ako se prekine veza između kontrolerom aktivni sustava i odgovarajuće kontroler EBOD se trebale provoditi prebacivanje, a uređaj mora se i dalje funkcionirati uobičajen.

Kada uklanjaju se oba kabeli serijski priložene SCSI (SAS) ili je veza između s prilozima EBOD i primarni s prilozima severed, uređaj će prestati raditi. Sada možete poduzeti sljedeće korake.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Da biste uključili uređaj nakon prekida veze

1. Pristup natrag na uređaju.

2. Ako se prekine SAS kabela vezu između web-mjesto s prilozima EBOD i u okvir za primarni s prilozima, sve SAS lane LEDs na s prilozima EBOD bit će isključeno.

3. Isključivanje Power i hlađenje Module (PCMs) na web-mjesto s prilozima EBOD i u okvir za primarni s prilozima.

4. Pričekajte dok se ne isključite svjetla na stražnjoj strani oba priloge.

5. Vodeni kabeli SAS pa provjerite postoji li dobru vezu između web-mjesto s prilozima EBOD i u okvir za primarni s prilozima.

6. Uključite s prilozima EBOD prvog prema zrcaljenje oba uo parametri u položaj Uključeno.

7. Provjerite je li s prilozima EBOD na tako da se zelena LED Uključeno.

8. Uključite primarni s prilozima.

9. Provjerite je li primarni s prilozima na tako da LED kontroler zeleni je Uključeno.

10. Provjerite je li veze s prilozima EBOD sa primarni s prilozima dobar tako da potvrdite da su lane SAS LEDs (četiri po EBOD kontroler) sve Uključeno.

>[AZURE.IMPORTANT] Ako su kabeli SAS neispravan ili veze između s prilozima EBOD i primarne s prilozima nije dobro je kada se uključite u sustavu, otvorite će u načinu rada za oporavak. Ako se to dogodi, imajte [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) .

## <a name="turn-off-a-running-device"></a>Isključivanje izvodi uređaja

Tekući uređaj StorSimple možda morati isključiti ako je premješten, snimanja iz usluge, ili je neispravan komponente koje treba zamijeniti. Koraci razlikuju se ovisno o tome je li uređaj StorSimple programa 8100 ili modelu 8600. Na 8100 sadrži jedan s prilozima primarni, dok je u 8600 lišće-s prilozima uređaj s primarni s prilozima i programa EBOD s prilozima. U ovom se odjeljku Detalji korake da biste isključili izvodi uređaja.

- [Uređaj s primarni s prilozima](#8100a)

- [Uređaj s EBOD s prilozima](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Uređaj s primarni s prilozima<a name="8100a"> 

Da biste isključili uređaj ravnomjerno i kontrolirati način, možete stvoriti putem portala za Azure klasični ili putem komponente Windows PowerShell za StorSimple. 

>[AZURE.IMPORTANT] Isključuje izvodi uređaj pomoću gumba power na stražnjoj strani uređaj.
>
>Prije isključivanja uređaj, provjerite jesu li sve komponente za uređaj dobar. Azure klasični portalu pronađite **uređaje** > **održavanja** > **Status hardver**i provjerite je li status svih komponenti zelena. To vrijedi samo za dobar sustav. Ako je sustav je u tijeku Zatvori da biste zamijenite neispravan komponentu, prikazat će se nije uspjelo (crveni) ili smanjena (žuto) status komponentu odgovarajući **Hardver Status**.

Nakon što pristupite komponente Windows PowerShell za StorSimple ili Azure klasični portal, slijedite korake u [isključiti StorSimple uređaja](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Uređaj s EBOD s prilozima<a name="8600a">

>[AZURE.IMPORTANT] Prije isključivanja primarni s prilozima i s prilozima EBOD, sve komponente za uređaj provjerite jesu li dobar. Azure klasični portalu pronađite **uređaje** > **održavanja** > **Status hardver**i provjerite jesu li sve komponente dobar.

#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Da biste isključili izvodi uređaj s EBOD s prilozima

1. Pratite sve korake navedene u [isključiti StorSimple uređaj](storsimple-manage-device-controller.md#shut-down-a-storsimple-device) za primarni s prilozima.

2. Nakon isključivanja primarni s prilozima, isključite na EBOD po zrcaljenje isključivanje skretnica snage i hlađenje modul (uo).

3. Da biste potvrdili da je zatvorite u EBOD, provjerite jesu li sve lampice na stražnjoj strani s prilozima EBOD isključeno.

>[AZURE.NOTE] SAS kabela koji se koriste za povezivanje s prilozima EBOD primarni s prilozima mora se ukloniti do nakon isključivanja sustava.

## <a name="next-steps"></a>Daljnji koraci

[Obratite se podršci Microsoft](storsimple-contact-microsoft-support.md) Ako naiđete na poteškoće prilikom uključivanja ili isključivanja StorSimple uređaja.

