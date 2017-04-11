<properties
   pageTitle="Instaliranje uređaja StorSimple 8600 | Microsoft Azure"
   description="Opisuje kako otpakiravanje bicikle postavljanja i kabelski uređaju StorSimple 8600 prije implementacije i konfiguriranje softvera."
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
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Otpakiravanje, za bicikle postavljanja i kabelski uređaju StorSimple 8600

## <a name="overview"></a>Pregled
Vaše 8600 Microsoft Azure StorSimple je riječ o uređaju s dva prilozima i sastoji se od primarne i programa EBOD s prilozima. Pomoću ovog praktičnog vodiča objašnjava kako otpakiravanje, za bicikle postavljanja i kabel StorSimple 8600 hardver prije nego što konfigurirati StorSimple softvera.

## <a name="unpack-your-storsimple-8600-device"></a>Otpakiravanje StorSimple 8600 uređaj

Sljedeći koraci pružaju Očisti, detaljne upute o tome kako otpakiravanje uređaj za pohranu StorSimple 8600. Ovaj uređaj je isporučena u dva okvira, jedan za primarni s prilozima, a drugi za s prilozima EBOD. Ove dva okvira pa spremaju se u jedan okvir.

### <a name="prepare-to-unpack-your-device"></a>Priprema za otpakiravanje uređaju

Prije otpakiravanje uređaju, pročitajte članak sljedeće informacije.


![Ikona upozorenja](./media/storsimple-safety/IC740879.png)![podebljanom Debljina ikona](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **upozorenje!**

1. Provjerite jeste li povezani s dvije osobe dostupan za upravljanje Debljina uređaja ako su je zadužen ručno. Potpuno konfiguriran s prilozima možete obzir uzeli i do 32 kg (70 funte.).
1. U okviru postavite na površini za nehijerarhijsku, razine.

Nakon toga slijedite sljedeće korake da biste otpakiravanje uređaju.

#### <a name="to-unpack-your-device"></a>Da biste otpakiravanje uređaju

1. Provjeri okvira i foam pakiranja za crushes, rezovima, vode oštećenja ili drugim očite oštećenja. Ako je okvir ili pakiranja ozbiljno oštećen, otvorite okvir. Imajte [obratite se Microsoftovoj službi za podršku](storsimple-contact-microsoft-support.md) da biste pomoći pri procjeni li uređaj dobro radi li ispravno.

2. Otvaranje okvira za vanjski, a zatim uklanjanje dva okvira odgovara primarni i EBOD priloge. Sada možete otpakiravanje primarni i EBOD priloge. Na sljedećoj je slici prikazan raspakirane prikaz jedne od na priloge.

    ![Otpakiravanje uređaj za pohranu](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)

    **Raspakirane prikaz uređaj za pohranu**

     Oznaka | Opis
     ----- | -------------
     1     | Okvir za spremanje
     2     | SAS kabeli (paleti dodatna oprema i kabeli)
     3     | Dno foam
     4     | Uređaj
     5     | Gornji foam
     6     | Jezični okvir

3. Nakon Raspakiravanje dva okvira, provjerite jeste li povezani s:

  - 1 primarni s prilozima (primarni s prilozima i s prilozima EBOD nalaze se u dva zasebna okvira)
  - 1 EBOD s prilozima
  - power kablovima 4, 2 u svaki okvir
  - 2 kabeli SAS (povezati primarni s prilozima s prilozima EBOD)
  - 1 ga Ethernet kabela
  - 2 kabeli serijskih konzole
  - 1 serijski USB pretvornik za serijski pristup
  - 4 QSFP-na-SFP + prilagodnika za korištenje s 10 sučelje GbE mreže
  - 2 za bicikle postavljanja Kompleti (4 strani tračnicama s postavljanje hardver, 2 svaki primarni s prilozima i s prilozima EBOD), 1 u svaki okvir
  - Početak rada dokumentacija

    Ako niste ne primite nikakve stavki naveden, [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md).  

Sljedeći je korak za bicikle postavljanja uređaja.

## <a name="rack-mount-your-storsimple-8600-device"></a>Za bicikle postavljanja StorSimple 8600 uređaj

Poduzmite sljedeće korake da biste instalirali uređaj za pohranu StorSimple 8600 u standardni bicikle 19-inčni s prednje i stražnjeg objave. Ovaj uređaj dolazi s dva priloge: primarni s prilozima i programa EBOD s prilozima. Oboje od sljedećeg moraju biti bicikle postavljen.

Instalacija se sastoji od više koraka, od kojih svaka se spominju u sljedećih postupaka.

> [AZURE.IMPORTANT]
Uređaji StorSimple mora biti postavljena za bicikle za pravilnog.



### <a name="site-preparation"></a>Priprema za web-mjesta

Na priloge mora biti instaliran u standardni bicikle 19-inčni prednji plan i stražnjeg objave. Koristite sljedeći postupak da biste se pripremili za bicikle instalaciju.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Da biste pripremili web-mjesto za instalaciju za bicikle

1. Provjerite jesu li da primarni i EBOD priloge koji se sigurno na površina ravne, stabilan i razine rad obrubu (ili sličan).

2. Provjerite postoji li web-mjesta koje namjeravate postaviti standardne struje iz izvora neovisno ili bicikle Power jedinica za raspodjelu (PDU) s neprekinuto napajanje (UPS).

3. Provjerite je li taj vremensko razdoblje (2 X 2U) jedan 4U dostupne na za bicikle u kojoj namjeravate dostupnosti u priloge.

![Ikona upozorenja](./media/storsimple-safety/IC740879.png)![podebljanom Debljina ikona](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **upozorenje!**

 Provjerite jeste li povezani s dvije osobe dostupan za upravljanje težinu ako su ručno rukovanje postavke uređaja. Potpuno konfiguriran s prilozima možete obzir uzeli i do 32 kg (70 funte.).

### <a name="rack-prerequisites"></a>Preduvjeti za bicikle

Instalacija u u standardnom 19 inčni bicikle kabinetska s namijenjena na priloge:

- Minimalna dubinu 27.84 inča iz objavu na objavu za bicikle
- Najveća težina 32 kg za uređaj
- Maksimalna natrag tlaka od 5 Pascal (mjerača vode 0,5 mm)

### <a name="rack-mounting-rail-kit"></a>Komplet za bicikle ugrađivanjem rail

Skup postavljanje tračnicama objavit ćemo zajedno za bicikle 19-inčni kabinetska. Na tračnicama testirate učiniti Debljina Maksimalna s prilozima. Ove tračnicama će omogućiti instalacije više priloge bez gubitka prostor unutar za bicikle. Prvo instalirajte s prilozima EBOD.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>Da biste instalirali s prilozima EBOD na na tračnicama

2. Izvršite taj korak samo ako unutarnji tračnicama su instalirani na vašem uređaju. Obično unutarnji tračnicama instaliranih na na tvorničke. Ako tračnicama su instalirane, instalirajte rail za lijevo i desno rail slajdova u strane nosačima s prilozima. Ih priložite pomoću šest metričkim vijke na svaku stranicu. Da biste lakše s usmjerenje, rail slajdovi označeni **LH – prednji plan** i **RH – prvi plan**, a kraja koji je ponuđenima prema stražnja na s prilozima ima tapered Završi.

    ![Prilaganje rail slajdova s prilozima nosačima](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Prilaganje rail slajdova strane na s prilozima**

    Oznaka | Opis
    ----- | -----------
    1     | M 3 x 4 vijke gumb glave
    2     | Nosačima slajdova

3. Priloži rail lijevo i desno rail sklopova članovima za bicikle kabinetska okomito. Zagrade označene su **LH** **RH**i **ovoj strani gore** će vas voditi kroz pravom smjeru.

4. Pronađite PIN-ove rail na prednju i stražnja u sklopu rail. Proširivanje rail između objave za bicikle i umetnuti u PIN-ove u prednji plan i rupe okomiti člana stražnja bicikle objavu. Provjerite je li u sklopu rail razinu.

5. Sigurne u sklopu rail da biste za bicikle okomiti članovi pomoću dvije metričkim vijke navedene. Pomoću jednog vijak na u prvi plan, a drugi na na stražnja.

6. Ponovite te korake za u sklopu rail.

     ![Prilaganje rail slajdova kutije za bicikle](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Prilaganje rail skupovi za bicikle**

     Oznaka | Opis
     ----- | -----------
     1     | Clamping vijak
     2     | Vijak objavu prednju kvadrat rupe za bicikle
     3     | Lijeva prednju rail mjesto PIN-ove
     4     | Clamping vijak
     5     | Lijevi stražnjeg rail mjesto PIN-ove

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>Postavljanje s prilozima EBOD u za bicikle

Koristite za bicikle tračnicama koje ste upravo instalirali, izvedite sljedeće korake postavljanja s prilozima EBOD u za bicikle.

#### <a name="to-mount-the-ebod-enclosure"></a>Postavljanje s prilozima EBOD

1. Pomoću pomoćnika, Podignite na s prilozima i poravnali s tračnicama za bicikle.

2. Pažljivo na s prilozima umetnuli na tračnicama, a zatim automatske je potpuno u za bicikle kabinetska.

    ![Umetanjem uređaj za bicikle](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Postavljanje s prilozima u za bicikle**

3. Ukloniti verzal lijevo i desno prednju flange izvlačenja besplatne verzal. Tipka CAPS LOCK flange jednostavno poravnati premjestite na flanges.

4. Sigurne na s prilozima u za bicikle instalacijom jedan navedeni križni glave vijak kroz svaki flange, lijevo i desno.

4. Instalirajte flange caps tako da ih pritisnete položaj i poravnavanje oblika u mjesto.

     ![Instaliranje flange velika slova](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)

    **Instaliranje flange tipka CAPS LOCK**

     Oznaka | Opis
     ----- | -----------
     1     | Vijak fastening s prilozima


### <a name="mounting-the-primary-enclosure-in-the-rack"></a>Postavljanje primarnog s prilozima u za bicikle

Nakon što dovršite postavljanje s prilozima EBOD, morat ćete postaviti primarni s prilozima slijedeći korake.

> [AZURE.NOTE]
>
> - Nije moguće imati nekoliko prazan slobodnih u bicikle između primarni s prilozima i s prilozima EBOD.
> - Pomoću kabela SAS navedeni 2m primarni s prilozima povezati s prilozima EBOD.
> - Postoje bez ograničenja relativni položaj glavni jedinice EBOD jedinicu. Stoga se primarni s prilozima možete staviti u gornjoj vremensko razdoblje i s prilozima EBOD u nastavku – ili obrnuto.

Sljedeći je korak da biste kabelski uređaja radi power, mreže i serijskog pristup.

## <a name="cable-your-storsimple-8600-device"></a>Kabel vaše 8600 StorSimple uređaja

Sljedeći postupci objašnjavaju kako kabelski StorSimple 8600 uređaja radi power, mreže i serijskog veze.

### <a name="prerequisites"></a>Preduvjeti

Prije nego počnete kabelski uređaj, morate:

- Vaš primarni s prilozima i s prilozima EBOD potpuno raspakirane
- 4 power kabeli (2 svaki primarni i s prilozima EBOD) koju ste dobili s uređajem
- 2 SAS kabeli dobili uz uređaj da biste se povezali s prilozima EBOD za primarni s prilozima
- Pristup 2 Power raspodjele jedinice (PDUs) (preporučeno)
- Mrežni kabeli
- Dobiveni serijskog kabela
- Serijski USB pretvornik s odgovarajući upravljački program na vašem Računalu instalirana (Ako je potrebno)
- Dobiveni 4 QSFP-na-SFP + prilagodnika za korištenje s 10 sučelje GbE mreže
- [Podržani hardver 10 sučelja GbE mrežom na uređaju StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SAS i Power kabeli

Uređaj je primarni s prilozima i programa EBOD s prilozima. Potreban je jedinice da biste se zajedno cabled za povezivanje serijski priložene SCSI (SAS) i power.

Kada postavljate ovaj uređaj prvi put, poduzeti korake za SAS kabeli prvi put, a zatim dovršite korake za power kabeli.

[AZURE.INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Mrežni kabeli

Uređaj je u konfiguraciji aktivno čekanja: u bilo kojem trenutku, jednog modula kontroler je aktivna i obrada sve operacije na disku i mrežne tijekom u modulu kontroler uključen čekanja. Ako dođe do pogreške kontroler, kontrolerom čekanja odmah aktivira i nastavljaju se sve na disku i rad s mrežom operacije.

Da biste podržava ovaj prebacivanje suvišnih kontroler, morate kabelski mrežom uređaju, kao što je prikazano u sljedećim koracima.

#### <a name="to-cable-for-network-connection"></a>Da biste kabel za mrežne veze

1. Vaš uređaj ima šest sučelje mreže na svakom kontroleru: četiri 1 Gbps i dva 10 Gbps Ethernet priključci. Pogledajte na sljedećoj slici identificirati priključke podataka na backplane uređaju.

     ![Backplane 8600 uređaja](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Ponovno od uređaju prikazuje priključke podataka**

     Oznaka   | Opis
     ------- | -----------
     0,1,4,5 |  1 sučelje GbE mreže
     2,3     | 10 sučelje GbE mreže
     6       | Serijski priključci



1. Pogledajte na sljedećem su dijagramu za mrežni kabeli. (Konfiguriranje minimalne mreže se prikazuje pune crte plave. Visoke dostupnosti i performanse, dodatne konfiguracijske potreban je prikazana po točkaste crte.)

![Kabela uređaju 4U za mrežu](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Mrežni kabeli za svoj uređaj**

Oznaka | Opis
----- | -----------
ODGOVORA    | LAN-a s Internetom
B    | Kontroler 0
C    | UO 0
D    | Kontroler 1
E    | UO 1
F    | EBOD kontroler 0
G    | EBOD kontroler 1
H, LI  | Glavno računalo (na primjer, datoteka poslužitelji)
0-5  | Sučelje mreže
6    | Primarni s prilozima
7    | S EBOD prilozima

Kada kabeli uređaj, zahtijeva minimalne konfiguracije:


- Najmanje dva sučelja mreže povezani na svakom kontroleru s jedan pristup putem oblaka i jedan za iSCSI. Podaci 0 priključak automatski omogućiti i konfigurirati putem serijski konzole za uređaj. Osim podataka 0, neki drugi priključak podataka i potrebno je konfigurirati putem portala za Azure klasični. U ovom slučaju povezivanje podataka 0 priključak za primarni LAN-a (mreža uz pristup Internetu). Druge priključke podataka moguće je povezati segment SAN/iSCSI LAN-a (VLAN) u mreži, ovisno o željenim uloge.

- Jednake sučelja na svakom kontroleru povezani s istom mrežom da biste bili sigurni dostupnost u slučaju kontroler prebacivanje. Na primjer, ako se odlučite za povezivanje podataka 0 i 3 podataka za jednu od kontrolera, morate povezati odgovarajuće podatke 0 i 3 podataka na drugi kontroleru.

Imajte na umu visoke dostupnosti i načina rada:


- Kada je to moguće, na svakom kontroleru konfigurirati par mrežno sučelje za pristup putem oblaka (1 GbE) i drugi par za iSCSI (10 GbE preporučuje se).

- Kada je to moguće, povežite sučelje mreže svaki kontroler s dvije različite skretnice osiguraj protiv promjenu pogreške. Slici dva 10 GbE mrežom sučelja, podataka 2 i 3 podataka iz svakog kontroler povezan s dvije različite skretnice. Dodatne informacije potražite **sučelje mreže** u odjeljku [visoke dostupnosti preduvjeti za svoj uređaj StorSimple](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] Ako koristite SFP + transceivers s vašeg 10 sučelja GbE mreže, pomoću navedenih QSFP-SFP + prilagodnika. Dodatne informacije potražite na [podržani hardvera i 10 sučelja GbE mrežom na uređaju StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

### <a name="serial-port-cabling"></a>Serijski priključak kabeli

Izvršite sljedeće korake da biste kabelski serijskog priključka.

#### <a name="to-cable-for-serial-connection"></a>Da biste kabel za serijsku vezu

1. Uređaj je serijski priključak na svakom kontroleru koja je označena ikonom ključa. Da biste pronašli Serijski priključci odnose se na slici koji podatke prikazuje na priključaka na stražnjoj strani uređaj.

2. Odredite aktivni kontroler na vaš uređaj backplane. Trepćućeg plava LED označava kontrolerom aktivna.

3. Pomoću navedenih serijskog kabela (Ako je potrebno, USB serijski pretvornika za vaše računalo), a računala (uz terminal emulacija uređaju) ili konzoli povezati serijski priključak active kontrolera.

4. Instalirajte upravljačke programe za serijski USB (koje se isporučuju uz uređaj) na računalu.

5. Postavljanje serijskog veze na sljedeći način:
   - Brzina 115,200
   - bitova 8 podataka.
   - bitne 1 tabulatora
   - Nema slične značajke
   - Tok postavljen na **ništa**

6. Provjerite je li radi li se veza pritiskom na tipku Enter na konzoli sustava. Izbornik serijski konzole prikazivati.

> [AZURE.NOTE] **Upravljanja lights-Out:** Kada uređaj instalirate u udaljene podatkovnog centra ili u sobu za računala imaju ograničeni pristup, provjerite je li da serijski veze s obje kontrolera uvijek povezani s konzole za serijski promjenu ili slične opreme. Time se omogućuje-grupiranje daljinskog upravljanja i postupci za podršku u slučaju prekidu mreže ili neočekivane pogreške.

Dovršite kabeli uređaja radi power, pristup mreži i serijsku vezu. Sljedeći je korak konfiguriranje softvera na vašem uređaju.

## <a name="next-steps"></a>Daljnji koraci

Sada ste spremni za [implementaciju i konfiguriranje lokalnog StorSimple uređaj](storsimple-deployment-walkthrough-u2.md).
