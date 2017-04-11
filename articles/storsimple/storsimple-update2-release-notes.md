<properties 
   pageTitle="Napomene u StorSimple 8000 niz ažuriranju 2 | Microsoft Azure"
   description="Opisuje novih značajki, probleme i zaobilazna rješenja za StorSimple 8000 niz ažuriranju 2."
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
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-2-release-notes"></a>Napomene u StorSimple 8000 niz ažuriranju 2  

## <a name="overview"></a>Pregled

Sljedeće napomene opisuju nove značajke i prepoznati kritičnih problema Otvori StorSimple 8000 niz ažuriranju 2. Koje sadrže i popis StorSimple softver, upravljački program i disk koje ovo izdanje obuhvaća ažuriranja za opreme. 

Ažuriranje 2 primjenjuje se na bilo kojem uređaju StorSimple izdanje (GA) ili ažuriranje 0,1 primjene Update 1.2. Verzija uređaja koji je povezan s ažuriranju 2 je 6.3.9600.17673.

Pregledajte informacije koje se nalaze u napomenama u prije implementacije ažuriranja u rješenje StorSimple.

>[AZURE.IMPORTANT]
> 
- Traje približno 4-7 sata za instaliranje tog ažuriranja (uključujući ažuriranja za Windows). 
- Ažuriranje 2 sadrži softver, upravljački program za LSI i SSD ažuriranja opreme.
- Za novi izdanja, možda nećete vidjeti ažuriranja odmah jer moramo fazama uvođenje ažuriranja. Pričekajte nekoliko dana, a zatim traženje ažuriranja ponovno kao one će biti dostupna uskoro.


## <a name="whats-new-in-update-2"></a>Što je novo u ažuriranju 2

Ažuriranje 2 predstavlja sljedeće nove značajke.

- **Lokalno prikvačene količine** – u prethodnim verzijama StorSimple 8000 niz blokova podataka nisu tiered u oblak na temelju upotrebe. Došlo je nije moguće jamči da blokira će ostati na lokalno. U ažuriranju 2, prilikom stvaranja jedinice, možete odrediti jedinice kao lokalno prikvačeni i primarni podatke s tog jedinice će biti tiered u oblak. Snimke lokalno prikvačene jedinicama kopirat će se i dalje u oblak za sigurnosno kopiranje tako da u oblak koje se mogu koristiti za podataka mobilnost i Izrada oporavak. Osim toga, možete promijeniti vrstu glasnoću (to jest, Pretvori tiered količine lokalno prikvačene jedinicama i količine Pretvori lokalno prikvačena na tiered). 

- **Poboljšanja virtualnog uređaja StorSimple** – prethodno, niz StorSimple 8000 smješten virtualnog uređaja Izrada oporavka ili razvoj i testiranje rješenja. Došlo je samo jedan model virtualnog uređaja (modela 1100). Ažuriranje 2 predstavlja dva virtualnog uređaja modela: 

     - 8010 (prije se zvao na 1100) – bez promjena ima kapacitet 30 TB i koristi Azure standardne prostora za pohranu.
     - 8020 – ima kapacitet 64 TB i koristi Azure Premium prostora za pohranu za bolje performanse.

    Postoji jedan VHD za oba modele virtualnog uređaja (8010 na 8020). Prilikom prvog pokretanja virtualnog uređaja, otkriva parametara platforme i primjenjuje verzija točan modela.

- **Poboljšanja umrežavanje** – ažuriranje 2 sadrži sljedeća poboljšanja umrežavanje:

     - NIC-ovi više je moguće omogućiti za oblak tako da se prebacivanje se može dogoditi ako ne uspije u NIC.
     - Usmjeravanje poboljšanja, s fiksnim metriku oblaka omogućen blokovi.
     - Online pokušaj nije uspio resursa prije nego što je prebacivanje.
     - Nova upozorenja za servis pogreške.

- **Ažuriranje poboljšanja** – ažuriranje 1.2 i noviji, niz StorSimple 8000 ažurirana putem dva kanala: Klasteriranje, iSCSI, i tako dalje i Microsoft Update za binarne datoteke i opreme servisa Windows Update.
    Ažuriranje 2 koristi Microsoft Update za sve pakete ažuriranja. To treba dovesti do kraće zakrpa ili način failovers. 

- **Ažuriranja opreme** – sljedeće uključene su ažuriranja opreme:
    - LSI: lsi_sas2.sys verziju proizvoda 2.00.72.10
    - SSD samo (bez ažuriranja podizanje tvrdog diska): XMGG, XGEG, KZ50, F6C2 i VR08

- **Podrška za određene proaktivne** – ažuriranje 2 Microsoftu povući dodatna dijagnostičke informacije s uređaja. Kada naš tim za operacije prepozna uređaje koji se pojave problemi, nismo bolje posebnog za prikupljanje informacija s uređaja i otklanjanja poteškoća. **Tako da prihvaća ažuriranju 2 dopustite nam ovaj određene proaktivne podršku**.    
 

## <a name="issues-fixed-in-update-2"></a>Problemi s fiksiran ažuriranju 2

Sljedeća tablica sadrži sažetak problema koje je fiksnih ažuriranja 2.    

| ne. | Značajka | Problem | Odnosi se na fizički uređaj | Odnosi se na virtualnog uređaja |
|-----|---------|-------|--------------------------------|--------------------------------|
| 1 | Sučelje mreže | Nakon nadogradnje na Update 1, servis za upravitelja StorSimple je da na jedan kontroler nije uspjela priključke Data2 i Data3. Taj se problem riješen. | Da | ne |
| 2 | Ažuriranja | Nakon nadogradnje na Update 1, na portalu Azure klasični na više uređaja došlo je do alarma zvučna upozorenja. Taj se problem riješen. | Da | ne |
| 3 | Provjera autentičnosti Openstack | Prilikom korištenja Openstack kao vaš davatelj usluga oblaka, nije moguće o pogrešci da je niz za provjeru autentičnosti oblaka predug. To je riješen. | Da | ne |


## <a name="known-issues-in-update-2"></a>Poznati problemi u ažuriranju 2

Sljedeća tablica daje sažetak Poznati problemi u ovom izdanju.

| ne. | Značajka | Problem | Komentari / zaobilazno rješenje | Odnosi se na fizički uređaj | Odnosi se na virtualnog uređaja |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Kvorum disk | U rijetko slučajevima, ako su povezani Većina diskova s prilozima EBOD 8600 uređaja rezultira kvorum bez diska pa na resurse za pohranu će raditi izvanmrežno. On će ostati izvanmrežne čak i ako na diskova se ponovno povezati sa sustavom. | Morat ćete ponovno pokrenite uređaj. Ako se problem nastavi pojavljivati, obratite se Microsoft Support za sljedeće korake. | Da | ne |
| 2 | ID netočan kontrolera | Zamjena kontroler provodi, kontroler 0 možda prikazuju se kao kontroler 1. Tijekom zamjenski kontroler prilikom učitavanja slike iz čvor ravnopravnih članova ID kontrolera može prikazivati na početku kao kontrolerom ravnopravnih članova ID-a. U rijetko instancama takvo ponašanje mogu se vidjeti i nakon ponovnog pokretanja sustava. | Potreban je bez korisničku akciju. U ovom slučaju će riješiti sam nakon zamjenski kontroler. | Da | ne |
| 3 | Računi za pohranu | Putem servisa za pohranu da biste izbrisali račun za pohranu je nepodržane scenarij. To će dovesti do situacije u kojima ne može dohvatiti podatke o korisniku.|  | Da | Da |
| 4 | Prebacivanje na uređaju | Više failovers glasnoću spremnika na istom uređaju izvora različite ciljnih nije podržana. Prebacivanje s jednog reagira uređaja na više uređaja će postati spremnika glasnoće na prvom nije uspjela putem uređaja izgubiti vlasništvo nad podacima. Nakon takve prebacivanje te spremnika glasnoću će prikazuju se ili se drugačije ponašaju prilikom prikaza na portalu za Azure klasični. | | Da | ne |
| 5 | Instalacija | Tijekom StorSimple prilagodnik za instalaciju sustava SharePoint, morate unijeti IP uređaj za instalaciju da biste uspješno završili.    | | Da | ne |
| 6 | Web proxy | Ako je vaše web-konfiguracija proxy HTTPS kao navedeni protokol, utjecat će vaše komunikacije servisa uređaja i će izvanmrežni uređaj. Podrška za pakete i moguće generirati u postupku troše značajan resursa na uređaju. | Provjerite ima li web-URL proxy HTTP kao navedeni protokol. Dodatne informacije, idite na [konfiguracija web proxy za svoj uređaj](storsimple-configure-web-proxy.md). | Da | ne |
| 7 | Web proxy | Ako konfigurirati i omogućite web proxy na uređaju registriranog, zatim morat ćete ponovno pokrenite active kontroler na uređaju. | | Da | ne |
| 8 | Latencija visoke oblaka i visoke/i radno opterećenje | Kada naiđe na uređaju StorSimple kombinaciju vrlo visoka oblaka latencies (redoslijeda sekundi) i visoke/i radno opterećenje, količine uređaj idite u su smanjene stanje i na I/Os možda neće funkcionirati uz poruku o pogrešci "uređaj nije spreman". | Morat ćete ručno ponovno kontrolera uređaja ili ponovno prebacivanje uređaj da biste oporavili u tom slučaju. | Da | ne |
| 9 | Azure PowerShell | Kada koristite cmdlet StorSimple **Get-AzureStorSimpleStorageAccountCredential & #124; Odaberite objekt - najprije 1 - čekanja** da biste odabrali prvi objekt tako da možete stvoriti novi objekt **VolumeContainer** , cmdlet vraća sve objekte. | Prelamanje cmdlet u zagradama na sljedeći način: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Odaberite objekt - najprije 1 - čekanja** | Da | Da |
| 10| Migracije | Kada više spremnika glasnoću dodjeljuju se za migraciju, ETA za zadnje sigurnosne kopije je točne samo za prvome spremniku glasnoću. Uz to, paralelnog migracije započet će nakon prva 4 sigurnosnih kopija u prvome spremniku glasnoću migriraju. | Preporučujemo da migrirati jedan spremnik glasnoću odjednom. | Da | ne |
| 11| Migracije | Nakon vraćanja, količine ne dodaju sigurnosne kopije pravila ili grupi virtualne diskova. | Morat ćete dodati ove jedinice sigurnosne kopije pravila za stvaranje sigurnosne kopije. | Da | Da |
| 12| Migracije | Po završetku migracije uređaja 5000/7000 niz morate pristupiti spremnika premještene podatke. | Preporučujemo da nakon migracije potpune i izvršenja brisanje spremnika premještene podatke. | Da | ne |
| 13| Kloniraj i DR | Na StorSimple uređaja sa sustavom Update 1 nije moguće Kloniraj ili ponovno Izrada oporavak da biste s uređaja sa sustavom stara update 1 softver. | Morat ćete ažuriranje ciljni uređaj ažuriranje 1 da biste omogućili te operacije | Da | Da |
| 14 | Migracije | Sigurnosno kopiranje konfiguracije za migraciju možda neće uspjeti na uređaju niz 5000 7000 kad postoje glasnoću grupe s nema pridruženi jedinica. | Brisanje svih grupa prazan glasnoću s bez pridružene količine i ponovno pokušajte konfiguracije sigurnosnu kopiju.| Da | ne |
| 15 | Azure PowerShell cmdleti i lokalno prikvačene količine | Ne možete stvoriti lokalno prikvačene glasnoću putem cmdleta ljuske PowerShell Azure. (Bilo koje jedinice stvorite PowerShell Azure će biti tiered.) |Uvijek koristi StorSimple Upravitelj servisa da biste konfigurirali lokalno prikvačene količine.| Da | ne |
| 16 |Dostupnog prostora za lokalno prikvačene količine | Ako izbrišete lokalno prikvačene glasnoće, prostora za nove jedinice možda se neće ažurirati odmah. Servis za upravitelja StorSimple ažurira lokalne dostupnom prostoru približno svaki sat.| Pričekajte jedan sat prije nego se pokušate stvoriti novu jedinicu. | Da | ne |
| 17 | Lokalno prikvačene količine | Vaš posao Vrati izlaže sigurnosne kopije privremene snimke u katalogu sigurnosnog kopiranja, ali samo za trajanje zadatak obnavljanja. Uz to, izlaže grupa virtualne diskova s prefiksa **tmpCollection** na stranici **Sigurnosne kopije pravila** , ali samo za trajanje zadatka vraćanja. | U ovom se može dogoditi ako je vaš posao vraćanja sadrži samo lokalno prikvačene količine ili kombinacije lokalno prikvačeni i tiered jedinice. Ako je zadatak obnavljanja obuhvaća samo tiered količine, zatim takvo ponašanje neće dogoditi. Potreban je bez intervencije korisnika. | Da | ne |
| 18 | Lokalno prikvačene količine | Ako odustanete zadatak obnavljanja i kontroler prebacivanje pojavljuje se odmah nakon toga zadatak obnavljanja vode **nije uspjelo** umjesto **Otkazano**. Ako ne uspije, zadatak obnavljanja kontroler prebacivanje pojavljuje odmah nakon toga zadatak obnavljanja vode **otkazana** umjesto **nije uspjelo**. | U ovom se može dogoditi ako je vaš posao vraćanja sadrži samo lokalno prikvačene količine ili kombinacije lokalno prikvačeni i tiered jedinice. Ako je zadatak obnavljanja obuhvaća samo tiered količine, zatim takvo ponašanje neće dogoditi. Potreban je bez intervencije korisnika. | Da | ne |
| 19 |Lokalno prikvačene količine | Ako odustanete zadatak obnavljanja ili vraćanja ne uspije, a zatim pojavljuje kontroler prebacivanje, zadatak programa dodatne obnavljanja pojavit će se na stranici **Poslovi** . | U ovom se može dogoditi ako je vaš posao vraćanja sadrži samo lokalno prikvačene količine ili kombinacije lokalno prikvačeni i tiered jedinice. Ako je zadatak obnavljanja obuhvaća samo tiered količine, zatim takvo ponašanje neće dogoditi. Potreban je bez intervencije korisnika. | Da | ne |
| 20 |Lokalno prikvačene količine | Ako pokušate jedinicu pretvoriti u tiered (stvorene i kloniranu s Update 1.2 ili stariji) lokalno prikvačene glasnoću i uređaju nema dovoljno mjesta ili postoji oblak prekida, zatim na clone(s) možete oštećen.| Taj se problem pojavljuje samo s količine koje su stvorene i kloniranu s stara Update 2 softver. Trebali biste rijetko scenarij.|
| 21 | Pretvorba glasnoće | Ažuriranje ACRs priložiti jedinice dok glasnoću pretvorbe (tiered na lokalno prikvačene i obrnuto). Ažuriranje na ACRs može rezultirati oštećenja podataka. | Ako je potrebno, ažurirajte ACRs prije pretvaranja glasnoće, a ne upućujete izvući ACR ažuriranja dok pretvorbe. |

## <a name="controller-and-firmware-updates-in-update-2"></a>Kontrolera i opreme ažuriranja u ažuriranju 2

U ovom izdanju ažurirati upravljački program i opreme disk na uređaju.
 
- Dodatne informacije o firmver LSI ažuriranje, potražite u članku Microsoftove baze znanja 3121900. 
- Dodatne informacije o firmver disk ažuriranje, potražite u članku Microsoftove baze znanja 3121899.
 
## <a name="virtual-device-updates-in-update-2"></a>Ažuriranja virtualnog uređaja u ažuriranju 2

Ažuriranje nije moguće primijeniti na virtualnog uređaja. Novim uređajima virtualne ćete će biti stvoren. 

## <a name="next-step"></a>Sljedeći korak

Saznajte kako na uređaju StorSimple [instalirajte ažuriranje 2](storsimple-install-update-2.md) .
