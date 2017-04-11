<properties
   pageTitle="Trenutni pretpregled ograničenja za suradnju Azure Active Directory B2B | Microsoft Azure"
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
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Azure AD B2B suradnju pretpregled: trenutni pretpregled ograničenja

- Višestruka provjera autentičnosti (MFA) nije podržano na vanjskim korisnicima. Ako, na primjer, ako Contoso sadrži MFA, ali Partner tvrtke ili ustanove ne, pa Partner tvrtke ili ustanove korisnici nije moguće odobriti MFA kroz B2B suradnje.
- Moguće samo putem CSV; su pozivnice pojedinačne pozivnice i pristup API-JA nisu podržane.
- Samo Azure AD globalni administratori mogu prenositi .csv datoteke.
- Pozivnice za korisničke adrese e-pošte (primjerice hotmail.com, Gmail.com ili comcast.net) trenutno nije podržano.
- Pristup vanjskim korisnicima lokalnog aplikacija ne testirati.
- Vanjski korisnici ne automatski očistiti kada konkretni korisnik izbriše s njihovim direktorija.
- Pozivnice za popise za raspodjelu nisu podržani.
- Moguće je maksimalno 2000 zapisa prenijeti putem CSV.

## <a name="related-articles"></a>Povezani članci
Pregled naše ostale članaka na Azure AD B2B suradnje:

- [Što je Azure AD B2B suradnje?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Kako funkcionira](active-directory-b2b-how-it-works.md)
- [Detaljni vodič](active-directory-b2b-detailed-walkthrough.md)
- [Referenca za oblikovanje CSV datoteke](active-directory-b2b-references-csv-file-format.md)
- [Tokena oblik vanjskog korisnika](active-directory-b2b-references-external-user-token-format.md)
- [Promjene atributa objekt vanjskog korisnika](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
