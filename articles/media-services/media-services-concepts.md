<properties 
    pageTitle="Azure Media Services koncepata | Microsoft Azure" 
    description="Ova tema sadrži pregled Azure Media Services koncepti" 
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

#<a name="azure-media-services-concepts"></a>Azure Media Services koncepti 

Ova tema sadrži pregled najvažnije koncepte Media Services.

##<a id="assets"></a>Resursi i prostora za pohranu

###<a name="assets"></a>Resursi

[Resursa](https://msdn.microsoft.com/library/azure/hh974277.aspx) sadrži digitalne datoteke (uključujući videozapis, zvuk, slike, minijatura zbirke, prati tekst i titlova datoteka) i metapodataka o tim datotekama. Nakon digitalnog datoteke prenose se u sredstvo, ne može se koristiti u Media Services kodiranja i strujanje tijekova rada.

Sredstva mapirani spremniku blob u račun za Azure prostora za pohranu i datoteke imovine spremaju se kao BLOB-ova u njih.

Pri odlučivanju koji medijskog sadržaja da biste prenijeli i pohraniti u sredstvo, primjenjuju se Imajte na umu sljedeće:

- Sredstva smiju sadržavati samo jednu, jedinstveno instancu medijskog sadržaja. Ako, na primjer, u jednom Uredi TV epizode, film ili oglašavanje.
- Sredstva ne smiju sadržavati više vizualizacija ili uređivanja audiovisual datoteke. Primjer nepravilnom upotrebe sredstava bi pokušaj pohrana više epizoda TV, oglašavanje ili više kamera kutova iz jedne proizvodnje unutar sredstava. Spremanje više vizualizacija ili uređivanja audiovisual datoteke u sredstvo može uzrokovati poteškoće slanje kodiranja zadataka, strujanje i zaštita isporuku resursa u nastavku tijeka rada.  

###<a name="asset-file"></a>Datoteka resursa 
[AssetFile](https://msdn.microsoft.com/library/azure/hh974275.aspx) predstavlja stvarni videoisječka ili zvučnog datoteku koja je pohranjena u spremniku blob. Datoteku resursa uvijek je povezan s sredstvo, a sredstvo možda sadrži jednu ili više datoteka. Zadatak Media Encoder usluge neće uspjeti ako je objekt datoteka resursa nije povezan s digitalnih datoteka u spremniku blob.

**AssetFile** instance i stvarne medijske datoteke su dva različita objekta. AssetFile instance sadrži metapodatke o medijske datoteke dok medijske datoteke sadrži stvarni medijskog sadržaja.

Biste trebali biste uspjeli da biste promijenili sadržaj blob spremnika koje je sastavljao Media Services bez korištenja API servisa za medijske sadržaje.

###<a name="asset-encryption-options"></a>Mogućnosti šifriranja resursa

Ovisno o vrsti sadržaja koji želite prenijeti, pohranjivanje i održavanje, Media Services nudi razne mogućnosti šifriranja koje možete odabrati.

**Ništa** Koristi se bez šifriranje. To je zadana vrijednost. Imajte na umu da koristite tu mogućnost sadržaja nije zaštićena na putu ili na ostale u prostor za pohranu.

Ako namjeravate održati programa MP4 pomoću progresivno preuzimanje, koristite tu mogućnost da biste prenijeli sadržaj.

**StorageEncrypted** – koristite ovu mogućnost da biste šifrirali Očisti sadržaj lokalno koristi AES 256 bitno šifriranje, a zatim prenese Azure za pohranu gdje je pohranjena šifriraju na ostale. Resursi koji su zaštićeni šifriranjem prostora za pohranu se automatski nešifrirani smještene u sustavu šifriranu datoteku prije no što kodiranja i po želji se ponovno šifrirane prije no što prenesete natrag kao novi izlaz resursa. Slučaj prvenstveno se koristi za pohranu šifriranje je kada želite sigurne visoke kvalitete unos medijske datoteke s šifriranjem na ostale na disku. 

Da bi se isporučiti resursa za šifriranu prostora za pohranu, morate konfigurirati sredstva isporuke pravila da bi Media Services znali način za isporuku sadržaja. Prije moguće je strujanjem vaše resursa, strujanje poslužitelja uklanja šifriranje prostora za pohranu i strujanja sadržaja putem pravila navedeni isporuke (na primjer, AES, PlayReady ili nema šifriranja). 

**CommonEncryptionProtected** – korištenje tu mogućnost ako želite šifrirati (ili prijenos već šifrirane) sadržaja s uobičajenih šifriranje ili PlayReady DRM (na primjer, izglađenim strujanje zaštićen PlayReady DRM).

**EnvelopeEncryptionProtected** – korištenje tu mogućnost ako želite da biste zaštitili (ili prijenos već zaštićen) HTTP Live strujanje (HLS) šifrirane s Napredno šifriranje standardne (AES). Imajte na umu da prenosite HLS već šifrirane i zaštićene AES, on mora šifrirane Upravitelj pretvaranje.

###<a name="access-policy"></a>Pravilnik o pristupu 

[AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx) sredstva definira dozvole (kao što je čitanje, pisanje i popis) i trajanje programa access. Obično bi prenesite objekta AccessPolicy locator koji će se koristiti za pristup datotekama koje se nalaze u sredstava.


###<a name="blob-container"></a>Spremnik blobova platforme

Spremnik blob nudi grupiranja skupa blob-ova. Blob spremnika se koriste u Media Services kao točku granicu za kontrolu pristupa i locators zajednički pristup potpis (SAS) na imovinu. Račun za Azure pohranu može sadržavati neograničen broj blob spremnika. Spremnik možete spremiti neograničen broj blob-ova.

>[AZURE.NOTE]Biste trebali biste uspjeli da biste promijenili sadržaj blob spremnika koje je sastavljao Media Services bez korištenja API servisa za medijske sadržaje.

###<a id="locators"></a>Locators

[Locator](https://msdn.microsoft.com/library/azure/hh974308.aspx)s pružaju ulaza da biste pristupili datotekama koje se nalaze u sredstava. Pravilo programa access koristi se za definiranje dozvole i trajanje da klijent ima pristup određenom resursa. Locators može imati više jedan odnos s pravilima programa access tako da drugi locators pružaju različite vremena i vrsta veze različite klijentima dok sve koriste iste dozvole i trajanje postavke; Međutim, zbog ograničenje zajednički pristup pravila odredio servisa Azure prostora za pohranu, ne možete imati više od pet jedinstveni locators odjednom povezan s određenom resursa. 

Podržava Media Services dvije vrste locators: locators OnDemandOrigin strujanja medijskih sadržaja (na primjer, MPEG CRTICE, HLS ili Smooth Streaming) ili progresivno preuzeli mediji i locators SAS URL za prijenos ili preuzimanje medijskih datoteka to\from Azure prostora za pohranu. 

Imajte na umu da popis dozvola (AccessPermissions.List) ne treba koristiti pri stvaranju programa locator OrDemandOrigin. 

###<a name="storage-account"></a>Račun za pohranu

Sve pristup pohrani Azure obavlja putem računa za pohranu. Račun za servis Media možete pridružiti jedan ili više prostora za pohranu računa. Račun može sadržavati neograničen broj spremnika, pod uvjetom da im ukupnu veličinu je u odjeljku 500TB po račun za pohranu.  Media Services nudi SDK razine tooling omogućuje vam upravljajte s više računa za pohranu i učitavanje saldo distribucija imovine tijekom prijenosa poslovne subjekte na temelju metriku ili izravnim distribuciju. Dodatne informacije potražite u članku Rad s [Azure prostora za pohranu](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

##<a name="jobs-and-tasks"></a>Zadataka i zadataka

[Zadatak](https://msdn.microsoft.com/library/azure/hh974289.aspx) obično se koristi u tijeku (na primjer, indeks ili kodiranje) jedne prezentacije audio/video. Ako su obrade više videozapisa, stvorite zadatak za svaki videozapis da biste kodirati.

Posao sadrži metapodatke o obrada koje je potrebno izvršiti. Svaki zadatak sadrži jednu ili više [zadataka](https://msdn.microsoft.com/library/azure/hh974286.aspx)s navedete zadatka atomske obrada, unos imovine, izlazna resursi, medijske sadržaje procesora i povezane postavke. Zadatke unutar posla mogu biti povezane zajedno, gdje zadan resursa izlaz od jedan zadatak kao unos resursa na sljedeći zadatak. Na taj način jedan zadatak mogu sadržavati sve potrebne za medijske sadržaje prezentaciju obrada.

##<a id="encoding"></a>Kodiranje

Azure Media Services pruža više mogućnosti za kodiranje medijskih sadržaja u oblak.

Kada počevši Media Services, važno je razlika između oblika kodecima i datoteke.
Kodeci su softver koji implementira algoritama za sažimanje/dekompresiju dok su formati spremnika koji sadrže Komprimirana videozapis.

Media Services nudi dinamički pakiranje koja vam omogućuje da izlaganje prilagodljivo brzina prijenosa MP4 ili Smooth Streaming kodirani sadržaj u strujanje oblici koje podržava Media Services (MPEG CRTICE, HLS, Smooth Streaming, HDS) bez potrebe za ponovno pakiranje u te strujanje oblici.

Da biste iskoristili [dinamički pakiranje](media-services-dynamic-packaging-overview.md), morate učinite sljedeće:

- Kodiranje datoteku mezzanine (izvor) u skupu prilagodljivo brzina prijenosa MP4 datoteke ili Smooth Streaming prilagodljivo brzina prijenosa datoteke (kodiranja korake su što je prikazano u nastavku ovog praktičnog vodiča).
- Pronađite najmanje jednu jedinicu strujanje na zahtjev za strujanje krajnju točku iz koje namjeravate isporuku sadržaja. Dodatne informacije potražite [u](media-services-portal-manage-streaming-endpoints.md)članku skaliranje na zahtjev strujanje rezervirane jedinice.

Media Services podržava sljedeće na zahtjev koderi koji su opisani u ovom članku:

- [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
- [Tijek rada za Media Encoder Premium](media-services-encode-asset.md#media-encoder-premium-workflow)

Informacije o podržanim koderi potražite u članku [koderi](media-services-encode-asset.md).


##<a name="live-streaming"></a>Live strujanje

U servisa Azure Media Services kanalom predstavlja kanal za obradu uživo strujanja medijskih sadržaja. Kanal prima uživo unos strujanja na jedan od dva načina:

- Kodiranje uživo lokalnog šalje RTMP višestruko-brzina prijenosa ili Smooth Streaming (fragmentirane MP4) kanal. Možete koristiti sljedeće uživo koderi izlaz više – brzina prijenosa Smooth Streaming: Elemental, Envivio, Cisco. Sljedeće uživo koderi izlaz RTMP: Adobe Flash uživo, Telestream Wirecast i Tricaster transcoders. Bez daljnje obrade ingested strujanja prenesite putem kanala. Kada ste tražili, Media Services nudi toka klijentima.

- Jedan brzina prijenosa strujanje (u jednom od sljedećih oblika: RTP (MPEG-TS)), RTMP ili Smooth Streaming (fragmentirane MP4)) se šalje na kanal koji je omogućen za izvođenje uživo kodiranje s Media Services. Kanal zatim izvodi uživo kodiranje na dolazni strujanje jedan brzina prijenosa za strujanje videozapisa višestruki-brzina prijenosa (prilagodljivo). Kada ste tražili, Media Services nudi toka klijentima.

###<a name="channel"></a>Kanal

U Media Services s [kanala](https://msdn.microsoft.com/library/azure/dn783458.aspx)ste odgovorni za obradu uživo strujanja medijskih sadržaja. Kanal nudi krajnje točke unosa (ingest URL-a) koju zatim omogućujete da biste uživo transcoder. Kanal prima uživo unos strujanja iz uživo transcoder i on postaje dostupan za strujanje putem jednog ili više StreamingEndpoints. Kanali pružaju pregled krajnje (Pretpregled URL-a) koju koristite za pretpregled i provjeriti vaše tok prije daljnje obrade i isporuke.

Kada stvorite kanal možete dobiti ingest URL-a i URL za pretpregled. Da biste dobili URL-ove, kanal nema u stanje rada. Kada ste spremni za početak odaberite podatke iz uživo transcoder u kanal, kanal mora biti pokrenut. Kada uživo transcoder pokrene ingesting podataka, možete pretpregledati vaše strujanje.

Svaki račun Media Services može sadržavati više kanala, videopoziva i više StreamingEndpoints. Ovisno o potrebama propusnost i sigurnost servisa StreamingEndpoint možete trakom za jednu ili više kanala. Bilo koji StreamingEndpoint možete uvesti iz bilo kojeg kanala.


###<a name="program"></a>Program

[Program](https://msdn.microsoft.com/library/azure/dn783463.aspx) omogućuje vam da biste odredili objavljivanje i prostora za pohranu segmente uživo strujanje. Kanala upravljanje programima. Odnos kanal i Program vrlo je sličan tradicionalni media gdje kanalom sadrži konstante strujanje sadržaja i programa implementaciju ograničen je na vremenskim događaju na taj kanal.
Možete navesti broj sati koje želite zadržati snimljeni sadržaj za program tako da svojstvo **ArchiveWindowLength** . Tu vrijednost postavite od najmanje pet minuta najviše 25 sati.

ArchiveWindowLength određuje i maksimalnu količinu vremena klijente možete traženje unatrag u vremenu od trenutnog mjesta uživo. Programi jer se može pokrenuti putem određeno vrijeme, ali sadržaja koja se nalazi iza prozora duljine neprestano odbacuju. Ta vrijednost ovo svojstvo određuje i koliko manifesti klijenta možete povećavaju jedan za drugim.

Svaki program povezan je s sredstava. Da biste objavili program stvarate locator povezane stavke. Imate ovaj locator će vam omogućiti da biste sastavili strujanje URL koje nude klijentima.

Kanal podržava tri istovremeno pokrenute programe tako da možete stvoriti više arhive isti dolazne toka. Time objaviti, a zatim arhiviranje različite dijelove događaja po potrebi. Na primjer, svojim potrebama tvrtke je za arhiviranje 6 sati programa, ali emitirati samo posljednjih 10 minuta. Da biste to postigli, morate stvoriti dva istovremeno pokrenute programe. Jedan program postavljena za arhiviranje 6 sati događaja, ali program nisu objavljene. U drugi program postavljeno za arhiviranje 10 minuta, a ovaj program je objavljen.


Dodatne informacije potražite u članku:

- [Rad s kanalima za koje je omogućeno izvođenje Live kodiranje pomoću servisa Azure Media Services](media-services-manage-live-encoder-enabled-channels.md)
- [Rad s kanale koje primate višestruki-brzina prijenosa uživo strujanje iz lokalnog koderi](media-services-live-streaming-with-onprem-encoders.md)
- [Kvota i ograničenja](media-services-quotas-and-limitations.md).

##<a name="protecting-content"></a>Zaštita sadržaja

###<a name="dynamic-encryption"></a>Dinamični šifriranje

Azure Media Services omogućuje sigurne medijskih sadržaja iz vrijeme ostavlja računalu putem prostora za pohranu, obrada i isporuke. Media Services omogućuje vam za isporuku sadržaja dinamički šifrirane i zaštićene Napredno šifriranje standardne (AES) (pomoću tipke 128-bitno šifriranje) i uobičajenih šifriranje (CENC) pomoću PlayReady i/ili Widevine DRM. Media Services i omogućuje uslugu za izlaganja AES tipke i licence PlayReady ovlašteni klijente.

Trenutno šifriranje sljedećih strujanje oblika: HLS MPEG CRTICE te Smooth Streaming. Ne možete šifrirati HDS streaming format ili Progresivni preuzimanja.

Ako želite Media Services da biste šifrirali sredstvo, morate pridružiti ključa za šifriranje (CommonEncryption ili EnvelopeEncryption) na resursa i konfigurirati pravila za provjeru autentičnosti tipke.

Ako želite strujanje resursa za šifriranu prostora za pohranu, morate konfigurirati sredstva isporuke pravila da bi se odredite kako izlaganja vaš resursa.

Strujanje primitku tako da je reproduktor Media Services koristi navedeni ključ za dinamično Šifriraj sadržaj pomoću omotnica šifriranja (s AES) ili uobičajenih šifriranja (PlayReady ili Widevine). Dešifrirati toka reproduktora će zatražiti tipku s servis za isporuku ključa. Odlučiti hoće li se korisnik ovlašteni da biste dobili ključ, servis vrednuje pravila za provjeru autentičnosti koji ste naveli za ključ.


###<a name="token-restriction"></a>Ograničenja tokena

Pravila za sadržaja ključa autorizacije može imati jednu ili više autorizacije ograničenja: otvaranje, token ograničenja ili IP ograničenja. Znak izdan tako da na sigurne tokena servisa (STS) mora biti praćeni tokena ograničena pravila. Media Services ne podržava tokeni u na jednostavne tokeni Web (SWT) i oblika JSON Web tokena (JWT). Media Services ne nudi tokena servisa za sigurnu. Možete stvoriti prilagođeni STS ili pod utjecajem Microsoft Azure ACS da biste tokeni problem. Na STS mora biti konfigurirano za stvaranje tokena prijavljeni pomoću navedeni ključ i problem zahtjevima koji ste naveli u konfiguraciji tokena ograničenja. Servis za ključne isporuku Media Services će vratiti tražene ključ (ili više licenci) klijent Ako vrijedi token i zahtjevima u tokena podudaranje one konfiguriran za ključ (ili licence).

Konfiguriranje token ograničeno pravilnika, morate navesti potvrdu primarni ključ, izdavača i publike parametre. Provjera primarni ključ sadrži ključ koji je potpisan token, izdavač je sigurna servis tokena koje izdaje token. Ciljne skupine (ponekad se zove i opseg) opisuje svrhu tokena ili resursa token neadministratorskog pristup. Servis za ključne isporuku Media Services Provjeri valjanost da te vrijednosti u token podudarati s vrijednostima u predlošku.

Dodatne informacije potražite u sljedećim člancima:

[Zaštita pregled sadržaja](media-services-content-protection-overview.md)
[Zaštita s AES 128](media-services-protect-with-aes128.md)
[Zaštita s DRM](media-services-protect-with-drm.md)

##<a name="delivering"></a>Izlaganja

###<a id="dynamic_packaging"></a>Dinamični pakiranje

Prilikom rada s Media Services preporučuje se kodiranje mezzanine datoteka u prilagodljivo brzina prijenosa MP4 postavite, a zatim skup pretvorili u željeni oblik pomoću [Dinamičke pakiranju](media-services-dynamic-packaging-overview.md).


###<a name="streaming-endpoint"></a>Strujanje krajnje točke

Na StreamingEndpoint predstavlja servis za strujanje koja možete ispuniti sadržajem izravno u aplikaciju za reprodukciju klijenta ili da biste na sadržaj isporuke mreže (CDN) za daljnje distribuciju (servisa Azure Media Services sada omogućuje integraciju sa servisom Azure CDN.) Izlazni toka iz servisa StreamingEndpoint može biti aktivno strujanje ili videozapis na zahtjev resursa na vašem računu Media Services. Osim toga, možete kontrolirati kapacitet servis StreamingEndpoint učiniti sve veći propusnosti potrebama prilagodbom skaliranje jedinice (poznat i kao strujanje jedinice). Preporučuje se dodijeliti jednu ili više jedinica mjerilo za aplikacije u radnom okruženju. Skaliranje jedinice vam ponuditi namjenski izlazne kapacitet koji je moguće kupiti u koracima od 200 MB/s i dodatnim funkcijama koje trenutno pripada dinamički pakiranja za korištenje.

Preporučuje se da biste koristili dinamički pakiranje and\or dinamički šifriranje. Da biste koristili te značajke, morate imati barem jedan strujanje jedinica za krajnju točku iz koje namjeravate strujanje. Dodatne informacije potražite u članku [Promjena veličine strujanje jedinice](media-services-portal-manage-streaming-endpoints.md).

Po zadanom može imati do 2 strujanje krajnje točke na vašem računu Media Services. Da biste zatražili veća ograničenja, potražite u članku [kvotama i ograničenja](media-services-quotas-and-limitations.md).

Samo se naplaćuju kada vaš StreamingEndpoint se izvodi stanje.

###<a name="asset-delivery-policy"></a>Pravila za isporuku resursa

Neki od koraka u tijeku rada za isporuku sadržaja Media Services konfigurira [isporuke pravila za resursi ](https://msdn.microsoft.com/library/azure/dn799055.aspx)koji želite da se strujanjem. Pravilnik za isporuku resursa govori Media Services način za svoje resursa za isporuku: u koje strujanje protokol treba vaše resursa dinamički pakirati (na primjer, MPEG CRTICE, HLS, Smooth Streaming ili sve), hoće li želite dinamički šifriranje vaše resursa i kako (omotnice ili uobičajenih šifriranje).

Ako sredstva šifriranim prostora za pohranu, prije vaše resursa možete strujanjem, strujanje poslužitelja uklanja šifriranje prostora za pohranu i strujanja putem pravila za isporuku navedeni sadržaj. Ako, na primjer, prilikom izlaganja vaš resursa šifrirane ključem za šifriranje Napredno šifriranje standardne (AES), postaviti vrstu pravila DynamicEnvelopeEncryption. Da biste uklonili šifriranje prostora za pohranu i strujanje resursa u isključite, postaviti vrstu pravila na NoDynamicEncryption.

###<a name="progressive-download"></a>Progresivno preuzimanje

Progresivno preuzimanje omogućuje vam da biste pokrenuli reprodukcijom medijskih sadržaja prije preuzeta cijelu datoteku. Samo progresivno možete preuzeti datoteku MP4.

Imajte na umu da morate dešifrirati sredstva šifriranim ako želite da se za njih da bi bio dostupan za progresivno preuzimanje.

Da biste korisnicima omogućili progresivno preuzimanje URL-ovi, najprije morate stvoriti programa locator OnDemandOrigin. Stvaranje Lokator omogućuje vam osnovni put sredstava. Zatim morate dodati naziv MP4 datoteke. Ako, na primjer:

http://amstest1.Streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.MP4

###<a name="streaming-urls"></a>Strujanje URL-ova

Strujanje sadržaj tako da klijenti. Da biste korisnicima omogućili strujanje URL-ovi, najprije morate stvoriti programa locator OnDemandOrigin. Stvaranje Lokator omogućuje vam osnovni put resursa koji sadrži sadržaj koji želite strujanje. Međutim, da biste mogli strujanje sadržaja ćete morati izmijeniti ovaj put. Sastavljanje cijeli URL strujanje manifesta datoteku, morate concatenate na locator put vrijednost i manifest (filename.ism) naziv datoteke. Nakon toga dodati /Manifest i odgovarajući oblik (Ako je potrebno) locator put.

Sadržaj možete strujanje i putem SSL veze. Da biste to učinili, provjerite je li strujanje URL-ovi počinju HTTPS.

Imajte na umu da koristite samo strujanja putem SSL ako strujanje krajnjoj točki iz kojeg izlaganja sadržaj stvoren nakon 10tu. rujan 2014.. Ako strujanje URL-ovi temelje se na strujanje krajnje točke stvorene nakon rujan 10tu, URL sadrži "streaming.mediaservices.windows.net" (u novi oblik). Strujanje URL-ovi koji sadrže "origin.mediaservices.windows.net" (stari oblik) ne podržavaju SSL. Ako je URL u stari oblik, a želite omogućiti strujanje SSL, stvorili krajnju točku strujanje. Pomoću URL-ova stvara na temelju krajnju strujanje strujanje sadržaja putem SSL.

Na popisu u nastavku opisuje različite oblike za strujanje te se iznosi primjera:

- Smooth Streaming

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/manifest


- MPEG CRTICA

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/manifest(format=mpd-time-CSF)



- Apple HTTP Live strujanje V4 (HLS)

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/manifest(format=m3u8-aapl)



- Apple HTTP Live strujanje V3 (HLS)

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/manifest(format=m3u8-aapl-v3)

- HDS (za PrimeTime pristup Adobe licensees samo)

{strujanje krajnjoj točki naziv media services račun name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/manifest(format=f4m-f4f)


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
