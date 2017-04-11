<properties
   pageTitle="Pogreška način analizu | Microsoft Azure"
   description="Smjernice za analiziranja neuspjeh način rada za oblak rješenja Azure na temelju."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="mwasson"/>

# <a name="azure-resiliency-guidance-failure-mode-analysis"></a>Upute za Azure otpornost: pogreška način analizu

Nije uspjelo način analysis (FMA) je postupak za otpornost u sustavu, tako da izdvojite moguće pogreška točke u sustavu. Na FMA mora biti dio faze arhitektura i dizajna tako da možete izraditi oporavak pogreške u sustav od početka.

Slijede općenite postupak vođenje programa FMA: 

1. Odredite sve komponente u sustavu. Uvrštavanje vanjske ovisnosti, kao što su davatelji identiteta, servisa drugih proizvođača i tako dalje.   

2. Za svaku komponentu odredite potencijalne pogreške koji se može pojaviti. Jedan komponente može imati više od jedne pogreške način. Ako, na primjer, trebali biste razmislite o čitanju pogrešaka i zapisivanja u zasebne jer razlikovat će se mitigations utjecaj i moguće.

3. Ocjenjivanje svakom načinu rada nije uspjelo prema njezin cjelokupan rizika. Razmotrite sljedeće čimbenike:  

    - Što je vjerojatnost pogreške. Je li relativno uobičajenih? Extrememly rijetko? Ne morate točno brojeva. Svrha je rangiranje prioritet.

    - Kakav je učinak na aplikaciju, dostupnost, gubitka podataka ubuduće, novčani trošak i prekidu tvrtke? 

4. Za svaki način neuspjeh odredite kako će se aplikacija odgovaranje i oporavak. Razmislite o tradeoffs u složenosti troška i aplikacije.   

Kao početnu točku za proces FMA ovaj članak sadrži katalog potencijalne pogreške načina i njihovih mitigations. Katalog je organiziran prema tehnologije ili Azure service plus kategoriju Općenito za dizajn razini aplikacije. Katalog nije iscrpan, ali obuhvaća mnogo core Azure services. 


## <a name="app-service"></a>Aplikacije servisa

### <a name="app-service-app-shuts-down"></a>Isključuje se aplikacije servisa za aplikacije.

**Otkrivanje**. Mogući uzroci:

- Očekivani zatvaranja
    
    - Operator Zatvara aplikaciju; na primjer, pomoću portala za Azure.

    - Aplikacija je uklonjen jer je u stanju mirovanja. (Samo ako se `Always On` je onemogućena postavka.)

- Neočekivanih zatvaranja

    - Zatvara se aplikaciju.

    - Instance aplikacije servisa VM postane nedostupan.


Zapisivanje Application_End će Uhvatite domene zatvaranja aplikacije (ruši se proces meki), a je jedini način da biste privukli isključivanje računala domene za aplikacije.

**Oporavak**

- Ako je očekivao se isključivanje računala, pomoću događaja zatvaranja aplikacije isključiti bez poteškoća. Na primjer, u ASP.NET, koristite na `Application_End` način.

- Ako aplikacija nije učitan dok je u stanju mirovanja, automatski pokrene na zahtjev za sljedeće. Međutim, će uzrokovati "Hladna početka" trošak. 

- Da biste spriječili aplikacije iz radne memorije dok je u stanju mirovanja, omogućiti na `Always On` u web-aplikaciji. Potražite u članku [Konfiguriranje web-aplikacije u aplikacije servisa za Azure][app-service-configure].

- Da biste spriječili operator isključuje aplikaciju, postavite lock resursa s `ReadOnly` razinu. Potražite [Lock resursima pomoću upravitelja za Azure resursa][rm-locks].

- Ako aplikaciju ruši ili neku aplikaciju servisa VM postane nedostupan, aplikacije servisa za automatski ponovo pokrenuti aplikaciju. 


**Dijagnostika**. Aplikacija zapisnicima i zapisnici web poslužitelja. U odjeljku [Omogućivanje Dijagnostika bilježenje u zapisnik web-aplikacijama u aplikacije servisa za Azure][app-service-logging].


### <a name="a-particular-user-repeatedly-makes-bad-requests-or-overloads-the-system"></a>Određeni korisnički više puta čini neispravni zahtjeve ili overloads sustav. 

**Otkrivanje**. Provjeru autentičnosti korisnika i sadržavati korisničkog ID-JA u aplikaciji zapisnika.

**Oporavak**

- Koristite [Upravljanje API Azure] [ api-management] na zahtjeve ograničenja od korisnika. U odjeljku [Napredne zahtjev za ograničavanje s upravljanjem API Azure][api-management-throttling]

- Blokiranje korisnika.

**Dijagnostika**. Prijavite se sve zahtjeve za provjeru autentičnosti.


### <a name="a-bad-update-was-deployed"></a>Neispravni ažuriranje je uveden.

**Otkrivanje**. Praćenje stanja računala putem portala za Azure (potražite u članku [Monitor Azure web app performanse][app-insights-web-apps]) ili implementirati [stanja krajnjoj točki nadzora uzorak][health-endpoint-monitoring-pattern].

**Oporavak**. Korištenje više [slobodnih implementaciju] [ app-service-slots] i poništiti implementacije posljednje dobro. Dodatne informacije potražite u članku [osnovni web aplikacije referenca arhitektura][ra-web-apps-basic].


## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="openid-connect-oidc-authentication-fails"></a>Provjera autentičnosti povezivanje OpenID (OIDC) ne uspijeva.

**Otkrivanje**. Načini moguće pogreške obuhvaćaju sljedeće:

1. Azure AD nije dostupna ili nije moguće dohvatiti zbog problema s mrežom. Preusmjeravanje krajnja točka za provjeru autentičnosti ne uspije, a proizvod OIDC throws iznimku.
2. Azure AD klijent ne postoji. Preusmjeravanja na krajnjoj točki provjere autentičnosti vraća HTTP kod pogreške, a proizvod OIDC throws iznimku.
3. Korisnik ne može provjeriti valjanost. Nema strategije otkrivanje nužni; Azure AD rukuje prijavu pogrešaka.

**Oporavak**

1. Uhvatite neobrađenu iznimke iz na proizvod.
2. Rukovanje `AuthenticationFailed` događaja.
3. Preusmjeravanje korisnika na stranicu s pogreškom.
4. Ponovne pokušaje korisnika.


## <a name="azure-search"></a>Azure pretraživanja

### <a name="writing-data-to-azure-search-fails"></a>Pisanje podataka Azure pretraživanje neće uspjeti.

**Otkrivanje**. Uhvatite `Microsoft.Rest.Azure.CloudException` pogreške.

**Oporavak**

[Pretraživanje .NET SDK] [ search-sdk] automatski ponovnih pokušaja nakon tranzitne neuspješnih pokušaja. Pogreške koje nisu prolaznim smatrati iznimaka izbačena klijent SDK.

Zadana pravila Ponovi koristi eksponencijalne natrag slobodno. Da biste koristili različite pokušaj pravila, nazovite `SetRetryPolicy` na na `SearchIndexClient` ili `SearchServiceClient` predmete. Dodatne informacije potražite u članku [Automatsko ponovnih pokušaja][auto-rest-client-retry].

**Dijagnostika**. Korištenje [pretraživanja promet analize][search-analytics].


### <a name="reading-data-from-azure-search-fails"></a>Čitanje podataka iz pretraživanja Azure neće uspjeti.

**Otkrivanje**. Uhvatite `Microsoft.Rest.Azure.CloudException` pogreške.

**Oporavak**

[Pretraživanje .NET SDK] [ search-sdk] automatski ponovnih pokušaja nakon tranzitne neuspješnih pokušaja. Pogreške koje nisu prolaznim smatrati iznimaka izbačena klijent SDK.

Zadana pravila Ponovi koristi eksponencijalne natrag slobodno. Da biste koristili različite pokušaj pravila, nazovite `SetRetryPolicy` na na `SearchIndexClient` ili `SearchServiceClient` predmete. Dodatne informacije potražite u članku [Automatsko ponovnih pokušaja][auto-rest-client-retry].

**Dijagnostika**. Korištenje [pretraživanja promet analize][search-analytics].



## <a name="cassandra"></a>Cassandra


### <a name="reading-or-writing-to-a-node-fails"></a>Čitanje ili pisanje čvor neće uspjeti.

**Otkrivanje**. Uhvatite iznimku. Za .NET klijente, to obično bit će `System.Web.HttpException`. Drugi klijent možda drugih vrsta iznimke.  Dodatne informacije potražite u članku [Cassandra pogreškama gotovo desno](http://www.datastax.com/dev/blog/cassandra-error-handling-done-right).

**Oporavak**

- Svaki [klijent Cassandra](https://wiki.apache.org/cassandra/ClientOptions) ima vlastitu pravila Ponovi i mogućnosti. Dodatne informacije potražite u članku [Cassandra pogreškama gotovo desno][cassandra-error-handling].
- Koristite za bicikle umu implementaciji podataka čvorove raspodijeliti kvara domene.
- Implementirati na više područja s lokalnom kvorum dosljednost. Ako dođe do pogreške koje nisu prolaznim, uspjeti iznad neke druge regije.

**Dijagnostika**. Zapisnici aplikacije


## <a name="cloud-service"></a>Servis u oblaku

### <a name="web-or-worker-roles-are-unexpectedlybeing-shut-down"></a>Se neočekivano isključuje weba ili tempiranja uloge.

**Otkrivanje**. [RoleEnvironment.Stopping] [ RoleEnvironment.Stopping] je pokrenuta događaj.

**Oporavak**. Zanemarivanje [RoleEntryPoint.OnStop] [ RoleEntryPoint.OnStop] način da biste očistili bez poteškoća. Dodatne informacije potražite u članku [U način desno za rukovanje događajima Azure OnStop] [ onstop-events] (blog).


## <a name="documentdb"></a>DocumentDB

### <a name="reading-data-from-documentdb-fails"></a>Čitanje podataka iz DocumentDB neće uspjeti.

**Otkrivanje**. Uhvatite `System.Net.Http.HttpRequestException` ili `Microsoft.Azure.Documents.DocumentClientException`. 

**Oporavak**

- SDK automatski ponovnih pokušaja pokušaji nije uspjelo. Da biste postavili broj ponovne pokušaje i vrijeme čekanja Maksimalna, konfiguriranje `ConnectionPolicy.RetryOptions`. Podiže klijent iznimke su izvan pravila Ponovi ili nisu tranzitne pogreške. 

- Ako DocumentDB regulira klijent, vratit će pogrešku 429 HTTP-a. Prijava Šifra stanja na `DocumentClientException`. Ako vam se dosljedno prikazuju pogreške 429, razmislite o povećanju vrijednost propusnost zbirke DocumentDB.

- Baza podataka DocumentDB replicirati preko dva ili više područja. Sve replike su čitljiv. Pomoću klijentskog programa SDK-ovi, navedite u `PreferredLocations` parametar. To je numerirani popis Azure područja. Sve čitanja poslat će se prvi dostupan područja na popisu. Ako zahtjev ne uspije, klijent će pokušati područja na popisu redoslijedom. Dodatne informacije potražite u članku [razvojnom programiranju s računima DocumentDB više područja][docdb-multi-region].

**Dijagnostika**. Prijavite se sve pogreške na klijentskoj strani.


### <a name="writing-data-to-documentdb-fails"></a>Pisanjem podataka ne uspije DocumentDB.

**Otkrivanje**. Uhvatite `System.Net.Http.HttpRequestException` ili `Microsoft.Azure.Documents.DocumentClientException`. 

**Oporavak**

- SDK automatski ponovnih pokušaja neuspjelih pokušaja. Da biste postavili broj ponovne pokušaje i vrijeme čekanja Maksimalna, konfiguriranje `ConnectionPolicy.RetryOptions`. Podiže klijent iznimke su izvan pravila Ponovi ili nisu tranzitne pogreške. 

- Ako DocumentDB regulira klijent, vratit će pogrešku 429 HTTP-a. Prijava Šifra stanja na `DocumentClientException`. Ako vam se dosljedno prikazuju pogreške 429, razmislite o povećanju vrijednost propusnost zbirke DocumentDB.

- Baza podataka DocumentDB replicirati preko dva ili više područja. Primarni područja ne uspije, drugoj regiji će dobiti ulogu za pisanje. Na prebacivanje možete pokrenuti i ručno. SDK ne automatsko otkrivanje i usmjeravanje, tako da kod aplikacija će se i dalje funkcionira nakon na prebacivanje. Tijekom razdoblja za prebacivanje (obično u minutama) operacije pisanja imati veći Latencija kao SDK pronađe novo područje za unos. Dodatne informacije potražite u članku [razvojnom programiranju s računima DocumentDB više područja][docdb-multi-region].

- Kao pričuvne, zadržava dokument čekanja za sigurnosne kopije, a kasnije obradu reda čekanja.

**Dijagnostika**. Prijavite se sve pogreške na klijentskoj strani.


## <a name="elasticsearch"></a>Elasticsearch

### <a name="reading-data-from-elasticsearch-fails"></a>Čitanje podataka iz Elasticsearch neće uspjeti.

**Otkrivanje**. Uhvatite odgovarajuće iznimke za određeni [klijent Elasticsearch] [ elasticsearch-client] koristi. 

**Oporavak**

- Koristite mehanizam pokušajte ponovno. Svaki klijent ima vlastitu pravila Ponovi. 

- Implementacija više Elasticsearch čvorove i korištenje replikacije visoke dostupnosti.

Dodatne informacije potražite u članku [Pokretanje Elasticsearch na Azure][elasticsearch-azure].

**Dijagnostika**. Pomoću alata za nadzor za Elasticsearch ili prijavite sve pogreške na klijentskoj strani pomoću opseg. U odjeljku "Praćenja" u [Pokrenut Elasticsearch na Azure][elasticsearch-azure].


### <a name="writing-data-to-elasticsearch-fails"></a>Pisanjem podataka ne uspije Elasticsearch.

**Otkrivanje**. Uhvatite odgovarajuće iznimke za određeni [klijent Elasticsearch] [ elasticsearch-client] koristi.  

**Oporavak**

- Koristite mehanizam pokušajte ponovno. Svaki klijent ima vlastitu pravila Ponovi. 
 
- Ako aplikacija možete tolerate razinu smanjene dosljednost, razmislite o pisanje s `write_consistency` postavljanje od `quorum`.

Dodatne informacije potražite u članku [Pokretanje Elasticsearch na Azure][elasticsearch-azure].

**Dijagnostika**. Pomoću alata za nadzor za Elasticsearch ili prijavite sve pogreške na klijentskoj strani pomoću opseg. U odjeljku "Praćenja" u [Pokrenut Elasticsearch na Azure][elasticsearch-azure].


## <a name="queue-storage"></a>Red čekanja za pohranu

### <a name="writing-a-message-to-azure-queue-storage-fails-consistently"></a>Dosljedno pisanje poruke ne uspije Azure red za pohranu.

**Otkrivanje**. Kada je *N* ponovnim pokušajima, operacija pisanja i dalje ne uspijeva.

**Oporavak**

- Spremanje podataka u lokalnom predmemorije, a proslijediti na upisivanje prostora za pohranu kasnije, kada servis postane dostupna.
- Stvorite red čekanja za sekundarne i pisati u taj red ako primarni reda čekanja nije dostupan.

**Dijagnostika**. Koristite [za pohranu metrika][storage-metrics].


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Aplikacija ne može obraditi određenu poruku iz reda. 

**Otkrivanje**. Aplikacija određene. Na primjer, poruka sadrži podatke koji nisu valjani ili poslovne logike neće uspjeti zbog nekog razloga. 

**Oporavak**

Da biste premjestili poruku u zasebnom red. Pokrenite drugi proces da pregledate poruke u taj red.

Razmislite o korištenju redove Bus Azure servis za razmjenu poruka, koja omogućuje [reda čekanja reagira slovo] [ sb-dead-letter-queue] funkcionalnost u tu svrhu.

Napomena: Ako koristite za pohranu redova WebJobs, WebJobs SDK pruža rukovanje ugrađene poison porukama. Informirajte [se o korištenju Azure reda čekanja za pohranu s WebJobs SDK][sb-poison-message].

**Dijagnostika**. Korištenje aplikacije zapisivanje.


## <a name="redis-cache"></a>Redis predmemorije

### <a name="reading-from-the-cache-fails"></a>Čitanje iz predmemorije neće uspjeti.

**Otkrivanje**. Uhvatite `StackExchange.Redis.RedisConnectionException`.

**Oporavak**

1. Ponovno na tranzitne pogreške. Azure Redis predmemorije podržava ugrađene pokušaj putem potražite u članku [smjernice pokušaj Redis predmemorije][redis-retry].
2. Pogreške koje nisu prolaznim Smatraj neuspješno pronalaženje u predmemoriji i smanjenja izvornih podataka.

**Dijagnostika**. Pomoću [dijagnostike Redis predmemorije][redis-monitor].


### <a name="writing-to-the-cache-fails"></a>Pisanje u predmemoriju neće uspjeti.

**Otkrivanje**. Uhvatite `StackExchange.Redis.RedisConnectionException`.

**Oporavak**

1. Ponovno na tranzitne pogreške. Azure Redis predmemorije podržava ugrađene pokušaj putem potražite u članku [smjernice pokušaj Redis predmemorije][redis-retry].
2. Ako je pogreške koje nisu prolaznim, zanemarite je i pričekajte druge transakcije pisati u predmemoriju kasnije.

**Dijagnostika**. Pomoću [dijagnostike Redis predmemorije][redis-monitor].


## <a name="sql-database"></a>SQL baze podataka

### <a name="cannot-connect-to-the-database-in-the-primary-region"></a>Ne mogu se povezati s bazom podataka u području primarni.

**Otkrivanje**. Prekida se veza.

**Oporavak**

Preduvjeta: Baze podataka mora biti konfigurirana za aktivni zemlj replikaciju. Potražite u članku [SQL replikacije baze podataka Active zemlj. -][sql-db-replication].

- Za upite, pročitajte iz sekundarnog replike. 
- Umeće i ažuriranja, ručno uspjeti putem sekundarnog replike. Potražite u članku [pokretanje planiranog ili neplanirano prebacivanje za baze podataka SQL Azure][sql-db-failover].

Replike koristi niza za povezivanje različitih bit će potrebno da biste ažurirali niz za povezivanje u aplikaciji.


### <a name="client-runs-out-of-connections-in-the-connection-pool"></a>Klijentski se pokreće iz veze u vezu.

**Otkrivanje**. Uhvatite `System.InvalidOperationException` pogreške. 

**Oporavak**

- Ponovite postupak.
- Kao plan ublažiti izdvajanja grupe veze za svakom slučaju koristi da bi se jednom slučaju korištenja ne dominate sve veze.
- Povećanje grupe najveći dopušteni broj povezivanja.

**Dijagnostika**. Aplikacija zapisnika.


### <a name="database-connection-limit-is-reached"></a>Zbirka dostigne ograničenje vezu baze podataka. 

**Otkrivanje**. Baze podataka SQL Azure ograničava broj zaposlenici zaduženi za istodobni, prijave i sesije. Ograničenja ovise o sloju servisa. Dodatne informacije potražite u članku [ograničenja resursa za baze podataka SQL Azure][sql-db-limits].

Da biste otkrili te pogreške, Uhvatite `System.Data.SqlClient.SqlException` i provjerite vrijednost `SqlException.Number` za SQL kod pogreške. Popis šifri odgovarajući pogrešaka potražite u članku [šifre pogreške SQL baze podataka SQL klijentske aplikacije: baze podataka pogreška veze i ostalih problema][sql-db-errors].

**Oporavak**. Te pogreške smatraju tranzitne, pa ponovni može riješiti problem. Ako kliknete dosljedno te pogreške, preporučujemo da skaliranje bazu podataka.

**Dijagnostika**. - [Sys.event_log] [ sys.event_log] upit vraća veze uspješno baze podataka, neuspjeha veze i deadlocks.

- Stvaranje [upozorenja pravilo] [ azure-alerts] za nije uspjela veze.

- Omogućite [nadzor baze podataka SQL] [ sql-db-audit] i provjeri nije uspjelo prijave.


## <a name="service-bus-messaging"></a>Servis Bus poruka

### <a name="reading-a-message-from-a-service-bus-queue-fails"></a>Čitanja poruke iz servisa Bus reda neće uspjeti.

**Otkrivanje**. Uhvatite iznimke putem klijentskog programa SDK. Osnovni klasa za servis Bus iznimke je [MessagingException][sb-messagingexception-class]. Ako je pogreška tranzitne, u `IsTransient` svojstvo je true. 

Dodatne informacije potražite u odjeljku [iznimke za razmjenu poruka servisa Bus][sb-messaging-exceptions].

**Oporavak**

1. Ponovno na tranzitne pogreške. U odjeljku [Servis Bus ponovite smjernice][sb-retry].

2. Poruke koje nije moguće isporučiti sve tekstnog okvira spremaju se u *red reagira pismo*. Pomoću ovog reda čekanja da biste vidjeli poruka koje nije moguće primiti. Postoji bez automatskog čišćenje reda čekanja reagira pismo. Poruke ostaju dok ih izričito učitati. Potražite u članku [Bus za pregled usluge reagira slovo redova][sb-dead-letter-queue].


### <a name="writing-a-message-to-a-service-bus-queue-fails"></a>Pisanje poruke na servis Bus reda čekanja neće uspjeti.

**Otkrivanje**. Uhvatite iznimke putem klijentskog programa SDK. Osnovni klasa za servis Bus iznimke je [MessagingException][sb-messagingexception-class]. Ako je pogreška tranzitne, u `IsTransient` svojstvo je true. 

Dodatne informacije potražite u odjeljku [iznimke za razmjenu poruka servisa Bus][sb-messaging-exceptions].

**Oporavak**

1. Klijent servisa Bus automatski ponovnih pokušaja nakon tranzitne pogrešaka. Po zadanom koristi eksponencijalne natrag slobodno. Nakon maksimalan broj pokušaja broj ili maksimalne isteklo razdoblje klijentu throws iznimku. Dodatne informacije potražite u članku [Servis Bus ponovite smjernice][sb-retry].

2. Ako se prekorači kvotu za red čekanja, klijent throws [QuotaExceededException][QuotaExceededException]. Poruka iznimke daje više detalja. Praznite neke poruke iz reda čekanja provesti pa preporučujemo korištenje uzorka za prepoznavanje elektronička da biste izbjegli nastavak ponovne pokušaje dok je premašena kvote. Osim toga, provjerite je li [BrokeredMessage.TimeToLive] svojstvo nije postavljeno je prevelika. 

3. Unutar područja, otpornost mogu poboljšati pomoću [particioniranom redova ili teme][sb-partition]. Koje nisu particije reda čekanja ili temu dodijeliti jedan razmjenu trgovine. Ako je ovo razmjenu spremište nije dostupan, sve operacije na tom reda čekanja ili tema neće uspjeti. Particioniranom reda čekanja ili temu particije preko više poruka trgovine. 

4. Za dodatne otpornost stvorite dva servisa knjižna se prostori naziva u različitim područjima i replicirati poruke. Koristite active replikacije ili pasivni replikacije.

    - Aktivni replikacije: klijent šalje svaku poruku u oba redove. Primatelja očekuje podatke i redova. Označavanje poruke s Jedinstveni identifikator radi klijenta možete odbaciti duplicirane poruke.

    - Pasivne replikacije: klijent šalje poruku jedan red. Ako je došlo do pogreške, klijent pada natrag na red. Primatelja očekuje podatke i redova. Taj se način smanjuje broj dvostruke poruke koje se šalju. Međutim, primatelja i dalje morate ručicu duplicirane poruke.

    Dodatne informacije potražite u članku [GeoReplication uzorka] [ sb-georeplication-sample] i [najbolje prakse za insulating aplikacije izvoditi na servis Bus kvarove i disasters][sb-outages].


### <a name="duplicate-message"></a>Dupliciranje poruke.

**Otkrivanje**. Pregledajte na `MessageId` i `DeliveryCount` svojstva poruke.

**Oporavak**

- Ako je to moguće, Dizajnirajte poruku obradu operacije biti idempotent. U suprotnom pohranjivati poruke ID-ova poruka koje već obrađen, a zatim potvrdite ID prije obrade poruke.

- Omogućivanje duplikata, kao što su stvaranje reda čekanja s `RequiresDuplicateDetection` postavljen na true. Ova postavka servisa Bus automatski briše sve poruke koje se šalju s istim `MessageId` kao prethodne poruke.  Imajte na umu sljedeće:

    -  Ta postavka onemogućuje se staviti u redu čekanja duplicirane poruke. Ga neće spriječiti u više puta obrade ista poruka.

    - Duplikata ima prozor vremena. Ako je duplikat poslali izvan prozor, neće se provjeravati.

**Dijagnostika**. Prijavite se dupliciranu poruke.


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Aplikacija ne može obraditi određenu poruku iz reda. 

**Otkrivanje**. Aplikacija određene. Na primjer, poruka sadrži podatke koji nisu valjani ili poslovne logike neće uspjeti zbog nekog razloga. 

**Oporavak**

Postoje dva načina neuspjeh uzeti u obzir. 

- Primatelja pronalazi pogrešku. U ovom slučaju premjestili poruku red reagira pismo. Kasnije, pokrenite drugi proces da pregledate poruke u redu čekanja reagira pismo.

- Primatelja ne uspije tijekom obrade poruku &mdash; , na primjer, zbog neobrađenu iznimku. Rukovanje taj slučaj, koristite `PeekLock` načinu rada. U tom načinu ako zaključavanje istekne, poruku postaje dostupna drugih primatelja. Ako poruka premašuje maksimalno isporuke broji ili na vrijeme važenja, poruka automatski se premješta red reagira pismo.

Dodatne informacije potražite u članku [Bus za pregled usluge reagira slovo redova][sb-dead-letter-queue].

**Dijagnostika**. Kada aplikacija premješta poruke red reagira pismo, pisati događaja zapisnike aplikacije.


## <a name="service-fabric"></a>Tkanina servisa

### <a name="a-request-to-a-service-fails"></a>Zahtjev za uslugu neće uspjeti.

**Otkrivanje**. Servis vraća pogrešku.

**Oporavak**

- Pronađite proxy ponovno (`ServiceProxy` ili `ActorProxy`) i nazovite metodu servisa/glumca ponovno. 

- **S praćenjem stanja servisa**. Da biste prelomili operacije pouzdanog zbirke u transakcije. Ako je došlo do pogreške, transakcije bit će vraćen. Zahtjev, ako se povlače iz reda, obrađuju ponovno. 
 
- **Bez praćenja stanja servisa**. Ako servis nastavi podataka vanjskih Store, sve operacije moraju biti idempotent.


**Dijagnostika**. Zapisnik aplikacije

### <a name="service-fabric-node-is-shut-down"></a>Servis tkanina čvor isključiti.

**Otkrivanje**. Otkazivanje token se prenosi u servisu `RunAsync` način. Servis tkanina otkazuje zadatak prije isključivanja čvor.

**Oporavak**. Da biste otkrili zatvaranja pomoću token otkazivanja. Kad servisa tkanina zatraži otkazivanja, završite napravili i izlazak iz njega `RunAsync` najbrže što može.

**Dijagnostika**. Zapisnici aplikacije


## <a name="storage"></a>Prostor za pohranu

### <a name="writing-data-to-azure-storage-fails"></a>Pisanje podataka ne uspije Azure prostor za pohranu

**Otkrivanje**. Klijent prima pogreške prilikom pisanja.

**Oporavak**

1. Ponovite postupak da biste oporavili od tranzitne pogrešaka. [Ponovite pravila] [ Storage.RetryPolicies] u klijentu SDK ovime rukuje automatski.
2. Implementacija uzorak elektronička prepoznavanje da biste izbjegli izvršilo prostora za pohranu.
3. Ako je N pokušaj pokušaja ne uspije, izvršiti graceful pričuvne. Ako, na primjer:

    - Spremanje podataka u lokalnom predmemorije, a proslijediti na upisivanje prostora za pohranu kasnije, kada servis postane dostupna.
    - Ako je akcija za pisanje u transakcijskih opseg, vaše transakcije.

**Dijagnostika**. Koristite [za pohranu metrika][storage-metrics].


### <a name="reading-data-from-azure-storage-fails"></a>Čitanje podataka iz spremišta Azure neće uspjeti.

**Otkrivanje**. Klijent prima pogreške prilikom čitanja.

**Oporavak**

1. Ponovite postupak da biste oporavili od tranzitne pogrešaka. [Ponovite pravila] [ Storage.RetryPolicies] u klijentu SDK ovime rukuje automatski.
2. Za pohranu RA GRS čitanja iz primarnog krajnjoj točki ne uspije, pokušajte čitanja iz sekundarne krajnjoj točki. Klijent SDK možete riješiti automatski. [Replikacija Azure prostor za pohranu]potražite u članku[storage-replication].
3. Ako je *N* pokušaj pokušaja ne uspije, potrajati pričuvni akciju slabije bez poteškoća. Na primjer, ako je proizvod slike se ne može dohvatiti iz spremišta pokazati generički zamjensku sliku.

**Dijagnostika**. Koristite [za pohranu metrika][storage-metrics].


## <a name="virtual-machine"></a>Virtualnog računala

### <a name="connection-to-a-backend-vm-fails"></a>Vezu s pozadinskom VM neće uspjeti.

**Otkrivanje**. Mrežni pogreške pri povezivanju.

**Oporavak**

- Implementacija najmanje dva pozadinskog VMs u skupu dostupnost iza raspoređivača opterećenja.

- Ako je pogreška veze tranzitne, ponekad TCP uspješno vratit će slanja poruke. 

- Implementacija pravila Ponovi u aplikaciji. 

- Stalni ili nisu prolaznim pogrešaka implementirati [elektronička prepoznavanje] [ circuit-breaker] uzorka.

- Ako pozivanja VM premašuje ograničenje izlazne mreže, reda izlaznih poruka popuniti. Ako dosljedno pun reda izlaznih poruka, preporučujemo da skaliranje odgovor. 

**Dijagnostika**. Zapisnik događaja na servis ograničenja.

### <a name="vm-instance-becomes-unavailable-or-unhealthy"></a>VM instance postane nedostupan ili dobro.

**Otkrivanje**. Konfiguriranje opterećenja [probni stanja] [ lb-probe] koji signale je li instancu VM dobar. Na probni provjerite je li ključnih funkcije odgovarate pravilno. 

**Oporavak**. Za svaki razina aplikacije postavili više instanci VM u isti skup dostupnosti i postavite opterećenja ispred na VMs. Ako probni stanja ne uspije, raspoređivača opterećenja zaustavlja slanje nove veze dobro instance.

**Dijagnostika**. – Koristite opterećenja [Prijava analitiku][lb-monitor].
- Konfiguriranje nadzora sustava sve stanja krajnje točke za nadzor nadzirati.


### <a name="operator-accidentally-shuts-down-a-vm"></a>Operator slučajno isključi na VM.

**Otkrivanje**. N/D

**Oporavak**. Postavljanje lock resursa s `ReadOnly` razinu. Potražite [Lock resursima pomoću upravitelja za Azure resursa][rm-locks].

**Dijagnostika**. Korištenje [zapisnike Azure aktivnosti][azure-activity-logs].


## <a name="webjobs"></a>WebJobs

### <a name="continuous-job-stops-running-when-the-scm-host-is-idle"></a>Neprekinuti posao će se prestati izvoditi kada IO glavno računalo je neaktivno.

**Otkrivanje**. Prenesite otkazivanja token funkciju WebJob. Dodatne informacije potražite u članku [Graceful zatvaranja][web-jobs-shutdown]. 

**Oporavak**. Omogućivanje na `Always On` u web-aplikaciji. Dodatne informacije potražite u članku [pokretanje pozadinske zadatke s WebJobs][web-jobs].


## <a name="application-design"></a>Dizajniranje aplikacije

### <a name="application-cant-handle-a-spike-in-incoming-requests"></a>Aplikacija ne prepoznaje šiljka u zahtjevi za razgovore.

**Otkrivanje**. Ovisi o aplikaciji. Uobičajeni simptomi:

- Na web-mjestu pokreće vraćanje šifre pogrešaka 5xx HTTP-a.
- Zavisne servisa, kao što su baze podataka ili prostor za pohranu, početi throttle zahtjeva. Potražite pogreške HTTP kao što je 429 HTTP (previše mnogo zahtjeve), ovisno o servis.
- Duljina reda čekanja HTTP poveća.

**Oporavak**

- Promjena veličine izgleda za rukovanje Poboljšana učitavanja. 

- Smanjivanje pogrešaka da biste izbjegli kaskadnih neuspjeha poremetiti cijelu aplikaciju. Strategije ublažiti obuhvaćaju sljedeće:

    - Implementacija [Ograničavanje uzorak] [ throttling-pattern] da biste izbjegli izvršilo pozadinskog sustava.
    - Korištenje [utemeljen na red učitavanja ujednačavanje] [ queue-based-load-leveling] međuspremnika zahtjeve i obraditi odgovarajuće brzinom.
    - Određivanje prioriteta određene klijente. Na primjer, ako aplikacija je besplatna i plaćenu razine, throttle klijenata besplatne sloju, ali ne plaćenu klijentima. U odjeljku [prioritet reda čekanja uzorak][priority-queue-pattern].

**Dijagnostika**. Korištenje [aplikacije servisa za dijagnostičkih zapisivanje][app-service-logging]. Pomoću servisa kao što su [Azure prijava analitiku][azure-log-analytics], [Aplikacija uvida][app-insights], ili [Novi Relic] [ new-relic] da biste pomogao shvatiti dijagnostičke zapisnike.


### <a name="one-of-the-operations-in-a-workflow-or-distributed-transaction-fails"></a>Jedno od operacije u tijeku rada ili raspodijeljeno transakcije neće uspjeti.

**Otkrivanje**. Kada je *N* ponovnim pokušajima, ona i dalje ne uspijeva.

**Oporavak**

- Kao plan ublažiti implementirati [Raspored Agent nadzornik] [ scheduler-agent-supervisor] uzorka za upravljanje cijeli tijek rada. 
- Nemojte ponovno vremenska ograničenja. Postoji niskog uspješnosti za tu pogrešku. 
- Red čekanja posla, da bi se pokušajte ponovno kasnije.

**Dijagnostika**. Prijavite se sve operacije (uspješnih i neuspješnih), uključujući Kompenziranje akcije. Koristite korelacije ID-ove, tako da možete pratiti sve operacije u istom transakcijom.


### <a name="a-call-to-a-remote-service-fails"></a>Poziv na servis za daljinski neće uspjeti.

**Otkrivanje**. Šifra pogreške HTTP-a.

**Oporavak**

1. Ponovno na tranzitne pogreške. 
2. Ako poziv prekida se nakon što se pokušava *N* , pričuvni akcija. (Aplikacija određene.)
3. Implementirati [elektronička prepoznavanje uzorak] [ circuit-breaker] da biste izbjegli kaskadnih pogreške. 

**Dijagnostika**. Prijavite se sve pogreške na udaljenom poziv.


## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o postupku FMA potražite u članku [Resilience dizajnom za servise u oblaku] [ resilience-by-design-pdf] (Preuzimanje PDF-a).

<!-- links -->

[api-management]: https://azure.microsoft.com/documentation/services/api-management/
[api-management-throttling]: ../api-management/api-management-sample-flexible-throttling.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-web-apps]: ../application-insights/app-insights-azure-web-apps.md
[app-service-configure]: ../app-service-web/web-sites-configure.md
[app-service-logging]: ../app-service-web/web-sites-enable-diagnostic-log.md
[app-service-slots]: ../app-service-web/web-sites-staged-publishing.md
[auto-rest-client-retry]: https://github.com/Azure/autorest/blob/master/Documentation/clients-retry.md
[azure-activity-logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[azure-alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md
[azure-log-analytics]: ../log-analytics/log-analytics-overview.md
[BrokeredMessage.TimeToLive]: https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
[cassandra-error-handling]: http://www.datastax.com/dev/blog/cassandra-error-handling-done-right
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[docdb-multi-region]: ../documentdb/documentdb-developing-with-multiple-regions.md
[elasticsearch-azure]: guidance-elasticsearch-running-on-azure.md
[elasticsearch-client]: https://www.elastic.co/guide/en/elasticsearch/client/index.html
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[onstop-events]: https://azure.microsoft.com/blog/the-right-way-to-handle-azure-onstop-events/
[lb-monitor]: ../load-balancer/load-balancer-monitor-log.md
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md#learn-about-the-types-of-probes
[new-relic]: https://newrelic.com/
[priority-queue-pattern]: https://msdn.microsoft.com/library/dn589794.aspx
[queue-based-load-leveling]: https://msdn.microsoft.com/library/dn589783.aspx
[QuotaExceededException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx
[ra-web-apps-basic]: guidance-web-apps-basic.md
[redis-monitor]: ../redis-cache/cache-how-to-monitor.md
[redis-retry]: ../best-practices-retry-service-specific.md#cache-redis-retry-guidelines
[resilience-by-design-pdf]: http://download.microsoft.com/download/D/8/C/D8C599A4-4E8A-49BF-80EE-FE35F49B914D/Resilience_by_Design_for_Cloud_Services_White_Paper.pdf
[RoleEntryPoint.OnStop]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[RoleEnvironment.Stopping]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx
[rm-locks]: ../resource-group-lock-resources.md
[sb-dead-letter-queue]: ../service-bus-messaging/service-bus-dead-letter-queues.md
[sb-georeplication-sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/GeoReplication
[sb-messagingexception-class]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
[sb-messaging-exceptions]: ../service-bus-messaging/service-bus-messaging-exceptions.md
[sb-outages]: ../service-bus/service-bus-outages-disasters.md#protecting-queues-and-topics-against-datacenter-outages-or-disasters
[sb-partition]: ../service-bus-messaging/service-bus-partitioning.md
[sb-poison-message]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#poison
[sb-retry]: ../best-practices-retry-service-specific.md#service-bus-retry-guidelines
[search-sdk]: https://msdn.microsoft.com/library/dn951165.aspx
[scheduler-agent-supervisor]: https://msdn.microsoft.com/library/dn589780.aspx
[search-analytics]: ../search/search-traffic-analytics.md
[sql-db-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-db-errors]: ../sql-database/sql-database-develop-error-messages.md#resource-governanance-errors
[sql-db-failover]: ../sql-database/sql-database-geo-replication-failover-portal.md
[sql-db-limits]: ../sql-database/sql-database-resource-limits.md
[sql-db-replication]: ../sql-database/sql-database-geo-replication-overview.md
[storage-metrics]: https://msdn.microsoft.com/library/dn782843.aspx
[storage-replication]: ../storage/storage-redundancy.md
[Storage.RetryPolicies]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.aspx
[sys.event_log]: https://msdn.microsoft.com/library/dn270018.aspx
[throttling-pattern]: https://msdn.microsoft.com/library/dn589798.aspx
[web-jobs]: ../app-service-web/web-sites-create-web-jobs.md
[web-jobs-shutdown]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful

