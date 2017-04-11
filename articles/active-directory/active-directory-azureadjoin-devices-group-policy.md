<properties
    pageTitle="Povezivanje domene pridruženo uređaji Azure AD za Windows 10 iskustvo | Microsoft Azure"
    description="U članku se objašnjava kako administratori mogu konfigurirati pravila grupe da biste omogućili uređaje da biste se pridružili domene na mrežu tvrtke."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Povezivanje domene pridruženo uređaji Azure AD za Windows 10 sučelja

Uključivanje domena je način tvrtkama ili ustanovama koje ste povezali uređaji za rad za zadnjih 15 godina i još mnogo toga. Ima omogućeno korisnika da se prijavite se u svoje uređaje pomoću rade Windows Server Active Directory (Active Directory) ili školske račune i dopušteno IT potpuno upravljanje sljedećim uređajima. Tvrtke i ustanove obično ovise o slike metode uređaji dodjele resursa korisnicima i obično pomoću upravitelja konfiguracije sustava Center (SCCM) ili pravilnik grupe da biste upravljali njima.

Uključivanje domene u sustavu Windows 10 pronaći ćete sljedeće prednosti kada povežete uređaje Azure Active Directory (Azure AD):

- Jedinstvenu prijavu (SSO) za Azure AD resursi s bilo kojeg mjesta
- Pristup enterprise iz Windows trgovine pomoću računa tvrtke ili obrazovne ustanove (nema Microsoftova računa obavezno)
- Enterprise usklađen roaming od korisničkih postavki svim uređajima pomoću računa tvrtke ili obrazovne ustanove (nema Microsoftova računa obavezno)
- Jaka provjera autentičnosti i praktične prijava za tvrtke ili obrazovne ustanove račune za Microsoft Passport i Windows pozdrav
- Mogućnost da biste ograničili pristup samo uređaje koji se u skladu s postavke pravilnika grupe uređaj tvrtke ili ustanove

## <a name="prerequisites"></a>Preduvjeti

Uključivanje domenu i dalje biti korisno. Međutim, da biste dobili Azure AD pogodnosti SSO, roaming postavki s rad ili školske računi i pristup iz Windows trgovine pomoću računa tvrtke ili obrazovne ustanove, morat ćete sljedeće:

- Azure AD pretplate
- Azure AD Connect da biste proširili lokalnog imenika za Azure AD
- Pravilo koje je postavio povezati domene pridruženo uređaji Azure AD
- Windows 10 Sastavi (Sastavi 10551 ili novija verzija) za uređaje

Da biste omogućili Microsoft Passport za Službeni telefon i Windows pozdrav, također ćete sljedeće:

- Infrastruktura javnog ključa (PKI) za izdavanja potvrde korisnika.
- Sustav centar Upravitelj konfiguracije verzija 1509 za Tehnički pretpregled. Dodatne informacije potražite u članku [Microsoft sustava centar za konfiguraciju Manager (Tehnički pretpregled)](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) i [Blog tima za sustav centar Upravitelj konfiguracije](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx). Potrebna je za implementaciju korisničkih certifikata koji se temelji na Microsoft Passport tipke.

Kao zamjena za implementaciju obavezne PKI, možete učiniti sljedeće:

- Imate nekoliko kontrolera domena sa sustavom Windows Server 2016 Active Directory Domain Services.

Da biste omogućili pristup uvjetno, možete stvoriti postavke pravilnika grupe koji omogućuju pristup domene pridruženo uređaja s bez dodatnih implementacije. Da biste upravljali kontrola pristupa na temelju usklađenost uređaja, će vam sljedeće:

- Upravitelj konfiguracije centar sustava verzije 1509 za Tehnički pretpregled Passport scenarijima za

## <a name="deployment-instructions"></a>Upute za implementaciju



### <a name="step-1-deploy-azure-active-directory-connect"></a>Korak 1: Implementacija Azure Active Directory povezati

Azure AD Connect omogućit će vam Dodjela računala lokalnog kao objekata uređaja u oblaku. Da biste implementirali Azure AD Connect, pogledajte "Instaliraj Azure AD Connect" u članku [integriranje vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md#install-azure-ad-connect).

 - Ako ste pratili na [prilagođenu instalaciju za Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md) (ne instalacija Express), slijedite postupak za **Stvaranje veza servisa pokažite na lokalni Active Directory**, kasnije u ovom ćete koraku.
 - Ako imate pridruženim konfiguraciji s Azure AD prije instalacije Azure AD Connect (na primjer, ako ste implementiran Active Directory Federation Services (AD FS) prije), zatim slijedite postupak **pravila zahtjeva za konfiguriranje AD FS** , kasnije u ovom ćete koraku.

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>Stvaranje točke povezivanja usluge u lokalni Active Directory

Domene pridruženo uređaja koristit će točke povezivanja servisa za otkrivanje Azure AD klijentu podataka u trenutku automatsko Registracija sa servisom Azure uređaj Registracija.

Na poslužitelj za Azure AD Connect pokrenite sljedeće naredbe ljuske PowerShell:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


Kada se pokrene cmdlet $aadAdminCred = Get-vjerodajnica koriste oblik *user@example.com* za korisničko ime vjerodajnica uneseni kada se pojavi skočni prozor za dohvaćanje vjerodajnica.

Kada se pokrene cmdlet od inicijalizaciju ADSyncDomainJoinedComputerSync [*naziv računa poveznik*] zamijenite račun domene koja se koristi kao poveznik za račun servisa Active Directory.

#### <a name="configure-ad-fs-claim-rules"></a>Konfiguriranje pravila zahtjeva za AD FS
Konfiguriranje pravila zahtjeva za AD FS omogućuje trenutačno Registracija računala pomoću servisa Azure uređaja Registracija dopuštanjem računalima za provjeru autentičnosti putem Kerberos/NTLM putem servisa AD FS. Bez ovaj korak računala će dobiti za Azure AD odgođeno način (primjenjuje Azure AD Connect sinkronizaciju vremena).

>[AZURE.NOTE]
Ako nemate AD FS kao na vanjski pristup informacije o lokalnom poslužitelju, slijedite upute od dobavljača da biste stvorili zahtjev pravila.

Na poslužitelju za AD FS (ili na sesiju povezani s poslužiteljem za AD FS), pokrenite sljedeće naredbe ljuske PowerShell:

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Pomoću Windows integrirane provjere autentičnosti za krajnje WS pouzdanost aktivni hostira tvrtka AD FS će provjeriti autentičnost računala za Windows 10. Provjerite je li omogućen ovaj krajnjoj točki. Ako koristite provjeru autentičnosti proxyja Web i provjerite je li ovo krajnjoj točki objavljeno putem proxy. To možete učiniti tako da u adfs/services/pouzdanost/13/windowstransport. Trebali biste prikazali kao omogućeni u AD FS konzoli za upravljanje u odjeljku **servis** > **krajnje točke**.


### <a name="step-2-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>Korak 2: Konfiguriranje Registracija uređaja za automatsko putem pravilnika grupe u servisu Active Directory

Da biste konfigurirati svoje uređaje domene pridruženo Windows 10 automatski morate registrirati Azure AD možete koristiti pravila grupe u servisu Active Directory.

> [AZURE.NOTE]
> Najnovije upute za postavljanje Registracija automatsko uređaja potražite u člancima, [upute za postavljanje automatskog registracije domene Windows pridruženo uređaja s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
>
> Ovaj predložak pravila grupe preimenovana je u sustavu Windows 10. Ako koristite alat za pravila grupe s računala za Windows 10, pravila prikazat će se kao: <br>
> **Registrirali domenu pridruženo računala kao uređaja**<br>
> Pravilo je na sljedećem mjestu:<br>
> ***Registracija komponente/uređaja konfiguracije/pravila/Administrativni predlošci/Windows za računala***


## <a name="additional-information"></a>Dodatne informacije
* [Windows 10 za tvrtke: načini korištenja uređaji za posao](active-directory-azureadjoin-windows10-devices-overview.md)
* [Proširivanje mogućnosti oblak na uređajima Windows 10 do Azure Active Directory uključivanje](active-directory-azureadjoin-user-upgrade.md)
* [Saznajte više o korištenju scenariji za Azure AD uključivanje](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Povezivanje domene pridruženo uređaji Azure AD za Windows 10 sučelja](active-directory-azureadjoin-devices-group-policy.md)
* [Postavljanje uključivanje za Azure AD](active-directory-azureadjoin-setup.md)
