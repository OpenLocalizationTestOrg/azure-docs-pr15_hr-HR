<properties 
    pageTitle="Azure pretraživanje performanse i optimizirati pitanja vezana uz | Microsoft Azure" 
    description="Precizno podesite performanse Azure pretraživanja i konfiguriranje najbolju skala" 
    services="search" 
    documentationCenter="" 
    authors="LiamCavanagh" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="liamca"/>

# <a name="azure-search-performance-and-optimization-considerations"></a>Azure pretraživanje performanse i optimizirati pitanja vezana uz

Sjajan pretraživanju je ključ da biste uspješno za mnogo mobile i web-aplikacije. Iz Nekretnine, koristiti marketplaces automobila na mreži katalozi servisa fast search i relevantnih rezultata utjecat će na sučelje za klijenta. Ovaj dokument namijenjen za pomoć otkrijte najbolje prakse za iskoristite Azure pretraživanja, posebice za napredne scenarije sofisticirane preduvjetima za skalabilnost, višejezična podrška ili prilagođeni rangiranja.  Osim toga, ovaj dokument navode internals i pokriva pristupa učinkovito funkcionirati u aplikacijama za klijenta stvarnog života.

## <a name="performance-and-scale-tuning-for-search-services"></a>Performanse i ugađanje mjerilo za usluge pretraživanja

Ne možemo sve koriste se za tražilice kao što su Bing i Google i visoke performanse ponude.  Zbog toga kada korisnici pretraživanja s omogućenim web ili mobilnu aplikaciju, očekuju će iste karakteristike performansi.  Kada optimizacija za performanse pretraživanja, jedna od najbolje postupke je fokus na Latencija koji je vrijeme potrebno upita da biste dovršili i rezultate.  Kada optimizacija za Latencija pretraživanja važno je:

1. Odaberite na ciljnom Latencija (ili maksimalnu količinu vremena) kojima se traže uobičajeni pretraživanje traje da biste dovršili.

2. Stvaranje i testiranje realni radno opterećenje protiv servisa za pretraživanje s realističan skup podataka za mjerenje te Latencija stope.

3. Započnite s malim brojem upita u sekundi (QPS) i nastaviti da biste povećali broj izvršava test dok Latencija upita ispod Latencija definirani cilj.  To je važno usporednih da biste lakše isplanirali mjerilo kao što je aplikacija rastom u odjeljku korištenje.

4. Kad god je moguće ponovno koristiti HTTP veze.  Ako koristite SDK za .NET Azure pretraživanja, to znači da trebali biste koristiti instanci ili instancu [SearchIndexClient](https://msdn.microsoft.com/library/azure/microsoft.azure.search.searchindexclient.aspx) , a ako koristite REST API-JA, trebali biste koristiti jedan HttpClient.
 
Prilikom stvaranja ovo testiranje radnih opterećenja, postoje neke značajke pretraživanja Azure treba imati na umu:

1. Moguće je slanje tako mnogo pretraživanja odjednom, upiti da će overwhelmed dostupni na servisu Azure pretraživanje resursi.  Kada se to dogodi, vidjet ćete šifre HTTP 503 odgovor.  Zbog toga, preporučuje se da biste započeli s raznim rasponima zahtjeva za pretraživanje da biste vidjeli razlike u stope Latencija prilikom dodavanja više zahtjeva za pretraživanje.

2. Prijenos sadržaja radi pretraživanja Azure će utjecati ukupne performanse i Latencija servisa Azure pretraživanja.  Ako namjeravate poslati podatke dok korisnici izvršavate pretraživanja, važno je da bi ovaj radno opterećenje račun u testiranja.

3. Ne svaki upit za pretraživanje će izvesti na iste razine performansi.  Ako, na primjer, se dokumenta web-mjesto pretraživanja ili u okvir za pretraživanje prijedlog će obično brže nego upit s značajan broj pozornici i filtri izvesti.  Preporučuje se da bi različite upita očekujete da biste vidjeli u obzir prilikom stvaranje testiranja.  

4. Varijacije zahtjeva za pretraživanje važno je jer ako neprestano izvršavanje iste zahtjeva za pretraživanje, predmemoriranja podataka će se pokrenuti da biste performanse izgledati bolje nego što je možda pomoću više raznovrsne upita postavljen.

> [AZURE.NOTE] [Vizualni učitavanja Studio testiranje](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) zaista izvrstan je način za izvođenje usporednih testiranja kao omogućuje izvršavanje HTTP zahtjeva kao što je koji su vam potrebni za izvođenje upite odabiranja Azure pretraživanja i omogućuje parallelization zahtjeva.

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Skaliranje Azure traženje stope visoke upita i ograničio vrijeme zahtjeva

Kada primate previše ograničeni zahtjeve ili biti dulji od ciljne tečajeve za kašnjenje u iz programa učitavanja povećana upita, možete pretraživati da biste smanjili Latencija stope na jedan od dva načina:

1. **Povećati replike:**  Kopiju je kao kopiju podataka dopuštanja Azure pretraživanja da biste učitali zahtjeva protiv većeg broja primjeraka.  Sve uravnoteženje opterećenja i replikacije podataka preko replike upravlja Azure pretraživanja i možete promijeniti broj replike dodijeliti servisa u bilo kojem trenutku.  Može dodijeliti do 12 replike na servis za pretraživanje te 3 replike na servis za osnovno pretraživanje.  Replike može se prilagoditi ili s [Portala za Azure](search-create-service-portal.md) pomoću [pretraživanja Azure upravljanja API-JA](search-get-started-management-api.md).

2. **Povećati sloju pretraživanja:**  Azure pretraživanja dolazi u [broj razine](https://azure.microsoft.com/pricing/details/search/) i svaki od tih razine nudi različite razine performansi.  U nekim slučajevima može imati toliko upita sloju ste ne može dati potpuno niske latencije stope, čak i kad replike su Dostignut je maksimum.  U ovom slučaju trebali uzeti u obzir jedan od više razine pretraživanje kao što su sloju S3 Azure pretraživanje koje je dobro prikladniji scenarijima s velikim brojem dokumenata i radnih opterećenja iznimno visoke upita za korištenje.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Promjena veličine Azure traži sporo pojedinačne upite

Drugog razloga zašto može biti sporo stope kašnjenje je iz jedne predugo traje da biste dovršili upita.  U ovom slučaju dodavanje replike će poboljšati Latencija stope.  U ovom slučaju postoje dvije mogućnosti dostupne:

1. **Povećavanje particije** Particija je mehanizam za podjele podataka preko dodatne resurse.  Zbog toga kada dodate drugi particije podataka dobiti podijelite dva.  Treći particija dijeli indeks tri, itd.  To utječe da u nekim slučajevima sporo upiti će raditi brže zbog parallelization izračuni.  Postoji nekoliko primjera koju ste vidjeti ovaj parallelization iznimno dobro funkcioniraju s upita koji imaju manje selectivity upita.  Sastoji se od upita koji zadovoljavaju više dokumenata ili kada faceting treba navesti broji preko velikog broja dokumenata.  Budući da postoji mnogo izračunavanje potrebno da biste rezultat prema važnosti dokumenata ili za brojanje broja dokumenata, dodavanje dodatnih particije može pridonijeti pružaju dodatni izračuni.  

   Može biti najviše 12 particije na servis za pretraživanje i 1 particije u servis za osnovno pretraživanje.  Particije može se prilagoditi ili s [Portala za Azure](search-create-service-portal.md) pomoću [pretraživanja Azure upravljanja API-JA](search-get-started-management-api.md).

2. **Ograničiti visoke Cardinality polja:** Polje visoke cardinality sastoji se od facetable ili koje je moguće filtrirati polje koje sadrži značajan broj jedinstvene vrijednosti i zbog toga traje mnogo resursa za izračunavanje rezultata putem.   Na primjer, postavljanje polje ID proizvoda ili opis kao facetable filtrirati bi za visoke cardinality jer je jedinstvena su za većinu vrijednosti iz dokumenta u dokument. Kad god je to moguće, ograničite broj visoke cardinality polja.

3. **Povećati sloju pretraživanja:**  Premještanje do veći sloju Azure pretraživanje može biti drugi način da biste poboljšali performanse spore upita.  Svaki veći sloju omogućuje brže procesora i memorije koji mogu imati pozitivne utjecaj na performanse upita.

## <a name="scaling-for-availability"></a>Promjena veličine dostupnost

Replike ne samo pomažu smanjenju Latencija upita, ali možete omogućiti za visoke dostupnosti.  S jednom replike, možete očekivati periodičku nedostupnost zbog ponovna poslužitelja nakon softverska ažuriranja ili drugih održavanja događaja koji će se pojaviti.  Kao rezultat je Imajte na umu da ako vaše aplikacije potreban je visoke dostupnosti pretraživanja (upite) te zapisivanje (indeksiranja događaja).  Azure pretraživanje nudi SLA mogućnosti na sve ponude plaćenu pretraživanje sa sljedećim atributima:

- 2 replike za visoke dostupnosti radnih opterećenja samo za čitanje (upite)
- 3 ili više replike za visoke dostupnosti radnih opterećenja čitanje i pisanje (upita i indeksiranja)

Dodatne informacije o tome potražite [Ugovor o razini servisa za pretraživanje Azure](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Budući da replike su kopije vaših podataka, imate više replike omogućuje Azure pretraživanje tako da na računalu ponovna i održavanje na temelju jednog replike trenutku istovremeni upita da biste nastavili želite izvršiti u odnosu na druge replike.  Zbog toga će morate uzeti u obzir kako ovaj nedostupnost mogu utjecati na upite koji morate izvršiti protiv manje jednu kopiju podataka.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Promjena veličine zemlj distributed radnih opterećenja i osigurati redundanciju za zemlj.

Za radnih opterećenja zemlj distributed vidjet ćete da će korisnici nalazi far from podatkovnog centra gdje se nalazi na servisu Azure pretraživanje imati veći Latencija stope.  Zbog toga često važno je da bi većem broju servisa za pretraživanje u područja koje se nalaze u bliže Blizina tim korisnicima.  Azure pretraživanje ne trenutno nudi automatiziranog način zemlj replikaciju indeksi pretraživanja Azure preko područja, no postoje neke tehnike koje je moguće koristiti koje možete lakše postupak implementacije i upravljanja. Te su strukturirane u sljedećim odjeljcima nekoliko.

Cilj zemlj raspodijeliti skup usluge pretraživanja je da bi indeksi dva ili više dostupnih u dva ili više područja koje korisnik će biti proslijeđene servisa Azure pretraživanja koji vam omogućuje najniže kašnjenje u ovom primjeru:

   ![Unakrsno – kartica servisa po regijama][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Čuvanje podataka u sinkronizaciji preko većem broju servisa Azure pretraživanja

Postoje dvije mogućnosti čuvanja sinkronizacija raspodijeljeno pretraživanje servisa koji se sastoje od ili pomoću [Komponente za indeksiranje pretraživanja Azure](search-indexer-overview.md) ili API automatske (naziva [Azure pretraživanje REST API -JA](https://msdn.microsoft.com/library/dn798935.aspx)).  

### <a name="azure-search-indexers"></a>Indexers Azure pretraživanja

Ako koristite program za indeksiranje pretraživanja Azure, već uvozite promjene podataka iz središnje datastore kao što su baze podataka SQL Azure ili DocumentDB. Kada stvorite novu pretraživanje servisa, koje jednostavno i stvoriti nove indeksiranje pretraživanja Azure za servis koja upućuje na ovom isti datastore. Na taj način, kad god novih promjena isporučuju u spremištu podataka oni će zatim indeksirati prema različitim Indexers.  

Evo primjera kako taj arhitektura izgledati.

   ![Jedan izvor podataka s raspodijeljeno indeksiranje i kombinacije servisa][2]


### <a name="push-api"></a>Automatske API-JA 
Ako koristite na Azure automatske API pretraživanja da biste [ažurirali sadržaju indeksa pretraživanja Azure](https://msdn.microsoft.com/library/dn798930.aspx), možete zadržati različite servise za pretraživanje u sinkronizaciji pritiskom promjene na sve usluge pretraživanja kada je potrebno ažuriranje.  Kada to učinite važno je da biste provjerili je li za rukovanje slučajevima gdje ažuriranje servis za pretraživanje jedan ne uspije, a slijedi jedno ili više ažuriranja.

## <a name="leveraging-azure-traffic-manager"></a>Korištenje upravitelja Azure promet

[Upravitelj promet Azure](../traffic-manager/traffic-manager-overview.md) omogućuje usmjeravanje zahtjevi za više zemlj nalazi web-mjesta koji se zatim sigurnosno po većem broju servisa Azure pretraživanja.  Jedna prednosti upravitelja promet jest koje možete isprobati Azure pretraživanja da biste bili sigurni da je dostupan i usmjeravanje korisnika u uslugama zamjenski pretraživanja u slučaju isključiti.  Osim toga, ako su usmjeravanje zahtjeva za pretraživanje kroz Azure web-mjesta, Upravitelj promet Azure omogućuje morate učitati saldo slučajevima gdje je web-mjesto prema gore, ali ne Azure pretraživanja.  Evo primjera koje arhitektura koji upravlja upravitelj promet.

   ![Unakrsno – kartica servisa po regijama, sa središnje promet Manager][3]

## <a name="monitoring-performance"></a>Praćenje performansi

Azure pretraživanje nudi mogućnost za analizu i praćenje performansi servisa putem [Pretraživanja promet analize (STA)](search-traffic-analytics.md). Putem STA, možete po želji evidentirati operacije pojedinačne traženja, kao i Zbrojeno metriku na račun za Azure prostora za pohranu koji se zatim može obrađuju za analizu ili vizualizacije u dodatku Power BI.  Pomoću STA metriku, možete pregledati statistiku o izvedbi kao što su Prosječan broj upita ili puta odgovor na upit.  Uz to, zapisivanje operacija omogućuje dubinski analizirati detalje o operacije određeno pretraživanje.

STA koristan je alat da biste shvatili Latencija stope iz perspektive te Azure pretraživanja.  Budući da metriku performanse upita prijavljeni temelje se na vrijeme potrebno upit u potpunosti obraditi u pretraživanju Azure (iz vrijeme koje su potrebne da kada je poslati), vam se na tom se mogućnošću poslužite da bi se utvrdilo ako Latencija problemi sa strane servisa Azure pretraživanje ili izvan servisa, kao što su iz latenciju mreže.  

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o cijenama ograničenja razine i servise za svaki od njih potražite u članku [ograničenja servisa Azure pretraživanja](search-limits-quotas-capacity.md).

Potražite dodatne informacije o particija i replike kombinacije [Planiranje kapaciteta](search-capacity-planning.md) .

Za dodatne analiza na performanse i neke demonstracije kako implementirati optimizacije koji se spominju u ovom članku, pogledajte sljedeći videozapis:

> [AZURE.VIDEO azurecon-2015-azure-search-best-practices-for-web-and-mobile-applications]

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png