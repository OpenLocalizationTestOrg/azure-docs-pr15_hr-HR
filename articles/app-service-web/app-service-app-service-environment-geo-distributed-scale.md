<properties 
    pageTitle="Skaliranje s okruženja aplikacije servisa za Distributed zemlj." 
    description="Saznajte kako vodoravno skaliranje aplikacije pomoću upravitelja promet i okruženja aplikacije servisa zemlj.-distribuciju." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="stefsch"/>   

# <a name="geo-distributed-scale-with-app-service-environments"></a>Skaliranje s okruženja aplikacije servisa za Distributed zemlj.

## <a name="overview"></a>Pregled ##
Scenariji aplikacije koje zahtijevaju vrlo visoka skaliranje može biti veći od kapacitet resursa za računalnim na jednom implementacije aplikacije.  Aplikacija za glasovanje Sportska događaja i događaje televised zabave sve Primjeri su scenarija koji zahtijevaju iznimno visoke mjerilo. Vodoravno skaliranje izvan aplikacije, s više implementacijama aplikacije u tijeku pokušaj unutar jedno područje, kao i preko područja, rukovati zahtjevima ekstremne učitavanja može se podudarati visoke skaliranje preduvjeti.

Aplikacije servisa za okruženja su idealna platformu za vodoravno skaliranje odgovor.  Jednom aplikacije servisa okruženju odabrao konfiguracije koji podržava učestalost poznati zahtjeva, razvojni inženjeri možete implementirati dodatne okruženja servisa aplikacija "kolačića cutter" način stanje kapaciteta željeni Vršna učitavanja.

Na primjer ako aplikaciju sustavom programa konfiguracije okruženja aplikacije servisa testirano rukovati 20K zahtjeva sekundi (RPS).  Ako željeni Vršna učitavanja 100K RPS, zatim okruženja aplikacije servisa za pet (5) možete biti stvorili i konfigurirali da biste bili sigurni aplikaciju možete rukovati Maksimalna predviđene Učitaj.

Budući da kupci obično pristupiti pomoću prilagođene (ili vanity) domene, razvojni inženjeri potreban pristup raspodijelite sve instance aplikacije servisa okruženje zahtjeva za aplikaciju.  Odličan način za to je da biste riješili prilagođene domene pomoću [profila Upravitelja promet Azure][AzureTrafficManagerProfile].  Upravitelj promet profila mogu se konfigurirati za pokažite na sve pojedinačne okruženja servisa aplikacija.  Upravitelj promet automatski rukovati distribucija kupci preko svih okruženja servisa aplikacija na temelju postavke profila Upravitelja promet za ujednačavanje opterećenja.  Taj se način funkcionira bez obzira na to jesu li sve usluge okruženja aplikacije implementiran u jednom području Azure ili implementiran na cijelom svijetu preko više Azure regija.

Osim toga, budući da korisnici pristupati aplikacije kroz na prilagođenu domenu, klijenti su svjesni broja aplikacije servisa okruženja u kojima se izvodi aplikacije.  Kao rezultat razvojnim inženjerima možete brzo i jednostavno dodavanje i uklanjanje aplikacija servisa okruženja učitavanja opaženih promet na temelju.

Konceptualni dijagramu u nastavku prikazuje vodoravno skalirana izgleda u tri okruženjima aplikacije servisa unutar jedno područje aplikacije.

![Konceptualni arhitekture][ConceptualArchitecture] 

Ostatak u ovoj se temi vodi kroz koraka pri postavljanju raspodijeljeno Topologija aplikacije uzorka pomoću više okruženja aplikacije servisa.

## <a name="planning-the-topology"></a>Planiranje topologije ##
Prije sastavnih odgovor na ostavlja manji trag pri raspodijeljeno aplikacije, korisno je nekoliko dijelova informacije na vrijeme.

- **Prilagođene domene za aplikaciju:**  Što je naziv prilagođene domene koje će korisnici koristiti za pristup aplikaciju?  Ogledna aplikacije naziv prilagođene domene je *www.scalableasedemo.com*
- **Promet Upravitelj domena:**  Naziv domene nije potrebno odabrati pri stvaranju [profila Upravitelja promet Azure][AzureTrafficManagerProfile].  Ovaj naziv će se kombinirati s kraticom *trafficmanager.net* da biste registrirali domenu unos kojim se upravlja upravitelj promet.  Za aplikaciju uzorka naziv odabrali je *skalabilni-elika i mala slova-pokazni videozapis*.  Kao rezultat punim nazivom domene kojim se upravlja upravitelj promet je *skalabilni-elika i mala slova-demo.trafficmanager.net*.
- **Strategija skaliranje ostavlja manji trag pri za aplikaciju:**  Će ostavlja manji trag pri za aplikaciju moguće raspodijeliti više okruženja aplikacije servisa u jednom području?  Više područja?  Mix – i – podudaranje oba pristupa?  Odluku da se temelji na očekivanja gdje će potiče promet za klijenta, kao i koliko će se dobro mogu mijenjati veličinu ostale aplikacije programa potpore pozadinske infrastrukture.  Na primjer, s aplikacijom bez praćenja stanja 100%, aplikaciju možete biti massively skalirana kombinacijom više okruženja aplikacije servisa po Azure regijama pomnoži aplikacije servisa okruženja implementiran preko više Azure regija.  U 15 + javno Azure regija dostupne na raspolaganju kupci doista možete izraditi na ostavlja manji trag pri world wide hyper skaliranje aplikacije.  Za aplikaciju uzorka za ovaj članak tri okruženja aplikacije servisa stvorene u jedno područje Azure (Južnoafrička središnje NAM).
- **Konvencija imenovanja za servis okruženja aplikacije:**  Svako okruženje servisa aplikacije potreban je jedinstveni naziv.  Osim jednom ili dvjema okruženja servisa aplikacija je korisno imati imenovanja da vam pomogne identificirati svaki okruženja aplikacije servisa.  Za aplikaciju uzorka koristila jednostavne konvencija imenovanja.  Nazivi ta tri okruženja aplikacije servisa su *fe1ase*, *fe2ase*i *fe3ase*.
- **Konvencija imenovanja za aplikacije:**  Budući da će biti implementirano više instanci aplikacije, naziv je potrebno za svaku instancu distribuiranih aplikacije.  Jedna malo poznati, ali vrlo praktično značajka okruženja aplikacije servisa je da se isti naziv aplikacije može koristiti u više okruženjima aplikacije servisa.  Budući da svaki okruženja aplikacije servisa ima nastavak jedinstveni domene, razvojni inženjeri možete odabrati da biste ponovno koristili točno isti naziv aplikacije u svakom okruženju.  Na primjer razvojni inženjer može imati aplikacije pod nazivom na sljedeći način: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*itd.  Za aplikaciju uzorka kroz svaku instancu aplikacije ima jedinstveni naziv.  Nazive instanci aplikacije koji se koriste su *webfrontend1*, *webfrontend2*i *webfrontend3*.


## <a name="setting-up-the-traffic-manager-profile"></a>Postavljanje promet upravitelj profila ##
Kada više instanci aplikacije primjenjuju se na više okruženja aplikacije servisa, mogu instance pojedinačne aplikacije registrirani pomoću upravitelja promet.  Za aplikaciju uzorka promet upravitelj profila je potrebno za *skalabilni-elika i mala slova-demo.trafficmanager.net* koji mogu usmjeravati kupce u neku od sljedećih instance distribuiranih aplikacije:

- **webfrontend1.fe1ase.p.azurewebsites.net:**  Instance komponente aplikaciju uzorka implementiran na prvi servis okruženje za aplikacije.
- **webfrontend2.fe2ase.p.azurewebsites.net:**  Instance komponente aplikaciju uzorka implementiran u drugom okruženju servisa aplikacija.
- **webfrontend3.fe3ase.p.azurewebsites.net:**  Instance komponente aplikaciju uzorka implementiran na treći servisa okruženje za aplikacije.

Da biste registrirali više aplikacije servisa za Azure krajnje točke, sve se izvodi u na **isti** Azure regija najjednostavnije s Powershell [Azure resursa Upravitelj promet Upravitelj podržava][ARMTrafficManager].  

U prvi je korak da biste stvorili profil programa Upravitelj promet Azure.  Kod u nastavku prikazuje kako se profil stvoren uzorka aplikacije:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Obratite pozornost na to kako *RelativeDnsName* parametar postavljen je na *skalabilni-elika i mala slova-pokazni videozapis*.  To su kako stvoriti i povezanu s profilom promet Upravitelj naziva domene *skalabilni-elika i mala slova-demo.trafficmanager.net* .

Parametar *TrafficRoutingMethod* definira pravila promet Upravitelj će koristiti da biste saznali kako možete proširiti kupca učitavanja preko svih dostupnih krajnje točke za ujednačavanje opterećenja.  U ovom primjeru *Weighted* način je odabrali.  To će rezultirati zahtjeva za korisničku koje se šire preko svih krajnje točke registriranu aplikaciju na temelju relativni debljine pridruženo svaki krajnjoj točki. 

S profilom stvorili, svaku instancu aplikacije dodaje se u profil kao nativni Azure krajnjoj točki.  Kod ispod dohvaćanja reference na svakom sučelje web-aplikacije, a zatim dodaje svaku aplikaciju kao krajnja točka za Upravitelj promet putem parametar *TargetResourceId* .


    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10
    
    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile
    
Obratite pozornost na to kako postoji jedan poziv za *Dodavanje AzureTrafficManagerEndpointConfig* za svaku instancu pojedinačne aplikacije.  Parametar *TargetResourceId* u svaku naredbu Powershell odnosi se na jedan od tri instance distribuiranih aplikacije.  Upravitelj promet profila će šire učitavanja preko svih tri krajnje točke registrira u profilu.

Sve tri krajnje točke za parametar *Debljina* koristite iste vrijednosti (10).  Rezultat u upravitelju promet šire zahtjeva za korisničku preko sve instance tri aplikacije relativno ravnomjerno. 


## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Pokazuje aplikacije prilagođene domene na promet Upravitelj domena ##
Završnom koraku potrebno je pokažite prilagođenu domenu aplikaciju na promet Upravitelj domena.  Za aplikaciju uzorka to znači da pokažete *www.scalableasedemo.com* na *skalabilni-elika i mala slova-demo.trafficmanager.net*.  Ovaj korak nije potrebno dovršiti kod registrara domena koja upravlja prilagođenu domenu.  

Pomoću alata za upravljanje vaš registrar domene CNAME zapisa potrebama će biti stvoren koji pokazuje prilagođene domene na promet Upravitelj domena.  Na slici u nastavku prikazuje primjer izgleda tu konfiguraciju CNAME:

![CNAME za prilagođenu domenu][CNAMEforCustomDomain] 

Iako ne obuhvaća u ovoj temi, imajte na umu da svaku instancu pojedinačne aplikacije nije potrebno odobravati prilagođenu domenu kao i registriran s njim.  U suprotnom ako zahtjev olakšava instancu aplikacija, a aplikacija nemate prilagođenu domenu registriran u aplikaciju, zahtjev neće uspjeti.  

U ovom primjeru prilagođenu domenu je *www.scalableasedemo.com*i svaku instancu aplikacije ima prilagođenu domenu pridružen.

![Prilagođene domene][CustomDomain] 

Recap od registriranja prilagođenu domenu s aplikacijama aplikacije servisa za Azure, potražite u sljedećem članku za [Registriranje prilagođene domene][RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>Isprobavate topologije Distributed ##
Rezultat konfiguracije Upravitelj promet-a i DNS- a je da će zahtjeva za *www.scalableasedemo.com* tijeka kroz sljedeće slijed:

1. Preglednik ili uređaj će postati DNS pretraživanja za *www.scalableasedemo.com*
2. Unos CNAME kod registrara domena uzrokuje traženje DNS petog Azure promet upravitelju.
3. Pretraživanje DNS postala je za *skalabilni-elika i mala slova-demo.trafficmanager.net* u odnosu na jedan od Azure promet Upravitelj DNS poslužitelji.
4. Na temelju pravila ( *TrafficRoutingMethod* parametar ranije koristiti pri stvaranju profila Upravitelja promet) za ujednačavanje opterećenja promet Upravitelj će odaberite jednu od konfigurirani krajnje točke i vratite FQDN te krajnjoj točki preglednika ili uređaj.
5.  Budući da FQDN krajnju točku je Url instance aplikacije sustavom okruženju aplikacije servisa, pregledniku ili uređaj će vas pitati Microsoft Azure DNS poslužitelju da se raščlanjuje FQDN IP adresa. 
6. Preglednik ili uređaj će poslati zahtjev HTTP/S se s IP adresom.  
7. Zahtjev stizati na jednu od instanci koje aplikacije koje su pokrenute na jedan od servisa okruženja aplikacije.

Konzola slici u nastavku prikazuje DNS vrijednosti za aplikaciju uzorka prilagođenu domenu uspješno rješavanje instancu aplikacije koji se izvode na jedan od ta tri uzorka aplikacije servisa okruženja (u ovom slučaju drugi od tri okruženja servisa aplikacija):

![Traženje DNS-a][DNSLookup] 

## <a name="additional-links-and-information"></a>Dodatne veze i informacije ##
Svih članaka i kako – da biste je za aplikaciju servisa okruženja dostupne u [PROČITAJME za aplikaciju servisa okruženja](../app-service/app-service-app-service-environments-readme.md).

Dokumentacija na Powershell [Azure resursa Upravitelj promet Upravitelj podržava][ARMTrafficManager].  

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]:  https://azure.microsoft.com/documentation/articles/traffic-manager-manage-profiles/
[ARMTrafficManager]:  https://azure.microsoft.com/documentation/articles/traffic-manager-powershell-arm/
[RegisterCustomDomain]:  https://azure.microsoft.com/en-us/documentation/articles/web-sites-custom-domain-name/


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
