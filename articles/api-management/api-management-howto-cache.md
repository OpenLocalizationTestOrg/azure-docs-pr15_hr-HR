<properties
    pageTitle="Dodavanje predmemoriranje da biste poboljšali performanse u upravljanju API Azure | Microsoft Azure"
    description="Saznajte kako unaprijediti Latencija, utrošak propusnosti i opterećenje web servisa za upravljanje API servisa pozive."
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Dodavanje predmemoriranje da biste poboljšali performanse u upravljanju API Azure

Operacije u odjeljku Upravljanje API moguće je konfigurirati za predmemoriranje odgovor. Predmemoriranje odgovor možete znatno smanjiti API Latencija, utrošak propusnosti i web-opterećenja servisa za podatke koji se često mijenjaju.

Ovaj vodič prikazuje kako dodati odgovor predmemoriju za vaše API-JA i konfiguriranje pravilnika o operacija jeka API uzorka. Zatim možete nazvati postupak s portala za razvojne inženjere radi potvrde predmemoriranja u akciji.

>[AZURE.NOTE] Informacije o predmemoriranja stavke po ključa pomoću pravila izraza potražite u članku [Prilagođeno predmemoriranja u upravljanju Azure API -JA](api-management-sample-cache-by-key.md).

## <a name="prerequisites"></a>Preduvjeti

Prije nego što slijedeći korake u ovom vodiču, potreban vam je instanca servisa API upravljanje API, a proizvod konfigurirana. Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa API upravljanje][] praktičnog vodiča za [Početak rada s upravljanjem Azure API -JA][] .

## <a name="configure-caching"> </a>Konfiguriranje operacija za predmemoriranje

U ovom ćete koraku će pregledajte postavkama predmemoriranja operacije **DOBITI resursa (predmemorirani)** uzorka API jeku.

>[AZURE.NOTE] Svaku instancu servisa za upravljanje API dolazi konfiguriranog s jeka API-JA koje je moguće koristiti Eksperimentirajte s te informacije o upravljanju API-JA. Dodatne informacije potražite u članku [Početak rada s upravljanjem Azure API -JA][].

Da biste započeli, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA. Tako ćete doći do portala za upravljanje API programa publisher.

![Portal za Publisher][api-management-management-console]

Kliknite **API-ji** na izborniku **Upravljanje API -JA** na lijevoj strani, a zatim **API jeku**.

![Echo API][api-management-echo-api]

Kliknite karticu **operacije** , a zatim postupak **DOHVATI resursa (predmemorirani)** na popisu **operacija** .

![Operacije jeka API-JA][api-management-echo-api-operations]

Kliknite karticu **Međuspremanje** da biste vidjeli postavkama predmemoriranja za ovaj postupak.

![Kartica predmemoriranja][api-management-caching-tab]

Da biste omogućili predmemoriranje za operaciju, potvrdite okvir **Omogući** . U ovom primjeru predmemoriranje omogućena.

Odnose se svaki odgovor operacija na temelju vrijednosti u poljima **Razlikovanje po parametrima niza upita** i **Razlikovanje po zaglavlja** . Ako želite više odgovora na temelju parametara niza upita ili zaglavlja u predmemoriju, možete ih konfigurirati u ta dva polja.

**Trajanje** određuje intervala isteka predmemorirani odgovora. U ovom primjeru interval je **3600** sekundi, koja je jednaka jedan sat.

U ovom se primjeru koristi predmemoriranja konfiguraciju, prvi zahtjev za operaciju **DOBITI resursa (predmemorirani)** vraća odgovor iz servisa pozadinskog. Odgovor spremit će se, odnose prema navedenom zaglavlja i parametrima niza upita. Naknadni pozivi za rad s odgovarajućom parametrima će imati predmemorirani odgovor Vrati, dok je istekla interval trajanja predmemorije.

## <a name="caching-policies"> </a>Pregledajte predmemoriranja pravila

U ovom ćete koraku pregledajte postavkama predmemoriranja za operaciju **DOBITI resursa (predmemorirani)** uzorka API jeku.

Kada se postavkama predmemoriranja konfiguriran za operaciju na kartici **Međuspremanje** , predmemoriranja pravila dodaju se za operaciju. Ta pravila možete prikazati i uređivati u uređivaču pravila.

Na izborniku **Upravljanje API -JA** na lijevoj strani kliknite **pravila** , a zatim odaberite **Echo API / DOBITI resursa (predmemorirani)** na padajućem popisu **operacija** .

![Operacije pravilima dosega][api-management-operation-dropdown]

Prikazat će se pravila za ovaj postupak u uređivaču pravila.

![Uređivač pravila upravljanja API-JA][api-management-policy-editor]

Definicija pravila za ovaj postupak sadrži pravila koje definiraju predmemoriranja konfiguracije koji su pregledavati putem kartice **Međuspremanje** u prethodnom koraku.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] Promjene unesene u predmemoriranja pravila u uređivaču pravila odrazit će se na kartici **Međuspremanje** postupak i obratno.

## <a name="test-operation"> </a>Poziva operaciju i testiranje predmemoriranja

Da biste vidjeli predmemoriranja u praksi, ne možemo možete nazvati postupak s portala za razvojne inženjere. Kliknite **portala za razvojne inženjere** u gornji desni izbornik.

![Portala za razvojne inženjere][api-management-developer-portal-menu]

Kliknite **API-ji** u gornji izbornik, a zatim **API jeku**.

![Echo API][api-management-apis-echo-api]

>Ako imate samo jedan API konfiguriran ili vidljiva na vaš račun, zatim kliknete API-ji vas vodi izravno operacije za taj API-JA.

Odaberite operaciju **DOBITI resursa (predmemorirani)** , a zatim **Otvori konzolu**.

![Otvorene konzole][api-management-open-console]

Na konzoli omogućuje pozivanje operacije izravno na portalu za razvojne inženjere.

![Konzola][api-management-console]

Zadrži zadane vrijednosti za **param1** i **param2**.

Odaberite željenu tipku s padajućeg popisa **ključa za pretplatu** . Ako račun sadrži samo jedan pretplate, već biti odabran.

Unesite **sampleheader:value1** u tekstnom okviru **zahtjev zaglavlja** .

Kliknite **HTTP Get** i zabilježite zaglavlja odgovor.

Unesite **sampleheader:value2** u tekstnom okviru **zahtjev zaglavlja** , a zatim kliknite **HTTP Get**.

Imajte na umu da vrijednost **sampleheader** i dalje **vrijednost1** u odgovoru. Isprobajte neke druge vrijednosti i zabilježite vraća se predmemorirani odgovor prvi poziv.

Unesite **25** u polje **param2** , a zatim kliknite **HTTP Get**.

Imajte na umu da vrijednost **sampleheader** u odgovoru sada **vrijednost2**. Budući da se rezultati postupka odnose prema nizu upita, prethodni predmemorirani odgovor nije vraćen.

## <a name="next-steps"> </a>Sljedeće korake

-   Dodatne informacije o pravilima za predmemoriranja potražite u članku [Referenca za pravila upravljanja API][] [predmemoriranje pravila][] .
-   Informacije o predmemoriranja stavke po ključa pomoću pravila izraza potražite u članku [Prilagođeno predmemoriranja u upravljanju Azure API -JA](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Početak rada s upravljanjem API Azure]: api-management-get-started.md

[Referenca za pravila upravljanja API-JA]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Predmemoriranje pravila]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
