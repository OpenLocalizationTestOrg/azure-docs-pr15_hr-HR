<properties
    pageTitle="Onemogućavanje korisnika s znak dodatke za enterprise aplikacije u pretpregledu Azure Active Directory | Microsoft Azure"
    description="Kako onemogućiti enterprise aplikaciju tako da nema korisnici mogu prijaviti da biste ga Azure Active Directory"
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
    ms.date="10/17/2016"
    ms.author="curtand"/>


# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory-preview"></a>Onemogućavanje korisnika s znak dodatke za enterprise aplikacije u pretpregledu Azure Active Directory

Da biste onemogućili enterprise aplikaciju tako da se ne mogu prijaviti korisnici je u pretpregledu Azure Active Directory (Azure AD) jednostavno je. [Novosti u pretpregledu?](active-directory-preview-explainer.md) Morate imati odgovarajuće dozvole za upravljanje enterprise aplikacije. U trenutni pretpregled, morate biti globalni administrator za direktorij.

## <a name="how-do-i-disable-user-sign-ins"></a>Kako onemogućiti znak dodaci korisnika?

1. Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2. Odaberite **više usluga**, unesite **Azure Active Directory** u tekstni okvir, a zatim pritisnite **Enter**.

3. Na na * *Azure Active Directory - *direktorija* ** plohu (to jest, za Azure AD plohu za direktorij upravljate), odaberite **Enterprise aplikacije **.

    ![Otvaranje Enterprise aplikacije](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)

4. Na plohu **aplikacijama** odaberite **sve aplikacije**. Pogledajte popis aplikacija možete upravljati.

5. Na plohu **aplikacijama – sve aplikacije** , odaberite aplikacije.

6. Na ***appname*** plohu (to jest, na plohu pod nazivom aplikaciju odabrane u naslovu) odaberite **Svojstva**.

    ![Odabir naredbe sve aplikacije](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)

7. Na ***appname*** **-Svojstva** plohu, odaberite **ne** za **omogućene za korisnike za prijavu?**.

8. Odaberite naredbu **Spremi** .

## <a name="next-steps"></a>Daljnji koraci

- [Pogledajte sve grupe](active-directory-groups-view-azure-portal.md)
- [Dodjela korisnika ili grupu enterprise aplikacije](active-directory-coreapps-assign-user-azure-portal.md)
- [Uklanjanje korisnika ili grupu dodjele enterprise aplikacije](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Promijenite naziv ili logotip enterprise aplikacije](active-directory-coreapps-change-app-logo-user-azure-portal.md)
