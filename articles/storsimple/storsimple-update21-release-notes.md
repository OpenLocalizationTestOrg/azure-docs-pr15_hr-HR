<properties 
   pageTitle="Napomene u StorSimple 8000 niz ažuriranje 2.2 | Microsoft Azure"
   description="Opisuje novih značajki, probleme i zaobilazna rješenja za StorSimple 8000 niz ažuriranje 2.2."
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
   ms.date="07/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-22-release-notes"></a>StorSimple 8000 niz ažuriranje 2.2 napomene  

## <a name="overview"></a>Pregled

Sljedeće napomene opisuju nove značajke i prepoznati kritičnih problema Otvori StorSimple 8000 niz ažuriranje 2.2. Koje sadrže i popis softverska ažuriranja StorSimple koje ovo izdanje obuhvaća. 

Ažuriranje 2.2 primjenjuje se na bilo kojem uređaju StorSimple izdanje (GA) ili ažuriranje 0,1 primjene 2.1 ažuriranja. Verzija uređaja koji je povezan s ažuriranjem 2.2 je 6.3.9600.17708.

Pregledajte informacije koje se nalaze u napomenama u prije implementacije ažuriranja u rješenje StorSimple.

>[AZURE.IMPORTANT]
> 
> - Ažuriranje 2.2 sadrži samo ažuriranja. Traje približno 1,5 2 sata da biste instalirali to ažuriranje. 

> - Ako koristite 2.1 ažuriranja, preporučujemo da čim Primjena ažuriranja 2.2.

> - Za novi izdanja, možda nećete vidjeti ažuriranja odmah jer moramo fazama uvođenje ažuriranja. Pričekajte nekoliko dana, a zatim traženje ažuriranja ponovno kao one će biti dostupna uskoro.


## <a name="whats-new-in-update-22"></a>Što je novo u 2.2 ažuriranja

Sljedeća poboljšanja ključa uvedena u ažuriranje 2.2.

 
- **Automated prostor reclamation optimizacije** – kada podaci se brišu thinly dodijeljenu količine, moraju biti vraćeno blokove koji se ne koriste za pohranu. Ovo izdanje sadrži poboljšani postupka reclamation prostor iz oblaka rezultira Neiskorišteni prostor postaje dostupan brže to prethodne verzije.


- **Poboljšane performanse snimke** – ažuriranje 2.2 poboljšane vrijeme za obradu oblaka snimke u određenim slučajevima gdje je velike količine koriste i nema minimalnog da nema churn podataka. Scenarij u kojem bi im ovaj poboljšanja bi količine arhiva.


- **Utvrđivanje podršku paketa prikupljanje** – je izričito poboljšanja način paketa podrška nije prikupili i prenijeti u ovom izdanju. 


- **Ažuriranje pouzdanosti poboljšanja** – to je izdanje sadrži popravci programskih pogrešaka koje rezultiraju veća pouzdanost za ažuriranje.

  
 

## <a name="issues-fixed-in-update-22"></a>Problemi s fiksiran 2.2 ažuriranja

Sljedeća tablica sadrži sažetak problema koje je fiksnih 2.2 ažuriranja i 2.1.    

| ne | Značajka                                    | Problem                                                                                                                                                                                                                                                                                        | Odnosi se na fizički uređaj | Odnosi se na virtualnog uređaja |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Glavno računalo performansi                      | Starije verzije probleme s performansama glavno računalo strani su opaženih tijekom stvaranja lokalno prikvačene glasnoću i prilikom pretvorbe tiered jedinice lokalno prikvačene jedinicu. U ovom izdanju time rezultira poboljšanje performansi glavnog računala tijekom glasnoću stvaranja i Pretvorba postupke su fiksni te probleme.                                                                        | Da                        | ne                        |
| 2  | Lokalno prikvačene količine                     | Sustav će u rijetko instancama neočekivano zatvoriti prilikom stvaranja lokalno prikvačene glasnoću. U ovom izdanju fiksiran ovu pogrešku.                                                                                                                                                               | Da                        | ne                        |
| 3  | Tiering                                    | Došlo je do povremeni gubitak ruši prilikom metapodataka za oblak aparata StorSimple (8010 i 8020) tiered u oblak. Taj se problem ne riješi u ovom izdanju.                                                                                                                              | ne                         | Da                       |
| 4  | Stvaranje brze snimke                          | Došlo je do probleme vezane uz stvaranje rastuće snimke u scenarijima s velike količine i Minimalni broj stranica da biste churn bez podataka. U ovom izdanju su fiksni te probleme.                                                                                                                 | Da                        | Da                       |
| 5  | Provjera autentičnosti Openstack                   | Prilikom korištenja Openstack kao usluga oblaka, korisnik će naići rijetko problema koji se odnose na provjeru autentičnosti gdje JSON analizatora rezultirala neočekivanog. U ovom izdanju se popravi ovu pogrešku.                                                                                                                              | Da                        | ne                        |
| 6  | Kopiraj na drugo glavno računalo                             | U starijim verzijama programa rijetko problema koji se odnose na tempiranje ODX je vidjeti kada kopirate podatke iz jedna jedinica na drugu jedinicu. Rezultat kontroler prebacivanje i sustav nije potencijalno idu u načinu rada za oporavak. U ovom izdanju se popravi ovu pogrešku. | Da                        | ne       |
| 7  | Windows Management Instrumentation (WMI) | U starijim verzijama programa su više instanci web proxy neuspjeh uz iznimku "<ManagementException> pogreške prilikom učitavanja davatelja". Ovu pogrešku je atribut za curenje memorije WMI, a sada riješen.                                                               | Da                        | ne                        |
| 8  | Ažuriranje                                     | U određenim slučajevima rijetko, u prethodnim verzijama softver, korisnik primio "CisPowershellHcsscripterror" prilikom pokušaja skeniranje i instaliranje ažuriranja. Taj se problem ne riješi u ovom izdanju.                                                                                        | Da                        | Da                       |
| 9  | Paket za podršku                            | U ovom izdanju su poboljšanja način paketa podrška nije prikupili i prenijeti.                                                                                                                                                                                                      | Da                        | Da                                    |


## <a name="known-issues-in-update-22"></a>Poznati problemi u 2.2 ažuriranja

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
| 18 | Lokalno prikvačene količine | Ako odustanete zadatak obnavljanja i kontroler prebacivanje pojavljuje se odmah nakon toga zadatak obnavljanja vode **nije uspjelo** umjesto **Otkazano**. Ako ne uspije, zadatak obnavljanja kontroler prebacivanje pojavljuje odmah nakon toga zadatak obnavljanja vode **otkazana** umjesto **nije uspjelo**. | U ovom se može dogoditi ako je vaš posao vraćanja sadrži samo lokalno prikvačene količine ili kombinacije lokalno prikvačeni i tiered jedinice. Ako zadatak obnavljanja obuhvaća samo tiered jedinicama, zatim takvo ponašanje neće dogoditi. Potreban je bez intervencije korisnika. | Da | ne |
| 19 |Lokalno prikvačene količine | Ako odustanete zadatak obnavljanja ili vraćanja ne uspije, a zatim pojavljuje kontroler prebacivanje, zadatak programa dodatne obnavljanja pojavit će se na stranici **Poslovi** . | U ovom se može dogoditi ako je vaš posao vraćanja sadrži samo lokalno prikvačene količine ili kombinacije lokalno prikvačeni i tiered jedinice. Ako je zadatak obnavljanja obuhvaća samo tiered količine, zatim takvo ponašanje neće dogoditi. Potreban je bez intervencije korisnika. | Da | ne |
| 20 |Lokalno prikvačene količine | Ako pokušate jedinicu pretvoriti u tiered (stvorene i kloniranu s Update 1.2 ili stariji) lokalno prikvačene glasnoću i uređaju nema dovoljno mjesta ili postoji oblak prekida, zatim na clone(s) možete oštećen.| Taj se problem pojavljuje samo s količine koje su stvorene i kloniranu s stara Update 2.1 softver. Trebali biste rijetko scenarij.|
| 21 | Pretvorba glasnoće | Ažuriranje ACRs priložiti jedinice dok glasnoću pretvorbe (tiered na lokalno prikvačene i obrnuto). Ažuriranje na ACRs može rezultirati oštećenja podataka. | Ako je potrebno, ažurirajte ACRs prije pretvaranja glasnoće, a ne upućujete izvući ACR ažuriranja dok pretvorbe. |

## <a name="controller-and-firmware-updates-in-update-22"></a>Kontrolera i opreme ažuriranja u 2.2 ažuriranja

Ovo izdanje sadrži samo za softver ažuriranja. Međutim, ako ažurirate iz verzije prije ažuriranju 2, morat ćete instalirati upravljački program, Storport, Spaceport, a zatim (u nekim slučajevima) disk ažuriranja opreme na uređaju.
 
Dodatne informacije o instaliranju upravljački program, Storport, Spaceport i ažuriranja opreme diska potražite u članku [Instaliranje ažuriranja 2.2](storsimple-install-update-21.md) na uređaju StorSimple.

 
## <a name="virtual-device-updates-in-update-22"></a>Ažuriranja virtualnog uređaja u 2.2 ažuriranja

Ažuriranje nije moguće primijeniti na virtualnog uređaja. Novim uređajima virtualne ćete će biti stvoren. 

## <a name="next-step"></a>Sljedeći korak

Saznajte kako [instalirati ažuriranje 2.2](storsimple-install-update-21.md) na uređaju StorSimple.
