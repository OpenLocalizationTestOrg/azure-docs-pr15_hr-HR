<properties
    pageTitle="Korištenje kontrola pristupa na temelju uloga na portalu za Azure | Microsoft Azure"
    description="Uvod u access upravljanja kontrolu pristupa na temelju uloga na portalu za Azure. Da biste dodijelili dozvole resursa pomoću dodjele uloga."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>Upravljanje pristupom resursi za Azure pretplatu pomoću dodjele uloga

> [AZURE.SELECTOR]
- [Upravljanje pristupom korisnik ili grupa](role-based-access-control-manage-assignments.md)
- [Access možete upravljati tako da resursa](role-based-access-control-configure.md)

Azure na temelju uloga pristup kontrola (RBAC) omogućuje upravljanje preciznije pristup za Azure. Korištenje RBAC, možete dodijeliti samo razinu pristupa korisnici moraju da svoje zadatke. Ovaj članak sadrži brz početak rada s RBAC na portalu za Azure. Ako želite više detalja o tome kako RBAC pomaže Upravljanje pristupom, potražite u članku [što je na temelju uloga kontrola pristupa](role-based-access-control-what-is.md).

## <a name="view-access"></a>Prikaz programa access
Možete vidjeti tko ima pristup resursa, grupa resursa ili pretplate iz njegova glavni plohu [Azure portal](https://portal.azure.com). Na primjer, želimo da biste vidjeli tko ima pristup naš grupa resursa:

1. Na navigacijskoj traci na lijevoj strani odaberite **grupe resursa** .  
    ![Grupa resursa – ikona](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Odaberite naziv grupe resursa iz plohu **grupa resursa** .
3. Odaberite **kontrolu pristupa (IAM)** na lijevom izborniku.  
4. Plohu za kontrolu pristupa navedene sve korisnike, grupe i aplikacije koje je dodijeljen pristup u grupu resursa.  

    ![Korisnici plohu - Dodavanje veze za naslijeđene vanjskih dodijeljeni snimka programa access](./media/role-based-access-control-configure/view-access.png)

Obavijest da neki korisnici nisu **dodijeljene** dok drugi **Inherited** joj pristupiti. Pristup posebno dodijeljene grupi resursa ili nasljeđuju od dodjelu nadređenog pretplatu.

> [AZURE.NOTE] Klasični pretplate administratori i suadministratora smatraju vlasnici pretplate u novi RBAC model.


## <a name="add-access"></a>Dodavanje programa Access
Dopustite pristup iz programa resursa, grupa resursa ili pretplatu za koju je opseg Dodjela uloge.

1. Odaberite **Dodaj** na plohu kontrole programa Access.  
2. Odaberite ulogu za koju želite dodijeliti iz plohu **Odaberite ulogu** .
3. Odaberite korisnika, grupa ili aplikacije u direktoriju koje želite dopustiti pristup. Možete pretraživati imenik s prikazana imena, adrese e-pošte i identifikatora objekata.  

    ![Dodajte korisnike plohu – snimka zaslona za pretraživanje](./media/role-based-access-control-configure/grant-access2.png)

4. Odaberite **u redu** da biste stvorili dodjelu. **Dodavanje korisnika** skočni prati napredak.  
    ![Dodavanje korisnika tijeku traka - snimka](./media/role-based-access-control-configure/addinguser_popup.png)

Nakon uspješnog dodavanja Dodjela uloge, pojavit će se plohu **korisnika** .

## <a name="remove-access"></a>Uklanjanje pristupa

1. Odaberite Dodjela uloga na plohu kontrole programa Access.
2. Odaberite **Ukloni** pojedinosti plohu dodjele.  
3. Odaberite **da** da biste potvrdili uklanjanje.  
    ![Korisnici plohu - Ukloni iz snimka uloga](./media/role-based-access-control-configure/remove-access1.png)

Naslijeđene dodjele nije moguće ukloniti. Obratite pozornost na slici u nastavku da gumb Ukloni zasivljen. Umjesto toga pogledajte detalje **Dodijeljeni pri** . Idite na resursa nema na popisu da biste uklonili Dodjela uloge.

![Korisnici plohu - naslijeđene access onemogućuje uklanjanje snimka zaslona gumba](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Druge alate za upravljanje pristupom
Možete dodijeliti uloge i upravljanje pristupom s naredbama Azure RBAC u alatima osim Azure portal.  Slijedite veze na dodatne informacije o preduvjete i početak rada s naredbama za Azure RBAC.

- [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure sučelje naredbenog retka](role-based-access-control-manage-access-azure-cli.md)
- [REST API-JA](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Daljnji koraci
- [Stvaranje izvješća povijest promjena programa access](role-based-access-control-access-change-history-report.md)
- Pogledajte [RBAC ugrađene uloga](role-based-access-built-in-roles.md)
- Definiranje vlastiti [Prilagođeni ulogama u Azure RBAC](role-based-access-control-custom-roles.md)
