<properties
   pageTitle="Pregled virtualne polja StorSimple | Microsoft Azure"
   description="U članku se opisuje virtualne StorSimple polja, rješenje za integriranu prostora za pohranu koja upravlja prostora za pohranu zadataka između lokalnog virtualnog uređaja i sustava Microsoft Azure za pohranu u oblaku."
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
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="introduction-to-the-storsimple-virtual-array"></a>Uvod u polju virtualne StorSimple

## <a name="overview"></a>Pregled

Dobro došli u Microsoft Azure StorSimple virtualne polja, rješenje za integrirane pohrane koja upravlja prostora za pohranu zadataka između lokalnog virtualne uređaju sa sustavom izvodi u hypervisor i za pohranu u oblaku Microsoft Azure. Virtualni polja (poznat i kao StorSimple lokalnog virtualne uređaj) je učinkovitog učinkovit i jednostavno moguće upravljati datotečnog poslužitelja ili rješenje iSCSI poslužitelja uklanja mnoge problema i troškove pridružene zaštita prostora za pohranu i podataka tvrtke. Virtualna polja je osobito dobro prikladniji za office remote/granu scenariji za office (ROBO).

U ovom pregled usredotočuje se na virtualne polja. 

- Da biste saznali kako niza StorSimple 8000, idite na [StorSimple 8000 niz: rješenje oblaka hibridnog](storsimple-overview.md). 

- Informacije o uređaju niz StorSimple 5000 7000 otvorite [StorSimple internetska pomoć](http://onlinehelp.storsimple.com/).

Virtualna polja podržava iSCSI ili protokol bloka poruka na poslužitelju (SMB). Pokreće na postojeću infrastrukturu hypervisor i omogućuje tiering oblaka, oblaka sigurnosnog kopiranja, brzo vraćanje, oporavak razini stavke i Izrada oporavak značajke.

U sljedećoj su tablici navedene su važne značajke virtualne polja.

| Značajka | Virtualna polja |
| ------- | ------------- |
|Preduvjeti za instalaciju | Koristi virtualizacije infrastrukture (Hyper-V ili VMware)|
| Dostupnost | Jedan čvor |
| Ukupni kapacitet (uključujući oblaka) |Do 64 kapacitet upotrebljivosti TB po virtualnog uređaja |
| Lokalni kapaciteta | 390 GB 6.4 TB upotrebljivosti kapacitet po virtualnog uređaja (potrebe Dodjela 500 GB 8 TB prostora na disku)|
| Izvorni protokola | iSCSI ili SMB |
| Oporavak vrijeme cilj (RTO) | iSCSI: manje od dvije minute bez obzira na to veličina |
| Oporavak točke cilj (RPO) | Dnevni sigurnosne kopije i sigurnosnih kopija na zahtjev |
| Prostor za pohranu tiering | Koristi topline mapiranje da biste odredili podataka koje je potrebno tiered ili smanjivanje |
| Podrška | Virtualizacija infrastrukture podržava dobavljača |
| Performanse | Ovisi o pozadini infrastrukture |
| Mobilnost podataka | Možete vratiti na istom uređaju ili razini stavke oporavak (datoteka server) |
| Prostor za pohranu razine | Lokalni hypervisor prostora za pohranu i oblaka |
| Zajedničko korištenje veličina |Tiered: do 20 TB; lokalno prikvačene: do 2 TB |
| Veličina jedinice |Tiered: do 5 Terabajta; lokalno prikvačene: najviše 500 GB |
| Brze snimke | Pad dosljedan |
| Oporavak na razini stavke | Da; korisnicima možete vratiti iz zajedničke stavke |

## <a name="why-use-storsimple"></a>Zašto koristiti StorSimple?

StorSimple povezuje korisnika i poslužitelji Azure prostora za pohranu u minutama bez promjena aplikacije.

U sljedećoj su tablici opisani su neki od ključne prednosti koje nudi rješenje virtualne polja.

| Značajka | Pogodnost |
|---------|---------|
| Prozirni Integracija | Virtualna polja podržava iSCSI ili SMB protokol. Premještanje podataka između lokalne sloju i sloju oblak je objedinjenog i prozirne korisniku.|
| Troškovi smanjene spremanja | S StorSimple, dodijelite dovoljno lokalno spremište da bi odgovarao trenutni zahtjeva za najčešće korištenih najnovije podatke. Kao što je prostora za pohranu potrebno rasta, StorSimple razine Hladna podatke učinkovit cloud prostora za pohranu. Podaci se deduplicated i sažeti prije slanja u oblak da biste dodatno smanjili preduvjeti za pohranu i troškovima.|
| Upravljanje pohranom pojednostavljeni | StorSimple nudi središnje upravljanje u oblak pomoću upravitelja StorSimple da biste upravljali više uređaja.| 
| Poboljšana Izrada oporavak i usklađenost | StorSimple olakšava brže Izrada oporavak odmah vraćanje metapodatke i vraćanje podatke prema potrebi. To znači da normalno operacije možete nastaviti s minimalnim prekidima.|
| Mobilnost podataka | Tiered podataka s oblakom možete pristupiti iz drugih web-mjesta u svrhu oporavak i migracije. Imajte na umu možete vratiti podatke samo na izvornom virtualne polja. Međutim, pomoću značajke oporavka Izrada da biste vratili cijelu virtualne polja drugi virtualne polja.|

## <a name="storsimple-workload-summary"></a>Radno opterećenje StorSimple sažetka

Sažetak podržani opterećenjem StorSimple je pozivu ispod.

| Scenarij                | Radno opterećenje              | Podržana |  Ograničenja                                  | Verzija              |
|-------------------------|-----------------------|-----------|------------------------------------------------|----------------------|
| ROBO suradnju      | Zajedničko korištenje datoteka          | Da       | Potražite u članku [Maksimalna ograničenja datotečnog poslužitelja](storsimple-ova-limits.md). <br>Potražite u članku [sistemski preduvjeti za podržane SMB verzije](storsimple-ova-system-requirements.md).   | Sve verzije      |


## <a name="workflows"></a>Tijekovi rada

Polja virtualne StorSimple je osobito prikladna za sljedeće tijekove rada:

- [Upravljanje pohrane u oblaku](#cloud-based-storage-management)
- [Mjesto neovisno sigurnosnog kopiranja](#location-independent-backup)
- [Oporavak podataka zaštitu i Izrada](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Upravljanje pohrane u oblaku

Servis Upravitelj StorSimple pokrenut na portalu za Azure klasični možete koristiti za upravljanje podacima koji se pohranjuju na više uređaja te u više mjesta. To je osobito je korisna u slučajevima raspodijeljeno grani. Imajte na umu da morate stvoriti zasebnu pojavljivanja StorSimple Upravitelj servisa za upravljanje virtualne polja i fizičkih StorSimple uređaja. 

![Upravljanje pohrane u oblaku](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Mjesto neovisno sigurnosnog kopiranja

Virtualna polju oblaka snimke navedite mjesto neovisno, točke u vrijeme kopiju glasnoću ili zajedničko korištenje. Brze snimke oblak omogućena je prema zadanim postavkama, a ne može se onemogućiti. Sve količine i zajedničko korištenje su sigurnosne kopije u isto vrijeme putem jednom dnevno sigurnosne kopije pravila, a koje možete poduzeti dodatne ad hoc kad god je potrebno sigurnosno kopiranje.

### <a name="data-protection-and-disaster-recovery"></a>Oporavak podataka zaštitu i Izrada

Virtualna polja podržava sljedeće podatke zaštitu i Izrada oporavak scenarije:

- **Vraćanje volumen ili zajedničko korištenje** – korištenje Vrati kao novi tijek rada za oporavak volumen ili zajedničko korištenje. Ovaj pristup koristite da biste oporavili čitave jedinice ili zajedničko korištenje.
- **Oporavak razini stavke** – dionice dopustite pojednostavljeni pristup zadnje sigurnosne kopije. Jednostavno možete vratiti pojedinačne datoteke iz mape posebno .backup dostupne u oblaku. Ova mogućnost vraćanja je utemeljenih na korisnika i potreban je bez intervencije administratora.
- **Izrada oporavak** – korištenje prebacivanje mogućnost da biste oporavili sve količine ili zajedničko korištenje da biste novi virtualne polja. Stvaranje novog virtualne polja i registrirati sa servisom StorSimple Upravitelj zatim neće uspjeti preko izvornog virtualne polja. Novi virtualne polja pa pretpostavlja da dodijeljenu resursi. 

## <a name="virtual-array-components"></a>Virtualna komponenti polja

Virtualna polja obuhvaća sljedeće komponente:

- [Virtualna polja](#virtual-array) – uređaj za pohranu oblaka hibridnog na temelju virtualnog računala dodjeli u virtualiziranom okruženju ili hypervisor.  
- [Servis upravitelja StorSimple](#storsimple-manager-service) – datotečni nastavak Azure klasični portal koje možete upravljati jedan ili više StorSimple putem sučelja Jednostruko web-mjesto koje možete pristupiti s različitim zemljopisnim područjima. Servis StorSimple Manager možete koristiti za stvaranje i upravljanje uslugama, prikaz i upravljanje uređajima i upozorenja i upravljanje količine, zajedničko korištenje i postojeće snimke.
- [Lokalni web korisničko sučelje](#local-web-user-interface) – koji se temelji na web Sučelja koja se koristi za konfiguriranje uređaj tako da ga možete povezati lokalne mreže, a zatim registrirali uređaj sa servisom StorSimple Manager. 
- [Sučelje naredbenog retka](#command-line-interface) – Windows PowerShell sučelja koje možete koristiti da biste započeli sesiju za podršku na virtualne polja.
U sljedećim se odjeljcima opisuju svaki od tih komponenti detaljniji i objašnjavaju kako rješenje slaže podataka dodjeljuje prostora za pohranu i olakšava zaštita podataka i upravljanje pohranom.

### <a name="virtual-array"></a>Virtualna polja

Virtualne polja je rješenje jedne čvor prostora za pohranu koji omogućuje pohranu primarni, upravlja komunikaciju s za pohranu u oblaku i pomaže da biste bili sigurni sigurnost i povjerljivost sve podatke koji je pohranjen na uređaju.

Virtualna polja dostupna u jedan model koji je dostupan za preuzimanje. Polja za pohranu ima najveći kapacitet 6.4 TB na uređaj (u podlozi za pohranu preduvjet 8 TB) i uključujući 64 TB prostora za pohranu u oblaku. 

Virtualna polja sadrži sljedeće značajke:

- Stvarni trošak je. To čini korištenje postojeću infrastrukturu virtualizacije i može se uvesti na postojeće hypervisor Hyper-V ili VMware.
- S podatkovnim centrom se nalazi i moguće je konfigurirati kao iSCSSI poslužiteljem ili datotečnog poslužitelja. 
- Je integriran u oblak.
- Sigurnosne kopije spremaju se u oblaku, što može olakšati oporavak Izrada i pojednostavili oporavak na razini stavke (ILR). 
- Možete primijeniti ažuriranja virtualne polja, baš kao i želite ih primijeniti na fizički uređaj.

>[AZURE.NOTE] Virtualna polja nije moguće proširiti. Stoga je važno Dodjela dovoljno prostora za pohranu prilikom stvaranja virtualnog uređaja. 

### <a name="storsimple-manager-service"></a>Servis upravitelja StorSimple

Microsoft Azure StorSimple nudi utemeljen na webu korisničko sučelje (servis upravitelja StorSimple) koji vam omogućuje središnje Upravljanje podatkovnim centrom i prostora za pohranu u oblaku. Servis StorSimple Manager možete koristiti za izvođenje sljedećih zadataka:

- Upravljanje više StorSimple virtualne polja iz jedne usluge. 
- Konfiguriranje i upravljanje sigurnosnim postavkama za uređaje StorSimple. (Šifriranje u oblaku je ovisna o Microsoft Azure API-ji).
- Konfiguriranje vjerodajnice za pohranu računa i svojstva.
- Konfiguriranje i upravljanje jedinicama ili zajedničko korištenje.
- Sigurnosno kopiranje i vraćanje podataka na web-mjesto količine ili u okvir za zajedničko korištenje.
- Praćenje performansi.
- Pregledajte postavke sustava i prepoznavanje mogućih problema.

Pomoću upravitelja StorSimple servisa izvođenje dnevnih administracije sustava virtualne polja.

Dodatne informacije, idite na članak [Korištenje StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Lokalni web korisničko sučelje

Virtualna polja sadrži koji se temelji na web Sučelja koji se koristi za jednokratni konfiguraciju i registracija uređaja sa servisom StorSimple Manager. Možete je koristiti da biste isključili i ponovno pokrenite virtualne polja, pokrenite dijagnostičkih testova, ažurirajte softver, promijenite lozinku administratora uređaja, zapisniku sustava i obratite se Microsoftovoj Support za zahtjev za uslugu. 

Informacije o korištenju korisničkog Sučelja utemeljen na webu, idite na članak [Korištenje utemeljen na webu korisničko Sučelje za administraciju sustava StorSimple virtualne polja](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Sučelje naredbenog retka

Uključeni sučelja komponente Windows PowerShell omogućuje vam da biste započeli sesiju podršku s Microsoft Support tako da se oni omogućuju pronalaženje i otklanjanje poteškoća koje biste mogli naići na uređaju virtualne.

## <a name="storage-management-technologies"></a>Prostor za pohranu upravljanje tehnologije

Osim virtualne polja i druge komponente, rješenje StorSimple koristi sljedeće tehnologije softver za brz pristup važnih podataka, smanjiti potrošnju prostora za pohranu i zaštititi podatke pohranjene na vašem virtualne polja:

- [Automatsko spremanje tiering](#automatic-storage-tiering) 
- [Lokalno prikvačene dionice i jedinice](#locally-pinned-shares-and-volumes)
- [Poništavanje duplikacije i sažimanje podataka tiered ili sigurnosne kopije u oblak](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
- [Sigurnosno kopiranje zakazano i na zahtjev](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Automatsko spremanje tiering

Virtualna polja koristi novi tiering mehanizam za upravljanje pohranjene podatke putem virtualne polja u oblak i. Postoje samo dvije razine: lokalne virtualne polja i Azure prostora za pohranu u oblaku. Polja virtualne StorSimple automatski slaže podataka u razine koji se temelji na kartu ekstrema koje prati trenutnog korištenja, dobi i odnose na druge podatke. Podaci koji su najaktivnije (sigurnije) pohranjuju lokalno, dok je manje podataka aktivna i neaktivne automatski migrirati u oblak. (Sve sigurnosne kopije pohranjuju u oblaku) StorSimple prilagođava i preslaguju podataka, a dodjele kao uzoraka korištenja i spremanje promjena. Na primjer, neke informacije može postati manje aktivni tijekom vremena. Kao ona postaje progresivno manje aktivna, on je tiered izgleda u oblak. Ako iste podatke ponovno postaje aktivna, on je tiered u polju za pohranu.

Podaci za određeni tiered zajedničko korištenje ili količine je zajamčiti vlastitu prostora na lokalnom sloju. (približno 10% od ukupne dodijeljenu prostora za te zajedničko korištenje ili količine). Dok se time se smanjuje dostupan prostor za pohranu na virtualnog uređaja za određenu zajedničko korištenje ili glasnoće, osigurava da tiering za jedan zajedničko korištenje ili glasnoću neće utjecati tako da tiering potrebe drugih dionice i količine. Stoga vrlo zauzet radno opterećenje na jednom zajedničko korištenje ili jedinica ne može prikazati sve druge radnih opterećenja u oblak. 

![Automatsko spremanje tiering](./media/storsimple-ova-overview/automatic-storage-tiering.png)

>[AZURE.NOTE] Možete navesti glasnoću lokalno prikvačene, u tom slučaju podaci ostaju na virtualne polja i nikad nije tiered u oblak. Dodatne informacije potražite na [lokalno prikvačene dionice i količine](#locally-pinned-shares-and-volumes).

### <a name="locally-pinned-shares-and-volumes"></a>Lokalno prikvačene dionice i jedinice

Možete stvoriti odgovarajući dionice i količine lokalno prikvačene. Ova mogućnost omogućuje čitanje podataka potrebnih ključnih aplikacije ostaje u virtualne polja, a nikada tiered u oblak. Lokalno prikvačene dionice i količine su sljedeće značajke: 

- Nisu primjenjuju oblaka latencies ili problema s povezivanjem.
- Oni i dalje prednosti StorSimple oblak i Izrada sigurnosne kopije značajke oporavka.

Možete vratiti lokalno prikvačene zajedničko korištenje ili glasnoću kao tiered ili tiered zajedničko korištenje ili prikvačene lokalno glasnoću. 

Dodatne informacije o lokalno prikvačene količine, idite na članak [Korištenje StorSimple Upravitelj servisa za upravljanje količine](storsimple-manage-volumes-u2.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Poništavanje duplikacije i sažimanje podataka tiered ili sigurnosne kopije u oblak

StorSimple koristi sažimanje Poništavanje duplikacije i podataka da biste dodatno smanjili preduvjeti za pohranu u oblaku. Poništavanje duplikacije smanjuje Ukupna količina podataka pohranjenih uklanjanjem zalihosti pohranjene skupa podataka. Informacije o promijeni, StorSimple zanemaruje nepromijenjen podataka i spremit će samo promjene. Osim toga, StorSimple smanjuje količinu podataka pohranjenih otkrivanje i uklanjanje dvostrukih podataka. 

>[AZURE.NOTE] Podatke pohranjene na virtualne polja je deduplicated ili komprimirane. Sve deduplication i sažimanje pojavljuje samo podaci se šalju u oblak.

### <a name="scheduled-and-on-demand-backups"></a>Sigurnosno kopiranje zakazano i na zahtjev

Značajke zaštite podataka StorSimple omogućuju stvaranje sigurnosne kopije na zahtjev. Uz to, zadani raspored za sigurnosne kopije omogućuje čitanje podataka se sigurnosno svaki dan. Sigurnosno kopiranje uzimaju se u obliku rastuće snimke koje se spremaju u oblaku. Snimke, da biste snimili samo promjene nakon zadnje sigurnosne kopije, možete stvoriti i brzo vratiti. Ove snimke može biti kritično važne u scenarijima za oporavak Izrada jer zamjena sustavi sekundarne prostora za pohranu (kao što su sigurnosnu kopiju na traci), a omogućuju vam da biste vratili podatke na podatkovnim centrom ili na svaki drugi web-mjesta ako je potrebno.

## <a name="next-steps"></a>Daljnji koraci

Saznajte kako [pripremiti portal virtualne polja](storsimple-ova-deploy1-portal-prep.md).


