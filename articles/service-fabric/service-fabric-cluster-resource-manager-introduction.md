<properties
   pageTitle="Uvod u Voditelj resursa za servis tkanina klaster | Microsoft Azure"
   description="Uvod u tkanina klaster resursa Upravitelj servisa."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Uvod u Voditelj resursa za servis tkanina klaster
Najčešći upravljanje IT sustava ili skup usluga namijenjena početak nekoliko fizičke ili virtualnim strojevima trakom za te određene servisima ili sustavima. Mnoge glavne servise su svesti sloj "web" i "data" ili "prostor za pohranu" sloju, možda s nekoliko druge posebne komponente kao što su predmemoriju. Druge vrste aplikacije promijenile razmjenu sloju gdje flowed zahtjeve i smanjivati povezan s rad sloju sve analizirali ili pak transformaciju potrebne kao dio sustava za razmjenu poruka. Svaku vrstu radno opterećenje imate određene strojeva namjenski na njega: baze podataka imate nekoliko strojeva namjenski, web-poslužiteljima na mali. Ako određenu vrstu radno opterećenje strojeva je na da biste pokrenuli previše tipkovni, a zatim dodati više računala s tom vrstom radno opterećenje konfiguriran za izvođenje na njemu ili zamijeniti nekoliko strojeva veće strojeva. Jednostavan. Ako računalo nije uspjela, taj dio ukupnog aplikacije pokrenuli na donjem kapaciteta dok se ne može se obnoviti na računalu. I dalje prilično jednostavan (Ako je ne moraju nužno biti zabavna).

Sada, međutim, recimo da pronađete treba Vremensko mjerilo, a stupila na spremnika i/ili microservice plunge. Iznenada pronađete sami tens, nekoliko stotina ili čak tisuće strojeva, desetke različite vrste servise, možda stotine različite instance od tih servisa, svaka s jednog ili više instanci ili replike za visoke dostupnosti (HA).

Iznenada upravljanje okruženju sustava nije tako jednostavno upravljanje namjenski jedne vrste radnih opterećenja strojevima sa sustavom. Vaši poslužitelji su virtualne i više ne morate imena (koju *ste* mindsets iz [kućni LJUBIMCI za cattle](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) pojavljivali nakon svih). Konfiguracija je manji o strojeva i dodatne informacije o servisima sami. Namjenski hardver je u prošlost i usluge prekorače small raspodijeljeno sustave, koje se protežu na više dijelova manje obavješćivanje hardvera.

Kao posljedica prekidanje prethodno monolithic, tiered aplikacije u zasebnom servise obavješćivanje hardver, sada imate mnogo više kombinacije baviti. Tko odlučuje što vrste radnih opterećenja mogu se izvoditi na koji hardver ili koliko? Koje radnih opterećenja funkcioniraju na isti hardver i koji su u sukobu? Kada stroj funkcionira... što je čak i radi li? Koja je zadužena pazeći te radno opterećenje počinje ponovno pokrenuti? Učinite Pričekajte da se vratite (virtualne?) stroj ili učinite vaše radnih opterećenja automatski se neće iznad drugih računala i Zadrži radi? Je li Ljudski intervencije potrebne? Što je o nadogradnji u ovom sortiranje okruženje?

Razvojni inženjeri i operatora living u ovom sortiranju svijetu, ne možemo ćete je potrebna pomoć upravljanje toga te dobiti osjećaj da zapošljavanjem binge i pokušate papira putem složenosti s osobama nije desnom odgovor.

Što učiniti?

## <a name="introducing-orchestrators"></a>Uvod u orchestrators
"Orchestrator" je opći pojam dijela softver koji olakšava administratori upravljati sljedećim vrstama okruženja. Orchestrators su komponente koje koriste u zahtjevima za kao "Aspekt 5 kopije taj servis koji se izvodi u Moje okruženje", provjerite true, a zatim (isprobajte) da biste ga zadržali na taj način.

Orchestrators (ne ljudi) su što swing u akciji kada ne uspije stroj ili radno opterećenje prekida nekog razloga neočekivane. Većina Orchestrators dodatne mogućnosti osim običnog baviti pogreške, kao što su pomoć s novi implementacije, rukovanje nadogradnji i povezanima s potrošnje resursa, ali su svi bitno o zaštiti neke želji stanje konfiguracije u okruženju. Želite li mogu vam reći programa Orchestrator što želite i ste ga učiniti lifting podebljano. Chronos Marathon pri vrhu Mesos, tim, Swarm, Kubernetes i usluge tkanina sve Primjeri Orchestrators (ili ih ugrađena). Više se stvaraju stalno kao complexities upravljanja stvarnog svijeta implementacije u različitim okruženjima i uvjeta rasti i promijeniti.

## <a name="orchestration-as-a-service"></a>Djelovanje kao usluga
Zadatak Orchestrator unutar servisa tkanina klaster obrađuje prvenstveno klaster resursa Upravitelj. Voditelj resursa za servis tkanina klaster jedan je od servisa sustava unutar servisa tkanina i automatski se pokreće u svakom klaster.  Općenito govoreći, klaster Voditelj resursa posla je svesti tri dijela:

1. Nametanje pravila
2. Optimiziranje vaše okruženje
3. Nije u drugih postupaka

### <a name="what-it-isnt"></a>Što još nije
U tradicionalni N sloju web-aplikacijama došlo je uvijek neke notion na "opterećenja", obično se nazivaju mreže raspoređivača opterećenja (NLB) ili u aplikaciju učitavanja raspoređivača (ALB) ovisno o tome gdje sat u stogu za umrežavanje. Neke balancers učitavanja su hardver temelji kao što su F5 na BigIP nuditi, drugi softver koji se temelje kao što je Microsoftov NLB. U drugi okruženjima u može se prikazati nešto poput HAProxy ta uloga. U te arhitekturi posao opterećenja je da biste bili sigurni da sve računala različite bez praćenja stanja sučelja ili različitim računalima u klasteru dobivati (otprilike) jednakih razmaka rad. Strategije za to različiti šalje svaki drugi poziv na drugi poslužitelj za sesiju prikvačivanje/stickiness, da biste stvarni procjeni i poziv dodijeljeni na temelju njegove očekivani trošak i trenutno opterećenje računala.

Imajte na umu da je uz najbolju mehanizam za osiguravanje koji web razina pojavili otprilike raspoređen. Strategije za ujednačavanje sloju podataka nisu potpuno različite i namijenjen na mehanizam za pohranu podataka, obično centriranje oko sharding podataka, predmemoriranja, baze podataka upravljani prikazi i pohranjene procedure, itd.

Dok su neki od ovih strategije zanimljivih, upravitelj resursa klaster tkanina servisa nije ništa kao što su mreže raspoređivača opterećenja ili predmemoriju. Dok je mreža raspoređivača opterećenja osigurava da se u sučeljima raspoređen premještanjem promet u kojem su servisi pokrenuti, Voditelj resursa za servis tkanina klaster vodi potpuno različite strategije – bitno, servis tkanina premješta *services* odabirati najviše odgovara (i očekuje promet ili slijedite opterećenje). To može biti, na primjer, čvorove koje su trenutno Hladna jer servise koji postoje su način mnogo rada odmah ili koje ste izbrisali ili premjestili negdje drugdje. Drugi primjer Voditelj resursa klaster moguće premjestiti i usluge uz računalo koje uskoro će se nadograditi ili koji je pretrpan zbog šiljka u potrošnje uz servise koji su pokrenuti na njemu. Budući da je Voditelj resursa klaster odgovoran za premještanje servisa oko (ne isporučuje mrežnog prometa u kojima se usluge već su), sadrži skup znatno značajku u usporedbi s koje želite pronaći u mreži raspoređivača opterećenja i uključuje bitno različiti Strategije za osiguravanje su resursima hardvera u klasteru i koristi.

## <a name="next-steps"></a>Daljnji koraci
- Informacije o tijeku arhitektura i informacije unutar Upravitelj klaster resursa, pogledajte [članak](service-fabric-cluster-resource-manager-architecture.md)
- Voditelj resursa klaster sadrži mnogo mogućnosti za s opisom klaster. Da biste saznali više o njima pogledajte ovaj članak na [s opisom servisa tkanina klaster](service-fabric-cluster-resource-manager-cluster-description.md)
- Za dodatne informacije o drugim mogućnostima dostupnima za konfiguriranje usluge odjavu temu na druge konfiguracije Voditelj resursa klaster dostupne [Dodatne informacije o konfiguriranju Services](service-fabric-cluster-resource-manager-configure-services.md)
- Metriku su način na koji upravitelj resursa za servis tkanina klaster upravlja potrošnje i kapacitet u klasteru. Da biste saznali više o njima i kako ih konfigurirali pogledajte [članak](service-fabric-cluster-resource-manager-metrics.md)
- Voditelj resursa klaster funkcionira s mogućnostima upravljanja tkanina servisa. Da biste saznali više o toj integracije, pročitajte [Ovaj članak](service-fabric-cluster-resource-manager-management-integration.md)
- Da biste saznali kako Voditelj resursa klaster upravlja te salda Učitaj u klasteru, pogledajte članak na [za ujednačavanje opterećenja](service-fabric-cluster-resource-manager-balancing.md)
