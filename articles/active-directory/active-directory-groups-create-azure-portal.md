<properties
    pageTitle="Stvaranje nove grupe u pretpregledu Azure Active Directory | Microsoft Azure"
    description="Kako stvoriti grupu servisa Azure Active Directory i dodavanje korisnika (članovi) u grupi"
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


# <a name="create-a-new-group-in-azure-active-directory-preview"></a>Stvaranje nove grupe u pretpregledu Azure Active Directory

> [AZURE.SELECTOR]
- [Portal za Azure](active-directory-groups-create-azure-portal.md)
- [Azure klasični portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

U ovom se članku objašnjava kako stvoriti i popunjavanje nove grupe u pretpregledu Azure Active Directory (Azure AD). [Novosti u pretpregledu?](active-directory-preview-explainer.md) Pomoću grupe za izvođenje zadataka upravljanja kao što je Dodjela licence ili dozvole broj korisnika i uređaje odjednom.

## <a name="how-do-i-create-a-group"></a>Kako stvoriti grupu?

1. Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2. Odaberite **više usluga**, unesite **korisnika i grupa** u tekstni okvir, a zatim pritisnite **Enter**.

  ![Upravljanje korisnicima za otvaranje](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. Na plohu **korisnici i grupe** odaberite **sve grupe**.

  ![Otvaranje plohu grupe](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. Na plohu **korisnicima i grupama – sve grupe** odaberite naredbu **Dodaj** .

  ![Odabir naredbe Dodaj](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. Na plohu **grupe** dodajte naziv i opis grupe.

6. Da biste odabrali članovi da biste dodali u grupu, odaberite **kome je dodijeljeno** u okvir **Vrsta članstva** , a zatim **Članovi**. Dodatne informacije o dinamično upravljanje članstvom u grupi potražite u članku [korištenje atributa stvaranju naprednih pravila za članstvo u grupi](active-directory-groups-dynamic-membership-azure-portal.md).

  ![Odabir članovi da biste dodali](./media/active-directory-groups-create-azure-portal/select-members.png)

5. Na plohu **članova** odaberite jednu ili više korisnika ili uređaje da biste dodali u grupu, a zatim odaberite gumb **Odaberi** pri dnu zaslona u plohu da biste ih dodali u grupu. Okvir **korisnika** filtrira prikaza koji se temelji na koji se podudaraju unos na bilo koji dio ime korisnika ili uređaj. Nema zamjenski znakovi prihvaćaju u tom okviru.

6. Kada završite s dodavanjem članova u grupu, odaberite **Stvori** na plohu **grupe** .    

  ![Stvaranje grupe potvrdu](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## <a name="additional-information"></a>Dodatne informacije

Ti članci sadrže dodatne informacije na Azure Active Directory.

* [U odjeljku postojeće grupe](active-directory-groups-view-azure-portal.md)
* [Upravljanje postavkama grupe](active-directory-groups-settings-azure-portal.md)
* [Upravljanje članovima grupe](active-directory-groups-members-azure-portal.md)
* [Upravljanje članstvima grupe](active-directory-groups-membership-azure-portal.md)
* [Upravljanje dinamički pravila za korisnika u grupu](active-directory-groups-dynamic-membership-azure-portal.md)
