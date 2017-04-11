<properties 
    pageTitle="Vodiči za Avanced Media Encoder Premium tijeka rada" 
    description="Ovaj dokument sadrži vodiči koji pokazuju kako izvoditi napredne zadatke s tijekom rada sustava Media Encoder Premium i upute za stvaranje složenih tijekova rada s programom dizajner tijeka rada." 
    services="media-services" 
    documentationCenter="" 
    authors="xstof" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"  
    ms.author="xstof;xpouyat;juliako"/>

#<a name="advanced-media-encoder-premium-workflow-tutorials"></a>Napredni vodiče Premium tijeka rada za Media Encoder

##<a name="overview"></a>Pregled 

Ovaj dokument sadrži vodiči koji pokazuju kako prilagoditi tijekovi rada s programom **Dizajner tijeka rada**. Možete pronaći datoteke za stvarni tijek rada [ovdje](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

##<a name="toc"></a>TABLICA SADRŽAJA

Možete pronaći u sljedećim temama:

- [Kodiranje MXF u jednom brzina prijenosa MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
    - [Započinjanje novog tijeka rada](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new) 
    - [Pomoću unosa za medijske datoteke](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
    - [Pregled strujanja medijskih sadržaja](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
    - [Dodavanje videozapisa kodiranje za. MP4 datoteke generacije](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
    - [Kodiranje strujanje audiozapisa](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
    - [Višestrukosti strujanja Audio i videozapisa u programa MP4 spremnik](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
    - [Pisanje MP4 datoteke](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
    - [Stvaranje web Services medijski sadržaj iz Izlazna datoteka](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
    - [Testiranje dovršeni tijeka rada lokalno](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
- [Kodiranje MXF u multibitrate MP4s - dinamički pakiranje omogućeno](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
    - [Dodavanje jednog ili više dodatnih MP4 izlaze](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
    - [Konfiguriranje izlazne nazivima datoteka](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
    - [Dodavanje zasebnom audiozapisa](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
    - [Dodavanje u. ISM SMIL datoteka](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
- [Kodiranje MXF u multibitrate MP4 - poboljšane nacrt](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
    - [Pregled tijeka rada za poboljšanje](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_overview)
    - [Konvencije imenovanja datoteka](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
    - [Svojstva komponente na korijenskom tijeka rada za objavljivanje](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
    - [Generirao Izlazna datoteka imena ovise o vrijednosti objavljene svojstava](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
- [Dodavanje minijature multibitrate MP4 Izlaz](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
    - [Pregled tijeka rada da biste dodali minijature](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to_multibitrate_MP4_overview)
    - [Dodavanje JPG kodiranja](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
    - [Postupanje s pretvorbe prostor boja](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
    - [Pisanje minijature](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
    - [Otkrivanje pogrešaka u tijeku rada](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
    - [Dovršeni tijeka rada](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
- [Vremenske skraćivanje multibitrate MP4 Izlaz](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
    - [Početi s dodavanjem skraćivanje za pregled tijeka rada](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
    - [Korištenje skrivanje čvorova strujanje](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
    - [Dovršeni tijeka rada](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
- [Uvod u komponentu skriptiranih](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
    - [Skriptiranje unutar tijeka rada: pozdrav svijeta](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
- [Skraćivanje utemeljen na okvir multibitrate MP4 izlaza](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
    - [Nacrt početi s dodavanjem skraćivanje za pregled](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
    - [Rad s popisom isječak XML](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
    - [Izmjena popisa isječak iz komponente za postavljanje upita skriptiranih](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
    - [Dodavanje svojstvo ClippingEnabled pogodnost](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

##<a id="MXF_to_MP4"></a>Kodiranje MXF u jednom brzina prijenosa MP4
 
U ovom vodiču ćemo ćete stvoriti jednu brzina prijenosa. MP4 datoteka uz AAC HE kodirana zvuk iz programa. MXF ulazne datoteke. 

###<a id="MXF_to_MP4_start_new"></a>Započinjanje novog tijeka rada 

Otvorili dizajner tijeka rada, a zatim odaberite "Datoteke"-"novog radnog prostora"-"Transcode nacrt" 

Novi tijek rada prikazat će se 3 elemenata: 

- Primarni izvorišna datoteka
- Popis XML isječaka
- Datoteka resursa Izlaz  

![Novi tijek rada kodiranja](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Novi tijek rada kodiranja*

###<a id="MXF_to_MP4_with_file_input"></a>Pomoću unosa za medijske datoteke

Da biste prihvatili naš unos medijske datoteke, jedan započinje s dodavanjem medijske datoteke unos komponente. Da biste dodali komponentu tijek rada, potražite ga u okvir za pretraživanje u spremištu i povucite željenu stavku u okno dizajnera. Postupak za medijske datoteke unos i povežite komponentu primarni izvornu datoteku Filename pin za unos iz medijske datoteke unos.

![Povezani medijskih datoteka unos](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Povezani medijskih datoteka unos*

Prije toga moramo približno još, ćete prvi put moramo da biste naznačili u dizajneru tijeka rada koje oglednu datoteku željeli bismo dizajnirati naš tijeka rada s. Da biste to učinili, kliknite pozadina dizajnera okno i pogledajte svojstvo primarni izvornu datoteku u oknu svojstva na desnoj strani. Kliknite ikonu mape, a zatim odaberite željenu datoteku da biste testirali tijeka rada s. Čim to možete učiniti, komponentu medijske datoteke unos će provjera datoteku i popunjavanje njegov izlaz PIN-ove u skladu s vizualnim ga provjerava ima li datoteka.

![Popunjena medijskih datoteka unos](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Popunjena medijskih datoteka unos*

Dok se određuje s koje unos željeli bismo raditi, ona ne govori još kodiranih izlaz cilj da biste. Sličan način primarni izvorišna datoteka je konfiguriran, sada Konfiguriranje izlazne mape varijable svojstvo neposredno ispod njega.

![Konfigurirani ulazni i izlazni svojstva](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Konfigurirani ulazni i izlazni svojstva*

###<a id="MXF_to_MP4_streams"></a>Pregled strujanja medijskih sadržaja

Često je ga želite znati kako toka pretražuje tako tokova tijeka rada. Provjerite toka tijeka rada u bilo kojem trenutku, jednostavno kliknite Izlaz ili unos PIN-a na bilo kojoj od komponente. U ovom slučaju, pokušajte kliknuti na Izlazni pin koje nisu komprimirane videozapis iz naših unos medijskih datoteka. Dijaloški okvir, otvorit će koji omogućuje Provjera izlazni videozapis.

![Pregled Izlazni pin koje nisu komprimirane videozapis](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Pregled Izlazni pin koje nisu komprimirane videozapis*

U našem slučaju govori nam, primjerice da se ne možemo povezanima s 1920 x 1080 unos pri 24 slika u sekundi u 4:2:2 uzorkovanje videozapis gotovo dvije minute.

###<a id="MXF_to_MP4_file_generation"></a>Dodavanje videozapisa kodiranje za. MP4 datoteke generacije

Imajte na umu da sada, koje nisu komprimirane videozapis i više koje nisu komprimirane Audio izlazni PIN-ove dostupne su za korištenje na našem unos medijskih datoteka. Da bi se kodiranje ulaznog videozapis, moramo kodiranja komponente – u ovom slučaju za generiranje. MP4 datoteke.

Da biste kodiranje strujanje videozapisa da biste H.264, dodajte komponentu AVC videozapisa Encoder dizajnera površina. Komponente uzima strujanje videozapisa programa uncompress kao ulaz i nudi programa AVC Komprimirana strujanje videozapisa na njegov izlaz PIN-a.

![Ulazite AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Ulazite AVC Encoder*

Svojstva određuju kako kodiranje točno se događa. Pogledajmo o čemu susret neke važnijih postavke:

- Izlaz širinu i visinu izlaz: te odrediti razlučivost kodiranih videozapis. U slučaju da naš prođimo s 640 x 360
- Broj prikazanih: kada je postavljeno na passthrough ga samo prihvaćaju sekundi izvora, moguće je da biste nadjačali to kroz. Imajte na umu da takve pretvorbe frekvenciju okvira je ne kretanja-kompenzirati.
- Profil i razina: te odrediti AVC profila i razinu. Radi lakšeg dobiti dodatne informacije o različitim razinama i profili, kliknite ikonu upitnik u komponenti AVC videozapis kodiranje i stranice pomoći će prikazati dodatne detalje o svakoj od razina. Za naše uzorak podsjetimo se s profilom glavne na razini 3,2 (zadano).
- Ocjenjivanje načina rada za kontrolu i brzina prijenosa (KB/s): u našem scenariju smo uključivanje za s nepromjenjivim brzina prijenosa (CBR) Izlaz na 1200 kb/s
- : Videozapis to je oblik o VUI (videozapis upotrebljivosti informacije) koja se dobiva zapisati u strujanje H.264 (strani informacije koje možda koristi sadržaja drugog da biste poboljšali prikaz, ali ne ključna za pravilno dekodiranje):
- NTSC (Tipični SAD-a ili Japan, pomoću 30 fps)
- PAL (uobičajeni Europi, pomoću 25 fps)
- Način veličine GOP: smo ćete konfigurirati Fixed GOP veličina naš svrhe s ključ Interval od dvije sekunde s zatvoren GOPs. Taj se način kompatibilnosti s dinamički pakiranje Azure Media Services nudi.

Na sažetak sadržaja naše AVC encoder, povezati Izlazni pin koje nisu komprimirane videozapis iz medijske datoteke unos komponente koje nisu komprimirane videozapis unos PIN-a iz AVC encoder.

![Povezani AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Povezani glavne AVC encoder*

###<a id="MXF_to_MP4_audio"></a>Kodiranje strujanje audiozapisa

Sada ćemo imati kodirani videozapis, ali izvorni koje nisu komprimirane strujanje audio i dalje mora biti spojene. Za to ćemo uz AAC kodiranje komponenta za kodiranje AAC (Dolby). Dodajte je u tijek rada.

![Ulazite AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Kodiranje Ulazite AAC*

Sada je programa nekompatibilnost programa: postoji samo jedan koje nisu Komprimirane audio unos PIN-a iz AAC Encoder tijekom više od vjerojatno medijske datoteke unos će imati dvije različite koje nisu komprimirane strujanje audiozapisa dostupna: jedan za kanal lijevom audio i jedan za desno. (Ako ste povezanima s Uokviri zvuk, koji je 6 kanale.) Tako da nije moguće povezati izravno zvuk iz izvora unos medijskih datoteka u kodiranje audio AAC. Komponente AAC očekuje so-called "interleaved" strujanje audiozapisa: jedno strujanje koja ima lijevo i desno kanale interleaved međusobno. Nakon što smo informacije iz naših izvor medijske datoteke koje audiozapisa su na koje mjesto u izvoru smo generiranje takve interleaved audio strujanje položaja pravilno dodijeljene zvučnika za lijevo i desno.

Najprije će nešto želite interleaved strujanje generirao zvučni kanali potrebna izvora. Komponenta Interleaver strujanje Audio će riješiti za us. Dodavanje tijeka rada i povezivanje audio izlaza iz unos medijskih datoteka u nju.

![Povezani Interleaver strujanje audiozapisa](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Povezani Interleaver strujanje audiozapisa*

Imamo interleaved strujanje audiozapisa, ne možemo još uvijek niste odredite mjesto dodijeliti položaje zvučnika ulijevo ili udesno da biste. Da biste odredili, ne možemo omogućuje korištenje položaj Assigner govornika.

![Dodavanje Assigner za položaj zvučnika](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Dodavanje Assigner za položaj zvučnika*

Konfiguriranje Assigner položaj izlagača za upotrebu sa stereozvuka ulaznog toka putem programa kodiranje unaprijed postavljeni filtar "Prilagođeno", a unaprijed kanala naziva "2.0 (L, R)". (Ovo će dodijeliti položaj lijevom zvučnik da biste kanal 1 i položaj na desni zvučnika kanal 2.)

Izlaz iz Assigner položaj zvučnika povezati unos AAC Encoder. Nakon toga reći Encoder AAC raditi s "2.0 (L, R)" kanal unaprijed, da bi ga znali baviti stereozvuka zvuk kao unos.

###<a id="MXF_to_MP4_audio_and_fideo"></a>Višestrukosti strujanja Audio i videozapisa u programa MP4 spremnik

Navedeni naše AVC kodiranih strujanje videozapisa i naše AAC kodirani strujanje audiozapisa, ne možemo oboje u možete uhvatiti programa. Spremnik MP4. Postupak Miješanje različite strujanja u jednom jedan zove "višestrukosti" (ili "muxing"). U ovom slučaju ne možemo ste interleaving audio i videosadržaja u jednom koherentnost. MP4 paketa. Komponentu koordinate ovo da biste vidjeli programa. Spremnik MP4 naziva ISO MPEG-4 Multiplexer. Dodati dizajnera plošni i povežite videozapis Encoder AVC i kodiranje AAC njegov unosa.

![Povezani MPEG4 Multiplexer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Povezani MPEG4 Multiplexer*

###<a id="MXF_to_MP4_writing_mp4"></a>Pisanje MP4 datoteke

Prilikom pisanja Izlazna datoteka koristi se komponenta Izlazna datoteka. Ćemo možete povezati to izlaz ISO MPEG-4 Multiplexer tako da se dobiva rezultat napisan na disk. Da biste to učinili, povezati Izlazni pin spremnik (MPEG-4) pisanje unos PIN-a izlaza datoteke.

![Povezani Izlazna datoteka](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Povezani Izlazna datoteka*

Svojstvo datoteke određen je naziv koji će se koristiti. Iako to svojstvo mogu biti koji nisu zadani vrijednosti, najvjerojatnije jedan ćete željeti postavite ga u izrazu.

Da bi se tijek rada automatski odrediti izlaz datoteka naziv svojstva iz izraza, kliknite zasivljenim gumbom pokraj naziva datoteke (uz ikonu mape). Na padajućem izborniku pa odaberite "Izraz". To će se srušiti gore editor izraz. Najprije poništite sadržaj uređivaču.

![Prazan uređivač izraza](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Prazan uređivač izraza*

Uređivač izraza omogućuje da biste unijeli sve pisana vrijednost s jedne ili više varijabli. Varijable počinju znak za dolar. Kao što pritisnete tipku $, uređivaču prikazivat će se okvir padajući izbornik s izbor dostupna varijabli. U slučaju da naš ćemo pomoću kombinacije izlazna varijabla direktorija i naziv varijable osnovni Ulazna datoteka:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Ispunjen uređivač izraza za odgovor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Ispunjen uređivač izraza za odgovor*

>[AZURE.NOTE]Da biste vidjeli potražite u članku Izlazna datoteka vaše kodiranja posla Azure morate navesti vrijednost u uređivaču izraz. 

Kada potvrdite izraz tako da odlazak u redu, prozor svojstvo će pretpregledati što vrijednost razrješava svojstava datoteke u tom trenutku u trenutku.

![Datoteka izraza dir Izlaz](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Datoteka izraza dir Izlaz*

###<a id="MXF_to_MP4_asset_from_output"></a>Stvaranje web Services medijski sadržaj iz Izlazna datoteka

Dok zapisan MP4 izlaznu datoteku, i dalje moramo da biste označili tu datoteku pripada izlaz resursa koji kao rezultat izvođenja ovaj tijek rada generirat će media services. Na kraju, koristi se čvor Izlazna datoteka/resursa u području crtanja tijeka rada. Sve dolazne datoteke u ovaj čvor će postati dio rezultata resursa servisa Azure Media Services.

Povežite komponentu Izlazna datoteka s komponentom Izlazna datoteka/resursa za dovršavanje tijeka rada.

![Dovršeni tijeka rada](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Dovršeni tijeka rada*

###<a id="MXF_to_MP4_test"></a>Testiranje dovršeni tijeka rada lokalno

Da biste testirali lokalno tijeka rada, kliknite gumb Reproduciraj na alatnoj traci pri vrhu. Kada završite s tijek rada izvodi, provjeri izlaz generirane u konfigurirano Izlazna datoteka. Vidjet ćete dovršeni Izlazna datoteka MP4 koji je kodirani iz MXF unos izvorne datoteke.

##<a id="MXF_to_MP4_with_dyn_packaging"></a>Kodiranje MXF u MP4 - multibitrate omogućen za dinamičku pakiranje

U ovom vodiču smo stvorit ćete skup više brzina prijenosa MP4 datoteke uz AAC kodirani zvuka s jednom. MXF ulazne datoteke.

Kada izlaz za resursa višestruki-brzina prijenosa željenu za korištenje u kombinaciji sa značajkama za dinamičku pakiranje nudi više datoteka GOP poravnat MP4 svakog različite brzina prijenosa i razlučivost morat ćete se generira Azure Media Services. Da biste to učinili, prođite kroz [Kodiranje MXF u jednom brzina prijenosa MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) pruža nam dobru početnu točku.

![Pokretanje tijeka rada](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Pokretanje tijeka rada*

###<a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Dodavanje jednog ili više dodatnih MP4 izlaze

Sve datoteke MP4 u našem dobivene resursa servisa Azure Media Services će podržavati različite brzina prijenosa i rješenja. Dodat ćemo jednu ili više datoteka izlaz MP4 tijeka rada.

Da biste provjerili smo sve naše video koderi stvorena pomoću istih postavki, dobro je najčešće dupliciranje već postojeće kodiranje videozapisa AVC i konfiguriranje drugi kombinaciju razlučivost i brzina prijenosa (dodajmo nešto 960 x 540 pri 25 slika u sekundi pri 2,5 MB/s). Da biste duplicirali postojeće encoder, Kopiraj ga zalijepiti na površini za dizajnera.

Povezivanje videozapisa koje nisu komprimirane Izlazni pin ulaza medijskih datoteka u našem novu komponentu AVC.

![Drugi AVC encoder povezani](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Drugi AVC encoder povezani*

Sada možete prilagoditi konfiguraciju za naš novi AVC kodiranje za izlaz 960 x 540 pri 2,5 MB/s. (Koristi njegova svojstva "širine izlaza", "Izlaz visina" i "Brzina prijenosa (KB/s)" za to.)

Dani smo želite koristiti dobivene resursa zajedno s servisa Azure Media Services' dinamički pakiranje, krajnju točku strujanje mora biti instaliranog generiranje iz te datoteke MP4 fragmentirane MP4 CRTICE HLS/Fragmented koji su točno poravnani međusobno tako da klijenti koje se prebacujete između različitih bitrates dobiti jednu izglađenim neprekinuti videozapis i doživljaj zvuka. Da biste koji se pokreću, moramo da biste bili sigurni da u svojstvima oba AVC koderi GOP ("grupa slika") veličine za obje datoteke MP4 postavljen na dvije sekunde koje možete poduzeti:

- Postavljanje način veličine GOP Fiksna GOP veličinu i
- Interval okvir ključ za dvije sekunde.
- postaviti kontrolu IDR GOP da biste zatvorili GOP da biste bili sigurni sve GOP položaj su na vlastita bez ovisnosti

Da biste praktično da biste shvatili naš tijeka rada, preimenovanje prvi AVC kodiranje za "AVC videozapis Encoder 640 x 360 1200 kb/s" i drugi AVC kodiranje "videozapis AVC Encoder 960 x 540 2500 kb/s".

Sada možete dodati u drugi ISO MPEG-4 Multiplexer i drugi Izlazna datoteka. Povezati s multiplexer nove AVC kodiranje i provjerite preusmjeravaju li se rezultat je u izlaz datoteke. Zatim povezati AAC kodiranje audio izlaz novu multiplexer poštanskog ulazne podatke. Izlazna datoteka shodno zatim moguće je povezati čvor Izlazna datoteka/resursa da biste ga dodali medijski servisa koji će se stvoriti.

![Drugi Muxer i izlazna datoteka povezani](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Drugi Muxer i izlazna datoteka povezani*

Kompatibilnom sa servisa Azure Media Services dinamički pakiranje konfigurirajte na multiplexer na način bloka GOP broj ili trajanje i GOPs po bloka postavite na 1. (To mora biti zadana.)

![Načini rada s Muxer bloka](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Načini rada s Muxer bloka*

Napomena: trebali biste ponovite postupak za sve druge brzina prijenosa i razlučivost kombinacije želite ste dodali u rezultatu resursa.

###<a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Konfiguriranje izlazne nazivima datoteka

Imamo više od jedne jedne datoteke dodaje resursa izlaz. To omogućuje treba provjerite jesu li nazivi datoteka za svaku Izlazna datoteka razlikovati i možda čak primijeniti na konvencije imenovanja datoteka tako da postane Očisti od naziva što ste povezanima s.

Imenovanje Izlazna datoteka možete kontrolirati putem izraza za analizu. Otvaranje okna sa svojstvima zbog nekog od komponente izlaz datoteke i otvorite uređivač izraza za svojstva datoteke. Naš prvi Izlazna datoteka je konfiguriran pomoću sljedećeg izraza (pogledajte vodič za prijelaz s [MXF u jednom brzina prijenosa MP4 izlaz](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

To znači da naš filename ovisi o dvije varijable: izlaznog direktorija za pisanje i ime osnovni izvorne datoteke. Prva Bliža predstavljeni kao svojstvo na korijenskom tijeka rada, a drugu mogućnost određuje dolazne datoteke. Imajte na umu da izlaznog direktorija koristite za lokalni testiranje; Ovo svojstvo će biti nadjačati modul tijeka rada kada se tijek rada je izvršio procesora oblaku medijskih sadržaja u web-servisa Azure Media Services.
Da bi se dobilo obje naše datoteke izlaz imenovanja dosljedan Izlaz, promijenite imenovanja izraz u prvom datoteka:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

i sekundi da biste:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Izvršavanje programa Srednja pokretanje da biste provjerili je li pravilno generiraju obje datoteke MP4 izlaz testiranja.

###<a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Dodavanje zasebnom audiozapisa

Kao što je ćemo vidjeti kasnije prilikom ćemo stvoriti datoteku .ism za naše MP4 Izlazna datoteka, ne možemo treba MP4 datoteke samo za zvuk kao audiozapise za naše prilagodljivu streaming. Da biste stvorili datoteku, dodajte dodatne muxer tijeka rada (Multiplexer ISO – MPEG-4) te je povežite AAC encoder izlazni PIN-a s njegova unos PIN-om za praćenje 1.

![Audio Muxer dodali](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Audio Muxer dodali*

Stvaranje drugih komponenti Izlazna datoteka za izlaz izlaznog strujanje iz na muxer i konfiguriranje imenovanja izraz kao datoteka:
    
    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Audio Muxer stvaranje Izlazna datoteka](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Audio Muxer stvaranje Izlazna datoteka*

###<a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Dodavanje u. ISM SMIL datoteka

Za dinamičku pakiranja za rad u kombinaciji s MP4 datoteke (i MP4 samo zvuk) u našem resursa Media Services i moramo manifesta datoteke (naziva se i datoteke "SMIL": sinkronizirati jezik za integraciju Multimedija). Tu datoteku označava servisa Azure Media Services koje MP4 datoteke su dostupne za dinamičku pakiranje i što od onih kojima treba voditi računa za strujanje audio. Uobičajeni manifesta datoteke za skup MP4 korisnika s jednom strujanje audiozapisa izgleda ovako:
    
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

Datoteka .ism sadrži u sklopu promjenu naredbe, referenca na svakom pojedinačne MP4 video datoteka i osim one i jedan (ili više) zvučnu datoteku reference programa MP4 koji sadrži samo zvuk.

Generiranje manifesta datoteke za naše skup MP4, možete učiniti putem komponenta naziva u "AMS manifesta Writer". Da biste koristili, povucite na površina i povežite "Pisanje dovršeno" izlazni PIN-ove iz tri Izlazna datoteka komponente Writer manifesta AMS unos. Pazite da biste se povezali izlaz AMS manifesta Writer da biste Izlazna datoteka/sredstava.

Kao i kod naše ostale datoteke izlaz komponente, konfigurirati naziv izlazna .ism datoteke s izrazom:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Naš dovršeni tijeka rada izgleda u nastavku:

![Dovršeni MXF tijek rada MP4 multibitrate](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Gotovi MXF tijek rada MP4 multibitrate*

##<a id="MXF_to__multibitrate_MP4"></a>Kodiranje MXF u multibitrate MP4 - poboljšane nacrt

Na [Prethodni prikaz tijeka rada](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) smo vidjeli kako jedan resursa unos MXF se može pretvoriti u izlaz imovine s više – brzina prijenosa MP4 datoteke, samo zvuk MP4 datoteke i manifesta datoteku za upotrebu u kombinaciji s pakiranja za dinamičku servisa Azure Media Services.

Ovaj vodič prikazat će kako neke na aspekte možete Poboljšana i unijeli praktičnije.

###<a id="MXF_to_multibitrate_MP4_overview"></a>Pregled tijeka rada za poboljšanje

![Multibitrate MP4 tijeka rada za poboljšanje](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Multibitrate MP4 tijeka rada za poboljšanje*

###<a id="MXF_to__multibitrate_MP4_file_naming"></a>Konvencije imenovanja datoteka

U tijeku rada za prethodni ćemo navesti jednostavan izraz kao temelj za generiranje nazive Izlazna datoteka. Imamo neke dupliciranje kroz: sve u komponenti pojedinačne Izlazna datoteka naveden kao što je izraz.

Na primjer, naše datoteke izlaz komponenta za prvu video datoteka je konfiguriran za korištenje ovaj izraz:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

Za drugi izlaz videozapisa, imamo izraz kao što su:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Ne može li biti čišćenje manje pogrešaka podložni i praktičnije ako smo nije moguće ukloniti neke ovaj dupliciranje i provjerite stvari više konfigurirati umjesto toga? Na sreću možemo: je dizajner mogućnosti izraza u kombinaciji s omogućuje stvaranje prilagođenih svojstava u našem korijenu tijek rada će nam dodane sloju praktičnost.

Recimo da ćemo ćete pogon konfiguracije naziv datoteke iz bitrates pojedinačne datoteke MP4. Ove bitrates ćemo ćete usmjerite da biste konfigurirali središnje odjednom (na korijenu naš grafikonu), gdje im ćete pristupiti da biste konfigurirali i generiranje naziv datoteke za pogon. Da biste to učinili, ne možemo pokrenuti objavljivanjem svojstvo brzina prijenosa iz obje koderi AVC korijen naš tijeka rada tako da postane dostupna u oba korijenskom kao i: koderi AVC. (Čak i ako se prikaže u dvije različite spotne boje, postoji samo jedna vrijednost u podlozi.)

###<a id="MXF_to__multibitrate_MP4_publishing"></a>Svojstva komponente na korijenskom tijeka rada za objavljivanje

Otvorite prvi AVC encoder, idite na svojstvo brzina prijenosa (KB/s) i na padajućem izborniku odaberite Objavi.

![Svojstvo brzina prijenosa za objavljivanje](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Svojstvo brzina prijenosa za objavljivanje*

Konfiguriranje dijaloški okvir Objavi u Objavi u korijenu grafikonu naš tijeka rada s Objavljeni naziv "video1bitrate" i čitljiv zaslonsko ime od "Videozapis 1 brzina prijenosa". Konfiguriranje prilagođeni naziv grupe pod nazivom "Strujanje Bitrates" i kliknite Objavi.

![Svojstvo brzina prijenosa za objavljivanje](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Objavljivanje dijaloški okvir svojstva brzina prijenosa*

Ponovite isti za svojstvo brzina prijenosa drugi AVC kodiranje i nazovite ih "video2bitrate" s zaslonsko ime "Videozapis 2 brzina prijenosa", u istom prilagođenoj grupi "Strujanje Bitrates".

Ako ne možemo sada provjeri svojstva korijenski tijeka rada, ćemo vidjeti naš prilagođenu grupu s dva objavljene svojstva prikazuju. Obje se o vrijednost njihove odgovarajući AVC encoder brzina prijenosa.

![Rekvizita za objavljene brzina prijenosa u korijenu tijeka rada](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Kad god želimo da biste pristupili tim svojstvima iz kod ili izraz, ne možemo možete učiniti ovako:

- iz koda izravno iz komponente ispod korijenskom: node.getPropertyAsString('.. / video1bitrate', null)
- izraz: ${ROOT_video1bitrate}
 
Pogledajmo dovršite grupi "Strujanje Bitrates" objavljivanjem na našem audiozapis brzina prijenosa na njemu te. Unutar svojstva AAC Encoder, pretraživanja pronađite postavku brzina prijenosa, a zatim odaberite Objavi na padajućem izborniku uz nju. Objavljivanje u korijenskom direktoriju grafikon s nazivom "audio1bitrate" i zaslonsko ime "Audio 1 brzina prijenosa" unutar naš prilagođenu grupu "Strujanje Bitrates".

![Objavljivanje dijaloški okvir za audio brzina prijenosa](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Objavljivanje dijaloški okvir za audio brzina prijenosa*

![Rezultat rekvizita audiozapisa i videozapisa u korijenu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Rezultat rekvizita audiozapisa i videozapisa u korijenu*

Imajte na umu da promjena bilo koje ta tri vrijednosti i ponovno konfigurira i mijenja vrijednosti na odgovarajući komponente kojim su povezane s (i gdje je objavio).

###<a id="MXF_to__multibitrate_MP4_output_files"></a>Generirao Izlazna datoteka imena ovise o vrijednosti objavljene svojstava

Umjesto hardcoding naš generirani nazivima, ne možemo sada možete promijeniti naš filename izraz za svaku od Izlazna datoteka komponente navikli svojstva brzina prijenosa smo upravo objavili na korijenskom grafikonu. Počevši od naše prvi izlaz datoteke, pronađite svojstvo datoteke web-mjesta i uređivanje izraz ovako:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

Različite parametara u ovaj izraz možete pristupiti i unese odlazak znak za dolar na tipkovnici u prozoru izraz. Jedan od dostupnih parametara je naš video1bitrate svojstvo koje ćemo objavljene neke starije verzije.

![Pristup parametara izraz](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Pristup parametara izraz*

Isto učinite i za izlaz datoteke za naše drugi videozapis:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

i izlazna datoteka samo za zvuka:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Sada ćemo promijenili brzina prijenosa za bilo koju datoteku zvučnog ili videozapisa, odgovarajući encoder će biti rekonfigurirati, a naziv konvencija brzina prijenosa datoteka će se na snazi sve automatski.

##<a id="thumbnails_to__multibitrate_MP4"></a>Dodavanje minijature multibitrate MP4 Izlaz

Pokretanje tijeka rada koje generira [multibitrate MP4 izlaza iz programa MXF za unos](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), ne možemo će sada gledate u dodavanje minijature izlaz.

###<a id="thumbnails_to__multibitrate_MP4_overview"></a>Pregled tijeka rada da biste dodali minijature

![Multibitrate MP4 tijeka rada za pokretanje](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Multibitrate MP4 tijeka rada za pokretanje*

###<a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Dodavanje JPG kodiranja

Pozivanje našeg minijatura generacije bit će JPG Encoder komponente moći izlaz JPG datoteke.

![Kodiranje JPG](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*Kodiranje JPG*

Ne možemo ne no izravno povežite naš strujanje videozapisa koje nisu komprimirane unos medijskih datoteka u JPG encoder. Umjesto toga se očekuje da biste se ljevoruka pojedinačne okvira. To, možemo putem komponente prolaz okvira videozapisa.

![Povezivanje okvira prolaz JPG encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Povezivanje okvira prolaz JPG encoder*

Okvir prolaz svaki toliko sekundi, odnosno okvira omogućuje sličicu videozapisa za prosljeđivanje. Interval i vrijeme pomak uz koju se to dogodi je konfigurirati u svojstvima.

![Svojstva prolaz okvira videozapisa](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Svojstva prolaz okvira videozapisa*

Stvaranje minijature svake minute tako da postavite u način rada na vrijeme (u sekundama) i Interval u 60.

###<a id="thumbnails_to__multibitrate_MP4_color_space"></a>Postupanje s pretvorbe prostor boja

Dok bi čini logičke sada mogu povezati i PIN-ove koje nisu komprimirane videozapis prolaz okvira i unos medijske datoteke, ne možemo postiže upozorenje smo biste ga učinili.

![Pogreške prostora boja za unos](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Pogreške prostora boja za unos*

To je zato način u koje boja informacije predstavlja u našem izvorne neobrađenog koje nisu komprimirane strujanje videozapisa, koji dolaze iz naših MXF razlikuje se od što JPG Encoder očekuje. Preciznije, na so-called "boja razmak" "RGB" ili "U sivim tonovima" očekuje tijek u. To znači da u Video okvira prolaz poštanskog ulazne strujanje videozapisa moraju imati pretvorbe primjenjuje vezane uz njegov prostor boja.

Povucite tijeka rada prostora pretvornik boje – Intel i povežite ga s naš okvir prolaz.

![Povezivanje s Convertor prostor boja](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Povezivanje s Convertor prostor boja*

U prozoru svojstva odaberite BGR 24 stavke s popisa zadana Postava.

###<a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Pisanje minijature

Razlikuje od naše MP4 video, komponentu JPG Encoder će izlaz više od jedne datoteke. Da bi se baviti to, može se koristiti komponentu Scene pretraživanje JPG datoteke Writer: bit će potrajati dolazne JPG minijature i pisanje ih, svaki naziv u tijeku suffixed tako da drugi broj. (Broj obično koji označava broj sekundi/jedinice u toka koji minijaturu nacrtan iz.)


![Uvod u Writer Scene pretraživanje JPG datoteke](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Uvod u Writer Scene pretraživanje JPG datoteke*

Konfiguriranje svojstava put do mape izlaz uz izraz: ${ROOT_outputWriteDirectory} 

i svojstvo prefiks naziva datoteke s: 

    ${ROOT_sourceFileBaseName}_thumb_

Prefiks određujete kako su koji se naziva minijatura datoteke. Će biti suffixed s broj koji označava položaj na palac u toka.


![Svojstva pretraživanja JPG datoteke Writer scene](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Svojstva pretraživanja JPG datoteke Writer scene*

Povezivanje Scene Writer za pretraživanje JPG datoteke čvor datoteka resursa izlaz.

###<a id="thumbnails_to__multibitrate_MP4_errors"></a>Otkrivanje pogrešaka u tijeku rada

Povezivanje unos prostora pretvornik boja neobrađenog koje nisu komprimirane video izlaz. Sada možete izvršavati lokalne test pokrenuti tijek rada. Postoji vjerojatnost tijek rada će iznenada zaustavili izvršavanje i označavanje s crvenim strukture u komponenti koji je naišao na pogrešku:

![Boja prostora pretvornik pogreške](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Boja prostora pretvornik pogreške*

Kliknite ikonu crvena malo "E" u gornjem desnom kutu komponentu boja prostora pretvornik da biste vidjeli što je razloga kodiranja pokušaj nije uspjelo.

![Dijaloški okvir pogreške prostora pretvornik za boju](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Dijaloški okvir pogreške prostora pretvornik za boju*

To uključuje, kao što vidite, da dolazne prostor boja standarda za prostora pretvornik boja mora biti rec601 za naše tražene Pretvorba YUV u RGB. Trenutno naš strujanje ne označavaju je rec601. (SNI 601 je standard za šifriranje isprepleteno signale analognih videozapisa u digitalni oblik videozapisa. Određuje sustava active regija prekrivajući 720 osvijetljenost uzoraka i 360 chrominance uzoraka po retku. Boja kodiranje sustava zove YCbCr 4:2:2.)

Da biste riješili taj problem, ne možemo ćete upućivati na metapodacima naš strujanje koje ćemo se bave rec601 sadržaj. Da biste to učinili ćemo koristiti videozapisa za ažuriranje programa vrsta podataka komponente koje ćemo ćete staviti između naš neobrađenog izvora i komponentu za pretvorbu prostora boja. Ovo ažuriranje programa vrstu podataka za ručno ažuriranje određene video podataka omogućuje svojstva vrste. Konfigurirajte ga tako da biste naznačili u boji prostora Standard "SNI 601". Time će se na videozapis podataka vrsta ažuriranje programa da biste označili strujanje s prostor boja "SNI 601" Ako je definirani još prostora prostor boja. (Ga neće promijeniti postojeće metapodaci, osim ako je potvrđen nadjačavanje.)

![Ažuriranje standardna boja prostora na ažuriranje programa za vrste podataka](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Ažuriranje standardna boja prostora na ažuriranje programa za vrste podataka*

###<a id="thumbnails_to__multibitrate_MP4_finish"></a>Dovršeni tijeka rada

Sad kad naš naš tijeka rada je dovršen, učinite drugi test Pokreni da biste je vidjeli prosljeđivanje.

![Dovršeni tijeka rada za izlaz više mp4 s minijaturama](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Dovršeni tijeka rada za izlaz više mp4 s minijaturama*

##<a id="time_based_trim"></a>Vremenske skraćivanje multibitrate MP4 Izlaz

Pokretanje tijeka rada koje generira [multibitrate MP4 izlaza iz programa MXF za unos](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), ne možemo će sada gledate u izvora videozapisa na temelju vremenske oznake obrezivanja.

###<a id="time_based_trim_start"></a>Početi s dodavanjem skraćivanje za pregled tijeka rada

![Pokretanje tijeka rada da biste dodali ograničenje za](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Pokretanje tijeka rada da biste dodali ograničenje za*

###<a id="time_based_trim_use_stream_trimmer"></a>Korištenje skrivanje čvorova strujanje

Skrivanje čvorova strujanje komponente omogućuje da biste izdvojili početak i kraj programa ulaznog toka osnovni na tempiranja informacije (sekundi, minuta,...). Za skrivanje čvorova ne podržava utemeljen na okvir obrezivanja.

![Skrivanje čvorova strujanje](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Skrivanje čvorova strujanje*

Umjesto povezivanje AVC koderi i assigner položaj zvučnika unos medijskih datoteka izravno, ne možemo ćete staviti između one skrivanje čvorova strujanje. (Jedan za video signala, a jedan za interleaved zvučni signal.)

![U međuvremenu staviti skrivanje čvorova strujanje](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*U međuvremenu staviti skrivanje čvorova strujanje*

Pogledajmo konfigurirati za skrivanje čvorova tako da ne možemo obradit će se samo video i audiosadržaji između 15 sekundi i 60 sekundi u videozapisu.

Idite na svojstva skrivanje čvorova za strujanje videozapisa i konfiguriranje vrijeme početka (15s) i vrijeme završetka (60s) svojstva. Da biste provjerili je li i skrivanje čvorova naš audio i video uvijek konfigurirani tako da na isti početak i kraj vrijednosti, ne možemo objaviti one u korijen tijeka rada.

![Objavljivanje svojstvo vrijeme početka iz skrivanje čvorova strujanje](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Objavljivanje svojstvo vrijeme početka iz skrivanje čvorova strujanje*

![Objavljivanje dijaloški okvir svojstva za vrijeme početka](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Objavljivanje dijaloški okvir svojstva za vrijeme početka*

![Objavljivanje dijaloški okvir svojstva za vrijeme završetka](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Objavljivanje dijaloški okvir svojstva za vrijeme završetka*


Ako je sada ćemo Provjera korijenu naš tijeka rada, oba svojstva bit će jednostavno prikazati i konfigurirati iz nje.

![Objavljeni svojstva dostupna u korijenu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Objavljeni svojstva dostupna u korijenu*

Sada otvorite svojstva skraćivanje iz audio skrivanje čvorova i konfiguriranje vremena početka i završetka pomoću izraza koji se odnosi na objavljeni svojstva na korijenu naš tijeka rada.

Za vrijeme početka zvuka skraćivanje:

    ${ROOT_TrimmingStartTime}

i njegovo vrijeme završetka:

    ${ROOT_TrimmingEndTime}

###<a id="time_based_trim_finish"></a>Dovršeni tijeka rada

![Dovršeni tijeka rada](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Dovršeni tijeka rada*


##<a id="scripting"></a>Uvod u skriptiranih komponente

Skriptiranih komponente možete izvršiti proizvoljne skripte tijekom faze izvođenja naš tijeka rada. Postoje četiri različite skripte koje možete izvršiti, svaka s određene značajke i svoje mjesto u tijek rada – vijek:

- **commandScript**
- **realizeScript**
- **processInputScript**
- **lifeCycleScript**

Dokumentacija komponentu postavljanje upita skriptiranih ide detaljno za svaku od navedenog. U [sljedećem odjeljku](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)komponentu skriptiranja **realizeScript** koristi se za sastavljanje cliplist xml u hodu kada se tijek rada pokrene. Ova skripta zove se tijekom instalacije komponente koji se odvija samo jednom u njegova životnog ciklusa.


###<a id="scripting_hello_world"></a>Skriptiranje unutar tijeka rada: pozdrav svijeta

Povucite komponentu postavljanje upita skriptiranih dizajnera plošni i preimenujte je (na primjer, "SetClipListXML").

![Dodavanje skriptiranih komponente](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Dodavanje skriptiranih komponente*

Kada pregledavate svojstva komponentu postavljanje upita skriptiranih, četiri različite skripte vrste bit će prikazane, svakog konfigurirati različite skriptu.

![Svojstva skriptiranih komponente](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Svojstva skriptiranih komponente*

Isključite na processInputScript i otvorite uređivač za na realizeScript. Sada ćemo se postaviti i spremni ste za početak skriptiranje.

Su skripte pisane Groovy, dinamički kompilirane skriptnog jezika za platformu Java zadržava kompatibilnosti s Java. Zapravo, većina Java kod je valjan Groovy kod.

Pogledajmo napisati groovy skriptu svijeta jednostavne pozdrav u kontekstu naše realizeScript. U uređivaču unesite sljedeće:

    node.log("hello world");

Sada izvršiti lokalni test Pokreni. Nakon pokretanja, svojstvo zapisnika provjeri (putem kartica sustava u komponenti postavljanje upita skriptiranih).

![Pozdrav svijeta zapisnika Izlaz](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Pozdrav svijeta zapisnika Izlaz*

Objekt čvor ćemo način zapisnika poziva, odnosi naš trenutnog "čvora" ili komponentu smo si skriptiranje unutar. Svaka komponenta kao ima mogućnost zapisivanje podatke, dostupne putem kartice sustava. U ovom slučaju ne možemo izlaz slovni niz "Zdravo svijete". Važno je znati Evo to mogu biti moguće neprocjenjive alat za ispravljanje pogrešaka, što vam omogućuje uvid u koje skriptu zapravo radi.

Iz naših skriptiranja okruženja ćemo imati pristup svojstva na druge komponente. Isprobajte ovo:


    //inspect current node: 
    def nodepath = node.getNodePath(); 
    node.log("this node path: " + nodepath);
    
    //walking up to other nodes: 
    def parentnode = node.getParentNode(); 
    def parentnodepath = parentnode.getNodePath(); 
    node.log("parent node path: " + parentnodepath);
    
    //read properties from a node: 
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null ); 
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null); 
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

Naš prozor Evidencija prikazat će se nam sljedeće:

![Izlaz zapisnika za pristup čvor putova](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*Izlaz zapisnika za pristup čvor putova*


##<a id="frame_based_trim"></a>Skraćivanje utemeljen na okvir multibitrate MP4 izlaza

Pokretanje tijeka rada koje generira [multibitrate MP4 izlaza iz programa MXF za unos](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), ne možemo će sada gledate u skraćivanje izvora videozapisa koji se temelji na okvir broji.

###<a id="frame_based_trim_start"></a>Nacrt početi s dodavanjem skraćivanje za pregled

![Tijek rada pokrene skraćivanje za dodavanje](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Tijek rada pokrene skraćivanje za dodavanje*

###<a id="frame_based_trim_clip_list"></a>Rad s popisom isječak XML

U sve prethodne vodiče tijeka rada, koristi komponentu medijske datoteke unos kao naš izvor videozapisa unosa. Za određene scenarij kroz, ne možemo hoćete li koristiti komponentu isječak popisa izvor umjesto toga. Imajte na umu da to ne smije biti na željeni način rada; Popis izvora isječak koristiti samo kada je realni razlog da biste to učinili (kao u na ispod predmet, mjesto smo izrađujete mogućnosti skraćivanje popisa isječak).

Da biste se prebacili iz naše medijske datoteke unosa na popisu izvor isječak, povucite komponentu izvor popis isječaka na površini za dizajniranje i povežite pin XML popis isječak isječak popis XML čvor Pomozite poboljšati Office. To treba popunjavanje popisa izvor isječak s izlazni PIN-ove prema naš unos videozapisa. Sada se povezati s koje nisu komprimirane Video i audiosadržaji koje nisu komprimirane PIN-ove iz u isječak izvora popisa odgovarajući AVC koderi i Interleaver strujanje Audio. Sada možete ukloniti unos medijske datoteke.

![Zamjenjuje unos medijske datoteke izvora popisa isječka](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*Zamjenjuje unos medijske datoteke izvora popisa isječka*

Komponentu isječak popisa izvor uzima kao njegov unos "Isječak popis XML". Kada odaberete izvornu datoteku da biste testirali s lokalno, ovaj xml popis isječak automatski se ispunjava umjesto vas.

![Automatski ispunjava svojstvo XML popis isječka](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Automatski ispunjava svojstvo XML popis isječka*

Izgleda malo bliže xml, Evo kako izgleda:

![Dijaloški okvir popis isječak Uredi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Dijaloški okvir popis isječak Uredi*

To ipak ne odražava mogućnosti xml popis isječak. Jedna od mogućnosti imamo jest da biste dodali element "Trim" u odjeljku videosadržaj i audiosadržaj izvornoj, ovako:

![Dodavanje i trim element popis isječka](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Dodavanje i trim element popis isječka*

Ako izmijenite xml popis isječak ovako iznad i napraviti lokalni pokrenite test, vidjet ćete videozapis pravilno je obrezati od 10 do 20 sekundi u videozapisu.

Contrary to što se događa kada se kroz učinite lokalne cilja, ova vrlo istu xml cliplist bi imaju isti učinak primjenom u tijeku rada koji se izvodi u web-servisa Azure Media Services. Kada se pokrene Azure Premium Encoder, cliplist xml generirat će se svaki put kada ponovno na temelju Ulazna datoteka dobiven kodiranja posao. To znači da promjene moramo xml bi Nažalost moguće nadjačati.

Da biste brojač xml cliplist se brisanje kada je pokrenut kodiranja zadatak, ne možemo možete ponovno generirati ga u hodu neposredno nakon početka naš tijeka rada. Takve prilagođene akcije se može preuzeti putem pod nazivom "Postavljanje upita skriptiranih komponenta korisničkog sučelja". Dodatne informacije potražite u članku [Uvod u komponenti postavljanje upita skriptiranih](media-services-media-encoder-premium-workflow-tutorials.md#scripting).


Povucite komponentu postavljanje upita skriptiranih dizajnera plošni i preimenujte u "SetClipListXML".

![Dodavanje skriptiranih komponente](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Dodavanje skriptiranih komponente*

Kada pregledavate svojstava komponente postavljanje upita skriptiranih, četiri različite skripte vrste bit će se prikazati, svakog konfigurirati različite skriptu.

![Svojstva skriptiranih komponente](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Svojstva skriptiranih komponente*


###<a id="frame_based_trim_modify_clip_list"></a>Izmjena popisa isječak iz komponente za postavljanje upita skriptiranih

Prije nego što smo možete ponovno pisanje xml cliplist koje generira se prilikom pokretanja tijeka rada, ne možemo morat ćete imati pristup cliplist xml svojstava i sadržaj. Ne možemo možete učiniti ovako:

    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Dolazne isječak popis zapisuju](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Dolazne isječak popis zapisuju*

Najprije moramo način da biste utvrdili koji trenutka prije pomaka čemu želimo Obreži videozapis. Da biste to prikladno korisniku tehničku za manje tijeka rada, objavite dva svojstva korijenu na grafikonu. Da biste to učinili, desnom tipkom miša kliknite dizajnera plošni i odaberite "Dodaj svojstvo":

- Prvo svojstvo: "ClippingTimeStart" vrste: "TIMECODE"
- Amortizacija u drugoj svojstvo: "ClippingTimeEnd" vrste: "TIMECODE"

![Dodavanje dijaloški okvir svojstva za vrijeme početka isječak](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Dodavanje dijaloški okvir svojstva za vrijeme početka isječak*

![Objavljene isječak rekvizita za vrijeme u korijenu tijeka rada](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Objavljene isječak rekvizita za vrijeme u korijenu tijeka rada*

Konfiguriranje oba svojstva na odgovarajuću vrijednost:

![Konfiguriranje isječak početka i završetka svojstva](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Konfiguriranje isječak početka i završetka svojstva*

Sada u unutar naš skripte smo možete pristupiti i svojstva, ovako:

    
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Prozor Evidencija prikazuje početak i završetak isječak](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Prozor Evidencija prikazuje početak i završetak isječak*

Pogledajmo raščlaniti timecode nizove u je praktičnije za korištenje obrasca, pomoću jednostavne uobičajenog izraza:
    
    //parse the start timing: 
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1); 
    node.log("timecode start is: " + starttimecode); 
    def startframerate = startregresult.group(2); 
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend); 
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2); 
    node.log("framerate end is: " + endframerate);

![Prozor Evidencija s izlaz rastavljeni timecode](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Prozor Evidencija s izlaz rastavljeni timecode*

Pomoću tih informacija konkretnom ćemo sada promijenite xml cliplist u skladu s vizualnim vremena početka i završetka za željeni isječak točne okvira filma.

![Kodu skripte da biste dodali elemente obrezivanja](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Kodu skripte da biste dodali elemente obrezivanja*

To je učinjeno kroz normalni niz rukovanje operacija. Konačni xml popis izmijenjeni isječak zapisuje natrag svojstvo clipListXML na korijenskom tijeka rada metodom "setProperty". Prozor Evidencija nakon pokretanja Drugi test bi pokazuju sljedeće:

![Zapisivanje na nastalom popisu isječka](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Zapisivanje na nastalom popisu isječka*

Učinite na cilja Testiraj da biste vidjeli kako ste je skratio strujanja audiozapisa i videozapisa. Kao što podešava više test do cilja imaju različite vrijednosti za ograničavanje točke, primijetit ćete da oni neće se izbaciti račun no! Razlog za to je web-dizajnera, za razliku od Azure runtime neće promijeniti cliplist xml svakog pokretanja. To znači da se samo prvi put ste postavili u programu i odgovor točkama uzrokovat će xml pretvorba, sve ostale vrijeme, naš straža uvjet (Ako (clipListXML.indexOf ("<trim>") == -1)), onemogućuje tijek rada dodate drugi element trim kada postoji već postoji.

Da biste praktično da biste testirali lokalno naš tijeka rada, dodat ćemo najbolje neke kod kuće zadržavanje koji pregledava ako trim element već nalazila. Ako je tako, ne možemo ga ukloniti prije nastavka izmjenom xml s nove vrijednosti. Umjesto obične niz operacije je vjerojatno sigurnije da biste to učinili kroz realni xml objektni model analize.

Prije nego što smo možete dodati takve kod Premda smo morat ćete najprije dodajte broj naredbe Uvezi na početku naš skripte:
    
    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;

Nakon toga možete dodat ćemo potreban kod za čišćenje:

    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already: 
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes: 
    elementsToDelete.each{ 
        e -> e.getParentNode().removeChild(e);
    }; 
    node.log("deleted any existing trim nodes");
    
    //serialize the modified clip list xml dom into a string: 
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result); 
    clipListXML = result.getWriter().toString();
    
Kod dolazi neposredno iznad točke u kojoj dodat ćemo trim elemenata u cliplist xml.

Sada možemo pokrenuti i izmijenite naš tijeka rada kao koliko god želimo tijekom pojavljuju promjene primijeniti ikad vremena.    

###<a id="frame_based_trim_clippingenabled_prop"></a>Dodavanje praktičnost svojstvo ClippingEnabled

Kao što je možda ne uvijek želite da se dogodi obrezivanja, recimo Završi isključivanje naš tijeka rada dodavanjem praktičan Booleova zastavicu koji označava hoće li želimo da biste omogućili skraćivanje / isječak.

Kao što prije, objavite novo svojstvo korijen naš tijek rada pod nazivom "ClippingEnabled" od vrsta "Booleove vrijednosti".

![Objavljene svojstvo za omogućivanje isječak](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Objavljene svojstvo za omogućivanje isječak*

S na ispod jednostavne straža uvjet WHERE, možemo provjeri je li potreban je ograničenje i odlučiti ako našem popisu isječak kao potrebno izmijeniti ili ne.

    //check if clipping is required: 
    def clippingrequired = node.getProperty("../ClippingEnabled"); 
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML); 
        node.log("no clipping required"); 
        return; 
    }


###<a id="code"></a>Dovršavanje kod

    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;
    
    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping: 
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);
    
    //for local testing: delete any pre-existing trim elements 
    //from the clip list xml by parsing the xml into a DOM:
    
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder(); 
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath(); 
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }
    
    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e -> 
        e.getParentNode().removeChild(e); 
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return; 
    }

    //add trim elements to cliplist xml 
    if ( clipListXML.indexOf("<trim>") == -1 ) 
    {
        //trim video 
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode + 
            " </outPoint>\n </trim> \n"); 
        //trim audio 
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" + 
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML ); 
        node.setProperty("../clipListXml",clipListXML); 
    }


##<a name="also-see"></a>Vidi također 

[Uvod u Premium kodiranje Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Korištenje Premium kodiranje Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Kodiranje osvježavati sadržaja pomoću servisa Azure Media](media-services-encode-asset.md#media_encoder_premium_workflow)

[Media Encoder Premium tijeka rada oblici i kodeka](media-services-premium-workflow-encoder-formats.md)

[Ogledne datoteke tijeka rada](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Alat za Azure Media Services Explorer](http://aka.ms/amse)

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
