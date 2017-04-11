<properties 
    pageTitle="Zaštita sadržaja pregled | Microsoft Azure" 
    description="Članci pružaju pregled zaštitu sadržaja s Media Services." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="juliako"/>

#<a name="protecting-content-overview"></a>Zaštita pregled sadržaja


Microsoft Azure Media Services omogućuje sigurne medijskih sadržaja iz vrijeme ostavlja računalu putem prostora za pohranu, obrada i isporuke. Media Services omogućuje vam održavanje dinamički šifrirane i zaštićene Napredno šifriranje standardne (AES) (pomoću tipke 128-bitno šifriranje) ili bilo koji od glavnih DRMs uživo i osvježavati sadržaja: Microsoft PlayReady, Google Widevine i Apple FairPlay. Media Services i omogućuje uslugu za premještanje AES tipke i DRM (PlayReady, Widevine i FairPlay) licence ovlašteni klijente. 

Sljedeća slika prikazuje tijekova rada za zaštitu sadržaja koji podržava AMS. 

![Zaštita s PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[AZURE.NOTE]Da biste mogli koristiti dinamičke šifriranje, najprije se najmanje jednu strujanje rezervirane jedinicu na strujanje krajnjoj točki iz kojeg želite strujanje šifrirane sadržaj.

U ovoj se temi objašnjava [koncepata i terminologiju](media-services-content-protection-overview.md) odgovarajući zaštita sadržaja s AMS. U temi sadrži i [veze](media-services-content-protection-overview.md#common-scenarios) s temama koje pokazuju kako da biste postigli zadaci zaštitu sadržaja. 

##<a name="dynamic-encryption"></a>Dinamični šifriranje

Microsoft Azure Media Services omogućuje isporuku sadržaja dinamički šifrirane i zaštićene AES poništite okvir ključa ili DRM šifriranja: Microsoft PlayReady, Google Widevine i Apple FairPlay.

Trenutno šifriranje sljedećih strujanje oblika: HLS MPEG CRTICE te Smooth Streaming. Ne možete šifrirati HDS streaming format ili Progresivni preuzimanja.

Ako želite Media Services da biste šifrirali sredstvo, morate pridružiti ključa za šifriranje (CommonEncryption ili EnvelopeEncryption) na resursa i konfigurirati pravila za provjeru autentičnosti tipke.

Morate konfigurirati pravila za isporuku sredstava. Ako želite strujanje resursa za šifriranu prostora za pohranu, provjerite je li da biste odredili kako želite isporučiti konfiguriranjem resursa isporuke pravila.

Strujanje primitku tako da je reproduktor Media Services koristi navedeni ključ dinamički šifriranje sadržaja pomoću AES poništite okvir ključa ili DRM šifriranje. Dešifrirati toka reproduktora će zatražiti tipku s servis za isporuku ključa. Odlučiti hoće li se korisnik ovlašteni da biste dobili ključ, servis vrednuje pravila za provjeru autentičnosti koji ste naveli za ključ.

>[AZURE.NOTE]Da biste iskoristili dinamički šifriranje, najprije se najmanje jednu jedinicu strujanje na zahtjev za strujanje krajnju točku iz koje namjeravate isporuke šifrirane sadržaj. Dodatne informacije potražite [u](media-services-portal-manage-streaming-endpoints.md)članku skaliranje Media Services.

##<a name="storage-encryption"></a>Šifriranje prostora za pohranu

Koristite za pohranu šifriranje šifriranje Očisti sadržaj lokalno koristi AES 256 bitno šifriranje i prijenos Azure prostora za pohranu gdje je pohranjena šifriraju na ostale. Resursi koji su zaštićeni šifriranjem prostora za pohranu se automatski nešifrirani smještene u sustavu šifriranu datoteku prije no što kodiranja i po želji se ponovno šifrirane prije no što prenesete natrag kao novi izlaz resursa. Slučaj prvenstveno se koristi za pohranu šifriranje je kada želite sigurne visoke kvalitete unos medijske datoteke s šifriranjem na ostale na disku.

Da bi se isporučiti resursa za šifriranu prostora za pohranu, morate konfigurirati sredstva isporuke pravila da bi Media Services znali način za isporuku sadržaja. Prije moguće je strujanjem vaše resursa, strujanje poslužitelja uklanja šifriranje prostora za pohranu i strujanja sadržaja putem pravila navedeni isporuke (Ako, na primjer, AES, uobičajeni šifriranje ili nema šifriranja).

## <a name="common-encryption-cenc"></a>Uobičajeni šifriranja (CENC)

Šifriranje zajednički se koristi prilikom šifriranja sadržaja pomoću PlayReady ili / i Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Šifriranjem cbcs aapl

Cbcs aapl koristi prilikom šifriranja sadržaja pomoću FairPlay.

## <a name="envelope-encryption"></a>Šifriranje omotnice 

Tu mogućnost koristite ako želite zaštiti sadržaja pomoću AES 128 Očisti ključa. Ako želite dodatno zaštititi mogućnost, odaberite jednu od na DRMs navedena u ovoj temi. 

##<a name="licenses-and-keys-delivery-service"></a>Servis za isporuku licenci i ključevi

Media Services nudi uslugu izlaganja licence DRM (PlayReady, Widevine, FairPlay) i AES poništite prečace ovlašteni klijentima. [Portal za Azure](media-services-portal-protect-content.md), REST API-JA ili Media Services SDK za .NET možete koristiti da biste konfigurirali autorizacije i provjeru autentičnosti pravila za licence i ključevi.

##<a name="token-restriction"></a>Ograničenja tokena

Pravila za sadržaja ključa autorizacije može imati jednu ili više autorizacije ograničenja: otvaranje ili token ograničenja. Znak izdan tako da na sigurne tokena servisa (STS) mora biti praćeni tokena ograničena pravila. Media Services ne podržava tokeni u na jednostavne tokeni Web (SWT) i oblika JSON Web tokena (JWT). Media Services ne nudi tokena servisa za sigurnu. Možete stvoriti prilagođeni STS ili pod utjecajem Microsoft Azure ACS da biste tokeni problem. Na STS mora biti konfigurirano za stvaranje tokena prijavljeni pomoću navedeni ključ i problem zahtjevima koji ste naveli u konfiguraciji tokena ograničenja. Servis za ključne isporuku Media Services će vratiti tražene ključ (ili više licenci) klijent Ako vrijedi token i zahtjevima u tokena podudaranje one konfiguriran za ključ (ili licence).

Konfiguriranje token ograničeno pravilnika, morate navesti potvrdu primarni ključ, izdavača i publike parametre. Provjera primarni ključ sadrži ključ koji je potpisan token, izdavač je sigurna servis tokena koje izdaje token. Ciljne skupine (ponekad se zove i opseg) opisuje svrhu tokena ili resursa token neadministratorskog pristup. Servis za ključne isporuku Media Services Provjeri valjanost da te vrijednosti u token podudarati s vrijednostima u predlošku.

##<a name="streaming-urls"></a>Strujanje URL-ova

Ako je vaš resursa šifrirana s više od jedne DRM, koristite oznaku šifriranje u strujanje URL-: (oblik = 'm3u8-aapl' šifriranje = "xxx").

Primjena Imajte na umu sljedeće:

- Moguće je navesti samo nula ili jednu vrstu šifriranja.
- Vrsta šifriranja nema da bude navedena u URL-a ako samo jedan šifriranje je primijenjena na imovinu.
- Vrsta šifriranja je slova.
- Možete navesti sljedeće vrste šifriranja:  
    - **cenc**: uobičajene šifriranja (Playready ili Widevine)
    - **cbcs aapl**: Fairplay
    - **cbc**: AES šifriranje omotnice.

##<a name="common-scenarios"></a>Uobičajeni scenariji

U sljedećim temama Demonstracija zaštiti sadržaja u odjeljku pohrana izlaganje dinamički šifrirane strujanja medijskih sadržaja, koristite servis za isporuku AMS ključ/licence

- [Zaštita s AES](media-services-protect-with-aes128.md) 
- [Zaštita s PlayReady i/ili Widevine](media-services-protect-with-drm.md)
- [Strujanje na zaštićenom HLS sadržaja s Apple FairPlay i/ili PlayReady](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Dodatni scenariji

- [Integraciji servisa Azure PlayReady licence s poslužiteljem vlastite šifriranje/strujanje](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
- [Korištenje castLabs isporuka DRM licence servisa Azure Media Services](media-services-castlabs-integration.md)
 
##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Srodne veze

[Objavljivanje PlayReady kao servisa i AES šifriranje dinamički pomoću servisa Azure Media Services](http://mingfeiy.com/playready)

[Azure Media Services PlayReady licence isporuke cijene objašnjenje](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Upute za ispravljanje pogrešaka za AES šifrirane strujanje u servisa Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT tokena authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integrirati Azure Media Services OWIN MVC temelji aplikaciju pomoću servisa Azure Active Directory i ograničavanje isporuku sadržaja ključa koji se temelji na zahtjevima JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Korištenje Azure ACS da biste tokeni problem](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
