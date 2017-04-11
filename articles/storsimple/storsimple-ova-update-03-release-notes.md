<properties 
   pageTitle="Napomene o StorSimple virtualne polja ažuriranja | Microsoft Azure"
   description="Opisuje kritičnih Otvori problema i rješenja za polja virtualne StorSimple radi ažuriranja 0,3."
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
   ms.date="09/15/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple virtualne polja ažuriranje 0,3 napomene

## <a name="overview"></a>Pregled

Sljedeće napomene identificirati kritičnih problema Otvori i riješi probleme ima li ažuriranja za Microsoft Azure StorSimple virtualne polja.

Napomene u neprestano ažuriraju i kao prepoznate kritičnih problema koje je obavezna zaobilazno rješenje se dodaju. Prije implementacije sustava StorSimple virtualne polja pažljivo pregledajte informacije koje se nalaze u napomenama u.

Ažuriranje 0,3 odgovara verziji programa **10.0.10288.0**.

> [AZURE.NOTE] Ažuriranja su disruptive i ponovno pokrenite uređaj. Ako su/i u tijeku, uređaj uključuje isključiti.


## <a name="whats-new-in-the-update-03"></a>Što je novo u ažuriranje 0,3

Ažuriranje 0,3 je prvenstveno pogrešku popravak Sastavi. U ovoj verziji ste je adresirana nekoliko programskih pogrešaka rezultira sigurnosne kopije pogreške u prethodnoj verziji.

## <a name="issues-fixed-in-the-update-03"></a>Problemi s fiksiran ažuriranje 0,3

Sljedeća tablica sadrži sažetak problema fiksno u ovom izdanju.

| ne.  | Značajka                              | Problem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Sigurnosne kopije                                |Problem je vidio starije verzije koje zadovoljavaju sigurnosnih kopija da biste dovršili za zajedničko korištenje datoteka. Ako je došlo do taj problem, sigurnosno kopiranje zadovoljavaju i Kritično upozorenje je podignut na servis upravitelja StorSimple će obavijestiti korisnika. Taj se problem nije utječe na podatke na zajedničko korištenje ili pristup podacima. Uzrok je označena i fiksno u ovom izdanju. <br></br> Popravak nije primjenjiv retroactively zajedničke stavke koje već vide taj problem. Korisnici koji se prikazuje taj problem treba najprije primjene ažuriranja 0,3, a zatim obratite se Microsoftovoj Support za sigurnosno kopiranje cijelog sustava da biste riješili problem. Umjesto s Microsoft Support, korisnici možete i vratiti novi zajedničko korištenje iz sigurnosne kopije dobar za problematične zajedničke.                                                                                                                                                                                 |
| 2    | iSCSI                         | Problem je vidjeti u prethodnu verziju koju jedinice nestati prilikom kopiranja podataka glasnoću na polja virtualne StorSimple. Taj se problem riješen u ovom izdanju. <br></br> Popravke primjenjuje se samo na novostvorena količine. Popravke ne primjenjuju se retroactively jedinicama koje se već prikazuje taj problem. Korisnici su svakako Premjesti problematične jedinice Internetu putem portala za Azure klasični, sigurnosno te količine i vratiti te količine nove jedinice.                                                               |


## <a name="known-issues-in-the-update-03"></a>Poznati problemi u ažuriranje 0,3

Sljedeća tablica nudi sažetak poznatih problema za polja virtualne StorSimple i sadrži probleme s zabilježili izdanje iz prethodnog izdanja. 


| ne. | Značajka | Problem | Zaobilazno rješenje/komentara |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Ažuriranja | Virtualna uređaje stvorene u pretpregledu aplikacije nije moguće ažurirati u podržanu verziju Općenito dostupan. | Te virtualne uređaje mora biti nije uspjelo putem izdanju Općenito dostupan pomoću tijeka rada Izrada oporavak (DR). |
| **2.** | Dodjela resursa podatkovni disk | Nakon što ste dodjeli podatkovni disk određene veličine navedeni i stvorili odgovarajuće StorSimple virtualnog uređaja, morate ne proširivanje ili smanjivanje podatkovni disk. Pokušaj učinite rezultate gubitak podataka u lokalnom razine uređaja. |   |
| **3.** | Pravilnik grupe | Kada uređaj je domena pridružili, Primjena pravilnika grupe koji može negativno utjecati na operaciju uređaj. | Provjerite je li vaša virtualne polja je u vlastitom organizacijsku jedinicu (OU) za Active Directory i bez objekti pravilnika grupe (GPO) primjenjuju se na njega.|
| **4.** | Lokalni web korisničkog Sučelja | Ako bolja sigurnost značajke omogućene u pregledniku Internet Explorer (IE ESC), neke stranice lokalne web korisničkog Sučelja kao što su otklanjanje poteškoća ili održavanje možda neće pravilno funkcionirati. Gumbi na te stranice i možda neće funkcionirati. | Isključivanje značajke bolja sigurnost u pregledniku Internet Explorer.|
| **5.** | Lokalni web korisničkog Sučelja | U virtualnog računala za Hyper-V sučelje mreže na web-mjestu korisničkog Sučelja prikazuju se kao 10 Gbps sučelja. | To je odraz Hyper-V. Hyper-V uvijek prikazuje 10 Gbps za virtualne mrežnih prilagodnika. |
| **6.** | Tiered količine i zajedničko korištenje | Raspon bajtni zaključavanje za aplikacije koje funkcioniraju s StorSimple tiered jedinicama nisu podržani. Ako je omogućeno zaključavanje raspon bajta, StorSimple tiering ne funkcionira. | Preporučena mjere obuhvaćaju sljedeće: <br></br>Isključite bajtni raspon zaključavanje logike aplikacije.<br></br>Odaberite da biste postavili podaci za ovu aplikaciju lokalno prikvačene količine umjesto tiered količine.<br></br>*Caveat*: kada pomoću lokalno prikvačeni količine i zaključavanje raspon bajtni omogućena, lokalno prikvačene jedinica može biti online čak i prije dovršetka vraćanja. U takvim slučajevima, ako vraćanja je u tijeku, zatim morate pričekati da se vrati da biste dovršili. |
| **7.** | Tiered zajedničko korištenje | Rad s velikih datoteka može rezultirati sporo sloju odgovor. | Rad s velikih datoteka, preporučujemo da Najveća datoteka manja od 3% veličine zajedničko korištenje. |
| **8.** | Koristi kapacitet za zajedničko korištenje | Možda ćete vidjeti zajedničko korištenje potrošnju kada nema podataka na zajedničko korištenje. Ovo je jer korištenih kapaciteta za zajedničke sadrži metapodatke. |   |
| **9.** | Izrada oporavak | Možete izvršiti samo oporavak Izrada datotečnom poslužitelju za istu domenu kao izvor uređaja. Izrada oporavak ciljni uređaj u drugu domenu nije podržan u ovom izdanju. | To je implementirana u novije verzije. |
| **10.** | Azure PowerShell | Virtualna uređaje StorSimple nije moguće upravljati putem komponente PowerShell Azure u ovom izdanju. | Upravljanje virtualne uređaja računajući putem portala za Azure klasični i lokalne web korisničkog Sučelja. |
| **11.** | Promjena lozinke | Konzola za uređaj virtualne polja prihvaća samo unos u obliku tipkovnice hr-HR. |   |
| **12.** | CHAP | Nije moguće ukloniti vjerodajnice CHAP jednom stvorili. Osim toga, ako mijenjate CHAP vjerodajnice, morate izvanmrežni jedinice, a zatim ih Internetu se promjene primijenile. | Taj se problem s ljuskom sustava novije verzije. |
| **13.** | iSCSI poslužitelja  | Na "koristi za pohranu" prikazuje jedinici s iSCSI mogu se razlikovati u StorSimple Upravitelj servisa i iSCSI glavnog računala. | Glavno računalo iSCSI sadrži prikaz datotečnom sustavu.<br></br>Uređaj ne vidi blokovi dodijeliti kada je glasnoće na maksimalne veličine.|
| **14.** | Poslužitelj datoteka  | Ako je datoteka u mapi programa zamjenski podataka strujanje (REKLAMA) pridružena, na reklame nije sigurnosnu kopiju ili vratiti putem Izrada oporavak, Kloniraj i oporavak razinu stavke.| |


## <a name="next-step"></a>Sljedeći korak

[Instalirajte ažuriranje 0,3](storsimple-ova-install-update-01.md) na vašem StorSimple virtualne polja.

## <a name="references"></a>Reference

Tražite starije bilješke za izdanje? odi u: 

- [Bilješke o izdanju StorSimple virtualne polja ažuriranje 0.1 i 0,2](storsimple-ova-update-01-release-notes.md)

- [StorSimple virtualne polja Općenito dostupnost napomene](storsimple-ova-pp-release-notes.md)
 

