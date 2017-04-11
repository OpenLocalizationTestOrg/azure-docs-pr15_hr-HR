<properties
    pageTitle="Korištenje Azure ključa sigurnog iz web-aplikacije | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča Naučite koristiti Azure ključ sigurnog iz web-aplikacije."
    services="key-vault"
    documentationCenter=""
    authors="adhurwit"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-azure-key-vault-from-a-web-application"></a>Korištenje Azure ključa sigurnog iz web-aplikacije #

## <a name="introduction"></a>Uvod  
Pomoću ovog praktičnog vodiča Naučite koristiti Azure ključ sigurnog iz web-aplikacije u Azure. Ga vodit će vas kroz postupak pristupa na tajna iz zbirke ključeva programa za Azure ključ tako da se mogu koristiti u web-aplikaciji.

**Procijenjena vrijeme da biste dovršili:** 15 minuta


Pregled informacija o sigurnog ključ Azure, potražite u članku [što je sigurnog ključ Azure?](key-vault-whatis.md)

## <a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj Praktični vodič, morate imati sljedeće:

- URI za tajna u programa sigurnog ključ Azure
- ID klijenta i tajna klijenta za web-aplikaciju registriran Azure Active Directory koja ima pristup vašem sigurnog ključ
- Web-aplikacija. Će smo prikazuje korake za aplikaciju ASP.NET MVC u uveden u Azure kao web-aplikacijama.

> [AZURE.NOTE]  To je ključna dovršite korake navedene u [Početak rada s sigurnog Azure ključ](key-vault-get-started.md) za ovog praktičnog vodiča da bi URI na tajna i ID klijenta i tajna klijenta za web-aplikaciju.

Web-aplikacije koja će pristup sigurnog ključ jest ona koja je registrirana u servisu Azure Active Directory i je dodijelio pristup vašem sigurnog ključ. Ako to nije tako, vratite se u registar aplikacije praktičnog vodiča za početak rada i ponovite navedene korake.

Pomoću ovog praktičnog vodiča namijenjen je razvojni inženjeri osnove stvaranja web-aplikacije na Azure. Dodatne informacije o Azure Web Apps potražite u članku [Pregled web-aplikacije](../app-service-web/app-service-web-overview.md).



## <a id="packages"></a>Dodavanje Nuget paketa ##
Postoje dvije paketi koji web-aplikacije mora imati instaliran.

- Provjera autentičnosti biblioteke imenika Active Directory - sadrži načina za interakciju s Azure Active Directory i upravljanje identitet korisnika
- Azure biblioteka sigurnog ključ - sadrži načina za interakciju s sigurnog ključ Azure


Obje te pakete mogu instalirati korištenju konzole za Upravitelj paketa pomoću naredbe za instalaciju paketa.

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Izmjena Web.Config ##
Postoje tri postavke aplikacije koje je potrebno dodati u datoteku web.config na sljedeći način.

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Ako ne namjeravate hostira vaša aplikacija kao web-aplikaciju programa Azure, trebali biste dodati stvarnih vrijednosti ClientId, tajna klijenta i tajna URI datoteka web.config. U suprotnom ostavite te sustava vrijednosti jer smo će dodati stvarnih vrijednosti na portalu Azure za dodatnu razinu zaštite.


## <a id="gettoken"></a>Dodavanje načina da biste pristupili programa tokena ##
Da biste mogli koristiti API sigurnog ključ morate token za pristup. Klijent sigurnog ključ rukuje pozive sigurnog API ključa, ali potrebno navesti pomoću funkcija koja se dobiva token za pristup.  

Sljedeći kod da biste pristupili programa tokena iz servisa Azure Active Directory. Kod bilo kojeg mjesta možete prijeći u aplikaciji. Mogu li da biste dodali uslužni ili EncryptionHelper predmete.  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [AZURE.NOTE] 
> Najjednostavniji način za provjeru autentičnosti programa Azure AD pomoću ID klijenta i tajna klijent je. I korištenje u web-aplikaciji omogućuje odvojenosti te obavezama koje imate veću kontrolu nad sustava upravljanja ključem. No to ovise o umetanja tajna klijenta u konfiguracijske postavke koje se za neke mogu biti opasan, kao stavljanja tajna koji želite zaštititi u postavkama konfiguracije. Potražite u nastavku raspravu o tome kako koristiti ID klijenta i potvrda umjesto ID klijenta i tajna klijenta za provjeru autentičnosti Azure AD aplikacije.



## <a id="appstart"></a>Dohvaćanje tajna na pokretanje aplikacije ##
Sada moramo kod da biste pozvali sigurnog API ključ ili dohvatiti na tajna. Sljedeći kod se može smjestiti bilo gdje dok god se zove prije potrebno je koristiti. Kod ste staviti u događaj pokretanje aplikacije u Global.asax tako da ga pokreće jednom pri otvaranju i na tajna postaje dostupan za aplikaciju.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]).Result.Value;

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec;



## <a id="portalsettings"></a>Dodavanje postavki aplikacije na portalu za Azure (nije obavezno) ##
Ako imate web-aplikaciju programa Azure sada možete dodavati stvarnih vrijednosti za AppSettings na portalu za Azure. Time stvarnih vrijednosti neće biti u datoteka web.config, ali zaštićeni putem portala za koje se mogućnosti upravljanja zasebnog pristupa. Ove vrijednosti će biti zamjena za vrijednosti koje ste unijeli u vašem web.config. Provjerite jesu li nazivi isti.

![Postavke aplikacije prikazuju na portalu za Azure][1]


## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Autentičnost s potvrdom umjesto klijent tajna
Drugi način za provjeru autentičnosti programa Azure AD je pomoću ID klijenta i certifikat umjesto ID klijenta i tajna klijenta. Slijede upute za korištenje potvrde u web-aplikaciji programa Azure:

1. Dobivanje ili stvaranje certifikata
2. Certifikat pridružite program Azure AD
3. Dodavanje koda na web-aplikaciju za korištenje potvrde
4. Dodavanje certifikata na web-aplikaciju


**Dobivanje ili stvaranje certifikata** Naš svrhe dajemo će testnog certifikata. Evo nekoliko naredbi koje možete koristiti u programiranje naredbeni redak Stvaranje certifikata. Promjena direktorija na mjesto gdje želite da se datoteka certifikata će biti stvoren.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2015 -e 07/31/2016 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Zabilježite završnog datuma i lozinke za na .pfx (u ovom primjeru: 07/31/2016 i test123). Morate ih ispod.

Dodatne informacije o stvaranju testnog certifikata potražite u članku [Kako: Stvaranje vaše vlastite testiranje certifikata](https://msdn.microsoft.com/library/ff699202.aspx)


**Pridruživanje certifikata pomoću nekog od programa Azure AD** Sad kad ste certifikat, morate pridružiti Azure AD aplikacije. No Portal za upravljanje Azure ne podržava to odmah. Već imate pomoću komponente Powershell. Ovo su naredbe koje su vam potrebne za izvođenje:

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2

    PS C:\> $x509.Import("C:\data\KVWebApp.cer")

    PS C:\> $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    PS C:\> $now = [System.DateTime]::Now

    # this is where the end date from the cert above is used
    PS C:\> $yearfromnow = [System.DateTime]::Parse("2016-07-31")

    PS C:\> $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -KeyValue $credValue -KeyType "AsymmetricX509Cert" -KeyUsage "Verify" -StartDate $now -EndDate $yearfromnow

    PS C:\> $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    PS C:\>$x509.Thumbprint

Nakon pokretanja te naredbe, vidjet ćete aplikaciju Azure AD. Ako ne vidite aplikacije na prvi, potražite "Aplikacija je vlasnik Moja tvrtka" umjesto "Aplikacije Moja tvrtka koristi".

Da biste saznali više o Azure AD aplikacije i ServicePrincipal objekata, potražite u članku [aplikacija i servisa glavnicu objekata](../active-directory/active-directory-application-objects.md)



**Dodavanje koda na web-aplikaciju za korištenje potvrde** Sada dodat ćemo koda na web-aplikaciju da biste pristupili na certifikata i koristiti za provjeru autentičnosti.

Najprije postoji kod da biste pristupili na certifikata.

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


Imajte na umu da se StoreLocation CurrentUser umjesto LocalMachine. A da ne možemo su unošenju podataka "false" korakom traži zato što smo koriste certifikata za testiranje.


Sljedeći je kod koji se koristi u CertificateHelper i stvara ClientAssertionCertificate koji je potreban za provjeru autentičnosti.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Evo novi kod da biste nabavili token za pristup. Zamjenjuje gore navedeni GetToken način. Koje ste dodijelili ga neki drugi naziv praktičnost.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

Koje su sve kod staviti u predmete uslužni Moje web-aplikacije project radi jednostavnog korištenja.

Zadnja promjena kod nalazi se u način Application_Start. Najprije ćemo morati nazvati GetCert() način da biste učitali u ClientAssertionCertificate. A zatim ćemo promijenite način povratnog koji ne možemo dati prilikom stvaranja novog KeyVaultClient. Imajte na umu koji zamjenjuje kod koji smo imali iznad.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Dodavanje certifikata na web-aplikaciju putem portala za Azure** Dodavanje certifikata na web-aplikaciju je jednostavno dva koraka. Najprije otvorite Azure Portal i idite na web-aplikaciju. Na plohu postavki za web-aplikaciju, kliknite stavku "prilagođenih domena i SSL". Na plohu koji se otvori će moći prenijeti certifikat koji ste stvorili iznad KVWebApp.pfx, provjerite je li da morate zapamtiti lozinku na pfx.

![Dodavanjem certifikat web-aplikaciju na portalu za Azure][2]


Preostaje koje biste trebali učiniti tako da dodate na postavku aplikacije na web-aplikaciju koja ima naziv web-mjesta\_OPTEREĆENJA\_POTVRDE i vrijednost *. To će osigurati Svi certifikati učitavaju. Ako želite učitati certifikata koji ste prenijeli možete unijeti popis njihove thumbprints odvojenih zarezom.

Da biste saznali više o dodavanju certifikata na web-aplikacijama, potražite u članku [Korištenje potvrde u aplikacijama za Azure web-mjesta](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)


**Dodavanje certifikata radi sigurnog ključ kao na tajna** Umjesto da prenesete certifikata na servis za web-aplikacije izravno, možete spremiti u ključ sigurnog kao na tajna i implementacija iz nje. Ovo je postupak u dva koraka koji je naveden u sljedeće bloga [Implementacija Azure Web App certifikat putem sigurnog ključ](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)



## <a id="next"></a>Daljnji koraci ##


Programiranje reference, potražite u članku [Azure ključ sigurnog C# klijent API Reference](https://msdn.microsoft.com/library/azure/dn903628.aspx).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
