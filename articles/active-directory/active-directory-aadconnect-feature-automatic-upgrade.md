<properties
   pageTitle="Azure AD Connect: Automatska Nadogradnja | Microsoft Azure"
   description="U ovoj se temi opisuju ugrađene značajku automatskog nadogradnje u Azure AD Connect sinkronizaciju."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/24/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: Automatska Nadogradnja
Značajka je uvedena s Sastavi 1.1.105.0 (objavljeno veljače 2016).

## <a name="overview"></a>Pregled
Provjerite je li vaša instalacija Azure AD Connect uvijek je u tijeku nikada nije jednostavnijim značajka **automatskog nadogradnju** . Značajka je omogućena prema zadanim postavkama za eksplicitnih instalacije i DirSync nadogradnje. Kada je nova verzija, vaša instalacija automatski će se nadograditi.

Automatska Nadogradnja omogućena je prema zadanim postavkama za sljedeće:

- Izrazite postavke instalacije i DirSync nadogradnji.
- Korištenje SQL Express LocalDB, koji je što Express postavke uvijek koristi. DirSync sa SQL Express koristiti LocalDB.
- Račun za AD je stvorio Express postavke i DirSync zadani račun MSOL_.
- Imati manje od 100 000 objekata na metaverse.

Trenutno stanje automatske nadogradnje moguće je prikazati pomoću cmdleta ljuske PowerShell `Get-ADSyncAutoUpgrade`. Sastoji se od sljedećih stanja:

Stanje | Komentar
---- | ----
Omogućeno | Automatska Nadogradnja je omogućena.
Obustavljena | Postavite sustav samo. U sustavu više ne ispunjava uvjete za primati Automatska ažuriranja.
Onemogućeni | Automatska Nadogradnja je onemogućen.

Možete promijeniti između **omogućeno** i **onemogućene** s `Set-ADSyncAutoUpgrade`. U sustavu postavite stanje **obustavljeno**.

Automatska Nadogradnja koristi povežite zdravlje Azure AD za nadogradnju infrastrukture. Za automatsko nadogradnje za rad provjerite je li otvorene URL-ovi u proxy poslužitelj za **Azure AD povežite zdravlje** kao navedenih u [Office 365 URL-ovi i rasponi IP adresa](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Ako **Upravitelj sinkronizacije servisa** korisničkog Sučelja se izvodi na poslužitelju, Nadogradnja je obustavljena dok je zatvoren korisničkog Sučelja.

## <a name="troubleshooting"></a>Otklanjanje poteškoća
Ako vaša instalacija povezivanje nadograđivati sam prema očekivanjima, slijedite ove korake da biste saznali što može biti netočan.

Najprije možete ne očekivati automatske nadogradnje na pokušaj prvi dan je objavio novu verziju. Postoji namjerno nepravilnosti prije nadogradnje pokušava tako da ne alarmed ako je vaša instalacija nije odmah.

Ako mislite da nešto nije u desno, zatim prvog pokretanja `Get-ADSyncAutoUpgrade` da biste bili sigurni da je omogućena Automatska nadogradnja.

Pa provjerite je li potreban URL-ovi ste otvorili u proxy ili vatrozid. Automatsko ažuriranje koristi Azure AD povežite zdravlje kao što je opisano u [Pregled](#overview). Ako koristite proxy poslužitelj, provjerite je li stanje konfiguriran za korištenje [proxy poslužitelj](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Također testirati [povezivost stanja](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) za Azure AD.

S povezivanjem za Azure AD potvrđena, vrijeme je za pogleda na eventlogs. Pokrenite preglednik događaja, a zatim potražite u zapisniku događaja **aplikacije** . Dodajte u zapisniku događaja filtar za izvor **Azure povezivanje AD nadograditi** i id raspon **300 399**događaj.  
![Filtar zapisniku događaja za automatsko nadogradnje](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Sada možete vidjeti eventlogs pridružene status Automatska nadogradnja.  
![Filtar u zapisniku događaja za Automatska Nadogradnja](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

Kod rezultat je prefiks pregled stanja.

Rezultat kod prefiksa | Opis
--- | ---
Uspjeh | Instalacija uspješno nadograditi.
UpgradeAborted | Privremeno zaustaviti nadogradnje. To će pokušati ponovno, a na očekivanja je uspješnog da kasnije.
UpgradeNotSupported | Sustav ima konfiguracije koji blokira sustava iz automatski nadograđen. Će pokušati li stanje u kojem se mijenja, ali je u očekivanja da morate sustav ručno moguće nadograditi.

Slijedi popis Najčešći poruka pronađete. Popis svih, ali poruka rezultat mora biti poništite taj problem je.

Rezultat poruke | Opis
--- | ---
**UpgradeAborted** |
UpgradeAbortedCouldNotSetUpgradeMarker | Nije moguće pisati u registar.
UpgradeAbortedInsufficientDatabasePermissions | Grupe Administratori ugrađene potrebne dozvole za bazu podataka. Ručno nadograditi na najnoviju verziju programa Azure AD Connect da biste riješili taj problem.
UpgradeAbortedInsufficientDiskSpace | Nema dovoljno prostora na disku za podršku za nadogradnju.
UpgradeAbortedSecurityGroupsNotPresent | Nije moguće pronaći i riješiti svih sigurnosnih grupa koji se koriste modula za sinkroniziranje.
UpgradeAbortedServiceCanNotBeStarted | Pokretanje NT servisa **Microsoft Azure AD sinkronizacije** nije uspjelo.
UpgradeAbortedServiceCanNotBeStopped | Zaustavljanje NT servisa **Microsoft Azure AD sinkronizacije** nije uspjelo.
UpgradeAbortedServiceIsNotRunning | NT servisa **Microsoft Azure AD sinkronizacije** nije pokrenut.
UpgradeAbortedSyncCycleDisabled | Mogućnost SyncCycle u [raspored](active-directory-aadconnectsync-feature-scheduler.md) je onemogućen.
UpgradeAbortedSyncExeInUse | [Upravitelj sinkronizacije servisa korisničkog Sučelja](active-directory-aadconnectsync-service-manager-ui.md) je otvorena na poslužitelju.
UpgradeAbortedSyncOrConfigurationInProgress | Čarobnjak za instalaciju je pokrenut ili sinkronizaciju zakazan izvan na raspored.
**UpgradeNotSupported** |
UpgradeNotSupportedCustomizedSyncRules | Dodali ste prilagođenog pravila konfiguracije.
UpgradeNotSupportedDeviceWritebackEnabled | Omogućeno značajka [upisima uređaja](active-directory-aadconnect-feature-device-writeback.md) .
UpgradeNotSupportedGroupWritebackEnabled | Omogućeno značajka [upisima grupe](active-directory-aadconnect-feature-preview.md#group-writeback) .
UpgradeNotSupportedInvalidPersistedState | Instalacija nije web-mjesto sustava Express postavke ili u okvir za DirSync nadogradnju.
UpgradeNotSupportedMetaverseSizeExceeeded | Imate više od 100 000 objekata u na metaverse.
UpgradeNotSupportedMultiForestSetup | Se povezujete s više od jednog skupa stabala. Express postavljanje povezuje samo jedan skup stabala.
UpgradeNotSupportedNonLocalDbInstall | Ne koristite bazu podataka sustava SQL Server Express LocalDB.
UpgradeNotSupportedNonMsolAccount | [Poveznik za AD račun](active-directory-aadconnect-accounts-permissions.md#active-directory-account) više nije zadani MSOL_ račun.
UpgradeNotSupportedStagingModeEnabled | Poslužitelj je postavljeno u [pripremna način](active-directory-aadconnectsync-operations.md#staging-mode).
UpgradeNotSupportedUserWritebackEnabled | Omogućeno značajka [upisima korisnika](active-directory-aadconnect-feature-preview.md#user-writeback) .

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
