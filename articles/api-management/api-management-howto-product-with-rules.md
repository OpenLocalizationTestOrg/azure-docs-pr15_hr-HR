<properties
    pageTitle="Zaštita API-JA s upravljanjem Azure API | Microsoft Azure"
    description="Saznajte kako se zaštititi API-JA s kvotama i ograničavanje pravila (stopa-ograničavanjem)."
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

# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Zaštita vaše API-JA s ograničenjima stopa značajkom upravljanja API Azure

Ovaj vodič prikazuje kako je jednostavno dodajte zaštitu pozadinski API konfiguriranjem stopa ograničenje i kvote pravila s upravljanjem API Azure.

U ovom ćete praktičnom vodiču će stvoriti "Besplatnu probnu verziju" API-JA proizvoda koji razvojnim inženjerima omogućuje da do 10 pozive u minuti i maksimalno 200 pozive po tjednu do vaše API-JA pomoću [Stopa poziva ograničenja pretplate](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) i [Postavljanje kvota za korištenje po pretplati](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) pravila. Zatim će objaviti na API-JA i testiranje pravila ograničenje stopa.

Naprednije regulacije scenarija pomoću pravila [stopa ograničenje po ključ](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) i [kvote po ključ](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) , potražite u članku [Dodatne zahtjev za ograničavanje s upravljanjem Azure API -JA](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>Da biste stvorili proizvoda

U ovom ćete koraku stvarate besplatnu probnu verziju proizvoda koje je potrebno odobrenje pretplate.

>[AZURE.NOTE] Ako već imate proizvod koji je konfiguriran i želite je koristiti za ovaj vodič, možete prebaciti [Konfiguriraj poziva stopa pravila ograničenje i kvota][] i slijedite vodič s korištenjem proizvod umjesto besplatnu probnu verziju proizvoda.

Da biste započeli, kliknite **Upravljanje** u Azure klasični servisa za upravljanje API-JA. Tako ćete doći do portala za upravljanje API programa publisher.

![Portal za Publisher][api-management-management-console]

>Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa upravljanje API -JA][] u vodič za [Upravljanje prvi API-JA u upravljanju Azure API -JA][] .

Kliknite **proizvoda** na izborniku **Upravljanje API -JA** na lijevoj strani da biste prikazali stranicu **proizvoda** .

![Dodavanje proizvoda][api-management-add-product]

Kliknite **Dodaj proizvoda** da biste prikazali dijaloški okvir **Dodaj novi proizvod** .

![Dodaj novi proizvod][api-management-new-product-window]

U okvir **Naslov** upišite **Besplatnu probnu verziju**.

U okvir **Opis** upišite sljedeći tekst:  **pretplatnike će moći pokrenuti 10 pozive/minutni maksimalno 200 pozive/dan nakon čega je zabranjen pristup do.**

Proizvodi u odjeljku Upravljanje API-JA možete zaštititi ili otvorite. Zaštićeni proizvoda morate imati pretplatu na prije korištenja. Otvaranje proizvoda koristi se bez pretplatu. Provjerite **potrebna pretplata** je li odabrana da biste stvorili zaštićeni proizvod koji zahtijeva pretplatu. Ovo je zadana postavka.

Ako želite da se administrator za pregled i prihvaćanje ili odbacivanje pokušaja pretplata na ovaj proizvod, odaberite **potrebno odobrenje pretplate**. Ako nije potvrđen okvir, prilikom pokušaja pretplate bit će automatski odobrene. U ovom primjeru pretplate automatski odobrene, tako da potvrdite okvir.

Da biste omogućili račune za razvojne inženjere da biste se pretplatili više puta novi proizvod, odaberite potvrdni okvir **Dopusti višestruke pretplate istodobno** . Pomoću ovog praktičnog vodiča koristiti višestruke pretplate istodobno, pa ga ostavite poništen.

Kada se unose sve vrijednosti, kliknite **Spremi** da biste stvorili proizvoda.

![Proizvod dodali][api-management-product-added]

Po zadanom su vidljive korisnika u grupu **Administratori** novih proizvoda. Ne možemo prolaze da biste dodali grupe za **razvojne inženjere** . Kliknite **Besplatnu probnu verziju**, a zatim kliknite karticu **vidljivosti** .

>U odjeljku Upravljanje API-JA, grupe se koriste za upravljanje vidljivost proizvoda za razvojne inženjere. Proizvodi dopustiti vidljivost grupe i razvojni inženjeri možete pogledati i pretplata na proizvodi koje su vidljive u kojem pripadaju grupama. Dodatne informacije potražite [u][]članku Stvaranje i korištenje grupe u upravljanju API Azure.

![Dodavanje grupe za razvojne inženjere][api-management-add-developers-group]

Potvrdite okvir **razvojni inženjeri** , a zatim kliknite **Spremi**.

## <a name="add-api"> </a>Da biste dodali API proizvoda

U ovom koraku vodič dodat ćemo jeka API-JA za novi proizvod besplatnu probnu verziju.

>Svaku instancu servisa za upravljanje API dolazi unaprijed konfigurirana s jeka API-JA koje je moguće koristiti Eksperimentirajte s te informacije o upravljanju API-JA. Dodatne informacije potražite u članku [Upravljanje prvi API-JA u upravljanju Azure API -JA][].

Kliknite **Proizvodi** na izborniku **Upravljanje API -JA** na lijevoj strani, a zatim **Besplatnu probnu verziju** da biste konfigurirali proizvoda.

![Konfiguriranje proizvoda][api-management-configure-product]

Kliknite **Dodavanje API -JA proizvoda**.

![Dodavanje API-JA proizvoda][api-management-add-api]

Odaberite **Jeka API -JA**, a zatim **Spremi**.

![Dodavanje jeka API-JA][api-management-add-echo-api]

## <a name="policies"> </a>Da biste konfigurirali poziv stopa ograničenje kvote pravilnike o i

Ograničenja stopa i kvotama konfigurirane u uređivaču pravila. Na izborniku **Upravljanje API -JA** na lijevoj strani kliknite **pravila** . Na popisu **proizvoda** kliknite **Besplatnu probnu verziju**.

![Pravila proizvoda][api-management-product-policy]

Kliknite **Dodaj pravilo** da biste uvezli predložak pravila i počeli sa stvaranjem stopu pravila ograničenje i kvote.

![Dodavanje pravila][api-management-add-policy]

Da biste umetnuli pravila, postavite pokazivač u okvir **dolazni** ili **Izlazni** odjeljak predložak pravila. Stopa ograničenje i kvote pravila su Ulazna pravila, tako da postavite pokazivač na element dolazni.

![Uređivač pravila][api-management-policy-editor-inbound]

Dva pravila ne možemo dodajete ovog praktičnog vodiča su pravila [ograničenja Stopa poziva pretplate](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) i [Postavljanje kvota za korištenje po pretplati](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) .

![Izraze pravila][api-management-limit-policies]

Kada je pokazivač smješten u elementu **Ulazna** pravila, kliknite strelicu pokraj **ograničenje Stopa poziva za pretplatu** da biste umetnuli njegov predložak pravila.

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**Ograničenje prijenosa poziva po pretplati** mogu koristiti na razini proizvoda i može se koristiti na API-JA i pojedinačne operacija naziv razine. U ovom ćete praktičnom vodiču samo razinu proizvoda pravila koriste, pa brisanje elementi **api** i **operacija** s element **stopa ograničenje** , tako da se samo na vanjski **stopa ograničenje** element ostaje, kao što je prikazano u sljedećem primjeru.

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

U proizvodu besplatnu probnu verziju rate maksimalne dopuštene poziv je 10 pozive u minuti, pa upišite **10** kao vrijednost atributa **pozive** i **60** atributa **razdoblje obnove** .

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

Da biste konfigurirali pravilo **Postavljanje kvota za korištenje po pretplati** , postavite kursor neposredno ispod element novododani **stopa ograničenje** unutar **ulaznog** elementa, a zatim strelicu s lijeve strane **Postavljanje kvota za korištenje po pretplati**.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

Budući da se na razini proizvoda i namijenjen ovo pravilo, izbrišite naziv elementa **API-ja** i **operacija** kao što je prikazano u sljedećem primjeru.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

Kvota mogu se temeljiti na broj poziva po interval, propusnost ili i jedno i drugo. Pomoću ovog praktičnog vodiča smo su ograničavanje ovisno o propusnosti, pa izbrišite atribut **propusnosti** .

    <quota calls="number" renewal-period="seconds">
    </quota>

U proizvodu besplatnu probnu verziju kvote je 200 pozive po tjednu. Odredite **200** kao vrijednost atributa **poziva** , a zatim navedite **604800** kao vrijednost atributa **razdoblje obnove** .

    <quota calls="200" renewal-period="604800">
    </quota>

>Pravila intervalima su navedeni u sekundama. Da biste izračunali interval za tjedan, broj dana (7) možete pomnožite sa broj sati u danu (24) i broj minuta u sat (60), broj sekundi u minuti (60): 7 *24* 60 * 60 = 604800.

Kada završite konfiguriranje pravilnika, moraju podudarati u sljedećem primjeru.

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

Nakon pravilnicima za željenu konfiguriran, kliknite **Spremi**.

![Spremanje pravila][api-management-policy-save]

## <a name="publish-product"></a> Da biste objavili proizvoda

Sad kad u API-ji dodaju i pravilnike konfigurirane, proizvod mora biti objavljena tako da ga mogu poslužiti razvojnim inženjerima. Kliknite **Proizvodi** na izborniku **Upravljanje API -JA** na lijevoj strani, a zatim **Besplatnu probnu verziju** da biste konfigurirali proizvoda.

![Konfiguriranje proizvoda][api-management-configure-product]

Kliknite **Objavi**, a zatim kliknite da biste potvrdili **Da, objavite ga** .

![Objavljivanje proizvoda][api-management-publish-product]

## <a name="subscribe-account"> </a>Da biste se pretplatili na račun za razvojne inženjere za proizvod

Sad kad je objavljen proizvoda, on je dostupan da biste se pretplatili i koristi razvojnim inženjerima.

>Administratori instance API upravljanje automatski se pretplatili za svaki proizvod. U ovom koraku vodiča ne možemo će se pretplatiti jedan od računa koji nisu administratori za razvojne inženjere za besplatnu probnu verziju proizvoda. Ako je vaš račun za razvojne inženjere dio ulogu administratora, pa možete pratiti uz ovaj korak, čak i ako ste već pretplaćeni.

Kliknite **korisnici** na izborniku **Upravljanje API -JA** na lijevoj strani, a zatim naziv računa za razvojne inženjere. U ovom primjeru ćemo se pomoću računa za razvojne inženjere **Clayton Gragg** .

![Konfiguriranje za razvojne inženjere][api-management-configure-developer]

Kliknite **Dodavanje pretplate**.

![Dodavanje pretplate][api-management-add-subscription-menu]

Odaberite **Besplatnu probnu verziju**, a zatim **Pretplati se**.

![Dodavanje pretplate][api-management-add-subscription]

>[AZURE.NOTE] U ovom ćete praktičnom vodiču višestruke pretplate istodobno nisu omogućeni za besplatnu probnu verziju proizvoda. Ako su, koje želite zatražiti naziv pretplate, kao što je prikazano u sljedećem primjeru.

![Dodavanje pretplate][api-management-add-subscription-multiple]

Nakon klika na **pretplati**, proizvod pojavljuje se na popisu **pretplatu** za korisnika.

![Pretplata dodali][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Poziva operaciju i testiranje ograničenja stopa

Sad kad besplatnu probnu verziju proizvoda je konfiguriran i objavljivanja, možemo poziva neki postupci i testiranje pravila ograničenje stopa.
Prelazak na portala za razvojne inženjere tako da kliknete **portala za razvojne inženjere** na izborniku u gornjem desnom kutu.

![Portala za razvojne inženjere][api-management-developer-portal-menu]

Kliknite **API-ji** u gornji izbornik, a zatim **Jeka API -JA**.

![Portala za razvojne inženjere][api-management-developer-portal-api-menu]

Kliknite **DOHVATI resursa**, a zatim kliknite **isprobajte**.

![Otvorene konzole][api-management-open-console]

Zadržavanje zadanog vrijednosti parametra, a zatim odaberite ključa pretplatu za besplatnu probnu verziju proizvoda.

![Ključ za pretplatu][api-management-select-key]

>[AZURE.NOTE] Ako imate više pretplata, svakako odaberite tipku za **Besplatnu probnu verziju**, jer inače pravila koje ste konfigurirali u prethodnim koracima neće biti na snazi.

Kliknite **Pošalji**, a zatim pogledajte odgovor. Imajte na umu **status odgovora** **200 u redu**.

![Rezultati postupka][api-management-http-get-results]

Kliknite **Pošalji** brzinom veći od pravila stopa ograničenje od 10 pozive u minuti. Kada je premašuje ograničenje pravila stopa, vraća se status odgovora **429 previše mnogo** zahtjeva.

![Rezultati postupka][api-management-http-get-429]

**Sadržaj odgovor** označava preostale interval prije ponovne pokušaje će biti uspješno.

Kada je pravilnik stopa ograničenje od 10 pozive u minuti na snazi, naknadni pozivi neće uspjeti dok ste 60 sekundi proteklih od prvog 10 uspješno poziva na proizvod prije nego što je prekoračeno ograničenje stopa. U ovom primjeru preostale interval je 54 sekundi.

## <a name="next-steps"> </a>Sljedeće korake

-   Pogledajte pokazni videozapis o postavljanju ograničenja stopa i kvotama u ovom videozapisu.

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Upravljanje prvi API-JA u upravljanju API Azure]: api-management-get-started.md
[Kako stvoriti i koristiti grupe u upravljanju API Azure]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Konfiguriranje pravila ograničenje i kvote poziv stopa]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
