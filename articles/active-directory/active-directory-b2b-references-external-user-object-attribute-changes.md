<properties
   pageTitle="Promjene atributa vanjskog korisnika objekt za pregled suradnje Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B podržava više tvrtke odnosa omogućivanjem poslovni partneri za selektivno pristup tvrtke aplikacija"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Azure AD B2B suradnju pretpregled: promjene atributa objekt vanjskog korisnika

Svakog korisnika u imeniku programa Azure AD predstavlja korisnički objekt. Korisnički objekt u Azure AD prolazi kroz promjene atributa u različite faze B2B suradnje Pozovi-iskoristili tijek. Na korisnički objekt koji predstavlja partnera korisnika u imeniku sadrži atribute koje promijeniti iskoristi vrijeme kada partnera klika na vezu u pozivnicu e-poštom. Konkretno:

- Atributi **SignInName** i **AltSecId** bili popunjeni
- Promjena atribut **riješiti problem** s korisnikovo Glavno ime (user_fabrikam.com#EXT#@contoso.com) ime za prijavu(user@fabrikam.com)

Praćenje tih atributa u Azure AD može pomoći pri otklanjanju poteškoća s hoće li korisnik partnera sadrži aktivacije njihove B2B pozivnicu za suradnju.

## <a name="related-articles"></a>Povezani članci
Pregled naše ostale članaka na Azure AD B2B suradnje:

- [Što je Azure AD B2B suradnje?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Kako funkcionira](active-directory-b2b-how-it-works.md)
- [Detaljni vodič](active-directory-b2b-detailed-walkthrough.md)
- [Referenca za oblikovanje CSV datoteke](active-directory-b2b-references-csv-file-format.md)
- [Tokena oblik vanjskog korisnika](active-directory-b2b-references-external-user-token-format.md)
- [Trenutni pretpregled ograničenja](active-directory-b2b-current-preview-limitations.md)
- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
