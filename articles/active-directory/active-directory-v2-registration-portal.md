<properties
    pageTitle="Registracija aplikacije portala teme pomoći za | Microsoft Azure"
    description="Opis različite značajke na portalu za registraciju aplikacije Microsoft."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="app-registration-reference"></a>Referenca za registraciju aplikacija
Ovaj dokument sadrži kontekst i opise različite značajke koje se nalaze u Portal za registraciju aplikacije Microsoft [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Moje aplikacije
Ovaj popis sadrži sve aplikacija registriran za krajnju točku Azure AD 2.0.  Te aplikacije imate mogućnost da biste se prijavili u korisnike s obje osobni računi za Microsoftov račun i poslovne/školske računi iz servisa Azure Active Directory.  Da biste saznali više o krajnjoj 2.0 Azure AD, pogledajte naše [2.0 pregled](active-directory-appmodel-v2-overview.md).  Može se koristiti te aplikacije za integraciju s provjere autentičnosti krajnje račun za Microsoft `https://login.live.com`.

## <a name="live-sdk-applications"></a>Live SDK aplikacije
Ovaj popis sadrži sve aplikacije registrirani za korištenje isključivo pomoću Microsoftova računa.  Nije omogućeno za Azure Active Directory neće.  To je gdje ćete pronaći sve aplikacije koje ste prethodno registrirani s portala za razvojne inženjere MSA na `https://account.live.com/developers/applications`.  Sve funkcije koju ste prethodno stvorili pri `https://account.live.com/developers/applications` sada može izvršiti novom portalu `https://apps.dev.microsoft.com`.  Ako imate dodatnih pitanja o aplikacija Microsoftov račun, obratite nam se.

## <a name="application-secrets"></a>Tajne aplikacije
Aplikacija tajne su vjerodajnice koje omogućuju aplikacije da biste izvršili pouzdanog [Provjera autentičnosti klijenta](http://tools.ietf.org/html/rfc6749#section-2.3) s Azure AD.  U OAuth i povezivanje OpenID programa tajne aplikacije se obično naziva na `client_secret`.  U protokolu 2.0, bilo koju aplikaciju koja se dobiva sigurnosnog tokena na web-mjestu za moguće adresirati (pomoću programa `https` shema) morate koristiti u aplikaciji tajna da biste odredili sam Azure AD nakon otkup taj sigurnosni token.  Osim toga, sve nativni klijent te dobiva tokena na uređaj će biti nije dozvoljeno s da biste izvršili provjeru autentičnosti klijenta, možete koristiti u aplikaciji tajna da bi spriječio njegovo prostora za pohranu tajne u nepouzdanog okruženja.

Svaku aplikaciju mogu sadržavati dva valjana aplikacija tajne bilo kojem trenutku navedene u vremenu.  Zadržavanjem dva tajne imate ablilty za izvođenje periodičku ključa dinamične preko vaše aplikacije cijelo okruženje.  Kada se prenijela cijelosti aplikacije da biste novi tajna može brisanje starih tajna i dodjela novi.

Trenutačno samo dvije vrste aplikacija tajne dopušteno na portalu za registraciju aplikacije.  Odaberete **Stvaranje nove lozinke** stvaranje i spremanje dijeljena tajna u spremištu odgovarajući podataka koje možete koristiti u aplikaciji.  Odabir **Generiraj novi ključ par** stvorit će se novi javno/privatno ključa par koje se mogu preuzeti i koristiti za provjeru autentičnosti klijenta za Azure AD.

## <a name="profile"></a>Profil
U odjeljku profil portala za registraciju aplikacija može se koristiti za prilagođavanje stranica za svoju aplikaciju za prijavu.  Sada mogu mijenjati prijava stranice aplikacije logotip, uvjete URL servisa i izjavu o zaštiti privatnosti.  Logotip mora biti prozirne 48 x 48 ili 50 x 50 piksela slike u GIF, PNG ili JPEG datoteku koja je 15 KB ili manje.  Pokušajte mijenjanje vrijednosti i prikaz rezultata prijavi na stranici!

## <a name="live-sdk-support"></a>Podrška za uživo SDK
Kada omogućite "Live SDK podrška", sve tajne aplikacije stvorite dodijelit će se Resursi u oba sustava Azure AD i služi za pohranu podataka Microsoft Account.  Tako ćete omogućiti aplikacije za integraciju izravno sa servisom Microsoft Account (login.live.com).  Ako želite izraditi aplikaciju pomoću Microsoftova Account izravno (umjesto pomoću 2.0 krajnje Azure AD), provjerite je li Live podršku SDK omogućena.

Onemogućivanje Live SDK podrška će osigurati aplikaciji tajna samo napisan podatke Azure AD pohranu.  Azure AD podataka trgovine ugrađuje poslovne klase propisa kojih je da bi odgovarao standardima, kao što su SAVEZNOM usklađenosti.  Ako omogućite podršku za Live SDK aplikacije možda postigli usklađenost s nekim od ovih standarda.

Ako samo planirate koristiti 2.0 krajnje Azure AD, sigurno možete onemogućiti Live SDK podrška.

