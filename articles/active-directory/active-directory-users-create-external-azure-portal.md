<properties
    pageTitle="Dodavanje korisnika iz druge mape ili partnerske tvrtke u pretpregledu Azure Active Directory | Microsoft Azure"
    description="U članku se objašnjava kako dodavati korisnike ili promijeniti korisničke informacije u Azure Active Directory, uključujući vanjskih i goste korisnike."
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

# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory-preview"></a>Dodavanje korisnika iz druge mape ili partner tvrtke u pretpregledu Azure Active Directory

> [AZURE.SELECTOR]
- [Portal za Azure](active-directory-users-create-external-azure-portal.md)
- [Azure klasični portala](active-directory-create-users-external.md)

U ovom se članku objašnjava kako dodavati korisnike iz drugih direktorija u pretpregledu Azure Active Directory (Azure AD) ili od partnera tvrtki. [Novosti u pretpregledu?](active-directory-preview-explainer.md) Informacije o dodavanju novih korisnika u tvrtki ili ustanovi i dodajte korisnike koji imaju Microsoftovi računi potražite u članku [Dodavanje novog korisnika u Azure Active Directory](active-directory-users-create-azure-portal.md). Dodane korisnike Nemate administratorske dozvole po zadanom, no možete dodijeliti uloge na njih u bilo kojem trenutku.

## <a name="add-a-user"></a>Dodavanje korisnika

1.  Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2.  Odaberite **Dodatne usluge**, unesite **korisnici i grupe** u tekstni okvir, a zatim pritisnite **Enter**.

    ![Upravljanje korisnicima za otvaranje](./media/active-directory-users-create-external-azure-portal/create-users-user-management.png)

3.  Na plohu **korisnici i grupe** odaberite **korisnika**, a zatim odaberite **Dodaj**.

    ![Odabir naredbe Dodaj](./media/active-directory-users-create-external-azure-portal/create-users-add-command.png)

4. Na plohu **korisnika** Unesite zaslonsko ime u **nazivu** i korisničko ime za prijavu **korisničko ime**.

5. Kopirajte ili u suprotnom Imajte na umu generirani korisničku lozinku tako da ga možete unijeti korisnika po dovršetku ovog postupka.

6. Po želji odaberite **profil** da biste najprije dodajte korisnike i prezime, poslovna titula i naziv odjela.
    
    ![Otvaranje korisničkog profila](./media/active-directory-users-create-external-azure-portal/create-users-user-profile.png)

    - Odaberite **grupe** korisnika dodao u jednu ili više grupa.

        ![Dodavanje korisnika u grupe](./media/active-directory-users-create-external-azure-portal/create-users-user-groups.png)

    - Odaberite **ulogu tvrtke ili ustanove** da biste s popisa **uloge** korisnika dodijeliti uloge. Dodatne informacije o ulogama korisnika i administratora potražite u članku [Dodjela administratorskih uloga u Azure AD](active-directory-assign-admin-roles.md).

        ![Dodjela korisnika u ulogu](./media/active-directory-users-create-external-azure-portal/create-users-assign-role.png)

7. Odaberite **Stvori**.

8. Sigurno distribucija generirani lozinku da biste novom korisniku da bi se možete prijaviti korisnika.

> [AZURE.IMPORTANT] Ako vaša tvrtka ili ustanova koristi više domena, informacije o sljedeći problemi prilikom dodavanja korisnički račun:
>
> - Korisnički računi s na istom korisnikovo Glavno ime (UPN) svim domenama, **prvi** dodati, na primjer, geoffgrisso@contoso.onmicrosoft.com, **Slijedi** geoffgrisso@contoso.com.
> - **Nemoj** dodati geoffgrisso@contoso.com prije nego što dodate geoffgrisso@contoso.onmicrosoft.com. Ovim redoslijedom važno je, a može biti nezgodno da biste poništili.

Ako promijenite informacije za korisnika čiji identitet sinkronizira se s vašeg lokalnog servisa Active Directory, ne možete promijeniti korisničke informacije na portalu za Azure klasični. Da biste promijenili podatke o korisniku, pomoću alata za lokalni Active Directory upravljanje.


## <a name="whats-next"></a>Daljnji koraci

- [Dodavanje korisnika](active-directory-users-create-azure-portal.md)
- [Ponovno postavljanje korisničke lozinke na novom portalu Azure](active-directory-users-reset-password-azure-portal.md)
- [Dodijelite uloge korisnika u Azure AD](active-directory-users-assign-role-azure-portal.md)
- [Promijenite podatke o korisniku rad](active-directory-users-work-info-azure-portal.md)
- [Upravljanje korisničkim profilima](active-directory-users-profile-azure-portal.md)
- [Brisanje korisnika u Azure AD](active-directory-users-delete-user-azure-portal.md)
