<properties
   pageTitle="Najbolje prakse za Azure mreže sigurnost | Microsoft Azure"
   description="Ovaj članak sadrži skup Praktični savjeti za mrežni sigurnost korištenje ugrađena Azure mogućnosti."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="swadhwa"
   editor="TomShinder"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/25/2016"
   ms.author="TomSh"/>

# <a name="azure-network-security-best-practices"></a>Najbolje prakse za Azure mreže sigurnosti

Microsoft Azure omogućuje povezivanje virtualnim strojevima i aparata na drugim uređajima umreženi tako da ih stavite na Azure virtualne mreže. Azure virtualne mreže je konstrukta virtualne mreže koja omogućuje povezivanje virtualne mrežne kartice virtualne mreže da biste dopustili TCP/IP-bitne komunikaciju između omogućili uređaja. Azure virtualnim strojevima koji su povezani s mrežom virtualne Azure se možete povezati s uređajima na istoj Azure virtualne mreži, različite Azure virtualne mrežama na Internetu ili čak i na lokalni mreža.

U ovom članku smo obrađuje zbirka najbolje prakse za sigurnost Azure mreže. Najbolje prakse potječu iz naših doživljaj Azure mreže i iskustva klijenata kao što su sami.

Za svaki preporučenim načinom rada ne možemo ćete u članku se objašnjava:

- Što je najbolji način
- Zašto na koje želite omogućiti tu preporučenim načinom rada
- Ako omogućite preporučenim načinom rada što bi moglo biti rezultat
- Moguća alternativa najbolja praksa
- Kako saznati da biste omogućili najbolja praksa

U ovom se članku najbolje prakse sigurnost mreže Azure temelji se na mišljenje na suglasnost i mogućnosti platforme Azure i skupove, kao što je postoje trenutku ovaj članak. Mišljenja i tehnologije mijenjaju tijekom vremena, a u ovom se članku ažurirat će se redovito u skladu s tim promjenama.

Azure mreže sigurnost najbolje prakse spominju u ovom članku obuhvaćaju sljedeće:

- Logički podmreže segmenta
- Kontrola usmjeravanje ponašanje
- Omogućivanje prisilne tuneliranja
- Korištenje aparata virtualne mreže
- Implementacija DMZs za sigurnost zoning
- Izbjegavanje izlaganje Internet s namjenski WAN veze
- Optimiziranje vrijeme aktivnosti i performanse
- Korištenje globalni opterećenja
- Onemogućavanje pristupa RDP Azure virtualnim strojevima
- Centar za sigurnost Omogući Azure
- Širenje vaše podatkovnog centra za Azure


## <a name="logically-segment-subnets"></a>Logički podmreže segmenta

[Azure virtualne mreže](https://azure.microsoft.com/documentation/services/virtual-network/) su slične LAN-a na lokalnu mrežu. Smisao Azure virtualne mreže je stvoriti jednu IP adresu utemeljen na prostor privatnom na kojem možete postaviti sve na [virtualnim računalima sustava Azure](https://azure.microsoft.com/services/virtual-machines/). Privatne IP adresa razmake dostupne su u predmete A (10.0.0.0/8), Klasa B (172.16.0.0/12) i klase C raspona (192.168.0.0/16).

Slično onome što učinite lokalnog, želite fazi veći prostor adresu u podmreže. Koji se temelji na [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) subnetting načela možete koristiti da biste stvorili vaše podmreže.

Usmjeravanje između podmreže dogodit će se automatski, a ne morate ručno konfigurirati tablice usmjeravanja. Međutim, zadana je postavka da postoje nema pristup kontrolama za mrežni između podmreže stvorite Azure virtualne mreže. Da biste stvorili mrežni pristup kontrolama između podmreže, morate staviti nešto između na podmreže.

Neke stvari koje možete koristiti da biste izvršili taj zadatak je [Mreža sigurnosne grupe](../virtual-network/virtual-networks-nsg.md) (NSG). NSGs su jednostavni s praćenjem stanja paketa provjere uređaje koji koriste 5-torke (u IP izvora, izvorni Priključak, IP odredište, odredišni priključak i protokol sloja 4) pristup da biste stvorili Dopusti/odbijanje pravila za mrežni promet. Možete dopustiti ili zabraniti promet i iz jedne IP adresa i s više IP adresa ili čak i iz cijelog podmreže.

Korištenje NSGs za upravljanje pristupom mreži između podmreže omogućuje vam da biste postavili resurse koji pripadaju iste sigurnosne zone ili uloga u vlastite podmreže. Ako, na primjer, zamislite da je jednostavno 3 sloja aplikaciju koja ima web razina, razina logike za aplikacije i sloju baze podataka. Postavite virtualnim strojevima koji pripadaju svaki od tih razine u vlastite podmreže. Zatim NSGs koristite da biste kontrolirali promet između na podmreže:

- Web razina virtualnim strojevima možete pokrenuti samo veze na računalima logike aplikacije i prihvaća samo veze s Interneta
- Aplikacija logike virtualnim strojevima možete pokrenuti samo veze s sloju baze podataka i prihvaća samo veze s web razina
- Baza podataka sloju virtualnim strojevima nije moguće započeti vezu s nečim izvan vlastite podmreže i prihvaća samo veze iz razina logike aplikacije

Da biste saznali više o sigurnosnim grupama s mreže i kako ih možete koristiti da biste logično fazi virtualne mreže Azure, pročitajte članak [što je mreža sigurnosne grupe](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Kontrola usmjeravanje ponašanje

Kada postavite virtualnog računala na mreži virtualne Azure, primijetit ćete da virtualnog računala mogu povezati s bilo koje druge virtualnog računala na istoj Azure virtualne mreži, čak i ako su na virtualnim strojevima na različitim podmreže. Razlog zašto je to moguće je da postoji skup sustava usmjerava omogućene prema zadanim postavkama, a dopušta tu vrstu komunikacije. Ove zadane usmjerava Dopusti virtualnim strojevima na isti Azure virtualne mreže da biste započeli veze s drugom i s Internetom (za izlaznu komunikaciju s Internetom samo).

Dok su korisne su za mnoge scenariji za implementaciju sustava usmjerava zadane, ponekad je kada želite da biste prilagodili usmjeravanje konfiguraciju za vaše implementacije. Te prilagodbe će vam omogućiti da biste konfigurirali sljedeće adrese skoka dosegne određenu odredišta.

Preporučujemo da konfigurirate korisnički definirana usmjerava ako pokrenete potražite sigurnost virtualne mreže koje ćemo objasniti što u noviji preporučenim načinom rada.

> [AZURE.NOTE] usmjerava sustava zadani funkcioniraju u većini slučajeva i korisnički definirana usmjerava nisu potrebni.

Dodatne informacije o korisnički definirana usmjerava i kako ih konfigurirati tako da pročitate članak [što su korisnički definirana usmjerava i prosljeđivanje IP](../virtual-network/virtual-networks-udr-overview.md).

## <a name="enable-forced-tunneling"></a>Omogućivanje prisilne tuneliranja

Da biste bolje razumjeli prisilne tuneliranje je korisno je znati koje "podijeljene tuneliranje" je.
Najčešći primjera tuneliranje podijeljenog je vidjeti s VPN veza. Zamislite da uspostavljate VPN vezu iz sobe za hotelski s korporacijskom mrežom. Ova veza omogućuje vam da biste se povezali s resursima na mreži tvrtke i svu komunikaciju s resursima na mreži tvrtke proći kroz tunelom VPN-a.

Što se događa kada se želite povezati s resursima na Internetu? Kada je omogućen Podijeli tuneliranje, te veze Idite izravno s Internetom, a ne i putem tunelom VPN-a. Razmislite o nekim Stručnjaci za sigurnost to potencijalni rizik i zbog toga preporučujemo da Podijeli tuneliranje onemogućeno i sve veze, one namijenjene prikazivanju na Internetu i one namijenjene prikazivanju na resurse za tvrtke, proći kroz tunelom VPN-a. Prednost na taj način je da veza s Internetom pa prisilno putem uređaja za sigurnost korporacijskom mrežom koji ne može biti slučaj VPN klijent povezani s Internetom izvan tunelom VPN-a.

Sada ćemo premjestili vratite sve postavke na virtualnim strojevima na Azure virtualne mreže. Usmjerava zadano za Azure virtualne mreže dopusti virtualnih računala da biste započeli promet s Internetom. To predugo mogu predstavljati sigurnosni rizik, kao te izlazne veze može povećati površina napada virtualnog računala te se leveraged Napadači.
Zbog toga preporučujemo da omogućite prisilne tuneliranje na virtualnim strojevima kad imate više lokacija povezivanje između virtualne mreže Azure i lokalne mreže. Ne možemo će objasniti unakrsno povezivanje s lokalnom u nastavku ovog Azure umrežavanje najbolje prakse dokumenta.

Ako nemate unakrsni lokalnu vezu, provjerite je li iskoristiti sve prednosti mreže sigurnosnih grupa (spomenuli) ili Azure virtualne mreže sigurnost aparata (spominju Dalje) da biste spriječili izlazne veze s Internetom iz na virtualnim računalima sustava Azure.

Dodatne informacije o prisilno tuneliranje i kako omogućiti, pročitajte članak [Konfiguriranje prisilno tuneliranje pomoću komponente PowerShell i upravljanja resursima Azure](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Korištenje aparata virtualne mreže

Dok mreže sigurnosne grupe i korisnički definirani usmjeravanje možete unijeti određene mjere mrežne sigurnosti na mrežu i prijenosa Slojevi [modela upravljački](https://en.wikipedia.org/wiki/OSI_model), dva su će biti situacijama gdje ćete želite ili morati omogućiti sigurnosti kod visoke razine stog. U tim situacijama, preporučujemo da implementacije virtualne mreže sigurnost aparata nudi Azure partnera.

Azure mreže sigurnost aparata možete isporuku znatno poboljšane razine sigurnosti putem koje nudi mrežne razine kontrole. Mrežni sigurnosne mogućnosti nudi virtualne mreže sigurnost aparata obuhvaćaju sljedeće:

- Firewalling
- Otkrivanje podataka i podataka za sprječavanje
- Upravljanje slabe
- Kontrola aplikacije
- Otkrivanje značajkom utemeljen na mreži
- Filtriranje weba
- Antivirusnog softvera
- Zaštita Botnet

Ako tražite višu razinu sigurnosti mreže od možete dobiti s kontrolama razinu pristupa za mrežu, pa preporučujemo istražili i implementacija aparata sigurnost Azure virtualne mreže.

Da biste saznali o koje aparata sigurnost Azure virtualne mreže dostupne su i svoje mogućnosti, posjetite [Azure Marketplace](https://azure.microsoft.com/marketplace/) i traženje "Sigurnost" i "mrežne sigurnosti".

##<a name="deploy-dmzs-for-security-zoning"></a>Implementacija DMZs za sigurnost zoning
Na DMZ ili je "opseg mreža" fizičke ili logičke mreže segment koji omogućuje dodatne slojeva zaštite između svoje imovine i Interneta. Cilj na DMZ je da biste postavili specijalizirane mrežne uređaje za kontrolu pristupa na rub DMZ mrežu tako da samo željene promet je dopušteno prošle mrežni uređaj sigurnosti i u Azure virtualne mreže.

DMZs korisne su jer upravljanje mrežni pristup kontrola, možete se fokusirati nadzor, prijava i izvješćivanje o pogreškama na uređajima na rubu Azure virtualne mreže. Ovdje bi obično omogućite sprječavanje DDoS, sprječavanje otkrivanje/podataka podataka sustave (ID-ovi i IP-ovi), pravila vatrozida i pravila, filtriranje weba, antimalware mreže i više. Sigurnost mrežne uređaje sjesti između interneta i virtualne mreže Azure i imaju sučelja na oba mrežama.

Dok je osnovni dizajnerski na DMZ, postoje mnoge različite dizajne DMZ, kao što su paralelno pokretanje većeg tri matični, višestruki matični, i drugi korisnici.

Preporučujemo da za sve implementacije osiguran razmotrite implementacija DMZ da biste poboljšali razinu sigurnosti mreže za Azure resurse.

Da biste saznali više o DMZs i kako ih uvesti u Azure, pročitajte članak [Microsoftovim servisima u Oblaku i zaštitu mreže](../best-practices-network-security.md).

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Izbjegavanje izlaganje Internet s namjenski WAN veze
Mnoge organizacije odabrali ste usmjeravanje hibridnog IT. U hibridnog IT neke tvrtke informacije imovine su u Azure, dok drugi ostaju na lokalni. U mnogim slučajevima neke komponente servisa će imati servisu Azure dok drugih komponenti ostaju na lokalni.

U hibridnog IT scenarij postoji obično neke vrste povezivanja više lokacija. To više lokacija povezivanje omogućuje tvrtka povezati svoje lokalne mreže Azure virtualne mreže. Dostupne su dvije rješenja za povezivanje s više lokacija:

- Web-mjesto VPN-a
- ExpressRoute

[Web-mjesto VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) predstavlja virtualne privatne veze između lokalne mreže i Azure virtualne mreže. Povezivanje odvija putem Interneta, a omogućuje informacija "tunelom" unutar šifriranu vezu između mreže i Azure. Sigurna, starijih tehnologija koji je uveden po korporacije svih veličina za desetljeća je VPN-a web-mjesto. Šifriranje tunelom izvodi se [IPsec tunelom način](https://technet.microsoft.com/library/cc786385.aspx).

Dok je web-mjesto VPN tehnologija pouzdanih, pouzdanog i utvrđene, promet unutar na tunelom prolaziti s Internetom. Osim toga, propusnosti relativno ograničeno maksimalno o 200Mbps.

Ako tražite izvanredne razinu sigurnosti ili performansi za više lokacija veze, preporučujemo da koristite Azure ExpressRoute za stanje veze na više lokacija. ExpressRoute je Namjenska WAN veze između lokalne lokacije ili davatelja hostinga sustava Exchange. Budući da je telco veze, vaši podaci ne putovanja putem Interneta i stoga je izložen potencijalne opasnosti u komunikaciji internetske ugrađeno.

Da biste saznali više o tome kako funkcionira Azure ExpressRoute i kako implementirati, pročitajte članak [ExpressRoute Tehnički pregled](../expressroute/expressroute-introduction.md).

## <a name="optimize-uptime-and-performance"></a>Optimiziranje vrijeme aktivnosti i performanse
Povjerljivosti, integritet i dostupnosti (CIA) čine triad najčešće utjecajno model sigurnosti današnjeg. Povjerljivosti je o šifriranja i izjave o zaštiti privatnosti, integritet je riječ o pazeći da se podaci ne mijenjaju neovlašteno osoblje i dostupnost radi Provjerite jesu li ovlaštene osobe moći pristupiti podacima oni imaju dozvolu za pristup. Neuspješna instalacija u bilo koju od tih područja označava potencijalne kršenja sigurnosti programa.

Dostupnost možete smatrati se o vrijeme aktivnosti i performanse. Ako je neki servis prema dolje, informacije ne može pristupiti. Ako su performanse pa nisku kao bili nestabilan, zatim ćemo možete razmislite o podataka se ne može pristupiti. Stoga iz perspektive sigurnost, moramo učinite ono možemo da biste provjerili naših usluga optimalnih vrijeme aktivnosti i performanse.
Popularne i učinkovit način koristi za unaprjeđenje dostupnosti i performanse jest korištenje opterećenja. Način distribucije mrežni promet na poslužiteljima koji su dio usluge opterećenja je. Ako, na primjer, ako imate pristupnim web-poslužiteljima kao dio na servisu, možete koristiti opterećenja distribuirati promet na vaše više poslužiteljima sučelja web.

Raspodjele promet povećava dostupnost jer će ako jedan od web-poslužiteljima postane dostupan, raspoređivača opterećenja prestanu slati promet na tom poslužitelju i preusmjeravanje prometa na poslužitelje koji su još na mreži. Performanse, opterećenja pomaže jer procesor, mreže i memorije indirektni za posluživanje zahtjeve raspodijeliti je mogućnost Učitaj raspoređen poslužiteljima.

Preporučujemo da uključivanja opterećenja kad god možete i po potrebi za usluge. Ne možemo ćete adresa appropriateness u sljedećim odjeljcima.
Na razini Azure virtualne mreže Azure omogućuje vam tri primarni učitavanje ujednačavanje mogućnosti:

- Za ujednačavanje opterećenja koji se temelji na HTTP
- Vanjski za ujednačavanje opterećenja
- Interna za ujednačavanje opterećenja

## <a name="http-based-load-balancing"></a>Za ujednačavanje opterećenja koji se temelji na HTTP
Odluka o koji poslužitelj za slanje veza pomoću značajke HTTP protokola koji se temelji na HTTP opterećenja temelji. Azure ima programa HTTP opterećenja koja se proteže po nazivu aplikacije pristupnika.

Preporučujemo vam nam Azure aplikacije pristupnika kada:

- Aplikacije koje je potrebno zahtjeve iz iste sesije korisnika/klijent dosegne istu pozadinsku virtualnog računala. Primjeri to će biti košarice košaricu aplikacije i poslužitelja pošte web.
- Aplikacije koje želite da biste oslobodili farme poslužitelja web iz SSL prekid indirektni putem značajke [SSL offload](https://f5.com/glossary/ssl-offloading) pristupnik za aplikacije.
- Aplikacija, kao što je mreže za isporuku sadržaja koji zahtijevaju više HTTP zahtjeva na istom dugoročnih TCP veza moguće usmjeriti ili učitavanje raspoređen za različite pozadinskih poslužitelja.

Da biste saznali više o funkcioniranje Azure aplikacije pristupnika i kako je možete koristiti u vašem implementacijama, pročitajte u članku [Pregled pristupnika aplikacije](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Vanjski za ujednačavanje opterećenja
Opterećenja vanjskog odvija kada su rasporediti među poslužitelja koja se nalazi u Azure virtualne mreže opterećenje dolazne veze s Interneta. Na Azure na vanjskim opterećenja dat će vam tu mogućnost, a zatim preporučujemo da ih koristiti kad ne zahtijevaju ljepljive sesije ili SSL offload.

Za razliku od utemeljen na HTTP opterećenja, vanjski raspoređivača opterećenja informacije koristi na mrežu i prijenosa Slojevi modela umrežavanje upravljački odluke koje server da biste učitali saldo vezu.

Preporučujemo da koristite vanjski opterećenja održavali [bez praćenja stanja aplikacije](http://whatis.techtarget.com/definition/stateless-app) prihvaćanja zahtjevi za razgovore putem Interneta.

Dodatne informacije o funkcioniranje Azure vanjskih raspoređivača opterećenja i kako ga možete implementirati Imajte pročitajte članak [Prvi koraci stvaranje programa Internet nasuprotne opterećenja u Voditelj resursa pomoću komponente PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Interna za ujednačavanje opterećenja
Interna opterećenja je slična vanjskih opterećenja i koristi iste mehanizam da biste učitali saldo veze s poslužiteljima iza njih. Jedina razlika je da raspoređivača opterećenja u tom slučaju prihvaća veze s virtualnim strojevima koji se ne nalaze na Internetu. U većini slučajeva, veze koje prihvaćaju za opterećenja se pokrenuo uređajima na Azure virtualne mreže.

Preporučujemo da koristite ujednačavanja scenarija koji će im to omogućili, kao što je kada morate učitati saldo veze za SQL Server ili Interna web-poslužiteljima Interna opterećenja.

Da biste saznali više o tome kako Azure Interna opterećenja funkcionira i kako je možete implementirati, pročitajte članak [Prvi koraci stvaranje internog raspoređivača opterećenja pomoću komponente PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Korištenje globalni opterećenja
Javni oblaka računalno čini je moguće uvesti globalno distributed aplikacije koje ste komponente koja se nalazi u podatkovnim centrima iz cijelog svijeta. Ovo je moguće na Microsoft Azure zbog prisutnosti globalni podatkovnog centra korisnika Azure. Za razliku od tehnologije ranije spomenutih za ujednačavanje opterećenja, globalni za ujednačavanje opterećenja omogućuje dostupnost usluge čak i kad je cijeli podatkovnim centrima može postati nedostupna.

Možete dobiti ovu vrstu ujednačavanja u Azure putem upravitelja [promet Azure](https://azure.microsoft.com/documentation/services/traffic-manager/)globalni opterećenja. Upravitelj promet čini je moguće da biste učitali saldo veza sa servisa na temelju lokacije korisnika.

Ako, na primjer, ako korisnik je želite zahtjeva za uslugu iz EU-a, veza je preusmjereni na servisa koja se nalazi u podatkovnog centra sustava EU-a. Taj dio promet upravitelja globalni učitati ujednačavanje pomaže da biste poboljšali performanse jer povezivanje s najbližim podatkovnim centrom brže nego povezivanje s podatkovnim centrima koji su daleko.

Na strani dostupnost globalni opterećenja provjerava je li na servisu je dostupna čak i ako se cijeli podatkovnog centra mora biti dostupna.

Ako, na primjer, ako Azure podatkovnog centra potrebno više nije dostupan zbog okolini razloga ili zbog kvara (kao što su regionalne mrežnih pogrešaka), veze na servisu za bi preusmjereni da biste na najbliži online podatkovnog centra. U ovom globalni opterećenja se postiže prihvaćanjem prednost DNS pravila koja možete stvoriti u upravitelju promet.

Preporučujemo da koristite Upravitelj promet za razvijate rješenja oblaka koja ima široko raspodijeljeno doseg preko više regija i zahtijeva najvišu razinu vrijeme moguće aktivnosti.

Da biste saznali više o Upravitelj promet Azure te o tome kako implementirati, pročitajte članak [što je upravitelj promet](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-to-azure-virtual-machines"></a>Onemogućavanje pristupa RDP/SSH Azure virtualnim strojevima
Nije moguće pristupiti virtualnim računalima sustava Azure pomoću [Protokola udaljene radne površine](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) i protokoli [Sigurne ljuske](https://en.wikipedia.org/wiki/Secure_Shell) (SSH). Tim protokolima omogućuju upravljanje virtualnim strojevima s udaljenog mjesta i standardnih u računalstvu podatkovnog centra.

Potencijalni sigurnosni problem s tim protokolima putem Interneta je attackers možete koristiti različite tehnike [brute prisilno](https://en.wikipedia.org/wiki/Brute-force_attack) da biste pristupili virtualnim računalima sustava za Azure. Kada se attackers ostvariti pristup, možete ugrozi druga računala u vašoj mreži virtualne Azure koristite virtualnog računala kao točku za pokretanje ili čak i napasti umreženi uređaji izvan Azure.

Zbog toga preporučujemo da onemogućite izravan pristup RDP i SSH da biste na virtualnim računalima sustava Azure s Interneta. Nakon onemogućivanja izravan pristup RDP i SSH s Interneta imate druge mogućnosti možete koristiti za pristup te virtualnim strojevima za daljinsko upravljanje:

- Točka web VPN-a
- Web-mjesto VPN-a
- ExpressRoute

[Točke-na-web-mjesta VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) je drugi pojam za daljinski pristup VPN vezu klijent poslužitelj. VPN točke mjesta omogućuje jednom korisniku za povezivanje s mrežom virtualne Azure putem Interneta. Nakon uspostavljanja veze točke web korisnik će moći koristiti RDP ili SSH za povezivanje s bilo kojeg virtualnih računala na mreži virtualne Azure koji korisnik povezan s putem VPN točke na web. Pretpostavlja se da korisnik ovlašten dosegne te virtualnih računala.

VPN točke web je dodatno zaštititi od izravne RDP ili SSH veze jer korisnik ima za provjeru autentičnosti dvaput prije povezivanja s virtualnog računala. Prvo, korisnik mora provjere autentičnosti (i ovlašteni) da biste uspostavili VPN veza mjesta točke; drugo, korisnik mora provjere autentičnosti (i ovlašteni) da biste pokrenuli sesiju RDP ili SSH.

[Web-mjesto VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) povezuje čitava mreža s drugom mrežom putem Interneta. Možete koristiti web-mjesto VPN-a povezati lokalne mreže Azure virtualne mreže. Ako pokrenete VPN web-mjesto s korisnicima na lokalnu mrežu će se moći povezati virtualnih računala u vašoj mreži virtualne Azure pomoću protokola RDP ili SSH putem VPN veza web-mjesto i zahtijeva Dopusti izravan pristup RDP ili SSH putem Interneta.

Možete unijeti funkcija slična VPN web-mjesto možete koristiti i namjenski WAN veze. Glavne razlike ima vrijednost 1. namjenski WAN veze ne prolaziti s Internetom i 2. namjenski WAN veze obično su još stabilan i performant. Azure nudi rješenje namjenski WAN veze u obliku [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="enable-azure-security-center"></a>Centar za sigurnost Omogući Azure
Centar za sigurnost Azure pomaže vam sprječavanje, otkrivanje i odgovaranje na prijetnji, a omogućuje povećati uvid u, i kontrolu nad, sigurnost Azure resurse. Pruža integriranu sigurnost nadzor i pravila upravljanja preko pretplate Azure, pomaže u otkrivanju prijetnji koje možda u suprotnom otvorite uočen i radi s Bogata paleta sigurnost rješenja.

Centar za sigurnost Azure pomaže vam optimiziranje i nadzirati sigurnost mreže:

- Pružanje preporuke o sigurnosti mreže
- Praćenje stanja mrežna konfiguracija sigurnosti
- Koja vas upozorava na mreža temeljena prijetnji oba na razinama krajnjoj točki i mreže

Preporučujemo Omogućivanje centar za sigurnost Azure za sve svoje Azure implementacije.

Da biste saznali više o centru za sigurnost Azure i kako omogućiti za vaše implementacije, pročitajte u članku [Uvod u Centar za sigurnost Azure](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Sigurno vaše podatkovnog centra širenje Azure
Mnoge enterprise IT tvrtkama ili ustanovama tražite da biste proširili u oblak umjesto rast njihovim podatkovnim centrima lokalnog. U ovom proširenja predstavlja datotečni nastavak postojeću IT infrastrukturu u oblak javno. Putem više lokacija mogućnosti povezivanja moguće je virtualne mreže Azure Smatraj samo drugi podmreže na vaše lokalne mrežne infrastrukture.

No postoji mnogo planiranje i dizajniranje problemi koje morate najprije adresirana. To je osobito važno u području mrežne sigurnosti. Jedna od najboljih načina da biste razumjeli kako postići Ovakav dizajn jest da biste vidjeli primjera.

Microsoft je stvorio [Podatkovnog centra proširenje referenca arhitektura dijagrama](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) i popratnih materijala za podršku pomoću kojih se objašnjava kako takve nastavkom podatkovnog centra izgledati. To omogućuje implementacija primjer referencu koju koristite za planiranje i dizajniranje nastavkom sigurne enterprise podatkovnog centra u oblak. Preporučujemo da pregledate dokument da biste dobili ideju komponenti za ključne sigurne rješenja.

Da biste saznali više o tome kako sigurno vaše podatkovnog centra širenje Azure, prikaz videozapisa [Proširivanje vaše podatkovnog centra za Microsoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).
