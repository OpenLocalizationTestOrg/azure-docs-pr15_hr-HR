<properties 
    pageTitle="Integriranje aplikacije s Azure virtualne mreže" 
    description="Prikazuje kako povezati aplikaciju u aplikacije servisa za Azure novi ili postojeći Azure virtualne mreže" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor="cephalin"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="ccompy"/>

# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Pokrenite aplikaciju integrirati Azure virtualne mreže #

Ovaj dokument opisuju značajke za integraciju aplikacije servisa za Azure virtualne mreže i pokazuje kako ga postavili s aplikacijama na [Servisu Azure aplikacije](http://go.microsoft.com/fwlink/?LinkId=529714).  Ako niste upoznati s Azure virtualne mreže (VNETs), to je mogućnost koja omogućuje da biste smjestili mnoge Azure resurse koji nisu internet routeable mrežom koja kontrolu pristupa.  Te mreže moguće je povezati pa na lokalnu instancu mreža pomoću raznih VPN tehnologije.  Da biste saznali više o Azure virtualne mreže započnite ovdje informacije: [Azure virtualne mreže pregled][VNETOverview].  

Aplikacije servisa Azure ima dva oblika.  

1. Sustavi za više klijentu koji podržavaju široka lepeza cijene tarife
1. Značajka premium okruženje servisa aplikacija elika (i mala slova) koji implementira u vašem VNET.  

Ovaj dokument prolaze kroz VNET integracije i ne aplikacije servisa okruženju.  Ako želite dodatne informacije o značajci elika i mala slova, a zatim započnite ovdje informacije: [Uvod u okruženje za aplikaciju servisa][ASEintro].

Integracija VNET omogućuje pristup web app resursa u virtualne mreže, ali ne dopušta privatne pristup na web-aplikaciju iz virtualne mreže.  Pristup privatnih web-mjesta dostupan je samo pomoću programa elika i mala slova konfiguriran pomoću programa Interna učitavanja opterećenja (ILB).  Informacije o korištenju programa ILB elika i mala slova, započnite u članku: [Stvaranje i korištenje programa elika i mala slova ILB][ILBASE]. 

Uobičajeni scenarij kojemu želite koristiti VNET Integracija je omogućen pristup iz web-aplikacije na bazu podataka ili web-servisi koji se izvode na virtualnog računala u vašoj mreži Azure virtualne.  Uz integraciju VNET ne morate izlažu javno krajnja točka za aplikacije na vašem VM, ali može umjesto toga koristite privatne koje nisu internetske usmjeravati adrese.  

Značajka integracije VNET:

- potreban je standardna ili Premium cijene plan 
- funkcioniraju s Classic(V1) ili VNET Manager(V2) resursa 
- podržava TCP i UDP
- radi s aplikacijama Web, Mobile i API-JA
- omogućuje aplikaciju možete povezati samo 1 VNET odjednom
- omogućuje do 5 VNETs za integraciju sa u Plan za aplikaciju servisa 
- omogućuje isti VNET se može koristiti u više aplikacije u Plan za aplikaciju servisa
- podržava SLA 99.9% zbog na oslanjanje na lebdeće pokazivače VNET pristupnika

Evo nekoliko stvari koje VNET Integracija ne podržava uključujući:

- Postavljanje pogona
- Integracija AD 
- NetBios
- pristup privatnih web-mjesta

### <a name="getting-started"></a>Početak rada ###
Evo nekih Napomena koje valja imati na umu prije povezivanja web-aplikaciju programa virtualne mreže:

- Integracija VNET funkcionira samo s aplikacijama u **standardni** ili **Premium** cijene plan.  Ako omogućite značajku i skaliranje aplikacije servisa tarifa nepodržane cijene tarifu aplikacije da biste odbacili svoje veze da biste VNETs koriste.  
- Ako već postoji cilj virtualne mreže, morate imati točke web VPN omogućena za dinamičku pristupnik za usmjeravanje prije nego što je moguće je povezati aplikacije.  Točka web virtualne privatne mreže (VPN) ne možete omogućiti ako pristupnikom nije konfiguriran za korištenje statične usmjeravanja.
- Na VNET moraju biti iste pretplate kao vaše aplikacije servisa Plan(ASP).  
- Aplikacijama koje se integrirati s VNET koristit će DNS koje je navedeno za tu VNET.
- Po zadanom integrating aplikacija će samo usmjeriti promet na vaše VNET na temelju usmjerava koji su definirani u vašem VNET.  


## <a name="enabling-vnet-integration"></a>Omogućivanje VNET Integracija ##

Ovaj dokument namijenjen je prvenstveno pomoću portala za Azure za integraciju VNET.  Da biste omogućili integraciju VNET s aplikacijom pomoću komponente PowerShell, slijedite upute u nastavku: [Povezivanje aplikacije s mrežom virtualne pomoću komponente PowerShell][IntPowershell].

Imate mogućnost povezati aplikacije novi ili postojeći virtualne mreže.  Ako stvorite novu mrežu kao dio vašeg Integracija zatim osim stvaranja na VNET, dinamični pristupnik za usmjeravanje neće biti unaprijed konfigurirana za vas i pokažite na web-mjesta VPN će biti omogućene.  

>[AZURE.NOTE] Konfiguriranje nove Integracija virtualne mreže može potrajati nekoliko minuta.  

Da biste omogućili integraciju VNET Otvori aplikacije postavke, a zatim odaberite mreže.  Korisničkog Sučelja koji se otvara nudi tri umrežavanje izbora.  Ovaj vodič samo će u VNET Integracija kroz hibridnog veze, a zatim okruženja aplikacije servisa se spominju kasnije u ovom dokumentu.  

Ako aplikacija se nije u ispravno cijene plan korisničkog Sučelja će helpfully omogućuju skaliranje tarifa veći tarifu za cijene po izboru.


![][1]
 
###<a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Omogućivanje VNET Integracija s postojećih VNET###
Korisničko Sučelje za integraciju VNET omogućuje vam da biste odabrali s popisa na VNETs.  Klasični VNETs označava da su kao što su riječju "Klasični" u zagrade pokraj naziva VNET.  Na popisu se sortiraju tako da na početku se navode VNETs Voditelj resursa.  Na slici u nastavku možete vidjeti samo jedan VNET može biti odabran.  Postoji više mogućih razloga da je VNET će biti zasivljen uključujući:

- u VNET je u drugu pretplatu računa ima pristup
- u VNET ne sadrži točke omogućen web-mjesta
- na VNET nema dinamički usmjeravanje pristupnika


![][2]

Da biste omogućili integraciju jednostavno kliknite VNET koje želite integrirati s.  Nakon što odaberete u VNET, aplikacija će se automatski ponovno pokrenuti da bi promjene stupile na snagu.  

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Omogućivanje pokažite na web-mjesta u klasični VNET #####
Ako vaš VNET nema pristupnika niti je točke na web-mjesto imate koji postaviti prvo.  Da biste to učinili za klasični VNET, idite na [Portal za Azure] [ AzurePortal] te otvorite popis virtualne Networks(classic).  Ovdje kliknite na mreži želite integrirati s, a zatim kliknite velike okviru u odjeljku Osnove naziva VPN veza.  Na tom mjestu možete stvoriti točku za VPN-a web-mjesta i čak i njezino stvaranje pristupnika.  Nakon što proći kroz točke na web-mjesto s doživljaj stvaranja pristupnika bit će otprilike 30 minuta prije nego što je spremna.  

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Omogućivanje pokažite na web-mjesta u resursima VNET #####

Da biste konfigurirali resursima VNET pristupnika i pokažite na web-mjesta potrebno za korištenje ljuske PowerShell navedenih u nastavku, [Konfiguriranje veze točke web virtualne mreže pomoću komponente PowerShell][V2VNETP2S].  Korisničko Sučelje da biste izvršili tu mogućnost još nije dostupna. 

### <a name="creating-a-pre-configured-vnet"></a>Stvaranje unaprijed konfigurirana VNET ###
Ako želite da biste stvorili novi VNET koji je konfiguriran za korištenje pristupnika i zareza web aplikacije servisa umrežavanje korisničkog Sučelja ima mogućnost da biste to učinili ali samo za upravitelj resursa VNET.  Ako želite stvoriti klasični VNET pristupnika i zareza web morate učiniti ručno putem korisničkog sučelja s mrežom. 

Da biste stvorili VNET resursima putem korisničkog Sučelja za integraciju VNET, samo odaberite **Stvaranje nove virtualne mreže** i navedite na:

- Virtualna Network Name
- Virtualne mreže Adresni blok
- Naziv podmreže
- Podmreže Adresni blok
- Pristupnik Adresni blok
- Točka web Adresni blok

Ako želite da se ovaj VNET povezati na bilo koje druge mreže pa izbjegavajte izdvajanje prostor IP adrese koja se preklapa s njima.  

>[AZURE.NOTE] Stvaranje VNET resursima pomoću pristupnika traje 30 minuta i trenutno će ne Integracija s VNET s aplikacijom.  Nakon vaše VNET stvara se pomoću pristupnika morate vratite se u aplikaciju korisničkog Sučelja za integraciju VNET, a zatim odaberite svoje nove VNET.

![][3]

Azure VNETs obično se stvaraju unutar adrese privatne mreže.  Prema zadanim postavkama integraciju sa servisom VNET značajka će sve promet usmjerili namijenjene prikazivanju na te rasponi IP adresa u vašem VNET.  Privatni rasponi IP adresa su:

- 10.0.0.0/8 – to je isti kao 10.0.0.0 - 10.255.255.255
- 172.16.0.0/12 – to je isti kao 172.16.0.0 - 172.31.255.255 
- 192.168.0.0/16 – to je isti kao 192.168.0.0 - 192.168.255.255
 
Prostor za adrese VNET mora da bude navedena u CIDR notacije.  Ako niste upoznati s CIDR notacija, to je način za određivanje adresni blokovi pomoću IP adresa i cijeli broj koji predstavlja mrežne maske. Kao vodič kroz, preporučuje se da 10.1.0.0/24 bio 256 adrese i 10.1.0.0/25 bio 128 adrese.  IPv4 adrese pomoću programa /32 bi samo 1 adresa.  

Ako ovdje postavite DNS poslužitelj podaci pa koji će biti postavljen za vaše VNET.  Nakon stvaranja VNET možete urediti te podatke iz bogatih VNET.

Kada stvorite klasični VNET pomoću korisničkog Sučelja za integraciju VNET, stvorit će na VNET u istoj grupi resursa kao aplikacije. 

## <a name="how-the-system-works"></a>Kako funkcionira u sustavu ##
U odjeljku na naslovnice značajka sastavlja pri vrhu točke web VPN tehnologija povezati aplikacije vaše VNET.   Aplikacije u aplikacije servisa za Azure imati više klijenta sustava arhitektura koji precludes dodjeljivanje aplikacije izravno u na VNET kao obavlja s virtualnim računalima.  Radom tehnologije točke na web smo ograničiti mrežni pristup samo virtualnog računala nalaze aplikacije.  Pristup mreži dodatno je ograničen na te aplikacije domaćini tako da aplikacije mogu pristupiti samo mrežama za konfiguriranje dopustiti pristup.  

![][4]
 
Ako niste konfigurirali DNS poslužitelj s mrežom virtualne morat ćete koristiti IP adresa.  Prilikom korištenja IP adrese, imajte na umu da glavnih prednosti značajka je koji omogućuje korištenje Privatna adresa unutar privatne mreže.  Ako ste postavili aplikacije koristi javnu IP adrese za jednu od vašeg VMs ne koristite značajku VNET integracije i su komunikaciju putem Interneta.


##<a name="managing-the-vnet-integrations"></a>Upravljanje integracije VNET##

Mogućnost za povezivanje i prekidanje veze da biste na VNET je na razini aplikacije.  Operacije koji mogu utjecati na integraciju sa servisom VNET svim aplikacijama više su na razini ASP.  U korisničkom Sučelju koji je prikazan na razini aplikacije možete dohvatiti pojedinosti na vašem VNET.  Većina iste podatke prikazuje se i na razini ASP.  

![][5]

Na stranici stanje mreže značajku možete vidjeti ako aplikacije je povezano s vašeg VNET.  Ako je vaš pristupnik VNET prema dolje za ono razloga pa to želite prikazati kao ne vezom.  

Podaci koji se sada je dostupna u aplikaciji razine korisničkog Sučelja za integraciju VNET je isti kao detaljne informacije zatražite od ASP.  Evo tih stavki:

- Otvorit će se naziv VNET - ove veze s mrežom korisničkog Sučelja
- Mjesto - odražava mjesto vašeg VNET.  Nije moguće integrirati s VNET na neko drugo mjesto.
- Status potvrde - postoje certifikate koji se koristi za zaštitu VPN veza između aplikacije i u VNET.  To odražava probno da biste bili sigurni da su u sinkronizaciji.
- Stanje pristupnika - vaše pristupnika treba dolje bilo razloga zatim aplikacije ne može pristupiti resursa u na VNET.  
- VNET adresni prostor – to je prostor za IP adresa za vaše VNET.  
- Pokažite na prostor adrese web-mjesta – to je pokažite na web-mjesta IP adresa prostor za vaše VNET.  Pokrenite aplikaciju vode komunikacije kao proizašla iz jednog u IP-ovi u taj prostor adresu.  
- Web-mjesta da biste prostor adrese web-mjesta – VPN-ovi web-mjesta za web-mjesta možete koristiti da biste se povezali vaše VNET na lokalnu instancu resursa ili drugih VNETs.  Mora imati konfiguriran, a zatim IP rasponi definirana pomoću će ovdje prikazati VPN veza.
- DNS poslužitelji – ako imate DNS poslužitelji konfiguriran za korištenje na VNET, a zatim su navedene u nastavku.
- Usmjeriti VNET - tamo IP-ovi su popis IP adresa ima li vaš VNET usmjeravanje definirali za.  Adrese prikazivat će se ovdje.  

Samo operacije koje možete poduzeti u prikazu aplikacije za integraciju sustava VNET je da biste prekinuli aplikacije VNET trenutno povezano s.  Kako to jednostavno kliknite Prekini vezu na vrhu.  Time se mijenja vaše VNET.  U VNET i konfiguracija uključujući pristupnika ostaje nepromijenjen.  Ako pa želite izbrisati vaše VNET morate najprije izbrišite resursa u njoj uključujući pristupnika.  

Prikaz Plan za aplikaciju servisa sadrži brojne dodatne operacije.  To je pristupa i drugačije nego iz aplikacije.  Da biste dosegne umrežavanje ASP korisničkog Sučelja jednostavno otvorite ASP korisničkog Sučelja i pomaknite se prema dolje.  Postoji korisničkog Sučelja element naziva stanje mreže značajke.  Omogućit će manji Detalji oko vaše VNET integracija.  Klikom na ovom korisničkog Sučelja otvara se korisničkog Sučelja Status značajka mreže.  Ako se zatim "Kliknite ovdje da biste upravljali" će se otvoriti gore korisničkog Sučelja koja sadrži popis integracije VNET u ovom ASP.

![][6]

Dobro Imajte na umu prilikom pregledavanja mjesta VNETs su Integracija sa je mjesto ASP.  Kada je na drugom mjestu u VNET vam vjerojatno puno više potražite u članku problemi Latencija.  

VNETs integriran s je podsjetnik na koliko VNETs aplikacija integriran s ovom ASP i koliko možete imati.  

Da biste vidjeli dodatne detalje o svakom VNET, samo kliknite VNET koja vas zanima.  Osim detalje koji su navedene neke starije verzije vidjet ćete i popis aplikacija u ovom ASP koji koriste tu VNET.  

Vezana uz akcije postoje dvije primarnog akcije.  Prvi je mogućnost dodavanja usmjerava koji utječu na promet ostavite aplikacije u vašem VNET.  Druge akcije je mogućnost da biste sinkronizirali potvrde i podatke o mreži.

![][7]

**Usmjeravanje** Kako je navedeno ranije usmjerava koji su definirani u vašem VNET su čemu služi koja usmjeruje promet na vaše VNET iz aplikacije.  Postoje neke upotrebe iako Uvjet gdje korisnici želite poslati dodatne odlazni promet aplikacije u VNET i za njih tu mogućnost.  Što se događa s promet nakon toga najviše kako je korisnik odabrao konfigurira njihove VNET.  

**Certifikati** Status potvrde odražava provjere provodi aplikacije servis za provjeru valjanosti jesu li certifikate koji se ne možemo koristite za VPN veza i dalje dobro.  Kada VNET Integracija omogućen, pa ako je ovo prvi Integracija za tu VNET iz aplikacija u ovom ASP postoji potreban exchange certifikata osigurava vezu.  Uz certifikate ćemo dobiti konfiguriranje DNS-a, usmjerava i druge slične stvari koje opisuju mreže.
Ako se promijeni te bonovima ili podatke o mreži zatim morat ćete kliknite "Sinkroniziraj mreža".  **Napomena**: kada kliknete "Sinkroniziraj mreže", a zatim će uzrokovati kratak prekida u povezivanje između aplikacije i vaše VNET.  Dok će ponovno pokrenuti aplikaciju gubitak povezivanje uzrokovati web-mjesta nije funkcija ispravno.  

##<a name="accessing-on-premise-resources"></a>Pristup na lokalnu instancu resursi##

Jedna od prednosti značajki integracije VNET je koji ako je vaš VNET povezani s mrežom na lokalnu instancu programa s web-mjesta za web-mjesta VPN-a, a zatim aplikacija možete imati pristup na lokalnu instancu resursa iz aplikacije programa.  Kako bi to raditi možda ćete morati ažurirati na lokalnu instancu pristupnikom VPN smjerovima vaše točke na web-mjesta IP raspona.  Kada prvo postavljanje VPN-a web-mjesta za web-mjesta skripte za konfiguriranje je potrebno postaviti tako usmjerava uključujući točku za VPN-a web-mjesta.  Ako dodate točke VPN-a web-mjesta nakon vaše stvaranje web-mjesta za web-mjesta VPN-a, a zatim ćete morati ručno ažurirati smjerovima.  Pojedinosti o tome kako to učiniti razlikovat će se po pristupnika i su opisane u nastavku.  

>[AZURE.NOTE] Dok je značajka integracije VNET funkcioniraju s VPN-a web-mjesta za web-mjesta da biste pristupili lokalnu resursa je trenutno neće funkcionirati s ExpressRoute VPN-a u isto.  To se događa kada Integracija sa Classic ili Voditelj resursa VNET.  Ako vam je potrebna pristup izvorima putem programa VPN ExpressRoute možete koristiti za elika i mala slova koji se može pokrenuti u vašem VNET. 

##<a name="pricing-details"></a>Informacije o cijenama##
Postoji nekoliko cijene nuances koji ste trebali imati na umu prilikom korištenja značajke za integraciju VNET.  Postoje 3 povezane naknade za korištenje ove značajke:

- ASP cijene sloju preduvjeti
- Troškovi prijenos podataka
- Troškovi VPN pristupnika.

Za aplikacije da biste mogli koristiti ovu značajku, moraju biti u standardnu ili Premium aplikacije servisa planiranje.  Vidjet ćete dodatne detalje na tim troškovima: [Aplikacija rezervnih][ASPricing]. 

Zbog način točke za VPN-ovi web-mjesta se obrađuju, uvijek imate naknadu za izlazne podatke putem veze za integraciju VNET čak i ako je na VNET u centru za iste podatke.  Da biste vidjeli što te troškove su pogledajte ovdje: [Prijenos cijene pojedinosti o podacima][DataPricing].  

Posljednje stavke je trošak VNET pristupnika.  Ako ne trebate pristupnika Osnovna vremenska crta kao što je web-mjesta za web-mjesta VPN-ovi koji su plaćanja za pristupnik za podršku značajki integracije VNET.  Postoje pojedinosti te troškove: [Cijene za VPN pristupnika][VNETPricing].  

##<a name="troubleshooting"></a>Otklanjanje poteškoća##

Dok je lako Postavljanje značajke koje ne znači da iskustva će biti problem besplatno.  Trebali biste naiđete na probleme prilikom pristupa postoji željeni krajnje su neke uslužni programi možete koristiti za testiranje povezivanja s konzole za aplikacije.  Postoje dva sučelja konzole koje možete koristiti.  Jedan je konzoli Kudu, a drugi je dođete na portalu za Azure konzoli sustava.  Da biste na konzoli Kudu s aplikacijom Idi na Alati -> Kudu.  To je ista kao namjeravate [NazivWebMjesta]. scm.azurewebsites.net.  Kada koji će se otvoriti samo idite na karticu konzole za ispravljanje pogrešaka.  Da biste dobili Azure konzoli portala glavnom računalu, a zatim iz aplikacije programa Idi na Alati -> konzolu.  


####<a name="tools"></a>Alati####

Ping alata nslookup i tracert ne funkcioniraju putem konzole zbog sigurnosnih ograničenja.  Da biste nastavili Poništi tamo su dva zasebna alata dodali.  Da bi se testiranje DNS funkcionalnosti dodali smo alat pod nazivom nameresolver.exe.  Vidjet ćete da sintaksa je:

    nameresolver.exe hostname [optional: DNS Server]

Nameresolver možete koristiti da biste provjerili hostnames koja ovisi o aplikacije.  Na taj način možete testirati i ako imate ništa pogrešno konfigurirana s DNS-a ili možda nemaju pristup svojem DNS poslužitelju.

Alat za sljedeći omogućuje testiranje za TCP veza sa sustavom kombinaciju domaćin i priključak.  Ovaj alat zove tcpping.exe, a vidjet ćete da sintaksa izgleda:

    tcpping.exe hostname [optional: port]

Ovaj alat će se obavještava vas ako možete doći do određenog domaćin i priključak, ali će izvesti isti zadatak Uhvatite na ICMP temelji ping utility.  Ping uslužni ICMP se ako je vaš glavno računalo prema gore.  S tcpping ste saznali pristupate određeni priključak na glavnom računalu.  


####<a name="debugging-access-to-vnet-hosted-resources"></a>Ispravljanje pogrešaka pristup VNET hostira resursi####

Postoji nekoliko stvari koje možete spriječiti da određene domaćin i priključak aplikacije.  U većini slučajeva je jedan od tri stvari:

- **Postoji vatrozid na način**  Ako imate vatrozid na način, a zatim kliknete TCP vremenskog ograničenja.  U ovom slučaju to je 21 sekunde.  Pomoću alata za tcpping testirati povezivost.  TCP vremensko ograničenje mogu se zbog mnogo toga iza vatrozida, ali postoji start.  
- **DNS je nije moguć**  Vrijeme čekanja DNS je tri sekunde po DNS poslužitelj.  Ako imate 2 DNS poslužitelja koji je 6 sekundi.  Koristite nameresolver radi li DNS-a.  Imajte na umu nslookup ne možete koristiti kao koje koriste DNS vaše VNET je konfiguriran za korištenje.
- **Raspon koji nisu valjani P2S IP** Pokažite na rasponu IP web-mjesta mora biti u RFC 1918 privatne IP rasponi (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255) ako raspon koristi IP-ovi izvan koji zatim što neće funkcionirati.  

Ako se te stavke ne odgovorite problem, potražite prvo jednostavne elemente kao što su:  

- Mi pristupnika prikazuje kao prema gore na portalu?
- Učinite li certifikati prikaz u obliku koji se sinkroniziraju?
- Jeste li nikome promijeniti mrežna konfiguracija bez "Sinkroniziraj mrežom" problematične ASPs? 

Ako je vaš pristupnik prema dolje pa premjestiti je sigurnosno kopiranje.  Ako su certifikati usklađen prešli u prikaz ASP vaše integriranja VNET i kliknete "Sinkroniziraj mreže".  Ako mislite da je bio promjena u konfiguraciji VNET, a drugu sinkronizacija će se s vašeg ASPs pa Idi na prikaz ASP vaše integriranja VNET i kliknite "Sinkroniziraj mreže" samo kao podsjetnik, to će uzrokovati kratak prekida vezu VNET i aplikacija sustava.  

Ako je sve precizno potrebno pristupili malo:

- Postoje li druge aplikacije pomoću VNET Integracija dosegne resursa u istoj VNET? 
- Možete idite na konzolu za aplikacije i koristiti tcpping dosegne drugih resursa u vašem VNET?  

Ako nešto od navedenog istinito vaše VNET Integracija je precizno i problem koji je na neko drugo mjesto.  Ovo je gdje se na informacije u pitanja i odgovora jer nema jednostavnih načina da biste vidjeli zašto ne može pristupiti glavnog računala: priključak.  Uzroka obuhvaćaju sljedeće:

- imate vatrozid prema gore na vašem računalu koje hostira onemogućuju pristup priključak računala točku rasponu IP web-mjesta.  Često presjeka podmreže potreban je pristup javnosti.
- Ciljno glavno računalo nije dostupno
- aplikacija je prema dolje
- imali pogrešan IP ili naziv glavnog računala
- aplikacija priključuje na drugi priključak nego što ste očekivali.  Provjerite to tako da odete na glavnom računalu i korištenje "netstat - aon" iz cmd redak.  Time ćete prikazati koje postupak ID priključuje na što priključka.  
- sigurnosne grupe na mreži konfigurirane na način koji su onemogućuju pristup glavnog računala i priključak od točke u rasponu IP web-mjesta

Imajte na umu da ne znate što IP u točku raspon IP web-mjesta kojima će se aplikacije koristiti tako da morate dopustiti pristup s cijeli raspon.  

Ispravljanje dodatne korake obuhvaćaju sljedeće:

- Prijavite se na neki drugi VM u vašem VNET i pokušajte pristupiti vašem resursa priključak glavnog računala: iz nje.  Postoje neka uslužni programi ping TCP moraju biti možete koristiti u tu svrhu ili čak i ako možete koristiti telnet.  Svrha ovdje je samo za utvrđivanje povezivanje dva iz ovog VM. 
- Pozovite aplikacije na drugu VM i testiranje pristup tom glavno računalo i priključak konzoli iz aplikacije programa  
####<a name="on-premise-resources"></a>Na lokalnu instancu resursi####
Ako vaš ne može pristupiti resursa u lokalnu prvo što biste trebali provjeriti je ako se u vašem VNET dosegne resurs.  Ako koji funkcionira pa daljnji koraci su vrlo lako.  Iz VM u vašem VNET morate pokušajte pristupiti na lokalnu aplikaciju.  Možete koristiti telnet ili utility za ping TCP.  Ako vaš VM ne može pristupiti vašem na lokalnu instancu resursa, a zatim najprije provjerite je li VPN vezu s web-mjesta za web-mjesta radi.  Ako se radi Provjerite iste stvari ranije zabilježili i konfiguracije pristupnika za lokalnu i status.  

Sada hostira vaš VNET VM možete doći do sustav na lokalnu instancu programa no aplikacije ne Razlog je vjerojatno nešto od sljedećeg:
- puteve nisu konfigurirana s točku raspona IP web-mjesta u na lokalnu instancu pristupnikom
- sigurnosne grupe na mreži onemogućuju pristup za točku rasponu IP web-mjesta
- vaš na lokalnu instancu vatrozida onemogućuju promet od točke na rasponu IP web-mjesta
- u svoje VNET koji sprječava točku da biste promet web-mjesta koji se temelji na lokalnu mrežu da imate korisnički definirane Route(UDR)

## <a name="hybrid-connections-and-app-service-environments"></a>Hibridno veze i okruženja aplikacije servisa##
Postoje 3 značajke koje omogućuju pristup VNET hostira resursi.  Mogućnosti su sljedeće:

- Integracija VNET
- Hibridno veze
- Aplikacije servisa za okruženja

Hibridno veze potrebno je instalirati preusmjeravanja agent naziva veze Manager(HCM) hibridnog u vašoj mreži.  Na HCM mora se moći povezati Azure i aplikacije.  To je rješenje osobito sjajno iz daljinski mrežni kao što su na lokalnu mrežu ili neka druga oblaka hostira mreže jer ne zahtijeva krajnje pristupačnih internet.  Na HCM samo izvodi u sustavu Windows, a imate najviše 5 instanci koje su pokrenute omogućuju visoke dostupnosti.  Hibridno veze podržava samo TCP kroz i svaki HC krajnjoj točki ima tako da odgovara određenim priključak glavnog računala: kombinaciji.  

Značajke aplikacije servisa okruženje omogućuje vam pokretanje instance komponente aplikacije servisa Azure u vašem VNET.  Omogućuje resursa aplikacije programa access u vašem VNET bez sve dodatne korake.  Neke od drugih prednosti servisa okruženju aplikacije je možete koristiti zaposlenici zaduženi za 8 Jezgra namjenski s 14 GB RAM-a.  Druga pogodnost je da je mogu mijenjati veličinu sustava prema svojim potrebama.  Za razliku od više klijentu okruženja gdje je vaša ASP ograničen veličine, u programa elika i mala slova omogućuje upravljanje koliko resursa koje želite dodijeliti na sustav.  Vezana uz žarišta mreže ovog dokumenta kroz, nešto što s programa elika i mala slova koji ne uz integraciju VNET je da ga možete raditi na ExpressRoute VPN-a.  

Dok je neki koriste slučaja preklapanja, ništa te značajke možete zamijeniti bilo koji od drugih.  Kada zna koje značajku tako da koristi povezana sa svojim potrebama i kako ćete ga koristiti.  Ako, na primjer:

- Ako ste razvojni inženjer i jednostavno želite pokrenete web-mjesta u Azure, a zatim ga pristup bazi podataka na radne stanice u odjeljku stol na najjednostavniji način da biste koristili je hibridnog veze.  
- Ako ste velike tvrtke ili ustanove koji želi da biste stavili velikog broja web svojstava u javnosti u oblaku i upravljajte u vlastiti mreži, a zatim koji želite prijeći okruženje za aplikaciju servisa.  
- Ako imate broj aplikacije servisa za hostirane aplikacije i jednostavno želite pristupiti resursa u vašem VNET VNET Integracija je način za slanje.  

Napredno korištenje slučajeva Postoje neka jednostavnosti povezane aspekte.  Ako vaš VNET već povezani s mrežom lokalnu na pomoću VNET Integracija ili okruženju aplikacije servisa jednostavan je način za lokalnu resursa.  S druge strane, ako vaš VNET niste povezani s mrežom na lokalnu instancu programa, a zatim je mnogo više indirektni za postavljanje VPN-a web-mjesta za web-mjesta s vašeg VNET u usporedbi s instaliranjem sustava HCM.  

Osim funkcionalne razlike između postoje i cijene razlike.  Značajka aplikacije servisa okruženje je na Premium servis nudi ali nudi najviše mreže konfiguracija mogućnosti uz ostale sjajne značajke.  Integracija VNET može se koristiti s standardni prikaz ili Premium ASPs i nije savršen za sigurno troše resursa u vašem VNET iz više klijentske aplikacije servisa za.  Hibridno veze trenutno ovisi o BizTalk račun koji ima cijene razine koji počinju besplatne i dobiti progresivno više pozivnim na temelju iznosa vam je potrebna.  Kada je riječ o radu putem više mreža kroz, postoji druge značajke kao što su hibridnog veze koje omogućuju pristup izvorima u i 100 zasebnom mrežama.    


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
