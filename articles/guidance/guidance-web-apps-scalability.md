<properties
   pageTitle="Skalabilni web-aplikacije | Arhitektura Azure referenca | Microsoft Azure"
   description="Poboljšanje skalabilnost u web-aplikaciji koja se izvodi u Microsoft Azure."
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/12/2016"
   ms.author="mwasson"/>


# <a name="improving-scalability-in-a-web-application"></a>Poboljšanje skalabilnost u web-aplikaciji 

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

U ovom se članku prikazuje preporučene arhitektura za poboljšanje skalabilnost i performanse u web-aplikaciji sustavom Microsoft Azure. Arhitektura sastavlja [arhitektura Azure referenca: osnovni web-aplikacije][basic-web-app]. Preporuke i napomene iz članak koji se odnose na tu arhitekturu.

>[AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: Voditelj resursa i classic. U ovom se članku koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

## <a name="architecture-diagram"></a>Arhitektura dijagrama

![[0]][0]

Arhitektura sadrži sljedeće komponente:

- **Grupa resursa**. [Grupa resursa] [ resource-group] je logički spremnik za Azure resurse. 

- ** [Web-aplikacije] [ app-service-web-app] ** i ** [API aplikacije][app-service-api-app]**. Uobičajene Moderna aplikacije mogu sadržavati web-mjesto i jedan ili više RESTful web API-ji. Web-mjesto API se možda potrošena klijenti preglednika putem AJAX-a, aplikacije za nativni klijent ili poslužiteljsko aplikacije. Razmatranja na webu za dizajniranje API-ji potražite u članku [smjernice za dizajn API][api-guidance].    

- **WebJob**. Korištenje [Azure WebJobs] [ webjobs] da biste pokrenuli dugoročnih zadataka u pozadini. WebJobs mogu se izvoditi rasporedu, continously, ili kao odgovor na okidač, primjerice stavljanja poruku u redu. Na WebJob pokreće kao pozadinu proces u kontekstu aplikacije servisa za aplikacije. 

- **Red čekanja**. Arhitekture što je prikazano ovdje, redove aplikacije pozadinski zadaci postavljanjem poruke na [Pohranu reda čekanja Azure] [ queue-storage] red. Poruka pokreće funkcija u na WebJob. 

    Osim toga, možete koristiti redova Bus servisa. Da biste pogledali usporedbu potražite u članku [Azure redovima i redovi servisa Bus – usporedbi i iz Outlooka nasuprot][queues-compared].

- **Predmemoriju**. Spremanje podataka djelomično statične u [Predmemoriji Redis Azure][azure-redis].  

- **CDN**. Korištenje [Mreže za isporuku sadržaja Azure] [ azure-cdn] (CDN) predmemorije javno objavljivanje sadržaja, donjem Latencija i brže isporuku sadržaja.

- **Pohrana podataka.** Korištenje [Baze podataka SQL Azure] [ sql-db] za relacijskih podataka. Koje nisu relacijskih podataka, razmotrite NoSQL pohrana, kao što su Azure spremište tablica ili [DocumentDB][documentdb].

- **Pretraživanje azure**. Korištenje [Pretraživanja za Azure] [ azure-search] da biste dodali funkciju pretraživanja, uključujući prijedloge za pretraživanje, mutno pretraživanja i jezično specifične pretraživanja. Azure pretraživanje se obično koristi u kombinaciji s drugom spremišta podataka, osobito ako spremišta primarni podataka potreban je točno dosljednost. Na taj se način način mjerodavne podataka u druge spremišta podataka i postavili indeks pretraživanja u Azure pretraživanja. Azure pretraživanje se može koristiti i za konsolidaciju indeksa jednog pretraživanja s više spremišta podataka.  

- **E-pošte/SMS**. Ako aplikacija nije potrebno slanje e-pošte ili SMS poruke, pomoću servisa drugih proizvođača kao što su SendGrid ili Twilio, a ne sastavljanjem ta je funkcija izravno u aplikaciju.

## <a name="recommendations"></a>Preporuke

### <a name="app-service-apps"></a>Aplikacije servisa za aplikacije 

Preporučujemo stvaranje web-aplikacije i web API kao zasebna aplikacije servisa za aplikacije. Ovaj dizajn omogućuje pokretanje ih u zasebnom aplikacije servisa za tarife koje shodno omogućuje vam da ih neovisno skaliranja. Ako ne trebate te razine skalabilnost pri najprije možete uvesti aplikacije u istu tarifu i ih premjestite u zasebnom tarife kasnije, ako je potrebno. (Za tarife Basic, standardna i Premium naplatu instanci VM u planu ne po aplikacije. U odjeljku [aplikacije servisa za cijene][app-service-pricing])

Ako namjeravate koristiti *Jednostavno tablice* ili *Jednostavno API -ji* značajke servisa mobilna aplikacija, stvoriti zasebnu aplikacije servisa za aplikaciju u tu svrhu.  Te značajke oslanjate framework određenu aplikaciju da biste omogućili ih.

### <a name="webjobs"></a>WebJobs

Ako je na WebJob resurs intenzivno, razmislite o implementaciji prazne aplikacije servisa za aplikaciju u zasebnom aplikacije servisa za tarifu, da biste prikazali namjenski instance na WebJob. Potražite u članku [smjernice za zadatke pozadine][webjobs-guidance].  

### <a name="cache"></a>Predmemoriju

Možete poboljšati performanse i skalabilnost pomoću [Predmemorije Redis Azure] [ azure-redis] u predmemoriju neke podatke. Razmislite o korištenju Redis predmemoriju za:

- Podaci o djelomično statički transakcije.

- Stanje sesije.

- HTML izlaz. To može biti korisno u aplikacijama koje se iscrtavaju složene HTML izlaz. 

Detaljnije upute o dizajniranju predmemoriranja strategije, potražite u članku [smjernice za predmemoriranje][caching-guidance].

### <a name="cdn"></a>CDN 

Korištenje [Azure CDN] [ azure-cdn] u predmemoriji statički sadržaj. Glavna prednost ustvari CDN je da biste smanjili Latencija za korisnike, jer je sadržaj predmemorirana pri za *rubni poslužitelj* koji je geografski blizu korisnika. CDN možete smanjiti opterećenje aplikacije, budući da tu promet se obrađuju aplikacija.

- Ako aplikacije sastoji se od uglavnom statičnih stranica, preporučujemo da koristite CDN u predmemoriju cijelu aplikaciju. Potražite u članku [Korištenje Azure CDN u aplikacije servisa za Azure][cdn-app-service].

- U suprotnom, stavite statički sadržaj, kao što su slike, CSS i HTML datoteka, u Azure prostora za pohranu i korištenje CDN u predmemoriju te datoteke. U odjeljku [Integracija s računom za pohranu s CDN][cdn-storage-account].

> [AZURE.NOTE] Azure CDN ne može vam poslužiti sadržaj koji zahtijeva provjeru autentičnosti.

Detaljnije upute potražite u članku [smjernice sadržaja isporuke mreže (CDN)][cdn-guidance]. 

### <a name="storage"></a>Prostor za pohranu

Moderna aplikacije često obraditi velike količine podataka. Da bi se promijenila veličina za oblak, je važno da biste odabrali vrstu desno za pohranu. Ovo su neke osnovne preporuke.  Detaljnije upute potražite u članku [Assessing mogućnosti pohrane podataka za rješenja postojanost Polyglot][polyglot-storage].

Želite li pohraniti | Primjer | Preporučena prostora za pohranu
--- | --- | ---
Datoteka | Slike, dokumenti, PDF-ova | Spremište blobova platforme Azure
Parove ključa vrijednosti | Pretražuje korisnički ID podataka korisničkog profila | Spremište tablica platforme Azure
Kratke poruke koji želite pokrenuti daljnje obrade | Zahtjevi za narudžbe | Azure reda čekanja za pohranu, red čekanja Bus servisu ili servis Bus teme
Osobe koje nisu relacijskih podataka, s fleksibilne sheme koje je obavezna osnovni upita | Katalog | Dokument baze podataka, primjerice Azure DocumentDB, MongoDB ili Apache CouchDB
Relacijskih podataka koje je obavezna bogatiji podršku upita, izričite sheme i/ili istaknuti dosljednosti | Zalihe proizvoda | Baze podataka Azure SQL

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

### <a name="app-service-app"></a>Aplikacije servisa za aplikaciju

Ako rješenje sadrži nekoliko aplikacije servisa za aplikacije, razmislite o implementaciji ih za razdvajanje aplikacije servisa za tarife. Taj se način omogućuje skaliranje ih neovisno o jer se pokrenu na zasebnom instance. Dodatne informacije o skaliranje odgovor potražite u članku [pitanja vezana uz skalabilnost] [ basic-web-app-scalability] sekciju u [osnovni web aplikacije arhitektura][basic-web-app].

Isto tako, razmislite o stavljanje na WebJob u vlastiti plan tako da se ne pozadinske zadatke pokrenuti na istoj instanci koji upravljaju HTTP zahtjeva.  

### <a name="sql-database"></a>SQL baze podataka

Povećavanje skalabilnost baze podataka SQL *sharding* baze podataka &mdash; odnosno vodoravno particija baze podataka. Sharding omogućuje vodoravno skaliranje korištenje baze podataka pomoću [alata baze podataka za Elastic][sql-elastic]. Potencijalne prednosti sharding obuhvaćaju sljedeće:

- Bolje propusnost transakcije.

- Upiti se izvoditi brže putem podskup podataka. 

### <a name="azure-search"></a>Azure pretraživanja

Azure pretraživanja uklanja indirektnog izvođenje složenih podataka pretraživanja iz spremišta primarni podataka, a mogu mijenjati veličinu za rukovanje Učitaj. Potražite u članku [Skaliranje resursa razine upita i indeksiranja radnih opterećenja u pretraživanju Azure][azure-search-scaling].

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

### <a name="cross-origin-resource-sharing-cors"></a>Unakrsno polazište resursa zajedničko korištenje (CORS)

Ako stvorite web-mjesta i API na webu kao zasebna aplikacija, web-mjesta može se klijentsko AJAX poziva na API osim ako omogućite CORS. 

> [AZURE.NOTE] Sigurnost preglednika onemogućuje upućivanje zahtjeva za AJAX-a na drugu domenu web-stranicu. To ograničenje zove isti polazište pravila, a onemogućuje zlonamjernih web-mjesta u članku sentitive podataka iz drugog web-mjesta. CORS je standardna W3C koji omogućuje poslužitelja Opustite pravila isti polazište i dopustiti zahtjeva za neke unakrsno polazište tijekom odbijanje drugima.

Aplikacije servisa ima ugrađenu podršku za CORS, bez obzira na to pisanja koda aplikacije. Potražite u članku [utrošak API aplikacije iz JavaScript pomoću CORS][cors]. Dodavanje web-mjesta na popis dopuštenih drugačijeg izvora za na API-JA. 

### <a name="sql-database-encryption"></a>Šifriranje baze podataka SQL

Koristite [Prozirnu šifriranje podataka] [ sql-encryption] ako vam je potrebna za šifriranje podataka na ostale u bazi podataka. Ta značajka izvodi u stvarnom vremenu šifriranje i dešifriranje cijelu bazu podataka (uključujući sigurnosne kopije i datoteke zapisnika transakcije), bez promjene u aplikaciju. Šifriranje dodajte neki Latencija pa se preporučuje za sortiranje podataka koji se moraju biti siguran u vlastitu bazu podataka i omogućivanje šifriranja samo za tu bazu podataka.  

## <a name="next-steps"></a>Daljnji koraci

- Veća dostupnost implementacija aplikacije u više regija i pomoću [Upravitelja promet Azure] [ tm] za prebacivanje. Dodatne informacije potražite u članku [arhitektura Azure referenca: web-aplikacija s visoke dostupnosti][web-app-multi-region].    

<!-- links -->

[api-guidance]: ../best-practices-api-design.md
[app-service-web-app]: ../app-service-web/app-service-web-overview.md
[app-service-api-app]: ../app-service-api/app-service-api-apps-why-best-platform.md
[app-service-pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[azure-cdn]: https://azure.microsoft.com/en-us/services/cdn/
[azure-redis]: https://azure.microsoft.com/en-us/services/cache/
[azure-search]: https://azure.microsoft.com/en-us/documentation/services/search/
[azure-search-scaling]: ../search/search-capacity-planning.md
[background-jobs]: ../best-practices-background-jobs.md
[basic-web-app]: guidance-web-apps-basic.md
[basic-web-app-scalability]: guidance-web-apps-basic.md#scalability-considerations 
[caching-guidance]: ../best-practices-caching.md
[cdn-app-service]: ../app-service-web/cdn-websites-with-cdn.md
[cdn-storage-account]: ../cdn/cdn-create-a-storage-account-with-cdn.md
[cdn-guidance]: ../best-practices-cdn.md
[cors]: ../app-service-api/app-service-api-cors-consume-javascript.md
[documentdb]: https://azure.microsoft.com/en-us/documentation/services/documentdb/
[polyglot-storage]: https://github.com/mspnp/azure-guidance/blob/master/Polyglot-Solutions.md
[queue-storage]: ../storage/storage-dotnet-how-to-use-queues.md
[queues-compared]: ../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md
[resource-group]: ../resource-group-overview.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-elastic]: ../sql-database/sql-database-elastic-scale-introduction.md
[sql-encryption]: https://msdn.microsoft.com/en-us/library/dn948096.aspx
[tm]: https://azure.microsoft.com/en-us/services/traffic-manager/
[web-app-multi-region]: ./guidance-web-apps-multi-region.md
[webjobs-guidance]: ../best-practices-background-jobs.md
[webjobs]: ../app-service/app-service-webjobs-readme.md
[0]: ./media/blueprints/paas-web-scalability.png "Web-aplikacije u Azure s poboljšane skalabilnost"