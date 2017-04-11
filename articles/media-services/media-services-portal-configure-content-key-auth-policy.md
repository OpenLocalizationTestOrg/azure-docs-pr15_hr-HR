<properties 
    pageTitle="Konfiguriranje sadržaja pravila autorizacije ključ pomoću portala za Azure | Microsoft Azure" 
    description="Saznajte kako konfigurirati pravilo autorizacije za sadržaja ključ." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="juliako"/>



#<a name="configure-content-key-authorization-policy"></a>Konfiguriranje pravila sadržaja autorizacije ključa
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


##<a name="overview"></a>Pregled

Microsoft Azure Media Services omogućuje isporuku MPEG-CRTICE, Smooth Streaming i HTTP-Live-strujeće (HLS) strujanja zaštićena je Napredno šifriranje standardne (AES) (pomoću tipke 128-bitno šifriranje) ili [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS omogućuje i izlaganje crtica strujanja šifrirane i zaštićene Widevine DRM. PlayReady i Widevine šifrirane po specifikacija uobičajenih šifriranja (ISO IEC 23001 7 CENC).

Media Services omogućuje i **Servis za isporuku ključ/licence** iz kojeg klijente možete dobiti AES tipke PlayReady/Widevine licenci da biste reproducirali šifrirane sadržaj.

U ovoj se temi objašnjava konfiguriranje pravilnika sadržaja ključa autorizacije pomoću portala za Azure. Tipku možete naknadno za dinamično Šifriraj sadržaj. Bilješke koje trenutno Šifrirajte sljedeće strujanje oblikuje: HLS MPEG CRTICE te Smooth Streaming. Ne možete šifrirati HDS streaming format ili Progresivni preuzimanja.

Kad je reproduktor zatraži strujanje postavljenom dinamički šifriranje, Media Services koristi tipku konfiguriran za dinamički Šifriraj sadržaj šifriranjem AES ili DRM. Dešifrirati toka reproduktora će zatražiti tipku s servis za isporuku ključa. Odlučite li korisnik ovlašten da biste dobili tipku servis vrednuje pravila za provjeru autentičnosti koji ste naveli za ključ.


Ako planirate imati više tipki sadržaja ili da biste odredili URL **Servis za isporuku ključ/licencu** osim servis za ključne isporuku Media Services, koristite Media Services .NET SDK ili REST API-ji.

[Konfiguriranje sadržaja pravila autorizacije ključ pomoću Media Services .NET SDK](media-services-dotnet-configure-content-key-auth-policy.md)

[Konfiguriranje sadržaja pravila autorizacije ključ pomoću Media Services REST API-JA](media-services-rest-configure-content-key-auth-policy.md)

###<a name="some-considerations-apply"></a>Neke informacije vrijediti:

- Da biste mogli koristiti dinamičke pakiranje i dinamični šifriranje, morate provjeriti da biste imati barem jedno strujanje rezervirane jedinicu. Dodatne informacije potražite u članku [upute za promjenu veličine servis za medijske sadržaje](media-services-portal-manage-streaming-endpoints.md).
- Vaš resursa mora sadržavati skup prilagodljivo brzina prijenosa MP4s ili datoteke Smooth Streaming prilagodljivo brzina prijenosa. Dodatne informacije potražite u članku [Kodiraj sredstava](media-services-encode-asset.md).
- Servis za isporuku ključ predmemorira ContentKeyAuthorizationPolicy i njegovih povezanih objekata (mogućnosti pravila i ograničenja) za 15 minuta.  Ako stvorite na ContentKeyAuthorizationPolicy i koristiti "Tokena" ograničenja, testirajte, i ažuriranje pravila "Otvori" ograničenja, trebat će otprilike 15 minuta prije pravila prebacuje na "Otvori" verziju pravila.


##<a name="how-to-configure-the-key-authorization-policy"></a>Kako: Konfiguriranje pravila ključa autorizacije

Da biste konfigurirali pravila ključa autorizacije, odaberite **Zaštiti sadržaja** stranice.

Media Services podržava više načina provjere autentičnosti korisnika za upućivanje zahtjeva za ključa. Pravila za sadržaja ključa autorizacije mogu sadržavati **otvorite**, **tokena**ili **IP** autorizacije ograničenja (**IP** može konfigurirati OSTALE ili .NET SDK).

###<a name="open-restriction"></a>Otvaranje ograničenja

Ograničenja **otvorite** znači sustav ćete održati tipku svakome tko čini ključa zahtjev. Možda je to ograničenje korisno za testiranje.

![OpenPolicy][open_policy]

###<a name="token-restriction"></a>Ograničenja tokena

Da biste odabrali tokena ograničena pravila, pritisnite gumb **TOKENA** .

**Tokena** ograničena pravila mora biti praćeni token izdala **Tokena servis za sigurnu** (STS). Media Services ne podržava tokeni u na **Jednostavne tokeni Web** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) i oblika **JSON Web tokena** (JWT). Informacije potražite u članku [JWT tokena provjere autentičnosti](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services ne nudi **Tokena servisa za sigurnu**. Možete stvoriti prilagođeni STS ili pod utjecajem Microsoft Azure ACS da biste tokeni problem. Na STS mora biti konfigurirano za stvaranje tokena prijavljeni pomoću navedeni ključ i problem zahtjevima koji ste naveli u konfiguraciji tokena ograničenja. Servis za ključne isporuku Media Services će vratiti ključa za šifriranje klijent Ako vrijedi token i zahtjevima u token podudaraju s dimenzijama i konfiguriran za tipku sadržaja. Dodatne informacije potražite u članku [Korištenje Azure ACS da biste tokeni problem](http://mingfeiy.com/acs-with-key-services).

Prilikom konfiguriranja na **TOKENA** ograničeno pravilnika, morate postaviti vrijednosti za **potvrdu ključ**, **izdavač** i **ciljne skupine**. Provjera primarni ključ sadrži ključ koji je potpisan token, izdavač je sigurna servis tokena koje izdaje token. Ciljne skupine (ponekad se zove i opseg) opisuje svrhu tokena ili resursa token neadministratorskog pristup. Servis za ključne isporuku Media Services Provjeri valjanost da te vrijednosti u token podudarati s vrijednostima u predlošku.

###<a name="playready"></a>PlayReady

Kada zaštita sadržaja pomoću **PlayReady**neke stvari koje morate odrediti u Pravilnik za autorizaciju je XML niz koji definira predložak PlayReady licence. Po zadanom postavljena se sljedeća pravila:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <LicenseType>Nonpersistent</LicenseType>
          <PlayRight>
            <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
          </PlayRight>
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

Možete kliknite gumb **Uvoz pravila xml** i dati različite XML koji u skladu s XML shemom definirani [ovdje](https://msdn.microsoft.com/library/azure/dn783459.aspx).


##<a name="next-step"></a>Sljedeći korak

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

