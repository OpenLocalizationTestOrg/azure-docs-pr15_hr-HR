<properties
    pageTitle="Što je pretraživanje Azure | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Azure pretraživanje je servis za pretraživanje u potpunosti upravlja glavnom računalu oblaka. Dodatne informacije u ovom pregled značajki."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="what-is-azure-search"></a>Što je pretraživanje Azure?

Azure pretraživanje je rješenje oblaka-kao-na-servisa za pretraživanje koje Delegati poslužitelj i infrastrukture upravljanje Microsoftu, ostavite ste sa servisom spremni za korištenje koje možete popuniti podataka i zatim dodajte pomoću pretraživanja weba ili mobilnu aplikaciju. Azure pretraživanja omogućuje jednostavno robusne pretraživanju dodati aplikacije pomoću jednostavne [REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) ili [.NET SDK](search-howto-dotnet-sdk.md) bez upravljanje infrastruktura za pretraživanje ili postanete stručnjak pretraživanja.

## <a name="give-your-users-a-powerful-search-experience"></a>Dajte korisnicima sučelje za napredno pretraživanje

**Napredna upite** možete formulated pomoću [sintaksa za jednostavne upite](https://msdn.microsoft.com/library/azure/dn798920.aspx), koji nudi logički operatori, operatori za pretraživanje izraz, operatori sufiks, prednost operatora. Uz to, [Lucene sintaksu upita](https://msdn.microsoft.com/library/azure/mt589323.aspx) možete omogućiti mutno pretraživanja, Blizina pretraživanje, povećanje termina i regularne izraze. Azure pretraživanja podržava i prilagođene Leksički analyzers da biste omogućili aplikacije za rukovanje složene upite pretraživanja pomoću fonetski koji se podudaraju i regularne izraze.

**Jezična podrška** nije [uključena za 56 različite jezike](https://msdn.microsoft.com/library/azure/dn879793.aspx). Pomoću Lucene analyzers i Microsoft analyzers (suženo prema kriteriju godine prirodnim jezikom obrade u sustavima Office i Bing), Azure pretraživanja možete analizirati teksta u okvir za pretraživanje vaše aplikacije energija učiniti lingvistička jezično specifične uključujući glagolska, spol, nepravilne množini imenice (npr. "miša" nasuprot "miševi"), word poništite ukamaćivanja, prelamanja riječi (za jezike bez razmaka) i više.

**Prijedloge za pretraživanje** može biti omogućen za autocompleted trake za pretraživanje, a dovršena prije vrsta upita. [Stvarni dokumenata u indeks predlažemo](https://msdn.microsoft.com/library/azure/dn798936.aspx) kao korisnici unesite djelomičnog pretraživanja za unos.

**Kliknite isticanje** korisnicima [omogućuje](https://msdn.microsoft.com/library/azure/dn798927.aspx) da vide isječak teksta u svakom rezultata koji sadrži adrese za svoje upit. Koje možete odabrati koja se polja vratili istaknuti isječci.

**Slojevito navigacije** jednostavno dodati na stranicu rezultata pretraživanja pomoću pretraživanja Azure. Korištenje [samo jedan upit parametra](https://msdn.microsoft.com/library/azure/dn798927.aspx), Azure pretraživanje dat će sve potrebne informacije za sastavljanje slojevito pretraživanju u korisničkom Sučelju aplikacije programa da biste korisnicima omogućili kroz razine naniže i filtrirati rezultate pretraživanja (npr. filtar stavki kataloga raspon cijena ili marke).

Podrška za **prostorno zemlj** omogućuje energija postupka, filtriranje i prikaz zemljopisna mjesta. Azure pretraživanja omogućuje korisnicima Istraživanje podataka na temelju Blizina rezultata pretraživanja na određeno mjesto ili na temelju određenog regiji. U ovom se videozapisu objašnjava kako to funkcionira: [9 kanala: Azure pretraživanja i geoprostornih podataka](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

**Filtri** koje se mogu koristiti ugraditi slojevito navigacije u korisničkom Sučelju vaše aplikacije, poboljšati formulation upita, a filtar koji se temelji na korisnički ili za razvojne inženjere-definiran kriterije. Stvaranje naprednih filtara pomoću [OData sintakse](https://msdn.microsoft.com/library/azure/dn798921.aspx).

## <a name="empower-your-developers-with-an-easy-to-use-service"></a>Učine vaš inženjerima sa servisom za jednostavan upotrebu

**Visoke dostupnosti** osigurava iskustvo iznimno pouzdanog pretraživanje servisa. Kada skalirana ispravno, [Azure pretraživanje nudi 99.9% SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

**Potpuno upravlja** kao rješenje završetka do kraja, Azure pretraživanje potreban je apsolutno bez upravljanja infrastrukture. Na servisu mogu se jednostavno prilagoditi vašim potrebama po skaliranje u dvije dimenzije učiniti dodatnog prostora za pohranu dokumenata, veća opterećenje upita ili oboje.

**Integracija podataka** pomoću [indexers](https://msdn.microsoft.com/library/azure/dn946891.aspx) omogućuje Azure pretraživanja radi indeksiranja automatski baze podataka SQL Azure, Azure DocumentDB ili [spremište blobova platforme Azure](search-howto-indexing-azure-blob-storage.md) da biste sinkronizirali indeks pretraživanja sadržaja s trgovinom u primarni podataka.

**Dokument Zvuk pucketanja** je dostupno (trenutno u pretpregledu) [za čitanje i indeksirati glavne formata](search-howto-indexing-azure-blob-storage.md) uključujući Microsoft Office, kao i PDF-a i HTML dokumenata.

**Pretraživanje promet analytics** su [koji se prikupljaju i analizirati](search-traffic-analytics.md) da biste otključali uvida s što korisnici upisuju u okvir za pretraživanje.

**Jednostavan bilježenje rezultata** je ključna pogodnost Azure pretraživanja. [Bilježenje rezultata profila](https://msdn.microsoft.com/library/azure/dn798928.aspx) koriste omogućuju tvrtkama i ustanovama važnosti modela kao funkcija vrijednosti u dokumentima sami. Na primjer, možda želite novija proizvodi ili diskontno proizvode pojavljuju Visoko u rezultatima pretraživanja. Možete sastaviti i bilježenja rezultata profila koji se koriste oznake za personalizirane bilježenje rezultata na temelju preferencama pretraživanja klijenta ste prati, a zatim sprema zasebno.

**Sortiranje** je koja se koristila za više polja putem shemi indeks, a zatim preklapa upita vrijeme s jednim pretraživanjem parametra.

**Označavanje stranica** i ograničavanje rezultata pretraživanja je [uz precizno definirati kontrole](search-pagination-page-layout.md) koje nudi Azure pretraživanje iznad rezultata pretraživanja.  

**Search explorer** omogućuje problem upite odabiranja sve svoje indeksi desno od Azure portal za vaš račun da biste mogli jednostavno testiranje upita i sužavanje bilježenja rezultata profila.

## <a name="how-it-works"></a>Kako funkcionira

### <a name="1-provision-service"></a>1. servis za dodjeljivanje
Možete Okretni gore servis za pretraživanje Azure pomoću [Portala za Azure](https://portal.azure.com/) ili [Azure resursa upravljanje API -JA](https://msdn.microsoft.com/library/azure/dn832684.aspx).

Ovisno o tome kako konfigurirati servis za pretraživanje, koristit ćete ili besplatne sloj servis koji se zajednički koriste s drugim pretplatnike Azure pretraživanja ili na [plaćenu sloju](https://azure.microsoft.com/pricing/details/search/) koji dedicates resursa koji će se koristiti samo na servisu. Prilikom dodjele resursa na servisu, i odaberite područje podatkovnog centra u kojem se nalazi na servisu.

Ovisno o tome koje sloju servisa odaberete možete skaliranje uslugu u dvije dimenzije: 1) dodavanje replike radi povećanja kapacitet opterećenje podebljanom upita, a 2) dodati particije dodati prostora za pohranu za više dokumenata. Obradom dokumenta za pohranu i upit propusnost zasebno, možete prilagoditi servisa za pretraživanje za vašim potrebama.

### <a name="2-create-index"></a>2. stvaranje indeksa
Sadržaj možete prenijeti u funkcioniranju servisa Azure pretraživanja, najprije morate definirati indeks pretraživanja Azure. Indeks je poput bazu podataka koja sadrži podatke, a možete prihvatiti upita za pretraživanje. Definiranje shemi indeks za mapiranje strukturu dokumente koje želite pretraživati, slično kao polja u bazi podataka.

Shema tih indeksa ili moguće stvoriti na portalu za Azure ili programski [pomoću .NET SDK](search-howto-dotnet-sdk.md) ili [REST API -JA](https://msdn.microsoft.com/library/azure/dn798941.aspx). Kada je definiran indeks, pa možete prenijeti podatke servisa za pretraživanje Azure gdje se naknadno indeksirati.

### <a name="3-index-data"></a>3. Podaci indeksa
Kada definirate polja i atribute indeksa, spremni ste za prenijeti sadržaj u indeks. Možete koristiti bilo automatske ili povlačite model za prijenos podataka u indeks.

Model istaknuti se odvija pomoću indexers koji se može konfigurirati za na zahtjev ili zakazana ažuriranja (pogledajte [Indeksiranje operacije (Azure pretraživanje servisa REST API -JA)](https://msdn.microsoft.com/library/azure/dn946891.aspx)), koja vam omogućuje jednostavno ingest podataka i promjene podataka iz Azure DocumentDB, baze podataka SQL Azure, spremište blobova platforme Azure ili SQL Server smješten u VM programa Azure.

Model automatske se odvija pomoću SDK ili REST API-ji za slanje ažuriranih dokumenata na indeksa. Podatke možete automatske s gotovo bilo kojeg dataset korištenje JSON OSNOVNI oblik. Potražite u članku [Dodavanje, ažuriranje, ili brisanje dokumenata](https://msdn.microsoft.com/library/azure/dn798930.aspx) ili [kako koristiti .NET SDK)](search-howto-dotnet-sdk.md) upute o učitavanja podataka.

### <a name="4-search"></a>4. pretraživanje
Kada ste popunjeno indeksu pretraživanja Azure, možete odmah [problem upita za pretraživanje](https://msdn.microsoft.com/library/azure/dn798927.aspx) servisa krajnjoj točki pomoću jednostavne HTTP zahtjeva REST API-JA ili .NET SDK.

## <a name="try-it-now-for-free"></a>Isprobajte (besplatno!)
Azure pretraživanja možete pokušati danas! Ako već imate račun za Azure, možete ga [Dodjela resursa za uslugu u besplatne sloju](search-create-service-portal.md).

Ako nemate račun Azure pokušate sesije slobodni, 60 minuta bez registrirajte potrebna. Idite na [Pokušajte aplikacije servisa Azure](http://go.microsoft.com/fwlink/p/?LinkId=618214) i odaberite "Web App." Zatim odaberite željeni predložak "ASP.NET + Azure pretraživanja" za početak rada.
