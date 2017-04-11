<properties
    pageTitle="Izvješćivanje o pogreškama obavijesti Azure Active Directory"
    description="Kako koristiti Azure Active Directory izvješćivanja obavijesti za prijavu sumnjivih dodaci."
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-reporting-notifications"></a>Izvješćivanje o pogreškama obavijesti Azure Active Directory

## <a name="what-reports-generate-email-notifications"></a>Što izvješća generiranje obavijesti e-poštom

Trenutačno samo nepravilne prijava okidača izvješća aktivnosti e-pošte obavijesti.

## <a name="what-is-an-irregular-sign-in"></a>Što je "Nepravilne znak u"?

Nepravilni znak dodataka su oni koje identificirali po naš strojnog učenja algoritama na temelju uvjet "nemoguće putovanja" u kombinaciji s anomalous mjesto za prijavu i uređaja. To može značiti da haker sadrži je pokušaja prijave pomoću ovog računa.

## <a name="who-receives-the-email-notifications"></a>Tko prima obavijesti o e-pošte?

E-pošte šalje se sve globalni administratori koji ste dodijelili licencu za Active Directory Premium. Da biste je isporučeno, ne možemo poslati administratorima zamjensku adresu e-pošte kao i. Administratori moraju sadržavati aad-alerts-noreply@mail.windowsazure.com u svoj popis sigurnih pošiljatelja da ih ne bi promakli e-pošte.

## <a name="how-often-are-these-emails-sent"></a>Koliko se često te poruke e-pošte koje se šalju?

Ako 10 nove nepravilne prijavu aktivnosti pojavljuju se u zadnjih 30 dana ili jer je poslana posljednje e-pošte, ovisno o tome što je manji šalje se e-pošte.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Kako pristupiti izvješća koji se spominju u e-poštu?

Prilikom klika na vezu, bit će preusmjereni na stranici s izvješćem unutar Azure klasični portal. Da biste pristupili izvješća, morate biti oba:

- Administrator ili ko administrator pretplate Azure

- Globalni administrator u imeniku i dodijelili licencu za Active Directory Premium. Dodatne informacije potražite u članku [izdanja Azure Active Directory](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Mogu li isključiti te poruke e-pošte?

Da, da biste isključili obavijesti vezanih uz anomalous znak dodataka unutar Azure klasični portal, kliknite **Konfiguriraj**, a zatim **onemogućene** u odjeljku **obavijesti** .

## <a name="whats-next"></a>Daljnji koraci
- Zanima li dostupnih izvješća koje sigurnost, nadzora i aktivnosti? Pogledajte [Azure AD sigurnost, nadzor, i izvješća o aktivnosti](active-directory-view-access-usage-reports.md)
- [Uvod u Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Dodavanje tvrtke brendiranje prijava i ploča za pristup stranicama](active-directory-add-company-branding.md)
