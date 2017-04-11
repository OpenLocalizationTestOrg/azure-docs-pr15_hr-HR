<properties
    pageTitle="Koristite SCIM da biste omogućili automatsko dodjeljivanje korisnika i grupa iz servisa Azure Active Directory aplikacijama | Microsoft Azure"
    description="Azure Active Directory možete automatski dodjelu resursa korisnicima i grupama aplikacija ili identiteta spremište koje je web-servisa fronted sa sučeljem definirano u specifikacija protokola SCIM"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="using-scim-to-enable-automatic-provisioning-of-users-and-groups-from-azure-active-directory-to-applications"></a>Koristite SCIM da biste omogućili automatsko dodjeljivanje korisnika i grupa iz servisa Azure Active Directory aplikacijama

##<a name="overview"></a>Pregled

Azure Active Directory možete automatski Dodjela resursa za korisnike i grupe aplikacija ili identiteta spremište koje je web-servisa fronted sa sučeljem definirano u [Specifikacija protokola SCIM 2.0](https://tools.ietf.org/html/draft-ietf-scim-api-19). Azure Active Directory možete poslati zahtjevi za stvaranje, izmjena i brisanje dodijeljenih korisnicima i grupama za ovo web-servisa koji možete zatim Prevedite zahtjeve operacije nakon spremišta identiteta ciljne. 

![][1]
*Slika: Dodjele resursa Azure Active Directory za identitet iz trgovine putem web-servisa*

Tu mogućnost mogu se koristiti u kombinaciji s mogućnošću "[Premjesti vlastite aplikacije](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)" u Azure AD da biste omogućili jedinstvenu prijavu i automatsko korisnika dodjele resursa za aplikacije koje omogućuju ili su fronted po SCIM web-servisa.

Postoje dva slučaja koristi za SCIM servisa Azure Active Directory:

* **Dodjeljivanje korisnicima i grupama u programima koji podržavaju SCIM** - aplikacije koje podržavaju SCIM 2.0 i koristiti OAuth nošenja tokeni za provjeru autentičnosti funkcioniraju s Azure AD okvira.

* **Izrada vlastite dodjele resursa rješenja za aplikacije koje podržavaju druge koji se temelji na API dodjeljivanje** - za aplikacije koje nisu SCIM, možete stvoriti SCIM krajnje da biste preveli između Azure AD SCIM krajnjoj točki i ono podržava API aplikacija za dodjelu resursa korisnika.  Da biste olakšali razvoja krajnjoj točki SCIM, dajemo EŽA biblioteke uz primjere koda koji pokazuju pružaju krajnjoj točki SCIM i prevođenje poruke SCIM.  

##<a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Dodjeljivanje korisnicima i grupama za aplikacije koje podržavaju SCIM

Azure Active Directory moguće je konfigurirati automatski dodjele dodijeliti korisnicima i grupama aplikacija koje web-mjesto [sustava za upravljanje domenama identitetom 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) servisa i prihvatiti OAuth nošenja tokeni za provjeru autentičnosti. Unutar specifikacija SCIM 2.0 aplikacija mora zadovoljavati sljedeće preduvjete:

* Stvaranje korisnika i/ili grupe po dio 3,3 protokol SCIM podržava.  

* Izmjena korisnika i/ili grupe s zakrpu zahtjevi po dio 3.5.2 protokol SCIM podržava.  

* Dohvaćanje poznati resursa po dio 3.4.1 protokol SCIM podržava.  

*  Slanje upita korisnika i/ili grupe po dio 3.4.2 protokol SCIM podržava.  Prema zadanim postavkama, korisnici su mu po externalId i grupe su mu tako riješiti problem.  

* Slanje upita korisnički ID i Upravitelj po dio 3.4.2 protokol SCIM podržava.  

* Slanje upita grupe po ID-a i člana po dio 3.4.2 protokol SCIM podržava.  

* Prihvaća tokena nošenja OAuth autorizacije po sekcije 2.1 SCIM protokol.

Trebali biste provjerite kod davatelja usluge aplikacije ili davatelja aplikacije dokumentaciji izjave kompatibilnosti s tim preduvjetima.
 
###<a name="getting-started"></a>Početak rada

Aplikacije koje podržavaju profila SCIM prethodno opisan moguće je povezati Azure Active Directory pomoću značajke "Prilagođeno" aplikacije u galeriji Azure AD aplikacije. Nakon uspostave Azure AD pokreće postupak sinkronizacije svakih 5 minuta upiti krajnjoj točki SCIM aplikacije za dodijeljenih korisnicima i grupama, i stvara ondje gdje ih mijenja prema dodjele detalje.

**Da biste se povezali aplikaciju koja podržava SCIM:**

1.  U web-preglednik, pokrenite portal Azure upravljanja na https://manage.windowsazure.com.
2.  Pronađite **servisa Active Directory > direktorija > [vaše direktorija] > aplikacije**, a zatim odaberite **Dodaj > Dodaj aplikaciju iz galerije**.
3.  Odaberite karticu **Prilagođeno** s lijeve strane, unesite naziv za svoju aplikaciju i kliknite ikonu kvačicu da biste stvorili objekt aplikacije.

![][2]

4.  Na zaslonu rezultata, odaberite gumb za drugi **Dodjeljivanje za konfiguriranje računa** .
5.  U polje **URL krajnja točka za dodjelu resursa** unesite URL krajnje točke SCIM aplikacije.
6.  Ako krajnju točku SCIM zahtijeva OAuth token nošenja od programa izdavača osim Azure AD, kopirajte potrebna OAuth token nošenja u polje za **Provjeru autentičnosti tokena (neobavezno)** . Je ovo polje ostavite prazno, a zatim Azure AD neće sadržavati stavku izdaje Azure AD sa zahtjevom za svaki OAuth token nošenja. Aplikacijama koje koristite Azure AD kao davateljem idenity možete provjeriti u ovom Azure AD-izdavanja token.
7.  Kliknite **Dalje**, a zatim kliknite gumb **Start Test** da bi se pokušavate povezati krajnju točku SCIM Azure Active Directory. Ako se prilikom pokušaja ne uspije, prikazat će se dijagnostičke informacije.  
8.  Ako se prilikom pokušaja povezati uspio aplikacije pa kliknite **Dalje** na preostalih zaslone i kliknite **dovrši** da biste zatvorili dijaloški okvir.
9.  Na zaslonu rezultata odaberite gumb treći **Dodjela računa** . U stvorenom odjeljku korisnici i grupe, dodijeliti korisnike ili grupe koju želite za dodjelu resursa za aplikaciju.
10. Kada korisnicima i grupama dodjeljuju se, kliknite karticu **Konfiguracija** pri vrhu zaslona.
11. U odjeljku **Dodjela resursa računa**, potvrdite da je Status postavljen na na. 
12. U odjeljku **Alati**kliknite **Ponovno dodjeljivanje računa** za kick-start postupka dodjele resursa.

Imajte na umu možda prođe 5 10 minuta prije postupka dodjele resursa će se započeti slanje zahtjeva za krajnju točku SCIM.  Sažetak pokušaja povezivanja navedeni su na kartica nadzorna ploča aplikacije, a izvješća dodjele resursa aktivnosti i sve pogreške za dodjelu resursa mogu se preuzeti sa kartica izvještaji na direktorija.

##<a name="building-your-own-provisioning-solution-for-any-application"></a>Stvaranje vlastite dodjeljivanje rješenja za bilo koju aplikaciju

Stvaranjem SCIM web-servisa koji sučelja s Azure Active Directory možete omogućiti jednom korisniku prijave i automatski dodjele resursa za gotovo bilo koju aplikaciju koja nudi OSTALE ili SOAP korisnik dodjeljivanje API-JA.

Evo kako to učiniti:

1.  Azure AD sadrži uobičajene biblioteci infrastrukture jezik pod nazivom [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Web-mjesto sustava integrators i u okvir za razvojne inženjere možete koristiti tu biblioteku za stvaranje i implementacija u web-mjesto utemeljeno na SCIM krajnja točka servisa Azure AD povezivanje spremište identiteta za bilo koju aplikaciju.
2.  Mapiranja primjenjuju se na web-servisa za mapiranje shemi standardizirani korisnika sheme korisnika i protokol potrebnih aplikacije.
3.  URL krajnje je registrirana u Azure AD kao dio prilagođenu aplikaciju u galeriji aplikacije.
4.  Korisnicima i grupama dodjeljuju se na ovu aplikaciju u Azure AD. Nakon dodjele, oni staviti u redu čekanja za sinkronizaciju da biste ciljnu aplikaciju. Sinkronizacija rukovanje reda čekanja pokreće svakih 5 minuta.

###<a name="code-samples"></a>Primjere koda

Da biste olakšali taj proces, skup [kod uzoraka](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) su stvorili krajnju točku SCIM web servisa i demonstrirati automatsko dodjele resursa. Je jedan uzorak davatelja koji se održava datoteke s recima koji predstavlja korisnicima i grupama vrijednosti odvojenih zarezom.  Drugi je davatelj usluga koje se primjenjuju na servis za upravljanje pristupom i Amazon web-servisi identitet.  

**Preduvjeti**

* Visual Studio 2013 ili noviji
* [Azure SDK za .NET](https://azure.microsoft.com/downloads/)
* Windows stroju koji podržava framework ASP.NET 4,5 će se koristiti kao krajnja točka SCIM. Ovo računalo mora biti dostupne iz oblaka
* [Azure pretplatu na probnu ili licenciranu verziju preglednika Azure AD Premium](https://azure.microsoft.com/services/active-directory/)
* Ogledna Amazon AWS potreban je biblioteka s [AWS komplet alata za Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html). Datoteke README uključene uzorka za dodatne pojedinosti potražite u članku

###<a name="getting-started"></a>Početak rada

Možete implementirati SCIM krajnju točku koja možete prihvatiti dodjelu resursa zahtjeve iz Azure AD najjednostavnije omogućuje stvaranje i implementaciju uzorak koda koji proizvodi Dodjela resursa korisnicima u datoteku vrijednosti odvojenih zarezom (CSV).

**Stvaranje krajnje točke SCIM uzorak:**

1.  Preuzmite paket uzorka kod pri [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2.  Raspakiraj paketa i njegovo stavljanje na računalu Windows na mjestu kao što je C:\AzureAD-BYOA-Provisioning-Samples\.
3.  U ovoj mapi pokrenite FileProvisioningAgent rješenja u Visual Studio.
4.  Odaberite **Alati > Upravitelj paketa biblioteka > Upravitelj paketa konzole**, i izvršavanje naredbe ispod za projekt FileProvisioningAgent da biste riješili reference rješenja:

    Instalacija paketa instalaciju paketa Microsoft.SystemForCrossDomainIdentityManagement instalaciju paketa Microsoft.IdentityModel.Clients.ActiveDirectory instalaciju paketa za Microsoft.Owin.Diagnostics Microsoft.Owin.Host.SystemWeb

5.  Sastavljanje FileProvisioningAgent projekta.
6.  Pokrenite aplikaciju naredbeni redak u sustavu Windows (kao Administrator), a pomoću naredbe **CD-a** da biste promijenili direktoriju u mapi **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** .
7.  Pokrenite naredbu, ispod zamjena < ip adresa > IP ili domena naziv računala za Windows.

    FileAgnt.exe http://<ip-address>:9000 TargetFile.csv

8.  U sustavu Windows u odjeljku **Postavke sustava Windows > mreža i Internet postavke**, odaberite na **Vatrozid za Windows > Napredne postavke**, i stvaranje **Ulazna pravila** koja omogućuje pristup ulazni priključak 9000.
9.  Ako na računalu Windows nalazi iza usmjerivač, usmjerivač morat ćete je konfigurirati za izvođenje prevođenja mrežnih Access između njegov priključak 9000 koji je izložen putem Interneta i priključak 9000 na računalu Windows. Ovo je obavezan za Azure AD da biste mogli pristupiti ovaj krajnje točke u oblaku.


**Da biste registrirali krajnje SCIM uzorka Azure AD:**

1.  U web-preglednik, pokrenite portal Azure upravljanja na https://manage.windowsazure.com.
2.  Pronađite **servisa Active Directory > direktorija > [vaše direktorija] > aplikacije**, a zatim odaberite **Dodaj > Dodaj aplikaciju iz galerije**.
3.  Odaberite karticu **Prilagođeno** s lijeve strane, unesite naziv kao što su "SCIM aplikacije za testiranje" i kliknite ikonu kvačicu da biste stvorili objekt aplikacije. Imajte na umu da se objekt aplikacija stvoriti namjeravate označavanje ciljne aplikacije koju želite dodjele resursa za pa implementacijom jedinstvene prijave za, a ne samo SCIM krajnju točku.

![][2]

4.  Na zaslonu rezultata, odaberite gumb za drugi **Dodjeljivanje za konfiguriranje računa** .
5.  U dijaloškom okviru unesite internet izložen URL-a i priključak za vaše SCIM krajnjoj točki. Na ovom bi nešto kao http://testmachine.contoso.com:9000 ili http://<ip-address>:9000/, pri čemu je < ip adresa > Interneta izložen IP adresa.  
6.  Kliknite **Dalje**, a zatim kliknite gumb **Start Test** da bi se pokušavate povezati krajnju točku SCIM Azure Active Directory. Ako se prilikom pokušaja ne uspije, prikazat će se dijagnostičke informacije.  
7.  Ako pokušaji povezivanja s web-servisa ne uspije, kliknite **Dalje** na preostale zaslonima i kliknite **dovrši** da biste zatvorili dijaloški okvir.
8.  Na zaslonu rezultata odaberite gumb treći **Dodjela računa** . U stvorenom odjeljku korisnici i grupe, dodijeliti korisnike ili grupe koju želite za dodjelu resursa za aplikaciju.
9.  Kada korisnicima i grupama dodjeljuju se, kliknite karticu **Konfiguracija** pri vrhu zaslona.
10. U odjeljku **Dodjela resursa računa**, potvrdite da je Status postavljen na na. 
11. U odjeljku **Alati**kliknite **Ponovno dodjeljivanje računa** za kick-start postupka dodjele resursa.

Imajte na umu možda prođe 5 10 minuta prije postupka dodjele resursa će se započeti slanje zahtjeva za krajnju točku SCIM.  Sažetak pokušaja povezivanja navedeni su na kartica nadzorna ploča aplikacije, a izvješća dodjele resursa aktivnosti i sve pogreške za dodjelu resursa mogu se preuzeti sa kartica izvještaji na direktorija.

U posljednjem koraku pri provjeri valjanosti uzorka je da biste otvorili datoteku TargetFile.csv u mapi \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug na računalu Windows. Nakon pokretanja postupka dodjele resursa datoteka prikazuje detalje o svim dodijeljeni i dodjeli korisnici i grupe.

###<a name="development-libraries"></a>Biblioteka za razvoj

Za razvoj vlastitu web-servisa koji je sukladan s specifikacija SCIM, najprije upoznati s sljedeće biblioteke pruža Microsoft radi accelerate postupka razvoja: 

**1:**  Uobičajeni jezik infrastrukture biblioteke se nude za korištenje s jezika koji se temelji na toj infrastrukture, kao što su C#.  Jedno od te biblioteke [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/)deklarira sučelja, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, prikazano na slici u nastavku.  Razvojni inženjer pomoću biblioteke želite implementirati koji se povezuju s predmete koje mogu pozivati, generically, kao davatelj.  Biblioteke omogućiti inženjerima omogućuje jednostavno implementacija web-servisa koji je sukladan specifikacija SCIM, bilo koju hostira unutar Internet Information Services ili sve izvršna skupa uobičajenih infrastrukture jezik.  Zahtjevi za web-usluzi će može prevesti u pozive metode vašeg davatelja usluge koje bi razvojni programirati da biste upravljali neke trgovine identiteta.    

![][3]

**2:** [Express usmjeravanje rukovatelja](http://expressjs.com/guide/routing.html) dostupni su za Raščlanjivanje node.js zahtjev objekata koji predstavlja poziva (kako je definirano specifikacija SCIM), pokušaj node.js web-servisa.     

###<a name="building-a-custom-scim-endpoint"></a>Stvaranje prilagođene SCIM krajnje

Korištenje biblioteka na prethodno opisan razvojnim inženjerima pomoću te biblioteke možete hostirati svoje usluge unutar sve izvršna skupa uobičajenih infrastrukture jezik ili unutar Internet Information Services.  Evo uzorak koda za hostiranje na unutar izvršna sastavljanje, na adresi http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  
    
    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Važno je da Imajte na umu da bi taj servis mora imati HTTP adresa i poslužitelj provjere autentičnosti potvrdu čiji izdavanje je nešto od sljedećeg: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Certifikat za provjeru autentičnosti poslužitelja može povezati s priključkom na glavnom računalu Windows pomoću ljuske uslužni mreže ovako: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  
 
Ovdje, vrijednost navedena za certhash argument je otisak prsta potvrde, dok je vrijednost argumenta ID programa proizvoljne globalno jedinstveni identifikator.  

Za hostiranje servisa unutar Internet Information Services razvojni inženjer bi sastavljanje biblioteke skupa kod za uobičajene infrastrukture jezika s klase pod nazivom pokretanja u polje naziva zadanog u sklopu.  Evo primjera takve predmete. 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

###<a name="handling-endpoint-authentication"></a>Provjera autentičnosti rukovanja krajnje točke

Zahtjeve iz servisa Azure Active Directory obuhvaćaju programa token nošenja OAuth 2.0.   Bilo koji servis za primanje zahtjeva treba provjeriti autentičnost izdavača kao Azure Active Directory ime očekivani Azure Active Directory klijent, pristup servisu Azure Active Directory na grafikonu Web.  U token, izdavač otkrije iss zahtjeva, primjerice "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  U ovom primjeru osnovne adrese vrijednost zahtjeva https://sts.windows.net, prepoznaje Azure Active Directory kao izdavač, dok je relativna adresa segmentu cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, Jedinstveni identifikator klijentu Azure Active Directory ime koje je izdao token.  Ako token izdavanja za pristupa servisu Azure Active Directory na grafikonu Web, identifikator tu uslugu 00000002-0000-0000-c000-000000000000, mora biti u vrijednosti u token aud zahtjeva.  

Razvojnim inženjerima pomoću biblioteka uobičajenih jezik infrastrukture pruža Microsoft za izgradnju SCIM servisa možete provjeriti autentičnost zahtjeve iz Azure Active Directory pomoću značajke pakiranja Microsoft.Owin.Security.ActiveDirectory slijedeći ove korake: 

**1:**  U davatelja, implementirati svojstvo Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior tako da je vratite metode pozivanje prilikom svakog pokretanja usluge: 

    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }

**2:**  Na taj način da bi bilo koji zahtjev na bilo koju od krajnje točke servisa provjereno kao ćete token izdala Azure Active Directory ime navedeni klijent za pristup servisu Azure Active Directory na grafikonu Web dodati sljedeći kod: 

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }

##<a name="user-and-group-schema"></a>Korisnika i grupa sheme

Azure Active Directory možete Dodjela dvije vrste resursa za SCIM web-servisa.  Te vrste resursa su korisnici i grupe.  

Resursi za korisnika prepoznaju se po identifikator sheme urn: ietf:params:scim:schemas:extension:enterprise:2.0:User koja se nalazi u ovom specifikacija protokola: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Zadano mapiranje atributa korisnicima servisa Azure Active Directory atribute urn: ietf:params:scim:schemas:extension:enterprise:2.0:User resursa navedeni su u tablici 1, ispod.  

Grupa resursa prepoznaju se po identifikator sheme http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tablica 2, niže prikazano je zadano mapiranje atributa grupa servisa Azure Active Directory u atribute http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group resursa.  

###<a name="table-1-default-user-attribute-mapping"></a>Tablica 1: Zadana mapiranje atributa korisnika

| Azure Active Directory korisnika | urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| ------------- | ------------- |
| IsSoftDeleted | aktivni |
| riješiti problem | riješiti problem |
| Facsimile TelephoneNumber | .value phoneNumbers [Vrsta eq "faks"] |
| givenName | name.givenName |
| jobTitle | Naslov |
| pošta | .value poruke e-pošte [Vrsta eq "rad"] |
| mailNickname | externalId |
| Upravitelj | Upravitelj |
| mobilni | .value phoneNumbers [Vrsta eq "mobile"] |
| ID objekta | ID-a |
| Poštanski broj | .postalCode adrese [Vrsta eq "rad"] |
| proxy adrese | e-pošte [upišite eq "ostalo"]. Vrijednost |
| fizičke isporuke OfficeName | [upišite eq "ostalo"] adrese. Oblikovani |
| streetAddress | .streetAddress adrese [Vrsta eq "rad"] |
| Prezime | name.familyName |
| Broj telefona | .value phoneNumbers [Vrsta eq "rad"] |
| korisnik PrincipalName | korisničko ime |


###<a name="table-2-default-group-attribute-mapping"></a>Tablica 2: Zadane grupe mapiranje atributa

| Grupa Azure Active Directory | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| ------------- | ------------- |
| riješiti problem | externalId |
| pošta | .value poruke e-pošte [Vrsta eq "rad"] |
| mailNickname | riješiti problem |
| Članovi | Članovi |
| ID objekta | ID-a |
| proxyAddresses | e-pošte [upišite eq "ostalo"]. Vrijednost |


##<a name="user-provisioning-and-de-provisioning"></a>Dodjeljivanje korisnika i Poništi dodjelu resursa

Na slici u nastavku prikazuje pohranite poruka da Azure Active Directory šaljete SCIM servisa za upravljanje životnim ciklusom korisnika u drugi identitet.  Dijagram prikazuje i kako SCIM servisa implementirati pomoću biblioteke uobičajenih jezik infrastrukture pruža Microsoft za izgradnju takve usluge će Prevedite zahtjeve pozive načinima davatelja.  

![][4]
*Slika: Dodjeljivanje korisnika i Poništi dodjelu resursa niza*

**1:**  Azure Active Directory će upit uslugu za korisnika s vrijednošću externalId atributa koji se podudaraju vrijednost atributa mailNickname korisnika u servisu Azure Active Directory.  Upit će se izraziti kao zahtjev Hypertext Transfer Protocol poput ovoga, kojemu je jyoung uzorak mailNickname korisnika u servisu Azure Active Directory: 

    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...

Ako servis je ugrađena pomoću biblioteka uobičajenih jezik infrastrukture pruža Microsoft za implementaciju SCIM services, zahtjev će prevesti u pozivu način upita davatelja usluge.  Evo potpis taj način: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);

Evo definiciju Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters sučelja: 

    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }
    
    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }

U slučaju foregoing uzorak upit za korisnika s danom vrijednošću atributa externalId argumenata proslijeđena metodi upit će se vrijednosti na sljedeći način: 

* Parametri. AlternateFilters.Count: 1
* Parametri. AlternateFilters.ElementAt(0). AttributePath: "externalId"
* Parametri. AlternateFilters.ElementAt(0). ComparisonOperator: ComparisonOperator.Equals
* Parametri. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
* correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. RequestId"] 

**2:**  Ako odgovor na upit za uslugu za korisnika s vrijednošću externalId atributa koji se podudaraju vrijednost atributa mailNickname korisnika u servisu Azure Active Directory vratili sve korisnike, Azure Active Directory će zatražiti da servis Dodjela korisnika koji odgovara onome u Azure Active Directory.  Evo primjera takve zahtjev: 

    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}

Biblioteka uobičajenih jezik infrastrukture pruža Microsoft za implementaciju SCIM servisa bi Prevedite poziva na način stvaranja davatelja usluge taj zahtjev.  Način za stvaranje sadrži taj potpis: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);

Ako zahtjev za dodjeljivanje korisnika, vrijednost argumenta resursa bit će instance komponente na Microsoft.SystemForCrossDomainIdentityManagement. Klase Core2EnterpriseUser definirano u biblioteci Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  Ako uspješnog zahtjev Dodjela korisnika, a zatim implementacija metodu se očekuje da biste se vratili instance komponente sustava na Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser klase vrijednošću svojstvo identifikatora postavljena na Jedinstveni identifikator upravo dodjeli korisniku.  

**3:**  Da biste ažurirali korisnika zna da postoje u spremištu identiteta fronted tako da se SCIM, Azure Active Directory će nastaviti tako da zahtijevate trenutno stanje tog korisnika iz servisa sa zahtjevom za poput ove: 

    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...

U servisu ugrađeni pomoću biblioteka uobičajenih jezik infrastrukture pruža Microsoft za implementaciju SCIM services, zahtjev će prevesti u pozivu metodom Dohvati davatelja usluge.  Evo potpis dohvaćanje načina: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);
    
    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }

U slučaju foregoing primjer zahtjeva za dohvaćanje trenutnog stanja korisnika vrijednosti svojstva objekta koji je naveden kao vrijednost argumenta parametara bit će na sljedeći način: 

* Identifikator: "54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

**4:**  Ako je referenca atribut je ažurirati, a zatim Azure Active Directory će upit servisa da biste odredili hoće li trenutnu vrijednost atributa referencu u spremištu identiteta fronted servis već usklađuje vrijednost atribut servisa Azure Active Directory.  U slučaju korisnika, samo atributa koji će biti mu trenutnu vrijednost na taj način je atribut manager.  Evo primjera zahtjev da biste odredili ima li atribut Upravitelj objekta određeni korisnik trenutno određene vrijednosti: 

    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...

Vrijednost u parametra upita atributa ID-a, označava koji ako postoji u korisničkom objektu koji zadovoljava izraz naveden kao vrijednost parametra upita filtar, a zatim servis se očekuje da odgovor na poruke uz urn: ietf:params:scim:schemas:core:2.0:User ili urn: ietf:params:scim:schemas:extension:enterprise:2.0:User resursa, uključujući vrijednost atributa ID-a koji resurs.  Naravno, je vrijednost atributa id poznate podnositelja zahtjeva – to je sve obuhvaćeno vrijednost parametra upita filtar; Svrha je traži zapravo je da bi se zatražila minimalnog prikaz resursa koji koje zadovoljavaju izraz filtra kao upućuju li ili ne postoji takvo objekt.   

Ako servis je ugrađena pomoću biblioteka uobičajenih jezik infrastrukture pruža Microsoft za implementaciju SCIM services, zahtjev će prevesti u pozivu način upita davatelja usluge.  Vrijednost svojstva objekta koji je naveden kao vrijednost argumenta parametara bit će na sljedeći način: 

* Parametri. AlternateFilters.Count: 2
* Parametri. AlternateFilters.ElementAt(x). AttributePath: "id"
* Parametri. AlternateFilters.ElementAt(x). ComparisonOperator: ComparisonOperator.Equals
* Parametri. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
* Parametri. AlternateFilters.ElementAt(y). AttributePath: "upravitelja"
* Parametri. AlternateFilters.ElementAt(y). ComparisonOperator: ComparisonOperator.Equals
* Parametri. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
* Parametri. RequestedAttributePaths.ElementAt(0): "id"
* Parametri. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

Ovdje, vrijednost funkcije index x može biti 0 i vrijednost y indeksa može biti 1, ili vrijednost od x može biti 1 i vrijednost y može biti 0, ovisno o redoslijeda izraza parametra upita filtar.   

**5:**  Evo primjera zahtjeva iz servisa Azure Active Directory sa servisom SCIM da biste ažurirali korisnika: 

    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}

Biblioteka Microsoft uobičajenih infrastruktura za jezik za implementaciju SCIM servisa želite prevesti zahtjev u poziv metode Update davatelja usluge.  Evo potpis taj način: 

    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }
    
    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }
      
      public string Value
      { get; set; }
    }



U slučaju foregoing primjer zahtjeva za ažuriranje korisnika objekt koji je naveden kao vrijednost argumenta zakrpa će imati te vrijednosti svojstava: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (PatchRequest kao PatchRequest2). Operations.Count: 1
* (PatchRequest kao PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add
* (PatchRequest kao PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "upravitelja"
* (PatchRequest kao PatchRequest2). Operations.ElementAt(0). Value.Count: 1
* (PatchRequest kao PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Referenca: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (PatchRequest kao PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Vrijednost: 2819c223-7f76-453a-919d-413861904646

**6:**  Poništi Dodjela korisnika iz trgovine identiteta fronted tako da na servis SCIM, Azure Active Directory će poslati zahtjev poput ove: 

    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    
Ako servis je ugrađena pomoću biblioteka uobičajenih jezik infrastrukture pruža Microsoft za implementaciju SCIM services, zahtjev će prevesti u poziv metode brisanja davatelja usluge.   Taj način sadrži taj potpis: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
 
Objekt koji je naveden kao vrijednost argumenta resourceIdentifier će imati te vrijednosti svojstava slučaju foregoing primjer zahtjeva deaktivirali Dodjela korisnika: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

##<a name="group-provisioning-and-de-provisioning"></a>Dodjela resursa za skupinu i Poništi dodjelu resursa

Na slici u nastavku prikazuje pohranite poruka da Azure Active Directory šaljete SCIM servisa za upravljanje životnim ciklusom grupe u drugi identitet.  Te poruke razlikovati od poruka koji se odnose na korisnike na tri načina: 

* Shema resursa grupe će smatraju http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  
* Zahtjevi za dohvaćanje grupe će stipulate da je atribut članove koji treba izuzeti iz bilo kojeg resursa u odgovor na zahtjev.  
* Zahtjevi za određivanje ima li referenca atribut određene vrijednosti bit će zahtjeve o atribut članova.  

![][5]
*Slika: Grupiranje dodjele resursa i poništite dodjeljivanje niza*

##<a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Automatiziranje korisničkog dodjele resursa/Deprovisioning SaaS aplikacije](active-directory-saas-app-provisioning.md)
- [Prilagodba mapiranja atributa za dodjelu resursa korisnika](active-directory-saas-customizing-attribute-mappings.md)
- [Pisanje izraza za mapiranja atributa](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Određivanje opsega filtre za dodjelu resursa korisnika](active-directory-saas-scoping-filters.md)
- [Račun dodjeljivanje obavijesti](active-directory-saas-account-provisioning-notifications.md)
- [Popis vodiče za integraciju SaaS aplikacije](active-directory-saas-tutorial-list.md)


    
<!--Image references-->
[1]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
