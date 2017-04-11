<properties 
    pageTitle="Filtri i dinamični manifesti | Microsoft Azure" 
    description="U ovoj se temi opisuje kako stvoriti filtri tako da ih klijent možete koristiti za strujanje određene dijelove strujanje. Media Services stvara dinamički manifesti arhivirati ovaj selektivno strujanje." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="cenkdin;juliako"/>

# <a name="filters-and-dynamic-manifests"></a>Filtri i dinamični manifesti

Počevši od 2.11 izdanje, Media Services omogućuje definiranje filtara za imovinu. Ovi filtri su pravila na strani poslužitelja omogućit će klijentima da biste odabrali radnje kao što su: reprodukcija samo dio videozapisa (umjesto reproducira cijeli videozapis), ili samo podskup audio i video vizualizacija koji uređaj klijenta možete rukovati (umjesto sve vizualizacije koje su vezane uz imovine). Filtriranja imovine arhiviraju kroz **Dinamički manifesta**s stvorene nakon klijentovim zahtjeva za strujanje videozapisa koji se temelji na navedeni filter(s).

Ovaj članak govori o uobičajeni scenariji u kojima pomoću filtara biti vrlo korisni klijentima i veze na teme kojima se ilustrira programski stvaranju filtara (trenutno, možete stvoriti filtara s REST API-ji samo).

##<a name="overview"></a>Pregled

Tijekom izlaganja vaš sadržaj kupcima (strujanje uživo događaje ili video-on-demand) izlaganje visoke kvalitete videozapisa s različitim uređajima pod uvjetima drugog mrežnog je vaš cilj. Da biste postigli taj cilja, učinite sljedeće:

- kodiranje vaše strujanje za strujanje videozapisa brzina na prijenosa višestruki ([prilagodljivo brzina prijenosa](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) (to će voditi brigu o kvalitete i mrežni uvjeti) i 
- pomoću Media Services [Dinamički pakiranja](media-services-dynamic-packaging-overview.md) za dinamično ponovno pakiranje vaše strujanje u različitim protokola (to će voditi brigu o strujanje na različitim uređajima). Media Services podržava isporuku sljedeće prilagodljivo brzina prijenosa strujanje tehnologije: HTTP Live strujanje (HLS), Smooth Streaming, MPEG CRTICE i HDS (za PrimeTime pristup Adobe licensees samo). 

###<a name="manifest-files"></a>Manifest datoteka 

Kada kodiranje sredstvo za strujanje prilagodljivo brzina prijenosa, stvara se datoteka za **manifesta** (reproduciranje) (datoteka je temeljenom na tekstu ili koji se temelji na XML). Datoteka **manifesta** obuhvaća strujanje metapodataka kao što su: praćenje vrsta (zvuk, videozapis ili tekst), pratite naziv, vrijeme početka i završetka, brzina prijenosa (podvrste), jezika za praćenje prozora prezentacije (klizača prozor fiksnog trajanja), video kodek (FourCC). Također se upućuje reproduktora dohvatiti sljedeće dio unosom informacije o na sljedeći izvođenje video fragmentirane dostupne i njihovo mjesto. Fragmentirane (ili segmenata) su na stvarni "blokova" video sadržaj.


Evo primjera manifesta datoteke: 

    
    <?xml version="1.0" encoding="UTF-8"?>  
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">
    
    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />
    
    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    </SmoothStreamingMedia>
    
###<a name="dynamic-manifests"></a>Dinamični manifesti

Postoje [scenariji](media-services-dynamic-manifest-overview.md#scenarios) kada vaš klijent treba veću fleksibilnost nego što je opisano u datoteci manifesta zadani resursa. Ako, na primjer:

- Određeni uređaj: izlaganje samo and\or navedeni vizualizacija određenim jezični zapisi koje su podržane u uređaj koji je korišten reprodukcije sadržaja ("vizualizacija filtriranje"). 
- Smanjite manifest da bi se prikazala podređenu isječka uživo događaja ("podređenu isječak filtriranje").
- Obrezivanje na početak videozapisa ("obrezivanje videozapisa").
- Prilagodite prozor prezentacije (DVR) omogućili ograničeni duljinu prozoru DVR reproduktora ("Podešavanje prezentacije u prozoru").
 
Da biste postigli Ova fleksibilnost Media Services nudi **Dinamički manifesti** utemeljenog na unaprijed definiranih [filtara](media-services-dynamic-manifest-overview.md#filters).  Kada definirate filtre, klijentima ih mogli upotrijebiti za strujanje određene vizualizacije ili podmjesta isječke videozapisa. Oni će odrediti filter(s) u strujanje URL. Nije moguće primijenjeni filtri prilagodljivo brzina prijenosa protokoli podržava [Dinamičke pakiranja](media-services-dynamic-packaging-overview.md)za strujanje: HLS, MPEG-CRTICE, Smooth Streaming i HDS. Ako, na primjer:

MPEG CRTICE URL-a pomoću filtra

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Smooth Streaming URL-a pomoću filtra

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Dodatne informacije o izlaganje sadržaja i izraditi strujanje URL-ovi potražite u članku [Pregled sadržaja isporuka](media-services-deliver-content-overview.md).


>[AZURE.NOTE]Imajte na umu da dinamički manifesti ne mijenjaju imovina i zadane manifest tog resursa. Klijent možete odabrati da biste zatražili strujanje sa ili bez filtara. 


###<a id="filters"></a>Filtri 

Postoje dvije vrste filtara resursa: 

- Globalni filtri (može primijeniti na sve resursa na računu servisa Azure Media Services, imate sjajne račun) i 
- Lokalni filtri (možete primijeniti samo na imovinu koja je povezana nakon stvaranja filtar, su vijek imovine). 

Globalni i lokalne filtar vrste imaju točno ista svojstva. Glavna razlika između dviju je koji scenarijima za koju vrstu na filer prikladniji. Globalni Filtri se obično prikladna za uređaj profili (vizualizacije filtriranje) gdje se mogu koristiti lokalne filtre da biste izdvojili određene resursa.


##<a id="scenarios"></a>Uobičajeni scenariji 

Kao što je navedeno prije tijekom izlaganja vaš sadržaj kupcima (strujanje uživo događaje ili video-on-demand) je vaš cilj izlaganje visoke kvalitete videozapisa za različite uređaje drugog mrežnog uvjetima. Osim toga, vaš možda drugim tretiraju koje obuhvaćaju filtriranje svoje imovine ili korištenje **Dinamički manifesta**s. U sljedećim se odjeljcima navode kratki pregled scenarija filtriranja.

- Prikaz podskupa samo audio i video vizualizacija koji određenim uređajima možete rukovati (umjesto sve vizualizacije koje su vezane uz imovine). 
- Reprodukcija samo dio videozapisa (umjesto reprodukciju cijeli videozapisa).
- Prilagodite DVR prozora prezentacije.

##<a name="rendition-filtering"></a>Filtriranje vizualizacije 

Možete odabrati kodiranje vaše resursa više kodiranja profila (osnovne H.264, H.264 visoka, AACL, AACH, Dolby Digital Plus) i više bitrates kvalitetu. Međutim, ne svim klijentskim uređajima će podržavati profili i bitrates sve svoje sredstava. Na primjer, starije uređaje sa sustavom Android podržava samo H.264 osnovne + AACL. Pošaljete veći bitrates uređaj koji se ne može pristupiti prednosti, zauzima izračuni za propusnost i uređaja. Takav uređaj mora dekodiranje sve navedene podatke, samo da biste skalirali prema dolje za prikaz.

Dinamični manifesta omogućuje stvaranje profila uređaj kao što je mobitel konzole HD/SD, itd., i sadrže zapise i podvrste koje želite obuhvatiti svaki profil.

 
![Vizualizacija filtriranja primjer][renditions2]

U sljedećem primjeru kodiranje korišten za kodiranje mezzanine resursa u sedam vizualizacija videozapisa ISO MP4s (iz 180p 1080p). Šifrirana resursa mogu dinamički pakirati u bilo koju od sljedećih protokola za strujanje: HLS, bila glatka, MPEG CRTICE i HDS.  Pri vrhu dijagram, prikazuje se manifest HLS imovine s nema filtara (sadrži sve vizualizacije sedam).  U donjem lijevom kutu, prikazana je manifest HLS na koju je primijenjena filtra pod nazivom "ott". Filtar "ott" navodi da biste uklonili sve bitrates ispod 1Mbps koja je rezultirala dna dva kvalitete razine koji se uklone u odgovoru.  U desnom donjem prikazan je manifest HLS na koju je primijenjena filtra pod nazivom "mobilne". Određuje "mobilne" filtar da biste uklonili vizualizacija gdje je razlučivost veća od 720p, koja je rezultirala dviju 1080p vizualizacija se uklone.

![Filtriranje vizualizacije][renditions1]

##<a name="removing-language-tracks"></a>Uklanjanje jezični zapisi

Vaše imovine mogu sadržavati više jezika zvuka, kao što su engleski, španjolski, francuski, itd. Obično upravitelji reproduktora SDK zadani odabir audiozapisa i dostupne audio prati po odabir korisnika. Je zahtjevne za razvoj takvo Player SDK-ovi, bit će potrebno različite implementacije preko reproduktora okviri specifične za uređaj. Neke platforme reproduktora API-ji ograničeni su i nude značajku audio odabira gdje korisnici ne mogu odabrati ili promijenite zadane audiozapis. S filtrima resursa možete kontrolirati ponašanje stvaranjem filtri koji sadrže samo željene audio jezike.

![Jezični zapisi filtriranja][language_filter]


##<a name="trimming-start-of-an-asset"></a>Skraćivanje start sredstava 

U većini uživo strujanje događaja, operatori pokrenuti neke testove prije stvarni događaja. Na primjer, mogu sadržavati na pločica ili tablica ovako prije početka događaja: "Program će početi invertiraju". Ako je program arhiviranje, testiranje i pločica ili tablica podataka i arhiviraju i uvrstiti u prezentaciji. Međutim, ti podaci ne bi trebala biti prikazana za klijente. S dinamički manifesta možete stvoriti filtrom vrijeme početka i uklonite neželjene podatke iz manifesta.

![Početak obrezivanja][trim_filter]

##<a name="creating-sub-clips-views-from-a-live-archive"></a>Stvaranje podmjesta isječaka (prikazi) iz uživo arhive

Broj događaja uživo predugački pokrenut i uživo arhiva mogu sadržavati više događaja. Nakon uživo događaj završava broadcasters možda će se želite prekinuti gore uživo arhiviranje u logičke program start i stop nizove. Nakon toga objavite programi virtualne zasebno bez objavu obrade uživo arhive i ne stvaranje zasebnom imovine (koji će dobiti prednost postojeće predmemorirani fragmentirane na CDN-ovi). Primjeri takvi virtualne programi (podređenu isječci) su tromjesečja u nogometu ili košarkaškim motivom utakmica, innings u bejzbol ili pojedinačne događaje programa poslijepodne programa olimpijskim igrama.

S dinamički manifesta možete stvoriti filtara pomoću vremena početka i završetka i stvaranje virtualne prikaza preko vrha svoju uživo arhivu. 

![Subclip filtra][subclip_filter]

Filtrirani resursa:

![Skiing][skiing]

##<a name="adjusting-presentation-window-dvr"></a>Prilagodba prozora prezentacije (DVR)

Trenutno servisa Azure Media Services nudi kružni arhivske gdje mogu konfigurirati trajanje između 5 minuta – 25 sati. Manifesta filtriranja mogu se stvoriti rolling prozor DVR preko vrha u arhivu bez brisanja medijske sadržaje. Postoji mnogo scenarija gdje broadcasters želite omogućiti ograničeni DVR prozor koje premješta neporavnatu uživo i istovremeno zadržavanje veći arhiviranja prozor. Televizijska htjeti koristiti podatke koji je izvan DVR prozora da biste istaknuli isječke ili he\she možda želite omogućiti različitim prozorima DVR za različite uređaje. Ako, na primjer, većina mobilnim uređajima ne ručicu za veliki DVR windows (moguće je prozor DVR 2 minute za mobilne uređaje i 1 sat za klijente za stolna računala).

![Prozor DVR][dvr_filter]

##<a name="adjusting-livebackoff-live-position"></a>Prilagodba LiveBackoff (uživo položaj)

Manifesta filtriranja se poslužite da biste uklonili nekoliko sekundi od uživo ruba uživo program. Time se omogućuje broadcasters gledati na točke publikacije pretpregled i stvoriti oglašavanje točke umetanja prije gledatelji primaju strujanje (obično sigurnosno-isključeno po 30 sekundi). Broadcasters možete automatske te oglasa za svoje okviri klijenta u vremenu, za njih primljene i obradu podataka o prije oglašavanje prilike.

Osim podršku oglašavanje LiveBackoff može se koristiti za prilagodbu klijent uživo preuzimanje položaj tako da se prilikom klijenti drift i pritisnete uživo rub oni i dalje moći pristupiti fragmentirane s poslužitelja umjesto početak 404 ili 412 HTTP pogrešaka.

![livebackoff_filter][livebackoff_filter]


##<a name="combining-multiple-rules-in-a-single-filter"></a>Kombiniranje više pravila u jedan filtar

Možete kombinirati više pravila filtriranja u jedan filtar. Kao primjer možete definirati raspon pravilo da biste uklonili pločica ili tablica iz uživo arhive i filtrirati dostupne bitrates. Više pravila filtriranja rezultat je sastavljanje (samo za sjecište) ta pravila.

![više pravila][multiple-rules]

##<a name="create-filters-programmatically"></a>Programski stvaranju filtara

Sljedeće se temi opisuju entiteti Media Services koje su povezane s filtrima. U temi prikazuje i programski stvaranju filtara.  

[Stvaranje filtara pomoću REST API -ji](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Kombiniranje više filtara (filtar sastavljanje)

Možete kombinirati više filtara u jednom URL-a. 

Sljedeći scenarij objašnjava zašto možda želite kombinirati filtre:

1. Morate filtrirati na video podvrste za mobilne uređaje kao što je Android ili iPAD (da biste ograničili videozapisa podvrste). Da biste uklonili neželjene podvrste bi stvaranje filtra globalni koja je prikladna za uređaj profila. Kao što je rečeno iznad globalni Filtri se mogu koristiti za sva sredstva u odjeljku isti media services račun bez izvući pridruživanja. 
2. Želite obrezati vrijeme početka i završetka sredstava. Da biste to postigli, želite stvoriti lokalnu filtar i postavljanje vremena početka i završetka. 
3. Želite kombinirati obje te filtre (bez kombinacija potrebni da biste dodali kvalitete filtriranje filtar skraćivanje reproducirat će korištenje filtar teška).

Da biste kombinirali filtara, prvo morate postaviti nazive filtar manifest/popis URL sa zarezom točka sa zarezom. Recimo da imate filtra pod nazivom *MyMobileDevice* podvrste filtre i imate druge imenovani *MyStartTime* da biste postavili vrijeme početka. Možete ih spojiti ovako:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

Možete kombinirati do 3 filtre. 

Dodatne informacije potražite u članku [ovom](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.


##<a name="know-issues-and-limitations"></a>Saznajte problemima i ograničenjima

- Dinamični manifest funkcionira u GOP ograničenja (ključ okvira) dakle skraćivanje sadrži GOP točnost. 
- Možete koristiti isti naziv filtra za lokalni i globalnim filtre. Imajte na umu lokalne filtar imate veće prednosti i nadjačat će globalni filtre.
- Ako ažurirate filtar, to može potrajati do dvije minute za strujanje krajnju točku da biste osvježili pravila. Ako je sadržaj je poslužena pomoću filtara (i predmemorirani u poslužitelji i CDN predmemorije), ažuriranje ovih filtara može uzrokovati pogreške reproduktora. Nije preporučeno da biste očistili predmemoriju nakon ažuriranja filtar. Ako ta mogućnost nije moguće, preporučujemo da koristite drugačiji filtar.


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Vidi također

[Isporuke sadržaja za pregled klijentima](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
 