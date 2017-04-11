<properties
    pageTitle="Uklanjanje korisnika ili grupu dodjele enterprise aplikacije u pretpregledu Azure Active Directory | Microsoft Azure"
    description="Kako ukloniti dodjelu pristupa za korisnika ili grupu s enterprise aplikacije servisa Azure Active Directory"
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
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory-preview"></a>Uklanjanje korisnika ili e-pošta s enterprise aplikacije u pretpregledu Azure Active Directory

Da biste uklonili korisnika ili grupu s dodijeljene pristup nekom aplikacijama u pretpregledu Azure Active Directory (Azure AD) jednostavno je. [Novosti u pretpregledu?](active-directory-preview-explainer.md) Morate imati odgovarajuće dozvole za upravljanje enterprise aplikacije. U trenutni pretpregled, morate biti globalni administrator za direktorij.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Kako ukloniti korisnika ili e-pošta?

1. Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2. Odaberite **više usluga**, unesite **Azure Active Directory** u tekstni okvir, a zatim pritisnite **Enter**.

3. Na na * *Azure Active Directory - *direktorija* ** plohu (to jest, za Azure AD plohu za direktorij upravljate), odaberite **Enterprise aplikacije **.

    ![Otvaranje Enterprise aplikacije](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)

4. Na plohu **aplikacijama** odaberite **sve aplikacije**. Vidjet ćete popis aplikacija možete upravljati.

5. Na plohu **aplikacijama – sve aplikacije** , odaberite aplikacije.

6. Na ***appname*** plohu (to jest, na plohu pod nazivom aplikaciju odabrane u naslovu) odaberite **korisnici i grupe**.

    ![Odabir korisnika ili grupa](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)

7. Na ***appname*** **-korisnika i grupa dodjele** plohu, odaberite jednu od druge korisnike ili grupe, a zatim odaberite naredbu **Ukloni** . Potvrdite svoju odluku naredbenom.

    ![Odabir naredbe Ukloni](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Daljnji koraci

- [Pogledajte sve svoje grupe](active-directory-groups-view-azure-portal.md)
- [Dodjela korisnika ili grupu enterprise aplikacije](active-directory-coreapps-assign-user-azure-portal.md)
- [Onemogućavanje korisnika s znak dodatke za enterprise aplikacije](active-directory-coreapps-disable-app-azure-portal.md)
- [Promijenite naziv ili logotip enterprise aplikacije](active-directory-coreapps-change-app-logo-user-azure-portal.md)
