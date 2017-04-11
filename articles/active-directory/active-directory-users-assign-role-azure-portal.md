<properties
    pageTitle="Korisniku dodijeliti administratorske uloge u pretpregledu Azure Active Directory | Microsoft Azure"
    description="U članku se objašnjava kako promijeniti korisničke informacije administratora servisa Azure Active Directory"
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
    ms.date="09/12/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory-preview"></a>Korisniku dodijeliti administratorske uloge u pretpregledu Azure Active Directory

U ovom se članku objašnjava kako dodijeliti administratorske uloge korisnika u pretpregledu Azure Active Directory (Azure AD). [Novosti u pretpregledu?](active-directory-preview-explainer.md) Informacije o dodavanju novog korisnika u tvrtki ili ustanovi potražite u članku [Dodavanje novog korisnika u Azure Active Directory](active-directory-users-create-azure-portal.md). Dodane korisnike Nemate administratorske dozvole po zadanom, no možete dodijeliti uloge na njih u bilo kojem trenutku.

## <a name="assign-a-role-to-a-user"></a>Dodijelite uloge korisnika

1.  Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2.  Odaberite **Dodatne usluge**, unesite **korisnici i grupe** u tekstni okvir, a zatim pritisnite **Enter**.

    ![Upravljanje korisnicima za otvaranje](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3.  Na plohu **korisnici i grupe** odaberite **sve korisnike**.

    ![Otvaranje plohu za sve korisnike](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)

4. Na plohu **korisnicima i grupama – svi korisnici** odaberite korisnika s popisa.

5. Na plohu za odabranog korisnika, odaberite **ulogu direktorija**i dodjela korisnika u ulogu na popisu **direktorija uloge** . Dodatne informacije o ulogama korisnika i administratora potražite u članku [Dodjela administratorskih uloga u Azure AD](active-directory-assign-admin-roles.md).

      ![Dodjela korisnika u ulogu](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

6. Odaberite **Spremi**.


## <a name="whats-next"></a>Daljnji koraci

- [Dodavanje korisnika](active-directory-users-create-azure-portal.md)
- [Ponovno postavljanje korisničke lozinke na novom portalu Azure](active-directory-users-reset-password-azure-portal.md)
- [Promijenite podatke o korisniku rad](active-directory-users-work-info-azure-portal.md)
- [Upravljanje korisničkim profilima](active-directory-users-profile-azure-portal.md)
- [Brisanje korisnika u Azure AD](active-directory-users-delete-user-azure-portal.md)
