<properties 
    pageTitle="Napomene o Media Services | Microsoft Azure" 
    description="Media Services napomene" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="media" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

# <a name="azure-media-services-release-notes"></a>Azure Media Services napomene

Ove napomene sažetak promjene iz prethodnog izdanja i poznati problemi.

>[AZURE.NOTE] Želimo da biste čuli klijentima i usredotočite se na rješavanje problema koji utječu na koje. Da biste prijavite problem ili postavljati pitanja, ponovno objaviti na [MSDN Forum za Azure Media Services].


##<a id="issues"></a>Trenutno Poznati problemi

### <a id="general_issues"></a>Media Services općeniti problemi

Problem|Opis
---|---
Nekoliko uobičajenih HTTP zaglavlja ne prikazuje se u REST API-JA.|Ako razvijate Media Services aplikacije koje se koriste REST API-JA, pronađete koji nekoliko uobičajenih polja zaglavlja HTTP (uključujući – zahtjev-ID KLIJENTA, ID ZAHTJEVA i POVRATNA-– zahtjev-ID KLIJENTA) nisu podržane. Zaglavlja dodat će se u budućem ažuriranju.
Šifriranje u obliku postotaka nije dopušteno.|Media Services koristi vrijednost svojstva IAssetFile.Name kada sastavljanje URL-ovi za strujanje sadržaj (na primjer, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Zbog toga šifriranje u obliku postotaka nije dopušteno. Svojstvo **ime** ne smije sadržavati sljedeće [znakove postotak kodiranje-rezervirana](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Osim toga, može postojati samo jedan '. " za nastavak.
Način ListBlobs koja je dio ne uspije 3.x verziju SDK Azure prostora za pohranu.|Media Services stvara SAS URL-ovi koji se temelji na verziji [2012 02 12](http://msdn.microsoft.com/library/azure/dn592123.aspx) . Ako želite koristiti SDK Azure prostora za pohranu na popisu blob-ova u spremniku blob, upotrijebite metodu [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) koja je dio SDK za pohranu Azure verzija 2.x. Metoda ListBlobs koja je dio verzije SDK za pohranu Azure neće uspjeti 3.x.
Media Services ograničavanje mehanizam ograničava korištenje resursa za aplikacije koje viškom zahtjev za uslugu. Servis može vratiti servis nije dostupan (503) HTTP kod stanja.|Dodatne informacije potražite u članku opis 503 Šifra stanja HTTP u temi [Šifre pogrešaka za Azure Media Services](http://msdn.microsoft.com/library/azure/dn168949.aspx) .
Prilikom postavljanja upita entiteti, postoji ograničenje od 1000 entiteti vraća odjednom jer javno OSTALE v2 Ograničava rezultate upita 1000 rezultate. | Morate koristiti **Preskoči** i **iskoristite** (.NET) / **Vrh** (REST) kao što je opisano u [ovom primjeru .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) i [u ovom se primjeru REST API -JA](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
Neki klijenti mogu potjecati preko problema ponavljanja oznaka u manifestu Smooth Streaming.|Dodatne informacije potražite u članku [ovaj](media-services-deliver-content-overview.md#known-issues) odjeljak.
Azure Media Services .NET SDK objekata ne može se serijalizirati i kao rezultat ne rade s predmemoriranje Azure.|Ako pokušate serijalizirati SDK AssetCollection objekt da biste dodali Azure predmemoriranje Izbačena je iznimka.
Kodiranje poslovi neće funkcionirati uz poruku niz "faza: DownloadFile. Kod: System.NullReferenceException ".|Uobičajeni kodiranja tijek rada je da biste prenijeli unos video datoteka za unos resursa i poslati jedan ili više kodiranja poslova za taj unos resursa, bez daljnje izmjene koje resursa za unos. Međutim, ako mijenjate unos resursa (na primjer po Dodavanje i brisanje i preimenovanje datoteke unutar imovine), zatim sljedeći poslovi možda neće funkcionirati uz DownloadFile pogreške. Rješenje se da biste izbrisali unos resursa i ponovno prenijeti unos datoteke novu imovinu. 

##<a id="rest_version_history"></a>REST API povijest

Informacije o Media Services REST API-JA povijesti verzija, potražite u članku [Azure Media Services REST API Reference].

##<a id="july_changes16"></a>Izdanje srpanj 2016

###<a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Ažuriranja u datoteku manifesta (*. ISM) generira kodiranje zadataka

Kodiranje zadatak je poslan na Media Encoder standardno ili Azure Media Encoder, kodiranja zadatak generira [strujanje datoteka manifesta](media-services-deliver-content-overview.md) (* .ism) datoteke u izlaz resursa. Pomoću najnovije izdanje servisa, vidjet ćete da sintaksa strujanje manifesta datoteke je ažuriran.

>[AZURE.NOTE]Vidjet ćete da sintaksa strujanje manifest (.ism) datoteke je rezervirano za internu upotrebu, a mogu se promijeniti u buduća izdanja. Provjerite ne mijenjajte i manipulirati sadržaj ove datoteke.

###<a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Novi klijent manifest (*. U rezultatu generira se ISMC) datoteka resursa kada kodiranja zadatka proizvodi MP4 datoteke

Počevši od najnovije izdanje servisa nakon dovršetka kodiranja zadataka koje generira nešto više datoteka MP4, izlaz resursa i sadržavat će strujanje datoteke klijent manifest (*.ismc). Datoteka .ismc poboljšava performanse dinamički strujanje. 

>[AZURE.NOTE]Vidjet ćete da sintaksa manifest (.ismc) datoteke klijent je rezervirano za internu upotrebu, a mogu se promijeniti u buduća izdanja. Provjerite ne mijenjajte i manipulirati sadržaj ove datoteke.

Dodatne informacije potražite u članku [ovom](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blogu.

### <a name="known-issues"></a>Poznati problemi

Neki klijenti mogu potjecati aross problema ponavljanja oznaka u manifestu Smooth Streaming. Dodatne informacije potražite u članku [ovaj](media-services-deliver-content-overview.md#known-issues) odjeljak.

##<a id="apr_changes16"></a>2016 u travnju izdanje

### <a name="azure-media-analytics"></a>Azure Media Analytics

Azure Media Servces uvedena Azure Media Analytics za napredne obavještavanje videozapisa. Detaljne informacije potražite u članku [Azure Media Services analize pregled](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (pretpregled)

Azure Media Services sada omogućuje dinamički šifriranje vaše HTTP Live strujanje (HLS) sadržaja s Apple FairPlay. Servis za isporuku AMS licence možete koristiti i izlaganje FairPlay licence za klijente. Više informacija potražite u članku [korištenje servisa Azure Media Services za strujanje sustava HLS sadržaja zaštićenog s Apple FairPlay ](media-services-protect-hls-with-fairplay.md).
  
##<a id="feb_changes16"></a>Izdanje veljača 2016

Najnoviju verziju programa Azure Media Services SDK za .NET (3.5.3) sadrži fix pogrešku povezane Widevine. Problem je bio: AssetDeliveryPolicy nije moguće ponovno iskoristiti više imovine šifrirane i zaštićene Widevine. Kao dio taj se popravak pogrešku sljedeće svojstvo dodan u SDK: **WidevineBaseLicenseAcquisitionUrl**.
    
    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},
        
    };

##<a id="jan_changes_16"></a>Siječanj 2016 izdanje

Kodiranje rezervirane jedinice preimenovati da biste smanjili zbrku s nazivima kodiranje.

Basic, standardna i Premium kodiranja rezervirane jedinice preimenuju se S1, S2, a S3 rezervirana jedinice, odnosno.  Korisnike koji koriste osnovni RUs kodiranje danas prikazat će se S1 kao natpis u Azure Portal (i naplate), tijekom standardnu i Premium vidjet ćete oznake S2 i S3 odnosno. 

##<a id="dec_changes_15"></a>Izdanje prosinac 2015.

Tim za Azure SDK objaviti novo izdanje [Azure SDK za i](http://github.com/Azure/azure-sdk-for-php) paket koji sadrži nove značajke i ažuriranjima za Microsoft Azure Media Services. Posebno Azure Media Services SDK za i sada podržava najnovije značajke [zaštitu sadržaja](media-services-content-protection-overview.md) : dinamički šifriranja s AES i DRM (PlayReady i Widevine) sa ili bez ograničenja Token. Također podržava skaliranje [Kodiranje jedinice](media-services-dotnet-encoding-units.md).

Dodatne informacije potražite u članku:

- Blog [Microsoft Azure Media Services SDK za i](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) .
- Sljedeća [primjere koda](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) za jednostavniji početak rada brzo:
    - **vodworkflow_aes.php**: Ovo je datoteka PHP koji pokazuje kako koristiti AES 128 dinamički šifriranja i servis za isporuku ključ. Temelji se na ogledne .NET uputama u [ovom](media-services-protect-with-aes128.md) se članku.
    - **vodworkflow_aes.php**: Ovo je datoteka i koji pokazuje kako koristiti PlayReady dinamički šifriranja i servis za isporuku licence. Temelji se na ogledne .NET uputama u [ovom](media-services-protect-with-drm.md) se članku.
    - **scale_encoding_units.php**: PHP datoteka koja prikazuje kako skaliranje kodiranja rezervirane jedinicu.


##<a id="nov_changes_15"></a>Izdanje studenom 2015.

Azure Media Services sada nudi servis za isporuku licence Google Widevine u oblaku. Dodatne pojedinosti potražite [sljedeću objavu na blogu](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Pogledajte [ovaj Praktični vodič](media-services-protect-with-drm.md) i [GitHub spremište](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Imajte na umu da Widevine licence isporukom nudi Azure Media Sevices u pretpregledu. Dodatne informacije potražite u članku [ovom blogu](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

##<a id="oct_changes_15"></a>Izdanje listopad 2015.

Azure Media Services (AMS) sada se uživo centre za sljedeće podatke: Brazil Jug, Indija zapada, Južna Indija i Indija središnje. Sada možete koristiti Azure klasični Portal za [Stvaranje računa za servis Media](media-services-portal-create-account.md) i izvršiti razne zadatke koji su opisani [u nastavku](https://azure.microsoft.com/documentation/services/media-services/). Međutim, Live kodiranje nije omogućena u tih podataka. Dodatno, sve vrste kodiranja rezervirane jedinice dostupne su u tih podataka.

- Brazil Jug: Standardno i osnovni kodiranje rezervirane jedinice su dostupne samo
- Indija Zapad, Južna Indija i Indija središnje: dostupne su samo osnovni kodiranje rezervirane jedinice


##<a id="september_changes_15"></a>Izdanje rujan 2015. 

- AMS sada nudi mogućnost da biste zaštitili Video-On-Demand (VOD) te uživo strujanja tehnologijom Widevine modularan DRM. Možete koristiti sljedeće usluge partnera za isporuku da biste lakše izlaganje Widevine licenci: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/). Dodatne informacije potražite u članku [ovom blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).

    Da biste konfigurirali svoje AssetDeliveryConfiguration da biste koristili Widevine možete koristiti [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (počevši od verzije 3.5.1) ili REST API-JA.  

- AMS dodaje podršku za Apple ProRes videozapisa. Sada možete prenijeti datotekama QuickTime izvora videozapisa koji koristi Apple ProRes ili druge kodecima. Dodatne informacije potražite u članku [ovom blogu](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).

- Sada možete koristiti Media Encoder standardne učiniti podgrupama isječak i uživo izdvajanjem arhiva. Dodatne informacije potražite u članku [ovom blogu](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

- Sljedeća ažuriranja filtriranja nastali: 

    - Sada možete koristiti oblik Apple HTTP Live strujanje (HLS) s filtrom samo zvuk. To ažuriranje omogućuje vam da biste uklonili samo zvuk evidentiranje navođenjem (samo za zvuk = false) u URL-u.
    - Pri definiranju filtara za imovinu sada imate mogućnost za kombiniranje više (do 3) filtri u jednom URL-a.

    Dodatne informacije potražite u članku [ovom](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

- AMS sada podržava li – okvira u HLS v4. Podrška za li okvira optimizira operacije brzi prijelaz naprijed ili natrag. Po zadanom sve HLS v4 izlaza obuhvaćaju li okvira popisa (EXT-X-I-FRAME-STREAM-INF).
 
    Dodatne informacije potražite u članku [ovom](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

##<a id="august_changes_15"></a>Izdanje kolovoz 2015.

- Azure Media Services SDK za Java V0.8.0 izdanje i nove uzorke Zasad su dostupni. Dodatne informacije potražite u članku:

    - [Članak na blogu](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
    - [Spremište uzoraka Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- Ažuriranje Azure Media Player s podrškom za strujanje više audiozapisa. Dodatne informacije potražite u članku:
    - [Članak na blogu](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

##<a id="july_changes_15"></a>Izdanje srpanj 2015.

- Objavljivanje Općenito dostupnost Media Encoder Standard. Dodatne informacije potražite u članku [Ovaj članak na blogu](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).

    Media Encoder standardne koristi unaprijed postavljeno navedena u [ovom](http://go.microsoft.com/fwlink/?LinkId=618336) odjeljku. Imajte na umu da kada šifrira pomoću gotovu za 4k, trebali biste dobiti jedinica vrsta **Premium** rezervirana. Dodatne informacije potražite [u](media-services-scale-media-processing-overview.md)članku skaliranje kodiranja.
- Uživo u stvarnom vremenu opisa pomoću servisa Azure Media Services i reproduktora. Dodatne informacije potražite u članku [Ovaj članak na blogu](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK ažuriranja

Azure Media Services .NET SDK sada je verzija 3.4.0.0. U ovom izdanju dodan je sljedeće funkcije:  

- Implementirani podrška za arhiviranje uživo. Imajte na umu da nije moguće preuzeti sredstva koja sadrži uživo arhiva.
- Implementirani podrška za dinamičkih filtara.
- Implementirani funkcionalnost koja omogućuje korisnicima da biste zadržali spremnik za pohranu prilikom brisanja resursa.
- Popravci programskih pogrešaka povezane pokušati pravila u kanala.
- Omogućeno **tijeka rada za Media Encoder Premium**.

##<a id="june_changes_15"></a>Izdanje lipnja 2015.

###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK ažuriranja

Azure Media Services .NET SDK sada je verzija 3.3.0.0. U ovom izdanju dodan je sljedeće funkcije:  

- Podrška za specifikacija OpenId povezivanje predočavanje elektroničkih dokumenata
- Podrška za rukovanje ključeva prilikom prelaska na strani davatelja identiteta. 

Ako koristite davateljem identiteta koji se izlaže OpenID povezivanje dokument otkrivanja (kao sljedećih davatelja: Azure Active Directory, Google, Salesforce), možete uputiti servisa Azure Media Services da biste dobili potpisivanja tipke za provjeru valjanosti JWT tokena iz OpenID povezivanje otkrivanje specifikacija. 

Dodatne informacije potražite u članku [Korištenje Json Web tipki iz OpenID povezivanje specifikacija otkrivanje za rad s JWT tokena provjere autentičnosti u servisa Azure Media Services](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).


##<a id="may_changes_15"></a>Izdanje svibanj 2015.

Objavljivanje sljedeće nove značajke:

- [Pretpregled uživo kodiranje s Media Services](media-services-manage-live-encoder-enabled-channels.md)
- [Dinamični manifest](media-services-dynamic-manifest-overview.md)
- [Pretpregled procesor Azure Media Hyperlapse medijske sadržaje](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

##<a id="april_changes_15"></a>Izdanje Travanj 2015.

 ###<a name="general-media-services-updates"></a>Općenito Media Services ažuriranja

- [Objavljivanje Azure Media Player](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
- Počevši od Media Services OSTALE 2.10, kanala koja su konfigurirana za ingest protokol za RTMP, stvaraju s primarni i sekundarni ingest URL-ova. Dodatne informacije potražite u članku [kanala ingest konfiguracija](media-services-live-streaming-with-onprem-encoders.md#channel_input)
- Azure Media indeksiranje ažuriranja
- Podrška za španjolski jezik
- Novi konfiguracije xml oblik

Dodatne informacije potražite u članku [ovom blogu](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).
###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK ažuriranja

Azure Media Services .NET SDK sada je verzija 3.2.0.0.

Slijede neki od kupca nasuprotne ažuriranja:

- **Prekidanje promjena**: promijeniti **TokenRestrictionTemplate.Issuer** i **TokenRestrictionTemplate.Audience** se vrste niz.
- Novosti vezane uz stvaranje prilagođenih ponovno pravila.
- Popravci programskih pogrešaka vezanih uz prijenos i preuzimanje datoteka.
- Klase **MediaServicesCredentials** sada prihvaća primarnih i sekundarnih pristupa krajnjoj točki kontrole za provjeru autentičnosti.



##<a id="march_changes_15"></a>Izdanje ožujak 2015.

### <a name="general-media-services-updates"></a>Općenito Media Services ažuriranja

- Media Services sada omogućuje integraciju Azure CDN. Za podršku integraciju sa servisom za **StreamingEndpoint**dodan je svojstvo **CdnEnabled** .  **CdnEnabled** mogu se koristiti s REST API-ji počevši od verzije 2.9 (Dodatne informacije potražite u članku [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)).  **CdnEnabled** mogu se koristiti s .NET SDK počevši od verzije 3.1.0.2 (Dodatne informacije potražite u članku [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint(v=azure.10).aspx)).
- Objava tijeka rada za **Media Encoder Premium**. Dodatne informacije potražite u članku [Uvod u šifriranje Premium u servisa Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).
 


##<a id="february_changes_15"></a>Izdanje veljača 2015.

### <a name="general-media-services-updates"></a>Općenito Media Services ažuriranja

Media Services REST API-JA sada je verzija 2.9. Počevši od ovu verziju, možete omogućiti Azure CDN integracije s strujanje krajnje točke. Dodatne informacije potražite u članku [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

##<a id="january_changes_15"></a>Izdanje siječnju 2015.

### <a name="general-media-services-updates"></a>Općenito Media Services ažuriranja

Objava od opće dostupnih (GA) sadržaja zaštite dinamički šifriranjem. Dodatne informacije potražite u članku [servisa Azure Media Services poboljšava strujanje vrijednosnice s tehnologije Općenito dostupnost DRM](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK ažuriranja

Azure Media Services .NET SDK sada je verzija 3.1.0.1.

U ovom izdanju označen zadani Graditelj Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate kao zastarjeli. Novi Graditelj vodi TokenType kao argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


##<a id="december_changes_14"></a>Izdanje prosinac 2014.

###<a name="general-media-services-updates"></a>Općenito Media Services ažuriranja

- Neka ažuriranja i nove značajke dodani procesor Azure Media za indeksiranje. Dodatne informacije potražite u članku [Azure Media indeksiranje verziju 1.1.6.7 napomene](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
- Dodane u novu REST API-JA koji omogućuje vam da biste ažurirali kodiranje rezervirane jedinice: [EncodingReservedUnitType s OSTATKOM](http://msdn.microsoft.com/library/azure/dn859236.aspx).
- Dodane CORS podrške za servis za isporuku ključa.
- Su radi poboljšanja performansi sustava ispitivanje mogućnosti pravila za provjeru autentičnosti.
- U Kini podatkovnog centra, [URL-a za isporuku ključ](http://msdn.microsoft.com/library/azure/ef4dfeeb-48ae-4596-ab28-44d6b36d8769#get_delivery_service_url) sada je kupac (jednako kao i u drugim podataka).
- Dodane trajanje cilj automatski HLS. Pri izvođenju uživo strujanje, HLS uvijek dinamički pakirat. Prema zadanim postavkama Media Services automatski izračunava HLS segmenta pakiranje omjer (FragmentsPerSegment) na temelju interval keyframe (KeyFrameInterval) koja se naziva grupiranje od slika – GOP koji je poslao uživo kodiranje. Dodatne informacije potražite u članku [Rad s Azure Media Services Live strujanje].
 
###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK ažuriranja

- [Azure Media Services.NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) sada je verzija 3.1.0.0.
- .NET Framework 4,5 nadograditi ovisnost .net SDK.
- Dodaje novi API koji omogućuje vam da biste ažurirali kodiranja rezervirane jedinice. Dodatne informacije potražite u članku [Ažuriranje rezervirane jedinica vrsta i povećanje kodiranje RUs pomoću .NET](http://msdn.microsoft.com/library/azure/jj129582.aspx).
- Dodane JWT (JSON Web tokena) podrške za tokena provjere autentičnosti. Dodatne informacije potražite u članku [JWT tokena provjere autentičnosti u web-mjesto servisa Azure Media Services i u okvir za dinamičku šifriranje](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
- Dodane relativni pomake za BeginDate i ExpirationDate u predlošku PlayReady licence.


##<a id="november_changes_14"></a>Izdanje studenom 2014.

- Media Services sada omogućuje ingest aktivni sadržaj Smooth Streaming (FMP4) putem SSL veze. Da biste ingest putem SSL, provjerite jeste li ažurirati ingest URL HTTPS.  Dodatne informacije o live strujanje potražite u članku [Rad s Azure Media Services Live strujanje].
- Imajte na umu da trenutno koje nije moguće ingest RTMP aktivno strujanje putem SSL veze.
- Sadržaj možete strujanje i putem SSL veze. Da biste to učinili, provjerite je li strujanje URL-ovi počinju HTTPS.
- Imajte na umu da koristite samo strujanja putem SSL ako strujanje krajnjoj točki iz kojeg izlaganja sadržaj stvoren nakon 10tu. rujan 2014.. Ako strujanje URL-ovi temelje se na strujanje krajnje točke stvorene nakon rujan 10tu, URL sadrži "streaming.mediaservices.windows.net" (u novi oblik). Strujanje URL-ovi koji sadrže "origin.mediaservices.windows.net" (stari oblik) ne podržavaju SSL. Ako je URL u stari oblik, a želite omogućiti strujanje SSL, [stvorili krajnju točku strujanje](media-services-portal-manage-streaming-endpoints.md). Pomoću URL-ova stvara na temelju krajnju strujanje strujanje sadržaja putem SSL.

##<a id="october_changes_14"></a>Izdanje listopad 2014.

### <a id="new_encoder_release"></a>Media Services Encoder izdanje

Objavljivanje novo izdanje Media servisa Azure Media Encoder. S novostima Azure Media Encoder samo se naplatiti za izlaz GBs, ali u suprotnom novi encoder je značajka kompatibilan s prethodne kodiranje. Dodatne informacije [Media Services cijene pojedinosti]).

### <a id="oct_sdk"></a>Media Services .NET SDK 

Media Services SDK za .NET proširenja sada je verzija 2.0.0.3.

Media Services SDK za .NET sada je verzija 3.0.0.8.

Izvršene su sljedeće promjene:

- Restrukturiranje u klase pravila Ponovi.
- Dodavanje niz agenta korisnika http zahtjev zaglavlja.
- Dodavanje koraka za sastavljanje nuget vraćanja.
- Rješavanje testira scenarij da biste koristili x509 certifikata spremištu.
- Provjera valjanosti postavke prilikom ažuriranja kanala, a zatim Završi strujanje.
 

### <a name="new-github-repository-to-host-media-services-samples"></a>Novi GitHub spremište na glavno računalo Media Services uzorka

Uzorci nalaze se u [servisa Azure Media Services uzoraka GitHub spremište](https://github.com/Azure/Azure-Media-Services-Samples).


##<a id="september_changes_14"></a>Izdanje rujan 2014.

OSTALE Media Services metapodataka sada je verzija 2.7. Dodatne informacije o najnovijim ažuriranjima OSTALE potražite u članku [Azure Media Services REST API Reference].

Media Services SDK za .NET sada je verzija 3.0.0.7
 
### <a id="sept_14_breaking_changes"></a>Najnovije promjene

* **Polazište** preimenovana je u [StreamingEndpoint].
* Promjena u zadano ponašanje pri korištenju **Azure klasični Portal** za kodiranje, a zatim objavite MP4 datoteke.

Prethodno, kada pomoću portala za klasični Azure za objavljivanje u jednoj datoteci MP4 video sredstvo SAS URL-a stvorit će se (URL-ovi SAS omogućuju da biste preuzeli videozapis iz spremišta blobova). Trenutno pomoću portala za klasični Azure kodiranje, a zatim objavite jedne datoteke videozapisa resursa za MP4 generirani URL upućuje na programa servisa Azure Media Services strujanje krajnjoj točki.  Ta promjena ne utječe na MP4 videozapise koje se izravno prenijeli Media Services i objavljuju bez se kodirati putem servisa Azure Media Services.

Trenutno imate sljedeće dvije mogućnosti za rješavanje problema.

* Omogućite strujanje jedinice i koristite dinamičke pakiranja za strujanje resursa .mp4 kao izglađenim strujanje prezentaciju.

* Stvaranje SAS URL-a preuzimanje (ili progresivno reproducirati) na .mp4. Dodatne informacije o stvaranju SAS locator potražite u članku [Izlaganja sadržaja].


### <a id="sept_14_GA_changes"></a>Nove značajke/scenarija koji su dio GA izdanje

* **Procesor medijskih sadržaja za indeksiranje**. Dodatne informacije potražite u članku [Indeksiranje medijskih datoteka s Azure Media indeksiranje].

* Entitet [StreamingEndpoint] sada omogućuje vam da biste dodali prilagođenu domenu (glavni) imena.

    Za prilagođeni naziv domene koja će se koristiti kao naziv strujanje krajnje točke Media Services, morate dodati nazivi prilagođene glavnog računala na vašem strujanje krajnje točke. Koristite Media Services REST API-ji ili .NET SDK za dodavanje naziva prilagođene glavnog računala.
    
    Primjena Imajte na umu sljedeće:
    
    * Vlasništvo nad naziv prilagođene domene, morate imati.
    
    * Vlasništvo nad naziv domene mora biti valjanost servisa Azure Media Services. Da biste provjerili valjanost domene, stvorite CName koja mapira <MediaServicesAccountId>.<parent domain> Da biste verifydns. < mediaservices dns-zona >. 
    
    * Morate stvoriti drugi CName koje mape u prilagođeni naziv glavnog računala (na primjer, sports.contoso.com) na naziv glavnog računala na Media Services StreamingEndpont (na primjer, amstest.streaming.mediaservices.windows.net).


    Dodatne informacije potražite u članku svojstvo **CustomHostNames** u temi [StreamingEndpoint] .

### <a id="sept_14_preview_changes"></a>Nove značajke/scenarija koji su dio javno pretpregled izdanja

* Strujanje pretpregled uživo. Dodatne informacije potražite u članku [Rad s Azure Media Services Live strujanje].

* Servis za isporuku ključa. Dodatne informacije potražite u članku [Korištenje AES 128 dinamički šifriranja i servis za isporuku ključ].

* Dinamični AES šifriranje. Dodatne informacije potražite u članku [Korištenje AES 128 dinamički šifriranja i servis za isporuku ključ].

* Servis za isporuku PlayReady licence. Dodatne informacije potražite u članku [Korištenje PlayReady dinamički šifriranje i servis za isporuku licence].

* Dinamični PlayReady šifriranje. Dodatne informacije potražite u članku [Korištenje PlayReady dinamički šifriranje i servis za isporuku licence].

* Media Services predložak PlayReady licence. Dodatne informacije potražite u članku [Pregled predloška za Media Services PlayReady licence].

* Strujanje prostora za pohranu šifrirane resursi. Dodatne informacije potražite u članku [Strujanje prostora za pohranu šifrirane sadržaja].

##<a id="august_changes_14"></a>Izdanje kolovoz 2014.

Kada linijski sredstvo sredstvo izlaz je sastavio po dovršetku kodiranja posla. Dok se ne to je izdanje Azure Media Services Encoder proizvodi metapodatke o imovini izlaz. Kodiranje počevši s ovom izdanju i daje metapodataka o imovini unosa. Dodatne informacije potražite u temama [Metapodataka ulazni] i [Izlazni metapodataka] .


##<a id="july_changes_14"></a>Izdanje srpanj 2014.

Sljedeće popravci programskih pogrešaka napravljeni program za pakiranje Azure Media Services i šifriranje:

* Samo zvuk se reproducira natrag kada transmuxing uživo arhiva resursa za HTTP Live strujanja – to je riješen, a sada reprodukcije audiozapisa i videozapisa.

* Kada pakiranje imovine HTTP Live strujanje i AES 128-bitno omotnica šifriranje zapakirani strujanja neće reproducirati na uređaje sa sustavom Android – ovu pogrešku fiksiran i zapakirani toka reprodukcije uređajima sa sustavom Android koji podržavaju HTTP Live strujanje.

##<a id="may_changes_14"></a>Izdanje iz svibnja 2014.

### <a id="may_14_changes"></a>Općenito Media Services ažuriranja

Sada možete koristiti [Dinamičke pakiranja] za v3 strujanje HTTP Live strujanje (HLS). Za strujanje HLS v3, dodajte sljedeći oblik locator put polazište: * .ism/manifest(format=m3u8-aapl-v3). Dodatne informacije potražite u članku [Blog Nadimak Drouin].

Dinamični pakiranje sada i podržava izlaganja HLS (v3 i v4) šifrirane i zaštićene PlayReady na temelju Smooth Streaming statički šifrirane i zaštićene PlayReady. Informacije o tome kako šifrirati Smooth Streaming s PlayReady potražite u članku [Protecting izglađenim strujanje PlayReady].

### <a name="may_14_donnet_changes"></a>Media Services .NET SDK ažuriranja

Uključene su sljedeća poboljšanja u izdanju Media Services .NET SDK 3.0.0.5:

* Bolje brzina i resilience za prijenos i preuzimanje medijskih sadržaja.

* Poboljšanja u pokušaj logiku i prolaznim iznimku rukovanje: 

    * Otkrivanje i pokušajte ponovno logike tranzitne pogreške poboljšani su za iznimke koje uzrokuju upita, računanje, prijenos ili preuzimanje datoteka. 
    
    * Kada prijeđete iznimke Web (primjerice, tijekom ACS tokena zahtjeva za razmjenu), primijetit ćete da kobne pogreške su neuspješnih brže sada.

Dodatne informacije potražite u odjeljku [Ponovno pokušajte logike u Media Services SDK za .NET].

##<a id="april_changes_14"></a>Izdanje Encoder Travanj 2014.

### <a name="april_14_enocer_changes"></a>Media Services Encoder ažuriranja

* Podrška za ingesting AVI datoteka stvorenih pomoću uređivača nelinearni trava udubljenje EDIUS gdje je videozapis svijetlim komprimiran pomoću kodeka trava udubljenje HQ/HQX. Dodatne informacije potražite u članku [Trava udubljenje najavljuje EDIUS 7 strujanje putem u Oblaku].

* Podrška za određivanje konvencije imenovanja datoteka koje je stvorio Media Encoder. Dodatne informacije potražite u članku [Kontrola medijskog sadržaja servisa Encoder izlaz nazivi datoteka].

* Podrška za prekriva videozapis i/ili zvuka. Dodatne informacije potražite u članku [Stvaranje slojeva].

* Podrška za Šivanje zajedno više video segmenata. Dodatne informacije potražite u članku [Šivanje segmenata videozapis].

* Ispraviti pogreške vezane uz prekodiranje MP4s koju audiodatoteku kodiran s MPEG-1 Audio layer 3 (ili MP3).


##<a id="jan_feb_changes_14"></a>Izdanja siječanj/veljača 2014.

### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.1, 3.0.0.2 i 3.0.0.3

Promjene u 3.0.0.1 i 3.0.0.2 obuhvaćaju sljedeće:

* FIXED problema vezanih uz korištenje LINQ upiti s OrderBy izvješća.

* Podjela test rješenja u [GitHub] u utemeljen na jedinica testira i konkretni scenariji testira.

Dodatne informacije o promjenama potražite u članku: [Azure Media Services .NET SDK 3.0.0.1 i 3.0.0.2 izdanja].

Sljedeće su izvršene u 3.0.0.3:

* Nadogradnja Azure prostora za pohranu ovisnosti da biste koristili verziju 3.0.3.0. 

* Fiksni kompatibilnost s prijašnjim verzijama problem 3.0. *.* izdanja. 


##<a id="december_changes_13"></a>Izdanje prosinac 2013

### <a name="dec_13_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.0

>[AZURE.NOTE] 3.0.x.x izdanja nisu kompatibilna s 2.4.x.x izdanja.

Najnoviju verziju Media Services SDK sada je 3.0.0.0. Možete preuzeti najnoviju paketa iz Nuget ili zatražite u bitova od [GitHub].

Počevši od Media Services SDK verziju 3.0.0.0, pa je možete koristiti tokeni [Azure Active Directory pristup kontrola servisa (ACS)] . Dodatne informacije potražite u odjeljku "Ponovnog korištenja pristup kontrola servisa tokeni" u članku [Povezivanje sa Media Services s Media Services SDK za .NET] .

### <a name="dec_13_donnet_ext_changes"></a>Azure Media Services .NET SDK proširenja 2.0.0.0

Azure Media Services .NET SDK proširenja je skup Pomoćnik za funkcije koje će Pojednostavnite kodu i lakše razviti pomoću servisa Azure Media Services i nastavak metode. Najnovije bitova možete pristupiti iz [Azure Media Services .NET SDK proširenja].

##<a id="november_changes_13"></a>Izdanje studenom 2013

### <a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK promjene

Počevši s tom verzijom, Media Services SDK za .NET rukuje tranzitne kvara pogreške koje se može pojaviti pri upućivanju poziva sloju Media Services REST API-JA.

##<a id="august_changes_13"></a>Izdanje kolovoz 2013

### <a name="aug_13_powershell_changes"></a>Media Services PowerShell cmdleti obuhvaćeno Azure Sdk Alati

Sljedeće Cmdlete Media Services PowerShell sada obuhvaćen [azure sdk Alati].

* Get-AzureMediaServices 

    Na primjer, `Get-AzureMediaServicesAccount`.

* Novi AzureMediaServicesAccount 

    Na primjer, `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.

* Novi AzureMediaServicesKey 

    Na primjer, `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.

* Uklanjanje AzureMediaServicesAccount 

    Na primjer, `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

##<a id="june_changes_13"></a>Izdanje lipnja 2013.

### <a name="june_13_general_changes"></a>Azure Media Services promjene

Promjene koji se spominju u ovom odjeljku su ažuriranja uključena u izdanjima Media Services lipnja 2013.

* Mogućnost da biste se povezali više prostora za pohranu računa na račun servisa medijske sadržaje. 

    StorageAccount
    
    Asset.StorageAccountName i Asset.StorageAccount

* Mogućnost da biste ažurirali Job.Priority. 

* Obavijest o povezane entiteti i svojstva: 

    JobNotificationSubscription
    
    NotificationEndPoint
    
    Zadatak

* Asset.Uri 

* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Azure Media Services .NET SDK promjene

Uključene su sljedeće promjene u lipnja 2013 Media Services SDK izdanja. Najnovije Media Services SDK dostupan je na GitHub.

* Počevši od verzije 2.3.0.0, podržava Media Services SDK povezivanje više prostora za pohranu računa na račun servisa Media Services. Sljedeće API-ji to podržava:
    
    Vrsta IStorageAccount.
    
    Svojstvo Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts.
    
    Svojstvo StorageAccount.
    
    Svojstvo StorageAccountName.
    
    Dodatne informacije potražite u članku [Upravljanje imovine Media Services u više računa za pohranu].

* Obavijest o vezi API-ji. Počevši od verzije 2.2.0.0 mogućnost da biste preslušali Azure red za pohranu obavijesti. Dodatne informacije potražite u člancima, [Rukovanje Media Services posao obavijesti o].
    
    Svojstvo Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions.
    
    Vrsta Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint.
    
    Vrsta Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription.
    
    Vrsta Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection.
    
    Vrsta Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType.
    
    Vrsta Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState.

* Zavisnost o Azure prostora za pohranu klijenta SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).

* Zavisnost o OData 5,5 (Microsoft.Data.OData.dll).


##<a id="december_changes_12"></a>Izdanje prosinac 2012.

### <a name="dec_12_dotnet_changes"></a>Azure Media Services .NET SDK promjene

* IntelliSense: Dodaje nedostaju Intellisense dokumentaciju za mnoge vrste.

* Microsoft.Practices.TransientFaultHandling.Core: Riješiti problem gdje SDK-a i dalje imali ovisnost staru verziju ovog skupa. SDK sada se odnosi na verziju 5.1.1209.1 ovaj skup.

Rješenja za probleme pronađene u studenom 2012 SDK:

* IAsset.Locators.Count: Ovaj broj sada ispravno prijavljuje na novo sučelje IAsset nakon brisanja svih locators.

* IAssetFile.ContentFileSize: Tu vrijednost je sada ispravno postavio nakon za prijenos IAssetFile.Upload(filepath).

* IAssetFile.ContentFileSize: Ovo svojstvo možete postaviti sada kada stvarate datoteku resursa. To je prethodno samo za čitanje.

* IAssetFile.Upload(filepath): Riješiti problem gdje ovu metodu sinkrono prijenos je prijavi sljedeća pogreška kada prijenos više datoteka za sredstvo. Pogreška "nije uspjela provjera zahtjev poslužitelj. Provjerite je li vrijednost zaglavlja autorizacije oblikovan pravilno uključujući potpis."

* IAssetFile.UploadAsync: Riješiti problem kojima najviše 5 datoteke nije moguće prenijeti istodobno.

* IAssetFile.UploadProgressChanged: Ovaj događaj je sada nudi SDK-a.

* IAssetFile.DownloadAsync (niz znakova, BlobTransferClient, ILocator, CancellationToken): Ova metoda preopterećenja sada navedene.

* IAssetFile.DownloadAsync: Riješiti problem gdje najviše 5 datoteke nije moguće preuzeti istodobno.

* IAssetFile.Delete(): Dugotrajne problem gdje pozivanja Izbriši mogu vratiti iznimku ako nijedna datoteka je prenijeti u IAssetFile.

* Poslovi: Dugotrajne problem gdje Ulančavanje na "MP4 izglađenim strujanja zadatak" s "PlayReady zaštitu zadatka" pomoću predloška za posao želite stvoriti sve zadatke uopće.

* EncryptionUtils.GetCertificateFromStore(): Ta metoda više throws null referenca iznimku zbog pogreške pronalaženje certifikat na temelju problema s konfiguracijom certifikata.

##<a id="november_changes_12"></a>Izdanje studenom 2012.

Promjene koji se spominju u ovom odjeljku su ažuriranja obuhvaćeno 2012 studenom (verzija 2.0.0.0) SDK. Te promjene možda potreban kod sastavljene za pretpregled lipnja 2012 SDK pustite da biste promijenili ili potrebno ponovno napisati.

* Resursi
    
    IAsset.Create(assetName) je funkcija stvaranja samo resursa. IAsset.Create više ne prenosi datoteke kao dio pozivanje metode. Korištenje IAssetFile za prijenos.
    
    Način IAsset.Publish i vrijednost numeriranja AssetState.Publish uklonjeni su iz Services SDK. Kod koji se zasniva na ta vrijednost mora biti ponovno pisane.

* FileInfo

    Klase uklonjena je i zamjenjuje IAssetFile.

    IAssetFiles

    IAssetFile zamjenjuje FileInfo i ponaša drugačije. Da biste koristili, stvoriti instancu objekta IAssetFiles, nakon čega slijedi prijenos datoteke na jedan od sljedećih načina korištenja Media Services SDK ili prostora za pohranu SDK Azure. Možete koristiti sljedeće overloads IAssetFile.Upload:

    * IAssetFile.Upload(filePath): Sinkronizirano način koji blokira niti se preporučuje samo kada se prenosi u jednoj datoteci.
    
    * IAssetFile.UploadAsync (filePath, blobTransferClient, locator, cancellationToken): asinkronog način. Ovo je mehanizam Preferirani prijenos. 

    Poznatih problema: korištenje na cancellationToken uistinu prekinuli prijenos; Međutim, otkazivanje stanje zadataka može biti nešto od broj države. Morate pravilno Uhvatite i obradu iznimke.

* Locators
    
    Uklonjene su verzije specifične za izvor. Kontekst SAS specifične. Locators.CreateSasLocator (resursa, accessPolicy) bit će označene zastarjeli ili ukloniti tako da ZŽ. U odjeljku Locators u odjeljku nove funkcije ažurirane ponašanje.


##<a id="june_changes_12"></a>Lipnja 2012 pretpregled izdanja

Sljedeće funkcije je novo u studenom izdanju SDK.

* Brisanje entiteti

    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey objekata sada se brišu na razini objekta, odnosno IObject.Delete(), umjesto potrebno Izbriši u zbirci koja je cloudMediaContext.ObjCollection.Delete(objInstance).

* Locators

    Locators sada mora biti stvoren pomoću metode CreateLocator i koristiti redni broj vrijednosti LocatorType.SAS ili LocatorType.OnDemandOrigin kao argument za određenu vrstu locator koju želite stvoriti.

    Nova svojstva su dodani u Locators da biste olakšali da biste dobili upotrebljivosti ji za sadržaj. U ovom promijeni dizajn od Locators je namijenjena omogućuju veću fleksibilnost za buduće proširenja drugih proizvođača i povećati pristupačnost –-korištenja za medijske sadržaje klijentske aplikacije.

* Podrška za asinkronog način

    Podrška za asinkronog dodana sve načine.


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services na MSDN]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API Reference]: http://msdn.microsoft.com/library/azure/hh973617.aspx 
[Media Services informacije o cijenama]: http://azure.microsoft.com/pricing/details/media-services/
[Unos metapodataka]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Izlaz metapodataka]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Isporuke sadržaja]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Indeksiranje medijskih datoteka s Azure Media indeksiranje]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Rad sa servisa Azure Media Services Live strujanje]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Korištenje AES 128 dinamički šifriranja i servis za isporuku ključa]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Korištenje PlayReady dinamičkih šifriranja i servis za isporuku licence]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services pregled predloška PlayReady licence]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Strujanje prostora za pohranu šifrirane sadržaja]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure Classic Portal]: https://manage.windowsazure.com
[Dinamični pakiranje]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nadimak Drouin bloga]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Zaštita izglađenim strujanje PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Ponovite logike u Media Services SDK za .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Trava udubljenje najavljuje EDIUS 7 strujanje putem oblaka]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Upravljanje Media servisa nazivi Encoder Izlazna datoteka]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Stvaranje prekrivanja]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Šivanje Video segmenata]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 i 3.0.0.2 izdanja]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Servisa Azure Active Directory kontrole pristupa (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[Povezivanje s Media Services s Media Services SDK za .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services .NET SDK proširenja]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure sdk Alati]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Upravljanje Media Services imovine u više prostora za pohranu računa]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Rukovanje Media Services posao obavijesti]: http://msdn.microsoft.com/library/azure/dn261241.aspx
 
