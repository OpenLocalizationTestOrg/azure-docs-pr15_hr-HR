<properties
    pageTitle="Registracija aplikacije 2.0 | Microsoft Azure"
    description="Kako registrirati aplikacije s Microsoftom za omogućivanje prijave i pristup Microsoftove servise pomoću krajnju točku 2.0"
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

# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Kako registrirati aplikacije s krajnju točku 2.0

Da biste sastavili aplikaciju koja prihvaća MSA & Azure AD prijavu, najprije morate da biste registrirali aplikaciju s Microsoftom.  Trenutno ih nećete moći koristiti postojeće aplikacija možda imaju Azure AD ili MSA – morat ćete stvoriti potpuno novi.

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

## <a name="visit-the-microsoft-app-registration-portal"></a>Posjetite Registracija portal za aplikacije Microsoft
Prvi stvari-dođite do [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Ovo je novi portal Registracija aplikacije kojima možete upravljati aplikacija sustava Microsoft.

Prijavite se pomoću ili osobnog ili ili školske Microsoftova računa.  Ako nemate ili, prijavite se za novi osobnog računa. Nastaviti, nećete trebati dugo – ne možemo ćete pričekati u nastavku.

Jeste li dovršili? Trebali biste sada se gledate popis aplikacija Microsoft koji se vjerojatno prazan.  Pogledajmo to promijeniti.

Kliknite **Dodaj aplikaciju**, a zatim dajte naziv.  Na portalu će dodijeliti aplikacije globalno jedinstveni Id aplikacije koja će se koristiti u nastavku kod.  Ako aplikacije sadrži poslužiteljsko komponente koje treba pristupna tokena za pozivanja API-ji (zamislite da je: Office, Azure ili vlastite API na webu), želite stvoriti kao i u **Aplikaciji tajna** ovdje.
<!-- TODO: Link for app secrets -->

Zatim dodajte platforme koji će koristiti aplikaciju.

- Temelji u web-aplikacijama omogućuju **Preusmjeravanje URI** gdje se mogu slati poruke za prijavu.
- Mobilne aplikacije kopirajte dolje uri preusmjeravanje zadani automatski stvara za vas.

Po želji možete prilagoditi izgled i dojam stranici za prijavu u odjeljku profil.  Provjerite jeste li prije no što isprobate kliknite **Spremi** .

> [AZURE.NOTE] Prilikom stvaranja aplikacije korištenjem [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)aplikacija će biti registriran u početnom klijentu za račun koji koristite za prijavu na portal sustava.  To znači da možete ne Registracija aplikacije u klijentu za Azure AD pomoću osobnog Microsoftova računa.  Ako želite izričito Registracija aplikacije u određenom klijentu, prijavite se pomoću računa koji je izvorno stvorio u klijentu.

## <a name="build-a-quick-start-app"></a>Stvoriti aplikaciju za brzi početak rada
Sad kad ste aplikaciju Microsoft, možete Provedite jedan od naše 2.0 vodiče za brzi početak rada.  Evo nekoliko preporuka:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]
