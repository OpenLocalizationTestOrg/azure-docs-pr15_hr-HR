<properties
   pageTitle="Sigurnost servisa i mreže Microsoft cloud | Microsoft Azure"
   description="Naučite neke ključne značajke dostupne u Azure će vam olakšati stvaranje sigurnog okruženja mreže"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jonor;sivae"/>

# <a name="microsoft-cloud-services-and-network-security"></a>Sigurnost servisa i mreže Microsoft cloud

Microsoftovim servisima u oblaku isporučiti hyperscale services i infrastrukture, mogućnosti poslovne klase i mnoštvo mogućnosti za hibridno povezivanje. Korisnici možete odabrati da biste pristupili tih servisa putem Interneta ili s Azure ExpressRoute koji omogućuje povezivanje privatne mreže. Platforme Microsoft Azure omogućuju korisnicima jednostavno proširiti svoju infrastrukturu u oblak i izraditi višeslojne arhitekturi. Uz to, trećim stranama možete omogućiti poboljšane mogućnosti pomoću usluge zaštite i virtualne aparata. Ova studija sadrži Pregled sigurnosti i arhitektonski problemi koji korisnici morate imati na umu prilikom korištenja Microsoftovim pristupiti putem ExpressRoute servisima u oblaku. Također pokriva stvaranja sigurnije servisa u Azure virtualne mreže.

## <a name="fast-start"></a>Brzo pokretanje
Sljedeći logike grafikon možete usmjeriti određene primjeru mnogo sigurnost tehnika dostupno u sklopu Azure platforme. Pronađite primjer najprikladniji slučaj za brzi pregled. Za potpuniji objašnjenja nastaviti s čitanjem kroz papir.
![Dijagram toka sigurnosne mogućnosti][0]

[Primjer 1: Izrada opseg mrežom (poznat i kao DMZ, demilitarized zone i screened podmreže) da biste zaštitili aplikacije sa sigurnosnim grupama s mrežom (NSGs).](#example-1-build-a-simple-dmz-with-nsgs)</br>
[Primjer 2: Izrada opseg mreže da biste zaštitili aplikacije s vatrozid i NSGs.](#example-2-build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs)</br>
[Primjer 3: Stvaranje opsega mreže da biste zaštitili mrežama s vatrozid, korisnički definirane usmjeravanje (UDR) i NSG.](#example-3-build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg)</br>
[Primjer 4: Dodavanje veze hibridnog sa na web-mjesto, virtualne potražite virtualne privatne mreže (VPN-a).](#example-4-adding-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Primjer 5: Dodavanje hibridnog povezivanja s web-mjesto, Azure pristupnika VPN-a.](#example-5-adding-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn)</br>
[Primjer 6: Dodavanje veze hibridnog sa ExpressRoute.](#example-6-adding-a-hybrid-connection-with-expressroute)</br>
Primjeri za dodavanje veze između virtualne mreže, visoke dostupnosti i servis za Ulančavanje dodat će se na ovaj dokument putem sljedećih nekoliko mjeseci.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Zaštita usklađenost i infrastrukture Microsoft
Microsoft je poduzeo određene vodstvo položaj usklađenost inicijative potrebnih tvrtke za podršku. Slijede neki certifications usklađenost za Azure: ![Azure usklađenost tipki][1]

Dodatne informacije potražite u članku informacije o usklađenosti s [Microsoftova centra za pouzdanost](https://azure.microsoft.com/support/trust-center/compliance/).

Microsoft ima potpun pristup da biste zaštitili potrebne za pokretanje hyperscale globalni servisi infrastrukture u oblaku. Microsoft cloud infrastrukture obuhvaća hardver, softver, mrežama i administrativnih i drugo osoblje operacije, osim fizičke podatkovnim centrima.

![Značajke sigurnosti Azure][2]

Taj se način nudi sigurnije foundation za klijente za implementaciju svoje usluge u programu Microsoft Excel. Sljedeći je korak za klijente za dizajniranje i stvaranje sigurnosna arhitektura da biste zaštitili tih servisa.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Tradicionalni sigurnost arhitekturi i perifernih mreža
Iako Microsoft intenzivnog invests u zaštiti infrastrukture u oblaku, korisnici moraju zaštititi njihove servise u oblaku i grupa resursa. Višeslojna pristup sigurnost nudi najbolje obrane. Sigurnosne zone mreže opseg štiti internoj mreži resursa iz nepouzdanih mreže. Opseg mreže odnosi se na rubovima ili sjesti između interneta i zaštićene enterprise IT infrastrukturu dijelovima mreže.

U standardne korporacijskim mrežama, Infrastruktura temeljni je intenzivnog fortified na perimeters s više Slojevi sigurnosni uređaji. Granicu svaki sloj sastoji se od uređaja i pravila provođenja točke. Uređaji mogu obuhvaćaju sljedeće: vatrozidima, sprječavanje Distributed uskraćivanje usluge (DDoS), otkrivanje podataka ili zaštita sustavi (ID-ovi i IP-ovi) i uređaje VPN-a. Uvođenje pravila može potrajati obliku pravila vatrozida, popise kontrole pristupa (ACL) ili određene usmjeravanja. U prvom retku obrane u mreži, izravno prihvaćanje dolazne promet s Interneta, je kombinacija te mehanizme bloka napadi i štetnih promet istovremeni valjane zahtjeva za daljnje u mrežu na. Usmjerava promet ovo izravno s resursima u mrežu na opseg. Resursa možda zatim "obraćajte" resursi dublju na mreži transiting granice sljedeće provjere valjanosti prvi. Krajnje sloj zove mreže opseg jer se taj dio mreže je izložen putem Interneta, na obično s neki oblik zaštite na obje strane. Sljedeća slika prikazuje primjer mreže opseg jedan podmreže u korporacijskom mrežom, s dvije granice sigurnost.

![Opseg mreže u korporacijskom mrežom][3]

Postoji mnogo arhitekturi za implementaciju opseg mreži, iz jednostavne opterećenja ispred web-farme više mreža opseg podmreže uz brojeva mehanizme pri svakom granicu blokirati promet, a zatim zaštiti dublju Slojevi poslovnoj mreži. Kako se temelji na mrežu opseg ovisi o određenim potrebama tvrtke ili ustanove i cjelokupan dopušteno odstupanje od rizika.

Kao što je kupci premjestili njihove radnih opterećenja javno oblaka, je važnosti da slične mogućnosti podrške za opseg mreže arhitektura u Azure da bi odgovarao sukladnost i sigurnosne preduvjete. Ovaj dokument sadrži upute na kako korisnici možete izraditi sigurne mrežnom okruženju servisu Azure. Usredotočuje se na mreži opseg, ali uključuje raspravi potpun brojne aspekte mrežne sigurnosti. Sljedeća pitanja obavještavanje o ovom rasprave:

- Kako mrežom opseg Azure može biti komponenti?
- Što su Azure značajki dostupnih da biste sastavili opseg mreže?
- Kako radnih opterećenja pozadinske mogu biti zaštićene?
- Kako se upravlja komunikacije Internet da biste radnih opterećenja servisu Azure?
- Kako mogu lokalne mreže biti zaštićene iz implementacije u Azure?
- Kada treba nativni Azure sigurnosnim značajkama koristiti i drugih proizvođača aparata ili usluge?

Sljedeći dijagram prikazuje razne slojeve sigurnosti Azure omogućuje korisnicima. Te slojeve su lokalnog u platforme Azure sam i korisnički definirane značajke:

![Azure sigurnosna arhitektura][4]

Dolazni s Interneta, Azure DDoS pridonosi zaštiti protiv veliki napada protiv Azure. Sljedeći sloj je korisnički definirane javno krajnje točke, koji se koriste za utvrđivanje koji promet mogu prolaziti kroz servisa u oblaku da biste virtualne mreže. Izvorni Azure virtualne mreže odvajanja osigurava dovršeno odvajanja s drugim mrežama, a da promet samo teče putem korisnika konfiguriran putove i metode. Ove putova i postupci su sljedeći sloja gdje NSGs, UDR i virtualne aparata mreže mogu se da biste stvorili granice sigurnost da biste zaštitili implementacijama aplikacija zaštićenu mreže.

Sljedeći odjeljak sadrži pregled Azure virtualne mreže. Te virtualne mreže stvaraju se kupci, a su što njihove distribuiranih radnih opterećenja povezani. Virtualne mreže su temelj sve značajke sigurnosti mreže potrebne da biste uspostavili opseg mreže da biste zaštitili implementacijama kupca u Azure.

## <a name="overview-of-azure-virtual-networks"></a>Pregled Azure virtualne mreže
Prije internetski promet doći do Azure virtualne mreže, postoje dvije slojeva zaštite postojećih Azure platformi:

1.  **Zaštita DDoS**: zaštita DDoS je sloj Azure fizičke mrežne štiti platforme Azure sam od napada za veliki utemeljene na Internetu. Tih napada pomoću više čvorove "robot" u pokušaj pretrpati internetski servis. Azure ima robusne DDoS zaštitu mreža na sve ulaznog s Internetom. U ovom slojeva zaštite DDoS ne može se konfigurirati atributima korisnika i nije dostupno za korisnike. Time se štiti Azure kao platformu od napada veliki, ali se ne izravno štite pojedinačne korisnike računala. Koje je korisnik odabrao protiv lokalizirane napada moguće je konfigurirati dodatne Slojevi resilience. Ako, na primjer, ako je kupca odgovora napada s veliki napada DDoS na javno krajnje točke, Azure blokira veze na tom servisu. Korisnička odgovora nije moguće se neće putem drugog virtualne mreže ili krajnja točka ne vezanih uz napada da biste vratili servis za servis. Ga Primijetit ćete da iako kupca odgovora može utjecati na tom krajnje točke, drugim servisima izvan te krajnje točke bi utjecati. Uz to, ostali Kupci i servise vidjeti bez utjecaja na tom napada.
2.  **Krajnje točke servisa**: krajnje točke Dopusti oblaka services ili resursa grupe da bi javno Internet IP adresa i priključaka vidljiva. Krajnja točka će koristiti Prijevod mrežne adrese (NAT) da biste usmjerili promet na internu adresu i priključak na Azure virtualne mreže. Ovo je primarni put do vanjskih promet za prosljeđivanje u virtualne mreže. Krajnje točke servisa su korisnik može konfigurirati da biste utvrdili koji promet se prenosi u i gdje i kako ga je preveden na virtualne mreže.

Kada promet dostigne virtualne mreže, dostupne su mnoge značajke koje se isporučuju u Reproduciraj. Azure virtualne mreže su temelj za kupce priložiti njihove radnih opterećenja i gdje se primjenjuje osnovni sigurnosti na razini mreže. Privatne mreže (prekriveno virtualne mreže) je u Azure za kupce s sljedeće značajke i značajke:
 
- **Promet odvajanja**: virtualne mreže je granicu odvajanja promet na platformi Azure. Virtualnim strojevima (VMs) u jedan virtualne mreže komunikacija izravno u VMs u različitim virtualne mreže, čak i ako su oba virtualne mreže stvaraju istog kupca. Ovo je ključnih svojstvo koje omogućuje kupac VMs i komunikacije ostaje privatnim unutar virtualne mreže.
- **Topologija multitier**: virtualne mreže dopusti kupcima da biste definirali multitier topologije dodjeljivanje podmreže i određivanje razmake zasebnom adresa za različite elemente ili "razine" njihove radnih opterećenja. Ove logička grupiranja topologija omogućiti kupcima da biste definirali pravilnik o pristupu različite temelje na vrsti radno opterećenje i kontrolirati promet tijekove između na razine.
- **Povezivanje s više lokacija**: korisnici možete uspostaviti više lokacija veze između virtualne mreže i više lokalnog web-mjesta ili druge virtualne mreže u Azure. Da biste to učinili, korisnici možete koristiti Azure VPN pristupnika ili virtualne aparata mreže drugih proizvođača. Azure podržava web-mjesto VPN pomoću standardnih IPsec-IKE protokoli i privatni povezivanje ExpressRoute (S2S)-ovima.
- **NSG** omogućuju korisnicima stvaranje pravila (ACL) na željenu razinu preciznosti: mreže sučelja, pojedinačne VMs ili virtualne podmreže. Korisnike možete kontrolirati pristup značajki ili odbijanje komunikaciju između radnih opterećenja unutar virtualne mreže Systems kupca mrežama putem povezivanje više lokacija ili izravno internetskih komunikacija.
- **UDR** i **Prosljeđivanje IP** omogućuju kupcima da biste definirali putova komunikacije između različite razine unutar virtualne mreže. Korisnici možete implementirati vatrozid, ID-ovi i IP-ovi i druge virtualne aparata i mrežni promet usmjeravajte te aparata sigurnosti za poboljšavanje pravila granicu sigurnosti, nadzor i provjere.
- **Mrežni virtualne aparata** u Azure Marketplace: sigurnost aparata kao što su vatrozidima, balancers učitavanje i ID-ovi i IP-ovi su dostupne u trgovine Windows Azure i Galerija slika VM. Da biste dovršili okruženje za osiguranje mreže multitiered koji korisnici mogu implementirati te aparata u svoje virtualne mreže i Konkretno, na njihove sigurnosnih ograničenja (uključujući mreže podmreže opseg).

Pomoću ove značajke i mogućnosti jednog primjera kako arhitekturu mreže opseg može biti nastala Azure je sljedeće:

![Opseg mrežom Azure virtualne mreže][5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Opseg mreže značajke i preduvjeti
Opseg mreže namijenjen je moguće sučelje mreže izravno interfacing komunikaciju putem Interneta. Dolazne pakete treba tijeka kroz aparata sigurnost, kao što je vatrozid, ID-ovi i IP-ovi, prije Dostizanje pozadinskih poslužitelja. Internet granica pakete iz na radnih opterećenja možete tijeka kroz aparata sigurnosti u mreži opseg za provođenje pravila, provjere i nadzor svrhe, prije ostavite s mrežom. Uz to, s mrežom opseg možete hostirati pristupnika VPN-lokacija između kupca virtualne mreže i lokalne mreže.

### <a name="perimeter-network-characteristics"></a>Opseg mreže značajke
Upućuju na prethodni slici, neke značajke mreža dobar opseg su na sljedeći način:

- Mjesto na Internetu:
    - Podmreže mreže opseg sam je komuniciraju s Internetom, izravno komunikaciju s Interneta.
    - Javnu IP-ovi, VIPs i/ili krajnje točke servisa prenesite internetski promet sučelja mreže i uređaje.
    - Dolazni promet s Interneta prolazi kroz sigurnosni uređaji prije drugih resursa na mreži sučelja.
    - Ako je omogućeno izlaznog sigurnost, promet se prolazi kroz sigurnosni uređaji kao završnom koraku prije Prenos s Internetom.
- Zaštićeni mreže:
    - Postoji infrastrukture core nema Izravni puta s Interneta.
    - Morate kanala infrastrukture core prolaziti kroz sigurnosni uređaji kao što su NSGs, vatrozida ili VPN uređaja.
    - Ostali uređaji ne morate premostiti internetske i core infrastrukture.
    - Sigurnosni uređaji na Internet okrenutom i zaštićene mreže nasuprotne ograničenja mreže opseg (na primjer, dva vatrozid ikone prikazano na slici prethodne) zapravo možda u jednom virtualne potražite isporučujte prilagođena pravila ili sučelja za svaki granicu. (To jest, jedan uređaj logično odvojene rukovanje opterećenje za oba ograničenja mreže opseg.)
- Drugi uobičajeni prakse i ograničenja:
    - Radnih opterećenja morate spremiti ključnih podataka tvrtke.
    - Pristup i ažuriranja opseg mrežnim konfiguracijama i implementacijama ograničeni su na samo ovlašteni administratori.

### <a name="perimeter-network-requirements"></a>Opseg mreže
Da biste omogućili te karakteristike, slijedite ove smjernice virtualne mreže zahtjevima za implementaciju uspješno opseg mreže:

- **Arhitektura podmreže:** Navedite virtualne mreže tako da se cijeli podmreže predano kao opseg mrežu, odvojeni od drugih podmreže u istom virtualne mreže. Na taj način promet između opseg mreže i druge interni ili privatne podmreže razine tokova kroz vatrozid ili ID-ovi i IP-ovi virtualne potražite na granice podmreže pomoću korisnički definiranih usmjerava.
- **NSG:** Podmreže mreže opseg sam mora biti otvoren dopustiti komunikaciju s Internetom, ali koje ne znači da korisnici moraju zaobilaženje NSGs. Slijede uobičajeni postupke za sigurnost da biste minimizirali površine mreže izložen putem Interneta. Zaključati raspona Udaljena adresa dopušten pristup s implementacijama ili protokola za određenu aplikaciju i priključaka koje su otvorene. Mogu postojati okolnostima, međutim, u kojoj to nije uvijek moguće. Ako, na primjer, ako korisnici imaju vanjskih web-mjesta u Azure, mreže opseg dopustite zahtjevi web na javnu IP adrese, ali Otvarajte samo priključke aplikacije na web: TCP:80 i TCP:443.
- **Usmjeravanja tablice:** Podmreže mreže opseg sam trebali biste moći izravno komuniciranje s Internetom, ali bez prolaze kroz vatrozid ili sigurnosne potražite treba Dopusti izravne komunikacije i iz natrag end ili lokalne mreže.
- **Konfiguracije sigurnosti potražite:** Da biste mogli usmjeravanje i provjera pakete između opseg mreže i ostatak zaštićeni mrežama, sigurnost aparata kao što je vatrozid, ID-ovi i IP-ovi uređajima možda višestruki matični. Možda imaju zasebna NIC-ovi za mrežni opsega i pozadinske podmreže. Mrežne kartice u mreži opseg komunicirati izravno i putem Interneta, s odgovarajuće NSGs i opseg tablicu usmjeravanja mreže. Mrežne kartice povezati podmreže pozadinske imati više ograničeno NSGs i usmjeravanje tablice odgovarajuće podmreže pozadinske.
- **Sigurnosnim funkcijama potražite:** Sigurnost aparata obično implementiran u mreži opseg izvedite sljedeće funkcije:
    - Vatrozid: Provoditi pravila vatrozida ili pravila za kontrolu pristupa za dolazni zahtjevi za.
    - Prijetnju otkrivanje i sprječavanje: otkrivanje i mitigating zlonamjernog napada s Interneta.
    - Nadzor i zapisivanje: održavanje detaljne zapisnicima nadzora i analize.
    - Obrnuti proxy: usmjerite dolaznih zahtjeva u odgovarajuće pozadinskih poslužitelja. To obuhvaća mapiranje i prevođenja adrese odredište na pristupnom uređajima obično vatrozidi, adrese pozadinskih poslužitelja.
    - Prosljeđivanje proxy: nudi NAT i izvođenja nadzor za komunikaciju pokrenuto iz unutar virtualne mreže s Internetom.
    - Usmjerivač: Prosljeđivanja dolaznih i unakrsno podmreže promet unutar virtualne mreže.
    - Uređaj VPN: ulozi pristupnika VPN-lokacija za povezivanje VPN-lokacija između kupca lokalne mreže i Azure virtualne mreže.
    - VPN poslužitelja: prihvaćanja VPN klijenti za povezivanje s Azure virtualne mrežama.

>[AZURE.TIP] Staviti sljedeće dvije grupe: pojedinaca dozvolu za pristup zupčanika za sigurnost opseg mreže i pojedincima ovlašteni kao razvoj, uvođenje ili operacije administratori aplikacije. Te grupe zadržavanje zasebne omogućuje segregation o obavezama koje imate i onemogućuje zaobilaženje Sigurnost aplikacije i mreže sigurnosne kontrole jednoj osobi.

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Pitanja na koja treba zatražiti prilikom stvaranje ograničenja mreže
U ovom odjeljku, osim ako izričito navedeno termina "mreža" se odnosi na privatne Azure virtualne mreže stvorio administrator pretplate. Izraz ne odnosi na pozadini fizičke mreža unutar Azure.

Osim toga, Azure virtualne mreže često se koristi da biste proširili tradicionalni lokalne mreže. Moguće je ugraditi web-mjesto ili ExpressRoute hibridnog umrežavanje rješenja s arhitekturi opseg mreže. To je važna činjenica u mreži sigurnosna ograničenja.

Sljedeća tri pitanja su ključnih na koja treba odgovoriti kada se stvara u mreži s mrežom opsega i više sigurnosnih ograničenja.

#### <a name="1-how-many-boundaries-are-needed"></a>1) ograničenja koliko su vam potrebne?
Prva točka odluke je odlučiti koliko sigurnosnih ograničenja su vam potrebne u određenom scenariju:

- Jedan granicu: na mreži sučelja opseg između virtualne mreže i s Internetom.
- Dvije granice: nešto na Internet strani opseg mreže, a drugi između mreže podmreže opsega i podmreže pozadinske u Azure virtualne mreže.
- Tri ograničenja: na strani Internet mreže opseg, jedan između opseg mreže i pozadinske podmreže i jedan između podmreže pozadinske i lokalne mreže.
- Ograničenja N: varijable broj. Ovisno o sigurnosne preduvjete postoji zaista bilo koji broj granica sigurnosti koje se mogu primijeniti u određenom mreži.

Broj i upišite granica potrebno će razlikuju se ovisno o odstupanje rizika u tvrtki i određene scenarij koji se primijenjeno. To je često joint odluka načinio više grupa u tvrtki ili ustanovi, često uključujući rizika i usklađenost tima, mreže i platformu tima i tim za razvoj programa aplikacije. Osobe s poznavanje sigurnost, koji je uključen podataka i tehnologije koja se koristi treba imati na izgovorite u tu odluku da biste bili sigurni stance odgovarajuće sigurnosne svaki implementacije.

>[AZURE.TIP] Koristite najmanji broj granica koji zadovoljavaju sigurnosne preduvjete za dani situaciju. S više ograničenja na teže operacije i otklanjanje poteškoća može biti, kao i upravljanje indirektnog vezanih uz upravljanje više pravila granicu tijekom vremena. Međutim, dovoljno granice povećati rizika. Pronalaženje saldo je važnosti.

![Hibridno mreža s tri sigurnosna ograničenja][6]

Na prethodnoj slici prikazuje više razine prikaz tri sigurnosna granicu mreže. Granice se između opseg mreže i s Internetom, na Azure sučelja i stražnje privatne podmreže, Azure podmreži pozadinsku i lokalnu mrežu tvrtke.

#### <a name="2-where-are-the-boundaries-located"></a>2) gdje se nalaze granice?
Kada su odlučili broj granica, gdje ih provesti je sljedeći odluka točke. Obično su tri mogućnosti:
- Korištenje internetske privremene (na primjer, oblaku Web aplikacije vatrozid, koji se spominju u ovom dokumentu)
- Korištenje značajki nativnih i/ili mreže virtualne aparata u Azure
- Korištenje fizičkih uređaja na lokalnu mrežu

Mogućnosti na isključivo Azure mrežama su izvorni Azure značajke (na primjer, Azure učitavanja Balancers) ili mrežni virtualne aparata s obogaćenim partnera zajednici programa Azure (na primjer, potvrdite točku vatrozidima).

Granicu potrebi između Azure i lokalne mreže sigurnosni uređaji može se nalaziti na obje strane veze (ili obje). Stoga odluka mora izvršiti na mjestu da biste postavili sigurnosne zupčanika.

Na prethodni slici mreže Internet opseg granice prednju-na-pozadinskoj potpuno nalaze Azure i mora biti izvorni Azure značajke ili mreže virtualne aparata. Uređaji za sigurnost na granicu između Azure (pozadinske podmreži) i poslovnoj mreži može biti Azure strani ili strani lokalnog ili čak i kombinacije uređajima na obje strane. Može biti vrlo prednosti i nedostatke mogućnost koji moraju utjecati smatrati.

Korištenje postojeće zupčanika fizičke sigurnosti na strani lokalne mreže, na primjer, ima prednost da nema novi zupčanika potreban. Samo se mora ponovno konfiguriranje. Nedostatak, no je da sve promet mora potjecati natrag iz Azure za lokalnu mrežu da vide zupčanika sigurnost. Stoga Azure Azure promet može uzrokovati znatnog Latencija i utječu na performanse računala i korisničko sučelje, ako je morao ponovno lokalne mreže za uvođenje sigurnosnih pravila.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) kako primjenjuju granice
Svaki granicu sigurnost će vjerojatno tretiraju drugu mogućnost (Ako, na primjer, ID-ove i pravila vatrozida na Internet strani opseg mreže, ali samo ACL-a između opseg mreže i pozadinske podmreže). Odabiru uređaja za korištenje ovisi o preduvjetima scenarij i sigurnost. U sljedećem odjeljku Primjeri 1, 2 i 3 navode neke mogućnosti koje se mogu koristiti. Pregled značajki Azure nativni mreže i uređaji koji su dostupni servisu Azure s partnerom zajednici prikazuje myriad mogućnosti dostupne za rješavanje gotovo bilo kojeg scenarij.

Drugu točku odluke o implementaciji ključa je kako povezati lokalne mreže s Azure. Morate koristiti Azure virtualne pristupnika ili uređaj za virtualne mreže? Ove mogućnosti se spominju u nešto podrobnije u sljedećem odjeljku (Primjeri 4, 5 i 6).

Uz to, promet između virtualne mreže unutar Azure možda je potreban. Sljedećim scenarijima dodat će se u budućnosti.

Kada znate odgovore na pitanja prethodno, u odjeljku [Brzo pokretanje](#fast-start) olakšava prepoznavanje koje Primjeri su najprikladniji za dani scenarija.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Primjeri: Stvaranje sigurnosnih ograničenja Azure virtualne mreža
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Primjer 1: Izrada opseg mreže da biste zaštitili aplikacije s NSGs
[Natrag na početak brzo](#fast-start) | [detaljnom sastavljanje upute u ovom primjeru][Example1]

![Dolazni opseg mreža s NSG][7]

#### <a name="environment-description"></a>Opis okruženje
U ovom primjeru prikazuje se na pretplatu koja sadrži sljedeće:

- Dva servise u oblaku: "FrontEnd001" i "BackEnd001"
- Virtualne mreže, "CorpNetwork" s dvije podmreže: "Sučelju" i "Pozadinskog"
- Mrežni sigurnosne grupe koje je primijenjeno na oba podmreže
- Windows server koja predstavlja aplikacije web-poslužitelj ("IIS01")
- Dva poslužitelja sustava Windows koji predstavljaju aplikacije pozadinskih poslužitelja ("AppVM01", "AppVM02")
- Windows server koja predstavlja DNS poslužitelja ("DNS01")

Skripte i predložak Azure upravljanja resursima potražite u članku [Sastavljanje detaljne upute][Example1].

#### <a name="nsg-description"></a>Opis NSG
U ovom primjeru granu NSG je ugrađena i zatim učitati sa šest pravila.

>[AZURE.TIP] Općenito govoreći, stvorite određena pravila "Dopusti" najprije slijedi više generički pravila "Ne". Navedeni prioritet određuje pravila koja vrednuju se prvi put. Kada promet nalazi se da biste primijenili na određenom pravilu, vrednuju se daljnje pravila. Pravila NSG možete primijeniti u svakom dolazne i odlazne smjeru (iz perspektive podmreži).

Deklarativno, sljedeća pravila koja se ugrađenih za dolazni promet:

1.  Nije dopuštena interne DNS promet (priključak 53).
2.  Nije dopuštena RDP promet (priključak 3389) s Interneta na bilo kojem virtualnog računala.
3.  Nije dopuštena HTTP promet (priključak 80) s Interneta na web-poslužitelj (IIS01).
4.  Bilo koji promet (svi priključci) iz IIS01 AppVM1 je dopušteno.
5.  Odbijen je bilo koji promet (svi priključci) s Interneta na cijelu virtualne mreže (obje podmreže).
6.  Bilo koji promet (svi priključci) iz sučelja podmreže podmreži pozadinsku blokiran.

Pomoću tih pravila vezana uz svaki podmreže ako HTTP zahtjev je ulaznog s Interneta na web-poslužitelj, i pravila 3 (Omogući) i 5 (Uskrati) primjenjivati. No jer je pravilo 3 ima viši prioritet, samo na koji želite primijeniti i pravila 5 bi isporučuju u Reproduciraj. Stoga HTTP zahtjev želite omogućiti na web-poslužitelj. Ako taj isti promet je pokušaju pristupiti poslužitelju DNS01, pravilo 5 (Uskrati) bio prvi da biste primijenili i promet bi nije dopuštena za prosljeđivanje na poslužitelj. Pravilo 6 (Uskrati) blokira sučelja podmreže iz razgovor podmreži pozadinsku (osim dopuštene promet u pravilima 1 i 4). Time se štite mreže pozadinske u slučaju napadaču compromises web-aplikacija na sučelju. Napadač bi ograničeni pristup mreži "zaštićeni" pozadinske (samo za resurse koji se prikazuje na poslužitelju AppVM01).

Postoji zadani izlazni pravilo koje dopušta promet odgovor s Internetom. U ovom primjeru smo si omogućivanja odlazni promet i ne izmjena izlaznog pravila. Da biste zaključali dolje promet u oba smjera, korisnički definirane usmjeravanje potreban je (pogledajte primjer 3).

#### <a name="conclusion"></a>Zaključak
To je razmjerno jednostavne i jasan način izoliranja podmreži pozadinsku iz ulaznog promet. Dodatne informacije potražite u članku [Sastavljanje detaljne upute][Example1]. Ove upute obuhvaćaju sljedeće:

- Upute za sastavljanje ovu mrežu opseg s skripte komponente PowerShell.
- Upute za sastavljanje ovu mrežu opseg pomoću predloška Azure Voditelj resursa.
- Detaljne opise svaku naredbu NSG.
- Detaljne promet scenarija tijeka, prikazuje kako promet dopušten ili zabranjen svaki sloju.


 ### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Primjer 2: Izrada opseg mreže da biste zaštitili aplikacije s vatrozid i NSGs
[Natrag na početak brzo](#fast-start) | [detaljnom sastavljanje upute u ovom primjeru][Example2]

![Dolazni opseg mreže s alatima NVA i NSG][8]

#### <a name="environment-description"></a>Opis okruženje
U ovom primjeru prikazuje se na pretplatu koja sadrži sljedeće:

- Dva servise u oblaku: "FrontEnd001" i "BackEnd001"
- Virtualne mreže, "CorpNetwork" s dvije podmreže: "Sučelju" i "Pozadinskog"
- Mrežni sigurnosne grupe koje je primijenjeno na oba podmreže
- Mrežni virtualne uređaj, u ovom slučaju u vatrozid, povezani sučelja podmreže
- Windows server koja predstavlja aplikacije web-poslužitelj ("IIS01")
- Dva poslužitelja sustava Windows koji predstavljaju aplikacije pozadinskih poslužitelja ("AppVM01", "AppVM02")
- Windows server koja predstavlja DNS poslužitelja ("DNS01")

Skripte i predložak Azure upravljanja resursima potražite u članku [Sastavljanje detaljne upute][Example2].

#### <a name="nsg-description"></a>Opis NSG
U ovom primjeru granu NSG je ugrađena i zatim učitati sa šest pravila.

>[AZURE.TIP] Općenito govoreći, stvorite određena pravila "Dopusti" najprije slijedi više generički pravila "Ne". Navedeni prioritet određuje pravila koja vrednuju se prvi put. Kada promet nalazi se da biste primijenili na određenom pravilu, vrednuju se daljnje pravila. Pravila NSG možete primijeniti u svakom dolazne i odlazne smjeru (iz perspektive podmreži).

Deklarativno, sljedeća pravila koja se ugrađenih za dolazni promet:

1.  Nije dopuštena interne DNS promet (priključak 53).
2.  Nije dopuštena RDP promet (priključak 3389) s Interneta na bilo kojem virtualnog računala.
3.  Bilo koji internetski promet (svi priključci) da biste potražite virtualne mreže (Vatrozid) je dopušteno.
4.  Bilo koji promet (svi priključci) iz IIS01 AppVM1 je dopušteno.
5.  Odbijen je bilo koji promet (svi priključci) s Interneta na cijelu virtualne mreže (obje podmreže).
6.  Bilo koji promet (svi priključci) iz sučelja podmreže podmreži pozadinsku blokiran.

Pomoću tih pravila vezana uz svaki podmreže ako HTTP zahtjev je ulaznog s Interneta vatrozidu, i pravila 3 (Omogući) i 5 (Uskrati) primjenjivati. No jer je pravilo 3 ima viši prioritet, samo na koji želite primijeniti i pravila 5 bi isporučuju u Reproduciraj. Stoga HTTP zahtjev želite omogućiti vatrozid. Ako taj isti promet je pokušaju pristupiti poslužitelju IIS01 čak i ako je na pristupnom podmreži, pravilo 5 (Uskrati) primjenjivati, a promet bi nije dopuštena za prosljeđivanje na poslužitelj. Pravilo 6 (Uskrati) blokira sučelja podmreže iz razgovor podmreži pozadinsku (osim dopuštene promet u pravilima 1 i 4). Time se štite mreže pozadinske u slučaju napadaču compromises web-aplikacija na sučelju. Napadač bi ograničeni pristup mreži "zaštićeni" pozadinske (samo za resurse koji se prikazuje na poslužitelju AppVM01).

Postoji zadani izlazni pravilo koje dopušta promet odgovor s Internetom. U ovom primjeru smo si omogućivanja odlazni promet i ne izmjena izlaznog pravila. Da biste zaključali dolje promet u oba smjera, korisnički definirane usmjeravanje potreban je (pogledajte primjer 3).

#### <a name="firewall-rule-description"></a>Opis pravila vatrozida
Na vatrozid, pravila za prosljeđivanje moraju se stvoriti. Budući da u ovom primjeru samo usmjerava internetski promet u povezanih vatrozidu i zatim na web-poslužitelj samo jedan prosljeđivanje mrežnih prevođenje adresa (NAT) potreban je pravilo.

Pravilo prosljeđivanja prihvaća bilo koje adrese ulazni izvorni koji dodirne vatrozid pokušaju kontaktirati HTTP (priključak 80 i 443 HTTPS). Ona sadrži šalju iz vatrozida lokalno sučelje i preusmjerava na web-poslužitelj s IP adresom 10.0.1.5.

#### <a name="conclusion"></a>Zaključak
To je razmjerno jasan način zaštita aplikacije s vatrozidom i izoliranja podmreži pozadinsku iz ulaznog promet. Dodatne informacije potražite u članku [Sastavljanje detaljne upute][Example2]. Ove upute obuhvaćaju sljedeće:

- Upute za sastavljanje ovu mrežu opseg s skripte komponente PowerShell.
- Upute za sastavljanje u ovom se primjeru pomoću predloška Azure Voditelj resursa.
- Detaljne opise svako NSG naredbe i vatrozid za pravilo.
- Detaljne promet scenarija tijeka, prikazuje kako promet dopušten ili zabranjen svaki sloju.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-udr-and-nsg"></a>Primjer 3: Stvaranje opsega mreže da biste zaštitili mrežama s vatrozid, UDR i NSG
[Natrag na početak brzo](#fast-start) | [detaljnom sastavljanje upute u ovom primjeru][Example3]

![Opseg dvosmjerni mreže s NVA, NSG i UDR][9]

#### <a name="environment-description"></a>Opis okruženje
U ovom primjeru prikazuje se na pretplatu koja sadrži sljedeće:

- Tri servise u oblaku: "SecSvc001", "FrontEnd001" i "BackEnd001"
- Virtualne mreže, "CorpNetwork" s tri podmreže: "SecNet", "Sučelju" i "Pozadinskog"
- Mrežni virtualne uređaj, u ovom slučaju u vatrozid, povezani podmreže SecNet
- Windows server koja predstavlja aplikacije web-poslužitelj ("IIS01")
- Dva poslužitelja sustava Windows koji predstavljaju aplikacije pozadinskih poslužitelja ("AppVM01", "AppVM02")
- Windows server koja predstavlja DNS poslužitelja ("DNS01")

Skripte i predložak Azure upravljanja resursima potražite u članku [Sastavljanje detaljne upute][Example3].

#### <a name="udr-description"></a>Opis UDR
Prema zadanim postavkama sljedeće usmjerava sustava definiraju se kao:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Na VNETLocal uvijek je definirani adresu prefix(es) virtualne mreže za tu određenu mrežu (to jest, mijenja iz virtualne mreže virtualne mrežu, ovisno o tome kako je definiran svaki određene virtualne mreže). Preostali usmjerava sustava statični su i zadane kao što je naznačeno u tablici.

U ovom primjeru dvije tablice usmjeravanja stvaraju, jedna za podmreže sučelja i stražnje. Statički smjerovi prikladna za dani podmreže umetnut svaku tablicu. Radi u ovom se primjeru svaki tablica ima tri usmjerava koji usmjerili sav promet (0.0.0.0/0) kroz vatrozid (Dalje parametra = virtualnu potražite IP adresu):

1. Lokalni podmreže promet s bez sljedeći parametra definiraju tako da dopušta promet lokalnoj podmreži da biste zaobišli vatrozida.
2. Virtualna mrežnog prometa s na sljedeći parametra definiran kao vatrozid; To nadjačava zadano pravilo koji omogućuje lokalni virtualne mrežni promet za usmjeravanje izravno.
3. Sve preostale promet (0 na 0) s na sljedeći parametra definiran kao vatrozida.

>[AZURE.TIP] Nemate lokalnoj podmreži stavka na UDR će prekinuti komunikacije lokalne podmreže. 
> - U našem primjeru 10.0.1.0/24 koja pokazuje na VNETLocal je važnosti kao suprotnom paketa iz web-poslužitelj (10.0.1.4) namijenjene prikazivanju na drugi lokalni poslužitelj (na primjer) 10.0.1.25 neće uspjeti kao one će se slati putem NVA će ga poslati podmreži, a podmreži će ponovno pošaljite ga na NVA i tako dalje.
> - Vjerojatno petlje usmjeravanje su obično veća aparata višestruki nic izravno povezani svaki podmreže oni komunicirate, koji je često tradicionalni, informacije o lokalnom aparata. 

Kada se stvaraju tablice usmjeravanja, oni su povezane s njihovim podmreže. Pristupne podmreže usmjeravanje tablice kad stvorili i vezana uz podmreži, izgleda ovako:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

>[AZURE.NOTE] UDR sada se mogu primijeniti podmreže pristupnika povezano elektronička ExpressRoute.
>
> Primjeri kako omogućiti opseg mreže s ExpressRoute ili web-mjesto s mrežom prikazane su u primjere 3 i 4.


#### <a name="ip-forwarding-description"></a>Prosljeđivanje IP opis
Prosljeđivanje IP je značajka suradnika na UDR. To je postavka na virtualne uređaj koji omogućuje primanje promet nisu posebno adresirane na uređaj i prosljeđivanje tog promet na odredište ultimate.

Ako, na primjer, ako promet s AppVM01 izvrši zahtjeva za poslužitelj DNS01, UDR bi usmjeravanje to u vatrozid. Omogućeno prosljeđivanje IP promet za odredište DNS01 (10.0.2.4) je prihvatio potražite (10.0.0.4) i zatim proslijediti odredište ultimate (10.0.2.4). Bez IP prosljeđivanje omogućena na vatrozid, promet ne prihvaća se tako da na uređaj iako usmjeravanje tablica ima vatrozid kao sljedeći put. Da biste koristili virtualne potražite je važnosti da Imajte na umu da biste omogućili IP prosljeđivanja u kombinaciji s UDR.

#### <a name="nsg-description"></a>Opis NSG
U ovom primjeru granu NSG ugrađen i zatim učitati pomoću jedne pravila. Ovu grupu je povezana samo s podmreže sučelja i stražnje (ne SecNet). Deklarativno u tijeku je ugrađena sljedeća pravila:

- Odbijen je bilo koji promet (svi priključci) s Interneta na cijelu virtualne mreže (sve podmreže).

U ovom se primjeru koriste NSGs, čija je Glavna svrha je kao sekundarni sloj obrane ručno konfiguraciji. Cilj je da biste blokirali sve dolazni promet s Interneta na sučelja ili pozadinsku podmreže. Promet samo treba tijeka kroz SecNet podmreži omogućili vatrozida (te, ako je potrebno, na podmreže sučelja ili pozadinsku). Plus, s pravilima UDR na mjestu, bilo koji promet koji nije pretvorite u podmreže sučelja ili pozadinsku bi preusmjereni izvan vatrozidu (uz pomoć UDR). Vatrozid vidjeti to kao asimetričnim tijek i želite ispustite odlazni promet. Stoga postoje tri slojeva zaštite zaštita u podmreže:
- Otvaranje krajnje točke na FrontEnd001 i BackEnd001 servisa u oblaku.
- NSGs odbijanju promet s Interneta.
- Vatrozid za ispuštanje asimetričnim promet.

Jednu točku zanimljive vezane uz NSG u ovom primjeru je da sadrži samo jedno pravilo koje je odbiti internetski promet za cijelu virtualne mreže, uključujući podmreže sigurnost. Međutim, budući da u NSG samo vezana uz podmreže sučelja i stražnje, pravilo nije obrađen na promet ulaznog u podmreži sigurnost. Zbog toga promet teče će podmreže sigurnost.

#### <a name="firewall-rules"></a>Pravila vatrozida
Na vatrozid, pravila za prosljeđivanje moraju se stvoriti. Budući da je vatrozid blokira ili prosljeđivanja sve ulaz, Izlaz i intra-virtualne mrežnog prometa, potrebne su mnoge pravila vatrozida. Osim toga, sav dolazni promet će kliknite sigurnost javnu IP adresu (na različitim priključci), obraditi vatrozida. Preporučenim načinom rada je dijagram logičke tokova prije no što postavite na podmreže i pravila vatrozida da biste izbjegli dorada radi korištenja kasnije. Na slici u nastavku je logički prikaz pravila vatrozida u ovom primjeru:
 
![Logička prikaz pravila vatrozida][10]

>[AZURE.NOTE] Na temelju potražite virtualne mreže koristi priključke za upravljanje razlikovat će se. U ovom primjeru vatrozid NextGen Barracuda poziva, koji koristi priključke 22, 801 i 807. Potražite u dokumentaciji dobavljača uređaj da biste pronašli točno priključke za upravljanje uređaja koji koristi.

#### <a name="firewall-rules-description"></a>Opis pravila vatrozida
Na prethodni logičke dijagramu podmreže sigurnost se ne prikazuje. To je zato Vatrozid je samo resursa na tom podmreži i ovaj dijagram koji prikazuje pravila vatrozida i kako ih logično dopustiti ili zabraniti promet tokova, ne stvarne usmjerena put. Vanjski priključke odabrali za RDP promet koji su na višem ranged priključke (8014 – 8026) i odabranima Pomalo poravnali s zadnje dvije okteta od lokalna IP adresa za lakše čitljivosti (Ako, na primjer, lokalni poslužitelj adresa 10.0.1.4 povezan s vanjskog priključka 8014). Sve veći koje nisu sukobljenih priključke, međutim, može koristiti.

U ovom primjeru potrebna vam je sedam vrste pravila:

- Vanjski pravila (za dolazni promet):
  1.    Pravila upravljanja Vatrozid: ovu aplikaciju preusmjeravanje pravilo dopušta promet za prosljeđivanje upravljanje priključaka od potražite virtualne mreže.
  2.    Pravila RDP (za svaki Windows server): ta četiri pravila (jedan za svaki poslužitelj) dopustiti upravljanje pojedinačnim poslužitelja putem RDP. To također se objediniti u jedno pravilo, ovisno o mogućnostima potražite virtualne mreže koristi.
  3.    Aplikacija promet pravila: postoje dva ta pravila, najprije za promet sučelja web, a drugi za pozadinsku promet (na primjer, web-poslužitelj sloju podataka). Konfiguriranje ta pravila ovisi o arhitektura mreže (gdje poslužitelja smještaju) i promet tokova (kojem ga smjeru tokova promet i priključaka koji koriste).
      - Prvo pravilo dopušta promet stvarni aplikacije dosegne poslužitelj aplikacije. Dok se pravila omogućili za sigurnost i upravljanje njima, aplikacija promet su pravila što omogućuje vanjske korisnike ili services da biste pristupili u aplikacijama. U ovom se primjeru postoji jedan web-poslužitelj na priključak 80. Stoga aplikacije pravila vatrozida za jednu preusmjerava dolazni promet vanjske IP, na web-poslužiteljima interne IP adresu. Sesije preusmjereni promet želite prevesti putem NAT interni poslužitelj.
      - Drugo pravilo je pozadinske pravilo da biste omogućili web-poslužitelj razgovarati s poslužitelja AppVM01 (ali ne AppVM02) putem bilo koji priključak.
- Interna pravila (za intra-virtualne mrežnog prometa)
  4.    Izlazni Internet pravilo: Ovo pravilo dopušta promet s mreže za prosljeđivanje odabrane mreže. Tim se pravilom obično je zadano pravilo već na vatrozidu, ali u stanje onemogućenosti. U ovom primjeru želite omogućiti to pravilo.
  5.    Pravilo DNS: Ovo pravilo omogućuje samo DNS (priključak 53) promet za prosljeđivanje na DNS poslužitelj. U ovom okruženju Većina promet s sučelje za pozadinska je blokirana. Tim se pravilom posebno omogućuje DNS iz bilo kojeg lokalnoj podmreži.
  6.    Podmreže podmreže pravilo: Ovo pravilo je omogućiti bilo koji poslužitelj na podmreži pozadinsku povezati s bilo kojeg poslužitelja na pristupnom podmreže (ali ne obrnuto).
- Fail-Safe pravilo (za promet koji ne zadovoljava bilo koji od prethodna):
  7.    Odbijanje sve promet pravilo: to mora uvijek biti konačni pravilo (pomoću prioritet), a kao Ako tijek prometa ne odgovaraju nijednom prethodni pravila će ispušteni tim pravilom. Ovo je zadana pravila i obično aktiviran. Promjene nisu potrebni su obično.

>[AZURE.TIP] Drugo pravilo promet za aplikaciju, da biste pojednostavnili u ovom se primjeru bilo koji priključak je dopušteno. U scenariju s realni najčešće priključka i rasponi adresa mora se koristiti da biste smanjili plošni napada ovog pravila.

Kada sve prethodne pravila stvaraju, važno je da biste pregledali prioritet svako pravilo da biste bili sigurni promet će dopušten ili zabranjen po želji. U ovom primjeru pravila nalaze se prioritetu.

#### <a name="conclusion"></a>Zaključak
To je složenije, ali potpuniji način zaštite i izoliranja od prijašnjih primjera mreže. (Primjer 2 štiti samo aplikacije i primjera 1 samo izolira podmreže). Ovaj dizajn omogućuje praćenje promet u oba smjera i štiti ne samo poslužitelj ulazne aplikacije, ali nameće mreže sigurnosna pravila za sve poslužitelje na ovoj mreži. Uz to, ovisno o tome potražite koristi, promet Puni nadzor i planiranje obaviještenosti mogu se postići. Dodatne informacije potražite u članku [Sastavljanje detaljne upute][Example3]. Ove upute obuhvaćaju sljedeće:

- Upute za sastavljanje u ovom primjeru opseg mreža s skripte komponente PowerShell.
- Upute za sastavljanje u ovom se primjeru pomoću predloška Azure Voditelj resursa.
- Detaljne opise svake UDR NSG naredba i pravila vatrozida.
- Detaljne promet scenarija tijeka, prikazuje kako promet dopušten ili zabranjen svaki sloju.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-virtual-private-network-vpn"></a>Primjer 4: Dodavanje veze hibridnog sa na web-mjesto, virtualne potražite virtualne privatne mreže (VPN-a)
[Natrag na brz početak](#fast-start) | Detaljne upute za sastavljanje dostupna uskoro

![Opseg mreža uz NVA povezanog hibridnog mreže][11]

#### <a name="environment-description"></a>Opis okruženje
Hibridno mreže pomoću potražite virtualne mreže (NVA) mogu se dodati na bilo koje vrste mrežnih opseg opisane u primjerima 1, 2 ili 3.

Kao što je prikazano na slici prethodne VPN vezu putem Interneta (web-mjesta na odredište) se koristi za povezivanje lokalne mreže Azure virtualne mrežom putem programa NVA.

>[AZURE.NOTE] Ako koristite ExpressRoute s mogućnošću Azure javno Peering omogućena, statički smjer trebalo stvoriti. Usmjeravanje moraju se NVA VPN IP adresom izvan vaše tvrtke Internet, a ne putem ExpressRoute WAN. NAT potrebna mogućnost ExpressRoute Azure javno Peering možete prekinuti sesiju VPN-a.

Kada je VPN-om na mjestu, na NVA postaje središnje koncentrator za sve mreže i podmreže. Prosljeđivanje pravila vatrozida određivanje tokova koji promet dopušteno, su prevesti putem NAT, vas preusmjerava ili ispuštaju (čak i za tijekove promet između lokalne mreže i Azure).

Promet tokova razmatranje pažljivo, kao što su mogu se optimizirati ili smanjena tako da ovaj uzorak dizajna, ovisno o određenim slučaj upotrebe.

Korištenje okruženja ugrađen u primjeru 3, a zatim dodate web-mjesto VPN hibridnog mrežne veze, daje sljedeće dizajna:

![Opseg mreža s NVA povezani putem VPN-a web-mjesto][12]

Lokalni usmjerivač ili neki drugi uređaj mreže kompatibilan s vašeg NVA za VPN, bilo bi VPN klijenta. Ovaj uređaj fizički bi odgovorni za pokretanje i održavanje VPN vezu s vašeg NVA.

Logički da biste NVA, mreža izgleda četiri zasebna "sigurnosne zone" pomoću pravila na NVA primarni director prometa između te zone u tijeku:

![Domainexplorer-BP iz perspektive NVA][13]

#### <a name="conclusion"></a>Zaključak
Zbrajanje mrežne veze web-mjesto hibridnog VPN-a za Azure virtualne mreže mogu proširiti lokalne mreže u Azure siguran način. Pomoću VPN vezu, promet za svoju šifriran i preusmjerava putem Interneta. NVA u ovom primjeru predstavlja središnje mjesto za provođenje i upravljanje sigurnosna pravila. Dodatne informacije potražite u članku sastavljanje detaljne upute (forthcoming). Ove upute obuhvaćaju sljedeće:

- Upute za sastavljanje u ovom primjeru opseg mreža s skripte komponente PowerShell.
- Upute za sastavljanje u ovom se primjeru pomoću predloška Azure Voditelj resursa.
- Detaljne promet scenarija tijeka, prikazuje tok promet kroz ovaj dizajn.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn"></a>Primjer 5: Dodavanje hibridnog povezivanja s web-mjesto, Azure pristupnika VPN-a
[Natrag na brz početak](#fast-start) | Detaljne upute za sastavljanje dostupna uskoro

![Opseg mreže s mrežom povezanog hibridnog pristupnika][14]

#### <a name="environment-description"></a>Opis okruženje
Hibridnog mreže pomoću pristupnik za Azure VPN-a mogu se dodati na bilo koje vrste opseg mreže opisane u primjerima 1 ili 2.

Kao što je prikazano na prethodnoj slici, VPN vezu putem Interneta (web-mjesta na odredište) se koristi za povezivanje lokalne mreže Azure virtualne mrežom putem pristupnik za Azure VPN-a.

>[AZURE.NOTE] Ako koristite ExpressRoute s mogućnošću Azure javno Peering omogućena, statički smjer trebalo stvoriti. Usmjeravanje moraju se NVA VPN IP adresom izvan vaše tvrtke Internet, a ne putem ExpressRoute WAN. NAT potrebna mogućnost ExpressRoute Azure javno Peering možete prekinuti sesiju VPN-a.

Sljedeća slika prikazuje dva rubova mreže u tu mogućnost. Na prvom rubu NVA i NSGs kontrolirati tokova promet za intra-Azure mreže i između Azure i Interneta. Drugi rub je pristupnika Azure VPN, rub posve zasebne i Izolirani mreže između lokalnog i Azure.

Promet tokova razmatranje pažljivo, kao što su mogu se optimizirati ili smanjena tako da ovaj uzorak dizajna, ovisno o određenim slučaj upotrebe.

Korištenje okruženja ugrađena primjer 1, a zatim dodate web-mjesto VPN hibridnog mrežne veze, daje sljedeće dizajna:

![Opseg mreža s pristupnika povezani putem veze ExpressRoute][15]

#### <a name="conclusion"></a>Zaključak
Zbrajanje mrežne veze web-mjesto hibridnog VPN-a za Azure virtualne mreže mogu proširiti lokalne mreže u Azure siguran način. Korištenje nativni pristupnika Azure VPN, promet za svoju je IPSec šifrirane i usmjerava putem Interneta. Osim toga, pomoću Azure VPN pristupnika možete unijeti donjem mogućnost trošak (bez dodatnih licenciranje troška s drugih proizvođača NVAs). To je najčešće najekonomičniji u primjeru 1, kojoj se koristi bez NVA. Dodatne informacije potražite u članku sastavljanje detaljne upute (forthcoming). Ove upute obuhvaćaju sljedeće:

- Upute za sastavljanje u ovom primjeru opseg mreža s skripte komponente PowerShell.
- Upute za sastavljanje u ovom se primjeru pomoću predloška Azure Voditelj resursa.
- Detaljne promet scenarija tijeka, prikazuje tok promet kroz ovaj dizajn.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Primjer 6: Dodavanje veze hibridnog sa ExpressRoute
[Natrag na brz početak](#fast-start) | Detaljne upute za sastavljanje dostupna uskoro

![Opseg mreže s mrežom povezanog hibridnog pristupnika][16]

#### <a name="environment-description"></a>Opis okruženje
Hibridno mreže putem veze privatne peering ExpressRoute mogu dodati bilo koje vrste opseg mreže opisane u primjerima 1 ili 2.

Kao što je prikazano na prethodnoj slici, ExpressRoute privatne peering pruža izravne veze između lokalne mreže i Azure virtualne mreže. Promet transits samo s mrežom davatelja usluge i na mreži Microsoft Azure nikad dodirivanje s Internetom.

>[AZURE.NOTE] Postoje određene ograničenja prilikom korištenja UDR s ExpressRoute, zbog složenost koristi u Azure virtualne pristupnika dinamički usmjeravanja. Ovo su na sljedeći način:
>
>- UDR treba primijeniti u podmreži pristupnika povezano ExpressRoute povezane Azure virtualne pristupnika.
>- ExpressRoute povezane Azure virtualne pristupnika nije moguće NextHop uređaja za druge UDR vezana podmreže.
>
>
<br />

>[AZURE.TIP] Korištenje ExpressRoute zadržava tvrtke mrežnog prometa s Internetom radi bolje sigurnosti i značajno povećati performanse. Omogućuje i ugovore o razini usluge davatelja ExpressRoute. Azure pristupnika možete proslijediti do 2 Gb/s s ExpressRoute, dok s web-mjesto VPN-ovima Azure pristupnika Maksimalna propusnost 200 Mb/s.

Kako se vidi u sljedećem su dijagramu, uz tu se mogućnost okruženje sada ima dva rubova mreže. NVA i NSG kontrolirati tokova promet za intra-Azure mreže i između Azure i Internet, dok je pristupnika posve zasebne i Izolirani mreže rub između lokalnog i Azure.

Promet tokova razmatranje pažljivo, kao što su mogu se optimizirati ili smanjena tako da ovaj uzorak dizajna, ovisno o određenim slučaj upotrebe.

Korištenjem u okruženju ugrađena primjer 1, a zatim dodate na ExpressRoute hibridnog mrežom, daje sljedeće dizajna:

![Opseg mreža s pristupnika povezani putem veze ExpressRoute][17]

#### <a name="conclusion"></a>Zaključak
Dodatak programa ExpressRoute privatno Peering mrežne veze možete proširiti lokalne mreže u Azure u u sigurnom, niže Latencija, veća izvođenje način. Pomoću nativnog pristupnika Azure, kao u ovom primjeru omogućuje i na donjem mogućnost trošak (ne dodatne licenciranje s NVAs trećih strana). Dodatne informacije potražite u članku sastavljanje detaljne upute (forthcoming). Ove upute obuhvaćaju sljedeće:

- Upute za sastavljanje u ovom primjeru opseg mreža s skripte komponente PowerShell.
- Upute za sastavljanje u ovom se primjeru pomoću predloška Azure Voditelj resursa.
- Detaljne promet scenarija tijeka, prikazuje tok promet kroz ovaj dizajn.

## <a name="references"></a>Reference
### <a name="helpful-websites-and-documentation"></a>Korisni web-mjesta i u dokumentaciji o
- Pristup Azure s Azure Voditelj resursa:
- Pristup Azure sa servisom PowerShell: [https://azure.microsoft.com/documentation/articles/powershell-install-configure/](./powershell-install-configure.md)
- Virtualna mrežne dokumentacije: [https://azure.microsoft.com/documentation/services/virtual-network/](https://azure.microsoft.com/documentation/services/virtual-network/)
- Mrežne dokumentacije sigurnosne grupe: [https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/](./virtual-network/virtual-networks-nsg.md)
- Korisnički definirane usmjeravanje dokumentacija: [https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/](./virtual-network/virtual-networks-udr-overview.md)
- Azure virtualne pristupnici: [https://azure.microsoft.com/documentation/services/vpn-gateway/](https://azure.microsoft.com/documentation/services/vpn-gateway/)
- VPN-ovi web-mjesto: [https://azure.microsoft.com/documentation/articles/vpn-gateway-create-site-to-site-rm-powershell](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- Dokumentacija ExpressRoute (svakako pogledajte odjeljke "Prvi koraci" i "Kako da biste"): [https://azure.microsoft.com/documentation/services/expressroute/](https://azure.microsoft.com/documentation/services/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Dijagram toka sigurnosne mogućnosti"
[1]: ./media/best-practices-network-security/compliancebadges.png "Azure usklađenost tipki"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "Značajke sigurnosti Azure"
[3]: ./media/best-practices-network-security/dmzcorporate.png "DMZ u korporacijskom mrežom"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "Azure sigurnosna arhitektura"
[5]: ./media/best-practices-network-security/dmzazure.png "DMZ u Azure virtualne mreže"
[6]: ./media/best-practices-network-security/dmzhybrid.png "Hibridno mreža s tri sigurnosna ograničenja"
[7]: ./media/best-practices-network-security/example1design.png "Dolazni DMZ s NSG"
[8]: ./media/best-practices-network-security/example2design.png "Dolazni DMZ s NVA i NSG"
[9]: ./media/best-practices-network-security/example3design.png "Dvosmjerni DMZ s NVA, NSG i UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "Logička prikaz pravila vatrozida"
[11]: ./media/best-practices-network-security/example4designoptions.png "DMZ s NVA povezani hibridnog mreže"
[12]: ./media/best-practices-network-security/example4designs2s.png "DMZ s NVA povezani putem VPN-a web-mjesto"
[13]: ./media/best-practices-network-security/example4networklogical.png "Domainexplorer-BP iz perspektive NVA"
[14]: ./media/best-practices-network-security/example5designoptions.png "DMZ s Azure pristupnika povezani mreže Hibridne web-mjesto"
[15]: ./media/best-practices-network-security/example5designs2s.png "DMZ s Azure pristupnika pomoću web-mjesto VPN-a"
[16]: ./media/best-practices-network-security/example6designoptions.png "DMZ s Azure pristupnika povezani ExpressRoute hibridnog mreže"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "DMZ s pristupnik za Azure ExpressRoute vezom"

<!--Link References-->
[Example1]: ./virtual-network/virtual-networks-dmz-nsg-asm.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
