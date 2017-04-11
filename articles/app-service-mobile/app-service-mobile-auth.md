<properties
    pageTitle="Provjere autentičnosti i autorizacije u Azure mobilne aplikacije | Microsoft Azure"
    description="Konceptualni reference i pregled provjeru autentičnosti / autorizacije značajki za Azure mobilne aplikacije"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Provjere autentičnosti i autorizacije u Azure mobilne aplikacije

## <a name="what-is-app-service-authentication--authorization"></a>Što je provjera autentičnosti za aplikaciju servisa / autorizacije?

> [AZURE.NOTE] Će se premjestiti u ovoj se temi da biste na Konsolidirani [Provjera autentičnosti za aplikaciju servisa / autorizacije](../app-service/app-service-authentication-overview.md) tema, kojemu su opisane Web, Mobile i aplikacije API-JA.

Provjera autentičnosti za aplikaciju servisa / autorizacije je značajka koja omogućuje aplikacije da biste se prijavili u korisnicima bez ikakvih promjena kod potrebna pozadinska aplikacija. Pruža jednostavan način da biste zaštitili svoju aplikaciju i rad s podacima po korisniku.

Aplikacije servisa za koristi pridruženim identiteta, u kojem 3 proizvođača **davatelja identiteta** ("IDP") sprema računi i potvrđuje korisnika, a aplikacija koristi ovaj identitet umjesto vlastitu. Aplikacije servisa za podržava pet davatelji identiteta iz okvir: _ _Azure Active Directory_, _Facebook_, _Google_, Microsoftov račun_, a _na Twitteru_. Možete proširiti i tu podršku za aplikacije po integriranje drugog davatelja identiteta ili prilagođenog identiteta rješenje.

Pokrenite aplikaciju možete koristiti bilo koji broj od tih davatelja usluga identiteta tako da unesete krajnjim korisnicima s mogućnostima kako se prijaviti.

Ako želite započeti odmah, pogledajte jedan od sljedećih vodiči za:

- [Dodavanje provjere autentičnosti za aplikaciju za iOS]
- [Dodavanje provjere autentičnosti Xamarin.iOS aplikacije]
- [Dodavanje provjere autentičnosti Xamarin.Android aplikacije]
- [Dodavanje provjere autentičnosti aplikacija za Windows]

## <a name="how-authentication-works"></a>Kako funkcionira provjera autentičnosti

Da bi se autentičnost Neki davatelji identiteta, najprije morate konfigurirati davatelja identiteta informacije o aplikaciji. Davatelja identiteta zatim omogućit će vam ID-ove i tajne koje ste omogućili natrag u aplikaciji. Dovršava odnos pouzdanosti i omogućuje aplikacije servisa za provjeru identiteta navedene na njega.

Ove korake detaljno opisane u sljedećim temama:

- [Upute za konfiguriranje aplikacije da biste koristili prijava Azure Active Directory]
- [Upute za konfiguriranje aplikacije da biste koristili prijava za Facebook]
- [Upute za konfiguriranje aplikacije da biste koristili prijavu u Google]
- [Upute za konfiguriranje aplikacije da biste koristili Microsoftov Account prijava]
- [Upute za konfiguriranje aplikacije da biste koristili Twitter prijava]

Nakon što je konfiguriran na pozadinski, možete izmijeniti klijent za prijavu. Postoje dva načina prikaza ovdje:

- Koristite samo jedan redak kod, omogućuju mobilne aplikacije klijenta SDK Prijava korisnika.
- Korištenje programa SDK objavljene davatelj određenom identitetu da biste ostvarili identiteta i ostvariti pristup aplikacije servisa.

>[AZURE.TIP] Većina aplikacija trebali biste koristiti davatelja SDK da biste dobili sučelje za prijavu više lokalnog osjećaj i odražava podršku za osvježavanje i ostale pogodnosti karakteristične za davatelja.

### <a name="how-authentication-without-a-provider-sdk-works"></a>Kako funkcionira provjera autentičnosti bez davatelja SDK

Ako ne želite da biste postavili davatelja SDK, možete omogućiti mobilne aplikacije za izvođenje prijave za vas. Klijent mobilne aplikacije SDK će otvoriti web-prikaz s davateljem vašem odabiru i ispunite prijave. Povremeno blogova i forume vidjet ćete ovo naziva kao "poslužitelj tijek" ili "poslužitelj usmjereni tijek" od poslužitelj je upravljanje prijave i klijent SDK nikad ne prima token za davatelja usluga.

Kod koji je potrebno da biste pokrenuli ovaj tijek opisana su u odjeljku Provjera autentičnosti vodič za svaki platformu. Na kraju tijeka klijent SDK ima token za aplikacije servisa, a sve zahtjeve za pozadinski automatski priložiti token.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Kako funkcionira provjera autentičnosti kod davatelja usluga SDK

Rad s davatelja SDK omogućuje sučelje prijavljuje više čvrsto interakciju s platformu OS aplikaciju radi. Time se davatelja token i neke korisničke informacije na klijentskom računalu, što olakšava mnogo zauzeti grafikonu API-ji i prilagodbu korisničkog sučelja. Povremeno na blogove i forume vidjet ćete ovo se nazivaju "tijek klijent" ili "klijent usmjereni tijek" od kod na klijentskom računalu je zadužen za prijavu i kod klijenta ima pristup token za davatelja usluga.

Kada se dobiva se davatelja tokena, mora se šalje aplikacije servisa za provjeru valjanosti. Na kraju tijeka klijent SDK ima token za aplikacije servisa, a sve zahtjeve za pozadinski automatski priložiti token. Programer možete i zadržati referenca token davatelja ako ih tako da odaberete.

## <a name="how-authorization-works"></a>Kako funkcionira autorizacije

Provjera autentičnosti za aplikaciju servisa / autorizacije izlaže nekoliko mogućnosti za **poduzeti kada zahtjev je autentičnost provjerena**. Prije koda primi zahtjev za dani, možete odrediti aplikacije servisa za provjerite je li zahtjev provjere autentičnosti i ako nije, odbiti i pokušati je korisnik prijaviti prije ponovnog pokušaja.

Jedan je imati neovlašten zahtjeva za preusmjeravanje na jedan od davatelja identiteta. U web-preglednik, to zapravo osvojio korisnika na novu stranicu. Međutim, mobilnog klijenta ne može biti preusmjereni na taj način, a Neprovjereni odgovore primit će je HTTP _401 Unauthorized_ odgovor. Uz to, prvi zahtjev za vaš klijent čini uvijek mora biti krajnja točka za prijavu, a zatim možete upućivati pozive na sve ostale API-ji. Ako pokušate drugi API poziva prije prijave u klijentu će primiti pogrešku.

Ako želite imati precizniji kontrolu nad kojim krajnje točke zahtijeva provjeru autentičnosti, možete odabrati "ne poduzmete (Omogući zahtjev)" Neprovjereni zahtjeva za. U ovom slučaju sve provjere autentičnosti odluke odgođena su u kodu aplikacije. To omogućuje vam dopustiti pristup određene korisnike na temelju pravila za prilagođene autorizacija.

## <a name="documentation"></a>Dokumentacija

Sljedeći vodiči za pokazati kako dodati provjere autentičnosti na svoje mobilne klijente pomoću aplikacije servisa za:

- [Dodavanje provjere autentičnosti za aplikaciju za iOS]
- [Dodavanje provjere autentičnosti Xamarin.iOS aplikacije]
- [Dodavanje provjere autentičnosti Xamarin.Android aplikacije]
- [Dodavanje provjere autentičnosti aplikacija za Windows]

Sljedeći vodiči za prikaz upute za konfiguriranje aplikacije servisa za odražava davatelja usluge provjere autentičnosti za različite:

- [Upute za konfiguriranje aplikacije da biste koristili prijava Azure Active Directory]
- [Upute za konfiguriranje aplikacije da biste koristili prijava za Facebook]
- [Upute za konfiguriranje aplikacije da biste koristili prijavu u Google]
- [Upute za konfiguriranje aplikacije da biste koristili Microsoftov Account prijava]
- [Upute za konfiguriranje aplikacije da biste koristili Twitter prijava]

Ako želite koristiti u sustavu identiteta osim one navedene Ovdje možete koristiti [Pretpregled podrška za prilagođene provjere autentičnosti na poslužitelju za .NET SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Dodavanje provjere autentičnosti za aplikaciju za iOS]: app-service-mobile-ios-get-started-users.md
[Dodavanje provjere autentičnosti Xamarin.iOS aplikacije]: app-service-mobile-xamarin-ios-get-started-users.md
[Dodavanje provjere autentičnosti Xamarin.Android aplikacije]: app-service-mobile-xamarin-android-get-started-users.md
[Dodavanje provjere autentičnosti aplikacija za Windows]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Upute za konfiguriranje aplikacije da biste koristili prijava Azure Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Upute za konfiguriranje aplikacije da biste koristili prijava za Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Upute za konfiguriranje aplikacije da biste koristili prijavu u Google]: app-service-mobile-how-to-configure-google-authentication.md
[Upute za konfiguriranje aplikacije da biste koristili Microsoftov Account prijava]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Upute za konfiguriranje aplikacije da biste koristili Twitter prijava]: app-service-mobile-how-to-configure-twitter-authentication.md
