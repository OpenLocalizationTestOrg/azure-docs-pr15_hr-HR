<properties
    pageTitle="Azure servisa Active Directory Domain Services: Omogući sinkronizaciju lozinke | Microsoft Azure"
    description="Uvod u Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Omogući sinkronizaciju lozinke za Azure servisa Active Directory Domain Services
U prethodnom zadaci omogućiti Azure AD domenske servise za vaš klijent Azure AD. Sljedeći zadatak je omogućiti sinkronizaciju lozinke Azure servisa Active Directory Domain Services. Kada postavite vjerodajnica sinkronizacije, korisnici mogli prijaviti upravljanih domenu pomoću svoje tvrtke vjerodajnice.

Koraka razlikuju se ovisno o tome je li tvrtka ili ustanova koristi samo oblak Azure AD smjernice ili je postavljena za sinkronizaciju s lokalnog imenika pomoću Azure AD Connect.

<br>

> [AZURE.SELECTOR]
- [Smjernice za samo oblak Azure AD](active-directory-ds-getting-started-password-sync.md)
- [Sinkronizirane Azure AD klijentu](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Zadatak 5: Omogući sinkronizaciju lozinke za AAD domenske servise za sinkronizirane Azure AD klijentu
A sinkronizirali Azure AD klijentu postavljen za sinkronizaciju s vaše tvrtke ili ustanove lokalnog imenika pomoću Azure AD Connect. Azure AD Connect sinkronizirajte NTLM i Kerberos vjerodajnica raspršivanja za Azure AD prema zadanim postavkama. Da biste koristili Azure servisa Active Directory Domain Services, morate konfigurirati Azure AD Connect da biste sinkronizirali vjerodajnica raspršivanja potreban za provjeru autentičnosti NTLM i Kerberos. Sljedeći koraci omogućiti sinkronizacije raspršivanja potrebne vjerodajnice za vaš klijent Azure AD.


### <a name="install-or-update-azure-ad-connect"></a>Instalacija i ažuriranje Azure AD Connect
Instalirajte najnovije preporučene izdanju programa Azure AD Connect na računalu na domeni. Ako imate postojeće instance komponente Azure AD Connect postavljanje, morate ažurirati koristiti najnoviju verziju programa Azure AD Connect. Da biste izbjegli Poznati problemi/programskih pogrešaka koje možda ste već riješen, provjerite je li uvijek koristite najnoviju verziju programa Azure AD Connect.

**[Povezivanje preuzimanje Azure AD](http://www.microsoft.com/download/details.aspx?id=47594)**

Preporučeno verzija: **1.1.281.0** - objavljena na rujan 7, 2016.

  > [AZURE.WARNING] POTREBNO je instalirati najnovija preporučene izdanju programa Azure AD Connect da biste omogućili vjerodajnice naslijeđene lozinke (obavezno za provjeru autentičnosti NTLM i Kerberos) da biste sinkronizirali klijentu za Azure AD. Ova funkcija nije dostupna iz prethodnih izdanja Azure AD Connect ili pomoću alata DirSync naslijeđene.

Upute za instalaciju za Azure AD Connect dostupne u sljedećem članku – [Prvi koraci s Azure AD Connect](../active-directory/active-directory-aadconnect.md)


### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Omogući sinkronizaciju NTLM i Kerberos raspršivanja vjerodajnica za Azure AD
Izvršiti sljedeću skriptu komponente PowerShell na svaki skup stabala AD, da biste nametnuli sinkronizacije cijelog lozinku i omogućite raspršivanja vjerodajnica svih lokalnog korisnika da biste sinkronizirali klijentu za Azure AD. Ova skripta omogućuje raspršivanja vjerodajnica potreban za provjeru autentičnosti NTLM/Kerberos za sinkronizaciju u klijent Azure AD.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Ovisno o veličini direktorija (broj korisnika, grupa etc.), sinkronizacija raspršivanja vjerodajnica za Azure AD traje. Lozinke će moći koristiti na domeni upravljanih Azure servisa Active Directory Domain Services uskoro nakon raspršivanja vjerodajnica sinkronizirali su se za Azure AD.


<br>

## <a name="related-content"></a>Srodni sadržaji

- [Omogući sinkronizaciju lozinke za AAD domenske servise za samo oblak Azure AD direktorija](active-directory-ds-getting-started-password-sync.md)

- [Administriranje domene upravljanih programa Azure servisa Active Directory Domain Services](active-directory-ds-admin-guide-administer-domain.md)

- [Uključivanje virtualnog računala za Windows s domenom upravljanih programa Azure servisa Active Directory Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)

- [Uključivanje crveno je vaša Enterprise Linux virtualnog računala s domenom upravljanih programa Azure servisa Active Directory Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
