<properties 
    pageTitle="Konfiguriranje pravilnika o zaštiti sadržaja pomoću portala za Azure | Microsoft Azure" 
    description="U ovom se članku objašnjava kako pomoću portala za Azure konfiguriranje pravilnika o zaštiti sadržaja. U članku također prikazuje kako omogućiti dinamički šifriranje za imovinu." 
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
    ms.date="10/24/2016"    
    ms.author="juliako"/>

# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Konfiguriranje pravilnika o zaštiti sadržaja pomoću portala za Azure

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

## <a name="overview"></a>Pregled

Microsoft Azure Media Services (AMS) omogućuje sigurne medijskih sadržaja iz vrijeme ostavlja računalu putem prostora za pohranu, obrada i isporuke. Media Services omogućuje vam za isporuku sadržaja šifrirane dinamički s Napredno šifriranje standardne (AES) (pomoću tipke 128-bitno šifriranje), uobičajeni šifriranja (CENC) pomoću PlayReady i/ili Widevine DRM i Apple FairPlay. 

AMS nudi uslugu izlaganja DRM licenci i AES poništite prečace ovlašteni klijentima. Portal za Azure omogućuje stvaranje jednog **pravila autorizacije ključ/licencu** za sve vrste encryptions.

U ovom se članku objašnjava kako konfigurirati pravila za zaštitu sadržaja pomoću portala za Azure. U članku prikazuje i kako da biste primijenili dinamički šifriranje svoje imovine.

> [AZURE.NOTE]  Ako ste koristili Azure klasični portal za stvaranje pravila za zaštitu, pravilnike možda se neće pojaviti [Azure portal](https://portal.azure.com/). No sva pravila stari i dalje postoji. Možete provjeriti pomoću Azure Media Services .NET SDK ili alat za [Azure-Media-servise-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) (da biste vidjeli pravilnike, desnom tipkom miša na imovinu -> Prikaz podataka (F4) -> kliknite na kartici sadržaja tipke -> pritisnite tipku). 
> 
> Ako želite šifrirati vaše resursa pomoću novog pravila, konfigurirati ih pomoću portala za Azure, kliknite Spremi i ponovna primjena dinamički šifriranje. 

## <a name="start-configuring-content-protection"></a>Pokretanje konfiguriranje zaštitu sadržaja

Da biste koristili portalu da biste pokrenuli konfiguriranje zaštitu sadržaja, globalni s vašim računom AMS, učinite sljedeće:

1. [Portal za Azure](https://portal.azure.com/)odaberite svoj račun servisa Azure Media Services.
2. Odaberite **Postavke** > **zaštitu sadržaja**.

![Zaštita sadržaja](./media/media-services-portal-content-protection/media-services-content-protection001.png)
 

## <a name="keylicense-authorization-policy"></a>Pravila autorizacije ključ/licence

AMS podržava više načina provjere autentičnosti korisnika za upućivanje zahtjeva za web-mjesto ključa ili u okvir za licencu. Pravila za sadržaja ključa autorizacije mora biti konfigurirana tako da vam i podudarati klijent redoslijedom za ključ/licencu za biti delived klijentu. Pravila za sadržaja ključa autorizacije može imati jednu ili više autorizacije ograničenja: **Otvaranje** ili **tokena** ograničenja.

Portal za Azure omogućuje stvaranje jednog **pravila autorizacije ključ/licencu** za sve vrste encryptions.

###<a name="open"></a>Otvaranje 

Otvaranje ograničenja znači sustav ćete održati tipku svakome tko čini ključa zahtjev. To ograničenje može biti korisno za potrebe za testiranje. 

### <a name="token"></a>Tokena

Znak izdan tako da na sigurne tokena servisa (STS) mora biti praćeni tokena ograničena pravila. Media Services ne podržava tokeni u na jednostavne tokeni Web (SWT) i oblika JSON Web tokena (JWT). Media Services ne nudi tokena servisa za sigurnu. Možete stvoriti prilagođeni STS ili pod utjecajem Microsoft Azure ACS da biste tokeni problem. Na STS mora biti konfigurirano za stvaranje tokena prijavljeni pomoću navedeni ključ i problem zahtjevima koji ste naveli u konfiguraciji tokena ograničenja. Servis za ključne isporuku Media Services će vratiti tražene ključ (ili više licenci) klijent Ako vrijedi token i zahtjevima u tokena podudaranje one konfiguriran za ključ (ili licence).

Konfiguriranje token ograničeno pravilnika, morate navesti potvrdu primarni ključ, izdavač i publike parametara. Provjera primarni ključ sadrži ključ koji je potpisan token, izdavač je sigurna servis tokena koje izdaje token. Ciljne skupine (ponekad se zove i opseg) opisuje svrhu tokena ili resursa token neadministratorskog pristup. Servis za ključne isporuku Media Services Provjeri valjanost da te vrijednosti u token podudarati s vrijednostima u predlošku.

![Zaštita sadržaja](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>Predložak PlayReady prava

Detaljne informacije o pravima predložak PlayReady potražite u članku [Pregled predloška za Media Services PlayReady licence](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Osobe koje nisu stalni

Ako licence konfigurirati kao koje nisu stalni, ga samo čuva se memorije dok reproduktora koristi licencu.  

![Zaštita sadržaja](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Stalni

Ako konfigurirate licence kao što je stalni, sprema se u stalnih prostor za pohranu na klijentu.

![Zaštita sadržaja](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Predložak Widevine prava

Detaljne informacije o pravima predložak Widevine potražite u članku [Pregled predloška za Widevine licence](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Osnovni

Kad odaberete **Osnovni**predložak se stvara s sve zadane vrijednosti.

### <a name="advanced"></a>Napredno

Detaljno objašnjenje o advance mogućnost konfiguracija Widevine potražite u članku [u ovoj](media-services-widevine-license-template-overview.md) se temi.

![Zaštita sadržaja](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>Konfiguriranje FairPlay

Da biste omogućili FairPlay šifriranja, morate unijeti certifikat aplikaciju i aplikaciji tajna ključ (ASK) putem mogućnost konfiguracija FairPlay. Detaljne informacije o konfiguraciji FairPlay i preduvjetima potražite u [ovom](media-services-protect-hls-with-fairplay.md) članku.

![Zaštita sadržaja](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Primjena dinamički šifriranje vaše resursa

Da biste iskoristili dinamički šifriranja, morate učinite sljedeće:

- Kodiranje izvornu datoteku u skupu prilagodljivo brzina prijenosa MP4 datoteke.
- Pronađite najmanje jednu jedinicu strujanje na zahtjev za strujanje krajnju točku iz koje namjeravate održati sadržaj. Dodatne informacije potražite u članku [upute za promjenu veličine na zahtjev strujanje rezervirane jedinice](media-services-portal-manage-streaming-endpoints.md).

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Odaberite sredstvo koju želite šifrirati

Da biste vidjeli sva sredstva, odaberite **Postavke** > **Resursi**.

![Zaštita sadržaja](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Šifriraj pomoću AES ili DRM

Kada pritisnete tipku **šifriranje** na sredstvo, su usmjereni s dva izbora: **AES** ili **DRM**. 

#### <a name="aes"></a>AES

Očisti ključa šifriranja omogućit će se na sve protokola za strujanje AES: Smooth Streaming, HLS i MPEG-crtica.

![Zaštita sadržaja](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM

Kad odaberete karticu DRM, su usmjereni s različitim mogućnostima pravila za zaštitu sadržaja (koji moraju imati konfigurirao sada) + skup protokoli za strujanje.

- **PlayReady i Widevine crticom MPEG** - dinamički šifriranje vaše strujanje MPEG-CRTICOM PlayReady i Widevine DRMs.
- **PlayReady i Widevine s MPEG-CRTICOM + FairPlay s HLS** – će dinamički šifriranje koje MPEG-CRTICOM strujanje PlayReady i Widevine DRMs. Će šifriranja strujanje HLS s FairPlay.
- **PlayReady samo s Smooth Streaming HLS te CRTICOM MPEG** - će dinamički Šifrirajte Smooth Streaming, HLS, MPEG-CRTICOM strujanja s PlayReady DRM.
- **Widevine samo s CRTICOM MPEG** - dinamički šifriranje koje MPEG-CRTICOM s Widevine DRM.
- **FairPlay samo s HLS** - će dinamički šifriranje vaše HLS strujanje FairPlay.

Da biste omogućili FairPlay šifriranja, morate pružaju certifikat aplikaciju i aplikaciji tajna ključ (ASK) do mogućnost konfiguracija FairPlay plohu postavke za zaštitu sadržaja.

![Zaštita sadržaja](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Kada unesete odabir šifriranja, pritisnite **Primijeni**.

##<a name="next-steps"></a>Daljnji koraci

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





