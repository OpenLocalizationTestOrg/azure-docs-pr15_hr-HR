<properties
    pageTitle="Postavljanje automatskog registracije domene pridruženo uređaje sa sustavom Windows s Azure Active Directory | Microsoft Azure"
    description="Postavljanje domene pridruženo uređaje sa sustavom Windows da biste registrirali automatski i tihu s Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="markvi"/>



# <a name="set-up-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Postavljanje automatskog Registracija sustava Windows domene pridruženo uređajima pomoću servisa Azure Active Directory

Da biste koristili [Azure Active Directory uređaj pristupom utemeljeno na uvjetno](active-directory-conditional-access.md), Windows domene pridruženo računala mora biti registriran u Azure Active Directory (Azure AD). U ovom se članku možete saznati što vam je potrebno učiniti da biste postavili registracije domene pridruženo uređaje sa sustavom Windows s Azure AD u tvrtki ili ustanovi.

Korištenje uvjetnog programa access u Azure AD pruža sljedeće prednosti:

-   Poboljšana jedinstvenu prijavu (SSO) pojaviti u aplikacijama za Azure AD pomoću računa tvrtke ili obrazovne ustanove
-   Enterprise roaming postavki putem uređaja
-   Pristup iz Windows trgovine
-   Bolja o provjeri autentičnosti i praktične prijavite se pomoću Windows pozdrav

> [AZURE.NOTE] Windows 10 studenom Update nudi neke poboljšane korisničkog sučelja u Azure AD, ali Obljetnica ažuriranja za Windows 10 potpuno podržava pristup uvjetno utemeljen na uređaj. Dodatne informacije o uvjetno access potražite u članku [Azure Active Directory utemeljen na uređaju uvjetno pristup](active-directory-conditional-access.md). Dodatne informacije o uređaja Windows 10 na radnom mjestu i način na koji korisnik registrira uređaj sa sustavom Windows 10 s Azure AD potražite u članku [Windows 10 za tvrtke: Korištenje uređaja za posao](active-directory-azureadjoin-windows10-devices-overview.md).

Možete registrirati neke starije verzije sustava Windows, uključujući sljedeće verzije:

-   Windows 8.1
-   Windows 7

Ako koristite Windows Server računalu kao radnu površinu, možete registrirati te platforme:

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012.
- Windows Server 2008 R2


## <a name="prerequisites"></a>Preduvjeti

Glavni obavezu automatsko registracije domene pridruženo uređajima pomoću Azure AD je imati ažuran verziju Azure Active Directory povezivanje (Azure AD Connect).

Ovisno o tome kako uvesti Azure AD Connect i li koristiti eksplicitnih ili prilagođenu instalaciju ili nadogradnju na mjestu, sljedeći preduvjeti možda nije konfiguriran automatski:

-   **Točka povezivanja servisa u lokalnim servisom Active Directory**. Za otkrivanje Azure AD informacije o klijentu i računala koja se registrirati za Azure AD.

-   **Active Directory Federation Services (AD FS) izdavanja transformacija pravila**. Za provjeru autentičnosti računala na Registracija (primjenjivo pridruženim konfiguracije).

Ako neki uređaji u vaše tvrtke ili ustanove nisu domene pridruženo uređaja Windows 10, provjerite je li izvršiti sljedeće zadatke:

- Postavljanje pravila u Azure AD da bi korisnici mogli registrirati uređaja
- Postavljanje integriranu provjeru autentičnosti sustava Windows (IWA) kao valjani zamjena za višestruke provjere autentičnosti u AD FS



## <a name="set-a-service-connection-point-for-discovery-of-the-azure-ad-tenant"></a>Postavljanje točke povezivanja usluge za otkrivanje klijenta Azure AD

Objekt točke za povezivanje servisa mora postojati u konfiguraciji imenovanja kontekst particija od skupa stabala domene koje su se pridružili računala. Točka povezivanja servisa sadrži otkrivanje podatke o klijentu Azure AD gdje se registrirati računala. U konfiguraciji za Active Directory s više skupa stabala točke povezivanja servisa moraju se nalaziti u sve šuma koji imaju domene pridruženo računala.

Postavljanje točke povezivanja servisa na sljedećim mjestima u servisu Active Directory:

    CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Configuration Naming Context]

> [AZURE.NOTE] Za skup stabala na na servisa Active Directory domene naziv *example.com*, konfiguracije imenovanja kontekst je CN = Konfiguracija Kontroler =, primjerice, Kontroler = com.

Za Azure AD klijentu objekta i otkrivanje vrijednosti možete provjeriti pomoću sljedeće skripte komponente Windows PowerShell. (Zamijenite kontekst imenovanja konfiguraciju u primjeru kontekst imenovanja konfiguracije.)

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com";

    $scp.Keywords;

Na `$scp.Keywords` izlaz prikazuju se podaci o klijentu za Azure AD:

    azureADName:microsoft.com

    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Ako ne postoji točka povezivanja servisa, stvorite ga tako da pokrenete sljedeću skriptu komponente PowerShell na poslužitelj za Azure AD Connect:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


> [AZURE.NOTE]Kada pokrenete `$aadAdminCred = Get-Credential`, upotrijebite oblik *user@example.com* za korisničko ime u dijaloškom okviru **Dohvaćanje vjerodajnica** .  
> Kada pokrenete cmdlet za inicijalizaciju ADSyncDomainJoinedComputerSync, zamijenite [*naziv računa poveznik*] račun domene koja se koristi u poveznik za račun servisa Active Directory.  
> Cmdlet koristi Active Directory PowerShell modul koji ovisi servisa Active Directory Web Services u kontroler domene. Active Directory Web Services podržava kontrolera domena u sustavu Windows Server 2008 R2 i novijim verzijama. Za kontrolera domena u sustavu Windows Server 2008 ili starije verzije, pomoću API System.DirectoryServices PowerShell da biste stvorili točke povezivanja servisa, a zatim dodijeliti vrijednosti ključne riječi.

## <a name="create-ad-fs-rules-for-instant-device-registration-in-federated-organizations"></a>Stvaranje pravila za AD FS za Registracija uređaja trenutno u pridruženim tvrtke ili ustanove

U pridruženim Azure AD Konfiguracija uređaja je AD FS (ili na vanjskom poslužitelju lokalnog) za provjeru autentičnosti Azure AD. Nakon toga ih registrirati protiv Azure servis Active Directory uređaj Registracija (servis za registraciju uređaja Azure AD).

> [AZURE.NOTE] U konfiguraciji koje nisu pridružene raspršivanja lozinku za korisnika se sinkroniziraju za Azure AD i Windows 10 i Windows Server 2016 domene pridruženo računalima provjeru autentičnosti servis za registraciju uređaja Azure AD. Korisnik se autentificira servisa pomoću vjerodajnica koje korisnik zapisuju u svoje račune računala lokalnog i koje se povezivati za Azure AD putem Azure AD Connect. Koje nisu iz programa Windows 10 i Windows Server 2016 računala u konfiguraciji koje nisu pridružene, imate mogućnosti postavki utemeljen na uređaju ustanova za izdavanje potvrda u tvrtki ili ustanovi. Dodatne informacije potražite u članku paketa preuzeti Windows Installer sekcije za računala koje nisu iz programa Windows 10 u ovom članku.

Za računala sa sustavom Windows 10 i Windows Server 2016, Azure AD Connect pridružuje objekt uređaja u Azure AD objekta poslovnog subjekta lokalnog računala. Tijekom provjere autentičnosti za servis za registraciju Azure AD uređaj da biste dovršili registraciju i stvaranje objekt uređaja mora postojati zahtjevima za sljedeće:

- http://schemas.microsoft.com/ws/2012/01/accounttype sadrži vrijednost DJ koja služi za identifikaciju glavni autentikatora kao i domene pridruženo računalu.
- http://schemas.microsoft.com/Identity/claims/onpremobjectguid sadrži vrijednost atributa **objectGUID** račun lokalnog računala.
- http://schemas.microsoft.com/ws/2008/06/Identity/claims/primarysid sadrži računala primarni sigurnosni identifikator (ID), koji odgovara vrijednost atributa **objectSid** računa lokalnog računala.
- http://schemas.microsoft.com/ws/2008/06/Identity/claims/issuerid sadrži vrijednost koja se koristi Azure AD pouzdanosti znak izdan iz AD FS ili iz na lokalni sigurnosnih tokena servisa (STS). To je važno u konfiguraciji više skupa stabala servisa Active Directory. U ovoj konfiguraciji računala može biti pridruženo skupa stabala koja povezuje Azure AD pri AD FS ili lokalni STS. Za AD FS koristite vrijednost http://<*naziv domene*>/adfs/services/pouzdanost /, pri čemu <*naziv domene*> je naziv provjerene domene u Azure AD.

Da biste ručno stvorili ta pravila u AD FS koristite sljedeću skriptu komponente PowerShell u sesiji koji je povezan s poslužiteljem. Zamijenite prvi redak naziv domene provjerene tvrtke ili ustanove u Azure AD.

> [AZURE.NOTE] Ako ne koristite AD FS za lokalni poslužitelj za vanjski pristup, slijedite upute za proizvođaču tog za stvaranje pravila za taj problem tih zahtjeva.

    $validatedDomain = "example.com"      # Replace example.com with your organization's validated domain name in Azure AD

    $rule1 = '@RuleName = "Issue object GUID"

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

        => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain-joined computers"

      c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

> [AZURE.NOTE] Ako je vaše okruženje lokalnog jedan skup stabala, koristite samo prva tri pravila. Ako u drugi skup stabala od jednog sinkronizaciju s Azure AD računala ili ako koristite zamjenske nazive onima u konfiguraciji sinkronizacije, morate uključiti i preostalih pravila.

    $rule3 = '@RuleName = "Pass through primary SID"

      c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(claim = c2);'

    $rule4 = '@RuleName = "Issue AccountType with the value User when it’s not a computer account"

      NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"])

      => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");'

    $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID"

      c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));'

    $rule6 = '@RuleName = "Update issuer for DJ computer auth"

      c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$validatedDomain+'/adfs/services/trust/");'

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5 + $rule6

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

> [AZURE.NOTE] Windows 10 i Windows Server 2016 domene pridruženo računalima autentičnost pomoću IWA aktivni WS pouzdanost krajnju točku koja se održava AD FS. Provjerite je li krajnju točku aktivna. Ako koristite proxy poslužitelj aplikacije za Web, obavezno ovaj krajnjoj točki objavljeno putem proxy poslužitelj. Krajnja točka je adfs/services/pouzdanost/13/windowstransport. Da biste provjerili je li aktivan u konzola za AD FS idite na **servis** > **krajnje točke**. Ako ne koristite AD FS za lokalni poslužitelj za vanjski pristup, slijedite upute za proizvođaču tog da biste bili sigurni aktivan odgovarajuće krajnjoj točki.



## <a name="set-up-ad-fs-for-authentication-of-device-registration"></a>Postavljanje AD FS za provjeru autentičnosti Registracija uređaja

Provjerite je li IWA postavljena kao valjani zamjena za višestruke provjere autentičnosti za registraciju uređaja u AD FS. Da biste to učinili, morate koristiti programa izdavanja transformacija pravilo koje prolazi kroz načina provjere autentičnosti.

1. Konzola za upravljanje AD FS, idite na **AD FS** > **Odnosima pouzdanosti** > **Potrebe za oslanjanjem strana smatra pouzdanima**.

2. Desnom tipkom miša kliknite pouzdanost objekt za relying davatelja identiteta platformu za Microsoft Office 365, a zatim odaberite **Pravila zahtjeva za uređivanje**.

3.  Na kartici **Pravila transformacija izdavanja** odaberite **Dodaj pravilo**.

4.  Na popisu **pravila zahtjeva** predložaka, odaberite **Pošalji zahtjevima pomoću pravila Prilagođeno**.

5.  Odaberite **Dalje**.

6.  U okvir **naziv pravila zahtjeva** , unesite **Pravila zahtjeva metoda provjere autentičnosti**.

7.  U okvir **pravila zahtjeva** , unesite sljedeću naredbu:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
    => issue(claim = c);`.

8. Na vanjskom poslužitelju unesite sljedeću naredbu komponente PowerShell:

    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

<*RPObjectName*> je naziv objekta relying proizvođača za Azure AD relying strana pouzdanost objekta. Taj objekt obično naziva *Platformu identiteta za Microsoft Office 365*.



## <a name="deployment-and-rollout"></a>Uvođenje i uvođenje

Kada domenu pridruženo računala zadovoljava preduvjete, jesu li spremni za registriranje Azure AD.

Godišnjica ažuriranja za Windows 10 i Windows Server 2016 domene pridruženo računala automatski registrirate Azure AD sljedeći put ponovnog pokretanja uređaja ili prilikom prijave u Windows. Registrirajte se nova računala koji su povezani s domenom s Azure AD prilikom ponovnog pokretanja uređaja nakon Operacija pridruživanja domene.

> [AZURE.NOTE] Računala domene pridruženo Windows 10 automatski registrirate Azure AD samo ako je postavljena uvođenje objekt pravilnika grupe.

Objekt pravila grupe možete koristiti da biste odredili uvođenje automatsko Registracija Windows 10 i Windows Server 2016 domene pridruženo računala. Distribucija automatsko registracije domene pridruženo računala koje nisu iz programa Windows 10, možete implementirati paket programa Windows Installer za računala koje ste odabrali.

> [AZURE.NOTE] Pravila grupe za kontrolu uvođenje pokreće i registracije domene pridruženo računala Windows 8.1. Možete koristiti pravila za registriranje računala domene pridruženo Windows 8.1. Ili, ako imate više verzija sustava Windows, uključujući verzije sustava Windows 7 ili Windows Server, možete registrirati sve koji nisu iz programa Windows 10 i Windows Server 2016 računala pomoću paketa Windows Installer.

### <a name="create-a-group-policy-object-to-control-the-rollout-of-automatic-registration"></a>Stvaranje objekta pravilnika grupe da biste odredili uvođenje automatsko Registracija

Da biste odredili uvođenje automatsko registracije domene pridruženo računalima s Azure AD, možete implementirati pravilnik grupe **registrirali domenu pridruženo računala kao uređaji** za računala želite registrirati. Na primjer, možete implementirati pravila Organizacijska jedinica ili sigurnosne grupe.

Da biste postavili pravilnik, učinite sljedeće:

1. Otvorite upravitelj poslužitelja, a zatim **Alati** > **Upravljanje pravilnikom grupe**.

2. Idite na čvor domene koja odgovara domenu koju želite aktivirati automatski Registracija računala za Windows 10 ili Windows Server 2016.

3. Desnom tipkom miša kliknite **Objekti pravilnika grupe**, a zatim odaberite **Novo**.

4. Unesite naziv za objekt pravilnika grupe. Na primjer, *Automatsko Registracija Azure AD*. Odaberite **u redu**.

4. Desnom tipkom miša kliknite novi objekt pravilnika grupe, a zatim odaberite **Uređivanje**.

5. Idite na **konfiguracije računala** > **pravila** > **Administrativni predlošci** > **komponente Windows** > **Registracija uređaja**. Desnom tipkom miša kliknite **domene Register pridruženo računala kao uređaja**, a zatim odaberite **Uređivanje**.

    > [AZURE.NOTE] Ovaj predložak pravila grupe preimenovana je u prethodnim verzijama programa konzole za upravljanje pravilnikom grupe. Ako koristite stariju verziju na konzoli, idite na **Konfiguracije računala** > **pravila** > **Administrativni predlošci** > **Komponente Windows** > **Radnom mjestu spoj** > **automatski radnom mjestu spoj klijentskim računalima**.

6. Odaberite **omogućeno**, a zatim odaberite **Primijeni**.

7. Odaberite **u redu**.

8. Veza na objekt pravilnika grupe na mjesto po izboru. Na primjer, možete povezati ga određene Organizacijska jedinica. Također mogli povezati ga na određene sigurnosne grupe računala koja se automatski morate registrirati Azure AD. Da biste postavili ovo pravilo za sve domene pridruženo Windows 10 i Windows Server 2016 računala u vašoj tvrtki ili ustanovi, povezati objekt pravilnika grupe domene.

### <a name="download-windows-installer-packages-for-non-windows-10-computers"></a>Preuzimanje paketa Windows Installer za računala koje nisu iz programa Windows 10  

Da biste registrirali domenu pridruženo računalima sa sustavom Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 ili Windows Server 2008 R2, možete preuzeti i instalirati te datoteke programa Windows Installer package (.msi):

- [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
- [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

Implementacija značajke pakiranja pomoću sustava raspodjele softver kao što je Upravitelj konfiguracije centar sustava. Paket podržava na standardni tihu mogućnosti instalacije s parametrom *Tihi* . Upravitelj konfiguracije centar sustava 2016 nudi dodatne pogodnosti iz starijih verzija, kao što je mogućnost za praćenje dovršenih registracije. Dodatne informacije potražite u članku [2016 centar sustava](https://www.microsoft.com/en-us/cloud-platform/system-center).

Instalacijski program stvara zakazani zadatak u sustavu koji se izvodi u kontekstu korisnika. Zadatak se pokreće kada korisnikove prijave u Windows. Zadatak tihu registrira uređaj s Azure AD pomoću korisničke vjerodajnice nakon provjere autentičnosti putem IWA. Da biste vidjeli zakazani zadatak, otvorite **Microsoft** > **Uključivanje radnom mjestu**, a zatim idite u biblioteku raspored zadataka.



## <a name="next-steps"></a>Daljnji koraci

- [Azure Active Directory uvjetno pristup](active-directory-conditional-access.md)
