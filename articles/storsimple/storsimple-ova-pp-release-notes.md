<properties 
   pageTitle="Napomene u StorSimple virtualne polja | Microsoft Azure"
   description="Opisuje kritičnih Otvori problema i rješenja za polja virtualne StorSimple."
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
   ms.workload="NA"
   ms.date="05/13/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-release-notes"></a>Napomene u StorSimple virtualne polja

## <a name="overview"></a>Pregled

Sljedeće napomene odredite kritičnih problema otvorene za ožujak 2016 Općenito dostupan (GA) izdanje sustava Microsoft Azure StorSimple virtualne polja (poznat i kao virtualnog uređaja za lokalni StorSimple ili StorSimple virtualnog uređaja). U ovom izdanju odgovara verzija softvera 10.0.10271.0.

Napomene u neprestano ažuriraju i kao prepoznate kritičnih problema koje je obavezna zaobilazno rješenje se dodaju. Prije implementacije virtualnog uređaja StorSimple pažljivo pregledajte informacije koje se nalaze u napomenama u. 

Sljedeća tablica daje sažetak Poznati problemi u ovom izdanju.


| ne. | Značajka | Problem | Zaobilazno rješenje/komentara |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Ažuriranja | Virtualna uređaje stvorene u pretpregledu aplikacije nije moguće ažurirati u podržanu verziju Općenito dostupan. | Te virtualne uređaje mora biti nije uspjelo putem izdanju Općenito dostupan pomoću tijeka rada Izrada oporavak (DR). |
| **2.** | Dodjela resursa podatkovni disk | Nakon što ste dodjeli podatkovni disk određene veličine navedeni i stvorili odgovarajuće StorSimple virtualnog uređaja, morate ne proširivanje ili smanjivanje podatkovni disk. To pokušate rezultirat će do gubitka podataka u lokalnom razine uređaja. |   |
| **3.** | Pravilnik grupe | Kada uređaj je domena pridružili, Primjena pravilnika grupe koji može negativno utjecati na operaciju uređaj. | Provjerite je li vaša virtualne polja je u vlastitom organizacijsku jedinicu (OU) za Active Directory i bez objekti pravilnika grupe (GPO) primjenjuju se na njega.|
| **4.** | Lokalni web korisničkog Sučelja | Ako bolja sigurnost značajke omogućene u pregledniku Internet Explorer (IE ESC), neke stranice lokalne web korisničkog Sučelja kao što su otklanjanje poteškoća ili održavanje možda neće pravilno funkcionirati. Gumbi na te stranice i možda neće funkcionirati. | Isključivanje značajke bolja sigurnost u pregledniku Internet Explorer.|
| **5.** | Lokalni web korisničkog Sučelja | U virtualnog računala za Hyper-V sučelje mreže na web-mjestu korisničkog Sučelja prikazuju se kao 10 Gbps sučelja. | To je odraz Hyper-V. Hyper-V uvijek prikazuje 10 Gbps za virtualne mrežnih prilagodnika. |
| **6.** | Tiered količine i zajedničko korištenje | Raspon bajtni zaključavanje za aplikacije koje funkcioniraju s StorSimple tiered jedinicama nisu podržani. Ako je omogućeno zaključavanje raspon bajta, neće funkcionirati StorSimple tiering. | Preporučena mjere obuhvaćaju sljedeće: <br></br>Isključite bajtni raspon zaključavanje logike aplikacije.<br></br>Odaberite da biste postavili podaci za ovu aplikaciju lokalno prikvačene količine umjesto tiered količine.<br></br>*Caveat*: Ako pomoću lokalno prikvačene količine i zaključavanje raspon bajt je omogućena, imajte na umu da lokalno prikvačene jedinica može biti online čak i prije dovršetka vraćanja. U takvim slučajevima, ako vraćanja je u tijeku, zatim morate pričekati da se vrati da biste dovršili. |
| **7.** | Tiered zajedničko korištenje | Rad s velikih datoteka može rezultirati sporo sloju odgovor. | Rad s velikih datoteka, preporučujemo da Najveća datoteka manja od 3% veličine zajedničko korištenje. |
| **8.** | Koristi kapacitet za zajedničko korištenje | Možda ćete vidjeti zajedničko korištenje potrošnje u Izostanak sve podatke na zajedničko korištenje. Ovo je jer korištenih kapaciteta za zajedničke sadrži metapodatke. |   |
| **9.** | Izrada oporavak | Možete izvršiti samo oporavak Izrada datotečnom poslužitelju za istu domenu kao izvor uređaja. Izrada oporavak ciljni uređaj u drugu domenu nije podržan u ovom izdanju. | To će se implementirati u novije verzije. |
| **10.** | Azure PowerShell | Virtualna uređaje StorSimple nije moguće upravljati putem komponente PowerShell Azure u ovom izdanju. | Upravljanje virtualne uređaja računajući putem portala za Azure klasični i lokalne web korisničkog Sučelja. |
| **11.** | Promjena lozinke | Konzola za uređaj virtualne polja prihvaća samo unos u obliku tipkovnice hr-HR. |   |
| **12.** | CHAP | Nije moguće ukloniti vjerodajnice CHAP jednom stvorili. Osim toga, ako mijenjate CHAP vjerodajnice, morat ćete izvanmrežni jedinice, a zatim ih Internetu se promjene primijenile. | To će biti adresirana novije verzije. |
| **13.** | iSCSI poslužitelja  | Na "koristi za pohranu" prikazuje jedinici s iSCSI mogu se razlikovati u StorSimple Upravitelj servisa i iSCSI glavnog računala. | Glavno računalo iSCSI sadrži prikaz datotečnom sustavu.<br></br>Uređaj ne vidi blokovi dodijeliti kada je glasnoće na maksimalne veličine.|
