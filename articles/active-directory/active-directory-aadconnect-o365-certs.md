<properties
    pageTitle="Potvrda obnove upute za korisnike sustava Office 365 i Azure Active Directory | Microsoft Azure"
    description="U ovom se članku objašnjava korisnicima sustava Office 365 da biste riješili probleme s porukama e-pošte koja će obavijestiti o obnovom certifikat."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Obnavljanje certifikata za vanjski pristup za Office 365 i Azure Active Directory

##<a name="overview"></a>Pregled

Za uspješan vanjski pristup između Azure Active Directory (Azure AD) i Active Directory Federation Services (AD FS), koristi AD FS da biste se prijavili sigurnosnih tokena za Azure AD treba podudaraju se certifikati što je konfiguriran u Azure AD. Bilo koji ne podudaraju se može dovesti do neispravne pouzdanost. Azure AD osigurava da se ti podaci se sinkroniziraju kada implementaciju AD FS i Proxy aplikacije Web (za ekstranet pristup).

Ovaj članak sadrži dodatne informacije za upravljanje vaš token potpisivanje certifikata i sinkroniziranosti ih s Azure AD u sljedećim slučajevima:

* Ne implementirate Proxy aplikacije Web i stoga nije dostupna u na ekstranet metapodataka za vanjski pristup.
* Ne koristite zadanu konfiguraciju AD FS token potpisivanje certifikata.
* Koristite davatelja identiteta treće strane.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Konfiguriranje zadanog AD FS token potpisivanje certifikata

Potpisivanje tokena i token dešifriranja certifikati su obično samopotpisane potvrde, a dobro traje jednu godinu. Po zadanom AD FS obuhvaćaju proces automatsko obnavljanje pod nazivom **AutoCertificateRollover**. Ako koristite AD FS 2.0 ili noviji, Office 365 i Azure AD automatski ažurirati potvrdu prije no što istekne.

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>Obnavljanje obavijesti s portala sustava Office 365 ili poruku e-pošte

>[AZURE.NOTE] Ako ste primili poruku e-pošte ili portala obavijest s pitanjem želite obnoviti certifikat za Office, potražite u članku [Upravljanje mijenja se u token potpisivanje certifikata](#managecerts) za provjeru potrebno ništa poduzimati. Microsoft je upoznat moguće problem koji može dovesti do obavijesti za obnovu certifikata koja se šalje, čak i kad je potreban je ne poduzmete.

Azure AD pokušava metapodataka za vanjski pristup za praćenje i ažuriranje token za potpisivanje certifikata kao što je naznačeno prema ovom metapodacima. 30 dana prije isteka certifikata za potpisivanje tokena Azure AD provjerava jesu li novog certifikata dostupni po provjere metapodataka za vanjski pristup.

* Ako je uspješno možete ankete metapodataka za vanjski pristup i dohvaćanje novog certifikata, bez obavijesti e-pošte ili upozorenje na portalu za Office 365 izdavanja korisniku.
* Ako ga ne može dohvatiti novi token potpisivanje certifikata, ili jer metapodataka za vanjski pristup nije dostupna ili automatskog dinamične nije omogućena, Azure AD šalje obavijest e-poštom i upozorenja na portalu za Office 365.

![Obavijesti portala za Office 365](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] Ako koristite AD FS, da biste bili sigurni poslovanje, provjerite je li vaši poslužitelji da imaju sljedeća ažuriranja tako da se neuspjele provjere autentičnosti o poznatim problemima Informirajte se pojaviti. To mitigates poznati AD FS proxy poslužitelja problemi obnove a razdoblja za buduća obnavljanje:
>
>Server 2012 R2 - [Windows Server Smotuljak svibnja 2014](http://support.microsoft.com/kb/2955164)
>
>Server 2008 R2 i 2012 – [Provjera autentičnosti putem proxyja ne uspije u sustavu Windows Server 2012 ili Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446)

## Potvrdite ako je potrebno ažurirati potvrde<a name="managecerts"></a>

### <a name="step-1-check-the-autocertificaterollover-state"></a>Korak 1: Provjera stanja AutoCertificateRollover

Na poslužitelj za ADFS otvorite PowerShell. Provjerite je li vrijednost AutoCertificateRollover postavljen na True.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] Ako koristite AD FS 2.0 prvog pokretanja Dodaj Pssnapin Microsoft.Adfs.Powershell.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Korak 2: Potvrda AD FS i Azure AD su u sinkronizaciji

Na poslužitelj za ADFS otvorite upit Azure AD PowerShell i povezati Azure AD.

>[AZURE.NOTE] Azure AD PowerShell možete preuzeti [ovdje](https://technet.microsoft.com/library/jj151815.aspx).

    Connect-MsolService

Provjerite certifikata koji je konfiguriran u AD FS i Azure AD pouzdanost svojstva za navedenu domenu.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Ako thumbprints u oba izlaze podudaraju, certifikati su sinkronizacija Azure AD.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>Korak 3: Provjera je li vaš certifikat uskoro će isteći

U izlaz Get-MsolFederationProperty ili Get-AdfsCertificate Provjeri datum u odjeljku "Ne nakon." Ako je datum manje od 30 dana Odsutan, morate poduzeti akciju.

| AutoCertificateRollover | Sinkronizacija Azure AD certifikata | Vanjski pristup metapodataka je javno dostupno | Valjanost | Akcija |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| Da | Da | Da | - | Nije potrebno ništa poduzimati. Potražite u članku [Obnavljanje token automatski potpisni certifikat](#autorenew). |
| Da | ne  | - | Manje od 15 dana | Obnoviti odmah. Potražite u članku [Obnavljanje token ručno potpisni certifikat](#manualrenew). |
| ne | - | - | Manje od 30 dana | Obnoviti odmah. Potražite u članku [Obnavljanje token ručno potpisni certifikat](#manualrenew). |

\[Nije važno –]

## Obnavljanje tokena automatski potpisni certifikat (preporučeno)<a name="autorenew"></a>

Ne morate izvršiti sve ručne korake ako su zadovoljena oba od sljedećeg:
- Uveli Web proxy poslužitelj aplikacije, koji omogućuje pristup vanjski pristup metapodataka iz na ekstranet.
- Koristite zadanu konfiguraciju AD FS (AutoCertificateRollover je omogućeno).

Provjerite sljedeće da biste potvrdili da se potvrda mogu se automatski ažurirati.

**1. na AD FS svojstvo AutoCertificateRollover mora biti postavljeno na True.** To označava da AD FS će se automatski stvoriti novi tokena prijava i tokena dešifriranje certifikata, prije stari one isteći.

**2. metapodataka vanjski pristup za AD FS je javno dostupno.** Provjerite metapodatka vanjski pristup je javno dostupno tako da odete sljedeći URL s računala na Internetu javno (s korporacijskom mrežom):


/federationmetadata/2007-06/federationmetadata.xml https:// (your_FS_name)

gdje `(your_FS_name) `je zamijenjena funkcijom naziv glavnog računala servisa vanjski pristup vaša tvrtka ili ustanova koristi, kao što su fs.contoso.com.  Ako se mogućnost da biste provjerili oboje od tih postavki uspješno, ne morate poduzeti ništa.  

Primjer: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Obnavljanje tokena ručno potpisni certifikat<a name="manualrenew"></a>

Možete odabrati da biste obnovili token ručno potpisivanje certifikata. Ako, na primjer, sljedećim scenarijima možda radi bolje ručno obnavljanje:
* Tokena potpisivanje certifikata su ne samopotpisane potvrde. Najčešći razlog za to je da vašoj tvrtki ili ustanovi upravlja upisani iz tvrtke ili ustanove ustanova certifikata za AD FS.
* Zaštita mreže ne dopušta metapodataka vanjski pristup bude javno dostupno.

U sljedećim scenarijima svaki put kada ažurirate token za potpisivanje certifikata, morate i ažurirati domene u sustavu Office 365 pomoću naredbe ljuske PowerShell ažuriranje MsolFederatedDomain.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Korak 1: AD FS provjerite ima li novi token potpisivanje certifikata

**Konfiguriranje koji nisu zadani**

Ako koristite konfiguracija koji nisu zadani AD FS (gdje **AutoCertificateRollover** postavljen na **False**), vjerojatno koristite prilagođene certifikata (nisu samopotpisane). Dodatne informacije o kako obnoviti tokena AD FS potpisivanje certifikata potražite u članku [upute za korisnike koji ne koriste AD FS samopotpisane potvrde](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Vanjski pristup metapodataka nije javno dostupna**

S druge strane, ako **AutoCertificateRollover** postavljen na **True**, ali nije javno dostupnu metapodatka vanjski pristup, najprije provjerite AD FS ste generirala novog tokena potpisnog certifikata. Potvrdite da ste novi token potpisivanje certifikata tako da poduzmete sljedeće korake:

1. Potvrdite da ste prijavljeni na primarni poslužitelj za AD FS.
2. Provjerite trenutne potpisnog certifikata u AD FS otvaranjem prozoru za naredbe ljuske PowerShell i pokretanjem sljedeće naredbe:

    PS C:\>Get-ADFSCertificate – CertificateType potpisivanje tokena

    >[AZURE.NOTE] Ako koristite AD FS 2.0, pokrećite Dodaj Pssnapin Microsoft.Adfs.Powershell prvi put.

3. Pogledajte naredba Izlaz na bilo koji certifikata na popisu. AD FS generirao novi certifikat, trebali biste vidjeti dva potvrde u ispisu: jednu za koju **IsPrimary** je vrijednost **True** i datum **NotAfter** 5 dana, a jedan za koje **IsPrimary** je **False** i **NotAfter** o godine u budućnosti.

4. Ako vidite samo jedan certifikat, a datum **NotAfter** nalazi unutar 5 dana, morate stvoriti novi.

5. Da biste generirali nove certifikata, pokrenite sljedeću naredbu u naredbenom retku PowerShell: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

6. Potvrdite okvir Ažuriraj tako da ponovno pokretanjem sljedeće naredbe: PS C:\>Get-ADFSCertificate – CertificateType potpisivanje tokena

Dva certifikata mora biti naveden odmah, od kojih je datum **NotAfter** otprilike jednu godinu u budućnosti i za koju je **IsPrimary** vrijednost **False**.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>Korak 2: Ažuriranje novi token potpisivanje certifikata za pouzdanost sustava Office 365

Ažuriranje sustava Office 365 s novi token potpisivanje certifikata koji će se koristiti za pouzdanost, na sljedeći način.

1.  Otvaranje modula Microsoft Azure Active Directory za Windows PowerShell.
2.  Pokrenite $cred = Get-vjerodajnica. Kada ovaj cmdlet traži vjerodajnice, upišite oblaka vjerodajnice za servis administrator računa.
3.  Pokrenite povezivanje MsolService – vjerodajnica $cred. Ovaj cmdlet povezuje servisa u oblaku. Stvaranje kontekstu koji se povezujete sa servisom cloud potreban je prije pokretanja neke dodatne cmdleta instalirao alat.
4.  Ako koristite naredbe na računalima na kojima nije primarni vanjskom poslužitelju AD FS, pokrenite postavljanje MSOLAdfscontext-računalo <AD FS primary server>, pri čemu <AD FS primary server> je naziv internog FQDN primarnog poslužitelja za AD FS. Ovaj cmdlet stvara kontekstu koji povezujete AD FS.
5.  Pokrenite ažuriranje MSOLFederatedDomain – DomainName <domain>. Ovaj cmdlet ažurira postavke iz AD FS u servis u oblaku i konfigurira odnos pouzdanosti između njih.


>[AZURE.NOTE] Ako vam je potrebna podrška više domena najviše razine, kao što je contoso.com i fabrikam.com, morate koristiti parametar **SupportMultipleDomain** sve cmdleta sustava. Dodatne informacije potražite u članku [podršku za više domena vrha razinu](active-directory-aadconnect-multiple-domains.md).

## Popravak pouzdanost Azure AD pomoću Azure AD Connect<a name="connectrenew"></a>

Ako ste konfigurirali farme AD FS i pouzdanost Azure AD pomoću Azure AD Connect Azure AD Connect možete koristiti da biste otkrili potrebno ništa poduzimati za vaš token potpisivanje certifikata. Ako je potrebno obnoviti certifikate Azure AD Connect možete koristiti da biste to učinili.

Dodatne informacije potražite u članku [popravak Vjeruj](./active-directory-aadconnect-federation-management.md#repairing-the-trust).
