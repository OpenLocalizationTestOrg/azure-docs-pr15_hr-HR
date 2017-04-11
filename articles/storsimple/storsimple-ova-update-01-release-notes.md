<properties 
   pageTitle="Napomene o StorSimple virtualne polja ažuriranja | Microsoft Azure"
   description="Opisuje kritičnih Otvori problema i rješenja za polja virtualne StorSimple radi ažuriranja 0,2 i 0,1."
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
   ms.date="06/16/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>Napomene za ažuriranje polja virtualne 0,2 i 0,1 StorSimple

## <a name="overview"></a>Pregled

Sljedeće napomene identificirati kritičnih problema Otvori i riješi probleme ima li ažuriranja za Microsoft Azure StorSimple virtualne polja. (Microsoft Azure StorSimple virtualne polja nije poznat i kao virtualnog uređaja za lokalni StorSimple ili virtualnog uređaja StorSimple.) 

Napomene u neprestano ažuriraju i kao prepoznate kritičnih problema koje je obavezna zaobilazno rješenje se dodaju. Prije implementacije virtualnog uređaja StorSimple pažljivo pregledajte informacije koje se nalaze u napomenama u.

Ažuriranje 0,2 odgovara na softver verzije **10.0.10280.0**; Ažuriranje 0,1 je verzija **10.0.10279.0**. U odjeljcima navedeni promjene za svako ažuriranje. 

> [AZURE.NOTE] Ažuriranja su disruptive i će ponovno pokrenite uređaj. Ako su/i u tijeku, uređaj će uzrokovati isključiti.

## <a name="issues-fixed-in-the-update-02"></a>Problemi s fiksiran ažuriranje 0,2
Ažuriranje 0,2 obuhvaća sve promjene Update 0,1 osim popravak što je opisano u sljedećoj tablici:

Značajka                              | Problem                                                                                                                                                                                                                                                                                                                           |
--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
Ažuriranja                                 | U zadnji izdanju nisu otkriju ažuriranja automatski Azure klasični portalu da bi morali ste instalirati ažuriranja pomoću lokalne korisničko Sučelje Web. Taj se problem ne riješi u ovom izdanju. Nakon instaliranja ažuriranja 0,2, možete instalirati buduća ažuriranja pomoću portala za Azure klasični.                       

## <a name="whats-new-in-the-update-01"></a>Što je novo u ažuriranje 0,1

Ažuriranje 0,1 sadrži sljedeće popravci programskih pogrešaka i poboljšanja. 

- **Improved otpornost za oblak kvarove**: Ovo izdanje sadrži nekoliko popravci programskih pogrešaka oko Izrada oporavak sigurnosne kopije, Vrati i tiering slučaju prekidu za povezivanje oblaka. 

- **Improved vratili performanse**: Ovo izdanje sadrži popravci programskih pogrešaka koje su znatno izrezivanje put dovršetku vraćanja zadataka.

- **Automated prostor reclamation optimizacije**: nakon brisanja podataka na thinly dodijeljenu jedinicama blokovi Neiskorišteni prostor za pohranu potrebno se povratiti. Ovo izdanje sadrži poboljšani postupka reclamation prostor iz oblaka rezultira Neiskorišteni prostor postaje dostupan brže to prethodne verzije.

- **Nove slike virtualne diska**: novi VHD, VHDX i VMDK Zasad su dostupni putem portala za Azure klasični. Možete preuzeti te slike dodjela novih ažuriranja 0,1 uređaja.

- **Poboljšanje točnost status zadataka na portalu**: U starijoj verziji programa stanja zadatka izvješćivanje o pogreškama na portalu nije zrnastog. Taj se problem riješen u ovom izdanju.

- **Sučelje domene spoja**: popravci programskih pogrešaka vezane uz domene uključivanja i preimenovanja uređaja.


## <a name="issues-fixed-in-the-update-01"></a>Problemi s fiksiran ažuriranje 0,1

Sljedeća tablica sadrži sažetak problema fiksno u ovom izdanju.

| ne.  | Značajka                              | Problem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | VMDK                                 | U nekim verzijama VMware OS disk je prikazivati kao kratke uzrokuje upozorenja i bavljenje normalno. To je fiksno u ovom izdanju.                                                                                                                                                                                    |
| 2    | iSCSI poslužitelja                         | U zadnji izdanju korisnika je potreban da biste odredili pristupnik za svaki omogućeni mrežni sučelje StorSimple virtualnog uređaja. Taj se problem se mijenja u ovom izdanju tako da korisnik ima da biste konfigurirali barem jedan pristupnik za sva sučelja omogućeni mrežni.                                                                              |
| 3    | Paket za podršku                      | U starijoj verziji programa zbirka paketa podrška nije uspjela kada su veličina paketa većim od 1 GB. Taj se problem ne riješi u ovom izdanju.                                                                                                                                                                               |
| 4    | Pristup putem oblaka                         |  U zadnji izdanju ako polja virtualne StorSimple ne sadrži veza s mrežom i ponovno pokrenut, lokalni korisničkog Sučelja promijenile problema s povezivanjem. Ovaj problem ne riješi u ovom izdanju.                                                                                                                            |
| 5    | Nadzor grafikoni                    | U prethodnog izdanja, pratiti prebacivanje na uređaju u oblak kapaciteta Upotreba grafikoni prikazuju neispravnih vrijednosti na portalu za Azure klasični. To je fiksiran u trenutnom izdanju.                                                                                                                          |



## <a name="known-issues-in-the-update-01"></a>Poznati problemi u ažuriranje 0,1

Sljedeća tablica nudi sažetak poznatih problema za polja virtualne StorSimple i sadrži probleme s zabilježili izdanje iz prethodnog izdanja. **Izdanje problema navedene u ovom izdanju su označene zvjezdicom. Gotovo svi problemi na ovom popisu imaju prenose iz izdanja GA StorSimple virtualne polja.**


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
| **14.** | Datoteke poslužitelja *  | Ako je datoteka u mapi programa zamjenski podataka strujanje (REKLAMA) pridružena, na reklame nije sigurnosnu kopiju ili vratiti putem Izrada oporavak, Kloniraj i oporavak razinu stavke.| |


## <a name="next-step"></a>Sljedeći korak

[Instaliranje ažuriranja](storsimple-ova-install-update-01.md) sustava StorSimple virtualne polja.
