<properties
   pageTitle="Instalacija uređaju StorSimple 8100 | Microsoft Azure"
   description="Opisuje kako otpakiravanje bicikle postavljanja i kabelski uređaju StorSimple 8100 prije implementacije i konfiguriranje softvera."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Otpakiravanje, za bicikle postavljanja i kabelski StorSimple 8100 uređaja

## <a name="overview"></a>Pregled

Vaš 8100 Microsoft Azure StorSimple je jedan s prilozima, uređaj postavljen za bicikle. Pomoću ovog praktičnog vodiča objašnjava kako otpakiravanje, za bicikle postavljanja i kabel StorSimple 8100 hardver prije konfigurirati i implementacija StorSimple uređaja.

## <a name="unpack-your-storsimple-8100-device"></a>Otpakiravanje StorSimple 8100 uređaja

Sljedeći koraci pružaju Očisti, detaljne upute o tome kako otpakiravanje StorSimple 8100 uređaja za pohranu. Ovaj uređaj je isporučena u jedan okvir.

### <a name="prepare-to-unpack-your-device"></a>Priprema za otpakiravanje uređaju

Prije otpakiravanje uređaju, pročitajte članak sljedeće informacije.

![Ikona upozorenja](./media/storsimple-safety/IC740879.png)![podebljanom Debljina ikona](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **upozorenje!**

1. Provjerite jeste li povezani s dvije osobe dostupan za upravljanje debljine na s prilozima ako su je zadužen ručno. Potpuno konfiguriran s prilozima možete obzir uzeli i do 32 kg (70 funte.).
1. U okviru postavite na površini za nehijerarhijsku, razine.

Nakon toga slijedite sljedeće korake da biste otpakiravanje uređaju.

#### <a name="to-unpack-your-device"></a>Da biste otpakiravanje uređaju

1. Provjeri okvira i foam pakiranja za crushes, rezovima, vode oštećenja ili drugim očite oštećenja. Ako je okvir ili pakiranja ozbiljno oštećen, otvorite okvir. Imajte [obratite se Microsoftovoj službi za podršku](storsimple-contact-microsoft-support.md) da biste pomoći pri procjeni li uređaj dobro radi li ispravno.

2. Otpakiravanje okvir. Sljedeća slika prikazuje raspakirane prikaz StorSimple uređaja.

     ![Otpakiravanje uređaj za pohranu](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)

    **Raspakirane prikaz uređaj za pohranu**

     Oznaka | Opis
     ----- | -------------
     1     | Okvir za spremanje
     2     | Dno foam
     3     | Uređaj
     4     | Gornji foam
     5     | Jezični okvir


3. Nakon Raspakiravanje okvir, provjerite jeste li povezani s:

   - 1 uređaj s jednom prilozima
   - 2 kablovima power
   - 1 ga Ethernet kabela
   - 2 kabeli serijskih konzole
   - 1 serijski USB pretvornik za serijski pristup
   - 1 mijenjati dokaz T10 odvijača
   - 4 QSFP-na-SFP + prilagodnika za korištenje s 10 sučelje GbE mreže
   - 1 za bicikle-postavljanja kit (2 strani tračnicama s postavljanje hardvera)
   - Početak rada dokumentacija

    Ako niste ne primite nikakve stavki naveden, [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md).

Sljedeći je korak za bicikle postavljanja uređaja.

## <a name="rack-mount-your-storsimple-8100-device"></a>Za bicikle postavljanja StorSimple 8100 uređaja

Poduzmite sljedeće korake da biste instalirali uređaj za pohranu StorSimple 8100 u standardni bicikle 19-inčni s prednje i stražnjeg objave. Uređaj StorSimple 8100 ima jednu primarnu s prilozima.

Instalacija se sastoji od više koraka, od kojih svaka se spominju u sljedećih postupaka.

> [AZURE.IMPORTANT]
Uređaji StorSimple mora biti postavljena za bicikle za pravilnog.

### <a name="prepare-the-site"></a>Priprema web-mjesta

Uređaj mora biti instaliran u standardni bicikle 19-inčni prednji plan i stražnjeg objave. Koristite sljedeći postupak da biste se pripremili za bicikle instalaciju.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Da biste pripremili web-mjesto za instalaciju za bicikle

1. Provjerite je li sigurno Postavi uređaj na službeni paušalni stabilan te razine plošni (ili sličan).

2. Provjerite postoji li web-mjesta koje namjeravate postaviti standardne struje iz izvora neovisno ili za bicikle power raspodjele jedinica (PDU) s neprekinuto napajanje (UPS).

3. Provjerite je li taj jedan 2U vremensko razdoblje dostupna je na za bicikle u kojoj namjeravate Postavljanje uređaja.

![Ikona upozorenja](./media/storsimple-safety/IC740879.png)![podebljanom Debljina ikona](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **upozorenje!**

Provjerite jeste li povezani s dvije osobe dostupan za upravljanje težinu ako su ručno rukovanje postavke uređaja. Potpuno konfiguriran s prilozima možete obzir uzeli i do 32 kg (70 funte.).

### <a name="rack-prerequisites"></a>Preduvjeti za bicikle

Instalacija u u standardnom 19-inčni bicikle kabinetska s namijenjen je s prilozima 8100:

- Minimalna dubinu 27.84 inča iz bicikle objavu za objavu.
- Najveća težina 32 kg za uređaj
- Maksimalna natrag tlaka 5 Pascal (mjerača vode 0,5 mm).

### <a name="rack-mounting-rail-kit"></a>Komplet za bicikle ugrađivanjem rail

Skup postavljanje tračnicama navedeni su za korištenje s kabinetska 19-inčni za bicikle. Na tračnicama testirate učiniti Debljina Maksimalna s prilozima. Ove tračnicama će omogućiti instalacije više priloge bez gubitka razmak unutar za bicikle.


#### <a name="to-install-the-device-on-the-rails"></a>Da biste instalirali uređaj na na tračnicama

2. Izvršite taj korak samo ako unutarnji tračnicama su instalirani na vašem uređaju. Obično unutarnji tračnicama instaliranih na na tvorničke. Ako tračnicama su instalirane, instalirajte rail za lijevo i desno rail slajdova u strane nosačima s prilozima. Ih priložite pomoću šest metričkim vijke na svaku stranicu. Da biste lakše s usmjerenje, rail slajdovi označeni **LH – prednji plan** i **RH – prvi plan**, a kraja koji je ponuđenima prema stražnja na s prilozima ima tapered Završi.<br/>

    ![Prilaganje rail slajdova s prilozima nosačima](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
    **Attaching unutarnji rail slajdova na stranama na s prilozima**

       Oznaka | Opis
       ----- | -----------
       1     | M 3 x 4 vijke gumb glave
       2     | Nosačima slajdova

3. Priložite vanjski lijevom rail i vanjski desnom rail sklopova članovima za bicikle kabinetska okomito. Uglatim zagradama označavaju **LH** **RH**i **ovoj strani gore** će vas voditi kroz u pravom smjeru.

4. Pronađite PIN-ove rail na prednju i stražnja u sklopu rail. Proširivanje rail između objave za bicikle i umetnuti u PIN-ove u prednji plan i stražnjeg bicikle objavu okomiti člana rupe. Provjerite je li u sklopu rail razinu.

5. Pomoću dvoje ponuđena metričkim vijke sigurne u sklopu rail da biste za bicikle okomiti članova. Pomoću jednog vijak na u prvi plan, a drugi na na stražnja.

6. Ponovite te korake za u sklopu rail.<br/>

     ![Prilaganje rail slajdova kutije za bicikle](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Prilaganje vanjski rail skupovi za bicikle**

     Oznaka | Opis
     ----- | -----------
     1     | Clamping vijak
     2     | Vijak objavu prednju kvadrat rupe za bicikle
     3     | Lijevi rail prednju mjesto PIN-ove
     4     | Clamping vijak
     5     | Lijevi rail stražnjeg mjesto PIN-ove


### <a name="mounting-the-device-in-the-rack"></a>Postavljanje uređaja u za bicikle

Pomoću tračnicama bicikle koje ste upravo instalirali, izvedite sljedeće korake za postavljanje uređaja u za bicikle.

#### <a name="to-mount-the-device"></a>Postavljanje uređaja

1. Pomoću pomoćnika, Podignite na s prilozima i poravnali s tračnicama za bicikle.

2. Pažljivo Umetnite uređaj u na tračnicama, a zatim automatske uređaj potpuno u za bicikle kabinetska.<br/>

    ![Umetanjem uređaj za bicikle](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Postavljanje uređaja u za bicikle**


3. Ukloniti verzal lijevo i desno prednju flange izvlačenja besplatne verzal. Tipka CAPS LOCK flange jednostavno poravnati premjestite na flanges.

5. Sigurne s prilozima u za bicikle instalacijom jedan navedeni križni glave vijak kroz svaki flange, lijevo i desno.

4. Instalirajte flange caps tako da ih pritisnete položaj i poravnavanje oblika na mjestu.<br/>

     ![Instaliranje flange velika slova](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)

    **Instaliranje flange tipka CAPS LOCK**

     Oznaka | Opis
     ----- | -----------
     1     | Vijak fastening s prilozima

Sljedeći je korak da biste kabelski uređaja radi power, mreže i serijskog pristup.

## <a name="cable-your-storsimple-8100-device"></a>Kabel vaše StorSimple 8100 uređaja

Sljedeći postupci objašnjavaju kako kabelski uređaju StorSimple 8100 napajanja, mreže i serijsku veze.

### <a name="prerequisites"></a>Preduvjeti

Prije nego počnete kabeli uređaj, morate:

- Uređaj za pohranu, u potpunosti raspakirane i za bicikle postavljen.

- 2 power kabela koji ste dobili s uređajem

- Pristup 2 Power raspodjele jedinice (preporučeno).

- Mrežni kabeli

- Dobiveni serijskog kabela

- Serijski USB pretvornik s odgovarajući upravljački program na vašem Računalu instalirana (Ako je potrebno)

- Dobiveni 4 QSFP-na-SFP + prilagodnika za korištenje s 10 sučelje GbE mreže

- [Podržani hardver 10 sučelja GbE mrežom na uređaju StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)


### <a name="power-cabling"></a>Power kabeli

Vaš uređaj sadrži suvišnih Power i hlađenje Module (PCMs). Oba PCMs mora biti instaliran i povezan s različitim power izvora da biste bili sigurni visoke dostupnosti.

Izvršite sljedeće korake da biste kabelski uređaja radi power.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Mrežni kabeli

Konfiguracija aktivno čekanja za vaš uređaj nije: u bilo kojem trenutku, jednog modula kontroler je aktivna i obrada sve operacije na disku i mrežne tijekom u modulu kontroler uključen čekanja. Ako kontroler ne uspije, kontrolerom čekanja odmah aktivirati i nastavlja na disku i umrežavanje operacije.

Da biste podržava ovaj prebacivanje suvišnih kontroler, morate kabelski mrežom uređaj, kao što je opisano u sljedećim koracima.

#### <a name="to-cable-for-network-connection"></a>Da biste kabel za mrežne veze

1. Vaš uređaj ima šest sučelje mreže na svakom kontroleru: četiri 1 Gbps i dva 10 Gbps Ethernet priključci. Odredite različite priključci podataka na backplane uređaju.

    ![Backplane 8100 uređaja](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Ponovno željenog uređaja prikazuje priključke podataka**

     Oznaka   | Opis
     ------- | -----------
     0,1,4,5 |  1 sučelje GbE mreže
     2,3     | 10 sučelje GbE mreže
     6       | Serijski priključci

2. Pogledajte na sljedećem su dijagramu za mrežni kabeli. (Konfiguriranje minimalne mreže se prikazuje pune crte plave. Dodatna konfiguracija potrebne za visoke dostupnosti i performanse prikazuje se po točkaste crte.)


    ![Kabel 2U uređaj za mrežu](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Mrežni kabeli za svoj uređaj**


  	|Oznaka | Opis |
  	|----- | ----------- |
  	| ODGOVORA    | LAN-a s Internetom |
  	| B    | Kontroler 0 |
  	| C    | UO 0 |
  	| D    | Kontroler 1 |
  	| E    | UO 1 |
  	| F, G | Glavno računalo |
  	| 0-5  | Sučelje mreže |



Kada kabeli uređaj, zahtijeva minimalne konfiguracije:


- Najmanje dva sučelja mreže povezani na svakom kontroleru s jedan pristup putem oblaka i jedan za iSCSI. Podaci 0 priključak automatski omogućiti i konfigurirati putem serijski konzole za uređaj. Osim podataka 0, neki drugi priključak podataka i potrebno je konfigurirati putem portala za Azure klasični. U ovom slučaju povezivanje podataka 0 priključak za primarni LAN-a (mreža uz pristup Internetu). Druge priključke podataka moguće je povezati segment SAN/iSCSI LAN-a (VLAN) u mreži, ovisno o željenim uloge.

- Jednake sučelja na svakom kontroleru povezani s istom mrežom da biste bili sigurni dostupnost u slučaju kontroler prebacivanje. Na primjer, ako se odlučite za povezivanje podataka 0 i 3 podataka za jednu od kontrolera, morate povezati odgovarajuće podatke 0 i 3 podataka na drugi kontroleru.

Imajte na umu visoke dostupnosti i načina rada:


- Kada je to moguće, na svakom kontroleru konfigurirati par mrežno sučelje za pristup putem oblaka (1 GbE) i drugi par za iSCSI (10 GbE preporučuje se).

- Kada je to moguće, povežite sučelje mreže svaki kontroler s dvije različite skretnice osiguraj protiv promjenu pogreške. Slici dva 10 GbE mrežom sučelja, podataka 2 i 3 podataka iz svakog kontroler povezan s dvije različite skretnice.

Dodatne informacije potražite **sučelje mreže** u odjeljku [visoke dostupnosti preduvjeti za svoj uređaj StorSimple](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] Ako koristite SFP + transceivers vaše 10 sučelje GbE mreže, poslužite se navedeni QSFP-SFP + prilagodnika. Dodatne informacije potražite na [podržani hardvera i 10 sučelja GbE mrežom na uređaju StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).



### <a name="serial-port-cabling"></a>Serijski priključak kabeli

Izvršite sljedeće korake da biste kabelski serijskog priključka.

#### <a name="to-cable-for-serial-connection"></a>Da biste kabel za serijsku vezu

1. Uređaj je serijski priključak na svakom kontroleru koja je označena ikonom ključa. Pogledajte na slici u odjeljku [mrežni kabeli](#network-cabling) da biste pronašli Serijski priključci na backplane uređaju.

2. Odredite aktivni kontroler na vaš uređaj backplane. Trepćućeg plava LED označava kontrolerom aktivna.

3. Pomoću navedenih serijski kabeli (Ako je potrebno, USB serijski pretvornika za vaše računalo), a računala (uz terminal emulacija na uređaju) ili konzoli povezati serijski priključak kontrolerom active.

4. Instalirajte upravljačke programe za serijski USB (koje se isporučuju uz uređaj) na računalu.

5. Postavljanje serijskog veze na sljedeći način: 115,200 brzina, bitova 8 podataka, bitne Zaustavi 1, bez slične značajke i tok postavljen na ništa.

6. Provjerite je li radi li se veza pritiskom na tipku Enter na konzoli sustava. Izbornik serijski konzole prikazivati.

>[AZURE.NOTE] **Upravljanje lights-Out**: kada uređaj je instaliran u udaljene podatkovnog centra ili u sobu za računala imaju ograničeni pristup, pazite da serijski veze s obje kontrolera uvijek povezani s konzole za serijski promjenu ili slične opreme. Time se omogućuje postupke za podršku i -grupiranje daljinskog upravljanja ako postoje to izbjeglo mreže ili neočekivane pogreške.

Vaš uređaj sada cabled napajanja, pristup mreži i serijsku povezivanje. Sljedeći je korak konfiguriranje softver i implementacija uređaj.

## <a name="next-steps"></a>Daljnji koraci

Saznajte kako [implementirati i konfigurirati StorSimple uređaju lokalnog](storsimple-deployment-walkthrough.md).
