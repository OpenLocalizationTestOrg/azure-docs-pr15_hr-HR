<properties 
    pageTitle="Referenca za pravila upravljanja Azure API-JA" 
    description="Saznajte više o pravilnicima za konfiguriranje upravljanja API-JA." 
    services="api-management" 
    documentationCenter="" 
    authors="vladvino" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="apimpm"/>

# <a name="azure-api-management-policy-reference"></a>Referenca za pravila upravljanja Azure API-JA

Ovo poglavlje sadrži indeks za pravila u odjeljku [Referenca pravila upravljanja API -JA][]. Informacije o dodavanju i konfiguriranju pravila, potražite u članku [pravila u odjeljku Upravljanje API -JA][].

Pravilnik o izrazima može se koristiti kao vrijednosti atributa ili tekstne vrijednosti u bilo kojem od pravilnike za upravljanje API osim ako pravilo određuje u suprotnom. Neka pravila kao što su pravila [tijek kontrolu][] i [Postavljanje varijable][] temelje se na izraze pravila. Dodatne informacije potražite u članku [Napredne pravilnike][] i [pravila izrazi][]

## <a name="policy-reference-index"></a>Indeks referenca pravila

-   [Pravila za pristup ograničenje][]
    -   [Provjera HTTP zaglavlje][] - nameće postojanje i/ili vrijednost HTTP zaglavlje.
    -   [Ograničenje Stopa poziva putem pretplate][] - API onemogućuje korištenje krivina ograničavanjem Stopa poziva, na temelju po pretplate.
    -   [Ograničenje prijenosa poziva tipke](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - API onemogućuje korištenje krivina ograničavanjem Stopa poziva, na temelju po ključ.
    -   [Ograničavanje pozivatelja IP-ovi][] – filtri (omogućuje/onemogućava) pozive iz određene IP adrese i/ili rasponi adresa.
    -   [Postavljanje kvota za korištenje po pretplate][] - omogućuje vam da biste nametnuli moguće ili vijek poziv glasnoću i/ili propusnost ograničenja, na temelju po pretplate.
    -   [Postavljanje kvota za korištenje tipke](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) - omogućuje vam da biste nametnuli moguće ili vijek poziv glasnoću i/ili propusnost ograničenja, na temelju po ključ.
    -   [Provjerite valjanost JWT][] - nameće postojanje i valjanost JWT dobivenih iz navedeni HTTP zaglavlje ili parametar navedeni upit.
-   [Napredni pravila][]
    -   [Kontrola tijek][] - uvjetno primjenjuje izjave pravila na temelju rezultata Booleovih [izraza][].
    -   [Prosljeđivanje zahtjeva][] - prosljeđuje zahtjev za uslugu pozadinskog.
    -   [Zapisnik događaja koncentrator][] - šalje poruke u navedenom obliku poruke ciljno definira [zapisivaču](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entitet.
    -   [Pokušajte ponovno](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) – izvršavanje ponovne pokušaje izraze umetnuti pravila ako i dok se ispuni uvjet. Izvršavanje će ponovite intervalima određeno vrijeme i najviše na određeni broj ponovnih pokušaja.
    -   [Vraćanje odgovor](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) – izvršavanje Aborts kanal i vraća navedenu odgovor izravno pozivatelju.
    -   [Pošaljite zahtjev za jednu pokrenuta](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - šalje zahtjev navedenom URL bez čeka se odgovor.
    -   [Pošaljite zahtjev za](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) – šalje zahtjev navedenom URL.
    -   [Metodu zahtjev set](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - omogućuje vam da biste promijenili način HTTP zahtjev.
    -   [Postavljanje statusa](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) – mijenja se HTTP kod stanja u određenu vrijednost.
    -   [Postavljanje varijable][] – zadržava vrijednost u varijablu imenovani [kontekst][] za access novijim.
    -   [Praćenje](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - dodaje niza u izlaz [Kontrola API -JA](../api-management/api-management-howto-api-inspector.md) .
    -   [Pričekajte](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - čeka umetnuti zahtjev za slanje, Dohvati vrijednosti iz predmemorije ili kontrolu tijek pravila da biste dovršili prije nastavka.
-   [Pravila za provjeru autentičnosti][]
    -   [Provjere autentičnosti uz Basic][] - provjere autentičnosti sa servisom pozadinskog pomoću osnovnu provjeru autentičnosti.
    -   [Provjere autentičnosti certifikatom klijent][] - provjere autentičnosti sa servisom pozadinskog pomoću certifikata klijenta.
-   [Predmemoriranje pravila][] 
    -   [Početak iz predmemorije][] - izvođenje predmemorije traženje i vratili valjani predmemorirani odgovora kada su dostupni.
    -   [Spremište predmemoriju][] - predmemorije odgovor prema konfiguraciju kontrole navedeni predmemoriju.
    -   [Preuzmite vrijednost iz predmemorije](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - Vrati predmemorirane stavke tipke.
    -   [Spremište vrijednost u predmemoriji](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - spremište stavku u predmemoriji tipke.
    -   [Uklonite vrijednost iz predmemorije](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - Ukloni stavku u predmemoriji tipke.
-   [Unakrsni pravila domene][] 
    -   [Dopusti domenama pozive][] – u API olakšava pristupačnost zbog klijenata Adobe Flash i Microsoft Silverlight utemeljenima na pregledniku.
    -   [CORS][] - dodaje unakrsno polazište resursa (CORS) podršku za operaciju ili API-JA da biste omogućili domenama pozive iz utemeljenima na pregledniku klijenata za zajedničko korištenje.
    -   [JSONP][] - dodaje JSON s podrškom za popunjavaju (JSONP) s operacijom ili API Dopusti pozive domenama JavaScript klijenata utemeljenima na pregledniku.
-   [Pretvorba pravila][] 
    -   [Pretvaranje JSON XML][] - pretvara zahtjeva i odgovora tijelo iz JSON XML.
    -   [Pretvaranje XML JSON][] - pretvara zahtjeva i odgovora tijelo iz XML-a JSON.
    -   [Traženje i zamjena niza u tijelu][] – pronalazi podniz zahtjeva i odgovora i zamjenjuje drugi podniz.
    -   [Maske za unos URL-ovi u sadržaju][] – ponovno piše (maske) veze u odgovoru tijelo tako da na ekvivalentnu vezu putem pristupnika.
    -   [Postavljanje servisa pozadinskog][] – mijenja pozadinskog služba za dolazni zahtjev.
    -   [Postavljanje tijelo][] - postavlja tijela poruke dolazne i odlazne zahtjeva za.
    -   [Postavljanje HTTP zaglavlje][] - dodjeljuje vrijednost u postojeće odgovor i/ili zahtjev za zaglavlje ili dodaje novi odgovor i/ili zahtjev za zaglavlje.
    -   [Postavljanje parametra niza upita][] - dodaje, zamjenjuje vrijednost ili briše zahtjev parametra niza upita.
    -   [Dopune URL][] - pretvara zahtjev za URL-a iz javne obrasca u obrazac očekivani po web-servisa.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o pravilima izraza potražite u članku ovom videozapisu.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Pravila za pristup ograničenje]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Provjera HTTP zaglavlje]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Ograničenje Stopa poziva putem pretplate]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Ograničavanje pozivatelja IP-ovi]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Postavljanje kvota korištenja putem pretplate]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Provjerite valjanost JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Napredni pravila]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Kontrola toka]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Postavljanje varijable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Izrazi]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[Kontekst]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Prosljeđivanje zahtjeva]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Zapisnik događaja koncentrator]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Pravila za provjeru autentičnosti]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Autentičnost uz Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Autentičnost certifikatom klijenta]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Predmemoriranje pravila]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Početak iz predmemorije]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Spremanje u predmemoriju]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Unakrsni pravila domene]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Dopusti pozive domenama]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Pretvorba pravila]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Pretvaranje JSON XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Pretvaranje XML JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Traženje i zamjena niza u tijelu]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Maske za unos URL-ovi u sadržaju]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Postavljanje pozadinskog servisa]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Postavljanje tijelo]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Postavljanje HTTP zaglavlje]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Parametra niza upita za postavljanje]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Dopune URL-a]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Pravila u odjeljku Upravljanje API-JA]: api-management-howto-policies.md
[Referenca za pravila upravljanja API-JA]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Pravilnik o izrazima]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 
