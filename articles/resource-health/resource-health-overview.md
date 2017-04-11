<properties
   pageTitle="Pregled stanja Azure resursa | Microsoft Azure"
   description="Pregled stanja Azure resursa"
   services="Resource health"
   documentationCenter="dev-center-name"
   authors="BernardoAMunoz"
   manager=""
   editor=""/>

<tags
   ms.service="resource-health"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Supportability"
   ms.date="06/01/2016"
   ms.author="BernardoAMunoz"/>

# <a name="azure-resource-health-overview"></a>Pregled stanja Azure resursa

Azure stanja resursa je servis koji se izlaže stanja pojedinačnih Azure resursa i njihovi s akcijama upute za otklanjanje poteškoća. U okruženje oblaka kojoj nije moguće izravno pristupiti poslužitelja ili infrastrukture elemenata cilj za stanje resursa je da biste smanjili vrijeme kupci potrošiti na otklanjanje poteškoća, posebice smanjivanje potrošeno vrijeme određivanje ako korijenu problem lays unutar aplikacije ili je uzrokovano događaja unutar platforme Azure.

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-the-resource-is-healthy-or-not"></a>Što se smatra resursa i kako resursa stanja odlučuje ako je resurs dobar ili ne? 
Resurs je korisnički stvorenim instancu vrste resursa nudi uslugu, na primjer: virtualnog računala, web-aplikacijama ili SQL baze podataka. 

Stanje resursa ovisi o signale čuje po resursa i/ili usluge za utvrđivanje resursa dobar ili ne. Važno je da obratite pozornost na to da trenutno resursa stanje samo račune za stanje jedan određeni resurs upišite i razmotrite ostale elemente koje možda pridonose stanje. Na primjer, prilikom prijavljivanja status virtualnog računala, samo računalnim dio Infrastruktura se smatra, odnosno problema u mreži neće se prikazivati u stanju resursa, osim ako postoji prekida u deklarirane servisa u tom slučaju ga će se kada povučete kroz natpis na vrhu na plohu. Dodatne informacije o nedostupnosti servisa je ponuđen u nastavku ovog članka. 

## <a name="how-is-resource-health-different-from-service-health-dashboard"></a>Kako se stanja resursa razlikuje od nadzorna ploča za stanje servisa?

Informacije koje ste dobili od stanja resursa je precizniji nego što je nudi nadzorna ploča za stanje servisa. Dok SHD komunicira događaje koje utjecati na dostupnost usluge u regiji, stanja resursa izlaže dodatne informacije o određenog resursa, npr će izložiti događaje koje utjecati na dostupnost virtualnog računala i web-aplikacijama ili u bazi podataka za SQL. Na primjer, ako čvor neočekivano izvršen, kupcima čiji virtualnim strojevima pokrećete na tom čvor bit će možete nabaviti razloga zašto njihove VM nije dostupna za određeno vremensko razdoblje.   

## <a name="how-to-access-resource-health"></a>Kako pristupiti stanja resursa
Za usluge dostupne putem resursa stanja načina 2 za pristup resursu stanja.

### <a name="azure-portal"></a>Portal za Azure
Stanje plohu resursa na portalu za Azure pruža podrobne informacije o stanju resursa kao i preporučene akcije koje se razlikuju se ovisno o trenutno stanje resursa. Ovaj plohu omogućuje najbolje mogućnosti rada kada ispitivanje stanja resursa, kao što je olakšava pristup drugim resursima unutar portalu. Kao što je rečeno prije nego što će razlikuju se ovisno o trenutno stanje skup preporučene akcije u stanju plohu resursa:

* Dobar resurse: jer nije problem koji može utjecati na stanje resursa otkrivena akcije su filtriran na pomoć procesom za otklanjanje poteškoća. Na primjer, nudi izravan pristup plohu za otklanjanje poteškoća koja sadrži upute za rješavanje najčešće nominalne kupci problema.
* Dobro resursa: ima li problema uzrokovanih Azure, na plohu prikazat će se akcije Microsoft traje (ili je) da biste oporavili resurs. Problema uzrokovanih korisnika pokrenuti Akcije, plohu će na popisu klijenata akcije može potrajati pa adresa problem i oporavak resurs.  

Kada ste prijavljeni na Portal Azure, dva su načina za pristup plohu stanja resursa: 

###<a name="open-the-resource-blade"></a>Otvorite plohu resursa
Otvorite plohu resursa za dani resurs. Na postavke plohu koji će se otvoriti pokraj resursa plohu kliknite stanje resursa da biste otvorili plohu stanja resursa. 

![Plohu stanja resursa](./media/resource-health-overview/resourceBladeAndResourceHealth.png)

### <a name="help-and-support-blade"></a>Pomoć i podrška plohu
Da biste otvorili plohu pomoći i podrške, klikom na upitnik u gornjem desnom kutu, zatim odaberete pomoć + podrška. 

**Na gornjoj navigacijskoj traci**

![Pomoć + podrška](./media/resource-health-overview/HelpAndSupport.png)

Klikom na pločici otvara se plohu za pretplatu stanja resursa koji će se popis svih resursa u svoju pretplatu. Pokraj svakog resursa postoji ikona koja označava njegov stanja. Klikom na svakom resursu otvorit će se stanje plohu resursa.

**Pločica stanja resursa**

![Pločica stanja resursa](./media/resource-health-overview/resourceHealthTile.png)

## <a name="what-does-my-resource-health-status-mean"></a>Što znači status stanja resursa?
Postoje 4 različite stanja statusi koje možete vidjeti vaše resursa.

### <a name="available"></a>Dostupna
Servis je otkrio probleme u platformu koja može biti koje utječu na dostupnost resursa. To je naznačeno ikonom Zelena kvačica. 

![Dostupna je resurs](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Nije dostupan

U ovom slučaju servis otkrio tijeku problem u platformu koja je koje utječu na dostupnost resursa, na primjer, čvor gdje je pokrenut na VM neočekivano sustava. To je označen crvenu ikonu upozorenja. Dodatne informacije o problemu navedeni su u odjeljku srednjem plohu, uključujući: 

1.  Akcije koje Microsoft traje da biste oporavili resursa 
2.  Detaljne vremenske crte problem, uključujući vrijeme očekivani razlučivosti
3.  Popis preporučene akcije za korisnike 

![Resurs nije dostupan](./media/resource-health-overview/Unavailable.png)

### <a name="unavailable--customer-initiated"></a>Nije dostupno – kupca pokrenut
Resurs nije dostupan zbog zahtjev kupca kao što je zaustavljanje resursa ili zahtijeva ponovno pokretanje računala. To je označen plavu ikonu informativna. 

![Resurs nije dostupan zbog korisnika pokrenuti akcije](./media/resource-health-overview/userInitiated.png)

### <a name="unknown"></a>Nepoznato
Servis je primio informacije o ovim resursom za više od pet minuta. To je označen sivom upitnik. 

Važno Imajte na umu da to nije potpuni oznaka da nešto nije u redu s resursa, da bi korisnici trebali biste slijedite ove preporuke je:

* Ako resurs radi prema očekivanjima, ali njegov stanja postavljen na Nepoznato u stanju resursa, nema problema, a možete očekivati status resursa za ažuriranje dobar nakon nekoliko minuta.
* Ako postoje probleme prilikom pristupa resursa i njegov stanja postavljen na Nepoznato u stanju resursa, to može biti Prijevremeni podatak koji se može biti problem i dodatne istrage računajući dok stanja ažurira dobar ili dobro

![Stanje resursa nije poznat](./media/resource-health-overview/unknown.png)

## <a name="service-impacting-events"></a>Događaji koje utječu na servis
Ako resurs možda utječe tijeku usluge koje utječu na događaj, na natpisu prikazat će se pri vrhu plohu stanja resursa. Klikom na na natpisu otvara se plohu događaje nadzora koja će se prikazati dodatne informacije o se prekida.

![Stanje resursa možda utječe na SIE](./media/resource-health-overview/serviceImpactingEvent.png)

## <a name="what-else-do-i-need-to-know-about-resource-health"></a>Što još treba znati o stanju resursa?

### <a name="signal-latency"></a>Latencija signal
Na signale koji sažetak sadržaja stanje resursa, možda je do 15 min odgođeno, što može izazvati nepodudaranje između trenutnog stanja statusa resursa i njegov stvarni dostupnost. Važno je da to imati na umu prilikom će olakšati uklanjanje nepotrebnih vremena utrošenog istražuje mogućih problema. 

### <a name="special-case-for-sql"></a>Posebne slučaj za SQL 
Izvješća o stanju za resursa stanja baze podataka SQL, ne SQL server. Tijekom rada ovu rutu nudi realističniji stanja slike, bit će potrebno da više komponente i usluge uzeti u obzir radi određivanja stanja baze podataka. Trenutni signal ovisi o prijave u bazu podataka, što znači da za baze podataka koje primate običnog prijave (koji sadrži između ostalog, prima zahtjeve za izvršavanje upita) stanja status prikazat će se redovito. Ako bazu podataka ne pristupiti razdoblja od 10 minuta ili više njih, će se premjestiti u Nepoznato stanje. To znači da je baza podataka nije dostupan, samo je da je nema signala čuje jer obavite bez prijave. Povezivanje s bazom podataka i izvodi upit će šalji signale potrebne za određivanje i ažuriranje statusa stanja baze podataka.

## <a name="feedback"></a>Povratne informacije
Uvijek smo otvoreno za povratne informacije i prijedlozi! Pošaljite nam [prijedloge](https://feedback.azure.com/forums/266794-support-feedback). Uz to, koje mogu sudjelovati s nama putem [na Twitteru](https://twitter.com/azuresupport) ili na [forume MSDN](https://social.msdn.microsoft.com/Forums/azure).
