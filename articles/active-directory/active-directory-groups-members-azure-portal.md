<properties
    pageTitle="Upravljanje članovima grupe u pretpregledu Azure Active Directory | Microsoft Azure"
    description="Kako korisnicima i uređajima koji su članovi grupe u servisu Azure Active Directory"
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


# <a name="manage-the-members-for-a-group-in-azure-active-directory-preview"></a>Upravljanje članovima grupe u pretpregledu Azure Active Directory

U ovom se članku objašnjava kako Upravljanje članovima grupe u pretpregledu Azure Active Directory (Azure AD). [Novosti u pretpregledu?](active-directory-preview-explainer.md)

## <a name="how-do-i-find-the-members-and-manage-them"></a>Kako pronaći članove i upravljanje njima?

1.  Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2.  Odaberite **Dodatne usluge**, unesite **korisnici i grupe** u tekstni okvir, a zatim pritisnite **Enter**.

  ![Upravljanje korisnicima za otvaranje](./media/active-directory-groups-members-azure-portal/search-user-management.png)

3.  Na plohu **korisnici i grupe** odaberite **sve grupe**.

  ![Otvaranje plohu grupe](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)

4. Na plohu **korisnicima i grupama – sve grupe** odaberite grupu.

5. Na u * *grupu - *naziv grupe* ** Odaberite plohu **članovi **.

  ![Otvaranje plohu članova](./media/active-directory-groups-members-azure-portal/view-group-members.png)

6. Da biste dodali članove u grupu, na plohu **grupa - članova** odaberite **Dodaj članove**.

  ![Dodavanje članova naredbe](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)

7. Na plohu **članova** odaberite jednu ili više korisnika ili uređaje da biste dodali u grupu, a zatim odaberite gumb **Odaberi** pri dnu zaslona u plohu da biste ih dodali u grupu. Okvir **korisnika** filtrira prikaza koji se temelji na koji se podudaraju unos na bilo koji dio ime korisnika ili uređaj. Nema zamjenski znakovi prihvaćaju u tom okviru.

8. Da biste uklonili članove iz grupe na plohu **grupa - članova** odaberite člana.

9. Na plohu ***membername*** odaberite naredbu **Ukloni** , a izboru kada se zatraži potvrda.

  ![Uklanjanje naredbe članova](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)

9. Kada završite s promjenom članova za grupu, odaberite **Spremi**.


## <a name="additional-information"></a>Dodatne informacije

Ti članci sadrže dodatne informacije na Azure Active Directory.

* [U odjeljku postojeće grupe](active-directory-groups-view-azure-portal.md)
* [Stvaranje nove grupe i dodavanje članova](active-directory-groups-create-azure-portal.md)
* [Upravljanje postavkama grupe](active-directory-groups-settings-azure-portal.md)
* [Upravljanje članstvima grupe](active-directory-groups-membership-azure-portal.md)
* [Upravljanje dinamički pravila za korisnika u grupu](active-directory-groups-dynamic-membership-azure-portal.md)
