<properties
    pageTitle="Izvlačenja javnih podataka u Azure događaj koncentratora | Microsoft Azure"
    description="Pregled događaja koncentratora uvoz iz uzorka za web"
    services="event-hubs"
    documentationCenter="na"
    authors="spyrossak"
    manager="timlt"
    editor=""/>

<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/25/2016"
    ms.author="spyros;sethm" />

# <a name="pulling-public-data-into-azure-event-hubs"></a>Izvlačenja javnih podataka u Azure događaj koncentratora

Uobičajeni scenariji Internet stvari (IoT), imate uređaje koji se mogu program za slanje podataka Azure ili programa Azure koncentrator za događaj ili koncentratora za IoT. Točke unosa u Azure su oba ta koncentratora za pohranu, analiza i vizualizacija s myriad alata postaju dostupni na Microsoft Azure Međutim, kad oba potreban je automatske podataka da biste ih oblikovan kao JSON i zaštiti na određene načine. Tako ćete prikazati sljedeće pitanje. Što učiniti ako želite prikupiti podatke iz izvora javna ili privatna gdje podaci predstavljeni kao web-servisa ili sažetak sadržaja neke sortiranja, ali nemate mogućnost da biste promijenili način se objavljuje podatke? Razmislite o vremenske prognoze ili promet ili o dionicama - koji ne može odrediti NOAA, ili WSDOT ili NASDAQ da biste konfigurirali Pritisni za koncentratora za događaj. Da biste riješili taj problem, ćemo ste napisali i Otvori-izvorni termini small oblaka uzorak koji možete izmijeniti i implementirati koji će povlačenje podataka iz nekog takve izvora i automatske da biste koncentratora za događaj. Iz nje, to možete učiniti ono volji, predmeta, Naravno, da biste odredbi iz producentu. Možete pronaći aplikacije [ovdje](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/).

Imajte na umu da kod u ovom primjeru prikazuje samo kako za izvlačenje podataka s weba uobičajeni sažeci sadržaja i pisati programa Azure koncentrator za događaj. To nije namijenjen da se u aplikaciju radnog i bez pokušaja uvedena da biste prikladna za upotrebu u takvim okruženju – je strictfly DIY, za razvojne inženjere odnosila primjer samo. Osim toga, postojanje parametra ovaj uzorak nije tantamount to preporuke treba **izvlačenje** podataka u Azure umjesto **automatske** ga. Trebali biste pregledajte sigurnost, performanse, funkcija i cijena čimbenika prije podmirenje na arhitekturi do kraja do kraja.

## <a name="application-structure"></a>Strukturu aplikacije

Aplikacija napisan C#, a [Uzorak opisa](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) sadrži sve potrebne informacije za izmjenu, stvaranje i objavljivanje aplikacija. Sljedeći odjeljci sadrže više razine pregled funkcija aplikacije.

Ne možemo počinju pretpostavci kojima imate pristup sažetka sadržaja podataka. Na primjer, možda ćete morati izvukli promet podatke iz na Washington stanje odjel od prijevoza ili vremenske prognoze podatke iz NOAA, da biste prikazali prilagođena izvješća ili da biste spojili podatke s drugih podataka u aplikaciji. Također morat ćete ste postavili programa Azure koncentrator za događaj i znate veza niz potrebne za pristup.

Kada je rješenje GenericWebToEH pokrene, čita konfiguracijska datoteka (App.config) da biste dobili nekoliko stvari:

1. URL ili popis URL-ovi, podaci za objavljivanje web-mjesta. Najbolje je web-mjesto koje objavljuje podatke u JSON, kao što su oni koje referencira WSDOT [ovdje](http://www.wsdot.wa.gov/Traffic/api/). 
2. Vjerodajnice za URL-a, ako je potrebno. Mnoge javni izvori potrebne vjerodajnice ili vjerodajnice možete pohraniti na niz URL-a. Neke su potrebne da unesete zasebno. (Imajte na umu da možete navesti samo jedan skup vjerodajnica u ovoj aplikaciji tako da ga će raditi samo ako navedete samo jedan URL-a, ne popis URL-ova.)
3. Niz za povezivanje i naziv središtu događaja u tom naziva koncentratora događaj koji će automatske podatke. Ove informacije možete pronaći na portalu za Azure.
4. Stanje mirovanja interval, u milisekundama, za interval između provjere javnih podataka web-mjesta. Postavljanje zahtijeva neke misli. Ako je to diskovni ankete, mogli biste propustiti podatke. s druge strane, ako to često ankete mogla bi vam se velike količine podataka koji se ponavljaju ili lošiji još koje možda blokira kao nefarious robot. Razmislite o učestalosti ažuriranja izvora podataka – vremenske prognoze ili promet podataka Osvježi svakih 15 minuta, no možda svakih nekoliko sekundi, ovisno o tome gdje ih nabaviti o dionicama. 
5. Zastavica za aplikaciju li uskoro podatke obliku JSON ili XML. Budući da vam je potrebna da biste podatke na koncentratora za događaj, aplikacija ima modulu pretvorite XML u JSON prije slanja.

Kad pročitate konfiguracijska datoteka aplikacije ulazi u petlja - pristupa na javno web-mjestu, pretvaranje podataka prema potrebi pisanja koncentratora za događaj, a zatim čeka se interval mirovanja prije prelaska na se sve ponovno. Konkretno:

  * Javno web-mjesto za čitanje. Primanje podataka spremna za slanje instancu klase RawXMLWithHeaderToJsonReader koristi se s Azure/GenericWebToEH/ApiReaders/RawXMLWithHeaderToJsonReader.cs. Čita izvorni tok u način GetData() i dijeli je na manjim dijelova (odnosno zapisi) pomoću GetXmlFromOriginalText. 
  Ova metoda će čitati XML te ispravnog JSON ili JSON polja. Zatim obrade pokretanja pomoću konfiguracije MergeToXML iz App.config (zadani = prazan).
  * Primanje i slanje podataka je implementirana kao petlje u način Process() u Program.cs. 
  Nakon primanja rezultata za izlaz iz GetData(), enqueues način odvojene zarezom koncentrator za događaj.

## <a name="next-steps"></a>Daljnji koraci

Da biste implementirali rješenje, Kloniraj ili preuzeti aplikaciju [GenericWebToEH](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) , uredite datoteku App.config, je i na kraju i objavljivanje. Kada ste objavili aplikaciju, vidjet ćete raditi na portalu Azure klasični u odjeljku servise u Oblaku, a na kartici **Konfiguriraj** možete promijeniti neke postavke konfiguracije (kao što su cilj koncentratora za događaj i interval mirovanja).

Pogledajte više uzoraka koncentratora događaja u [galeriju Azure uzoraka](https://azure.microsoft.com/documentation/samples/?service=event-hubs) i na [MSDN-u](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5).
