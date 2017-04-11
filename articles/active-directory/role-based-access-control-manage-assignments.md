<properties
    pageTitle="Prikaz dodjele resursa za Azure pristup | Microsoft Azure"
    description="Prikaz i upravljanje sve dodjele na temelju uloga kontrola pristupa za svaki korisnik ili grupa na portalu za Azure"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="jeffsta"/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal---public-preview"></a>Prikaz dodjele pristupa za korisnike i grupe na portalu Azure – javno pretpregled

> [AZURE.SELECTOR]
- [Upravljanje pristupom korisnik ili grupa](role-based-access-control-manage-assignments.md)
- [Access možete upravljati tako da resursa](role-based-access-control-configure.md)

S na temelju uloga pristup kontrola (RBAC) u pretpregledu Azure Active Directory, možete upravljati pristupom za Azure resurse. [Novosti u pretpregledu?](active-directory-preview-explainer.md)

Pristup pridružena RBAC je preciznije jer na dva načina možete ograničiti dozvole:

- **Opseg:** Dodjele uloga RBAC imaju ograničen prikaz samo određenih pretplatu, grupa resursa ili resurs. Dali pristup resursu jedan korisnik ne može pristupiti drugih resursa u okviru iste pretplate.
- **Uloga:** Unutar opsega dodjelu pristupa je sa suženim čak i dalje dodijelite uloge. Uloga može biti više razine, kao što je vlasnik ili određene, kao što je čitač virtualnog računala.

Uloge mogu samo dodijeliti iz iz pretplate, grupa resursa ili je opseg za dodjelu resursa. No možete pogledati sve dodjele pristup određenom korisniku ili grupi na jednom mjestu.

Saznajte više o načinu [dodjele uloga koristite da biste upravljali pristup resursi za Azure pretplate](role-based-access-control-configure.md).

##  <a name="view-access-assignments"></a>Prikaz dodjele za access

Da biste potražili dodjele pristup za jednog korisnika ili grupu, pokrenite Azure Active Directory [Azure portal](http://portal.azure.com).

1. Odaberite **Azure Active Directory**. Ako ta mogućnost nije vidljiva na popisu navigacije, odaberite **Više usluge** i pomaknite se prema dolje do traži **Azure Active Directory**.
2. Odaberite **korisnici i grupe**, a zatim ili **sve korisnike** ili **sve grupe**. Primjerice, ne možemo fokus na pojedinačnim korisnicima.
    ![Upravljanje korisnicima i grupama u Azure Active Directory - snimka](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Traženje korisnika prema imenu ili korisničko ime.
4. Odaberite **Resursi za Azure** plohu korisnika. Pojavit će se sve dodjele pristup za tog korisnika.

### <a name="read-permissions-to-view-assignments"></a>Prikaz dodjele dozvola za čitanje

Ova stranica prikazuje samo dodjele access koji imaju dozvolu za čitanje. Ako, na primjer, imaju pristup čitanje pretplate odgovora i idite na plohu Azure resurse da biste provjerili korisničke dodjele. Možete vidjeti njezinu dodjele pristup za pretplatu odgovora, ali nećete moći vidjeti da korisnik ima pristup dodjele pretplate B.

## <a name="delete-access-assignments"></a>Brisanje dodjele programa access

Iz ovog plohu možete izbrisati Dodjela access koje su izravno dodijeljena korisnika ili grupu. Ako je pristup dodjele nasljeđuju od nadređene grupe, morate resursa ili pretplate i upravljanje njima u dodjele.

1. Popis svih dodjela pristupa za korisnika ili grupu, odaberite onaj koji želite izbrisati.
2. Odaberite **Ukloni** , a zatim **da** da biste potvrdili.
    ![Uklanjanje dodjelu pristupa - snimka](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="related-topics"></a>Povezane teme

- Početak rada s kontrola pristupa na temelju uloga dodjele [uloga koristi za upravljanje pristupom resursi za Azure pretplate](role-based-access-control-configure.md)
- Pogledajte [RBAC ugrađene uloge](role-based-access-built-in-roles.md)
