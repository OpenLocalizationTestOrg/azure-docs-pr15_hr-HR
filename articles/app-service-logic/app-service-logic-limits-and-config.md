<properties
    pageTitle="Logika aplikacije ograničenja i konfiguraciji | Microsoft Azure"
    description="Pregled servisa ograničenja i dostupne za aplikacije logike konfiguracijskih vrijednosti."
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="logic-app-limits-and-configuration"></a>Logika aplikacije ograničenja i konfiguracija

U nastavku su informacije o trenutna ograničenja i konfiguracija detalja za Azure logike aplikacije.

## <a name="limits"></a>Ograničenja

### <a name="http-request-limits"></a>HTTP zahtjev ograničenja

To su ograničenja za jednu HTTP zahtjev i/ili poveznika poziva

#### <a name="timeout"></a>Prekoračenje vremena

|Ime|Ograničenje|Bilješke|
|----|----|----|
|Prekoračenje vremena zahtjeva|1 min|Programa [asinkrone uzorak](app-service-logic-create-api-app.md) ili [dok se ne Ponavljaj](app-service-logic-loops-and-scopes.md) vaše po potrebi|

#### <a name="message-size"></a>Veličina poruke

|Ime|Ograničenje|Bilješke|
|----|----|----|
|Veličina poruke|50 MB|Neke poveznika i API-možda ne podržava 50MB.  Pokretanje zahtjeva podržava do 25MB|
|Ograničenja za procjenu izraza|131,072 znakova|`@concat()`, `@base64()`, `string` ne smije biti dulji od ovog|

#### <a name="retry-policy"></a>Ponovite pravila

|Ime|Ograničenje|Bilješke|
|----|----|----|
|Ponovnim pokušajima|4|Možete konfigurirati s [ponovite parametar pravila](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Ponovite Maks odgode|1 sat|Možete konfigurirati s [ponovite parametar pravila](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Ponovite odgode min|20 min|Možete konfigurirati s [ponovite parametar pravila](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|

### <a name="run-duration-and-retention"></a>Pokretanje trajanje i zadržavanja

To su ograničenja za aplikaciju jedan logike pokrenuti.

|Ime|Ograničenje|Bilješke|
|----|----|----|
|Pokretanje trajanje|90 dana||
|Prostor za pohranu zadržavanja|90 dana|Ovo je od vremena početka izvođenja|
|Interval ponavljanja min|15 sec||
|Interval ponavljanja Max|500 dana||


### <a name="looping-and-debatching-limits"></a>Ponavljanje i debatching ograničenja

To su ograničenja za aplikaciju jedan logike pokrenuti.

|Ime|Ograničenje|Bilješke|
|----|----|----|
|ForEach stavke|5000|[Akcijski upit](../connectors/connectors-native-query.md) možete koristiti za filtriranje veće polja po potrebi|
|Do iteracija|10 000||
|SplitOn stavke|5000||
|ForEach Parallelism|20|Možete postaviti da uzastopnih foreach dodavanjem `"operationOptions": "Sequential"` da biste na `foreach` akcija|


### <a name="throughput-limits"></a>Ograničenja propusnost

To su ograničenja za instancu komponente jedan logike aplikacije. 

|Ime|Ograničenje|Bilješke|
|----|----|----|
|Okidača u sekundi|100|Možete raspodijeliti tijekovi rada preko više aplikacije po potrebi|

### <a name="definition-limits"></a>Definicija ograničenja

To su ograničenja za jednu logike definiciju aplikacije.

|Ime|Ograničenje|Bilješke|
|----|----|----|
|Akcije u ForEach|1|Ugniježđene tijekova rada da biste proširili po potrebi možete dodati|
|Akcije po tijeka rada|60|Ugniježđene tijekova rada da biste proširili po potrebi možete dodati|
|Dopuštene akcije gniježđenja dubine|5|Ugniježđene tijekova rada da biste proširili po potrebi možete dodati|
|Tokova po regijama po pretplati|1000||
|Okidača po tijeka rada|10||
|Max znakova po izraza|8,192||
|Max `trackedProperties` veličina u znakovima|16,000|
|`action`/`trigger`Ograničenje naziva|80||
|`description`ograničenje duljine|256||
|`parameters`ograničenje|50||
|`outputs`ograničenje|10||

## <a name="configuration"></a>Konfiguracija

### <a name="ip-address"></a>IP adresa

Pozive s [poveznik](../connectors/apis-list.md) će potjecati iz IP adresu navedeno u nastavku.

Pozive iz aplikacije logike izravno (odnosno putem [HTTP-a](../connectors/connectors-native-http.md) ili [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)) mogu potjecati iz bilo kojeg od [Azure podatkovnog centra IP raspone](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

|Područje logike aplikacije|Izlaznih IP|
|-----|----|
|Istok Australija|40.126.251.213|
|Australija Jugoistok|40.127.80.34|
|Južna Brazil|191.232.38.129|
|Središnje Indija|104.211.98.164|
|Središnje SAD-a|40.122.49.51|
|Istočnoazijski|23.99.116.181|
|Istočni SAD-a|191.237.41.52|
|Istočni sad 2|104.208.233.100|
|Istok Japan|40.115.186.96|
|Japan Zapad|40.74.130.77|
|Sjeverna središnje SAD-a|65.52.218.230|
|Sjeverna Europa|104.45.93.9|
|Južna središnje SAD-a|104.214.70.191|
|Jugoistočne Azije|13.76.231.68|
|Južna Indija|104.211.227.225|
|Europa Zapad|40.115.50.13|
|Indija Zapad|104.211.161.203|
|Zapad SAD-a|104.40.51.248|


## <a name="next-steps"></a>Daljnji koraci  

- Za početak rada s aplikacijama logike slijedite Praktični vodič za [Stvaranje logike aplikacije](app-service-logic-create-a-logic-app.md) .  
- [Primjeri uobičajenih prikaza i scenarijima](app-service-logic-examples-and-scenarios.md)
- [Možete automatizacija poslovnih procesa pomoću aplikacije logike](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Saznajte kako integrirati vaše sustavi s aplikacijama logike](http://channel9.msdn.com/Events/Build/2016/P462)