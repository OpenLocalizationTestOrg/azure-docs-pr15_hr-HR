<properties
    pageTitle="Korisnicima dodijeliti prilagođenu domenu u servisu Azure Active Directory | Microsoft Azure"
    description="Upute za popunjavanje prilagođene domene u Azure Active Directory s korisničkim računima."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="assign-users-to-a-custom-domain"></a>Korisnicima dodijeliti prilagođenu domenu

Nakon dodavanja prilagođene domene za Azure Active Directory, morate dodati korisničke račune za tu domenu da bi mogli početi provjere autentičnosti ih.

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>Korisnici koji se sinkroniziraju u iz imenika na mreži tvrtke

Ako ste već postavili vezu između lokalnih servisa Active Directory i Azure Active Directory, sinkronizacije možete popuniti račune. Dodatne informacije o tome kako Azure Active Directory će se sinkronizirati s lokalnim Active Directory potražite u članku [integriranje vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Korisnici dodali i upravlja u oblaku

Da biste promijenili domenu za postojeći korisnički račun:

1.  Otvorite portal za Azure klasični pomoću računa koji je globalni administrator ili administrator korisnicima.

2.  Otvorite direktorija.

3.  Odaberite karticu **korisnika** .

4.  Odaberite korisnika s popisa.

5.  Promjena domene korisnika, a zatim odaberite **Spremi**.

To se može učiniti i pomoću web-mjesto [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) ili u okvir za [API na grafikonu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Odaberite prilagođenu domenu, prilikom stvaranja novog korisnika

1.  Otvorite portal za Azure klasični pomoću računa koji je globalni administrator ili administrator korisnicima.

2.  Otvorite direktorija.

3.  Odaberite karticu **korisnika** .

4.  Na naredbenoj traci odaberite **Dodaj**.

5.  Kada dodate korisničko ime, na popisu domena odaberite prilagođenu domenu.

6.  Odaberite **Spremi**.

## <a name="next-steps"></a>Daljnji koraci

-   [Korištenje prilagođenih naziva domena za pojednostaviti prijave za korisnike](active-directory-add-domain.md)

-   [Upravljanje nazivima prilagođene domene](active-directory-add-manage-domain-names.md)

-   [Informirajte se o koncepti upravljanja domene u Azure AD](active-directory-add-domain-concepts.md)
