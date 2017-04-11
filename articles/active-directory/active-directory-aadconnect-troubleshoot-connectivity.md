<properties
    pageTitle="Azure AD Connect: Otklanjanje problema s povezivanjem | Microsoft Azure"
    description="U članku se objašnjava kako otkloniti poteškoće s povezivanjem s Azure AD Connect."
    services="active-directory"
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
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Otklanjanje poteškoća s povezivanjem s Azure AD Connect
U ovom se članku objašnjava kako funkcionira veze između Azure AD Connect i Azure AD te upute za otklanjanje problema s povezivanjem. Te probleme najčešće se vidjeti u okruženju s proxy poslužitelj.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Otklanjanje problema s povezivanjem u čarobnjaku za instalaciju
Azure AD Connect koristi Moderna provjere autentičnosti (pomoću ADAL biblioteke) za provjeru autentičnosti. Čarobnjak za instalaciju i proper modula za sinkroniziranje potreban je machine.config da biste biti pravilno konfigurirani jer su .NET aplikacijama.

U ovom članku Pokazat ćemo način na koji se Fabrikam povezuje s Azure AD putem njegova proxyja. Proxy poslužitelj pod nazivom fabrikamproxy i koristi priključak 8080.

Najprije ćemo morate provjerite je li pravilno konfiguriran [**machine.config**](active-directory-aadconnect-prerequisites.md#connectivity) .  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

>[AZURE.NOTE]
Na nekim blogovima proizvođača ga navedenih da moraju biti promjene miiserver.exe.config umjesto toga. Međutim, datoteka je prebrisati na svaku nadogradnje pa čak i ako radi tijekom početne instalacije, sustav će prestati raditi na prvi nadogradnje. Zbog toga u preporuka je da biste ažurirali machine.config umjesto toga.

Proxy poslužitelj mora imati URL-ove potreban otvoriti. Popis za službeno navedenih u [Office 365 URL-ovi i rasponi IP adresa ](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Od ovih, u sljedećoj su tablici je apsolutna Gola minimum da biste se mogli povezati s Azure AD uopće. Ovaj popis ne uključuje dodatne značajke, kao što su lozinka upisima ili Azure AD povezivanje stanja. Ga navedenih u nastavku da biste olakšati otklanjanje poteškoća za početnu konfiguraciju.

URL-A | Priključak | Opis
---- | ---- | ----
mscrl.microsoft.com | HTTP/80 | Služi za preuzimanje popise CRL-ova.
\*. verisign.com | HTTP/80 | Služi za preuzimanje popise CRL-ova.
\*. entrust.com | HTTP/80 | Služi za preuzimanje popise CRL-ova za MFA.
\*. windows.net | HTTP/443 | Koristi se za prijavu na Azure AD.
Secure.aadcdn.microsoftonline p.com | HTTP/443 | Koristi se za MFA.
\*. microsoftonline.com | HTTP/443 | Služi za konfiguriranje Azure AD direktorija i uvoz/izvoz podataka.

## <a name="errors-in-the-wizard"></a>Pogreške u čarobnjaku
Čarobnjak za instalaciju koristi dvije drugoj sigurnosnoj konteksta. Na stranici **Povezivanje s Azure AD** koristi trenutno prijavljeni korisnik. Na stranici **Konfiguracija** ga je promjena [računa pokretanja servisa za sinkronizaciju modula](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts). Konfiguracija proxy dajemo su globalni na računalu tako da ako je došlo do problema, problema će Većina vjerojatno već pojavljuje na stranici **Povezivanje s Azure AD** u čarobnjaku.

Ovo su najčešće pogreške prikazat će se u čarobnjaku za instalaciju.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>Čarobnjak za instalaciju nije ispravno konfigurirano
Ta se pogreška prikazat će se čarobnjak za samu ne može pristupiti proxy poslužitelj.
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

- Ako je ovo vidi, provjerite je li [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) ispravno konfigurirano.
- Ako koji izgleda pravilno, slijedite korake u [Provjeri proxy povezivanje](#verify-proxy-connectivity) da biste vidjeli ako se problem ne postoji izvan u čarobnjaku.

### <a name="the-mfa-endpoint-cannot-be-reached"></a>Nije moguće dohvatiti krajnju točku MFA
Ta se pogreška će se pojaviti ako krajnjoj točki **https://secure.aadcdn.microsoftonline-p.com** se ne može otvoriti, a globalni administrator ima MFA omogućena.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

- Ako to, provjerite je li dodati da secure.aadcdn.microsoftonline krajnja točka-p.com proxy poslužitelj.

### <a name="the-password-cannot-be-verified"></a>Lozinke se ne može provjeriti
Ako čarobnjak za instalaciju ne uspije u povezivanju s Azure AD, ali sam lozinku ne može provjeriti Primijetit ćete sljedeće: ![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

- Lozinka je privremenu lozinku i mora se promijeniti? Je li zaista ispravnu lozinku? Pokušajte se prijaviti https://login.microsoftonline.com (na drugom računalu od poslužitelj za Azure AD Connect) i provjerite je li se račun koristiti.

### <a name="verify-proxy-connectivity"></a>Provjerite je li povezivanje proxy poslužitelja
Da biste provjerili ima li poslužitelj za Azure AD Connect stvarni povezivanje s Proxy i Internet koristit ćemo neke komponente PowerShell da biste vidjeli ako je proxy dopuštanja web-zahtjevi ili ne. U odzivniku komponente PowerShell pokrenite `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Tehnički prvi poziv je https://login.microsoftonline.com i to će funkcionirati kao i, ali se brže da biste odgovorili na URI.)

PowerShell koristit će konfiguraciju u machine.config da se obratite proxy poslužitelj. Postavke u winhttp/netsh treba utjecati tih cmdleta.

Ako proxy poslužitelj je pravilno postavljen, trebali biste dobiti uspjeh status: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Ako vam se prikaže **nije moguće povezati s udaljeni poslužitelj** zatim PowerShell pokušava upućivanje poziva izravno bez korištenja proxy poslužitelja ili DNS nije ispravno konfigurirano. Provjerite je li pravilno konfiguriran u datoteku **machine.config** .
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Ako je proxy poslužitelj nije ispravno konfigurirano, ne možemo će se pogreška: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

Pogreška | Tekst pogreške | Komentar
---- | ---- | ---- |
403 | Zabranjen | Proxy poslužitelj nije otvoren za zatraženi URL. Ponovo posjetite konfiguracija proxy poslužitelja i provjerite je li otvorene [URL-ova](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) .
407 | Potrebna je provjera autentičnosti proxy poslužitelja | Proxy poslužitelj potrebna prijava i ništa nije naveden. Proxy poslužitelj zahtijeva provjeru autentičnosti, provjerite je li da bi to konfiguriran u na machine.config. I provjerite je li korisnik koji se pokreće čarobnjak kao i kao račun servisa koristite domenski računi.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Uzorak komunikacije između Azure AD Connect i Azure AD
Ako ste pratili sve korake iznad i još uvijek ne možete povezati sada može pokrenuti pogled na mreži zapisnika. U ovom se odjeljku je dokumentiranje običnom i uspjelo povezivanje s uzorkom. Je popis i uobičajenih herrings crvene boje koje možete zanemariti Ako čitate zapisnike mreže.

- Pojavit će se poziva https://dc.services.visualstudio.com. Nije obavezno da bi ovaj Otvori u proxy poslužitelja za instalaciju da, a to je zanemariti.
- Vidjet ćete da Razrješavanje dns prikazat će se popis stvarni domaćini u nsatc.net prostora za naziv DNS-a i druge prostora naziva nije u odjeljku microsoftonline.com. Međutim, neće biti sve zahtjeve za uslugu web na stvarni poslužitelja naziva i morati dodati ih na proxy poslužitelj.
- Krajnje točke adminwebservice i provisioningapi (pogledajte ispod u zapisnicima) su krajnje točke za otkrivanje i koristiti da biste pronašli stvarni krajnja točka za korištenje, a zatim će razlikovale ovisno o vašoj regiji.

### <a name="reference-proxy-logs"></a>Referenca proxy zapisnika
Evo ispis iz zapisnik stvarni proxy i stranica čarobnjaka za instalaciju iz gdje je snimljena (duplicirane stavke iste krajnjoj uklonjene su). To se može koristiti kao referenca za vlastite proxy i mrežne zapisnika. Stvarni krajnje točke se mogu razlikovati u svom okruženju (posebno onima u *kurziv*).

**Povezivanje s Azure AD**

Vrijeme | URL-A
--- | ---
11/1/2016 8:31 | Connect://login.microsoftonline.com:443
11/1/2016 8:31 | Connect://adminwebservice.microsoftonline.com:443
11/1/2016 8:32 | povezivanje: / /*bba800 sidrenja*. microsoftonline.com:443
11/1/2016 8:32 | Connect://login.microsoftonline.com:443
11/1/2016 8:33 | Connect://provisioningapi.microsoftonline.com:443
11/1/2016 8:33 | povezivanje: / /*bwsc02 prijenosnik*. microsoftonline.com:443

**Konfiguriranje**

Vrijeme | URL-A
--- | ---
11/1/2016 8:43 | Connect://login.microsoftonline.com:443
11/1/2016 8:43 | povezivanje: / /*bba800 sidro*. microsoftonline.com:443
11/1/2016 8:43 | Connect://login.microsoftonline.com:443
11/1/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
11/1/2016 8:44 | povezivanje: / /*bba900 sidro*. microsoftonline.com:443
11/1/2016 8:44 | Connect://login.microsoftonline.com:443
11/1/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
11/1/2016 8:44 | povezivanje: / /*bba800 sidro*. microsoftonline.com:443
11/1/2016 8:44 | Connect://login.microsoftonline.com:443
11/1/2016 8:46 | Connect://provisioningapi.microsoftonline.com:443
11/1/2016 8:46 | povezivanje: / /*bwsc02 prijenosnik*. microsoftonline.com:443

**Početna sinkronizacije**

Vrijeme | URL-A
--- | ---
11/1/2016 8:48 | Connect://login.Windows.NET:443
11/1/2016 8:49 | Connect://adminwebservice.microsoftonline.com:443
11/1/2016 8:49 | povezivanje: / /*bba900 sidrenja*. microsoftonline.com:443
11/1/2016 8:49 | povezivanje: / /*bba800 sidro*. microsoftonline.com:443

## <a name="authentication-errors"></a>Pogreške provjere autentičnosti
U ovom se odjeljku opisuje pogreške koje se može vratiti ADAL (u provjera autentičnosti biblioteke koristi Azure AD Connect) i PowerShell. Pogreška objašnjenje trebali biste pomoći u razumijevanje sljedeće korake.

### <a name="invalid-grant"></a>Dodjela koji nisu valjani
Nije valjan korisničko ime ili lozinka. Dodatne informacije potražite [Lozinka se ne može provjeriti](#the-password-cannot-be-verified) .

### <a name="unknown-user-type"></a>Vrsta nepoznatih korisnika
Azure AD direktorija ne može pronaći ili riješiti. Možda pokušavate se prijaviti pomoću korisničkog imena u Neprovjereni domene?

### <a name="user-realm-discovery-failed"></a>Otkrivanje za lokalni korisnik nije uspjela
Mrežni ili proxy problema s konfiguracijom. Mreža nije moguće otvoriti, pročitajte članak [otklanjanje problema s povezivanjem u čarobnjaku za instalaciju](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Istekla korisničke lozinke
Vjerodajnice su od njih istekle. Promjena lozinke.

### <a name="authorizationfailure"></a>AuthorizationFailure
Nepoznat problem.

### <a name="authentication-cancelled"></a>Provjera autentičnosti otkazana
Višestruka provjera autentičnosti (MFA) test je otkazan.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Provjera autentičnosti je uspjela, ali Azure AD PowerShell ima problema s provjere autentičnosti.

### <a name="azurerolemissing"></a>AzureRoleMissing
Provjera autentičnosti nije uspjelo. Niste globalni administrator.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Provjera autentičnosti nije uspjelo. Upravljanje identitetom povlaštene omogućio i trenutno ne globalni administrator. Dodatne informacije potražite u članku [Upravljanje povlaštene identitet](active-directory-privileged-identity-management-getting-started.md) .

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Provjera autentičnosti nije uspjelo. Informacije o tvrtki iz Azure AD se ne može dohvatiti.

### <a name="retrievedomains"></a>RetrieveDomains
Provjera autentičnosti nije uspjelo. Može dohvatiti informacija o domeni iz Azure AD.

## <a name="troubleshooting-steps-for-previous-releases"></a>Upute za otklanjanje poteškoća za prethodnog izdanja.
Pomoćnik za prijavu u povučena je s izdanja počevši od broj izdanja 1.1.105.0 (objavljeno veljača 2016). Ova sekcija i konfiguraciji treba više nije potrebna, ali se zadržavaju kao referenca.

Za na jedinstvene prijave u Pomoćnik za rad, morate ga konfigurirati winhttp. To se može učiniti s [**netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>Pomoćnik za prijavu nije ispravno konfigurirano
Ta se pogreška se Pomoćnik za prijavu u ne može pristupiti proxy poslužitelj i proxy poslužitelj ne dopušta zahtjev.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

- Ako to, pogledajte konfiguracija proxy poslužitelja u [netsh](active-directory-aadconnect-prerequisites.md#connectivity) i provjerite je li on točan.
![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
- Ako koji izgleda pravilno, slijedite korake u [Provjeri proxy povezivanje](#verify-proxy-connectivity) da biste vidjeli ako se problem ne postoji izvan u čarobnjaku.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
