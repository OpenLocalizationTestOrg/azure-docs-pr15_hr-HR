<properties 
    pageTitle="Azure RemoteApp - testiranje vašeg korištenja propusnosti mreže s nekim uobičajeni scenariji | Microsoft Azure"
    description="Saznajte kako otprilike uobičajeni scenariji za korištenje koje omogućuju utvrđivanje vašim potrebama propusnosti mreže za Azure RemoteApp."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />
    
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp - testiranje vašeg korištenja propusnosti mreže s nekim uobičajeni scenariji

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Kao što smo se spominju u [korištenja propusnosti mreže za procjenu Azure RemoteApp](remoteapp-bandwidth.md), najbolji način za utvrđivanje utjecaj Azure RemoteApp s mrežom novosti da biste pokrenuli neke korištenje testira. Pokrenite te testira određeno vremensko razdoblje i mjere propusnosti potrebne za svaki scenarij. Ako imate mogućnost, možete mjeriti i u paketu gubitka i mrežne razrješava zahtjeve da biste shvatili mreže uzorke koje će se stvoriti u okruženju sustava određene.

    
Prilikom procjene korištenja propusnosti, imajte na umu da korištenje razlikuje između različitim korisnicima unutar tvrtke. Na primjer, tekst čitateljima i autorima obično zauzimaju manje propusnosti od korisnika koji rade s videozapis. Da biste postigli najbolje rezultate, proučavanje potrebama korisnika i stvorite kombinacije sljedećim scenarijima koja najbolje predstavlja korisnicima u svojoj tvrtki. Imajte na umu da biste [pregledali čimbenici koji korištenja propusnosti utjecaj i korisničko sučelje](remoteapp-bandwidthexperience.md) - koji će vam olakšati prepoznavanje idealna testova.

Najprije Saznajte više o testova, odaberite vaše mix, a zatim pokrenite. U tablici u nastavku možete koristiti da biste pratili performansi.

>[AZURE.NOTE] Ako ne možete sami testiranje mreže ili nemate vremena da biste to učinili, pogledajte naše [osnovne mreže procjenjuje propusnost/preporuke](remoteapp-bandwidthguidelines.md). Vaš potrošnje mogu se razlikovati, međutim, pa ako vlastite testira se *može* pokrenuti, trebali biste.


## <a name="the-usage-tests"></a>Korištenje testova
Svaki od tih testira pokretanje za različite količine vremena i testirati različite funkcije i značajke koje propusnost mreže. Imajte na umu da biste odabrali mix testa koji najbolje odgovara tvrtke za pojedinačne korisnike.
 
### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>PowerPoint izvršni/complex – pokretanje 900 1000 sekundi

Korisnik predstavlja između 45 50 visoka kvaliteta slajdova pomoću programa Microsoft Office PowerPoint u načinu prikaza na cijelom zaslonu. Slajdovi smiju sadržavati slike, prijelaza (s animacijama) i pozadina s prijelazom boja koji su uobičajeni za svoju tvrtku. Korisnik mora potrošeno najmanje 20 sekundi na svaki slajd.
    
Ovaj scenarij stvara bursty promet kada u slajd prijelaze na sljedeći slajd u prezentaciji.
    
### <a name="simple-powerpoint---run-for-620-seconds"></a>Jednostavan PowerPoint – pokretanje ~ 620 sekundi

Korisnik predstavlja jednostavan datoteke programa PowerPoint s više od 30 slajdova pomoću programa Microsoft Office PowerPoint u načinu prikaza na cijelom zaslonu. Slajdovi su više teksta ćete morati usko od u scenariju izvršni/kompleksnog PowerPoint i imaju jednostavniji pozadine i slike (crna dijagrami). 
    
### <a name="internet-explorer---run-for-250-seconds"></a>Internet Explorer – pokretanje ~ 250 sekundi

Pregleda weba pomoću programa Internet Explorer. Korisnik traži i pomicanje kroz kombinacije teksta, prirodnim slike i neke schematic dijagrama. Web-stranica koja je spremljena na lokalni pogon diska poslužitelja udaljene radne površine glavnog (RD glavnog) kao programa. MHT datoteke. Korisnik se pomiče pomoću stranica gore, stranica dolje, gore i dolje, s različitim intervalima za svaku ključ/vrstu klizača:
    
    - Dolje - 250 pritisaka na tipke vrlo 500 ms
    - Page Up - 36 pritisaka na tipke svakih 1000 ms
    - Dolje - 75 pritisaka na tipke svakih 100 ms
    - Page Down - 20 pritisaka na tipke svaki 500 ms
    - Najviše - 120 pritisaka na tipke svaki 300 ms
    
### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF dokument - jednostavne – pokretanje ~ 610 sekundi
Korisnik čita i pretražuje PDF dokument na različite načine pomoću programa Adobe Acrobat Reader. Dokument mora se sastojati od tablica, sljedeća i više fontove. Dokument je 35 40 stranica dugi. Korisnik krećete kroz na dva različita učestalost unatrag i prosljeđuje s četiri veličinama zumiranja (Prilagodi stranicu, Prilagodi širinu, 100%, a drugi vašem odabiru). Na zumiranje osigurava da se tekst (font) prikazuje u različitim veličinama. Pomicanje nije dostupno korištenje stranica gore, stranica dolje, gore i dolje, s različitim interval za svaku klizača.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>PDF dokumenata - kombiniranim - cilja ~ 320 sekundi
Korisnik čita i pretražuje PDF dokument na različite načine pomoću programa Adobe Acrobat Reader. Dokument se sastoji od slike visoke kvalitete (uključujući fotografije), tablice, sljedeća i više fontove. Korisnik krećete kroz na dva različita učestalost unatrag i prosljeđuje s četiri veličinama zumiranja (Prilagodi stranicu, Prilagodi širinu, 100%, a drugi vašem odabiru). Na zumiranje osigurava da se tekst (font) prikazuje u različitim veličinama. Pomicanje nije dostupno korištenje stranica gore, stranica dolje, gore i dolje, s različitim interval za svaku klizača.

### <a name="flash-video-playback---run-for-180-seconds"></a>Flash video reprodukcija – pokretanje ~ 180 sekundi
Korisnik prikaze programa Adobe Flash kodirani videozapis ugrađen u web-stranicu. Web-stranicu pohranjen na lokalni disk RD glavnog poslužitelja. Videozapis se reproducira u programu Internet Explorer tako da je dodatak ugrađene reproduktora.

Ovaj scenarij oponaša korisnika koji pregledavaju obogaćenog sadržaja web-stranice koje sadrže multimedijske. Većina podatke trebali biste kvir kroz VOBR.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Word udaljene pisati – pokretanje ~ 1800 sekundi
Korisnik upisuje dokumenta putem sesiju RDP. Pritisaka na tipke šalju na strani klijenta do RDP sesije u dokument u programu Microsoft Word izvodi u udaljenu sesiju. Pisanje rate je svaki 250 ms (ukupan broj znakova za 7050) za jedan znak. 

To je jedna od Najčešći scenariji znanja tempiranja. Ovaj scenarij testira odziv upišete u procesor Moderna rad korisnika. Taj se scenarij osjetljive čak i male promjene u korištenja propusnosti.

## <a name="tracking-the-test-results"></a>Rezultati provjere za praćenje

U sljedećoj su tablici možete koristiti da biste procijenili scenariji u svom okruženju. Podaci navedene u nastavku su samo za ilustracija – možda je vastly razlikuje od što pridržavajte. 

Zbog jednostavnosti, ne možemo pretpostavlja da sve scenarije testiraju koristeći istu razlučivost zaslona 1920 x 1080 piksela i TCP prijenosa na mreži s Latencija (Odgoda) ispod 200 ms i mrežne jitter u oznaku 120 ms + oko 1%.

O tablici:
- **Prosjek sučelje** sadrži propusnost mreže kojima produktivnost korisnika nije znatno utjecati ali isključi Povremeni problemčiće zvučnog ili videozapisa. Sustav je moguće oporaviti brzo prihvaćanjem prednost dinamički logike. Mrežni propusnosti procjenjuje pokušaj jamči kvalitete korisničkog sučelja.
 - **Noticeable problemi (prijelom točku)** sadrži propusnost mreže gdje korisnici mogli biste primijetiti značajan problemi u svom korisničkom iskustvu i njihovih produktivnost utječe brzom vremenskih razdoblja. Sada algoritama RDP su struggling, a ne jamči korisnika kvalitetu sučelja zbog propusnost mreže dovoljno.
 - **Preporučeno** sadrži propusnost mreže preporučuje dobar ili izvrstan iskustvo. To je obično jedan korak veća od vrijednosti u stupcu za odgovarajuće **prosjek sučelje** .
 - **Bilješke** obuhvaćaju opažanja i komentare.
 
| Test                  | Prosječna sučelje | Uočljivijih problema (prijelom točku) | Propusnost mreže preporučene | Bilješke                                                              |
|-----------------------|--------------------|---------------------------------|-------------------------------|--------------------------------------------------------------------|
| Izvršni/kompleksnog PPT | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s Preferirani    | U 1 MB/s mnogo animacija gube se                                   |
| Jednostavan PPT            | 5 MB/s              | 256 KB/s                         | 10 MB/s                        | Pri 256 KB/s slajdove učitati uočljivijih odgode                   |
| Internet Explorer     | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s Preferirani    | U 1 MB/s web videozapisi su mutno i isprekidan, brzog klizanja ima poteškoća |
| Jednostavan PDF-a            | 1 MB/s              | 256 KB/s                         | 5 MB/s                         | Pri 256 KB/s potrebno neko vrijeme da biste učitali stranice                       |
| Koriste različite verzije PDF-a             | 1 MB/s             | 256 KB/s                         | 5 MB/s                         | Pri 256 KB/s stranici vodi na dosta vremena potrebnog za učitavanje    |
| Flash reprodukcije videozapisa  | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s Preferirani    | U 1 MB/s je zrnato videozapis, a neke okvira ispuštaju           |
| Word udaljene pisati    | 256 KB/s            | 128 KB/s                         | 1 MB/s                         | Pri 256 KB/s korisnik primijetiti vremena između pritisaka na tipke             |

Da biste procijenili propusnost mreže po korisniku, stvorite kombinacije iznad scenariji i odgovarajuće proporcije propusnost potrebnu mrežu. Odaberite najveći broj koja su potrebna za vašim scenarijima. Budući da korisnici gotovo nikad neće koristiti samostalno sustava, razmislite o nekim rezervnih za korisnike koji istovremeno raditi na istoj mreži.
     
## <a name="learn-more"></a>uči više
- [Procjenu korištenja propusnosti mreže Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp – kako učinite propusnost mreže i kvalitete iskustvo rada zajedno?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp propusnost mreže - opće smjernice (Ako ne možete testirati vlastite)](remoteapp-bandwidthguidelines.md)