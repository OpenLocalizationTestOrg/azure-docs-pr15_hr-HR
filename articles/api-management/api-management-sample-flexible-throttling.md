<properties
    pageTitle="Napredni zahtjev za ograničavanje s upravljanjem API Azure"
    description="Saznajte kako stvoriti i primijeniti fleksibilne kvota i stopa ograničavanje pravilnike za upravljanje Azure API-JA."
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="advanced-request-throttling-with-azure-api-management"></a>Napredni zahtjev za ograničavanje s upravljanjem API Azure

Moći throttle zahtjevi je ulogu upravljanja Azure API-JA. Ili nadzirete rata zahtjeve ili ukupni zahtjeve/podatke prenijeti upravljanje API omogućuje davatelja API-JA možete zaštititi svoje API-ji od zloupotreba i stvoriti vrijednosti za različite razine API-JA proizvoda.

## <a name="product-based-throttling"></a>Ograničavanje proizvoda koji se temelje
Do datuma, ograničavanje stopa mogućnosti su ograničeni iz djelokruga određenu pretplatu proizvoda (zapravo ključ), definiran na portalu za upravljanje API programa publisher. To je korisno za davatelja API da biste primijenili ograničenja razvojnim inženjerima koji su se prijavili za korištenje njihove API-JA, međutim, ona ne pomogne, na primjer, u ograničavanje pojedinačne krajnjim na API-JA. Nije moguće koja se za jedan korisnik aplikacije za razvojne inženjere zauzeti cijeli kvote i zatim onemogućili ostali korisnici programer da biste koristili aplikaciju. Osim toga, nekoliko korisnici koji su možda generiranje veliku količinu zahtjeve mogu ograničiti pristup Povremeni korisnici.

## <a name="custom-key-based-throttling"></a>Prilagođeni ključ koji se temelje ograničavanje
Nova pravila [stopa ograničenje po ključ](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) i [kvote po ključ](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) omogućuju znatno fleksibilnije rješenje promet kontrolu. Ta nova pravila omogućuju da odredite izraza za prepoznavanje tipke koje će se koristiti za praćenje korištenja promet. Način na koji to funkcionira easiest prikazanom s primjerom. 

## <a name="ip-address-throttling"></a>Ograničavanje IP adresa
Sljedeća pravila ograničiti jednog klijenta IP adresa samo 10 pozive svake minute s Ukupno 1,000,000 pozive i 10 000 kilobajta propusnosti mjesečno. 

    <rate-limit-by-key  calls="10"
              renewal-period="60"
              counter-key="@(context.Request.IpAddress)" />

    <quota-by-key calls="1000000"
              bandwidth="10000"
              renewal-period="2629800"
              counter-key="@(context.Request.IpAddress)" />

Ako se sve klijente na Internetu koristi jedinstveni IP adresa, to može biti učinkovit način od ograničavanje korištenje korisnika. Međutim, je prilično vjerojatno više korisnika će zajedničko korištenje jedne javnu IP adresu zbog ih pristup Internetu putem NAT uređaja. Bez obzira na to, za API-ji koji omogućuju pristup Neprovjereni na `IpAddress` možda je najbolja mogućnost.

## <a name="user-identity-throttling"></a>Ograničavanje identitet korisnika
Ako je autentičnost provjerena krajnjeg korisnika, zatim regulacije ključ se mogu stvoriti na temelju informacija koje služi kao jedinstvena identifikacija programa koji korisnik.

    <rate-limit-by-key calls="10"
        renewal-period="60"
        counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />

U ovom primjeru smo izdvojiti zaglavlje autorizacije, ga pretvoriti `JWT` objekta te pomoću predmet token identifikaciju korisnika i koje koristite kao stopa ograničavanje ključ. Ako je pohranjena identitet korisnika u `JWT` kao jednu od druge zahtjeve zatim vrijednost ne može se koristiti umjesto nje.

## <a name="combined-policies"></a>Kombinirani pravila
Iako se nova pravila regulacije omogućuju veću kontrolu od postojećih regulacije pravila, i dalje je riječ o vrijednost kombiniranje obje mogućnosti. Ograničavanje prema ključ proizvoda pretplate ([ograničenje Stopa poziva putem pretplate](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) i [Postavljanje kvota za korištenje po pretplate](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) sjajan je način da biste omogućili monetizing od API po puni koji se temelji na korištenje razine. Učinkovitiji grained kontrolu nad moći throttle korisnik je komplementarnu i onemogućuje Smanji sučelje drugog ponašanje jednog korisnika. 

## <a name="client-driven-throttling"></a>Klijent utemeljenima na ograničavanje
Pri definiranju tipku regulacije pomoću [pravila izraz](https://msdn.microsoft.com/library/azure/dn910913.aspx), zatim je davatelj usluga za API koji odaberete način je usmjeren na ograničavanje. Međutim, razvojni inženjer preporučuje se da biste odredili kako se ocijeniti ograničenje vlastite klijentima. To može biti omogućen davatelj API uvođenjem prilagođeno zaglavlje da biste omogućili klijentska aplikacija za razvojne inženjere za komunikaciju ključ za API-JA.

    <rate-limit-by-key calls="100"
              renewal-period="60"
              counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>

Time se omogućuje za razvojne inženjere klijentske aplikacije da biste odabrali način da biste stvorili stopa ograničavanje ključ. Uz malo domišljatosti klijenta za razvojne inženjere mogli stvoriti vlastite razine stopa dodjeljivanje skupa tipke korisnicima i rotiranje korištenja ključa.

## <a name="summary"></a>Sažetak
Azure upravljanja API nudi stopu i ponudu za ograničavanje zaštiti i dodajte vrijednost u funkcioniranju servisa API-JA. Nova regulacije pravila pomoću prilagođenih scoping pravila omogućuju učinkovitiji grained kontrolu nad ta pravila da biste omogućili klijentima da biste sastavili još bolje aplikacije. Primjeri u ovom se članku ilustrira korištenje ove nova pravila prema proizvodnje stopa ograničavanje tipke s IP adrese klijenta, identitet korisnika i vrijednosti klijent generira. No postoji mnogo dijelova poruke koje se mogu koristiti kao što su korisnički agent, fragmentirane put URL-a, a zatim veličinu poruke.

## <a name="next-steps"></a>Daljnji koraci
Pošaljite nam povratne informacije u niti Disqus ove teme. Bi odlične za primanje obavijesti o potencijalne ključa vrijednosti koje su logički odabir u vašim scenarijima.

## <a name="watch-a-video-overview-of-these-policies"></a>Pogledajte videozapis pregled tih pravila
Dodatne informacije o pravilima [stopa ograničenje po ključ](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) i [kvote po ključ](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) u ovom članku, pogledajte sljedeći videozapis.

> [AZURE.VIDEO advanced-request-throttling-with-azure-api-management]
