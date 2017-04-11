<properties 
    pageTitle="Korištenje Axinom isporuka Widevine licenci servisa Azure Media Services | Microsoft Azure" 
    description="U ovom se članku opisuje kako pomoću servisa Azure Media Services (AMS) izlaganje strujanje dinamički šifrirane po AMS s PlayReady i Widevine DRMs. Licenca PlayReady isporučuje se s poslužitelja licence Media Services PlayReady i Widevine licence je isporučen Axinom licence poslužitelj." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"   
    ms.author="willzhan;Mingfeiy;rajputam;Juliako"/>

#<a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Korištenje Axinom isporuka Widevine licenci servisa Azure Media Services  

> [AZURE.SELECTOR]
- [castLabs](media-services-castlabs-integration.md)
- [Axinom](media-services-axinom-integration.md)

##<a name="overview"></a>Pregled

Azure Media Services (AMS) dodao Google Widevine dinamički zaštitu (pogledajte [Mingfei na blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) detalje). Osim toga, Azure Media Player (AMP) i dodao podršku Widevine (Vidi [dokument AMP](http://amp.azure.net/libs/amp/latest/docs/) detalje). Ovo je glavna accomplishment u strujanje CRTICE sadržaj zaštićen CENC višestruka-native-DRM (PlayReady i Widevine) na Moderna preglednicima opremljeno MSE i EME.

Počevši od Media Services .NET SDK verzija 3.5.2, Media Services omogućuje konfiguriranje predloška Widevine licence te pronađite Widevine licence. Sljedeće AMS partnera možete koristiti i da biste lakše izlaganje Widevine licenci: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/).

U ovom se članku opisuje kako integrirati i testiranje Widevine licencni poslužitelj upravlja Axinom. Konkretno, pokriva:  

- Konfiguriranje dinamičkih uobičajenih šifriranja s više – DRM (PlayReady i Widevine) s odgovarajuće licence acquisition URL-ovima;
- Generiranje JWT token da bi se zadovoljava preduvjete poslužitelja licence;
- Razvoj aplikacija za Azure Media Player koji se rukuje dobivanje licence s JWT tokena autentičnosti;

Dovršavanje sustav i tijek sadržaja ključa, ključne ID-a, ključne Početni broj, JTW token i njegov zahtjevima se može najbolje opisati na sljedećem su dijagramu.

![CRTA i CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

##<a name="content-protection"></a>Zaštita sadržaja

Za konfiguriranje pravila ključa isporuke i dinamični zaštitu, pročitajte članak Mingfei na blogu: [kako konfigurirati Widevine pakiranje pomoću servisa Azure Media Services](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

Dinamični CENC zaštitu s više DRM možete konfigurirati za CRTICE strujanje pojavljuju oboje od sljedećeg:

1. Zaštita PlayReady za MS Edge i IE11 koja bi mogla imati tokena autorizacije ograničenja. Tokena ograničena pravila mora biti praćeni znak izdan tako da na sigurne tokena servisa (STS), kao što su Azure Active Directory;
1. Zaštita Widevine za Chrome je potreban tokena provjere autentičnosti s tokena koje izdaje drugi STS. 

[JWT tokena generacije](media-services-axinom-integration.md#jwt-token-generation) potražite u odjeljku Zašto Azure Active Directory nije moguće koristiti kao programa STS Axinom na Widevine licence poslužitelja.

###<a name="considerations"></a>Razmatranja

1. Morate koristiti Axinom navedene ključne Početni broj (8888000000000000000000000000000000000000) i uređajima generirani i odabrane ključa ID da biste generirali sadržaja ključ za konfiguriranje servisa za ključne isporuke. Axinom licencni poslužitelj će izdati sve licence koje sadrže sadržaja tipke koje se temelji na isti ključa polazne, koji je valjan za testiranje i radni.
1. URL za nabavu Widevine licencu za testiranje: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). HTTP- a HTTS dopušteno.

##<a name="azure-media-player-preparation"></a>Priprema Azure Media Player

AMP v1.4.0 podržava reprodukciju AMS sadržaja koji se dinamički isporučen PlayReady i Widevine DRM.
Ako Widevine licencni poslužitelj zahtijeva tokena provjeru autentičnosti, nema ništa dodatne što trebate učiniti da biste testirali CRTICE sadržaj zaštićen Widevine. Na primjer, tim AMP omogućuje jednostavno [uzorka](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), gdje možete ga vidjeti rad na rub i IE11 s PlayReady i Chrome s Widevine.
Poslužitelj za licence na Widevine nudi Axinom zahtijeva JWT tokena provjere autentičnosti. JWT token mora biti poslan sa zahtjevom za licence putem HTTP zaglavlje "X-AxDRM-poruke". U tu svrhu morate dodati sljedeći JavaScript koda na web-stranici nalaze AMP prije postavljanja izvorišnog web-mjesta:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

Ostatak AMP kod je standardni API AMP kao dokument AMP [ovdje](http://amp.azure.net/libs/amp/latest/docs/).

Imajte na umu da iznad javascript za zaglavlje postavka prilagođene autorizacije i dalje pristup kratki termina prije službenog dugoročnu pristup u AMP je objavio.

##<a name="jwt-token-generation"></a>JWT tokena generacije

Axinom Widevine licence poslužitelj za testiranje zahtijeva JWT tokena provjere autentičnosti. Uz to, jedan od zahtjevima u JWT token je složene objekt vrste umjesto vrsta jednostavne podataka.

Nažalost, Azure AD samo možete izdati JWT tokeni s jednostavne vrste. Isto tako, .NET Framework API-JA (System.IdentityModel.Tokens.SecurityTokenHandler i JwtPayload) samo omogućuje kao zahtjevima za unos vrsta složene objekta. Međutim, na zahtjevima se i dalje serijalizirani kao niz. Stoga smo ne možete koristiti bilo koji od dva za generiranje token JWT Widevine licence zahtjeva za.


Nevena Sheehan [JWT Nuget paketa](https://www.nuget.org/packages/JWT) odgovara potrebama pa ćemo namjeravate koristiti taj paket Nuget.

U nastavku je kod za stvaranje tokena JWT s potrebne zahtjevima prema potrebi Axinom Widevine licence poslužitelj za testiranje:

    
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;
    
    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.
    
                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };
    
                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);
    
                return token;
            }
    
            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }
    
                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }
    
                return HexAsBytes;
            }
    
        }  
    
    }  

Axinom Widevine licencni poslužitelj

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

###<a name="considerations"></a>Razmatranja

1.  Čak i ako je potreban je servis za isporuku licence AMS PlayReady "nošenja =" ispred token za provjeru autentičnosti, Axinom Widevine licencni poslužitelj ne koristi.
2.  Tipku komunikacije Axinom koristi se kao potpisivanja ključ. Imajte na umu da tipku hex niza, ali ga treba tretirati kao niz bajtova ne niz kada kodiranja. Pritom metodu ConvertHexStringToByteArray.

##<a name="retrieving-key-id"></a>Dohvaćanje ključne ID-a

Možda ste primijetili da kod za generiranje na JWT tokena, ključne ID-a potreban je. Budući da u JWT tokena mora biti jeste li spremni prije reproduktora učitavanja AMP ključa ID mora biti dohvaćeni da biste generirali JWT token.

Naravno, postoje različite načine da biste dobili držite tipku ID-a. Na primjer, možda nešto pohraniti ključa ID zajedno s sadržaja metapodataka u bazi podataka. Ili možete dohvatiti ključ ID iz datoteke MPD CRTICU (Opis prezentacije medijskih sadržaja). Kod u nastavku je za drugu mogućnost.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }
    
        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();
    
        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);
    
        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }
    
        return key_id;
    }

##<a name="summary"></a>Sažetak

S najnovijim Widevine podrške u zaštiti sadržaja servisa Azure Media i Azure Media Player, možemo implementirati strujanje CRTICE + višestruka-native-DRM (PlayReady + Widevine) oba servisa PlayReady licence u AMS i Widevine licencni poslužitelj iz Axinom za sljedeće Moderna preglednike:

- Chrome
- Microsoft Edge na Windows 10
- IE 11 sustavu Windows 8.1 i Windows 10
- Firefox (radna površina) i Safari na Mac (ne iOS) i podržani putem Silverlight i iste URL s Azure Media Player

Sljedećih parametara potrebni su u licencni poslužitelj za leveraging Axinom Widevine rješenja kliknite Spremi. Osim ključ ID-a, ostatak parametara nudi Axinom na temelju njihove Widevine instalacija poslužitelja.


Parametar|Kako se koristi
---|---
Komunikacija ključa ID-a|Potrebno je uključiti kao vrijednost zahtjeva "com_key_id" u JWT token (pogledajte članak [u ovom](media-services-axinom-integration.md#jwt-token-generation) se odjeljku).
Ključ komunikacije|Morate koristiti kao tipku za potpisivanje tokena JWT (pogledajte članak [u ovom](media-services-axinom-integration.md#jwt-token-generation) se odjeljku).
Ključni Početni broj|Moraju se koristiti za stvaranje sadržaja ključ uz sve sadržaje za dani ključa ID-a (pogledajte članak [u ovom](media-services-axinom-integration.md#content-protection) se odjeljku).
URL za nabavu Widevine licence|Mora se koristiti u konfiguriranje pravilnika za isporuku resursa za CRTICE strujanje (pogledajte [Ova](media-services-axinom-integration.md#content-protection) sekcija).
ID sadržaja ključa|Mora biti dio vrijednost zahtjeva prava poruke JWT tokena (pogledajte članak [u ovom](media-services-axinom-integration.md#jwt-token-generation) se odjeljku). 


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Potvrda 

Želimo svjesni sljedeće osobe pridonio pri stvaranju ovaj dokument: Kristjan Jõgi od Axinom, Mingfei Yan i Amit Rajput.
