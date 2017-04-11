<properties
   pageTitle="Pregled servisa tkanina | Microsoft Azure"
   description="Pregled servisa tkanina, gdje aplikacije sastoje se od mnogo microservices mjerilo i resilience. Tkanina servis je raspodijeljeno sustavi platformu za sastavljanje prilagodljivi, pouzdanog i lako upravlja aplikacije za oblak"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="mfussell"/>

# <a name="overview-of-service-fabric"></a>Pregled servisa tkanina
Servis tkanina je raspodijeljeno sustavi koji olakšava pakiranje, uvođenje i upravljanje skalabilni i pouzdane microservices. Servis tkanina adrese i značajan izazove u razvoju i upravljanje aplikacijama oblaka. Razvojni inženjeri i administratori možete izbjeći rješavanja složene infrastrukture probleme i fokus umjesto na implementacijom zaštita njihove privatnosti ovise ključnih, zahtjevne radnih opterećenja zna da su skalabilni, pouzdanog i moguće upravljati. Servis tkanina predstavlja platformu dalje generacije proizvod za stvaranje i upravljanje njima te enterprise predmete, sloju 1 oblaka skaliranje aplikacije.

Ovaj [kratki videozapis](https://aka.ms/servicefabricvideo) nudi Uvod u usluge tkanina i microservices.


## <a name="applications-composed-of-microservices"></a>Programi koji se sastoji od microservices
Servis tkanina omogućuje vam omogućuje stvaranje i upravljanje aplikacijama skalabilni i pouzdane sastoji od microservices sustavom zajednički skup strojeva (naziva se klaster) u gustoće vrlo visoka. Sadrži sofisticirane runtime za izgradnju raspodijeljeno, skalabilni bez praćenja stanja i s praćenjem stanja microservices. U njoj se nalaze mogućnosti upravljanja potpun aplikacije za dodjelu resursa, implementaciji, nadzor, Nadogradnja/zakrpa i brisanje implementiran aplikacije.

Zašto je microservices pristup važan? Dva glavna razloga zbog kojih su:

1. Oni omogućuju skaliranje različite dijelove aplikacija ovisno o njegov potrebama.

2. Razvojnim timovima mogu biti više agilno u vodoravnim out promjene i stoga omogućuju značajke klijentima brže i češće.

Servis tkanina powers mnoge Microsoftove servise danas, uključujući baze podataka SQL Azure, Azure DocumentDB, Cortana, Power BI, Microsoft Intune, Azure događaj koncentratora, Azure IoT, Skype za tvrtke i mnoge Jezgra servisa Azure.

Za stvaranje oblaka nativni servise koje možete započeti mali, po potrebi i Povećaj pretraživanje velikog Skaliranje s stotine ili tisuće strojeva konkretne tkanina servisa.

Servisa Internet skaliranje današnjeg ugrađenih od microservices. Microservices Primjeri protokol pristupnika, korisnički profili košarice za kupnju, zaliha obrade, redove, i predmemorira. Servis tkanina je microservices koji daje svaki microservice jedinstveni naziv koji se mogu bez praćenja stanja ili s praćenjem stanja.

Servis tkanina nudi potpun izvođenja i životni ciklus mogućnosti upravljanja aplikacijama koji se sastoji od ovih microservices. Ga hostira microservices unutar spremnika implementiran i aktivirati putem servisa tkanina klaster. Premještanje s VMs spremnika čini moguće povećava redoslijed veličina u gustoće. Isto tako, drugog stupnja slobode za redoslijed gustoće postaje moguće pomicanjem iz spremnika microservices. Ako, na primjer, jedan klaster baze podataka SQL Azure sastoji se od stotine računala izvode tens tisuće spremnika hosting Ukupno stotine tisuće baze podataka. Svaki baza podataka je servis tkanina microservice za s praćenjem stanja. Isto vrijedi i drugih servisa prethodno što je rečeno, koji je Zašto termina "hyperscale" se opisuju mogućnosti tkanina servisa. Ako spremnika daju visoke gustoće, zatim microservices vam hyperscale.

Dodatne informacije o microservices pristup, pročitajte [Zašto microservices pristup u stvaranje aplikacija?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Spremnik implementacije i djelovanje
Servis tkanina je [orchestrator](service-fabric-cluster-resource-manager-introduction.md) od microservices preko klaster strojeva. Microservices mogu se razviti na mnogo različitih načina korištenja [servisa tkanina programiranje modelima](service-fabric-choose-framework.md) implementacije [izvršne datoteke za goste](service-fabric-deploy-existing-app.md). Servis tkanina možete implementirati servisi u spremniku slike i važnije možete miješati services u postupaka i servisa sustava spremnika zajedno u aplikaciji. Ako samo želite [Upravljanje spremnik slike](service-fabric-containers-overview.md) i preko klaster strojeva, tkanina servis je savršen izbor za to.


## <a name="create-service-fabric-clusters-anywhere"></a>Stvaranje servisa tkanina klastere bilo kojeg mjesta
Servis tkanina klastere možete stvoriti u mnogo okruženja, uključujući Azure ili lokalno, Windows Server ili Linux. Osim toga, razvojno okruženje u SDK jednak okruženju proizvodnje s bez Emulatora vezanih. Drugim riječima, ako se izvodi na svoj klaster lokalne razvoj ga uvodi isti klaster u drugim okruženja.

Dodatne informacije o stvaranju klastere na lokaciji, pročitajte [Stvaranje klaster na Windows Server ili Linux](service-fabric-deploy-anywhere.md) ili za Azure stvara na klaster [putem portala za Azure](service-fabric-cluster-creation-via-portal.md).

![Servis tkanina platforme][Image1]

## <a name="stateless-and-stateful-service-fabric-microservices"></a>Bez praćenja stanja i s praćenjem stanja servisa tkanina microservices

Servis tkanina vam omogućuje sastavljanje aplikacije koji se sastoji od microservices. Bez praćenja stanja microservices (protokol pristupnika proxy poslužitelji web, itd.) imaju mutable stanje izvan sve navedene zahtjev i njegov odgovor servisa. Azure uloge suradnika za servise u Oblaku su primjera bez praćenja stanja servisa. S praćenjem stanja microservices (korisničke račune, baze podataka, uređaji košarice za kupnju, redove, itd.) imaju mutable mjerodavne stanje izvan zahtjeva i njegov odgovor. Današnji Internet skaliranje aplikacije se sastoje od kombinacije microservices bez praćenja stanja i s praćenjem stanja.

Zašto se s praćenjem stanja microservices uz bez praćenja stanja one? Dva glavna razloga zbog kojih su:

1. Mogućnost da biste sastavili visokom propusnošću najniža Latencija, pogreška pogreške mrežna transakcija obrade services (OLTP) držanjem kod i zatvorite podatke na tom računalu. Primjere su interaktivni storefronts, pretraživanja, sustavi Internet stvari (IoT), poslovnih sustavima, kreditne kartice obrada prijevare otkrivanje sustavi i i upravljanje osobnim zapisima.

2. Simplification dizajn aplikacije. S praćenjem stanja microservices ukloniti potrebe za dodatne redove i predmemorira, Najčešći morati adresa preduvjeti dostupnosti i Latencija isključivo bez praćenja stanja aplikacije. S praćenjem stanja servisa su prirodan visoku dostupnost i niska-kašnjenje, smanjite broj premještanje dijelova za upravljanje u aplikaciji kao cjelinu.

Dodatne informacije o aplikaciji uzoraka tkaninom servis, pročitajte [scenariji aplikacije](service-fabric-application-scenarios.md) , a [zatim odaberete programiranje framework modela](service-fabric-choose-framework.md) servisa za

## <a name="application-lifecycle-management"></a>Upravljanje aplikacijama životni ciklus
Servis tkanina omogućuje jednostavno prva liga podršku za punu aplikaciju životni ciklus upravljanje (ALM) oblaka aplikacijama – od razvoja do implementaciju, dnevno upravljanje i održavanje usmjerenog deaktivirati.

Mogućnosti usluge tkanina ALM omogućiti administratori aplikacije / IT operatori za korištenje jednostavne, najniža dodirom tijekovi rada za dodjelu resursa, uvođenje zakrpa, i nadzor aplikacija. Ove ugrađeni tijekovi rada znatno smanjiti teret na operatori IT da biste zadržali aplikacije neprestano dostupna.

Većina aplikacija se sastoje od kombinacije microservices bez praćenja stanja i s praćenjem stanja i ostale izvršne datoteke/runtimes implementiran zajedno. Postavljanjem istaknuti vrste aplikacija i zapakirani microservices tkanina servisa omogućuje implementacije više instanci aplikacije. Svaku instancu upravlja i nadograditi zasebno. Važno je napomenuti da, tkanina servis je moći uvesti *sve* izvršne datoteke ili runtime i učinite ih pouzdanog. Ako, na primjer, servis tkanina uvodi ASP.NET Core 1, Node.js, jezika Java VMs, skripte ili išta drugo koji čine aplikacije.

Dodatne informacije o Upravljanje aplikacijama životni ciklus pročitajte [životni ciklus aplikacije](service-fabric-application-lifecycle.md) , a na implementacija kod potražite [uvođenja izvršna gost](service-fabric-deploy-existing-app.md)

## <a name="key-capabilities"></a>Ključne mogućnosti
Pomoću servisa tkanina možete učiniti sljedeće:

- Razvoj massively skalabilni aplikacije koje se same healing.

- Razvijajte aplikacije koji se sastoji od microservices pomoću model programiranja tkanina servisa. Ili, jednostavno izvršne datoteke goste glavnog računala i drugih aplikacija okviri po izboru, kao što su ASP.NET osnovne 1 ili Node.js.

- Razvoj iznimno pouzdanog microservices bez praćenja stanja i s praćenjem stanja.

- Uvođenje i orkestrirali spremnika uključiti Windows spremnika i Docker spremnika preko klaster. Ove spremnika možete izvršne datoteke spremnik goste ili pouzdanog microservices bez praćenja stanja i s praćenjem stanja.  U svakom slučaju dobit spremnik priključak preslikavanje priključka glavno računalo, spremnik vidljivost i automatsko prebacivanje.

- Pojednostavnite dizajn aplikacije pomoću s praćenjem stanja microservices umjesto predmemorije "i" Redovi.

- Implementacija Azure ili oblaka za lokalne izvodi Windows Server ili Linux nula promjenama kod. Pisanje jednom i implementacija bilo gdje bilo koji servis tkanina klaster.

- Razvijajte s "podatkovnim centrom na ovom računalu" pristup. Lokalni platforme je isti kod koji se izvodi u podatkovnim centrima za Azure.

- Implementacija aplikacije u sekundama.

- Implementacija aplikacije, na veći gustoće od virtualnim strojevima implementacija stotine ili tisuće aplikacije svako računalo.

- Implementacija različitih verzija iste aplikacije usporedno, svakog neovisno koji se mogu nadograditi.

- Upravljanje životni ciklus s praćenjem stanja aplikacija bez sve nedostupnost, uključujući prekidanje i nerastavljajuće nadogradnji.

- Upravljanje aplikacijama pomoću .NET API-ji, Java (Linux), PowerShell, Azure EŽA (Linux) ili OSTALE sučelja.

- Nadogradite i neovisno zakrpu microservices aplikacija.

- Praćenje i dijagnosticiranje stanja aplikacija i postaviti pravila za izvođenje automatsko popravke.

- Mogućnost skaliranje iz broj čvorove klaster, kao i skaliranje gore u skale ili skaliranje prema dolje veličinu svakog čvor zna da aplikacija automatski skaliranja i distribucija u skladu dostupnih resursa.

- Pogledajte samostalno healing opterećenja resursa orkestrirali ponovne distribucije aplikacija preko klaster. Servis tkanina oporavlja od pogrešaka i optimizira raspodjele učitavanja ovisno o dostupnim resursima.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci

* Dodatne informacije:
    * [Zašto je microservices postići u stvaranje aplikacija?](service-fabric-overview-microservices.md)
    * [Pregled terminologija](service-fabric-technical-overview.md)
* Postavljanje servisa tkanina [razvojno okruženje](service-fabric-get-started.md)  
* [Odabir programiranje framework modela](service-fabric-choose-framework.md) servisa

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
