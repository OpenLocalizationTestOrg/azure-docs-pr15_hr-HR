<properties 
    pageTitle="Upravljanje API koncepti" 
    description="Saznajte više o API-ji, proizvodima, uloge, grupe i koncepata druge upravljanje API-JA." 
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
    ms.topic="hero-article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

#<a name="what-is-api-management"></a>Što je upravljanje API-JA?

Upravljanje API olakšava objavljivanje API-ji vanjskim, partnera i interna razvojnim inženjerima da biste otključali potencijal njihovih podataka i usluge tvrtke ili ustanove. Tvrtke ma gdje se nalazile su pogled da biste proširili njihove operacije kao digitalni platforme, stvaranju novog kanala, pronalaženje novih klijenata i vožnju dublju radnje s postojeće. Upravljanje API nudi competencies core da biste bili sigurni uspješno API programa kroz radnje za razvojne inženjere, uvida tvrtke, analize, sigurnost i zaštitu.

Pogledajte sljedeći videozapis da biste saznali kako Azure API upravljanja te Saznajte kako koristiti API upravljanje da biste dodali mnoge značajke API, uključujući kontrola pristupa, ograničavanje stopa, nadzor, zapisivanje događaja i odgovor predmemoriranje s minimalnim rad na svoj dio.

> [AZURE.VIDEO azure-api-management-overview]

Da biste koristili upravljanje API-JA, administratori mogu stvarati API-ji. Svaki API sastoji se od jednog ili više operacije, a svaki API mogu dodati jedan ili više proizvoda. Da biste koristili API, razvojni inženjeri pretplata proizvoda koji sadrži taj API-JA, a zatim poziva na API operacije primjenjuju sva korištenje pravila koja može biti na snazi.

Ova tema sadrži pregled upravljanja API koncepata.

>[AZURE.NOTE] Dodatne informacije potražite u [oblaku upravljanja API-JA: bogato Power API](http://j.mp/ms-apim-whitepaper) koje PDF-a. Ovaj uvodni koje na upravljanje API po CITO Istraživanje obuhvaća: 
>
> - Preduvjeti za uobičajene API-JA i izazove
>     - Decoupling API-ji i izlaganje facades
>     - Razvojni inženjeri njihovu postavljanju i brzo pokretanje
>     - Zaštita programa access
>     - Analitički i metrike
> - Pokušavaju kontrole i uvida s platforme upravljanje API-JA
> - Korištenje oblaka Dodavanje veze za vanjskih na lokaciji rješenja
> - Upravljanje Azure API-JA

## <a name="apis"> </a>API-ji i operacije

API-ji su foundation instanca servisa upravljanje API-JA. Svaki API predstavlja skup operacije dostupne za razvojne inženjere. Svaki API sadrži reference na servis pozadinske koji implementira u API-JA i njegov operacije karta operacije pomoću servisa pozadinske. Operacije u odjeljku Upravljanje API su iznimno konfigurirati, uz nadzor URL mapiranja, upita i parametara put, zahtjev i odgovor sadržaj i predmemoriju za operaciju odgovor. Ocijeniti ograničenje, kvota i ograničenja IP pravila se mogu primijeniti i na razini pojedinačnih operacija ili API-JA.

Dodatne informacije potražite u članku [Stvaranje API-ji][] i [Dodavanje operacije da biste API][].


## <a name="products"></a> Proizvoda

Proizvodi su način na koji su API-ji naredbama za razvojne inženjere. Proizvodi u odjeljku Upravljanje API imaju jedan ili više API-ji i konfigurirane naslova, opisa i uvjeti korištenja. Proizvodi može biti **otvorena** ili **zaštićenog**. Zaštićeni proizvoda morate imati pretplatu na prije korištenja, prilikom otvaranja proizvoda mogu se bez pretplatu. Kada je spremna za razvojne inženjere proizvoda možete objaviti. Nakon objavljivanja, on može pregledavati (i u slučaju zaštićeni proizvodi pretplaćeni na) tako da razvojnim inženjerima. Odobrenje pretplate je konfiguriran na razini proizvoda i možete potrebno je odobrenje administratora ili se automatski odobrene.

Grupe se koriste za upravljanje vidljivost proizvoda za razvojne inženjere. Proizvodi dopustiti vidljivost grupe i razvojni inženjeri možete pogledati i pretplata na proizvodi koje su vidljive u koji pripadaju grupama. 

Dodatne informacije potražite u članku Stvaranje i objavljivanje proizvoda i [s][] ovom videozapisu.

> [AZURE.VIDEO using-products]

## <a name="groups"></a> Grupe

Grupe se koriste za upravljanje vidljivost proizvoda za razvojne inženjere. Upravljanje API ima sljedeće grupe immutable sustava.

-   **Administratori** – administratori Azure pretplate su članovi ove grupe. Administratori upravljanje instanci servisa za upravljanje API-JA, stvaranje API-ji, operacije i proizvoda koji se koriste za razvojne inženjere.
-   **Razvojni inženjeri** - čija je autentičnost provjerena portala za razvojne inženjere podijeljene korisnike u ovu grupu. Razvojni inženjeri mogu kupaca kojima sastavljanje aplikacije koje koriste vaš API-ji. Razvojni inženjeri imaju pristup portala za razvojne inženjere i izraditi aplikacije koje mogu pozivati operacije API.
-   **Gosti** - Neprovjereni programiranje portala korisnike, kao što su potencijalnim klijentima posjeta portala za razvojne inženjere instance API upravljanje nalaziti u ovoj grupi. Mogu se dodijeliti određeni pristup samo za čitanje, kao što je mogućnost pregledavanja API-ji, ali ne i poziva.

Osim tih grupa sustava administratori mogu stvoriti prilagođene grupe ili [pod utjecajem vanjske grupe u povezane klijenata za Azure Active Directory](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Prilagođeni i vanjske grupe mogu se koristiti uz sustav grupe u daje vidljivosti za razvojne inženjere i pristup API-JA proizvoda. Ako, na primjer, možete stvoriti jednu prilagođenu grupu za razvojne inženjere povezan s određenim partner tvrtke ili ustanove i dopustite pristup za API-ji iz proizvoda koji sadrži odgovarajući API. Korisnik može biti član više od jedne grupe.

Dodatne informacije potražite [u][]članku Stvaranje i korištenje grupe.

## <a name="developers"></a> Razvojni inženjeri

Razvojni inženjeri predstavljaju korisničkim računima u instancu upravljanje API servisa. Razvojni inženjeri možete stvoriti ili pozvani uključite administratori ili ih možete se prijaviti s [portala za razvojne inženjere][]. Svaki za razvojne inženjere član jednu ili više grupa, a možete se pretplatiti proizvoda koji vidljivost dodijeliti tim grupama.

Pretplaćeni razvojnim inženjerima proizvod je dodijeljena primarnih i sekundarnih ključ proizvoda. Ovaj ključ koristi se kada upućivanje poziva u API-ji proizvoda.

Dodatne informacije potražite u članku [Stvaranje ili pozivanje razvojnim inženjerima][] i [kako združivanje s razvojnim inženjerima][].

## <a name="policies"></a> Pravila

Pravila su naprednih mogućnosti upravljanja API koje omogućuju publisher da biste promijenili ponašanje API kroz konfiguraciju. Pravila su skup izraza koji se izvode sekvencijalno na zahtjev ili odgovor API. Popularne naredbe obuhvaćaju pretvaranje oblika iz XML-a JSON i ograničavanje Stopa poziva da biste ograničili količinu dolazne pozive iz razvojni inženjer i dostupne su mnoge pravila.

Pravilnik o izrazima može se koristiti kao vrijednosti atributa ili tekstne vrijednosti u bilo kojem od pravilnike za upravljanje API osim ako pravilo određuje u suprotnom. Neka pravila kao što su pravila [tijek kontrolu](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) i [Postavljanje varijable](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) temelje se na pravilnik o izrazima. Dodatne informacije potražite u članku [dodatna pravila](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [pravila izraza](https://msdn.microsoft.com/library/azure/dn910913.aspx)i pogledajte sljedeći videozapis.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

Popis svih pravila upravljanja API potražite u članku [Referenca za pravila][]. Dodatne informacije o korištenju i konfiguriranju značajke pravilnika potražite u članku [pravila upravljanja API -JA][]. Vodič za stvaranje proizvoda s stopa pravila ograničenje i kvote, potražite u članku [kako stvoriti i konfigurirati proizvoda Napredne postavke][]. Pokazni videozapis, potražite u ovom videozapisu.

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"></a> Portala za razvojne inženjere

Portala za razvojne inženjere je gdje razvojnim inženjerima možete Informirajte se o vaše operacije API-ji, prikaz i poziv, a pretplata na proizvoda. Potencijalnim klijentima mogu posjetiti portal za razvojne inženjere prikaz API-ji i operacije i prijavite se. URL portala za razvojne inženjere nalazi se na nadzornoj ploči za Azure klasični portalu za vaše instancu servisa za upravljanje API-JA.

Možete prilagoditi izgled i dojam portala za razvojne inženjere tako da dodate prilagođeni sadržaj, prilagođavanje stilova, a dodate vaše brendiranje.

## <a name="api-management-and-the-api-economy"></a>Upravljanje API-JA i potrošnje API-JA

Da biste saznali više o upravljanju API-JA, pogledajte sljedeće prezentacije iz konferencije Microsoft Ignite 2015.

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Portala za razvojne inženjere]: #developer-portal

[Kako stvoriti API-ji]: api-management-howto-create-apis.md
[Kako dodati operacije API]: api-management-howto-add-operations.md
[Upute za stvaranje i objavljivanje proizvoda]: api-management-howto-add-products.md
[Kako stvoriti i koristiti grupe]: api-management-howto-create-groups.md
[Kako se pridružiti grupe za razvojne inženjere]: api-management-howto-create-groups.md#associate-group-developer
[Kako stvoriti i konfiguriranje postavki napredne proizvoda]: api-management-howto-product-with-rules.md
[Stvaranje ili pozivanje razvojni inženjeri]: api-management-howto-create-or-invite-developers.md
[Referenca za pravila]: api-management-policy-reference.md
[Pravila upravljanja API-JA]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance



 
