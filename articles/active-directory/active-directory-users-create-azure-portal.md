<properties
    pageTitle="Dodavanje novih korisnika pretpregleda Azure Active Directory | Microsoft Azure"
    description="U članku se objašnjava kako dodati novi korisnici ili promijeniti korisničke informacije u Azure Active Directory."
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


# <a name="add-new-users-to-azure-active-directory-preview"></a>Dodavanje novih korisnika pretpregleda Azure Active Directory

> [AZURE.SELECTOR]
- [Portal za Azure](active-directory-users-create-azure-portal.md)
- [Azure klasični portal](active-directory-create-users.md)

U ovom se članku objašnjava kako dodati nove korisnike u tvrtki ili ustanovi u pretpregledu Azure Active Direstory (Azure AD). [Novosti u pretpregledu?](active-directory-preview-explainer.md)

1.  Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2.  Odaberite **Dodatne usluge**, unesite **korisnici i grupe** u tekstni okvir, a zatim pritisnite **Enter**.

    ![Upravljanje korisnicima za otvaranje](./media/active-directory-users-create-azure-portal/create-users-user-management.png)

3.  Na plohu **korisnici i grupe** odaberite **sve korisnike**, a zatim odaberite **Dodaj**.

    ![Odabir naredbe Dodaj](./media/active-directory-users-create-azure-portal/create-users-add-command.png)

4.  Unesite detalje o korisniku, kao što su **ime** i **korisničko ime**. Dio naziva domene korisničko ime mora biti Početna zadani naziv domene "foo.onmicrosoft.com" naziv domene ili naziv provjereno, koje nisu pridružene domene kao što su "contoso.com."

5. Kopirajte ili u suprotnom Imajte na umu generirani korisničku lozinku tako da ga možete unijeti korisnika po dovršetku ovog postupka.

6. Po želji možete otvoriti i unesite podatke u plohu **profila** , plohu **grupe** ili plohu **uloga direktorija** za korisnika. Dodatne informacije o ulogama korisnika i administratora potražite u članku [Dodjela administratorskih uloga u Azure AD](active-directory-assign-admin-roles.md).

7.  Na plohu **korisnika** odaberite **Stvori**.

8. Sigurno distribucija generirani lozinku da biste novom korisniku da bi se možete prijaviti korisnika.

## <a name="whats-next"></a>Daljnji koraci

- [Dodavanje vanjskog korisnika](active-directory-users-create-external-azure-portal.md)
- [Ponovno postavljanje korisničke lozinke na novom portalu Azure](active-directory-users-reset-password-azure-portal.md)
- [Promijenite podatke o korisniku rad](active-directory-users-work-info-azure-portal.md)
- [Upravljanje korisničkim profilima](active-directory-users-profile-azure-portal.md)
- [Brisanje korisnika u Azure AD](active-directory-users-delete-user-azure-portal.md)
- [Dodijelite uloge korisnika u Azure AD](active-directory-users-assign-role-azure-portal.md)
