<properties 
    pageTitle="API DocumentDB Python & SDK | Microsoft Azure" 
    description="Saznajte više o Python API i SDK uključujući datumi izdanja, umirovljenje datume i promjene između svake verzije DocumentDB Python SDK." 
    services="documentdb" 
    documentationCenter="python" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>API-ji DocumentDB i SDK-ovi

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [OSTALE](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-python-api-and-sdk"></a>API DocumentDB Python i SDK

<table>
<tr><td>**Preuzmite SDK**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**Dokumentacija API-JA**</td><td>[Python API referentnu dokumentaciju](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>
<tr><td>**Upute za instalaciju SDK**</td><td>[Upute za instalaciju Python SDK](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Doprinos SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Početak rada**</td><td>[Početak rada s Python SDK](documentdb-python-application.md)</td></tr>
<tr><td>**Trenutni podržane platforme**</td><td>[Python 2.7](https://www.python.org/downloads/) i [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Napomene

### <a name="a-name200200httpspypipythonorgpypipydocumentdb200"></a><a name="2.0.0"/>[2.0.0](https://pypi.python.org/pypi/pydocumentdb/2.0.0)
- Podrška za Python 3.5.
- Podrška za okupljanje zahtjeva za modul.
- Podrška za sesiju dosljednost.
- Podrška za gore/ORDERBY upite za particioniranom zbirke.


### <a name="a-name190190httpspypipythonorgpypipydocumentdb190"></a><a name="1.9.0"/>[1.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- Podrška pravila Ponovi dodane ograničeni zahtjeva za. (Ograničeni zahtjevi za primanje na zahtjev stopa prevelika iznimku, kod pogreške 429.) Prema zadanim postavkama DocumentDB ponovnih pokušaja devet vremena za svaki zahtjev za kada naiđe kod pogreške 429 honoring retryAfter vremena u zaglavlje odgovor. Interval jedan fixed pokušaj sada moguće je postaviti kao dio svojstvo RetryOptions na objektu ConnectionPolicy ako želite zanemariti retryAfter vrijeme između na ponovne pokušaje koju vraća poslužitelj. DocumentDB sada čekanje najviše 30 sekundi zatražiti da je u tijeku ograničio vrijeme (bez obzira je pokušaj broj) i vraća odgovor kod pogreške 429. Ovaj put može biti nadjačati u svojstvu RetryOptions na ConnectionPolicy objektu.

- DocumentDB sada vraća x-ms-ograničenja-Ponovi-count i x-ms-throttle-retry-wait-time-ms kao zaglavlja odgovor na svaki zahtjev za označavanje Reguliranje ponovite count i vrijeme cummulative zahtjev waited između na ponovne pokušaje.

- Uklanja klase RetryPolicy i odgovarajuće svojstvo (retry_policy) koji se prikazuje na klase document_client, a umjesto uvodi RetryOptions predmete će se svojstva RetryOptions ConnectionPolicy predmete koje je moguće koristiti da biste nadjačali neke od zadanih mogućnosti pokušajte ponovno.

### <a name="a-name180180httpspypipythonorgpypipydocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - Dodaje podršku za više područja baze podataka računa.

### <a name="a-name170170httpspypipythonorgpypipydocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- Dodaje podrška za značajku vremena da biste Live(TTL) za dokumente.

### <a name="a-name161161httpspypipythonorgpypipydocumentdb161"></a><a name="1.6.1"/>[1.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- Popravci programskih pogrešaka koji se odnose na poslužitelju strani particija dopustili posebne znakove u parametru path partitionkey.

### <a name="a-name160160httpspypipythonorgpypipydocumentdb160"></a><a name="1.6.0"/>[1.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- Implementirani [particioniranom zbirke](documentdb-partition-data.md) i [korisnički definirana performanse razine](documentdb-performance-levels.md). 

### <a name="a-name150150httpspypipythonorgpypipydocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- Dodavanje raspršivanje & raspon particija resolvers radi jednostavnijeg sharding aplikacije preko više particija.

### <a name="a-name142142httpspypipythonorgpypipydocumentdb142"></a><a name="1.4.2"/>[1.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Implementirati Upsert. Novi načini za UpsertXXX dodali podržava značajku Upsert.
- Implementacija ID temelji usmjeravanja. Nema javno promjena API-JA, internog sve promjene.

### <a name="a-name120120httpspypipythonorgpypipydocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- Podržava geoprostornim indeks.
- Provjeri valjanost svojstvo id za sve resurse. ID-a za resurse ne smije sadržavati ?, /, #, \, znakove ili na kraju razmakom.
- Dodaje novi zaglavlje "indeks transformaciju tijeku" ResourceResponse.

### <a name="a-name110110httpspypipythonorgpypipydocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- Implementira V2 indeksiranja pravila.

### <a name="a-name101101httpspypipythonorgpypipydocumentdb101"></a><a name="1.0.1"/>[1.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Podržava proxy vezu.

### <a name="a-name100100httpspypipythonorgpypipydocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- GA SDK.

## <a name="release--retirement-dates"></a>Datum izdanja i njegovo povlačenje iz upotrebe
Microsoft će dati obavijesti barem **12 mjeseci** prije povlačenje programa SDK da bi se glačanje prijelaz na verziju novijom/podržane.

Nove značajke i funkcije i optimizacije samo dodaju se u trenutni SDK, kao što je je preporučujemo da uvijek nadogradnju na najnoviju SDK verziju ranijeg kao što je moguće. 

Bilo koji zahtjev za DocumentDB pomoću otpisa SDK će odbio servis.

> [AZURE.WARNING]
Sve verzije Azure DocumentDB SDK Python prije verzije **1.0.0** će biti povučena na **veljača 29, 2016**. 

<br/>

| Verzija | Datum izdanja | Datum njegovo povlačenje iz upotrebe 
| ---     | ---          | ---
| [2.0.0](#2.0.0) | Rujan 29, 2016 |---
| [1.9.0](#1.9.0) | Srpanj 07, 2016 |---
| [1.8.0](#1.8.0) | Lipnja 14, 2016 |---
| [1.7.0](#1.7.0) | Travanj 26, 2016 |---
| [1.6.1](#1.6.1) | Travanj 08, 2016 |---
| [1.6.0](#1.6.0) | Ožujak 29, 2016 |---
| [1.5.0](#1.5.0) | Siječanj 03, 2016 |---
| [1.4.2](#1.4.2) | Listopad 06, 2015. |---
| [1.4.1](#1.4.1) | Listopad 06, 2015. |---
| [1.2.0](#1.2.0) | Kolovoz 06, 2015. |---
| [1.1.0](#1.1.0) | 09. srpanj 2015. |---
| [1.0.1](#1.0.1) | 25. svibanj 2015. |---
| [1.0.0](#1.0.0) | 07. Travanj 2015. |---
| 0.9.4-prelease | 14. siječnju 2015. | Veljača 29, 2016
| 0.9.3-prelease | Prosinac 09, 2014. | Veljača 29, 2016
| 0.9.2-prelease | 25. studenom 2014. | Veljača 29, 2016
| 0.9.1-prelease | 23. rujan 2014. | Veljača 29, 2016
| 0.9.0-prelease | 21. kolovoz 2014. | Veljača 29, 2016

## <a name="faq"></a>NAJČEŠĆA PITANJA
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Vidi također

Da biste saznali više o DocumentDB, pogledajte stranice servisa [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) . 
