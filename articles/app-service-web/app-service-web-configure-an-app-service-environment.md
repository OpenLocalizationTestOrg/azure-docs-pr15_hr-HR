<properties
    pageTitle="Upute za konfiguriranje okruženja aplikacije servisa za | Microsoft Azure"
    description="Konfiguriranje, upravljanje i nadzor od aplikacije servisa okruženja"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>


# <a name="configuring-an-app-service-environment"></a>Konfiguriranje okruženja aplikacije servisa #

## <a name="overview"></a>Pregled ##

Visoke razine okruženja aplikacije servisa Azure sastoji se od nekoliko važnih komponenti:

- Resursi koji su pokrenuti na servisu aplikacije servisa okruženje hostirane za izračun
- Prostor za pohranu
- Baze podataka
- Classic(V1) ili resursa Manager(V2) Azure virtualne mreže (VNet) 
- Podmreže sa servisom aplikacije servisa okruženje hostira radi u njoj

### <a name="compute-resources"></a>Resursi za izračun

Koristite računalnim resursima za vaše grupe četiri resursa.  Svaku aplikaciju servisa okruženje (elika i mala slova) sadrži skup sučeljima i tri moguće tempiranja grupe. Ne morate koristiti sve tri tempiranja grupe – ako želite, možete jednostavno koristiti jednu ili dvije.

Glavno računalo u grupe resursa (sučeljima i zaposlenici zaduženi za) nisu izravno pristupiti drugih korisnika. Ne možete pomoću protokola udaljene radne površine (RDP) povežete s njima, promijenite njihove dodjele resursa ili poslužiti kao administrator na njima.

Možete postaviti količina skup resursa i veličina. U programa elika i mala slova, imate četiri mogućnosti veličine koji su označeni P1 kroz P4. Detalje o te veličine i njihovih cijene, potražite u članku [aplikacije servisa za cijene](../app-service/app-service-value-prop-what-is.md).
Promijenite količinu ili veličine naziva skaliranje operacija.  Samo jedan skaliranje operacije može biti u tijeku odjednom.

**Ispred završava**: U sučeljima su krajnje točke HTTP/HTTPS za aplikacije sustava drže u vašem elika i mala slova. Radnih opterećenja neće se izvoditi u u sučeljima.

- Na elika i mala slova počinje s dva P2s koji je dovoljno za razvojni i testiranje radnih opterećenja i radnih opterećenja najniže razine radnog. Preporučujemo da P3s za moderiranje za velike radnog radnih opterećenja.
- Ispisa da biste radnih opterećenja podebljanom radnog preporučujemo da imate barem četiri P3s da biste bili sigurni da su dovoljno sučeljima izvode kada se pojavi održavanje. Održavanje aktivnosti će se srušiti prema dolje za jedan sučelje odjednom. Time se ukupni smanjuje dostupan pristupne kapacitet tijekom održavanja aktivnosti.
- Trenutačno ne možete dodati nove instance sučelja. Može potrajati između 2 do 3 sata za dodjelu resursa.
- Za daljnje prilagodbom mjerilo, trebali biste praćenje procesora postotak, postotak memorije i metrike aktivnih zahtjeva za sučelja skup. Ako su postotaka procesora i memorije iznad 70% kada se pokrene P3s, dodajte dodatne sučeljima. Ako je vrijednost aktivnih zahtjeva prosjeka 15.000 za 20 000 zahtjevi po sučelje, trebali biste dodati i dodatne sučeljima. Cjelokupan cilj je da biste zadržali postotaka opterećenje procesora i memorije ispod 70% i Aktivni zahtjevi za izračunavanje prosjeka da biste ispod 15,000 zahtjevi po prednje završetka kada koristite P3s.  

**Zaposlenici zaduženi za**: Zaposlenici su gdje se aplikacija zapravo pokrenuti. Kada proširenja aplikacije servisa planove, koji se koristi gore zaposlenici zaduženi za u povezane tempiranja.

- Trenutačno ne možete dodati zaposlenici zaduženi za. Može potrajati od 2 do 3 sati za dodjelu resursa, bez obzira na to koliko dodaju.
- Skaliranje veličina računalnim resurs za sve grupe aplikacija se 2 do 3 sata po ažuriranje domene. Postoje 20 ažuriranje domene u programa elika i mala slova. Ako je promijenjena veličina veličine računalnim skup tempiranja s 10 instancama, može proći između 20 do 30 sati.
- Ako promijenite veličinu računalnim resursa koji se koriste u grupe aplikacija tempiranja, uzrokovat će Hladna pokretanja aplikacije koji se koriste u tom radnih grupa aplikacija.

Najbrži način da biste promijenili veličinu resursa računalnim radnih grupa aplikacija sa sustavom aplikacija će:

- Promjena veličine prema dolje na instancu broj 0. Trebat će otprilike 30 minuta da biste deallocate vaše instance.
- Odaberite novu veličinu računalnim i broj instanci. Na tom mjestu, trebat će između 2 do 3 sata da biste dovršili.

Ako aplikacija potreban veći računalnim resursa, nije moguće iskoristite prednost prethodne upute. Umjesto promjene veličine radnih grupe aplikacija na kojem se nalazi tim aplikacijama, možete popuniti drugi skup tempiranja s zaposlenici zaduženi za željene veličine i premjestiti aplikacija za tu grupu.

- Stvorite dodatne instance veličinu potrebne računalnim u drugom tempiranja. To će potrajati od 2 do 3 sati da biste dovršili.
- Ponovna dodjela planove aplikacije servisa koji su domaćini aplikacijama koje je potrebna veća resurse upravo konfigurirani radnih. Ovo je brzi postupak koji morate poduzeti manje od nekoliko minuta da biste dovršili.  
- Promjena veličine pritisnutu prvi skup tempiranja ako više nisu potrebne Neiskorišteni slučajevima. Ovaj postupak traje 30 minuta da biste dovršili.

**Autoscaling**: jedna od alatima koji olakšavaju upravljanje vaše potrošnje resursa računalnim je autoscaling. Možete koristiti autoscaling za sučelja ili grupe tempiranja. Možete radnje kao što su povećava vaše pojavljivanja bilo koju vrstu grupe aplikacija u Jutro i smanjivanje u na večer. Ili možda možete dodati instanci kada broj zaposlenici zaduženi za koje su dostupne u tempiranja skup ispod nekog praga.

Ako želite postaviti pravila za automatsko skaliranje oko metriku skup resursa za računalnim, a zatim Imajte na umu vremena koje je potrebno dodjele resursa. Dodatne informacije o autoscaling okruženja aplikacije servisa potražite u članku [kako konfigurirati Automatsko skaliranje u okruženju aplikacije servisa][ASEAutoscale].

### <a name="storage"></a>Prostor za pohranu

Svaki elika i mala slova konfiguriran 500 GB prostora za pohranu. Taj prostor koristi se u svim aplikacijama na elika i mala slova. Prostor za pohranu je dio na elika i mala slova i trenutno nije moguće prebacivati se za korištenje prostora za pohranu. Ako izrađujete prilagodbe usmjeravanje virtualne mreže ili sigurnost, morate i dalje dopustiti pristup pohrani Azure – ili ne može funkcionirati na elika i mala slova.

### <a name="database"></a>Baze podataka

Baza podataka sadrži podatke koji definira okruženja, kao i detalje o aplikacijama koje koristite u njoj. To predugo je dio pretplate Azure održava. Nije nešto što ste Izravni mogućnost za rukovanje. Ako izrađujete prilagodbe usmjeravanje virtualne mreže ili sigurnost, morate još uvijek Dopusti pristup SQL Azure – ili ne može funkcionirati na elika i mala slova.

### <a name="network"></a>Mreža

VNet koje ste primijenili na elika i mala slova može biti nešto koje ste napravili kad ste stvorili u elika i mala slova ili onaj koji ste napravili na vrijeme. Kada stvorite podmreži tijekom stvaranja elika i mala slova, nameće elika i mala slova u istoj grupi resursa kao virtualne mreže. Ako vam je potrebna grupa resursa koji se koristi da bi se razlikovati od onih na VNet vaše elika i mala slova, morate stvoriti vaše elika i mala slova pomoću predloška za upravitelj resursa.

Postoje neka ograničenja virtualne mreže koji se koristi za programa elika i mala slova:
 
- Virtualne mreže mora biti regionalne virtualne mreže.
- Mora postojati podmreže s 8 ili više adresa gdje je implementiran u elika i mala slova.
- Nakon podmreži koristi za hostiranje programa elika i mala slova, adresa raspon podmreži nije moguće promijeniti. Zbog toga preporučujemo da podmreži sadrži najmanje 64 adrese za sve budućeg rasta elika i mala slova.
- Mogu postojati i ništa drugo u podmreži, ali u elika i mala slova.

Za razliku od glavnom računalu servis koji sadrži na elika i mala slova, [virtualne mreže] [ virtualnetwork] te u odjeljku kontrola korisničkih podmreže.  Možete upravljati virtualne mreže putem virtualne korisničkog Sučelja mreže ili PowerShell.  U klasičnom ili VNet upravitelj resursa može uvesti programa elika i mala slova.  Portal i API sučelja su nešto drugačija između klasični i resursima VNets, ali imat ćete osjećaj elika i mala slova.

VNet koji se koristi za hostiranje elika programa i mala slova možete koristiti ili privatne RFC1918 IP adresa ili ga možete koristiti javnu IP adrese.  Ako želite koristiti raspona IP koji je prekriveni RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16), a zatim potrebnih za stvaranje VNet i podmreže koriste vaš elika i mala slova ispred stvaranja elika i mala slova.

Budući da tu mogućnost postavlja aplikacije servisa Azure u virtualne mreže, to znači da vaše aplikacije koje se nalaze u vašem elika i mala slova sada možete pristupiti resursi koji su bili dostupni putem ExpressRoute ili web-mjesto virtualne privatne mreže (VPN-ovi) izravno. Aplikacijama koje su u okruženju sustava aplikacije servisa nije potrebna dodatne značajke umrežavanje resurse koji su dostupni za virtualne mreže na kojem se nalazi vaše okruženje aplikacije servisa za pristup. To znači da ne morate koristiti VNET Integracija ili hibridnog veze da biste dobili resursima tijekom ili povezani s mrežom virtualne. Možete i dalje koristiti od tih značajki Premda je na resurse za pristup u mrežama koji su povezani s mrežom virtualne.

Ako, na primjer, možete koristiti VNET Integracija za integraciju s virtualne mreže koji se u vašoj pretplati, ali nije povezan s virtualne mrežu s kojom se vaš elika i mala slova. Također i dalje možete koristiti veze hibridnog pristupa resursima koji su na drugim mrežama, kao i obično.  

Ako imate virtualne mreže konfiguriran pomoću programa ExpressRoute VPN, paziti nekih potreba usmjeravanja koja ima programa elika i mala slova. Postoje neki korisnički definirane usmjeravanje (UDR) konfiguracije koje nisu kompatibilne s programa elika i mala slova. Dodatne informacije o pokretanju programa elika i mala slova u virtualne mreže s ExpressRoute, potražite u članku [pokretanje aplikacije servisa okruženje u virtualne mreže s ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Zaštita dolazni promet

Postoje dva načina primarni da biste odredili dolazni promet na vaše elika i mala slova.  Koristite Groups(NSGs) sigurnost mreže da biste kontrolirali koje IP adrese mogu pristupiti vašem elika i mala slova kao što je opisano u nastavku [kako kontrolirati dolaznog prometa u okruženju aplikacije servisa](app-service-app-service-environment-control-inbound-traffic.md) i vaše elika i mala slova možete konfigurirati i s internim Balancer(ILB) za učitavanje.  Te značajke možete koristit i zajedno ako želite ograničiti pristup pomoću NSGs za svoje ILB elika i mala slova.

Kada stvorite elika programa i mala slova, stvorit će na VIP u vašem VNet.  Postoje dvije vrste VIP, vanjskih i internih.  Kada stvorite programa elika i mala slova s vanjskim VIP aplikacije u vašem elika i mala slova bit će dostupni putem Interneta usmjeravati IP adresu. Kada odaberete internog vaše elika i mala slova će konfigurirati programa ILB i neće biti izravno pristupiti internet.  Na ILB elika i mala slova i dalje zahtijeva vanjski VIP, ali se koristi samo za Azure upravljanja i održavanje pristup.  

Tijekom stvaranja ILB elika i mala slova navedite poddomene koristi elika i ILB u mala slova i morat će upravljati vlastitim DNS za poddomena koju navedete.  Budući da se postaviti poddomene morate upravljati certifikat koji je korišten za pristup HTTPS.  Nakon stvaranja elika i mala slova morat ćete unijeti certifikata.  Da biste saznali više o stvaranju i korištenju programa elika i mala slova ILB pročitajte [pomoću internog raspoređivača opterećenja s okruženjem aplikacije servisa][ILBASE]. 


## <a name="portal"></a>Portal

Možete upravljati i praćenje okruženja aplikacije servisa na portalu za Azure pomoću korisničkog Sučelja. Ako imate programa elika i mala slova, pa ćete vjerojatno da biste vidjeli aplikacije servisa za simbol na bočnoj traci. Ovaj simbol koristi se za predstavljanje okruženja aplikacije servisa Azure portalu:

![Simbol okruženje servisa aplikacija][1]

Da biste otvorili korisničkog Sučelja koji navodi sve svoje aplikacije servisa okruženja, možete koristiti ikonu ili odaberite ševron (">" simbol) pri dnu na bočnoj traci odaberite aplikacije servisa okruženja. Odabirom jedne od ASEs naveden, otvorite korisničkog Sučelja koji se koristi za praćenje i upravljati njime.

![Korisničko Sučelje za nadzor i upravljanje njima okruženja aplikacije servisa][2]

U ovom prvom plohu prikazuje neka svojstva vaše elika i mala slova, zajedno s metričkim grafikona po resurse. Neka svojstva koje su prikazane u bloku **Essentials** su i hiperveze koje će se otvoriti gore plohu koji je povezan s njom. Ako, na primjer, možete odabrati naziv **Virtualne mreže** da biste otvorili gore korisničko Sučelje povezano s virtualne mreže koji radi na elika i mala slova u. **Aplikacije servisa za tarife** i **aplikacije** svaki otvorite gore blades kojima su stavke koje se nalaze u vašem elika i mala slova.  

### <a name="monitoring"></a>Nadzor

Grafikona omogućuju da biste vidjeli raznih metriku performanse u svaki skup resursa. Pristupne skup možete nadzirati average opterećenje procesora i memorije. Za tempiranja grupe, možete nadzirati na koji se koristi i količine koja je dostupna.

Korištenje aplikacije servisa za više tarife možete napraviti zaposlenici zaduženi u tempiranja skup. Povećavaju ne distribuira na isti način kao što je s sučelja, pa korištenje procesora i memorije ne unesete velik in the way of korisne informacije. Više važno da biste pratili koliko zaposlenici zaduženi za koje ste koristili i su dostupne – je osobito ako upravljate sustav za korištenje.  

Vam može poslužiti sve metriku koja se može pratiti u grafikonima da biste postavili upozorenja. Postavljanje upozorenja ovdje funkcionira na isti kao negdje drugdje na aplikacije servisa za. Možete postaviti upozorenja ili **upozorenja** korisničkog Sučelja dijelu ili iz Dubinska analiza sve metriku korisničkog Sučelja, a zatim odaberete **Dodaj upozorenje**.

![Metriku korisničkog Sučelja][3]

Metriku koja samo navedene su metriku okruženja aplikacije servisa. Postoje i metrike koji su dostupni na razini aplikacije servisa za planiranje. Ovo je gdje nadzor opterećenje procesora i memorije čini mnogo odgovara.

U programa elika i mala slova, sve aplikacije servisa za tarife su namjenski aplikacije servisa za tarife. To znači da koje samo aplikacije koje su pokrenute na domaćini dodijeliti aplikacije servisa za planiranje su aplikacije u tom aplikacije servisa za planiranje. Da biste vidjeli detalje o vašoj tarifi za aplikacije servisa, otvorili plan aplikacije servisa iz nekog od popisa u korisničkom Sučelju elika i mala slova ili iz **aplikacije servisa za pregled tarife** (koji navodi sve).   

### <a name="settings"></a>Postavke

Unutar plohu elika i mala slova postoji **Postavke** odjeljka koji sadrži nekoliko važnih mogućnosti:

**Postavke** > **Svojstva**: **Postavke** plohu automatski otvara kada pozovite vaše plohu elika i mala slova. Pri vrhu je **Svojstva**. Postoji nekoliko stavki ovdje koje su suvišna da biste vidjeli u **Essentials**, no što je vrlo koristan je **Virtualna IP adresa**, kao i **Izlaznih IP adresa**.

![Postavke plohu i svojstava][4]

**Postavke** > **IP adrese**: Stvorite aplikacije IP Secure Sockets Layer (SSL) u vašem elika i mala slova, morate IP SSL adresu. Da biste dobili, vaše elika i mala slova mora IP SSL adrese koje je vlasnik, a možete dodijeliti. Kada se elika i mala slova, sadrži jednu IP SSL adresu u tu svrhu, ali možete dodati više. Postoji naknadu za dodatne SSL IP adrese, kao što je prikazano u [Aplikacije servisa za cijene] [ AppServicePricing] (u odjeljku SSL veze). Dodatni cijena je cijena IP SSL.

**Postavke** > **Prednje resurse** / **Tempiranja grupe**: svaki od tih blades skup resursa nudi mogućnost da biste vidjeli podatke samo na taj skup resursa, osim pruža kontrole za potpuno promjenu veličine tog resursa.  

Osnovni plohu za svaki skup resursa nudi grafikon s metriku za taj skup resursa. Baš kao i s grafikonima iz plohu elika i mala slova možete i otvoriti u grafikon i postavite upozorenja po želji. Postavljanje upozorenja iz plohu elika i mala slova za određene resurse ne isto što činite s skup resursa. **Postavke** plohu skup radnih kojima imate pristup svim aplikacijama ili aplikacije servisa za tarife koji se koriste u ovu grupu tempiranja.

![Tempiranja skup postavki korisničkog Sučelja][5]

### <a name="portal-scale-capabilities"></a>Mogućnosti portala skala  

Postoje tri skaliranje postupci:

- Promjena broja IP adresa u elika i u mala slova koji su dostupni za korištenje IP SSL.
- Promjena veličine računalnim resursa koji se koriste u skup resursa.
- Promjena broja računalnim resursa koji se koriste u skup resursa ručno ili putem autoscaling.

Na portalu tri su načina da biste odredili koliko poslužitelje koje imate u svoje grupe resursa:

- Promjena veličine postupak iz glavnog plohu elika i mala slova pri vrhu. Možete unijeti promjene konfiguracije više mjerilo za grupe prednje i tempiranja. Sve primjene kao jedan operacija.
- Ručna promjena veličine postupak iz skup resursa za pojedinačne **Skaliranje** plohu koja je u odjeljku **Postavke**.
- Autoscaling, na koji postavljate iz plohu **mjerilo** za skup pojedinačnih resursa.

Da biste koristili postupka skaliranje na plohu elika i mala slova, povucite klizač za količinu i spremite. U ovom korisničkog Sučelja podržava i promjena veličine.  

![Promjena veličine korisničkog Sučelja][6]

Da biste koristili mogućnosti ručno i automatsko skaliranje određene resursa, idite na **Postavke** > **Prednje resurse** / **Tempiranja grupe** prema potrebi. Zatim otvorite gore skup koji želite promijeniti. Idite na **Postavke** > **mjerilo izgleda** ili **Postavke** > **proširenja**. Plohu **Vremensko mjerilo** omogućuje kontrolu količine instance. **Skaliranje gore** omogućuje kontrolirati veličinu resursa.  

![Postavke mjerila korisničkog Sučelja][7]

## <a name="fault-tolerance-considerations"></a>Razmatranja kvara odstupanje

Možete konfigurirati okruženje aplikacije servisa za do 55 ukupni računalnim resursi. Samo 50 tih 55 resursa računalnim može se koristiti radnih opterećenja glavnog računala. Razlog za to je dvostruk. Postoji najmanje 2 sučelja računalnim resursi.  Koje ostavlja do 53 za podršku tempiranja skup dodijeljeni. Da bi se omogućuje kvara odstupanje, morate koristiti resursa za dodatne računalnim dodijeljen prema sljedećim pravilima:

- Svaki skup tempiranja mora najmanje 1 dodatne računalnim resursa koji nije dostupan za radno opterećenje mora imati dodijeljenu.
- Kada količina računalnim resursa u tempiranja skup dolazi iznad određene vrijednosti, pa drugi resurs računalnim potreban je za odstupanje kvara. Ovo nije slova u sučelja.

Unutar bilo koje grupe aplikacija jedan tempiranja preduvjeti kvara odstupanje su koji danu vrijednost od X resurse dodijeljene tempiranja skup:

- Ako je X između 2 i 20, iznos upotrebljivosti računalnim resursa koje možete koristiti za radnih opterećenja je X-1.
- Ako je X između 21 i 40, iznos upotrebljivosti računalnim resursa koje možete koristiti za radnih opterećenja je X-2.
- Ako je X između 41 i 53, iznos upotrebljivosti računalnim resursa koje možete koristiti za radnih opterećenja je X-3.

Minimalna ostavlja manji trag pri ima 2 sučelja i zaposlenici zaduženi za 2.  Pomoću naredbe iznad pa Evo nekoliko primjera radi pojašnjavaju:  

- Ako imate zaposlenici zaduženi za 30 u jednu grupu, 28 ih možete koristiti za radnih opterećenja glavnog računala.
- Ako imate zaposlenici zaduženi za 2 u jednu grupu, 1 možete koristiti za radnih opterećenja glavnog računala.
- Ako imate zaposlenici zaduženi za 20 u jednu grupu, 19 možete koristiti za radnih opterećenja glavnog računala.  
- Ako zaposlenici zaduženi za 21 u jednu grupu, zatim još uvijek samo 19 mogu se radnih opterećenja glavnog računala.  

Važno je razmjer kvara odstupanje, ali potrebno je imati na umu prilikom skaliranje iznad određene pragovi. Ako želite dodati više kapacitet prelaska iz 20 instance, zatim prijeđite u odjeljak 22 ili noviji jer 21 ne dodaje sve dodatne kapaciteta. Isto vrijedi i početak iznad 40, pri čemu je sljedeći broj koji dodaje kapaciteta 42.  

## <a name="deleting-an-app-service-environment"></a>Brisanje aplikacije servisa za okruženja ##

Ako želite da biste izbrisali okruženju aplikacije servisa, zatim jednostavno akcijom **izbrisati** pri vrhu plohu okruženja aplikacije servisa. Kada to učinite, morat ćete unijeti naziv okruženja aplikacije servisa da biste potvrdili da zaista želite to učiniti. Imajte na umu da kada izbrišete okruženju aplikacije servisa, izbrisati sav sadržaj u njemu kao i.  

![Brisanje aplikacije servisa za okruženje korisničkog Sučelja][9]  

## <a name="getting-started"></a>Početak rada

Da biste započeli aplikacije servisa okruženja, potražite [u](app-service-web-how-to-create-an-app-service-environment.md)članku Stvaranje okruženja aplikacije servisa.

Dodatne informacije o platforme Azure aplikacije servisa potražite u članku [Aplikacije servisa za Azure](../app-service/app-service-value-prop-what-is.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
