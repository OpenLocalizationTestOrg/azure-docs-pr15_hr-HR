<properties 
    pageTitle="Povezivanje s računa za servise medijskih sadržaja pomoću .NET" 
    description="U ovoj se temi objašnjava kako se povezati sa Media Services uisng .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Povezivanje s računa za servise medijskih sadržaja pomoću Media Services SDK za .NET

> [AZURE.SELECTOR]
- [OSTALE](media-services-rest-connect-programmatically.md)
- [.NET](media-services-dotnet-connect-programmatically.md)


U ovoj se temi objašnjava kako nabaviti programski veze sa servisa Microsoft Azure Media Services kada su programiranje Media Services SDK za .NET.


## <a name="connecting-to-media-services"></a>Povezivanje s Media Services

Programski povezivanje Media Services, koje morate imati prethodno postavite račun za Azure, konfiguriran Media Services na taj račun i postavite projekt Visual Studio za razvoj s Media Services SDK za .NET. Dodatne informacije potražite u članku postavljanje za razvoj s Media Services SDK za .NET.

Na kraju postupka za postavljanje računa Media Services dobivaju sljedeće vrijednosti potrebna veza. Pomoću ove veze programski Media Services.

- Naziv računa Media Services.

- Ključ za račun servisa Media Services.

Da biste pronašli te vrijednosti, idite na Portal za upravljanje Azure, odaberite svoj račun za servis Media i kliknite ikonu "**Upravljanje tipke**" na dnu prozora portala. Klikom na ikonu pokraj svake tekstni okvir kopira vrijednost u međuspremnik sustava.


## <a name="creating-a-cloudmediacontext-instance"></a>Stvaranje CloudMediaContext instanci

Za pokretanje programiranja protiv Media Services ćete morati stvoriti instancu **CloudMediaContext** koji predstavlja kontekst poslužitelja. **CloudMediaContext** sadrži reference na važne zbirki uključujući zadacima, resursi, datoteke, pravilnike za pristup i locators.

>[AZURE.NOTE] Klase **CloudMediaContext** nije niti sigurni. Stvorite novi CloudMediaContext po niti ili po skup operacija.


CloudMediaContext ima pet overloads Graditelj. Preporučuje se da biste koristili constructors koje koriste **MediaServicesCredentials** kao parametar. Dodatne informacije potražite u članku **Ponovnog korištenja servisa tokena za pristup kontrole** koja slijedi. 

U sljedećem primjeru pomoću javnog Graditelj CloudMediaContext(MediaServicesCredentials credentials):

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Ponovno korištenje pristupna kontrola servisa tokena

U ovom se odjeljku objašnjava da biste ponovno koristili servisa za kontrolu pristupa tokeni pomoću constructors CloudMediaContext koje koriste MediaServicesCredentials kao parametar.


[Kontrola pristupa Azure Active Directory](https://msdn.microsoft.com/library/hh147631.aspx) (poznat i kao servisa za kontrolu pristupa ili ACS) je servis u oblaku koji omogućuje jednostavan način provjere autentičnosti i dopuštanja korisnicima da biste pristupili svoje web-aplikacije. Microsoft Azure Media Services kroz kontrolira pristup uslugama OAuth protokol koji zahtijeva token za ACS. Media Services prima ACS tokena iz poslužitelj za autorizaciju.

Prilikom razvoja s Media Services SDK, možete odabrati ne baviti tokena jer SDK kod upravitelja ih umjesto vas. Međutim, da SDK potpuno upravljanje tokeni ACS potencijalne klijente nepotrebne tokena zahtjeve za. Traženje tokeni traje i troši postupak klijentske i poslužiteljske resurse. Osim toga, ACS server regulira zahtjeve ako je stopa prevelika. Ograničenje je 30 za u sekundi, dodatne informacije potražite u odjeljku [ACS servisna ograničenja](https://msdn.microsoft.com/library/gg185909.aspx) .

Počevši od Media Services SDK verziju 3.0.0.0, pa je možete koristiti tokeni ACS. Constructors **CloudMediaContext** koje koriste **MediaServicesCredentials** kao parametar omogućite zajedničko korištenje tokena ACS između više konteksta. Klase MediaServicesCredentials encapsulates vjerodajnice Media Services. Ako je ACS token nije dostupna, a njegovo vrijeme isteka se nazivaju, možete stvoriti novu instancu MediaServicesCredentials s token i prenesite Graditelj CloudMediaContext. Imajte na umu da Media Services SDK automatski osvježava tokeni kad god isteknu. Dva su načina da biste ponovno koristili tokeni ACS, kao što je prikazano u primjerima koji slijede.

- Možete predmemorije objekta **MediaServicesCredentials** u memoriji (na primjer, u varijablu statične klase). Nakon toga prenesite predmemorije objekta Graditelj CloudMediaContext. Objekt MediaServicesCredentials sadrži token za ACS koji se može ponovno koristiti ako je još uvijek valjan. Ako token nije valjan, će se osvježiti po Media Services SDK pomoću vjerodajnica danih Graditelj MediaServicesCredentials.

    Imajte na umu da objekt **MediaServicesCredentials** dobiva valjani token nakon naziva u RefreshToken. **CloudMediaContext** poziva metodu **RefreshToken** u na Graditelj. Ako planirate da biste spremili tokena vrijednosti vanjskih za pohranu, svakako provjerite je li vrijednost TokenExpiration valjani prije spremanja tokena podataka. Ako nije valjan, nazovite RefreshToken prije predmemoriranje.

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        
        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }
        
        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

- Možete predmemorije i tekstni niz AccessToken i TokenExpiration vrijednosti. Da biste stvorili novi objekt MediaServicesCredentials s predmemoriranim podacima tokena kasnije se koristiti vrijednosti.  To je posebno korisno za scenariji kojima token mogu sigurno zajednički koristiti više procesa ili računala.

    Sljedeće koda poziva SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage i UpdateTokenDataInExternalStorageIfNeeded načina na koji su definirani u ovom primjeru. Možete definirati ovih metoda za pohranu, dohvaćanje i ažuriranje tokena podataka u vanjskim prostora za pohranu. 

        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
        
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
        
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
        
    Da biste stvorili MediaServicesCredentials pomoću spremljenu tokena vrijednosti.


        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;
        
        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);
        
        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };
        
        CloudMediaContext context2 = new CloudMediaContext(credentials);

    Ažurirajte tokena Kopiraj u slučaju da token ažurirao je Media Services SDK. 
    
        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }
        

- Ako imate više računa Media Services (na primjer, za zajedničko korištenje ili namjene zemlj raspodjele) možete predmemoriju objekata MediaServicesCredentials pomoću zbirke System.Collections.Concurrent.ConcurrentDictionary (zbirke ConcurrentDictionary predstavlja skup niti sigurnih parove ključa vrijednosti koje se mogu pristupati većem broju niti istovremeno). Zatim možete koristiti metodu GetOrAdd radi dobivanja predmemoriranih vjerodajnica. 

        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();
        

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;
        
            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));
        
            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);
        
            return cloudMediaContext;
        }
        
## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Veze s računom Media Services koja se nalazi u području Sjeverna Kina

Ako vaš račun nalazi se u Sjevernoj Kina regiji, koristite sljedeće Graditelj:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Ako, na primjer:


    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Spremanje veze vrijednosti u konfiguraciji

Je vrlo preporučljivo za pohranu veze vrijednosti, osobito osjetljive vrijednosti kao što su ime za račun i lozinku, u konfiguraciji. Osim toga, je preporučljivo da biste šifrirali konfiguracijske povjerljive podatke. Cijeli konfiguracijska datoteka Šifrirajte pomoću sustava Windows šifriranje datotečni sustav (EFS). Da biste omogućili EFS na datoteci, desnom tipkom miša kliknite datoteku, odaberite **Svojstva**i Omogući šifriranje na kartici **Dodatno** postavke. Ili možete stvoriti prilagođena rješenja za šifriranje odabranih dijelova konfiguracijska datoteka pomoću zaštićeni konfiguracije. U odjeljku [šifriranja konfiguracije informacije pomoću zaštićeni konfiguracije](https://msdn.microsoft.com/library/53tyfkaw.aspx).

Sljedeća App.config datoteka sadrži vrijednosti potrebna veza. Vrijednosti u <appSettings> element su potrebne vrijednosti koju ste dobili od postupka za postavljanje računa Media Services.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


Da biste dohvatili vrijednosti veze iz konfiguracije, možete koristiti **ConfigurationManager** predmete i zatim dodijeliti vrijednosti polja u kodu:
    
    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
