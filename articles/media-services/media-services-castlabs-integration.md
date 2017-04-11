<properties 
    pageTitle="Korištenje castLabs isporuka Widevine licenci servisa Azure Media Services | Microsoft Azure" 
    description="U ovom se članku opisuje kako pomoću servisa Azure Media Services (AMS) izlaganje strujanje dinamički šifrirane po AMS s PlayReady i Widevine DRMs. Licenca PlayReady isporučuje se s poslužitelja licence Media Services PlayReady i Widevine licence je isporučen castLabs licence poslužitelj." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="Mingfeiy;willzhan;Juliako"/>


#<a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>Korištenje castLabs isporuka Widevine licenci servisa Azure Media Services

> [AZURE.SELECTOR]
- [Axinom](media-services-axinom-integration.md)
- [castLabs](media-services-castlabs-integration.md)

##<a name="overview"></a>Pregled

U ovom se članku opisuje kako pomoću servisa Azure Media Services (AMS) izlaganje strujanje dinamički šifrirane po AMS s PlayReady i Widevine DRMs. Licenca PlayReady isporučuje se s poslužitelja licence Media Services PlayReady i Widevine licence je isporučen **castLabs** licence poslužitelj.

Za reprodukciju strujanje sadržaj zaštićen CENC (PlayReady i/ili Widevine), možete koristiti [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Detalje potražite u članku [AMP dokumenta](http://amp.azure.net/libs/amp/latest/docs/) .

Na sljedećem su dijagramu pokazuje više razine servisa Azure Media Services i castLabs Integracija arhitektura.

![Integracija](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

##<a name="typical-system-set-up"></a>Uobičajene sistemske postavljanje

- Medijski sadržaj pohranjen na AMS.
- Ključ ID-a sadržaja ključeva spremaju se u castLabs i AMS.
- castLabs i AMS imaju ugrađena tokena provjere autentičnosti. U sljedećim se odjeljcima navode tokeni za provjeru autentičnosti. 
- Kada zahtjeve klijenta za strujanje videozapisa, sadržaj se dinamički šifrirane i zaštićene **Uobičajenih šifriranja** (CENC) i dinamički pakirat po AMS Smooth Streaming i CRTICU. Ne možemo izlaganje i šifriranje osnovne strujanje PlayReady M2TS HLS strujanje protokol.
- Licenca PlayReady dohvaća iz AMS licencni poslužitelj i Widevine licence dohvaća iz castLabs licencni poslužitelj. 
- Reproduktor automatski odlučuje koje licencu za dohvaćanje na temelju platformu mogućnost klijenta. 

##<a name="authentication-token-generation-for-getting-a-license"></a>Provjera autentičnosti tokena generacije za početak licence

CastLabs i AMS podržava JWT (JSON Web tokena) tokena oblik koji se koristi da biste autorizirali licence. 

###<a name="jwt-token-in-ams"></a>Token JWT u AMS 

U sljedećoj tablici opisane token JWT u AMS. 

Izdavač|Izdavač niza iz s odabranim sigurne tokena servisa (STS)
---|---
Ciljne skupine|Ciljne skupine niza iz korištenih STS
Zahtjevima|Skup zahtjevima
NotBefore|Pokretanje valjanost tokena
Istječe|Kraj valjanost tokena
SigningCredentials|Ključ koji se zajednički koristi PlayReady licencni poslužitelj, castLabs licencni poslužitelj i STS, Razlog može biti simetričnu ili asimetričnim ključ.

###<a name="jwt-token-in-castlabs"></a>Token JWT u castLabs

U sljedećoj tablici opisane token JWT u castLabs. 

Ime|Opis
---|---
optData|JSON niz koji sadrži informacije o vama. 
CRT|JSON niz koji sadrži informacije o imovini, njegov licencna informacije i reprodukcija prava.
iat|Na trenutni datum i vrijeme u epoch.
jti|Jedinstveni identifikator o ovaj token (svaki token može se koristiti samo jednom u sustavu castLabs).

##<a name="sample-solution-set-up"></a>Ogledno rješenje postavljanje 

[Uzorak rješenja](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) sastoji se od dva projekata:

-   Konzola aplikacija koje je moguće koristiti za postavljanje ograničenja DRM na sredstvo već ingested PlayReady i Widevine.
-   Web-aplikacija koje ruke out tokeni koje se može prikazivati kao vrlo POJEDNOSTAVNJENI verziju programa STS.


Da biste koristili aplikaciju konzole:

1.  Promijenite app.config Postavljanje vjerodajnica za AMS, castLabs vjerodajnice, STS konfiguracije i zajednički ključ.
2.  Prenesite sredstva u AMS.
3.  Dohvaćanje na UUID iz prenesenih resursa i promijenite 32 redak u datoteci Program.cs:

         var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();

4.  Korištenje programa idstavke za davanje naziva imovine u sustavu castLabs (redak 44 u datoteci Program.cs).

    Morate postaviti idstavke za **castLabs**; ona mora biti jedinstveni alfanumerički niz.

5.  Pokrenite program.


Da biste koristili aplikaciju Web (STS):

1.  Promjena datoteka web.config u trgovini castlabs postavljanje ID, STS konfiguraciju i zajednički ključ.
2.  Implementacija Azure web-mjesta.
3.  Idite na web-mjesto.

##<a name="playing-back-a-video"></a>Reprodukcija videozapisa

Za reprodukciju videozapisa šifrirane i zaštićene uobičajenih šifriranja (PlayReady i/ili Widevine), možete koristiti [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Kada se pokrene aplikaciju konzole echoed su ID sadržaja ključ i Manifest URL.

1.  Otvorite novu karticu, a zatim pokretanje sustava STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2.  Idite na [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3.  Zalijepite URL za strujanje.
4.  Potvrdite okvir **Dodatne mogućnosti** .
5.  Na padajućem popisu **zaštite** odaberite PlayReady i/ili Widevine.
6.  Zalijepite token koju ste dobili od vašeg STS u tekstni okvir Token. 
    
    Licencni poslužitelj castLab nije potrebna na "nošenja =" prefiks ispred token. Tako, uklonite koji prije slanja token.
7.  Ažurirajte reproduktora.
8.  Videozapis se moraju se reproducira.


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
