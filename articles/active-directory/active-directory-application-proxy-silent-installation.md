<properties
    pageTitle="Tihu instalaciji Azure AD aplikacije Proxy poveznik | Microsoft Azure"
    description="Opisuje kako izvesti tihu instalaciju Azure AD aplikacije Proxy poveznik za sigurne daljinski pristup lokalnog aplikacija."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>Kako tihu instalirati Azure AD aplikacije Proxy poveznik

Želite da biste mogli slati instalacijskog skripta više poslužitelja sustava Windows ili Windows poslužiteljima koji nemaju korisničko sučelje omogućena. U ovoj se temi objašnjava kako stvoriti skriptu komponente Windows PowerShell koja omogućuje nenadziranog instalaciju da biste instalirali i registrirali Azure poveznik AD aplikacije proxy poslužitelja.

## <a name="enabling-access"></a>Omogućivanje pristupa
Proxy poslužitelj aplikacije radi instalacijom Tanki servisa Windows Server naziva poveznika u vašoj mreži. Poveznik Proxy aplikacije za rad ga mora biti registriran u direktoriju Azure AD pomoću globalni administrator i lozinke. Obično je to unijeti tijekom instalacije poveznika u skočnom dijaloškog okvira. Osim toga, možete koristiti Windows PowerShell da biste stvorili objekt vjerodajnica da biste unijeli vaši podaci za registraciju ili možete stvoriti vlastite token i koristite da biste unijeli vaši podaci za registraciju.

## <a name="step-1--install-the-connector-without-registration"></a>Korak 1: Instalacija poveznika bez Registracija


Instalacija poveznika MSIs bez registracije poveznik na sljedeći način:


1. Otvorite naredbeni redak.
2. Pokrenite sljedeću naredbu u kojem se/q znači Tihi instalacija – instalacija nije zatražit će da prihvatite licencni ugovor za krajnjeg korisnika.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>Korak 2: Registrirate poveznik Azure Active Directory
To je moguće napraviti pomoću bilo koju od sljedećih načina:


- Registrirajte se Connector pomoću vjerodajnica objekt programa Windows PowerShell
- Registrirajte se Connector pomoću token stvoren u izvanmrežnom načinu

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Registrirajte se poveznik pomoću vjerodajnica objekt programa Windows PowerShell


1. Stvaranje objekta vjerodajnice sustava Windows PowerShell ponovnim pokretanjem sljedećeg, gdje "<username>"i"<password>" treba zamijeniti pomoću korisničkog imena i lozinke za direktorija:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword

2. Idite na **Proxy poveznik za c:\Programske datoteke\Microsoft AAD aplikacija** i pokrenuti skriptu pomoću objekta vjerodajnice PowerShell koju ste stvorili, pri čemu je $cred naziv PowerShell vjerodajnice objekta koji ste stvorili:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred


### <a name="register-the-connector-using-a-token-created-offline"></a>Registrirajte se poveznik pomoću token stvoren u izvanmrežnom načinu

1. Stvaranje izvanmrežne token koji se pomoću klase AuthenticationContext vrijednostima u isječak koda:


        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }





2. Nakon što dodate token stvorite SecureString pomoću tokena: <br>
`$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`
3. Pokrenite sljedeću naredbu komponente Windows PowerShell, pri čemu je SecureToken naziv token koji ste stvorili iznad, a tenantID GUID klijenta sustava: <br>
`RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`



## <a name="see-also"></a>Vidi također

- [Omogućivanje Proxy aplikacije za Azure Active Directory](active-directory-application-proxy-enable.md)
- [Objavljivanje aplikacije koje koriste naziv domene](active-directory-application-proxy-custom-domains.md)
- [Omogućivanje jedinstvene prijave na](active-directory-application-proxy-sso-using-kcd.md)
- [Otklanjanje poteškoća s Proxy aplikacije](active-directory-application-proxy-troubleshoot.md)

Najnovije vijesti i ažuriranja, potražite u članku [blog Proxy aplikacije](http://blogs.technet.com/b/applicationproxyblog/)
