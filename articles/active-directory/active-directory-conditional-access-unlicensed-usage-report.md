<properties
    pageTitle="Izvješća o korištenju nelicenciranom | Microsoft Azure"
    description="Pridonosi izvješće nelicencirani korištenje prepoznavanje nelicenciranih korisnika koje koriste plaćanje Azure AD značajke."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="unlicensed-usage-report"></a>Izvješća o korištenju nelicencirani

Pridonosi izvješća nelicenciranih korištenje prepoznavanje nelicenciranih korisnika koje koriste plaćanje Azure AD značajke. Time se omogućuje da bi bolje koristite broj licenci koje ste kupili, a da biste odredili znate kada je potrebno dodatne licence. 

Izvješće prikazuje aktivni korištenje plaćeni značajki u zadnjih 30 dana. 

## <a name="report-structure"></a>Struktura izvješća
 
| Naziv stupca          |    Opis |
| :--                  | :--         |
| Nelicenciranih korisnika      |    Ime korisnika |
| Značajka              | Naziv značajke. Na primjer: uvjetno programa access |
| Aplikacija pristupa | Naziv aplikacije u kojoj se pristupa pomoću značajke. Na primjer: Office 365 SharePoint Online |

 
> [AZURE.NOTE] Ako je izbrisana korisnički račun stupcu "Nelicencirani korisnik" će popunjen ID, kao što je 1003000090D8B285


## <a name="conditional-access-feature"></a>Značajke uvjetnog pristupa

Vijesti označit će se prilikom pristupa servis koji sadrži pravila uvjetnog pristup primijeniti ako nemate licencu za Azure AD Premium. 

To se odnosi na MFA / pravila mjesto, kao i na uređaj pravila koja koristi Intune.
 

## <a name="see-also"></a>Vidi također

- [Korištenje uvjetnog programa Access u sustavu Office 365 i druge Azure Active Directory povezani aplikacije](active-directory-conditional-access.md)
- [Uvod u uvjetno pristup Azure AD](active-directory-conditional-access-azuread-connected-apps.md) 


