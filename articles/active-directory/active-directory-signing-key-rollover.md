<properties
    pageTitle="Prijava ključa prilikom prelaska u Azure AD | Microsoft Azure"
    description="U ovom se članku opisuje potpisivanja ključa prilikom prelaska najbolje prakse za Azure Active Directory"
    services="active-directory"
    documentationCenter=".net"
    authors="gsacavdm"
    manager="krassk"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="gsacavdm"/>

# <a name="signing-key-rollover-in-azure-active-directory"></a>Potpisivanje ključa prilikom prelaska u servisu Azure Active Directory

U ovoj se temi opisuje što trebate znati o javni ključ koji se koriste u Azure Active Directory (Azure AD) da biste se prijavili sigurnosnih tokena. Nije važno Imajte na umu da ove tipke prilikom prelaska na periodičnim i u slučaju nužde može biti vraćeni odmah. Sve aplikacije koje koriste Azure AD trebali biste moći programski rukovati postupka ključa prilikom prelaska ili uspostaviti periodičku ručno dinamične procesa. Nastavite čitati da biste razumjeli kako funkcioniraju tipki, kako procijenite utjecaj prelaska u aplikaciji i kako ažurirati aplikaciju ili uspostaviti procesa periodičku ručno dinamične učiniti ključa prilikom prelaska prema potrebi.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Pregled tipke prijavi Azure AD

Azure AD koristi javni ključ šifriranja utemeljena na standardima uspostaviti pouzdanosti između sa samim sobom i aplikacije koje koriste ga. Praktično rječnikom, to funkcionira na sljedeći način: Azure AD koristi potpisivanja ključ koji se sastoji od javnim i privatnim ključ par. Prilikom prijave za aplikaciju koja koristi Azure AD za provjeru autentičnosti, Azure AD stvara sigurnosni token koji sadrži informacije o korisniku. Ovaj token je potpisao Azure AD pomoću privatni ključ prije slanja natrag u aplikaciji. Token provjerite je li valjan i zapravo potječe od Azure AD, aplikacija morate provjeriti potpis na token pomoću javni ključ koji prikazuje Azure AD koji je sadržan u [OpenID povezivanje dokumenta za otkrivanje](http://openid.net/specs/openid-connect-discovery-1_0.html) ili WS/SAML-Fed [vanjski pristup metapodataka dokumenta](active-directory-federation-metadata.md)na klijentu.

Iz sigurnosnih razloga Azure AD potpisivanja ključa kumulira na periodičnim i, ako u slučaju nužde može biti vraćeni odmah. Bilo koju aplikaciju koja integrira Azure AD moraju se pripremite za rukovanje događajem ključa prilikom prelaska bez obzira na to koliko se često može doći. Ako ga ne, a aplikacija pokušava koristiti istekle ključ za provjeru potpisa token, zahtjev za prijavu neće uspjeti.

Uvijek više valjani ključ dostupna je u dokument za povezivanje OpenID otkrivanja i dokument metapodataka za vanjski pristup. Aplikacija treba očekivati da biste koristili neki od tipki navedene u dokumentu, budući da jedan ključ može biti uskoro vraćeni, drugi mogu biti njegov zamjenski itd.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Kako procijenite ako utjecat će vaše aplikacije i što učiniti

Način rukovanja aplikacija ključa prilikom prelaska ovisi o varijable kao što su vrste aplikacije ili koristila koje protokol identiteta i biblioteke. U odjeljcima procijenite li najčešće aplikacija su utječe ključa dinamične i navedene su upute o tome kako ažurirati aplikaciju podržava automatsko dinamične ili ručno ažurirati tipku.

* [Aplikacija za nativni klijent pristupa resursima](#nativeclient)
* [Web-aplikacije / API-ji pristupa resursima](#webclient)
* [Web-aplikacije / API-ji zaštita resurse i izgrađene pomoću servisa Azure aplikacije](#appservices)
* [Web-aplikacije / API-ji zaštita resurse pomoću .NET OWIN OpenID povezivanje, WS Fed ili WindowsAzureActiveDirectoryBearerAuthentication proizvod](#owin)
* [Web-aplikacije / API-ji zaštita resurse pomoću .NET Core OpenID povezati ili JwtBearerAuthentication proizvod](#owincore)
* [Web-aplikacije / API-ji zaštita resurse pomoću modula Node.js passport azure ad](#passport)
* [Web-aplikacije / API-ji zaštita resurse i stvorene pomoću Visual Studio 2015.](#vs2015)
* [Web-aplikacije zaštita resurse i stvorene pomoću Visual Studio 2013](#vs2013)
* [API-ji web zaštita resurse i stvorene pomoću Visual Studio 2013](#vs2013_webapi)
* [Web-aplikacije zaštita resurse i stvorene pomoću Visual Studio 2012](#vs2012)
* [Web-aplikacije zaštita resurse i stvorene pomoću Visual Studio 2010, 2008 o korištenju Windows Foundation identiteta](#vs2010)
* [Web-aplikacije / API-ji zaštita resursa pomoću bilo koje druge biblioteke ili ručno implementacijom svih podržanih protokola](#other)

U ovom smjernice **nije** primjenjivo za:

* Aplikacija dodali Azure AD aplikacija galeriji (uključujući prilagođena) imati zasebne smjernice koja se odnosi na potpisivanje tipke. [Dodatne informacije.](active-directory-sso-certs.md)
* Lokalni aplikacije koje su objavljene putem proxy aplikacije ne morate brinuti o prijavljivanju tipke.

### <a name="nativeclient"></a>Aplikacija za nativni klijent pristupa resursima

Aplikacijama koje su samo pristupa resursima (i.e Microsoft Graph, KeyVault, Outlook API i druge Microsoftove APIs) obično samo nabavite token i prenesite ga uzduž vlasnik resursa. Given oni su zaštite nikakve resurse, oni Provjera token te stoga nije potrebno da biste bili sigurni da je ispravno potpisan.

Aplikacije za nativni klijent li stolna računala ili mobilnog, ulaze u kategoriju i su stoga ne utječe na dinamične.

### <a name="webclient"></a>Web-aplikacije / API-ji pristupa resursima

Aplikacijama koje su samo pristupa resursima (i.e Microsoft Graph, KeyVault, Outlook API i druge Microsoftove APIs) obično samo nabavite token i prenesite ga uzduž vlasnik resursa. Given oni su zaštite nikakve resurse, oni Provjera token te stoga nije potrebno da biste bili sigurni da je ispravno potpisan.

Web-aplikacije i web-API-ji koji koriste tijek samo za aplikacije (klijent vjerodajnice / klijentski certifikat) ulaze u kategoriju te se stoga ne utječe na prilikom prelaska.

### <a name="appservices"></a>Web-aplikacije / API-ji zaštita resurse i izgrađene pomoću servisa Azure aplikacije

Provjera autentičnosti Azure aplikacije servisa / funkcionalnost autorizacije (EasyAuth) već ima potrebne logike za rukovanje ključa prilikom prelaska automatski.

### <a name="owin"></a>Web-aplikacije / API-ji zaštita resurse pomoću .NET OWIN OpenID povezivanje, WS Fed ili WindowsAzureActiveDirectoryBearerAuthentication proizvod

Ako koristite aplikacije za .NET OWIN OpenID povezivanje, WS Fed ili WindowsAzureActiveDirectoryBearerAuthentication proizvod, već ima potrebne logike za rukovanje ključa prilikom prelaska automatski.

Možete potvrditi da vaša aplikacija koristi nešto od sljedećeg traženjem bilo koji od sljedećih isječci u Startup.cs ili Startup.Auth.cs vaše aplikacije

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
    });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Web-aplikacije / API-ji zaštita resurse pomoću .NET Core OpenID povezati ili JwtBearerAuthentication proizvod

Ako aplikacija koristi .NET Core OWIN OpenID povezati ili JwtBearerAuthentication proizvod, već ima potrebne logike za rukovanje ključa prilikom prelaska automatski.

Možete potvrditi da aplikacija koristi nešto od sljedećeg traženjem bilo koji od sljedećih isječci u Startup.cs ili Startup.Auth.cs vaše aplikacije

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
    });
```

### <a name="passport"></a>Web-aplikacije / API-ji zaštita resurse pomoću modula Node.js passport azure ad

Ako aplikacija koristi passport ad modul Node.js, već ima potrebne logike za rukovanje ključa prilikom prelaska automatski.

Možete potvrditi koje vaše aplikacije passport ad traženjem sljedeće isječak u app.js vaše aplikacije

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Web-aplikacije / API-ji zaštita resurse i stvorene pomoću Visual Studio 2015.

Ako aplikacija gradnje pomoću web-aplikacije predložak u Visual Studio 2015, a odabrali ste **Rad školske račune i** na izborniku **Promijeni provjeru autentičnosti** , već ima potrebne logike za rukovanje ključa prilikom prelaska automatski. Logika, ugrađen u proizvod OWIN OpenID povezivanje dohvaća i predmemorira tipki iz dokumenta za povezivanje OpenID otkrivanje i povremeno osvježava.

Ako ste ručno dodali provjere autentičnosti rješenje, aplikacije možda neće imati logike potrebne dinamične ključa. Morat ćete pisati sami ili slijedite korake u [web-aplikacije / API-ji pomoću bilo koje druge biblioteke ili ručno implementacijom svih podržanih protokola.](#other).

### <a name="vs2013"></a>Web-aplikacije zaštita resurse i stvorene pomoću Visual Studio 2013

Ako aplikacija gradnje pomoću web-aplikacije predložak u Visual Studio 2013, a odabrali ste **Račun tvrtke ili ustanove** na izborniku **Promijeni provjeru autentičnosti** , već ima potrebne logike za rukovanje ključa prilikom prelaska automatski. U ovom logike pohranjuje Jedinstveni identifikator vaše tvrtke ili ustanove i potpisivanja podaci o ključu u dvjema tablicama baze podataka koji su povezane s projekta. Niz za povezivanje za bazu podataka možete pronaći u datoteci Web.config projekta.

Ako ste ručno dodali provjere autentičnosti rješenje, aplikacije možda neće imati logike potrebne dinamične ključa. Morat ćete pisati sami ili slijedite korake u [web-aplikacije / API-ji pomoću bilo koje druge biblioteke ili ručno implementacijom svih podržanih protokola.](#other).

Sljedeći koraci pomoći će vam provjerite logičku ispravno funkcionira u aplikaciji.

1. U Visual Studio 2013 otvorite rješenje, a zatim na kartici **Poslužitelj Explorer** na desni prozor.
2. Proširite **podatkovne veze**, **DefaultConnection**, a zatim **tablica**. Pronađite **IssuingAuthorityKeys** tablicu, kliknite ga desnom tipkom miša, a zatim kliknite **Prikaz podatkovne tablice**.
3. U tablici **IssuingAuthorityKeys** neće imati barem jedan redak koji odgovara otisak prsta vrijednost ključa. Izbrišite sve retke u tablici.
4. Desnom tipkom miša kliknite tablicu **klijenata** , a zatim kliknite **Prikaz podatkovne tablice**.
5. U tablici **klijenti** neće imati barem jedan redak koji odgovara klijentu identifikatora jedinstveni direktorija. Izbrišite sve retke u tablici. Ako ne izbrišete retke u tablici **klijenti** i **IssuingAuthorityKeys** tablice, dobit ćete pogrešku prilikom izvođenja.
6. Stvaranje i pokretanje aplikacija. Nakon prijave vašem računu, možete isključiti aplikacije.
7. Vratite u **Programu Explorer poslužitelja** i pogledajte vrijednosti u tablici **IssuingAuthorityKeys** i **drugih korisnika** . Primijetit ćete da se ste je automatski ponovno popuniti s odgovarajuće podatke iz dokumenta metapodataka za vanjski pristup.

### <a name="vs2013"></a>API-ji web zaštita resurse i stvorene pomoću Visual Studio 2013

Ako stvorili web-aplikacije za API-JA u Visual Studio 2013 pomoću predloška za Web API-JA, a odabrani **Račun tvrtke ili ustanove** na izborniku **Promijeni provjeru autentičnosti** , već imate potrebne logike u aplikaciji.

Ako ste ručno konfigurirali provjeru autentičnosti, slijedite upute u nastavku da biste saznali kako konfigurirati Web API-JA da bi automatski ažurirao njegov ključne informacije.

Sljedeći isječak koda pokazuje kako se najnovije tipke iz dokumenta metapodataka za vanjski pristup i zatim koristiti [JWT tokena događajima](https://msdn.microsoft.com/library/dn205065.aspx) za provjeru valjanosti token. Isječak koda pretpostavlja da ćete koristiti vlastitu predmemoriranja mehanizam za persisting ključ za provjeru valjanosti buduće tokena iz Azure AD hoće li se u bazu podataka, konfiguracijska datoteka ili negdje drugdje.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Web-aplikacije zaštita resurse i stvorene pomoću Visual Studio 2012

Ako aplikacija nije ugrađena u Visual Studio 2012, vjerojatno ste koristili identiteta i alat za pristup da biste konfigurirali aplikaciju. Također je vjerojatno koristite [Provjera valjanosti registra preglednika izdavač naziv (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). U VINR nije odgovoran za održavanje informacije o davatelj pouzdanog identiteta (Azure AD) i ključevi koji se koriste za provjeru valjanosti tokena koje izdaje ih. Na VINR pojednostavljuje da bi automatski ažurirao ključne podatke pohranjene u datoteci Web.config tako da preuzmete najnovije vanjski pristup metapodataka dokument je pridružen direktorija, provjera zastarjeli s najnovijim dokumentom konfiguraciju i ažuriranje aplikacije za korištenje novog ključa prema potrebi.

Ako ste stvorili aplikacija na bilo koji od primjere koda ili vodič dokumentaciji Microsoft, logika ključa prilikom prelaska već uvrštava u projektu. Primijetit ćete da se ispod kod već postoji u projektu. Ako aplikacija nije već logiku, slijedite korake u nastavku da biste ga dodali, a da biste provjerili funkcionira li ispravno.

1. U **Pregledniku rješenja**, dodajte referencu u sklopu **System.IdentityModel** odgovarajuće projekta.
2. Otvorite datoteku **Global.asax.cs** i dodajte sljedeće pomoću upute poslužitelja:
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. Dodajte na sljedeći način **Global.asax.cs** datoteku:
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. Pozivanje metoda **RefreshValidationSettings()** u način **Application_Start()** u **Global.asax.cs** kao što je prikazano:
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

Kada ste pratili korake, vaše aplikacije Web.config će se ažurirati najnovijim informacijama iz metapodataka dokumenta vanjski pristup, uključujući najnovije tipke. To ažuriranje će se svaki put kada se vaše aplikacija recycles u IIS; prema zadanim postavkama IIS postavljen je na koš za aplikacije svaka 29 sata.

Slijedite korake u nastavku da biste provjerili funkcionira li logike ključa prilikom prelaska.

1. Kada aplikacija je pomoću gore navedeni kod, otvaranje **Web.config** datoteke, a zatim je otvorite u **<issuerNameRegistry>** blok, posebice tražite sljedeće nekoliko redaka:
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. U na **<add thumbprint=””>** postavka, promijenite vrijednost otisak prsta jer zamjenjuje bilo koji znak na neku drugu. Spremite datoteku **Web.config** .

3. Sastavljanje aplikacije, a zatim ga pokrenuti. Ako dovršite postupak prijave aplikacije uspješno ažurira tipku tako da preuzmete potrebne podatke iz direktorija sustava vanjski pristup metapodataka dokumenta. Ako imate poteškoća s prijavom u, provjerite ispravni promjene u aplikaciji potražite u temi [Dodavanje prijave za korištenje aplikacije Web Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) ili preuzimanjem i pregled sljedećim primjerom koda: [Više klijentske aplikacije oblaka za Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).


### <a name="vs2010"></a>Web-aplikacije zaštita resurse i stvorene pomoću Visual Studio 2008 ili 2010 i v1.0 Windows identiteta Foundation (WIF) za .NET 3.5

Ugrađena u aplikaciju na WIF v1.0, postoji li bez navedeni mehanizam automatsko osvježavanje konfiguracija vaše aplikacije za korištenje novog ključa.

- *Najjednostavniji način* Koristite tooling FedUtil obuhvaćeno SDK WIF koje je moguće dohvatiti najnovija metapodataka dokumenta i ažurirati konfiguraciju.
- Ažuriranje aplikacije za .NET 4,5 koji sadrži najnovija verzija sustava WIF koja se nalazi u sustavu naziva. [Provjera valjanosti registra preglednika izdavač naziv (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) možete koristiti da biste izvršili Automatska ažuriranja konfiguracija aplikacije.
- Izvođenje ručno dinamične po upute na kraju ovog dokumenta upute.

Upute za korištenje u FedUtil da biste ažurirali vašoj konfiguraciji:

1. Provjerite je li v1.0 WIF SDK instaliran na vašem računalu razvoj za Visual Studio 2008 ili 2010. Možete [preuzeti ovdje](https://www.microsoft.com/en-us/download/details.aspx?id=4451) ako vam još niste instalirali ga.
2. U Visual Studio, otvorite rješenje, i desnom tipkom miša kliknite odgovarajuće projekta i odaberite **Ažuriranje metapodataka za vanjski pristup**. Ako ta mogućnost nije dostupna, FedUtil i/ili WIF v1.0 SDK nije instaliran.
3. Zatraži, odaberite **Ažuriranje** da biste započeli ažuriranje metapodatka vanjski pristup. Ako imate pristup poslužitelju okruženje gdje se nalazi aplikacija, po želji možete koristiti FedUtil na [Automatsko metapodataka ažuriranje raspored](https://msdn.microsoft.com/library/ee517272.aspx).
4. Kliknite **Završi** da biste dovršili postupak ažuriranja.

### <a name="other"></a>Web-aplikacije / API-ji zaštita resursa pomoću bilo koje druge biblioteke ili ručno implementacijom svih podržanih protokola

Ako koristite neke druge biblioteke ili ručno implementirana svih podržanih protokola, morat ćete pregled web-mjesto u biblioteku ili u okvir za implementaciju da biste bili sigurni da je dohvaćeni tipku s dokument za povezivanje OpenID otkrivanja ili vanjski pristup dokumentu metapodataka. Da biste provjerili za to je pretraživanje u kodu ili u biblioteku kod za sve pozive izvan dokument otkrivanja OpenID ili vanjski pristup dokumentu metapodataka.

Ako su ključa koji se pohranjuju negdje ili koji nisu u aplikaciji, možete ručno dohvatiti ključ i ažuriranje je sukladno tome tako da izvođenje ručno dinamične po upute na kraju ovog dokumenta upute. Strukturiranje **se preporučuje komentarima koje poboljšati aplikacija za podršku automatski prilikom prelaska** na bilo koji od pristupa navedenih u ovom članku da biste izbjegli buduće to izbjeglo, a indirektni ako Azure AD povećava se dinamički cadence ili sadrži hitne dinamične za izlaz grupiranje.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Kako testirati aplikacije da biste utvrdili je utjecat će

Možete provjeriti odgovara li vaša aplikacija podržava automatsko ključa dinamične preuzimanjem skripte i slijedite upute u [Ovo spremište GitHub.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Kako izvesti ručno dinamične ako vam aplikacija ne podržava automatsko dinamične

Ako aplikacija **nije** podršku automatskog dinamične, morat ćete uspostaviti proces kojim se povremeno nadzire Azure AD potpisivanja tipke i ručno dinamične izvodi sukladno tome. [Spremište GitHub ove](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) sadrži skripte i upute o tome kako to učiniti.
