<properties
    pageTitle="B2C Azure Active Directory: Pregled | Microsoft Azure"
    description="Razvoj aplikacija za korisničke dostupnog pomoću Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Azure Active Directory B2C: Registracija za Office i prijavite se u koje korisnici u aplikacija

Azure Active Directory B2C je rješenje upravljanja identiteta sveobuhvatan oblaka za korisničke dostupnog web i mobilne aplikacije. Je vrlo dostupna globalni servis koji mijenja veličinu da biste stotine milijuna potrošača identiteta. U komponenti na sigurne platforme poslovne klase Azure Active Directory B2C zadržava aplikacija, poslovanja i na koje korisnici zaštićeni.

U prošlosti, razvojnim inženjerima koji su željeli prijavite se i prijavite se u koje korisnici u svojim aplikacijama bi ste napisali vlastite kod. A želite imati koriste lokalne baze podataka ili sustavi za spremanje korisnička imena i lozinke. Azure Active Directory B2C nudi razvojnim inženjerima bolji način integracije upravljanje identitetom consumer u svojim aplikacijama uz pomoć sigurne, standarde platforme i bogatog skupa extensible pravila. Kada koristite Azure Active Directory B2C, vaše koje korisnici možete se prijaviti za aplikacije pomoću svoje postojeće računi društvenih (Facebook, Google, Amazon, LinkedIn) ili tako da stvorite novu vjerodajnice (adresu e-pošte i lozinku, ili korisničko ime i lozinka); ne možemo poziva na potonjem "lokalni računi."

## <a name="get-started"></a>Početak rada

Da biste sastavili aplikacija koja prihvaća potrošača registracije i prijavite se, koje ćete prvi morate registrirati aplikaciju putem programa Azure Active Directory B2C klijent. Dobiti vlastiti klijentu slijedeći korake navedene u [Stvori klijent za Azure AD B2C](active-directory-b2c-get-started.md).

Aplikacija na temelju servisa Azure Active Directory B2C možete napisati odabirom slanja poruka protokol izravno pomoću [OAuth 2.0](active-directory-b2c-reference-protocols.md#oauth2-authorization-code-flow) ili [Otvaranje povezivanje ID](active-directory-b2c-reference-protocols.md#openid-connect-sign-in-flow), ili pomoću naš biblioteke da biste učinili posla za vas. Odaberite svoju platformu omiljene u tablici u nastavku i započinje s radom.

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Što je novo

Provjerite natrag ovdje često Saznajte više o buduće promjene Azure Active Directory B2C. Ne možemo ćete i tweet o sva ažuriranja pomoću @AzureAD.

- Informirajte se o naš [okvir extensible pravila](active-directory-b2c-reference-policies.md) i vrste pravila koja možete stvoriti i koristiti u aplikacije.
- Knjižna oznaka naš [blog o servisima](https://blogs.msdn.microsoft.com/azureadb2c/) za obavijesti na problemi sa servisom manji, ažuriranja, status i mitigations. I dalje praćenje [nadzorne ploče Azure status](https://azure.microsoft.com/status/) kao i.
- Trenutni [servisna ograničenja, ograničenja i ograničenja](active-directory-b2c-limitations.md).
- Na kraju, [uzorak koda](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c) pomoću Azure AD B2C ASP.NET Core.

## <a name="how-to-articles"></a>Praktični članci

Saznajte kako pomoću značajke Azure Active Directory B2C:

- Konfiguriranje [servisa Facebook](active-directory-b2c-setup-fb-app.md), [Google +](active-directory-b2c-setup-goog-app.md), [Microsoftov račun](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md)i [LinkedIn](active-directory-b2c-setup-li-app.md) račune za korištenje u korisničke dostupnog aplikacija.
- [Korištenje prilagođene atribute za prikupljanje informacija o vašem koje korisnici](active-directory-b2c-reference-custom-attr.md).
- [Omogućivanje Azure višestruka provjera autentičnosti u korisničke dostupnog aplikacija](active-directory-b2c-reference-mfa.md).
- [Postavljanje samostalno ponovno postavljanje lozinke za vaše koje korisnici](active-directory-b2c-reference-sspr.md).
- [Prilagodite izgled i dojam registracije, prijavite se, i drugih stranica potrošača dostupnog](active-directory-b2c-reference-ui-customization.md) koja su služila po Azure Active Directory B2C.
- [Korištenje na Azure Active Directory grafikonu API programski stvaranje, čitanje, ažuriranje, i brisanje koje korisnici](active-directory-b2c-devquickstarts-graph-dotnet.md) u klijentu za Azure Active Directory B2C.

## <a name="next-steps"></a>Daljnji koraci

Ove veze će biti od koristi za proučavanje servisa detaljniju:

- Vidjeti [podatke o cijenama Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
- Zatražite pomoć na hrpu prelijevanje pomoću [azure active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) ili [adal](http://stackoverflow.com/questions/tagged/adal) oznake.
- Pošaljite nam svoje misli pomoću [Govorne korisnika](https://feedback.azure.com/forums/169401-azure-active-directory/)– rado bismo ih čuli! Korištenje izraza "AzureADB2C:" u naslovu objavu da bismo mogli pronaći.
- Pregledajte [Azure AD B2C protokol referencu](active-directory-b2c-reference-protocols.md).
- Pregledajte [Azure AD B2C tokena referencu](active-directory-b2c-reference-tokens.md).
- Pročitajte [Najčešća pitanja B2C Azure Active Directory](active-directory-b2c-faqs.md).
- [Datoteke podrške zahtjeva za Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Sigurnosna ažuriranja za naši proizvodi

Pozivamo vas da biste dobili obavijesti kada doći do sigurnošću potražite na [toj stranici](https://technet.microsoft.com/security/dd252948) i pretplata na upozorenja savjeta za sigurnost.
