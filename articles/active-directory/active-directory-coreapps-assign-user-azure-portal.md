<properties
    pageTitle="Dodjela korisnika ili grupu enterprise aplikacije u pretpregledu Azure Active Directory | Microsoft Azure"
    description="Kako odabrati enterprise aplikacije da biste dodijelili korisnika ili grupu ga u servisu Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory-preview"></a>Dodjela korisnika ili grupu enterprise aplikacije u pretpregledu Azure Active Directory

Jednostavno je Dodjela korisnika ili grupu aplikacijama u pretpregledu Azure Active Directory (Azure AD). [Novosti u pretpregledu?](active-directory-preview-explainer.md) Morate imati odgovarajuće dozvole za upravljanje enterprise aplikacije. U trenutni pretpregled, morate biti globalni administrator za direktorij.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Kako dodijeliti korisničkog pristupa enterprise aplikacije?

1. Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2. Odaberite **više usluga**, unesite Azure Active Directory u tekstni okvir, a zatim pritisnite **Enter**.

3. Na na * *Azure Active Directory - *direktorija* ** plohu (to jest, za Azure AD plohu za direktorij upravljate), odaberite **Enterprise aplikacije **.

    ![Otvaranje Enterprise aplikacije](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)

4. Na plohu **aplikacijama** odaberite **sve aplikacije**. Vidjet ćete popis aplikacija možete upravljati.

5. Na plohu **aplikacijama – sve aplikacije** , odaberite aplikacije.

6. Na ***appname*** plohu (to jest, na plohu pod nazivom aplikaciju odabrane u naslovu) odaberite **korisnici i grupe**.

    ![Odabir naredbe sve aplikacije](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)

7. Na ***appname*** **-korisnika i grupa dodjele** plohu, odaberite naredbu **Dodaj** .

8. Na plohu **Dodavanje dodjele** odaberite **korisnici i grupe**.

    ![Dodjela korisnika ili grupu u aplikaciju](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)

9. Na plohu **korisnici i grupe** odaberite jednu ili više korisnika ili grupa s popisa, a zatim odaberite gumb **Odaberi** pri dnu zaslona u plohu.

10. Na plohu **Dodavanje dodjele** odaberite **ulogu**. Zatim na plohu **Odaberite ulogu** odaberite uloge da biste primijenili na odabrane korisnike ili grupe, a zatim odaberite gumb **u redu** pri dnu zaslona u plohu.

11. Na plohu **Dodavanje dodjele** odaberite gumb **Dodjela** pri dnu zaslona u plohu. Dodijeljene korisnike ili grupe imati dozvole definira odabranoj ulozi za enterprise aplikacije.

## <a name="next-steps"></a>Daljnji koraci

- [Pogledajte sve svoje grupe](active-directory-groups-view-azure-portal.md)
- [Uklanjanje korisnika ili grupu dodjele enterprise aplikacije](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Onemogućavanje korisnika s znak dodatke za enterprise aplikacije](active-directory-coreapps-disable-app-azure-portal.md)
- [Promijenite naziv ili logotip enterprise aplikacije](active-directory-coreapps-change-app-logo-user-azure-portal.md)
