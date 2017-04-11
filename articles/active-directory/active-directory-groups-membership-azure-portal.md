<properties
    pageTitle="Upravljanje grupama vašoj grupi je član u pretpregledu Azure Active Directory | Microsoft Azure"
    description="Grupe mogu sadržavati druge grupe u servisu Azure Active Directory. Evo kako upravljati tim članstva."
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


# <a name="manage-the-groups-your-group-is-a-member-of-in-azure-active-directory-preview"></a>Upravljanje grupama vašoj grupi je član u pretpregledu Azure Active Directory

Grupe mogu sadržavati druge grupe u pretpregledu Azure Active Directory. [Novosti u pretpregledu?](active-directory-preview-explainer.md) Evo kako upravljati tim članstva.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Kako pronaći grupe moje grupe je član?

1.  Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2.  Odaberite **nove servise**, u tekstni okvir unesite **korisnici i grupe** , a zatim **Enter**.

  ![Upravljanje korisnicima za otvaranje](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  Na plohu **korisnici i grupe** odaberite **sve grupe**.

  ![Otvaranje plohu grupe](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. Na plohu **korisnicima i grupama – sve grupe** odaberite grupu.

5. Na *naziv *grupa - *grupe* ** Odaberite plohu **grupirati članstva **.

  ![Otvaranje plohu članstva u grupi](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Da biste grupu dodali na kao član drugu grupu, na plohu **grupa - članstva u grupi** odaberite naredbu **Dodaj** .

7. Odaberite grupu s plohu **Odaberite grupu** , a zatim **Odaberite** gumb pri dnu zaslona u plohu. Grupu možete dodati samo jednu grupu odjednom. Okvir **korisnika** filtrira prikaza koji se temelji na koji se podudaraju unos na bilo koji dio ime korisnika ili uređaj. Nema zamjenski znakovi prihvaćaju u tom okviru.

  ![Dodavanje članstvo u grupi](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. Da biste uklonili vašoj grupi kao član drugu grupu, na plohu **grupa - članstva u grupi** odaberite grupu.

9. Na plohu ***naziv grupe*** odaberite naredbu **Ukloni** , a izboru kada se zatraži potvrda.

  ![Uklanjanje naredbe članstvo](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. Kada završite s promjenom članstva u grupi za grupu, odaberite **Spremi**.


## <a name="additional-information"></a>Dodatne informacije

Ti članci sadrže dodatne informacije na Azure Active Directory.

* [U odjeljku postojeće grupe](active-directory-groups-view-azure-portal.md)
* [Stvaranje nove grupe i dodavanje članova](active-directory-groups-create-azure-portal.md)
* [Upravljanje postavkama grupe](active-directory-groups-settings-azure-portal.md)
* [Upravljanje članovima grupe](active-directory-groups-members-azure-portal.md)
* [Upravljanje dinamički pravila za korisnika u grupu](active-directory-groups-dynamic-membership-azure-portal.md)
