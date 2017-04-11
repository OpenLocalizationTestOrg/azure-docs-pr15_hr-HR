<properties
    pageTitle="Pregled krajnjoj točki 2.0 | Microsoft Azure"
    description="Uvod u stvaranje aplikacija s Microsoftovim Account i Azure Active Directory prijavu."
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
    ms.date="09/27/2016"
    ms.author="dastrock"/>

# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Prijava Microsoftov Account & Azure AD korisnika u jednu aplikaciju

U prošlosti je proizvođač koji podržavaju Microsoftovi računi i Azure Active Directory je potreban za integraciju s dva zasebna sustava.  Ne možemo novu verziju API provjere autentičnosti koja omogućuje prijavu korisnika pomoću obje vrste računa u sustavu Azure AD sada uvedena.  Ovaj konvergira sustav za provjeru autentičnosti zove **krajnju točku 2.0**.  Krajnja točka 2.0 jedan jednostavna Integracija omogućuje vam dosegne ciljne skupine koji obuhvaća milijune korisnici s osobne i poslovne/školske računi.

Aplikacijama koje koristite krajnju točku 2.0 također može raditi REST API-ji iz [Programa Microsoft Graph](https://graph.microsoft.io) i [Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) pomoću bilo koje vrste računa.

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>Početak rada
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

Odaberite svoju platformu omiljene s popisa u nastavku da biste sastavili aplikacije pomoću naš Otvori izvor biblioteke i okviri.  Osim toga, možete koristiti naše OAuth 2.0 i povezivanje OpenID dokumentaciju protokol za slanje i primanje poruka protokol izravno bez korištenja biblioteku provjere autentičnosti.

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Što je novo
Ovdje konceptualnih informacija će biti korisno za razumijevanje što je i što nije moguće s krajnju točku 2.0.

- Saznajte koje [vrste aplikacija možete izraditi s krajnju točku 2.0](active-directory-v2-flows.md).
- Razumjeti [ograničenja, ograničenja i ograničenjima](active-directory-v2-limitations.md) s krajnju točku 2.0.
- Nedavno dodali smo podrška za [administratore ograničene opsega](active-directory-v2-scopes.md) i [OAuth2 klijent dodijeliti vjerodajnice](active-directory-v2-protocols-oauth-client-creds.md).  Ih isprobate!

## <a name="reference"></a>Referenca
Ove veze će biti od koristi za proučavanje platformu detaljniju:

- Sastavljanje 2016: [Uvod u Microsoft identiteta: Enterprise ocjene prijava za aplikacije](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- Zatražite pomoć na hrpu prelijevanje korištenje [azure active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) ili [adal](http://stackoverflow.com/questions/tagged/adal) oznake.
- [Referenca protokol 2.0](active-directory-v2-protocols.md)
- [Referenca za tokena 2.0](active-directory-v2-tokens.md)
- [Reference biblioteke 2.0](active-directory-v2-libraries.md)
- [Opsega i pristanak na krajnjoj točki 2.0](active-directory-v2-scopes.md)
- [Portal za registraciju za aplikaciju Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)
- [Office 365 REST API Reference](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [Microsoft Graph](https://graph.microsoft.io)