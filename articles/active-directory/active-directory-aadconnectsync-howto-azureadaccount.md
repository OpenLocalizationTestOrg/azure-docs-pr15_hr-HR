<properties
    pageTitle="Azure AD Connect sinkronizacije: kako upravljati račun servisa Azure AD | Microsoft Azure"
    description="U ovoj se temi dokumenti kako vratiti račun servisa Azure AD."
    services="active-directory"
    keywords="AADSTS70002, AADSTS50054, kako vratiti izvornu lozinku za račun servisa poveznik za sinkronizaciju Azure AD Connect"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Connect sinkronizacije: kako upravljati račun servisa Azure AD
Račun servisa koji se koristi poveznik za Azure AD bi trebao biti besplatnog servisa. Ako je potrebno ponovno postavljanje vjerodajnica za njegov zatim članak namijenjen vama. Na primjer, ako je globalni Administrator ima prema pogrešku ponovno postavljanje lozinke na račun servisa pomoću komponente PowerShell.

## <a name="reset-the-credentials"></a>Ponovno postavljanje vjerodajnica
Ako je račun servisa definiranih poveznik za Azure AD ne može uspostaviti Azure AD došlo do problema provjeru autentičnosti, lozinku možete vratiti.

1. Prijaviti na poslužitelj za Azure AD Connect sinkronizaciju i pokretanje programa PowerShell.
2. Pokrenite `Add-ADSyncAADServiceAccount`.  
![Addadsyncaadserviceaccount cmdlet komponente PowerShell](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Unesite vjerodajnice za Azure AD globalnog administratora.

Ovaj cmdlet vraća lozinku za račun servisa i ažuriranje Azure AD i u modula za sinkroniziranje.

## <a name="known-issues-these-steps-can-solve"></a>Poznati problemi ove korake možete riješiti
U ovom se odjeljku je popis pogrešaka prijavili korisnici koji su popraviti pomoću vjerodajnica ponovno pokrenuti na račun servisa Azure AD.

-----------
Događaj 6900  
Poslužitelj je naišao pojavila se neočekivana pogreška tijekom obrade obavijest Promijeni lozinku:  
AADSTS70002: Pogreška pri provjeri valjanosti vjerodajnice. AADSTS50054: Stara lozinka se koristi za provjeru autentičnosti.

----------
Događaj 659  
Pogreška prilikom dohvaćanja konfiguracija sinkronizaciju pravila za lozinke. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Pogreška pri provjeri valjanosti vjerodajnice. AADSTS50054: Stara lozinka se koristi za provjeru autentičnosti.

## <a name="next-steps"></a>Daljnji koraci

**Pregled tema**

- [Azure AD Connect sinkronizacije: razumijevanje i Prilagodba sinkronizacije](active-directory-aadconnectsync-whatis.md)
- [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
