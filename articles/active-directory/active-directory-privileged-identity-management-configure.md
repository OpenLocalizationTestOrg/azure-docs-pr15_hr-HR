<properties
    pageTitle="Upravljanje identitetom Azure AD povlaštene | Microsoft Azure"
    description="Tema na kojem se objašnjava što je upravljanje Azure AD povlaštene identiteta i kako koristiti PIM za poboljšanje sigurnosti oblaka."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management"></a>Upravljanje identitetom povlaštene Azure AD

Azure Active Directory (AD) povlaštene upravljanje identitetom, možete upravljati kontrole, i nadzor pristupa unutar tvrtke ili ustanove. To obuhvaća pristup resursa u Azure AD i druge Microsoftove internetske servise kao što su Office 365 ili Microsoft Intune.  

> [AZURE.NOTE] Povlaštene upravljanje identitetom dostupan je samo sa Premium P2 izdanje sustava Azure Active Directory. Dodatne informacije potražite u članku [izdanja Azure Active Directory](active-directory-editions.md).

Tvrtke i ustanove želite smanjiti broj osobe koje imaju pristup sigurne informacije ili resursa, jer koji se smanjuju izgledi zlonamjeran korisnik početak da access. Međutim, korisnici i dalje potrebna za izvršavanje povlaštene operacije u aplikacijama za Azure, Office 365 ili SaaS. Tvrtke i ustanove dati pristup za povlaštene korisnike u Azure AD bez nadzor što ti korisnici rade s njihovim administratorskim ovlastima. Upravljanje identitetom Azure AD povlaštene pomaže da biste riješili taj rizik.  

Upravljanje identitetom Azure AD povlaštene pomaže vam:  

- U odjeljku korisnike koji su administratori Azure AD
- Omogućivanje na zahtjev, "samo u vremenu" Administrativni pristup Microsoft Online Services kao što su Office 365 i Intune
- Izvješća o administratorski pristup povijesti i promjene se pridružuju dodjele administratora
- Primati upozorenja o pristupu povlaštene ulogu

Azure AD povlaštene upravljanje identitetom možete upravljati ugrađene uloge tvrtke ili ustanove Azure AD, uključujući:  

- Globalni Administrator
- Administrator naplate
- Administrator servisa  
- Administrator za korisnika
- Administrator lozinki

## <a name="just-in-time-administrator-access"></a>Samo u vrijeme administratorski pristup

Prethodno, korisnik može dodijeliti administratorsku ulogu kroz Azure klasični portal ili komponente Windows PowerShell. Zbog toga korisnik postaje sustava **Trajna administrator**, uvijek aktivan u dodijeljena uloga. Upravljanje identitetom Azure AD povlaštene predstavlja pojam **uvjete administrator**. Administratori uvjete mora biti korisnika kojima pristupaju povlaštene sada i zatim, ali ne svaki dan. Uloga nije aktivna dok korisnik je potreban pristup, oni dovršavanje postupka sustava aktivacije i kako postati administrator aktivni unaprijed određenim vremenskog razdoblja.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Omogućivanje povlaštene upravljanje identitetom za direktorija

Možete početi koristiti Azure AD povlaštene upravljanje identitetom [Azure portal](https://portal.azure.com/).

>[AZURE.NOTE] Morate biti globalni administrator s računa tvrtke ili ustanove (, na primjer, @yourdomain.com), ne Microsoftova računa (Ako, na primjer, @outlook.com), da biste omogućili Azure AD povlaštene upravljanje identitetom direktorija.

1. Prijavite se na [portal za Azure](https://portal.azure.com/) kao globalni administrator imenik.
2. Ako tvrtka ili ustanova ima više od jedne direktorija, odaberite svoje korisničko ime u gornjem desnom kutu Azure portal. Odaberite imenik kojemu će koristiti Azure AD povlaštene upravljanje identitetom.
3. Odaberite **Dodatne servise** i koristiti tekstni okvir filtar da biste potražili **Azure AD povlaštene upravljanje identitetom**.
4. Provjera **Prikvači na nadzornu ploču** , a zatim kliknite **Stvori**. Otvorit će se aplikacija povlaštene upravljanje identitetom.

Ako ste prvu osobu da biste koristili Azure AD povlaštene upravljanje identitetom u direktoriju, zatim [Čarobnjak za sigurnost](active-directory-privileged-identity-management-security-wizard.md) vodit će vas kroz početne dodjele iskustvo. Nakon toga ćete automatski postati prvi **administratora sigurnosti** i **povlaštene ulogu administratora** direktorija.

Samo povlaštene ulogu administratora možete upravljati pristupom za drugi administratori. Možete [omogućiti drugim korisnicima u PIM upravljati](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-dashboard"></a>Nadzorna ploča za povlaštene upravljanje identitetom

Upravitelj identiteta Azure AD povlaštene omogućuje vam važne informacije kao što su nadzorna ploča:

- Upozorenja koji pokazuju prilika za poboljšanje sigurnosti
- Broj korisnika koji su dodijeljene ulogama povlaštene  
- Broj uvjete i trajna administratori
- Pregledi stalnog pristupa

![Nadzorna ploča PIM - snimka][2]

## <a name="privileged-role-management"></a>Upravljanje ulogama povlaštene

Upravljanje identitetom Azure AD povlaštene, možete upravljati administratori dodavanjem ili uklanjanjem trajna ili uvjete administratori za svaku ulogu.

![Administratori Dodaj/Ukloni PIM - snimka][3]

## <a name="configure-the-role-activation-settings"></a>Konfiguriranje postavki za aktivaciju uloga

Pomoću [postavki uloga](active-directory-privileged-identity-management-how-to-change-default-settings.md) možete konfigurirati svojstva aktivaciju uvjete uloga uključujući:

- Trajanje razdoblja aktivaciju uloga
- Aktivacija obavijesti uloga
- Informacije korisnika potrebne za pružanje tijekom procesa aktivacije servisa uloga  

![Snimka zaslona postavke - administrator aktivacija – PIM][4]

Imajte na umu da na slici, onemogućene su gumbi za **Višestruke provjere autentičnosti** . Za određene, Visoko ovlasti uloge tražimo MFA za heightened zaštitu.

## <a name="role-activation"></a>Aktivacija uloga  

Da biste [aktivirali ulogu](active-directory-privileged-identity-management-how-to-activate-role.md), administrator uvjete zahtjeve u vrijeme vezane "aktivaciju" uloge. Aktivacija možete zatražio koristeći **aktiviraj moj uloga** u Azure AD povlaštene upravljanje identitetom.

Administrator koji želi da biste aktivirali ulogu mora pokrenuti Azure AD povlaštene upravljanje identitetom na portalu za Azure.

Aktivacija uloga je moguće prilagoditi. U odjeljku postavke PIM možete odrediti duljinu sustava aktivacije i što informacije administrator potrebne za pružanje da biste aktivirali ulogu.

![PIM administrator zahtjev uloga aktivacija – snimka][5]

## <a name="review-role-activity"></a>Pregledavanje aktivnosti uloga

Da biste pratili kako zaposlenicima i administratore koristite povlaštene uloge na dva načina. Prva mogućnost koristi [povijest nadzora](active-directory-privileged-identity-management-how-to-use-audit-log.md). Povijest nadzora zapisnika evidentiranja promjena u dodjele uloga povlaštene i povijest aktivaciju uloge.

![Povijest aktivaciju PIM - snimka][6]

Druga mogućnost je da biste postavili običnog [access pregledava](active-directory-privileged-identity-management-how-to-start-security-review.md). Te preglede programa access mogu izvršavati i dodijeljeni pregledavatelja (kao što je timsko Upravitelj) ili zaposlenike možete pregledati sami. To je najbolji način za praćenje koji i dalje potreban pristup, a koji više ne.


## <a name="next-steps"></a>Daljnji koraci
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
