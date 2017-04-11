<properties
    pageTitle="API DocumentDB Node.js & SDK | Microsoft Azure"
    description="Saznajte više o Node.js API i SDK uključujući datumi izdanja, umirovljenje datume i promjene između svake verzije DocumentDB Node.js SDK."
    services="documentdb"
    documentationCenter="nodejs"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>API-ji DocumentDB i SDK-ovi

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [OSTALE](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

##<a name="documentdb-nodejs-api-and-sdk"></a>DocumentDB Node.js API-JA i SDK

<table>
<tr><td>**Preuzmite SDK**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**Dokumentacija API-JA**</td><td>[Dokumentacija referenca Node.js API-JA](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>
<tr><td>**Upute za instalaciju SDK**</td><td>[Upute za instalaciju](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**Doprinos SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**Uzorci**</td><td>[Primjere koda Node.js](documentdb-nodejs-samples.md)</td></tr>
<tr><td>**Početak rada vodiča**</td><td>[Početak rada s Node.js SDK](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Web-aplikacije vodič**</td><td>[Stvaranje web-aplikacije Node.js pomoću DocumentDB](documentdb-nodejs-application.md)</td></tr>
<tr><td>**Trenutni podržane platforme**</td><td>[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

##<a name="release-notes"></a>Napomene

###<a name="1.10.0"/>1.10.0</a>

- Podrška za više particija paralelno upita.
- Podrška za upite gore/ORDER BY za particioniranom zbirke.

###<a name="1.9.0"/>1.9.0</a>

- Podrška pravila Ponovi dodane ograničeni zahtjeva za. (Ograničeni zahtjevi za primanje na zahtjev stopa prevelika iznimku, kod pogreške 429.) Prema zadanim postavkama DocumentDB ponovnih pokušaja devet vremena za svaki zahtjev za kada naiđe kod pogreške 429 honoring retryAfter vremena u zaglavlje odgovor. Interval jedan fixed pokušaj sada moguće je postaviti kao dio svojstvo RetryOptions na objektu ConnectionPolicy ako želite zanemariti retryAfter vrijeme između na ponovne pokušaje koju vraća poslužitelj. DocumentDB sada čekanje najviše 30 sekundi zatražiti da je u tijeku ograničio vrijeme (bez obzira je pokušaj broj) i vraća odgovor kod pogreške 429. Ovaj put može biti nadjačati u svojstvu RetryOptions na ConnectionPolicy objektu.

- DocumentDB sada vraća x-ms-ograničenja-Ponovi-count i x-ms-throttle-retry-wait-time-ms kao zaglavlja odgovor na svaki zahtjev za označavanje Reguliranje ponovite count i vrijeme cummulative zahtjev waited između na ponovne pokušaje.

- Dodan je klasa RetryOptions će se svojstva RetryOptions ConnectionPolicy predmete koje je moguće koristiti da biste nadjačali neke od zadanih mogućnosti pokušajte ponovno.

###<a name="1.8.0"/>1.8.0</a>

 - Dodaje podršku za više područja baze podataka računa.

###<a name="1.7.0"/>1.7.0</a>

- Dodaje podrška za značajku vremena da biste Live(TTL) za dokumente.

###<a name="1.6.0"/>1.6.0</a>

- Implementirani [particioniranom zbirke](documentdb-partition-data.md) i [korisnički definirana performanse razine](documentdb-performance-levels.md).

###<a name="1.5.6"/>1.5.6</a>

- FIXED RangePartitionResolver.resolveForRead pogrešku gdje ga je ne daje veze zbog neispravni slika rezultata.

###<a name="1.5.5"/>1.5.5</a>

- Fiksna hashParitionResolver resolveForRead(): kada ključ ne particija navedena je prijavi iznimku, umjesto vraćanja popis svih veza registrirane.

###<a name="1.5.4"/>1.5.4</a>

- Ispravaka problema [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - namjenski Agent HTTPS: izbjegavanje izmjena globalni agent svrhe DocumentDB. Koristite namjenski agent za sve zahtjeve za biblioteke.

###<a name="1.5.3"/>1.5.3</a>

- Ispravaka problema [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - pravilno ručicu crtice media ID-ove dokumenata.

###<a name="1.5.2"/>1.5.2</a>

- Ispravaka problema [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - EventEmitter ga slušatelj osipanje upozorenje.

###<a name="1.5.1"/>1.5.1</a>

- Ispravaka problema [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - Preimenuj mapu predmemorije za Raspršivanje za sustave velika i mala slova.

### <a name="1.5.0"/>1.5.0</a>

- Implementacija sharding službe za podršku tako da dodate raspršivanje & raspon resolvers particije.

### <a name="1.4.0"/>1.4.0</a>

- Implementirati Upsert. Novi načini upsertXXX na documentClient.

### <a name="1.3.0"/>1.3.0</a>

- Da biste dodali brojeve verzija poravnanje s drugim SDK-ovi je preskočeno.

### <a name="1.2.2"/>1.2.2</a>

- Podjela pitanja promises omot novi spremištu.
- Ažurirajte do datoteke paketa za npm registra.

### <a name="1.2.1"/>1.2.1</a>

- Primjenjuje ID na temelju usmjeravanja.
- Rješava problem [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - trenutni je svojstvo u sukobu s current() način.

### <a name="1.2.0"/>1.2.0</a>

- Podrška za geoprostornim indeksa.
- Provjeri valjanost svojstvo id za sve resurse. ID-a za resurse ne smije sadržavati?, /, #, & #47; & #47; znakova ili na kraju razmakom.
- Dodaje novi zaglavlje "indeks transformaciju tijeku" ResourceResponse.

### <a name="1.1.0"/>1.1.0</a>

- Implementira V2 indeksiranja pravila.

### <a name="1.0.3"/>1.0.3</a>

- Problem [#40] (https://github.com/Azure/azure-documentdb-node/issues/40) – implementirana eslint i grunt konfiguracije u core i obećanje SDK.

### <a name="1.0.2"/>1.0.2</a>

- Problem [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - Promises omot Uključi zaglavlje uz poruku o pogrešci.

### <a name="1.0.1"/>1.0.1</a>

- Mogućnost upita za sukobe dodavanjem readConflicts, readConflictAsync i queryConflicts implementirani.
- Ažurirani dokumentaciju API-JA.
- Izdati [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync pogreške.

### <a name="1.0.0"/>1.0.0</a>

- GA SDK.

## <a name="release--retirement-dates"></a>Izdanje i njegovo povlačenje iz upotrebe datuma
Microsoft će dati obavijesti barem **12 mjeseci** prije povlačenje programa SDK da bi se glačanje prijelaz na verziju novijom/podržane.

Nove značajke i funkcije i optimizacije samo dodaju se u trenutni SDK, kao što je je preporučujemo da uvijek nadogradnju na najnoviju SDK verziju ranijeg kao što je moguće.

Bilo koji zahtjev za DocumentDB pomoću otpisa SDK će odbio servis.

> [AZURE.WARNING]
Sve verzije Azure DocumentDB SDK Node.js prije verzije **1.0.0** će biti povučena na **veljača 29, 2016**.

<br/>

| Verzija | Datum izdanja | Datum njegovo povlačenje iz upotrebe
| ---     | ---          | ---
| [1.10.0](#1.10.0) | Listopad 03, 2016 |---
| [1.9.0](#1.9.0) | Srpanj 07, 2016 |---
| [1.8.0](#1.8.0) | Lipnja 14, 2016 |---
| [1.7.0](#1.7.0) | Travanj 26, 2016 |---
| [1.6.0](#1.6.0) | Ožujak 29, 2016 |---
| [1.5.6](#1.5.6) | Ožujak 08, 2016 |---
| [1.5.5](#1.5.5) | Veljača 02, 2016 |---
| [1.5.4](#1.5.4) | Veljača 01, 2016 |---
| [1.5.2](#1.5.2) | Siječanj 26, 2016 |---
| [1.5.2](#1.5.2) | Siječanj 22, 2016 |---
| [1.5.1](#1.5.1) | Siječanj 4, 2016 |---
| [1.5.0](#1.5.0) | 31. prosinac 2015. |---
| [1.4.0](#1.4.0) | Listopad 06, 2015. |---
| [1.3.0](#1.3.0) | Listopad 06, 2015. |---
| [1.2.2](#1.2.2) | Rujan 10, 2015. |---
| [1.2.1](#1.2.1) | Kolovoz 15, 2015. |---
| [1.2.0](#1.2.0) | Kolovoz 05, 2015. |---
| [1.1.0](#1.1.0) | 09. srpanj 2015. |---
| [1.0.3](#1.0.3) | 04. lipnja 2015. |---
| [1.0.2](#1.0.2) | 23. svibanj 2015. |---
| [1.0.1](#1.0.1) | 15. svibanj 2015. |---
| [1.0.0](#1.0.0) | 08. Travanj 2015. |---
| 0.9.4-prerelease | 06. Travanj 2015. | Veljača 29, 2016
| 0.9.3-prerelease | 14. siječnju 2015. | Veljača 29, 2016
| 0.9.2-prerelease | 18. prosinac 2014. | Veljača 29, 2016
| 0.9.1-prerelease | 22. kolovoz 2014. | Veljača 29, 2016
| 0.9.0-prerelease | 21. kolovoz 2014. | Veljača 29, 2016


## <a name="faq"></a>NAJČEŠĆA PITANJA
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Vidi također

Da biste saznali više o DocumentDB, pogledajte stranice servisa [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) .
