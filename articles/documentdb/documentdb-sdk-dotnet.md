<properties 
    pageTitle="DocumentDB .NET API & SDK | Microsoft Azure" 
    description="Saznajte više o .NET API i SDK uključujući datumi izdanja, umirovljenje datume i promjene između svake verzije DocumentDB .NET SDK." 
    services="documentdb" 
    documentationCenter=".net" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>API-ji DocumentDB i SDK-ovi 

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [OSTALE](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-net-api-and-sdk"></a>DocumentDB .NET API i SDK

<table>
<tr><td>**Preuzimanje SDK**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**Dokumentacija API-JA**</td><td>[.NET API referentnu dokumentaciju](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**Uzorci**</td><td>[Primjere koda .NET](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**Početak rada**</td><td>[Početak rada s DocumentDB .NET SDK](documentdb-get-started.md)</td></tr>
<tr><td>**Web-aplikacije vodič**</td><td>[Razvoj aplikacija za web s DocumentDB](documentdb-dotnet-application.md)</td></tr>
<tr><td>**Trenutno podržanih framework**</td><td>[Microsoft .NET Framework 4,5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Napomene

> [AZURE.IMPORTANT] Počevši od verzije 1.9.2 izdanje, možda ćete primiti System.NotSupportedException kada ispitivanje particioniranom zbirke. Da biste izbjegli ovu pogrešku, provjerite je li proces glavnog računala 64-bitni. Za izvršna projekte, to možete učiniti poništavanjem mogućnost "Želite sami 32-bitni" u prozoru svojstva projekt na kartici Sastavi.

### <a name="a-name11001100httpswwwnugetorgpackagesmicrosoftazuredocumentdb1100"></a><a name="1.10.0"/>[1.10.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.10.0)

  - Dodane izravno povezivanje podrška za particioniranom zbirke.
  - Bolje performanse dosljednost razine Bounded Staleness.
  - Dodane poligona i vrste podataka LineString prilikom određivanja zbirke indeksiranje pravilnika o zemlj fencing prostorno upita.
  - LINQ podrška za StringEnumConverter, IsoDateTimeConverter i UnixDateTimeConverter tijekom prevođenja predikati.
  - Različite SDK popravci programskih pogrešaka.

### <a name="a-name195195httpswwwnugetorgpackagesmicrosoftazuredocumentdb195"></a><a name="1.9.5"/>[1.9.5](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - Fiksna problem koji se zbog sljedećih NotFoundException: čitanje sesija nije dostupna za unos sesiju token. U ovom došlo je do iznimke u nekim slučajevima kada slanje upita za čitanje područje zemlj distributed računa.
  - Koji prikazuje svojstva ResponseStream u predmete ResourceResponse koja omogućuje izravan pristup podlozi strujanje iz odgovor.

### <a name="a-name194194httpswwwnugetorgpackagesmicrosoftazuredocumentdb194"></a><a name="1.9.4"/>[1.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - Izmijenio klase ResourceResponse, FeedResponse, StoredProcedureResponse i MediaResponse implementaciju odgovarajuće javno sučelja tako da se mocked za testiranje utemeljenima na implementacije (TDD).
  - Riješiti problem koji se zbog oblikovan particija ključa zaglavlja prilikom korištenja prilagođeni objekt JsonSerializerSettings Serijalizacija podataka.

### <a name="a-name193193httpswwwnugetorgpackagesmicrosoftazuredocumentdb193"></a><a name="1.9.3"/>[1.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - Fiksna problema koje su uzrokovale dugo izvodi upita nije uspjela zbog pogreške: trenutno ovlaštenja nije valjan.
  - Riješiti problem koji se ukloniti izvorni SqlParameterCollection s više particija gore/pojedinih upita.

### <a name="a-name192192httpswwwnugetorgpackagesmicrosoftazuredocumentdb192"></a><a name="1.9.2"/>[1.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - Podrška za paralelne upite za particioniranom zbirke.
  - Podrška za više particija ORDER BY i VRHA upite za particioniranom zbirke.
  - Fiksni nedostaju reference na DocumentDB.Spatial.Sql.dll i Microsoft.Azure.Documents.ServiceInterop.dll koji su potrebni kada se pozivate DocumentDB projekta s referencom na DocumentDB Nuget paket.
  - Fiksni mogućnost korištenja parametara različitih vrsta prilikom korištenja korisnički definiranih funkcija u LINQ. 
  - Ispraviti pogrešku globalno repliciranu računa gdje Upsert pozive u tijeku preusmjereni da biste pročitali mjesta umjesto mjesta za pisanje.
  - Dodane metode IDocumentClient sučelje koje nedostaju: 
      - Način UpsertAttachmentAsync koji traje mediaStream i mogućnosti kao parametri
      - Način CreateAttachmentAsync koji traje mogućnosti kao parametar
      - Način CreateOfferQuery koja vas vodi querySpec kao parametar.
  - O nezapečaćenim javno klase prikazat će se u sučelju IDocumentClient.

### <a name="a-name180180httpswwwnugetorgpackagesmicrosoftazuredocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - Dodaje podršku za više područja baze podataka računa.
  - Podrška za pokušaja ograničeni zahtjevi.  Korisnik može prilagođavati broj ponovne pokušaje i vrijeme čekanja Maks konfiguriranjem svojstvo ConnectionPolicy.RetryOptions.
  - Dodaje novo sučelje IDocumentClient koji definira potpisa svih DocumenClient svojstva i metode.  Kao dio ta promjena, mijenjaju i nastavak metode koje stvorite IQueryable i IOrderedQueryable načinima na klase DocumentClient sam.
  - Dodaje mogućnost konfiguracije da biste postavili ServicePoint.ConnectionLimit za dani DocumentDB krajnje Uri.  Da biste promijenili zadanu vrijednost koja je postavljeno na 50 pomoću ConnectionPolicy.MaxConnectionLimit.
  - Zastarjeli IPartitionResolver i njegova implementacija.  Podrška za IPartitionResolver zastario. Preporučuje se korištenje particije zbirke za više prostora za pohranu i propusnost.


### <a name="a-name171171httpswwwnugetorgpackagesmicrosoftazuredocumentdb171"></a><a name="1.7.1"/>[1.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - Dodaje se preopterećenja za Uri temelji ExecuteStoredProcedureAsync način koji vodi RequestOptions kao parametar.
  
### <a name="a-name170170httpswwwnugetorgpackagesmicrosoftazuredocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - Dodane vrijeme live (TTL) podršci za dokumente.

### <a name="a-name163163httpswwwnugetorgpackagesmicrosoftazuredocumentdb163"></a><a name="1.6.3"/>[1.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - Ispraviti pogrešku u pakiranju Nuget od .NET SDK za pakiranje kao dio sustava rješenja Azure u Oblaku.
  
### <a name="a-name162162httpswwwnugetorgpackagesmicrosoftazuredocumentdb162"></a><a name="1.6.2"/>[1.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - Implementirani [particioniranom zbirke](documentdb-partition-data.md) i [korisnički definirana performanse razine](documentdb-performance-levels.md). 

### <a name="a-name153153httpswwwnugetorgpackagesmicrosoftazuredocumentdb153"></a><a name="1.5.3"/>[1.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[Fixed]** Slanje upita throws DocumentDB krajnje točke: "System.Net.Http.HttpRequestException: pogreška tijekom kopiranja sadržaja u strujanje.

### <a name="a-name152152httpswwwnugetorgpackagesmicrosoftazuredocumentdb152"></a><a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - Prošireno LINQ podržava uključujući novi operatori za paging, uvjetnih izraza i Usporedba u rasponu.
    - Iskoristite operator da biste omogućili odaberite VRH u LINQ
    - Operator CompareTo da biste omogućili usporedbama niza raspona
    - Uvjetno (?) i spajanje operatori (?)
  - **[Fixed]** ArgumentOutOfRangeException prilikom kombiniranja projekcije Model s gdje u u upitu linq.  [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151httpswwwnugetorgpackagesmicrosoftazuredocumentdb151"></a><a name="1.5.1"/>[1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[Fixed]** Ako odaberite nije posljednji izraz davatelja LINQ pretpostavlja da nema projekcije i proizvodi odaberite * neispravno.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150httpswwwnugetorgpackagesmicrosoftazuredocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - Implementirani Upsert, dodali UpsertXXXAsync metode
 - Poboljšanja performansi za sve zahtjeve
 - Spajanje LINQ davatelja usluge podrške za uvjetno, i CompareTo metode nizova
 - **[Fixed]** Davatelj LINQ--> implementacija sadrži metodu na popisu da biste generirali isti SQL kao IEnumerable i polja
 - **[Fixed]** BackoffRetryUtility ponovno koristi iste HttpRequestMessage umjesto stvaranja novog na pokušaj
 - **[Zastarjeli]** --> UriFactory.CreateCollection sada trebali biste koristiti UriFactory.CreateDocumentCollection
 
### <a name="a-name141141httpswwwnugetorgpackagesmicrosoftazuredocumentdb141"></a><a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[Fixed]** Lokalizacija problemi prilikom korištenja koji nisu en kulture informacije kao što su na servisu Servisu itd. 
 
### <a name="a-name140140httpswwwnugetorgpackagesmicrosoftazuredocumentdb140"></a><a name="1.4.0"/>[1.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - Usmjeravanje temelji ID-a
    - Novi UriFactory preglednika radi jednostavnijeg izgradnje ID temelji resursa veze
    - Novi overloads na DocumentClient snimanja u URI-JA
  - Dodane IsValid() i IsValidDetailed() u LINQ za geoprostornim
  - Poboljšana podrška za LINQ davatelja
    - Atan **matematičke** - Asin Abs, a zatim na Acos, Ceiling Cos, Tan Exp, Floor, zapisnika, Log10, Pow, Round, znak, Sin, Sqrt, Izrežite
    - **Niz** - slika, sadrži, EndsWith, IndexOf, broj, ToLower, TrimStart, zamijeniti, Zrcalili, TrimEnd, StartsWith, podniz, ToUpper
    - **Polja** - slika, sadrži, Count
    - Operator **u**

### <a name="a-name130130httpswwwnugetorgpackagesmicrosoftazuredocumentdb130"></a><a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - Podrška za izmjenu indeksiranja pravila
    - Nova metoda ReplaceDocumentCollectionAsync u DocumentClient
    - Novo svojstvo IndexTransformationProgress u ResourceResponse<T> za praćenje postotka tijeka promjene pravila indeksa
    - Sada je mutable DocumentCollection.IndexingPolicy
  - Podrška za prostorno indeksiranja i upita
    - Novi prostor naziva Microsoft.Azure.Documents.Spatial za prostorno vrste Serijalizacija/prekidanja serije kao što su točke i poligona
    - Novi SpatialIndex klase za indeksiranje GeoJSON podatke pohranjene u DocumentDB
  - **[Fiksno]** : netočan SQL upit generirao linq izraza [#38](https://github.com/Azure/azure-documentdb-net/issues/38)

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Zavisnost o Newtonsoft.Json v5.0.7 
- Promjene za podršku Order By
  - LINQ davatelja usluge podrške za OrderBy() ili OrderByDescending()
  - IndexingPolicy za podršku Order By 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- Podrška za particija podataka pomoću novog klase HashPartitionResolver i RangePartitionResolver i na IPartitionResolver
- DataContract serijalizacije
- Podrška za GUID u LINQ davatelja
- Podrška za UDF u LINQ

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- GA SDK

> [AZURE.NOTE]
Došlo je promjena naziva NuGet paketa između pretpregled i ZŽ. Ne možemo premjestiti iz **Microsoft.Azure.Documents.Client** **Microsoft.Azure.DocumentDB**
<br/>


### <a name="a-name09x-preview09x-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentsclient"></a><a name="0.9.x-preview"/>[0.9.x-Preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- SDK-ovi pretpregled [zastarjeli]

## <a name="release--retirement-dates"></a>Izdanje i njegovo povlačenje iz upotrebe datuma
Microsoft će dati obavijesti barem **12 mjeseci** prije povlačenje programa SDK da bi se glačanje prijelaz na verziju novija/podržane.

Nove značajke i funkcije i optimizacije samo dodaju se u trenutni SDK, kao što su preporučuje se uvijek nadogradnju na najnoviju SDK verziju ranijeg kao što je moguće. 

Bilo koji zahtjev za DocumentDB pomoću otpisa SDK će odbio servis.

> [AZURE.WARNING]
Sve verzije SDK DocumentDB Azure za .NET prije verzije **1.0.0** će biti povučena na **veljača 29, 2016**. 
 
<br/>
 
| Verzija | Datum izdanja | Datum njegovo povlačenje iz upotrebe 
| ---     | ---          | ---
| [1.10.0](#1.10.0) | Rujan 27, 2016 |---
| [1.9.5](#1.9.5) | Rujan 01, 2016 |---
| [1.9.4](#1.9.4) | Kolovoz 24, 2016 |---
| [1.9.3](#1.9.3) | Kolovoz 15, 2016 |---
| [1.9.2](#1.9.2) | Srpanj 23, 2016 |---
| 1.9.1 | Zastarjelo |---
| 1.9.0 | Zastarjelo |---
| [1.8.0](#1.8.0) | Lipnja 14, 2016 |---
| [1.7.1](#1.7.1) | Možda 06, 2016 |---
| [1.7.0](#1.7.0) | Travanj 26, 2016 |---
| [1.6.3](#1.6.3) | Travanj 08, 2016 |---
| [1.6.2](#1.6.2) | Ožujak 29, 2016 |---
| [1.5.3](#1.5.3) | Veljača 19, 2016 |---
| [1.5.2](#1.5.2) | 14. prosinac 2015. |---
| [1.5.1](#1.5.1) | 23. studenom 2015. |---
| [1.5.0](#1.5.0) | Listopad 05, 2015. |---
| [1.4.1](#1.4.1) | Kolovoz 25, 2015. |---
| [1.4.0](#1.4.0) | Kolovoz 13, 2015. |---
| [1.3.0](#1.3.0) | Kolovoz 05, 2015. |---
| [1.2.0](#1.2.0) | Srpanj 06, 2015. |---
| [1.1.0](#1.1.0) | 30. Travanj 2015. |---
| [1.0.0](#1.0.0) | 08. Travanj 2015. |---
| [0.9.3-prelease](#0.9.x-preview) | 12. ožujak 2015. | Veljača 29, 2016
| [0.9.2-prelease](#0.9.x-preview) | Siječnju 2015. | Veljača 29, 2016
| [.9.1-prelease](#0.9.x-preview) | 13. listopad 2014. | Veljača 29, 2016
| [0.9.0-prelease](#0.9.x-preview) | 21. kolovoz 2014. | Veljača 29, 2016

## <a name="faq"></a>NAJČEŠĆA PITANJA
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Vidi također

Da biste saznali više o DocumentDB, pogledajte stranice servisa [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) . 
