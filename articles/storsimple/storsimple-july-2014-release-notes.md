<properties 
   pageTitle="Pustite 8000 StorSimple verziju napomene | Microsoft Azure"
   description="Opisuje novih značajki, otvorene teme i dostupne zaobilazna rješenja za u srpnju 2014 Microsoft Azure StorSimple izdanje."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>Napomene u verziji pustite StorSimple 8000 niz - srpanj 2014. 

## <a name="overview"></a>Pregled

Napomene prati prepoznati kritičnih problema Otvori niz 8000 StorSimple srpanj 2014 Općenito dostupan (GA) izdanju sustava Microsoft Azure StorSimple. U ovom izdanju odgovara verzija softvera 6.3.9600.17215.  

Ako je naveden u suprotnom, te napomene Primijeni na sve modelima StorSimple uređaja. Napomene u neprestano ažuriraju; kao što su otkrio kritičnih problema koje je obavezna zaobilazno rješenje, ona se dodaju. Prije implementacije rješenja sustava Microsoft Azure StorSimple, razmotrite sljedeće informacije.  

## <a name="known-issues-in-this-release"></a>Poznati problemi u ovom izdanju
Sljedeća tablica daje sažetak Poznati problemi u ovom izdanju.  
 
| ne. | Značajka | Problem | Komentari/zaobilazno rješenje | Odnosi se na fizički uređaj | Odnosi se na virtualnog uređaja |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Ponovno postavljanje tvorničke | U nekim slučajevima prilikom izvršavanja tvorničke Vrati izvorne postavke uređaja StorSimple ostaju i prikazati ova poruka: **Vrati tvorničke je u tijeku (faza 8)**. To se događa ako pritisnete kombinaciju tipki CTRL + C dok cmdlet je u tijeku. | Pritisnite CTRL + C nakon pokretanja tvorničke Vrati izvorne postavke. Ako se već nalaze u tom stanju, obratite se Microsoft Support za sljedeće korake. | Da | ne |
| 2 | Kvorum disk | U rijetko slučajevima, ako Većina diskova s prilozima EBOD 8600 uređaja niste povezani rezultira kvorum bez diska pa na resurse za pohranu bit će izvanmrežno. On će ostati izvanmrežne čak i ako na diskova se ponovno povezati sa sustavom. | Morat ćete ponovno pokrenite uređaj. Ako se problem nastavi pojavljivati, obratite se Microsoft Support za sljedeće korake. | Da | ne |
| 3 | Oblak snimku stanja pogreške | U rijetko slučajevima, snimka oblaka možda neće funkcionirati uz poruku o pogrešci **maksimalno ograničenje sigurnosne kopije dosegne**. To se događa ako prelaziti 255 clones online na istom uređaju s istom izvornu glasnoću koji je izbrisan. | | Da | Da |
| 4 | ID netočan kontrolera | Zamjena kontroler provodi, kontroler 0 možda prikazuju se kao kontroler 1. Tijekom zamjenski kontroler prilikom učitavanja slike iz čvor ravnopravnih članova ID kontrolera može prikazivati na početku kao kontrolerom ravnopravnih članova ID-a. U rijetko instancama takvo ponašanje mogu se vidjeti i nakon ponovnog pokretanja sustava. | Potreban je bez korisničku akciju. U ovom slučaju će riješiti sam nakon zamjenski kontroler. | Da | ne |
| 5 | Uređaj nadzor grafikoni | U servisu StorSimple Upravitelj grafikoni nadzora uređaj ne funkcioniraju kad je osnovni ili NTLM je omogućena provjera autentičnosti u konfiguraciji proxy poslužitelja za uređaj. | Izmjena web-konfiguracija proxy poslužitelja za uređaj registrirani sa servisom za Upravitelj StorSimple stoga autentičnosti postavljena na nema. Da biste to učinili, pokrenite sustava Windows PowerShell za postavljanje HcsWebProxy StorSimple cmdlet. | Da | Da |
| 6 | Računi za pohranu | Putem servisa za pohranu da biste izbrisali račun za pohranu je nepodržane scenarij. To će dovesti do situacije u kojima ne može dohvatiti podatke o korisniku. | | Da | Da |
| 7 | Failback | Failback roku od 24 sata Izrada oporavak (DR) nije podržano. | | Da | ne |
| 8 | Prebacivanje na uređaju | Više failovers glasnoću spremnika na istom uređaju izvora različite ciljnih nije podržana. Prebacivanje s jednog reagira uređaja na više uređaja će postati spremnika glasnoće na prvom nije uspjela putem uređaja izgubiti vlasništvo nad podacima. Nakon takve prebacivanje te spremnika glasnoću će prikazuju se ili se drugačije ponašaju prilikom prikaza na portalu za Azure klasični. | | Da | ne |
| 9 | Instalacija | Tijekom StorSimple prilagodnik za instalaciju sustava SharePoint, morate unijeti IP uređaj za instalaciju da biste uspješno završili. | | Da | ne |
| 10 | Sučelje mreže | Mrežni sučelja podataka 2 i 3 podataka nisu zamjenjuju softvera. | Obratite se Microsoft Support Ako morate konfigurirati ta sučelja. | Da | ne |


 
