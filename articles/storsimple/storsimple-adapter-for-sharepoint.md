<properties 
   pageTitle="StorSimple prilagodnik za SharePoint | Microsoft Azure"
   description="U članku se opisuje kako instalirati i konfigurirati ili uklanjanje StorSimple prilagodnik za SharePoint u farmi sustava SharePoint server."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/11/2016"
   ms.author="v-sharos" />

# <a name="install-and-configure-the-storsimple-adapter-for-sharepoint"></a>Instaliranje i konfiguriranje StorSimple prilagodnik za SharePoint

## <a name="overview"></a>Pregled

StorSimple prilagodnik za SharePoint je komponenta korisničkog sučelja koje možete unijeti Microsoft Azure StorSimple fleksibilne prostora za pohranu i zaštitu podataka na farmama sustava SharePoint server. Prilagodnik možete koristiti da biste premjestili sadržaj binarni velike objekta (BLOB) iz sadržaja baze podataka sustava SQL Server uređaj za pohranu sustava Microsoft Azure StorSimple hibridnog oblaka.

StorSimple prilagodnik za SharePoint funkcionira kao davatelj udaljene spremišta blobova PLATFORME (RBS) i koristi značajku udaljenog spremišta BLOBOVA komponente SQL Server za pohranu nestrukturirane sadržaja sustava SharePoint (u obliku blob-ova) na datotečnom poslužitelju koji je sigurnosno StorSimple uređaj.

>[AZURE.NOTE] StorSimple prilagodnik za SharePoint podržava SharePoint Server 2010 alat za analizu daljinske BLOB prostora za pohranu (RBS). Ne podržava SharePoint Server 2010 vanjskih BLOB prostora za pohranu (EBS).

- Da biste preuzeli StorSimple prilagodnik za SharePoint, idite na [StorSimple prilagodnik za SharePoint] [ 1] u Microsoft Download Center.

- Informacije o planiranju RBS i RBS ograničenja potražite u odjeljku [Deciding da biste koristili RBS u sustavu SharePoint 2013] [ 2] ili [Osmišljavanje RBS (SharePoint Server 2010)][3].

Do kraja ovog pregled ukratko opisuje ulogu StorSimple prilagodnik za SharePoint i SharePoint ograničenja kapacitetu i performanse koja ste trebali imati na umu prije nego što ste instalirali i konfigurirali prilagodnik. Kad pregledate taj podatak, idite na [StorSimple prilagodnik za instalaciju sustava SharePoint](#storsimple-adapter-for-sharepoint-installation) da biste započeli s postavljanjem prilagodnika.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>StorSimple prilagodnik za prednosti sustava SharePoint

Na web-mjestu sustava SharePoint sadržaj pohranjen kao nestrukturirane podatke BLOB u jednu ili više baza podataka sadržaja. Prema zadanim postavkama, te baze podataka se nalaze na računalima na kojima su operacijski sustav SQL Server, a nalaze se na farmi poslužitelja sustava SharePoint. Blob-Ova informacija možete povećati veličinu, troše velike količine prostora za pohranu na lokalni. Zbog toga, možda ćete morati pronaći rješenje drugi, manje skupi prostor za pohranu. SQL Server sadrži tehnologiju koja se naziva udaljene Blob prostora za pohranu (RBS) koji omogućuje spremanje BLOB sadržaj u datotečnom sustavu izvan baze podataka SQL Server. S RBS, blob-Ova se može smjestiti u datotečnom sustavu na računalu sa sustavom SQL Server ili moguće pohraniti u datotečnom sustavu na drugom računalu poslužitelja.

RBS potreban je pomoću davatelja RBS, kao što su StorSimple prilagodnik za SharePoint, da biste omogućili RBS u sustavu SharePoint. StorSimple prilagodnik za sustav SharePoint radi s RBS, vam premještanje blob polja na poslužitelju sigurnosnu kopiju sustava Microsoft Azure StorSimple. Microsoft Azure StorSimple zatim sprema BLOB podatke lokalno i u oblaku, ovisno o korištenju. Lokalno nalaze blob-ova koji su vrlo aktivni (obično se nazivaju sloju 1 ili tipkovni podataka). Manje aktivni podataka i arhiviranje podataka koji se nalaze u oblaku. Kada omogućite RBS na baze podataka sadržaja, sve nove BLOB stvarati u sustavu SharePoint je sadržaj pohranjen na uređaju StorSimple, a ne u bazi podataka sadržaja.

Implementacija sustava Microsoft Azure StorSimple RBS pruža sljedeće prednosti:

- Premještanje BLOB sadržaj na istom poslužitelju, možete smanjiti učitavanja upita na SQL Server koja možete poboljšati reaktivnosti sustava SQL Server. 

- Azure StorSimple koristi Poništavanje duplikacije i sažimanje da biste smanjili veličinu podataka.

- Azure StorSimple omogućuje zaštitu podataka u obliku lokalno i oblaka snimke. Osim toga, ako stavite same baze podataka na StorSimple uređaju, možete stvoriti sigurnosne kopije baze podataka sadržaja i blob-ova zajedno pad dosljedno. (Premještanje baze podataka sadržaja na uređaju podržano je samo za uređaj niz StorSimple 8000. Ta značajka nije podržana za 5000 ili 7000 nizova.)

- Azure StorSimple uključuje značajke oporavka Izrada uključujući prebacivanje, oporavak datoteka i glasnoću (uključujući test oporavak) i brzog vraćanje podataka.

- Softver za oporavak podataka, kao što su Kroll Ontrack PowerControls, možete koristiti s StorSimple snimke BLOB podataka obaviti oporavak na razini stavke sadržaja sustava SharePoint. (Ovaj softver za oporavak podataka nije zasebnu kupnju).

- StorSimple prilagodnik za SharePoint priključuje u središnjoj administraciji sustava SharePoint portal omogućujući vam da biste upravljali cijelu rješenje sustava SharePoint sa središnjeg mjesta.

Premještanje sadržaja BLOB u datotečnom sustavu unijeti druge ušteda troškova i pogodnosti paketa. Na primjer, pomoću RBS možete smanjiti potrebe za skupi prostor za pohranu sloju 1 i jer je smanjuje baza podataka sadržaja, RBS možete smanjiti broj baze podataka potreban u farmi sustava SharePoint server. Međutim, Ostali čimbenici, kao što su ograničenja veličine baze podataka i količinu sadržaja koji nisu RBS i utječu preduvjeti za pohranu. Dodatne informacije o troškovima i prednosti korištenja RBS potražite u članku [Planiranje RBS (SharePoint Foundation 2010)] [ 4] i [Deciding da biste koristili RBS u sustavu SharePoint 2013][5].

### <a name="capacity-and-performance-limits"></a>Ograničenja kapacitetu i performanse

Prije nego što se preporučuje korištenje RBS u rješenje sustava SharePoint, imajte na umu da testirano performanse i kapaciteta ograničenja sustava SharePoint Server 2010 i SharePoint Server 2013 i te ograničenja odnosa prihvatljiva performansi. Dodatne informacije potražite u članku [ograničenja softvera i ograničenja za SharePoint 2013](https://technet.microsoft.com/library/cc262787.aspx).

Prije no što konfigurirate RBS, pogledajte sljedeće:

- Provjerite je li ukupnu veličinu sadržaja (Veličina baze podataka sadržaja) te veličinu sve povezane externalized blob-ova premašuje ograničenje veličine RBS podržava sustava SharePoint. To ograničenje je 200 GB. 

    **Baza podataka sadržaja mjere i BLOB veličina**

     1. Pokrenite ovaj upit u središnjoj administraciji WFE. Pokretanje ljuske za upravljanje sustavom SharePoint, a zatim unesite sljedeću naredbu komponente Windows PowerShell da biste dobili Veličina baze podataka sadržaja:

        `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`

         Ovaj korak dohvaća Veličina baze podataka sadržaja na disku.

     2. Pokrenite jedan od sljedećih SQL upita u SQL Management Studio u okviru za SQL server na svakom baze podataka sadržaja, a rezultat dodati broj dobivenog u korak 1.

        U sustavu SharePoint 2013 baza podataka sadržaja, unesite:

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`

        U sustavu SharePoint 2010 baza podataka sadržaja, unesite:

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`

        Ovaj korak dohvaća veličina blob polja koja ste je externalized.

- Preporučujemo da na uređaju StorSimple lokalno spremanje svih BLOB i baza podataka sadržaja. Uređaj StorSimple je dva čvor klaster za visoke dostupnosti. Smještanje baza podataka sadržaja i blob polja na uređaju StorSimple nudi visoke dostupnosti.

    Da biste premjestili sadržaj baze podataka na uređaju StorSimple, koristite tradicionalni SQL Server migracije najbolje prakse. Premjestite bazu podataka samo kada je za zajedničko korištenje datoteka putem RBS premještena sav sadržaj BLOB iz baze podataka. Ako odaberete da biste premjestili baze podataka sadržaja na StorSimple uređaju, preporučujemo da Konfiguriranje baze podataka sadržaja prostora za pohranu na uređaju kao primarni jedinica.

- U programu Microsoft Azure StorSimple, ako koristite tiered jedinicama, ne postoji način da bi sadržaj spremiti lokalno na StorSimple uređaj će biti tiered za Microsoft Azure za pohranu u oblaku. Dakle, preporučujemo korištenje StorSimple lokalno prikvačene količine u kombinaciji s RBS sustava SharePoint. To će osigurati sav sadržaj BLOB ostaje lokalno na uređaj StorSimple i neće se premjestiti u Microsoft Azure.

- Ako ne spremate baza podataka sadržaja na uređaju StorSimple, koristite tradicionalni SQL Server visoke dostupnosti najbolje prakse koji podržavaju RBS. SQL Server Klasteriranje podržava RBS, tijekom SQL Server zrcaljenje ne. 

>[AZURE.WARNING] Ako niste omogućili RBS, ne preporučujemo Premještanje baze podataka sadržaja u StorSimple uređaja. Ovo je neprovjerenog konfiguracije.
 
## <a name="storsimple-adapter-for-sharepoint-installation"></a>StorSimple prilagodnik za instalaciju sustava SharePoint

Da biste mogli instalirati StorSimple prilagodnik za SharePoint, morate konfigurirati StorSimple i provjerite je li farme poslužitelja sustava SharePoint i SQL Server instancijacije zadovoljavaju sve preduvjete. Pomoću ovog praktičnog vodiča opisuju preduvjeti za konfiguraciju, kao i postupke za instaliranje i nadogradnja StorSimple prilagodnik za SharePoint. 

## <a name="configure-prerequisites"></a>Konfiguriranje preduvjeti

Da biste mogli instalirati StorSimple prilagodnik za SharePoint, provjerite je li uređaj StorSimple, farme poslužitelja sustava SharePoint i SQL Server instancijacije zadovoljava sljedeće preduvjete.

### <a name="system-requirements"></a>Sistemski preduvjeti

StorSimple prilagodnik za sustav SharePoint radi s sljedeće hardvera i softvera:

- Podržani operacijski sustav – Windows Server 2008 R2 SP1, Windows Server 2012 ili Windows Server 2012 R2 

- Podržani verzije sustava SharePoint – web-mjesto sustava SharePoint Server 2010 ili u okvir za SharePoint Server 2013

- Podržana verzija sustava SQL Server – SQL Server 2008 Enterprise Edition, SQL Server 2008 R2 Enterprise Edition ili SQL Server 2012 Enterprise Edition

- Podržani uređaji StorSimple – StorSimple 8000 niz, StorSimple 7000 niz ili StorSimple 5000 niz.

### <a name="storsimple-device-configuration-prerequisites"></a>Preduvjeti za konfiguraciju StorSimple uređaja

Uređaj StorSimple bloka uređaja i kao zahtijeva datotečnom poslužitelju na kojem se podaci mogu nalaziti. Preporučujemo da koristite zaseban server umjesto postojećeg poslužitelja s farme sustava SharePoint. Ovaj poslužitelj datoteka mora biti na na istom lokalne mreže (LAN-a) kao SQL Server računalu koje hostira baze podataka sadržaja. 

>[AZURE.TIP] 
>
>- Ako konfigurirate farme sustava SharePoint za visoke dostupnosti, trebali biste i implementacija poslužitelja datoteka za visoke dostupnosti.
>
>- Ako ne spremate baze podataka sadržaja na uređaju StorSimple, koristite tradicionalni visoke dostupnosti najbolje prakse koji podržavaju RBS. SQL Server Klasteriranje podržava RBS, tijekom SQL Server zrcaljenje ne. 

Provjerite je li uređaj StorSimple ispravno konfigurirano i odgovarajuće jedinice za podršku implementaciju sustava SharePoint konfigurirane i pristupiti s računala sustava SQL Server. Ako još nije implementirati i konfigurirati StorSimple uređaj, idite na [uvođenja lokalnog StorSimple uređaj](storsimple-deployment-walkthrough.md) . Imajte na umu IP adresu na StorSimple uređaj. jer će vam tijekom StorSimple prilagodnik za instalaciju sustava SharePoint. 

Osim toga, provjerite je li ispunjava li glasnoće će se koristiti za BLOB externalization sljedeće preduvjete:

- Glasnoću morate oblikovan veličina jedinice za dodjelu 64 KB.

- Web-sučelja (WFE) i poslužitelji aplikacija moraju imati mogućnost da biste pristupili glasnoće putem puta univerzalni imenovanja konvencija (UNC). 

- Pisanje glasnoće, morate ga konfigurirati farme poslužitelja sustava SharePoint.

>[AZURE.NOTE] Nakon što instalirate i konfigurirate prilagodnik, sve externalization BLOB mora proći kroz StorSimple uređaj (uređaj će izlaganje jedinice za SQL Server i upravljanje razine za pohranu). Ne možete koristiti druge ciljeve BLOB externalization.
 
Ako planirate pomoću upravitelja snimka StorSimple da bi snimke podataka BLOB i baze podataka, svakako instalirajte StorSimple snimke Manager na poslužitelju baze podataka da bi je mogli koristiti Writer servisa SQL za implementaciju sustava Windows glasnoću sjene Kopiraj servisa (VSS). 

>[AZURE.IMPORTANT] Upravitelj snimka StorSimple ne podržava u sustavu SharePoint vss program i ne možete preuzeti aplikaciju dosljedan snimke podataka sustava SharePoint. U scenariju s SharePoint StorSimple snimke Upravitelj sadrži samo rušenje dosljedan sigurnosne kopije. 
 
## <a name="sharepoint-farm-configuration-prerequisites"></a>Preduvjeti za konfiguraciju farme sustava SharePoint

Provjerite je li u farmi sustava SharePoint server pravilno konfigurirana, na sljedeći način:

- Provjerite je li farme poslužitelja sustava SharePoint u dobar stanju pa provjerite sljedeće: 

- Sve WFE sustava SharePoint i registrirana na farmi poslužitelja aplikacije koristite i možete pinged s poslužitelja na kojem će biti instalirate StorSimple prilagodnik za SharePoint.

- Servis za mjerenje vremena sustava SharePoint (SPTimerV3 ili SPTimerV4) radi svih WFE poslužitelja i poslužitelj aplikacije.

- Servis za mjerenje vremena sustava SharePoint i IIS aplikacija na kojem se izvodi web-mjestu središnje administracije sustava SharePoint imati administratorske ovlasti. 

- Provjerite je li Internet Explorer Poboljšana sigurnost kontekstu (IE ESC) je onemogućen. Slijedite ove korake da biste onemogućili ESC preglednika Internet Explorer:

    1. Zatvorite sve instance programa Internet Explorer.

    2. Pokrenite Upravitelj poslužitelja.

    3. U lijevom oknu kliknite **Lokalni poslužitelj**.

    4. **U desnom oknu uz **Poboljšana konfiguracija sigurnosti preglednika Internet Explorer**, kliknite.**

    5. **U odjeljku **administratora**, kliknite.**

    6. Kliknite **u redu**.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Preduvjeti udaljenog spremišta BLOBOVA (RBS)

Provjerite koristite li podržanu verziju sustava SQL Server. Samo sljedeće verzije nisu podržane i moći koristiti RBS:

- SQL Server 2008 Enterprise Edition

- SQL Server 2008 R2 Enterprise Edition

- SQL Server 2012 Enterprise Edition

Blob polja mogu biti externalized samo količine koja predstavlja StorSimple uređaj sa sustavom SQL Server. Podržane su druge ciljeve za BLOB externalization.

Nakon dovršetka svih koraka konfiguracije pripremni, otvorite da biste [instalirali StorSimple prilagodnik za SharePoint](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-the-storsimple-adapter-for-sharepoint"></a>Instaliranje StorSimple prilagodnik za SharePoint

Poduzmite sljedeće korake da biste instalirali StorSimple prilagodnik za SharePoint. Ako se ponovno instalirati softver, pročitajte članak [nadograditi ili ponovno instalirajte StorSimple prilagodnik za SharePoint](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). Vrijeme potrebno za instalaciju ovisi o ukupan broj baza podataka sustava SharePoint u farmi sustava SharePoint server.

[AZURE.INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]


## <a name="configure-rbs"></a>Konfiguriranje RBS

Nakon što instalirate StorSimple prilagodnik za SharePoint, konfigurirati RBS kao što je opisano u nastavku.

>[AZURE.TIP] StorSimple prilagodnik za SharePoint priključuje u stranicu sustava SharePoint središnje administracije dopuštanja RBS da biste ga omogućiti ili onemogućiti na svakom baze podataka sadržaja u farmi sustava SharePoint. Međutim, omogućivati ili onemogućivati RBS bazu podataka sadržaja uzrokuje Vrati IIS, a koje, ovisno o vašoj konfiguraciji farme može invertiraju poremetiti dostupnost na SharePoint web-sučelja (WFE). (Čimbenicima kao što su korištenje front-end opterećenja, trenutni radno opterećenje poslužitelja i tako dalje možete ograničiti ili uklonili ovaj prekidu). Da biste zaštitili korisnika iz programa prekidu, preporučujemo da omogućite ili onemogućite RBS samo tijekom planiranog održavanja prozora.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]
 

## <a name="configure-garbage-collection"></a>Konfiguriranje smeća

Kada se objekti se brišu s web-mjesta sustava SharePoint, oni se ne automatski brišu iz spremišta glasnoće RBS. Umjesto toga programa asinkronog, pozadine održavanja program briše bez nadređene blob-ova iz trgovine datoteka. Administratori sustava mogu zakazivati ovaj postupak da biste pokrenuli povremeno ili ga po potrebi mogu se pokrenuti.

Održavanje programa (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) automatski se instalira na svim poslužiteljima sustava SharePoint WFE i poslužitelji aplikacija Kada omogućite RBS. Program je instaliran na sljedećem mjestu: *pokretanje pogon*: \Programske udaljenog spremišta blobova SQL 10.50\Maintainer\

Informacije o konfiguriranju i pomoću programa za održavanje potražite u članku [Održavanje RBS u sustavu SharePoint Server 2013][8].

>[AZURE.IMPORTANT] RBS maintainer program je intenzivno resursa. Potrebno je planirati da bi se izvoditi samo tijekom razdoblja osnovnu aktivnosti na farmi sustava SharePoint.

### <a name="delete-orphaned-blobs-immediately"></a>Brisanje odmah napuštena blob-ova

Ako trebate izbrisati bez nadređene blob-ova odmah, možete koristiti sljedeće upute. Imajte na umu da ove upute primjera kako to možete učiniti u okruženju sustava SharePoint 2013 pomoću sljedeće komponente:

- Naziv baze podataka sadržaja je WSS_Content.
- SQL Server naziv je SHRPT13 SQL12\SHRPT13.
- Naziv web-aplikacije je SharePoint – 80.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]


## <a name="upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint"></a>Nadogradite ili ponovno instalirati StorSimple prilagodnik za SharePoint

Pomoću sljedećeg postupka nadogradnje sustava SharePoint server i zatim ponovno instalirajte StorSimple prilagodnik za SharePoint ili jednostavno nadograditi ili ponovno instalirati prilagodnik u postojećoj farmi poslužitelja sustava SharePoint. 

>[AZURE.IMPORTANT] Prije nego se pokušate nadograditi sustava SharePoint softver i/ili Nadogradnja ili ponovno instalirati StorSimple prilagodnik za SharePoint, pregledajte sljedeće informacije:
>
>- Sve datoteke koje ste prethodno premjestili vanjskog prostora za pohranu putem RBS neće biti dostupne sve dok ne završi ponovnom i ponovno je omogućena značajka RBS. Da biste ograničili učinak korisnika, izvršiti nadogradnju ni ponovne instalacije tijekom planiranog održavanja prozora.
>
>- Vrijeme potrebno za nadogradnju/ponovne instalacije možete ovisi o ukupan broj baza podataka sustava SharePoint u farmi sustava SharePoint server.
>
>- Po dovršetku nadogradnje/ponovne instalacije morate omogućiti RBS za baze podataka sadržaja. Dodatne informacije potražite u članku [Konfiguriranje RBS](#configure-rbs) .
>
>- Ako konfigurirate RBS za farme sustava SharePoint koje sadrži velikog broja baze podataka (veće od 200), stranica **Središnje administracije sustava SharePoint** može istek vremena. U tom slučaju osvježite stranicu. To ne utječe na proces konfiguracije.

[AZURE.INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]
 
## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple prilagodnik za uklanjanje sustava SharePoint

Sljedeći postupci opisuju kako povratak na blob-Ova baza podataka sadržaja za SQL Server, a zatim deinstalirati StorSimple prilagodnik za SharePoint. 

>[AZURE.IMPORTANT] Morate povratak na blob-Ova baza podataka sadržaja prije deinstalacije softver prilagodnika. 

### <a name="before-you-begin"></a>Prije početka 

Prije nego što vraćanje podataka sadržaja baze podataka sustava SQL Server i započnite postupak uklanjanja prilagodnik, prikupljanje sljedeće informacije:

- Nazivi sve baze podataka za koje je omogućen RBS
- UNC puta konfigurirani spremišta blobova PLATFORME

### <a name="move-the-blobs-back-to-the-content-databases"></a>Povratak na blob-Ova baza podataka sadržaja

Prije deinstalacije StorSimple prilagodnik za softver sustava SharePoint, morate sve blob polja koja su externalized migrirati natrag na sadržaj baze podataka sustava SQL Server. Ako pokušate deinstalirati StorSimple prilagodnik za SharePoint prije nego što ste povratak sve blob-Ova baza podataka sadržaja, vidjet ćete sljedeću poruku upozorenja.

![Poruka upozorenja](./media/storsimple-adapter-for-sharepoint/sasp1.png)
 
####<a name="to-move-the-blobs-back-to-the-content-databases"></a>Za povratak u BLOB-Ova baza podataka sadržaja 

1. Svaki externalized objekt preuzmite.

2. Otvorite stranicu **Središnje administracije sustava SharePoint** , a zatim idite na **Postavke sustava**. 

3. U odjeljku **Azure StorSimple**, kliknite **Konfiguriraj StorSimple prilagodnika**.

4. Na stranici **Konfiguracija prilagodnika StorSimple** , kliknite gumb **Onemogući** ispod svakog baza podataka sadržaja koji želite ukloniti iz vanjske blobova. 

5. Brisanje objekata iz sustava SharePoint, a zatim ih ponovno prenijeti.

Osim toga, možete koristiti Microsoft` RBS Migrate()` cmdlet ljuske PowerShell u sklopu sustava SharePoint. Dodatne informacije potražite u članku [migriranje sadržaja pojavljivanje ili iščezavanje RBS](https://technet.microsoft.com/library/ff628255.aspx).

Nakon što ste povratak na blob-Ova baza podataka sadržaja, prijeđite na sljedeći korak: [Deinstalacija prilagodnika](#uninstall-the-adapter).

### <a name="uninstall-the-adapter"></a>Deinstalacija prilagodnika

Nakon premještanja u BLOB-ova natrag da biste sadržaj baze podataka sustava SQL Server, koristite neku od sljedećih mogućnosti da biste deinstalirali StorSimple prilagodnik za SharePoint.

#### <a name="to-use-the-installation-program-to-uninstall-the-adapter"></a>Da biste koristili instalacijski program da biste deinstalirali prilagodnika 

1. Pomoću računa s ovlastima administratora da biste se prijavili web-sučelja (WFE) poslužitelja.
2. Dvokliknite StorSimple prilagodnik za instalacijski program sustava SharePoint. Pokreće se čarobnjak za postavljanje.

    ![Čarobnjak za postavljanje](./media/storsimple-adapter-for-sharepoint/sasp2.png)

3. Kliknite **Dalje**. Pojavljuje se sljedeća stranica.

    ![Uklanjanje stranica čarobnjaka za postavljanje](./media/storsimple-adapter-for-sharepoint/sasp3.png)

4. Kliknite **Ukloni** da biste odabrali postupak uklanjanja. Pojavljuje se sljedeća stranica.

    ![Potvrda stranica čarobnjaka za postavljanje](./media/storsimple-adapter-for-sharepoint/sasp4.png)

5. Kliknite **Ukloni** da biste potvrdili uklanjanje. Pojavit će se na sljedećoj stranici napredak.

    ![Stranica za praćenje napretka Čarobnjak za postavljanje](./media/storsimple-adapter-for-sharepoint/sasp5.png)

6. Po dovršetku uklanjanje pojavljuje se stranica Završi. Kliknite **Završi** da biste zatvorili čarobnjak za postavljanje.

#### <a name="to-use-the-control-panel-to-uninstall-the-adapter"></a>Da biste koristili upravljačku ploču da biste deinstalirali prilagodnika 

1. Otvorite upravljačku ploču, a zatim kliknite **Programi i značajke**.

2. Odaberite **StorSimple prilagodnik za SharePoint**, a zatim kliknite **Deinstaliraj**. 

## <a name="next-steps"></a>Daljnji koraci

[Dodatne informacije o StorSimple](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/en-us/library/ff943565.aspx
