<properties 
   pageTitle="Napomene u StorSimple 8000 niz ažuriranje 3 | Microsoft Azure"
   description="U članku se opisuje novih značajki, probleme i zaobilazna rješenja za StorSimple 8000 niz ažuriranje 3."
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
   ms.date="10/14/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-3-release-notes"></a>Napomene u StorSimple 8000 niz ažuriranje 3  

## <a name="overview"></a>Pregled

Sljedeće napomene opisuju nove značajke i prepoznati kritičnih problema Otvori StorSimple 8000 niz ažuriranje 3. Koje sadrže i popis softverska ažuriranja StorSimple koje ovo izdanje obuhvaća. 

Ažuriranje 3 primjenjuje se na bilo kojem uređaju StorSimple izdanje (GA) ili ažuriranje 0,1 primjene ažuriranje 2.2. Verzija uređaja koji je povezan s 3 ažuriranje je 6.3.9600.17759.

Pregledajte informacije koje se nalaze u napomenama u prije implementacije ažuriranja u rješenje StorSimple.

>[AZURE.IMPORTANT]
> 
> - Ažuriranje 3 sadrži softver uređaja, upravljački program za LSI i opreme i Storport i Spaceport prilagođava. Traje približno 1,5 2 sata da biste instalirali to ažuriranje. 

> - Za novi izdanja, možda nećete vidjeti ažuriranja odmah jer moramo fazama uvođenje ažuriranja. Pričekajte nekoliko dana, a zatim traženje ažuriranja ponovno kao one će biti dostupna uskoro.


## <a name="whats-new-in-update-3"></a>Što je novo u ažuriranje 3

Sljedeća poboljšanja ključa i popravci programskih pogrešaka uvedena u ažuriranje 3.

 
- **Automated prostor reclamation promjene** – početni Ažuriraj 3, algoritama reclamation prostor se izvoditi na kontrolerom čekanja sustava rezultira brže izvršavanje. Dodatne informacije o priključke koje su potrebne za rad s reclamation prostora priručniku [StorSimple umrežavanje preduvjeti](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

- **Poboljšane performanse** – ažuriranje 3 poboljšane performanse čitanja i pisanja u oblak.

- **Poboljšanja vezanih uz migracije** – u ovom izdanju, nekoliko popravci programskih pogrešaka i poboljšanja dovršetka za značajku migracije s uređaja niz 5000/7000 8000 niz uređajima. Dodatne informacije o korištenju značajke za migraciju, idite na [migraciju putem uređaja niz 5000/7000 8000 niz uređaj](https://www.microsoft.com/en-us/download/details.aspx?id=47322). 

- **Nadzor povezane ispravaka** – u ovom izdanju programskih pogrešaka vezanih uz nadzor grafikone, nadzorna ploča servisa i nadzorne ploče uređaj fiksnih.


## <a name="issues-fixed-in-update-3"></a>Problemi s fiksiran ažuriranje 3

Sljedeća tablica sadrži sažetak problema koje je fiksnih ažuriranje 3.    

| ne | Značajka                                    | Problem                                                                                                                                                                                                                                                                                        | Odnosi se na fizički uređaj | Odnosi se na virtualnog uređaja |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Prijenos podataka drugo glavno računalo                      | Na prethodnu verziju potražite oblaka StorSimple je izvanmrežni tijekom migracije podataka strani glavnog računala. Taj se problem ne riješi u ovom izdanju.                                                | ne                        | Da                        |
| 2  | Lokalno prikvačene jedinicama                     | U prethodnog izdanja došlo je do problema vezanih uz/i pogreške, glasnoću pretvorbe pogrešaka i datapath pogrešaka za lokalno prikvačene količine. Te probleme kada su korijenske uzrokovati ili fiksno u ovom izdanju.                | Da                        | ne                       |
| 3  | Nadzor                                    | Pojavile su se više problema vezanih uz izvješćivanja jedinice i nadzor te uređaj nadzorne ploče grafikoni gdje je pogrešne podatke prikazane lokalno prikvačene količine. U ovom izdanju su fiksni te probleme. | Da                         | ne                       |
| 4  | / I velike zapisivanja                          | Prilikom korištenja StorSimple za radnih opterećenja koje obuhvaćaju podebljanom zapisivanje, korisnik će naići rijetko pogrešku gdje je u tijeku u oblak tiered radni skup. U ovom izdanju se popravi ovu pogrešku. | Da                        | Da                       |
| 5  | Sigurnosno kopiranje                                     | U određenim slučajevima rijetko, u prethodnim verzijama softver, kada korisnik snimljene sigurnosnu kopiju udaljene Kloniraj, mogu raditi u oblak pogreške i postupak bi pogreške izgleda. U ovom izdanju problem ne riješi, a zatim uspješno dovršetka postupka.             | Da                        | Da                       |
| 6  | Sigurnosne kopije pravila                                     | U nekim slučajevima rijetko iz prethodnih izdanja softver, pojavio problema vezanih uz brisanje sigurnosne kopije pravila. Taj se problem ne riješi u ovom izdanju.       | Da                        | Da                       |



## <a name="known-issues-in-update-3"></a>Poznati problemi u ažuriranje 3

Sljedeća tablica daje sažetak Poznati problemi u ovom izdanju.

| ne. | Značajka | Problem | Komentari / zaobilazno rješenje | Odnosi se na fizički uređaj | Odnosi se na virtualnog uređaja |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Kvorum disk | U rijetko slučajevima, ako su povezani Većina diskova s prilozima EBOD 8600 uređaja rezultira kvorum bez diska pa na resurse za pohranu će raditi izvanmrežno. On će ostati izvanmrežne čak i ako na diskova se ponovno povezati sa sustavom. | Morat ćete ponovno pokrenite uređaj. Ako se problem nastavi pojavljivati, obratite se Microsoft Support za sljedeće korake. | Da | ne |
| 2 | ID netočan kontrolera | Zamjena kontroler provodi, kontroler 0 možda prikazuju se kao kontroler 1. Tijekom zamjenski kontroler prilikom učitavanja slike iz čvor ravnopravnih članova ID kontrolera može prikazivati na početku kao kontrolerom ravnopravnih članova ID-a. U rijetko instancama takvo ponašanje mogu se vidjeti i nakon ponovnog pokretanja sustava. | Potreban je bez korisničku akciju. U ovom slučaju će riješiti sam nakon zamjenski kontroler. | Da | ne |
| 3 | Računi za pohranu | Putem servisa za pohranu da biste izbrisali račun za pohranu je nepodržane scenarij. To će dovesti do situacije u kojima ne može dohvatiti podatke o korisniku.|  | Da | Da |
| 4 | Prebacivanje na uređaju | Više failovers glasnoću spremnika na istom uređaju izvora različite ciljnih nije podržana. Prebacivanje s jednog reagira uređaja na više uređaja će postati spremnika glasnoće na prvom nije uspjela putem uređaja izgubiti vlasništvo nad podacima. Nakon takve prebacivanje te spremnika glasnoću će prikazuju se ili se drugačije ponašaju prilikom prikaza na portalu za Azure klasični. | | Da | ne |
| 5 | Instalacija | Tijekom StorSimple prilagodnik za instalaciju sustava SharePoint, morate unijeti IP uređaj za instalaciju da biste uspješno završili.    | | Da | ne |
| 6 | Web proxy | Ako je vaše web-konfiguracija proxy HTTPS kao navedeni protokol, utjecat će vaše komunikacije servisa uređaja i će izvanmrežni uređaj. Podrška za pakete i moguće generirati u postupku troše značajan resursa na uređaju. | Provjerite ima li web-URL proxy HTTP kao navedeni protokol. Dodatne informacije, idite na [konfiguracija web proxy za svoj uređaj](storsimple-configure-web-proxy.md). | Da | ne |
| 7 | Web proxy | Ako konfigurirati i omogućite web proxy na uređaju registrirani, zatim morat ćete ponovno pokrenite active kontroler na uređaju. | | Da | ne |
| 8 | Latencija visoke oblaka i visoke/i radno opterećenje | Kada naiđe na uređaju StorSimple kombinaciju vrlo visoka oblaka latencies (redoslijeda sekundi) i visoke/i radno opterećenje, količine uređaj idite u su smanjene stanje i na I/Os možda neće funkcionirati uz poruku o pogrešci "uređaj nije spreman". | Morat ćete ručno ponovno kontrolera uređaja ili ponovno prebacivanje uređaj da biste oporavili u tom slučaju. | Da | ne |
| 9 | Azure PowerShell | Kada koristite cmdlet StorSimple **Get-AzureStorSimpleStorageAccountCredential & #124; Odaberite objekt - najprije 1 - čekanja** da biste odabrali prvi objekt tako da možete stvoriti novi objekt **VolumeContainer** , cmdlet vraća sve objekte. | Prelamanje cmdlet u zagradama na sljedeći način: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Odaberite objekt - najprije 1 - čekanja** | Da | Da |
| 10| Migracije | Kada više spremnika glasnoću dodjeljuju se za migraciju, ETA za zadnje sigurnosne kopije je točne samo za prvome spremniku glasnoću. Uz to, paralelnog migracije započet će nakon prva 4 sigurnosnih kopija u prvome spremniku glasnoću migriraju. | Preporučujemo da migrirati jedan spremnik glasnoću odjednom. | Da | ne |
| 11| Migracije | Nakon obnavljanja, količine ne dodaju sigurnosne kopije pravila ili grupi virtualne diskova. | Morat ćete dodati ove jedinice sigurnosne kopije pravila za stvaranje sigurnosne kopije. | Da | Da |
| 12| Migracije | Po završetku migracije uređaja 5000/7000 niz morate pristupiti spremnika premještene podatke. | Preporučujemo da nakon migracije potpune i izvršenja brisanje spremnika premještene podatke. | Da | ne |
| 13| Kloniraj i DR | Na StorSimple uređaja sa sustavom Update 1 nije moguće Kloniraj ili ponovno Izrada oporavak da biste s uređaja sa sustavom prije update 1 softver. | Morat ćete ažuriranje ciljni uređaj ažuriranje 1 da biste omogućili te operacije | Da | Da |
| 14 | Migracije | Sigurnosno kopiranje konfiguracije za migraciju možda neće uspjeti na uređaju niz 5000 7000 kad postoje glasnoću grupe s nema pridruženi jedinica. | Brisanje svih grupa prazan glasnoću s bez pridružene količine i ponovno pokušajte konfiguracije sigurnosnu kopiju.| Da | ne |
| 15 | Azure PowerShell cmdleti i lokalno prikvačene količine | Ne možete stvoriti lokalno prikvačene glasnoću putem cmdleta ljuske PowerShell Azure. (Bilo koje jedinice stvorite PowerShell Azure će biti tiered.) |Uvijek koristi StorSimple Upravitelj servisa da biste konfigurirali lokalno prikvačene količine.| Da | ne |
| 16 |Dostupnog prostora za lokalno prikvačene količine | Ako izbrišete lokalno prikvačene glasnoće, prostora za nove jedinice možda se neće ažurirati odmah. Servis za upravitelja StorSimple ažurira lokalne dostupnom prostoru približno svaki sat.| Pričekajte jedan sat prije nego se pokušate stvoriti novu jedinicu. | Da | ne |
| 17 | Lokalno prikvačene jedinicama | Vraćanje posla izlaže sigurnosne kopije privremene snimke u katalogu sigurnosnog kopiranja, ali samo za trajanje zadatka vraćanja. Uz to, izlaže grupa virtualne diskova s prefiksa **tmpCollection** na stranici **Sigurnosne kopije pravila** , ali samo za trajanje zadatka vraćanja. | U ovom se može dogoditi ako je vaš posao vraćanja sadrži samo lokalno prikvačene količine ili kombinacije lokalno prikvačeni i tiered jedinice. Ako je zadatak obnavljanja obuhvaća samo tiered količine, zatim takvo ponašanje neće dogoditi. Potreban je bez intervencije korisnika. | Da | ne |
| 18 | Lokalno prikvačene jedinicama | Ako odustanete zadatak obnavljanja i kontroler prebacivanje pojavljuje se odmah nakon toga zadatak obnavljanja vode **nije uspjelo** umjesto **Otkazano**. Ako ne uspije, zadatak obnavljanja kontroler prebacivanje pojavljuje odmah nakon toga zadatak obnavljanja vode **otkazana** umjesto **nije uspjelo**. | U ovom se može dogoditi ako je vaš posao vraćanja sadrži samo lokalno prikvačene jedinicama ili kombinacije lokalno prikvačeni i tiered jedinice. Ako je zadatak obnavljanja obuhvaća samo tiered količine, zatim takvo ponašanje neće dogoditi. Potreban je bez intervencije korisnika. | Da | ne |
| 19 |Lokalno prikvačene jedinicama | Ako odustanete zadatak obnavljanja ili vraćanja ne uspije, a zatim pojavljuje kontroler prebacivanje, zadatak programa dodatne obnavljanja pojavit će se na stranici **Poslovi** . | U ovom se može dogoditi ako je vaš posao vraćanja sadrži samo lokalno prikvačene količine ili kombinacije lokalno prikvačeni i tiered jedinice. Ako je zadatak obnavljanja obuhvaća samo tiered količine, zatim takvo ponašanje neće dogoditi. Potreban je bez intervencije korisnika. | Da | ne |
| 20 |Lokalno prikvačene količine | Ako pokušate jedinicu pretvoriti u tiered (stvorene i kloniranu s Update 1.2 ili stariji) lokalno prikvačene glasnoću i uređaju nema dovoljno mjesta ili postoji oblak prekida, zatim na clone(s) možete oštećen.| Taj se problem pojavljuje samo s količine koje su stvorene i kloniranu s stara Update 2.1 softver. Trebali biste rijetko scenarij.|
| 21 | Pretvorba glasnoće | Ažuriranje ACRs priložiti jedinice dok glasnoću pretvorbe (tiered na lokalno prikvačene i obrnuto). Ažuriranje na ACRs može rezultirati oštećenja podataka. | Ako je potrebno, ažurirajte ACRs prije pretvaranja glasnoće, a ne upućujete izvući ACR ažuriranja dok pretvorbe. |
| 22 | Ažuriranja | Kada primijenite ažuriranje 3, stranice za **Održavanje** na portalu za Azure klasični prikazat će se sljedeća poruka vezanih uz ažuriranju 2 – "StorSimple 8000 niz ažuriranje 2 obuhvaća mogućnost Microsoftu doći prikupljanje informacija zapisnika s uređaja kada možemo prepoznati potencijalne probleme". To je navođenjem kao što znači da uređaj ažurira ažuriranju 2. Kada je uređaj succeesfully ažurirati ažuriranje 3, nestat će se ova poruka. | Takvo ponašanje će biti popravljeno u buduće izdanje. | Da | ne |


## <a name="controller-and-firmware-updates-in-update-3"></a>Kontrolera i opreme ažuriranja u ažuriranje 3

Ovo izdanje sadrži LSI ažuriranja za upravljački program i opreme. Dodatne informacije o instaliranju upravljački program LSI i ažuriranja opreme, potražite u članku [Instaliranje ažuriranja 3](storsimple-install-update-3.md) na uređaju StorSimple.

 
## <a name="virtual-device-updates-in-update-3"></a>Ažuriranja virtualnog uređaja u ažuriranje 3

Ažuriranje nije moguće primijeniti na oblak uređaj StorSimple (poznat i kao virtualnog uređaja). Novim uređajima virtualne ćete će biti stvoren. 


## <a name="next-step"></a>Sljedeći korak

Saznajte kako [instalirati ažuriranje 3](storsimple-install-update-3.md) na uređaju StorSimple.
