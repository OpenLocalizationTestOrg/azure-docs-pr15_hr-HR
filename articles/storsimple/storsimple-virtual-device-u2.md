<properties 
   pageTitle="StorSimple virtualnog uređaja ažuriranju 2 | Microsoft Azure"
   description="Saznajte kako stvoriti, uvođenje i upravljanje StorSimple virtualnog uređaja u virtualne mreže Microsoft Azure. (Odnosi se na StorSimple ažuriranje 2)."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/23/2016"
   ms.author="alkohli" />

# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Uvođenje i upravljanje StorSimple virtualnog uređaja u Azure


##<a name="overview"></a>Pregled
Niz StorSimple 8000 virtualnog uređaja je dodatna mogućnost koja se dobiva sa sustava Microsoft Azure StorSimple rješenja. StorSimple virtualnog uređaja pokreće virtualnog računala u virtualne mreže Microsoft Azure, a možete je koristiti za stvaranje sigurnosne kopije i Kloniraj podataka iz vaše domaćini. Pomoću ovog praktičnog vodiča u članku se opisuje postupak implementacije i upravljanje virtualnog uređaja u Azure i nije primjenjivo na kojem je instalirana verzija programa ažuriranju 2 virtualne uređaje i donje.


#### <a name="virtual-device-model-comparison"></a>Usporedba modela virtualnog uređaja

StorSimple virtualnog uređaja dostupan je u dva modela, standardni 8010 (prijašnjeg na 1100) i premium 8020 (uvedena u ažuriranju 2). Usporedba dviju modela je pozivu ispod.


| Model uređaja          | 8010<sup>1</sup>                                                                     | 8020                                                                                                                               |
|-----------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Maksimalnog kapaciteta**      | 30 TB                                                                     | 64 TB                                                                                                                                |
| **Azure VM**              | Standard_A3 (4 jezgri, 7 GB memorije)                                                                      | Standard_DS3 (4 jezgri, 14 GB memorije)                                                                                                                          |
| **Kompatibilnost verzija** | Verzija izvodi unaprijed ažuriranje 2 ili novije verzije                                             | Verzija radi ažuriranja 2 ili novija verzija                                                                                                  |
| **Dostupnost regija**   | Sva područja koja Azure                                                         | Azure regije koje podržavaju pohranu Premium<br></br>Popis područja, potražite u članku [podržane područja za 8020](#supported-regions-for-8020) |
| **Vrsta prostora za pohranu**          | Koristi Azure standardne prostora za pohranu za lokalni disk<br></br> Saznajte kako [stvoriti račun standardne prostora za pohranu]() | Koristi Azure Premium prostora za pohranu za lokalni disk<sup>2</sup> <br></br>Saznajte kako [stvoriti račun Premium prostora za pohranu](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)                                                               |
| **Upute za radno opterećenje**     | Stavke razine dohvaćanje datoteka iz sigurnosnih kopija                                              | Razvojni u oblaku i testiranje scenariji, niske latencije, veća radnih opterećenja performansi <br></br>Sekundarni uređaja za oporavak Izrada                                                                                            |
 
<sup>1</sup> *prijašnji na 1100*.

<sup>2</sup> *u 8010 i 8020 pomoću Azure standardne prostora za pohranu za sloju oblaka. Razlika postoji samo u lokalnom sloju unutar uređaj*.

#### <a name="supported-regions-for-8020"></a>Podržanih regija za 8020

Prostor za pohranu Premium regije koje podržani su za 8020 pozivu su ispod. Ovaj popis ažurirat će se neprestano kao što je prostor za pohranu Premium postaje dostupan u više područja. 

| S. ne.                                                  | Trenutno podržava regije |
|---------------------------------------------------------|--------------------------------|
| 1                                                       | Središnje SAD-a                     |
| 2                                                       |  Istočni SAD-a                       |
| 3                                                       |  Istočni sad 2                     |
| 4                                                       | Zapad SAD-a                        |
| 5                                                       | Sjeverna Europa                   |
| 6                                                       | Europa Zapad                    |
| 7                                                       | Jugoistočne Azije                 |
| 8                                                       | Istok Japan                     |
| 9                                                       | Japan Zapad                     |
| 10                                                      | Istok Australija                 |
| 11                                                      | Australija Jugoistok *           |
| 12                                                      | Istočnoazijski *                     |
| 13                                                      | Južna središnje sad *              |

* Prostor za pohranu premium pokrenut nedavno u te geos.

U ovom se članku opisuju detaljan postupak implementacije StorSimple virtualnog uređaja u Azure. Kad pročitate članak ćete:

- Objašnjenje kako virtualnog uređaja razlikuje se od fizički uređaj.

- Moći stvaranje i konfiguriranje virtualnog uređaja.

- Povezivanje s virtualnog uređaja.

- Saznajte kako raditi s virtualnog uređaja.

Pomoću ovog praktičnog vodiča primjenjuje se na sve StorSimple virtualne uređaje pokreće ažuriranje 2 ili noviji. 

## <a name="how-the-virtual-device-differs-from-the-physical-device"></a>Kako virtualnog uređaja razlikuje se od fizički uređaj

StorSimple virtualnog uređaja je samo za softver verzija StorSimple koja se izvršava na jedan čvor u u programu Microsoft Azure virtualnog računala. Virtualnog uređaja podržava Izrada oporavak scenariji u kojima fizički uređaj nije dostupan, i odgovarajuće za korištenje na razini stavke dohvaćanja iz sigurnosne kopije, lokalni Izrada oporavak i scenariji razvojni i testiranje oblaka.

#### <a name="differences-from-the-physical-device"></a>Razlike iz fizički uređaj

Sljedeća tablica prikazuje neke ključne razlike između StorSimple virtualnog uređaja i StorSimple fizički uređaj.

|                             | Fizički uređaj                                          | Virtualnog uređaja                                                                            |
|-----------------------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Mjesto**                    | Nalazi se u s podatkovnim centrom.                               | Pokreće se u Azure.                                                                            |
| **Sučelje mreže**          | Sadrži šest sučelje mreže: podataka od 0 do 5 podataka.                  | Sadrži samo jedan mrežnog sučelja: podataka 0.                                        |
| **Registracija**                | Registrirana tijekom koraka konfiguracije.                | Registracija je zasebnom zadatka.                                                          |
| **Ključ za šifriranje podataka usluge** | Obnovi na fizički uređaj, a zatim ažurirajte virtualnog uređaja novog ključa.           | Nije moguće Obnovi putem virtualne uređaja. |


## <a name="prerequisites-for-the-virtual-device"></a>Preduvjeti za virtualnog uređaja

U sljedećim odjeljcima objašnjavaju konfiguracije preduvjeti za StorSimple virtualnog uređaja. Prije no što implementirate virtualnog uređaja, pregledajte [Sigurnosna pitanja vezana uz za korištenje virtualnog uređaja](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Preduvjeti za Azure

Prije dodjele resursa virtualnog uređaja koje morate donijeti sljedeće pripreme u okruženju sustava Azure:

- Virtualna uređaja, [Konfiguriranje virtualne mreže na Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Ako koristite za pohranu Premium, morate stvoriti virtualne mreže Azure regija koji podržava Premium prostora za pohranu. Dodatne informacije o [regije koje trenutno podržava za 8020](#supported-regions-for-8020).
- Preporučuje se da biste koristili zadani poslužitelj DNS-a nudi Azure umjesto navodeći vlastiti naziv DNS poslužitelj. Ako naziv poslužitelja za DNS-a nije valjan ili DNS poslužitelj nije ispravno riješiti IP adrese, stvaranje virtualnog uređaja neće uspjeti.
- Točka web i web-mjesto su obavezno, ali nije obavezno. Ako želite, možete konfigurirati sljedeće mogućnosti za naprednije scenariji. 
- Možete stvoriti [virtualnim računalima sustava Azure](../virtual-machines/virtual-machines-linux-about.md) (glavno računalo poslužitelji) u virtualne mreže koje mogu koristiti količine koji prikazuje virtualnog uređaja. Sljedećim poslužiteljima mora zadovoljavati sljedeće uvjete:                            
    - Biti Windows ili Linux VMs s iSCSI pokretač softver.
    - Imati u istom virtualne mreže kao virtualnog uređaja.
    - Moći povezati cilj iSCSI virtualnog uređaja putem interne IP adresu virtualnog uređaja.

- Provjerite je li ste konfigurirali podrška za iSCSI i oblaka promet na istom virtualne mreže.


#### <a name="storsimple-requirements"></a>Preduvjeti za StorSimple

Provjerite sljedeće ažuriranja u funkcioniranju servisa Azure StorSimple prije stvaranja virtualnog uređaja:


- Dodavanje [zapisa za kontrolu pristupa](storsimple-manage-acrs.md) za VMs koji su će biti glavnog računala poslužitelja za virtualnog uređaja.

- Pomoću [računa za pohranu](storsimple-manage-storage-accounts.md#add-a-storage-account) u području isti kao virtualnog uređaja. Računi za pohranu u različitim područjima može uzrokovati slabe performanse. Možete koristiti standardni prikaz ili prostora za pohranu Premium račun s virtualnog uređaja. Dodatne informacije o stvaranju [standardne prostora za pohranu račun]() ili [račun za pohranu Premium](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)

- Koristiti različite prostora za pohranu račun za stvaranje virtualnog uređaja od onog koji se koristi za vaše podatke. Koristeći isti prostor za pohranu račun može uzrokovati slabe performanse.

Pripazite da imate sljedeće informacije prije nego što počnete:

- Azure klasični portala račun pomoću vjerodajnica programa access.

- Kopija ključa za šifriranje podataka servisa s fizičke uređaja.


## <a name="create-and-configure-the-virtual-device"></a>Stvaranje i konfiguriranje virtualnog uređaja

Prije izvođenja te postupke, provjerite je li ispunjenim [preduvjeti za virtualnog uređaja](#prerequisites-for-the-virtual-device). 

Nakon što ste stvorili virtualne mreže, konfiguriran StorSimple Upravitelj servisa i registrirana uređaj fizički StorSimple sa servisom, koristite sljedeće korake za stvaranje i konfiguriranje StorSimple virtualnog uređaja. 

### <a name="step-1-create-a-virtual-device"></a>Korak 1: Stvaranje virtualnog uređaja

Izvršite sljedeće korake da biste stvorili StorSimple virtualnog uređaja.

[AZURE.INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Stvaranje virtualnog uređaja ne uspije u ovom ćete koraku, možda nemate povezani s Internetom. Dodatne informacije potražite na [otkloniti poteškoće s povezivanjem Internet s](#troubleshoot-internet-connectivity-errors) kada creatig virtualnog uređaja.


### <a name="step-2-configure-and-register-the-virtual-device"></a>Korak 2: Konfigurirati i registrirati virtualnog uređaja

Prije početka ovog postupka, provjerite imate li kopiju ključa za šifriranje podataka servisa. Servis ključa za šifriranje podataka stvorena je kada ste konfigurirali prvi uređaj za StorSimple i su upute da biste spremili na sigurnom mjestu. Ako nemate kopiju ključa za šifriranje servisa podataka, morate se obratiti Microsoft Support za pomoć.

Izvršite sljedeće korake možete konfigurirati i registrirati StorSimple virtualnog uređaja.
[AZURE.INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Korak 3: (Neobavezno) Izmijeni konfiguracijske postavke uređaja

Sljedeći odjeljak opisuje konfiguracijske postavke uređaja potrebne za na StorSimple virtualnog uređaja ako želite koristiti CHAP StorSimple snimku Upravitelj ili promijenite lozinku administratora uređaja.

#### <a name="configure-the-chap-initiator"></a>Konfiguriranje pokretač CHAP

Ovaj parametar sadrži vjerodajnice koje očekuje virtualnog uređaja (cilj) iz Pokretači (poslužitelji) pristupate jedinice. Pronaći ćete u Pokretači CHAP korisničko ime i lozinku CHAP za prepoznavanje same uređaju tijekom ovu provjeru autentičnosti. Detaljne korake potražite za [Konfiguriranje CHAP za svoj uređaj](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-the-chap-target"></a>Konfiguriranje ciljne CHAP

Ovaj parametar sadrži virtualnog uređaja koristi kad s omogućenim CHAP pokretač Zatraži provjeru autentičnosti međusobna ili dvosmjerni vjerodajnice. Virtualnog uređaja koristit će obrnuti CHAP korisničko ime i lozinku obrnuti CHAP za prepoznavanje sam pokretač tijekom postupka provjere autentičnosti. Imajte na umu da su postavke ciljne CHAP globalnih postavki. Kada te se primjenjuju sve količine povezanih s virtualne uređaj za pohranu će koristiti CHAP provjere autentičnosti. Detaljne korake potražite za [Konfiguriranje CHAP za svoj uređaj](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>Konfiguriranje StorSimple snimke Upravitelj lozinke

Softver StorSimple snimke Upravitelj nalazi na Windows host i administratorima omogućuje upravljanje sigurnosne kopije uređaju StorSimple u obliku lokalno i brze snimke u oblaku.

>[AZURE.NOTE] Za virtualnog uređaja Windows host je Azure virtualnog računala.

Prilikom konfiguriranja uređaja u upravitelju snimka StorSimple, zatražit će se StorSimple uređaj IP adresa i lozinka za provjeru autentičnosti uređaj za pohranu. Detaljne upute, idite na [Konfiguriranje Upravitelj snimka StorSimple lozinku](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

#### <a name="change-the-device-administrator-password"></a>Promjena lozinke administratora uređaja 

Prilikom korištenja sučelja komponente Windows PowerShell za pristup virtualnog uređaja, će vas zatraži da unesete administratorsku lozinku za uređaj. Sigurnost vaših podataka, koji su potrebni da biste promijenili lozinku prije korištenja virtualnog uređaja. Detaljne korake potražite [Konfiguriraj uređaj administratorsku lozinku](storsimple-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-to-the-virtual-device"></a>Daljinsko povezivanje s virtualnog uređaja
Po zadanom je omogućena daljinski pristup s uređajem virtualne putem sučelja komponente Windows PowerShell. Morate najprije Omogućivanje daljinskog upravljanja na virtualnog uređaja, a zatim je omogućiti na klijentskom računalu koja će se koristiti za pristup virtualnog uređaja.

Dva koraka za povezivanje daljinski je detaljan ispod.

### <a name="step-1-configure-remote-management"></a>Korak 1: Konfiguriranje daljinskog upravljanja

Izvršite sljedeće korake da biste konfigurirali daljinsko upravljanje za svoj uređaj virtualne StorSimple.

[AZURE.INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-virtual-device"></a>Korak 2: Udaljeni pristup virtualnog uređaja

Nakon što ste omogućili daljinskog upravljanja na stranicu za konfiguriranje StorSimple uređaja, možete koristiti remoting komponente Windows PowerShell za povezivanje s virtualnog uređaja s drugog računala virtualne unutar iste virtualne mreže; na primjer, možete se povezati s računala koje hostira VM koji je konfiguriran, a služe za povezivanje iSCSI. U većini implementacijama će ste već otvorili javno krajnja točka za pristup VM koje možete koristiti za pristup virtualnog uređaja glavnog računala.

>[AZURE.WARNING] **Radi bolje sigurnosti, preporučujemo da koristite HTTPS prilikom povezivanja s krajnjih točaka, a zatim izbrišite krajnjih točaka nakon što dovršite udaljenu sesiju ljuske PowerShell.**

Slijedite postupke u [Povezivanje daljinski s uređajem StorSimple](storsimple-remote-connect.md) da biste postavili remoting za virtualnog uređaja.

## <a name="connect-directly-to-the-virtual-device"></a>Priključite izravno na virtualnog uređaja

Možete povezati i izravno u virtualnog uređaja. Ako se želite povezati izravno virtualnog uređaja s nekog drugog računala izvan virtualne mreže ili izvan okruženje sustava Microsoft Azure potrebnih za stvaranje dodatnih krajnje točke, kao što je opisano u nastavku. 

Izvršite sljedeće korake da biste stvorili javno krajnje točke na virtualnog uređaja.

[AZURE.INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Preporučujemo da povežete s drugog računala virtualne unutar iste virtualne mreže jer praksa smanjuje broj javno krajnje točke na virtualne mreže. Kada koristite ovaj postupak, jednostavno povežete s virtualnog računala putem sesiji udaljene radne površine i konfiguriranje tog virtualnog računala za korištenje, kao i drugi klijent sustava Windows u lokalnoj mreži. Ne morate dodati broj priključka javno jer priključak već poznati.

## <a name="work-with-the-storsimple-virtual-device"></a>Rad s StorSimple virtualnog uređaja

Sad kad ste stvorili i konfigurirali StorSimple virtualnog uređaja, spremni ste za početak rada s njom. Možete raditi s spremnika glasnoće, količine i sigurnosne kopije pravila na virtualnog uređaja kao što biste to učinili na fizički uređaj StorSimple; jedina razlika je potrebno da biste provjerili je li na popisu uređaja odaberite virtualne uređaj. Potražite [Upravitelj StorSimple servis za upravljanje virtualnog uređaja](storsimple-manager-service-administration.md) za detaljne postupke različitih zadataka upravljanja za virtualnog uređaja.

U sljedećim se odjeljcima navode neke od razlika naići ćete pri radu s virtualnog uređaja.

### <a name="maintain-a-storsimple-virtual-device"></a>Održavanje StorSimple virtualnog uređaja

Budući da je riječ o uređaju samo za softver, održavanje virtualnog uređaja je minimalna pri usporedbi održavanje fizički uređaj. Imate sljedeće mogućnosti:

- **Softverska ažuriranja** – možete vidjeti datum koji softver zadnjeg ažuriranja, zajedno s bilo ažuriranje poruke o statusu. Gumb **Pregled ažuriranja** pri dnu stranice možete koristiti da biste izvršili ručno pregleda ako želite da biste provjerili ima li novih ažuriranja. Ako su dostupna ažuriranja, kliknite **Instaliranje ažuriranja** za instalaciju. Jer je samo jedan sučelja na virtualnog uređaja, to znači da će se pružanju napraviti manje usluge kad su primijenjeni ažuriranja. Virtualnog uređaja će isključiti i ponovno pokrenite (Ako je potrebno) da biste primijenili promjene objavljene. Detaljni opis postupka, otvorite da biste [ažurirali uređaj](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
- **Podržava paket** – možete stvoriti i prenijeti paket za podršku da biste lakše Microsoft Support rješavanje problema s uređajem virtualne. Detaljni opis postupka potražite za [Stvaranje i upravljanje paket za podršku](storsimple-create-manage-support-package.md).

### <a name="storage-accounts-for-a-virtual-device"></a>Računi za pohranu za virtualnog uređaja

Računi za pohranu se stvaraju za korištenje usluga StorSimple upravitelj, virtualnog uređaja i fizički uređaj. Kada stvorite račune za pohranu, preporučujemo da koristite identifikatora regija u neslužbeni naziv da biste bili sigurni da je područje riječima svih komponenti sustava. Za virtualnog uređaja je važno sve komponente se na istom području da biste spriječili probleme s performansama.

Detaljni opis postupka potražite u članku [Dodavanje računa za pohranu](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>Deaktiviranje StorSimple virtualnog uređaja

Deaktivacija virtualnog uređaja briše se VM i resursima stvara kada ga je dodijeljena. Kada je deaktiviran virtualnog uređaja, nije moguće vratiti u prethodno stanje. Prije nego što deaktivirate virtualnog uređaja, provjerite je li da biste zaustavili ili izbrisali klijenti i glavnih računala koje ovise o njemu.

Deaktivacija virtualnog uređaja rezultira sljedeće radnje:

- Uklanja se virtualnog uređaja.

- OS disk i diskova podataka za virtualnog uređaja stvara se uklanjaju.

- Servis i virtualne mreže tijekom dodjele resursa stvara se zadržavaju. Ako se ne koriste ih, trebali biste ih izbrisati ručno.

- Snimke oblaka za virtualnog uređaja stvara se zadržavaju.

Detaljni opis postupka potražite u odjeljku [deaktiviranje i brisanje uređaja StorSimple](storsimple-deactivate-and-delete-device.md).

Čim virtualnog uređaja se prikazuje kao što je deaktiviran na stranici upravitelja StorSimple servisa, možete izbrisati virtualnog uređaja s popisom uređaja na stranici **uređaja** .


### <a name="start-stop-and-restart-a-virtual-device"></a>Pokretanje, zaustavljanje i ponovno pokrenite virtualnog uređaja
Za razliku od uređaj fizički StorSimple postoji nema power uključeno ili isključeno gumb da biste na uređaju virtualne StorSimple potencija. Međutim, možda postoje prilike tamo gdje vam je potreban da biste prekinuli i ponovno pokrenite virtualnog uređaja. Ako, na primjer, neka ažuriranja možda tražiti da biste dovršili postupak ažuriranja ponovno pokrenuti na VM. Za pokretanje, tabulatora i ponovno pokrenite virtualnog uređaja najjednostavnije konzole za upravljanje virtualnim računalima.

Kada pogledate konzolu za upravljanje stanje virtualnog uređaja je **pokrenut** jer je pokrenut prema zadanim postavkama nakon stvaranja. Možete pokrenuti, zaustaviti i ponovno pokrenite virtualnog računala u bilo kojem trenutku.

[AZURE.INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-to-factory-defaults"></a>Vraćanje na tvorničke zadane postavke

Ako odlučite da želite da biste započeli s virtualnog uređaja, jednostavno je deaktivirajte i izbrišite je i zatim stvorite novi. Baš kao i kada je ponovno fizički uređaj, novi virtualne uređaj neće imati instaliran; ažuriranja Stoga svakako Provjeri ima li ažuriranja prije korištenja.


## <a name="fail-over-to-the-virtual-device"></a>Nije uspjelo putem virtualne uređaja

Izrada oporavak (DR) jedan je od ključni Scenariji koji je namijenjen StorSimple virtualnog uređaja. U ovom scenariju fizički StorSimple uređaj ili cijele podatkovnog centra možda neće biti dostupne. Srećom, virtualnog uređaja možete koristiti da biste vratili operacije na drugo mjesto. Tijekom DR, spremnika glasnoću s uređaja izvora promijenite vlasništvo i prenose se na virtualnog uređaja. Preduvjeti za DR su da stvorili i konfigurirali virtualnog uređaja, jedinicama u spremniku glasnoću ste je izvanmrežno i spremnik glasnoću sadrži pridružene oblaka snimke.

>[AZURE.NOTE] 
>
> - Kada koristite virtualnog uređaja kao sekundarni uređaj za DR, imajte na umu da u 8010 je 30 TB prostora za pohranu na standardni i 8020 sadrži 64 TB prostora za pohranu Premium. Veći kapacitet 8020 virtualne uređaj možda više prikladniji za scenarij DR.
> - Nije moguće prebacivanje ili Kloniraj s uređaja sa sustavom ažuriranju 2 s uređaja sa sustavom stara Update 1 softver. No uspijeva putem uređaja sa sustavom ažuriranju 2 s uređaja sa sustavom Update 1 (1.1 ili 1.2)

Detaljni opis postupka, idite na [Prebacivanje virtualne uređaj](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).
 

## <a name="shut-down-or-delete-the-virtual-device"></a>Isključivanje ili brisanje virtualnog uređaja

Ako ste prethodno konfigurirali i koristiti StorSimple virtualnog uređaja ali sada želite prekinuti accruing računalnim naknade za korištenje, možete isključiti virtualnog uređaja. Isključuje virtualnog uređaja ne briše njegov operacijski sustav ili diskova podataka u prostor za pohranu. Ga zaustavili naknade accruing pretplate na, ali će i dalje naknade za pohranu za diskova OS i podatke.

Ako izbrišete ili isključiti virtualnog uređaja, to prikazat će se kao **izvanmrežno** na stranici uređaja StorSimple Upravitelj servisa. Možete odabrati deaktiviranje i brisanje uređaja ako želite izbrisati sigurnosne kopije stvorio virtualnog uređaja. Dodatne informacije potražite u članku [deaktiviranje i brisanje StorSimple uređaja](storsimple-deactivate-and-delete-device.md).

[AZURE.INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[AZURE.INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

   
## <a name="troubleshoot-internet-connectivity-errors"></a>Otklonite pogreške prilikom povezivanje s Internetom 

Tijekom stvaranja virtualnog uređaja, ako postoji bez povezivanja s Internetom korak stvaranja neće uspjeti. Da biste otklonili ako je neuspjeh zbog internetske veze, na portalu za Azure klasični poduzeti sljedeće korake:

1. Stvaranje virtualnog računala za Windows server 2012 u Azure. U ovom virtualnog računala trebali biste koristiti isti prostor za pohranu račun, VNet i podmreže kao koristi virtualnog uređaja. Ako već imate postojeće Windows Server glavno računalo u Azure koristi isti prostor za pohranu računa, Vnet i podmreže, također ga možete koristiti otklanjanje poteškoća s internetskom vezom.
2. Udaljena Prijava na virtualnog računala stvorili u prethodnom koraku. 
3. Otvorite prozor naredbenog unutar virtualnog računala (Win + R, a zatim upišite `cmd`).
4. Kada se zatraži, pokrenite sljedeće naredbe.

    `nslookup windows.net`

5. Ako `nslookup` ne uspije, zatim nije uspjelo povezivanje Internet virtualnog uređaja onemogućuje Registracija StorSimple Upravitelj servisa. 
6. Unesite potrebne promjene da biste virtualne mrežu da biste bili sigurni da je virtualnog uređaja možete pristupiti Azure web-mjesta kao što su "windows.net".

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [pomoću upravitelja StorSimple servisa za upravljanje virtualnog uređaja](storsimple-manager-service-administration.md).
 
- Objašnjenje kako [vratiti StorSimple jedinice iz sigurnosne kopije skupa](storsimple-restore-from-backup-set.md). 

