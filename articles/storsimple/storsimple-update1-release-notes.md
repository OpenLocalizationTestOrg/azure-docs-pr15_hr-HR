<properties 
   pageTitle="Napomene u StorSimple 8000 niz Update 1.2 | Microsoft Azure"
   description="Opisuje novih značajki, probleme i zaobilazna rješenja za StorSimple 8000 niz Update 1.2."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-12-release-notes"></a>Napomene u StorSimple 8000 niz Update 1.2  

## <a name="overview"></a>Pregled

Sljedeće napomene opisuju nove značajke i identificirati kritičnih problema Otvori StorSimple 8000 niz Update 1.2. Koje sadrže i popis StorSimple softver, upravljački program i disk koje ovo izdanje obuhvaća ažuriranja za opreme. 

Ažuriranje 1.2 primjenjuje se na bilo kojem uređaju StorSimple instalirano izdanje (GA), ažuriranje 0,1, ažuriranje 0,2 ili ažuriranje 0,3 softvera. Ažuriranje 1.2 nije dostupna ako vaš uređaj radi Update 1 ili ažuriranje 1.1. Ako vaš uređaj radi izdanje (GA), imajte [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) radi jednostavnijeg instalacije to ažuriranje.

U sljedećoj su tablici navedeni verzija softvera uređaja koji odgovaraju ažuriranja 1, 1.1 i 1.2.

| Ako se izvodi ažuriranje... | To je vaša verzija softver uređaja. |
|---------------------|---------------------------------------|
| Ažuriranje 1.2          | 6.3.9600.17584                        |
| Ažuriranje 1.1          | 6.3.9600.17521                        |
| Ažuriranje 1.0          | 6.3.9600.17491                        |

Pregledajte informacije koje se nalaze u napomenama u prije implementacije ažuriranja u rješenje StorSimple. Dodatne informacije potražite u članku kako [instalirati ažuriranje 1.2 na uređaju StorSimple](storsimple-install-update-1.md). 

>[AZURE.IMPORTANT]
> 
- Traje otprilike 5 10 sati nabaviti (uključujući ažuriranja za Windows). 
- Ažuriranje 1.2 sadrži softver, upravljački program za LSI i ažuriranja opreme disk. Da biste instalirali, slijedite upute navedene u članku [Instaliranje ažuriranja 1.2 na uređaju StorSimple](storsimple-install-update-1.md).
- Za novi izdanja, možda nećete vidjeti ažuriranja odmah jer moramo fazama uvođenje ažuriranja. Traženje ažuriranja u nekoliko dana ponovno kao te će postati dostupne uskoro.


## <a name="whats-new-in-update-12"></a>Što je novo u Update 1.2

Te značajke nisu najprije objavio ažuriranje 1 koji je bili dostupni za ograničeni skup korisnika. Uz izdanje Update 1.2 većina korisnika StorSimple želite vidjeti sljedeće nove značajke i poboljšanja:

- **Migracija iz serije 5000 7000 8000 niz uređajima** – to je izdanje predstavlja nove značajke za migraciju koje se StorSimple 5000 7000 niza potražite korisnicima omogućuje migrirati svoje podatke za uređaj fizički StorSimple 8000 niz ili virtualne potražite. Značajka za migraciju sastoji se od dva propositions vrijednost ključa:                                                                  

    - **Neprekidno poslovanje**, omogućivanjem Migracija postojeće podatke na 5000 7000 aparata niz za aparata 8000 niz.
    - **Ponude značajka Improved od niza aparata 8000**, kao što su učinkovitog središnje upravljanje više aparata putem servisa StorSimple Upravitelj bolje predmete hardvera i ažuriranje opreme, virtualne aparata, mobilnost podataka i značajke za buduće vodič.

    Potražite [Vodič za migraciju](http://www.microsoft.com/download/details.aspx?id=47322) pojedinosti o migriranju 5000 7000 niz StorSimple uređaju 8000 niz. 

- **Dostupnost na portalu za državne ustanove Azure** – StorSimple sada je dostupan na portalu za državne ustanove Azure. Pogledajte kako [implementirati StorSimple uređaj na portalu za državne ustanove Azure](storsimple-deployment-walkthrough-gov.md).

- **Podrška za drugim davateljima usluga oblak** – druge oblaka davateljima usluga podržani su Amazon S3, Amazon S3 RRS, HP i OpenStack (beta).

- **Ažuriranje najnovije API -ji za pohranu** – s ovom izdanju StorSimple ažurirao najnovije servisu Azure prostora za pohranu API-ji. StorSimple 8000 niz uređaja koji su pokrenuti verzija softvera stara ažuriranje 1 (izdanje 0,1, 0,2 i 0,3) verzija programa Azure prostora za pohranu servisa API-ji starije od srpanj 17, 2009. Prema uputama iz ažurirane [objave o uklanjanje verzija servisa za pohranu](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx)po kolovoz 1, 2016 te API-ji će ukinuta. Izuzetne primjenjujete Update 1 StorSimple niz 8000 prije kolovoz 1, 2016 je. Ako uspjeti da biste to učinili, uređaji StorSimple će prestati funkcionira ispravno.

- **Podrška za Zone suvišnih prostora za pohranu (ZRS)** – na nadogradnju na najnoviju verziju prostora za pohranu API-ji niz StorSimple 8000 će podržavati Zone suvišnih prostora za pohranu (ZRS) uz lokalno suvišnih prostora za pohranu (LRS) i zemlj suvišnih prostora za pohranu (GRS). Pogledajte ovaj [članak mogućnosti zalihosti Azure pohrane](../storage/storage-redundancy.md) detalje ZRS.

- **Poboljšana početne implementacije i ažuriranje sučelje** – u ovom izdanju, instalacija i ažuriranje procesa poboljšani su. Slanje povratnih informacija korisnika ako mrežna konfiguracija i postavke vatrozida nisu pravilne je poboljšana instalacije kroz Čarobnjak za postavljanje. Dodatne dijagnostičke cmdleta je dodijeljen će vam pomoći s otklanjanjem poteškoća s mrežom na uređaju. Potražite u [članku implementacija za otklanjanje poteškoća](storsimple-troubleshoot-deployment.md) za dodatne informacije o cmdletima za nove dijagnostičkih koristiti za otklanjanje poteškoća.

## <a name="issues-fixed-in-update-12"></a>Problemi s fiksiran Update 1.2

Sljedeća tablica sadrži sažetak problema koje je fiksnih ažuriranja 1.2, 1.1 i 1.    


| ne. | Značajka | Problem | FIXED u ažuriranja | Odnosi se na fizički uređaj | Odnosi se na virtualnog uređaja |
|-----|---------|-------|-----------------|---------------------------------|--------------------------------|
| 1 | Windows PowerShell za StorSimple | Kada korisnik daljinski pristupiti uređaju StorSimple pomoću komponente Windows PowerShell za StorSimple i pokrenuli čarobnjak za postavljanje, neočekivanog došlo je do uskoro kao IP točan unos podataka 0. Ovu pogrešku sada je fiksiran Update 1. | Ažuriranje 1 | Da | Da |
| 2 | Ponovno postavljanje tvorničke | U nekim slučajevima, kada ste izvršili tvorničke Vrati izvorne postavke StorSimple uređaja postala zamrzne i prikazati ova poruka: **ponovno postavljanje na tvorničke je u tijeku (faza 8)**. To dogodilo ako pritisnete CTRL + C dok cmdlet je u tijeku. Sada se popravi ovu pogrešku.| Ažuriranje 1 | Da | ne |
| 3 | Ponovno postavljanje tvorničke | Nakon što ponovno nije uspjelo dva kontroler tvorničke, su dopušteni da biste nastavili s Registracija uređaja. To je rezultirala u konfiguraciji sustav nije podržan. U Update 1, prikazuje se poruka o pogrešci, a registracija blokiran na uređaju da je nije uspio tvorničke ponovno postavljanje. | Ažuriranje 1 | Da | ne |
| 4 | Ponovno postavljanje tvorničke | U nekim slučajevima potenciju su false pozitivne ne podudara upozorenja. Upozorenja netočan ne podudaraju se više ne stvaraju na uređajima sa sustavom Update 1. | Ažuriranje 1 | Da | ne |
| 5 | Ponovno postavljanje tvorničke | Ako ponovno postavljanje tvorničke prekinut je prije dovršetka, uređaj unijeli način rada za oporavak i pristup komponente Windows PowerShell za StorSimple ne dopuštaju. Sada se popravi ovu pogrešku. | Ažuriranje 1 | Da | ne |
| 6 | Izrada oporavak | Oporavak (DR) pogrešku Izrada je fiksno kojemu DR zadovoljavaju tijekom otkrivanje sigurnosnih kopija na ciljni uređaj. | Ažuriranje 1 | Da | Da |
| 7 | Nadzor LEDs | U nekim slučajevima, nadzor LEDs at the back of potražite jeste li ne označavaju ispravan status. Plavi LED isključena. PODATAKA 0 i podataka 1 LEDs su Bljeskave čak i kada ta sučelja su nije konfiguriran. Problem riješen i nadzor LEDs sada označavanje ispravan status.  | Ažuriranje 1 | Da | ne |
| 8 | Nadzor LEDs | U određenim slučajevima nakon primjene Update 1, plava osnovnu na aktivni kontroleru isključena time što teško prepoznati aktivan kontroler. Taj se problem riješen u ovom izdanju zakrpu.| Ažuriranje 1.2 | Da | ne |
| 9 | Sučelje mreže | U starijim verzijama StorSimple uređaj konfiguriran pomoću koje nisu usmjeravati pristupnik nije moguće raditi izvanmrežno. U ovom izdanju usmjeravanje metriku za podataka 0 je izvršena na najniže; Dakle, čak i ako su drugi sučelje mreže oblaka omogućena, sve promet oblaka s uređaja će usmjeren putem podataka 0. | Ažuriranje 1 | Da | Da | 
| 10 | Sigurnosne kopije | Pogreška u Update 1 koji je uzrok sigurnosne kopije farmi nakon 24 dana fiksiran na izdanju zakrpu ažuriranja 1.1. | Ažuriranje 1.1 | Da | Da |
| 11 | Sigurnosne kopije | Pogreška u prethodnim verzijama rezultirala slabe performanse za oblak snimke s malo promjena stopama. U ovom izdanju zakrpu fiksiran ovu pogrešku.| Ažuriranje 1.2 | Da | Da |
| 12 | Ažuriranja | U ovom izdanju zakrpu fiksiran na pogrešku u Update 1, a koje prijavljenih nije uspjelo nadogradnje i uzrokovati kontrolera da biste prešli u način rada za oporavak.| Ažuriranje 1.2 | Da | Da |


## <a name="known-issues-in-update-12"></a>Poznati problemi u Update 1.2

Sljedeća tablica daje sažetak Poznati problemi u ovom izdanju.

| ne. | Značajka | Problem | Komentari/zaobilazno rješenje | Odnosi se na fizički uređaj | Odnosi se na virtualnog uređaja |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Kvorum disk | U rijetko slučajevima, ako Većina diskova s prilozima EBOD 8600 uređaja niste povezani rezultira kvorum bez diska pa na resurse za pohranu bit će izvanmrežno. On će ostati izvanmrežne čak i ako na diskova se ponovno povezati sa sustavom. | Morat ćete ponovno pokrenite uređaj. Ako se problem nastavi pojavljivati, obratite se Microsoft Support za sljedeće korake. | Da | ne |
| 2 | ID netočan kontrolera | Zamjena kontroler provodi, kontroler 0 možda prikazuju se kao kontroler 1. Tijekom zamjenski kontroler prilikom učitavanja slike iz čvor ravnopravnih članova ID kontrolera može prikazivati na početku kao kontrolerom ravnopravnih članova ID-a. U rijetko instancama takvo ponašanje mogu se vidjeti i nakon ponovnog pokretanja sustava. | Potreban je bez korisničku akciju. U ovom slučaju će riješiti sam nakon zamjenski kontroler. | Da | ne |
| 3 | Računi za pohranu | Putem servisa za pohranu da biste izbrisali račun za pohranu je nepodržane scenarij. To će dovesti do situacije u kojima ne može dohvatiti podatke o korisniku. | Da | Da |
| 4 | Prebacivanje na uređaju | Više failovers glasnoću spremnika na istom uređaju izvora različite ciljnih nije podržana. Uređaj prebacivanje s jednog reagira uređaja na više uređaja će postati spremnika glasnoće na prvi nije uspjela putem uređaja izgubiti vlasništvo nad podacima. Nakon takve prebacivanje te spremnika glasnoću će prikazuju se ili se drugačije ponašaju prilikom prikaza na portalu za Azure klasični. | | Da | ne |
| 5 | Instalacija | Tijekom StorSimple prilagodnik za instalaciju sustava SharePoint, morate unijeti IP uređaj za instalaciju da biste uspješno završili.    | | Da | ne |
| 6 | Web proxy | Ako je vaše web-konfiguracija proxy HTTPS kao navedeni protokol, utjecat će vaše komunikacije servisa uređaja i će izvanmrežni uređaj. Podrška za pakete i moguće generirati u postupku troše značajan resursa na uređaju. | Provjerite ima li web-URL proxy HTTP kao navedeni protokol. Dodatne informacije, idite na [konfiguracija web proxy za svoj uređaj](storsimple-configure-web-proxy.md). | Da | ne |
| 7 | Web proxy | Ako konfigurirati i omogućite web proxy na uređaju registrirani, zatim morat ćete ponovno pokrenite active kontroler na uređaju. | | Da | ne |
| 8 | Latencija visoke oblaka i visoke/i radno opterećenje | Kada naiđe na uređaju StorSimple kombinaciju vrlo visoka oblaka latencies (redoslijeda sekundi) i visoke/i radno opterećenje, količine uređaj idite u su smanjene stanje i na I/Os možda neće funkcionirati uz poruku o pogrešci "uređaj nije spreman". | Morat ćete ručno ponovno kontrolera uređaja ili ponovno prebacivanje uređaj da biste oporavili u tom slučaju. | Da | ne |
| 9 | Azure PowerShell | Kada koristite cmdlet StorSimple **Get-AzureStorSimpleStorageAccountCredential & #124; Odaberite objekt - najprije 1 - čekanja** da biste odabrali prvi objekt tako da možete stvoriti novi objekt **VolumeContainer** , cmdlet vraća sve objekte. | Prelamanje cmdlet u zagradama na sljedeći način: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Odaberite objekt - najprije 1 - čekanja** | Da | Da |
| 10| Migracije | Kada više spremnika glasnoću dodjeljuju se za migraciju, ETA za zadnje sigurnosne kopije je točne samo za prvome spremniku glasnoću. Uz to, paralelnog migracije započet će nakon prva 4 sigurnosnih kopija u prvome spremniku glasnoću migriraju. | Preporučujemo da migrirati jedan spremnik glasnoću odjednom. | Da | ne |
| 11| Migracije | Nakon obnavljanja, količine ne dodaju sigurnosne kopije pravila ili grupi virtualne diskova. | Morat ćete dodati ove jedinice sigurnosne kopije pravila za stvaranje sigurnosne kopije. | Da | Da |
| 12| Migracije | Po završetku migracije uređaja 5000/7000 niz morate pristupiti spremnika premještene podatke. | Preporučujemo da nakon migracije potpune i izvršenja brisanje spremnika premještene podatke. | Da | ne |
| 13| Kloniraj i DR | Na StorSimple uređaja sa sustavom Update 1 nije moguće Kloniraj ili ponovno Izrada oporavak da biste s uređaja sa sustavom stara update 1 softver. | Morat ćete ažuriranje ciljni uređaj ažuriranje 1 da biste omogućili te operacije | Da | Da |
| 14 | Migracije | Sigurnosno kopiranje konfiguracije za migraciju možda neće uspjeti na uređaju niz 5000 7000 kad postoje glasnoću grupe s nema pridruženi jedinica. | Brisanje svih grupa prazan glasnoću s bez pridružene količine i ponovno pokušajte konfiguracije sigurnosnu kopiju.| Da | ne |

## <a name="physical-device-updates-in-update-12"></a>Ažuriranja fizički uređaj u Update 1.2

Ako ažurirate zakrpu 1.2 primjenjuje se na fizički uređaj (koji se pokreće verzijama ažuriranje 1), verzija softvera promijenit će se u 6.3.9600.17584.

## <a name="controller-and-firmware-updates-in-update-12"></a>Kontrolera i opreme ažuriranja u Update 1.2

U ovom izdanju ažurirati upravljački program i opreme disk na uređaju.
 
- Dodatne informacije o ažuriranju kontroler SAS potražite u članku [Ažuriranje 1 za LSI SAS kontrolera Microsoft Azure StorSimple uređaj](https://support.microsoft.com/kb/3043005). 

- Dodatne informacije o firmver disk ažuriranje potražite u članku [Disk firmver ažuriranje 1 za Microsoft Azure StorSimple uređaj](https://support.microsoft.com/kb/3063416).
 
## <a name="virtual-device-updates-in-update-12"></a>Ažuriranja virtualnog uređaja u Update 1.2

Ažuriranje nije moguće primijeniti na virtualnog uređaja. Novim uređajima virtualne ćete će biti stvoren. 

## <a name="next-steps"></a>Daljnji koraci

- [Instalacija ažuriranja 1.2 na uređaju](storsimple-install-update-1.md).
 
