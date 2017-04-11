<properties
    pageTitle="Isporuke sadržaja klijentima | Microsoft Azure"
    description="Ova tema sadrži pregled sustava izlaganja sadržaja pomoću servisa Azure Media Services."
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
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="deliver-content-to-customers"></a>Izlaganje sadržaja klijentima
Kada održavate strujanje ili video-on-demand sadržaj klijentima za isporuku videozapise visoke kvalitete za različite uređaje pod uvjetima drugog mrežnog je vaš cilj.

Da biste postigli cilja, možete učiniti sljedeće:

- Kodiranje vaše strujanje za strujanje videozapisa brzina na prijenosa višestruki (prilagodljivo brzina prijenosa). To će voditi brigu o kvalitete i mrežne uvjeta.
- Pomoću servisa Microsoft Azure Media Services [dinamički pakiranje](media-services-dynamic-packaging-overview.md) dinamički ponovno pakiranje vaše strujanje u različitim protokola. To će voditi brigu o strujanje na različitim uređajima. Media Services podržava isporuku sljedeće prilagodljivo brzina prijenosa strujanje tehnologije: HTTP Live strujanje (HLS), Smooth Streaming, MPEG-CRTICOM i HDS (za Primetime pristup Adobe licensees samo).

U ovom se članku daje pregled koncepta važne isporuku sadržaja.

Da biste provjerili poznatim problemima potražite u članku [Poznati problemi](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dinamični pakiranje

S dinamički pakiranje koja omogućuje Media Services izlažete prilagodljivo brzina prijenosa MP4 ili Smooth Streaming kodirani sadržaj u strujanje oblici koje podržava Media Services (MPEG-CRTICE, HLS, Smooth Streaming, HDS) bez potrebe za ponovno pakiranje u te strujanje oblici. Preporučujemo da izlaganja sadržaja pomoću dinamičke pakiranju.

Da biste iskoristili dinamički pakiranje, morate učinite sljedeće:

- Kodiranje datoteku mezzanine (izvor) u skupu prilagodljivo brzina prijenosa MP4 datoteke ili datoteke Smooth Streaming prilagodljivo brzina prijenosa.
- Pronađite najmanje jednu jedinicu strujanje na zahtjev za strujanje krajnju točku iz koje namjeravate održati sadržaj. Dodatne informacije potražite u članku [upute za promjenu veličine na zahtjev strujanje rezervirane jedinice](media-services-portal-manage-streaming-endpoints.md).

S dinamički pakiranje, pohraniti i platiti za datoteke u obliku jedan prostora za pohranu. Media Services će Sastavljanje i posluživanje prikladan odgovor koji se temelji na vašim zahtjevima.

Osim osiguravanja pristupa mogućnosti dinamički pakiranje, na zahtjev strujanje rezervirane jedinice vam ponuditi namjenski izlazne kapacitet koji je moguće kupiti u koracima od 200 MB/s. Prema zadanim postavkama strujanje na zahtjev je konfiguriran u modelu zajednički instancu za koji poslužitelj resursa (na primjer, računalnim ili izlazne kapacitet) se zajednički koriste s drugim korisnicima. Kupnjom osvježavati strujanje rezervirane jedinice možete poboljšati propusnost strujanje sustava na zahtjev.

Dodatne informacije potražite u članku [dinamički pakiranju](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Filtri i dinamični manifesti

Možete definirati filtre za imovinu s Media Services. Ovi filtri su poslužiteljska pravila kojih korisnici primjerice reproducirati određeni dio videozapisa ili prikaz podskupa audio i video vizualizacija koji uređaj klijenta možete rukovati (umjesto sve vizualizacije koje su vezane uz imovine). Postići filtriranja kroz *dinamički manifesti* stvorit će se kada klijent zahtjeve za strujanje videozapisa koji se temelje na navedenim filtrima.

Dodatne informacije potražite u članku [Filtri i dinamični manifesti](media-services-dynamic-manifest-overview.md).

## <a name="locators"></a>Locators

Da bi korisnički URL-om na koje je moguće koristiti za strujanje ili preuzimanje sadržaja, najprije morate objaviti svoje resursa stvaranjem na locator. Na locator nudi ulaza da biste pristupili datotekama koje se nalaze u sredstava. Media Services podržava dvije vrste locators:

- OnDemandOrigin locators. Ove se koriste za strujanje medijskih sadržaja (na primjer, MPEG-CRTICE, HLS ili Smooth Streaming) ili progresivno preuzimanje datoteka.
-  Zajednički pristup potpis (SAS) URL locators. Koriste se za preuzimanje medijskih datoteka s vašim lokalnim računalom.

*Pravilnik o pristupu* koristi se za definiranje dozvola (primjerice, čitanje, pisanje i popis) i trajanje za koji klijent ima pristup određenom resursa. Imajte na umu da popis dozvola (AccessPermissions.List) ne treba koristiti pri stvaranju programa locator OrDemandOrigin.

Locators imati datuma isteka. Portal za Azure locators postavlja datum isteka 100 godina u budućnosti.

>[AZURE.NOTE] Ako ste koristili Azure portal da biste stvorili locators prije ožujak 2015, te locators postavljeni istječe nakon dvije godine.

Da biste ažurirali datum isteka na na locator, koristite [OSTALE](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) ili [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-ji. Imajte na umu da ažurirate datum isteka SAS locator URL promijeni.

Locators su namijenjene upravljanju kontrola pristupa po korisniku. Prava pristupa za različite pojedinačnim korisnicima možete dati pomoću rješenja za upravljanje digitalnim pravima (DRM). Dodatne informacije potražite u članku [Zaštita medijske sadržaje](http://msdn.microsoft.com/library/azure/dn282272.aspx).

Kada stvorite na locator, mogu postojati odgodu 30 sekundi zbog potreban prostor za pohranu i prijenos procesi u odjeljku pohrana Azure.


## <a name="adaptive-streaming"></a>Prilagodljivo strujanje

Prilagodljivo brzina prijenosa tehnologije omogućuju reproduktora videosadržaja aplikacije da biste odredili uvjeti na mreži, a zatim odaberite nekoliko bitrates. Kada se smanjuje mrežnu komunikaciju, klijent možete odabrati donjem brzina prijenosa da bi se reprodukcija možete nastaviti s slabije kvalitete videozapisa. Kao što je mrežni uvjeti poboljšali, klijent možete se prebaciti na višu brzina prijenosa s poboljšane kvalitetu videozapisa. Azure Media Services podržava sljedeće tehnologije prilagodljivo brzina prijenosa: HTTP Live strujanje (HLS), Smooth Streaming, MPEG-CRTICOM i HDS.

Da biste korisnicima omogućili strujanje URL-ovi, najprije morate stvoriti programa locator OnDemandOrigin. Stvaranje Lokator omogućuje vam osnovni put resursa koji sadrži sadržaj koji želite strujanje. Međutim, da biste mogli strujanje sadržaja, ćete morati izmijeniti ovaj put. Sastavljanje cijeli URL strujanje manifesta datoteku, morate concatenate na locator put vrijednost i manifest (filename.ism) naziv datoteke. Zatim **Dodaj/manifesta** i odgovarajući oblik (Ako je potrebno) locator put.

>[AZURE.NOTE]Sadržaj možete strujanje i putem SSL veze. Da biste to učinili, provjerite je li strujanje URL-ovi počinju HTTPS.

SSL možete strujanje samo ako je strujanje krajnjoj točki iz kojeg izlaganja sadržaj stvoren nakon 10tu. rujan 2014.. Sadrži li strujanje URL-ovi temelje se na strujanje krajnje točke stvorene nakon rujan 10tu 2014., URL "streaming.mediaservices.windows.net." Strujanje URL-ovi koji sadrže "origin.mediaservices.windows.net" (stari oblik) ne podržavaju SSL. Ako je URL u stari oblik, a želite omogućiti strujanje SSL, stvorili krajnju točku strujanje. Pomoću URL-ovi koji se temelji na krajnju strujanje strujanje sadržaja putem SSL.


## <a name="streaming-url-formats"></a>Oblici strujanje URL-a

### <a name="mpeg-dash-format"></a>MPEG-CRTICOM oblik

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/manifest(format=mpd-time-CSF)



### <a name="apple-http-live-streaming-hls-v4-format"></a>Oblikovanje V4 Apple HTTP Live strujanje (HLS)

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/manifest(format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Oblik V3 Apple HTTP Live strujanje (HLS)

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/manifest(format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Oblikovanje Apple HTTP Live strujanje (HLS) s filtrom samo za zvuk

Prati samo zvuk po zadanom su uključeni u na HLS manifest. Ovo je obavezan za Apple Store certifikata za mobilne mreže. U ovom slučaju Ako klijent nema dovoljno propusnosti ili je povezan putem veze 2G, reprodukcija prelazi samo zvuk. To onemogućuje sadržaja strujanje bez međuspremnik, ali postoji nijedan videozapis. U nekim slučajevima player međuspremnik možda Preferirani putem samo zvuk. Ako želite da biste uklonili praćenje samo zvuk, dodajte **samo zvuk = false** URL.

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/manifest(format=m3u8-aapl-v3,audio-only=FALSE)

Dodatne informacije potražite u članku [podršku za dinamičku manifesta Sastavljanje i HLS izlaz dodatne značajke](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).


### <a name="smooth-streaming-format"></a>Oblik Smooth strujeće

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Primjer:

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/manifest

### <a id="fmp4_v20"></a>Smooth Streaming 2.0 manifest (naslijeđene manifest)

Prema zadanim postavkama, Smooth Streaming manifesta oblikovanje sadrži ponavljanje oznaka (r oznaka). No neke uređaje podržavaju oznaku r. Klijenti za sljedeće uređaje možete koristiti oblik koji onemogućuje oznaku r:

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

### <a name="hds-for-adobe-primetimeaccess-licensees-only"></a>HDS (za PrimeTime pristup Adobe licensees samo)

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f)

## <a name="progressive-download"></a>Progresivno preuzimanje

S progresivno preuzimanje možete pokrenuti reprodukcijom medijskih sadržaja prije preuzeta cijelu datoteku. Progresivno ne može preuzeti (.ism * ismv, isma, ismt ili ismc) datoteke.

Da biste progresivno preuzimanje sadržaja, slijedite OnDemandOrigin vrstu locator. Sljedeći primjer prikazuje URL-a koji se temelji na vrstu OnDemandOrigin locator:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Morate dešifrirati neku prostora za pohranu šifrirane imovinu koju želite strujanje iz servisa polazište progresivno preuzimanje.

## <a name="download"></a>Preuzimanje

Da biste preuzeli sadržaj klijentskog uređaja, morate stvoriti SAS Locator. SAS locator omogućuje vam pristup spremniku Azure prostora za pohranu u kojoj se nalazi datoteka. Da biste sastavili preuzimanje URL-a, morate uložiti naziv datoteke između glavnog računala i SAS potpis.

Sljedeći primjer prikazuje URL-a koji se temelji na SAS locator:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

Primjena Imajte na umu sljedeće:

- Morate dešifrirati neku prostora za pohranu šifrirane imovinu koju želite strujanje iz servisa polazište progresivno preuzimanje.
- Preuzimanje koje još nije završio u roku od 12 sati neće uspjeti.

## <a name="streaming-endpoints"></a>Strujanje krajnje točke

Strujanje krajnjoj točki predstavlja servis za strujanje koja možete ispuniti sadržajem izravno klijentska aplikacija reproduktora ili s mreže za isporuku sadržaja (CDN) za daljnje distribuciju. Izlazni strujanje iz strujanje krajnjoj točki može biti aktivno strujanje ili video-on-demand resursa na vašem računu Media Services. Možete odrediti i kapacitet servis za strujanje krajnja točka za rukovanje sve veći propusnosti potrebama prilagodbom strujanje rezervirane jedinice. Za aplikacije u okruženju proizvodnje treba dodijeliti najmanje jednu rezervirane jedinicu. Dodatne informacije potražite u članku [upute za promjenu veličine servis za medijske sadržaje](media-services-portal-manage-streaming-endpoints.md).

## <a name="known-issues"></a>Poznati problemi

### <a name="changes-to-smooth-streaming-manifest-version"></a>Promjene Smooth Streaming manifesta verzija

Prije izdanja servisa srpanj 2016 – kada imovine osnovu Media Encoder standardne Media Encoder Premium tijek rada ili starijih Media Encoder Azure su strujanjem pomoću dinamičke pakiranje – Smooth Streaming manifest vraća bi skladu s verzije 2.0. U verzije 2.0 trajanja fragment pomoću oznaka so-called ponavljanje ("r"). Ako, na primjer:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

Servis izdanje 2016 srpanj generirani manifest Smooth Streaming udovoljava verziju 2.2, s dijelom trajanje korištenje ponavljanje oznaka. Ako, na primjer:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Neke naslijeđene Smooth Streaming klijenti možda ne podržava ponavljanje oznaka i neće se učitati manifest. Da biste smanjili taj problem, možete koristiti parametar naslijeđenog manifesta oblika **(oblik = fmp4 v20)** ili ažurirati klijent na najnoviju verziju, koji podržava Ponovi oznake. Dodatne informacije potražite u članku [Smooth Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Povezane teme

[Ažuriranje locators Media Services nakon vodoravnim ključeva za pohranu](media-services-roll-storage-access-keys.md)
