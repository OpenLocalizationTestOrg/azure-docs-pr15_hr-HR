<properties 
    pageTitle="Pregled Azure Media Services i uobičajeni scenariji | Microsoft Azure" 
    description="Ova tema sadrži pregled servisa Azure Media Services" 
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
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>

#<a name="azure-media-services-overview-and-common-scenarios"></a>Pregled Azure Media Services i uobičajeni scenariji

Microsoft Azure Media Services je extensible oblaku platforme koja omogućuje inženjerima omogućuje sastavljanje skalabilni media upravljanja i isporuke aplikacijama. Media Services temelji se na REST API-ji koje omogućuju sigurno prijenos, spremanje, kodiranje i pakiranje videoisječka ili zvučnog sadržaja na zahtjev i uživo strujanje isporuke različite klijentima (na primjer, TV, PC i mobilne uređaje).

Možete izrađivati tijekovi rada završetka do kraja pomoću potpuno Media Services. Možete odabrati i pomoću komponente drugih proizvođača za neki dijelovi tijeka rada. Na primjer, kodiranje pomoću drugih proizvođača kodiranje. Zatim, prijenos, zaštita, pakiranje, izlaganje pomoću Media Services.

Možete odabrati strujanje sadržaja uživo i izlaganje sadržaja na zahtjev. Ova tema prikazuje uobičajeni scenariji za izlaganja vaš [live](media-services-overview.md#live_scenarios) sadržaja ili [na zahtjev](media-services-overview.md#vod_scenarios). Teme i veze na druge odgovarajući teme.

## <a name="sdks-and-tools"></a>Alati i SDK-ovi

Da biste sastavili rješenja Media Services, možete koristiti:

- [Media Services REST API-JA](https://msdn.microsoft.com/library/azure/hh973617.aspx)
- Jedan od dostupnih klijent SDK-ovi:
- [Azure Media Services SDK za .NET](https://github.com/Azure/azure-sdk-for-media-services)
- [Azure SDK Java](https://github.com/Azure/azure-sdk-for-java)
- [Azure i SDK](https://github.com/Azure/azure-sdk-for-php)
- [Azure Media Services za Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Ovo nije proizvođača verziju Node.js SDK. Održava zajednice i trenutno imate na 100% opseg AMS API-ji).
- Alati za postojeće:
- [Portal za Azure](https://portal.azure.com/)
- [Azure-Media-servise-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) je na Winforms / C# aplikacija za Windows)

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

Možete pogledati AMS Tečajevi ovdje:

- [AMS Live strujanje tijeka rada](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
- [AMS na zahtjev strujanje tijeka rada](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

##<a name="prerequisites"></a>Preduvjeti

Da biste počeli koristiti web-servisa Azure Media Services, imat ćete sljedeće:

3. Račun Azure. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com).
2. Račun servisa Azure Media Services. Da biste stvorili račun servisa Azure Media Services pomoću portala za Azure, .NET ili REST API-JA. Dodatne informacije potražite u članku [Stvaranje računa](media-services-portal-create-account.md).
3. (Neobavezno) Postavljanje okruženja za razvoj. Odaberite .NET ili REST API-JA za razvojno okruženje. Dodatne informacije potražite u članku [Postavljanje okruženja](media-services-dotnet-how-to-use.md).

Osim toga, Saznajte kako se povezati programski [za povezivanje](media-services-dotnet-connect-programmatically.md).
4. (Preporučuje se) Dodijeliti jedan ili više skaliranje jedinice. Preporučuje se dodijeliti jednu ili više jedinica mjerilo za aplikacije u radnom okruženju.   Dodatne informacije potražite u članku [Upravljanje strujanje krajnje točke](media-services-portal-manage-streaming-endpoints.md).

##<a name="concepts-and-overview"></a>Koncepti i pregled

Pojmovi servisa Azure Media Services potražite u članku [koncepata](media-services-concepts.md).

S uputama niz koji predstavlja za sve glavne komponente servisa Azure Media Services potražite u članku [vodiči za Azure Media Services Step-by-Step](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Ovaj niz ima sjajan pregled koncepata i koristi alat za AMSE da bismo pokazali AMS zadataka. Imajte na umu da alat za AMSE alat za Windows. Ovaj alat podržava većinu zadataka možete postići programski [AMS SDK za .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK Java](https://github.com/Azure/azure-sdk-for-java)ili [Azure i SDK](https://github.com/Azure/azure-sdk-for-php).

##<a id="vod_scenarios"></a>Izlaganja medijskih sadržaja na zahtjev pomoću servisa Azure Media Services: uobičajeni scenariji i zadaci

U ovom se odjeljku opisuju uobičajeni scenariji i navode veze na odgovarajuću teme. Sljedeći dijagram prikazuje glavne dijelove platformu Media Services koje slijede u izlaganja sadržaja na zahtjev. 

![VoD tijeka rada](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)


###<a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Zaštita sadržaja u prostor za pohranu i izlaganje strujanja medijskih sadržaja u isključite (koji nisu šifrirane)

1. Prijenos datoteke visoke kvalitete mezzanine u sredstava.
    
    Preporučuje se da biste primijenili mogućnost šifriranja pohrane vaše resursa da bi se zaštiti sadržaja tijekom prijenosa i na ostale u prostor za pohranu.
 
1. Kodiranje skup prilagodljivo brzina prijenosa MP4 datoteke. 

    Preporučuje se da biste primijenili mogućnost šifriranja pohrane izlaz resursa da bi se zaštiti sadržaja na ostale.
    
1. Konfiguriranje pravila za isporuku resursa (koristi dinamički pakiranje). 
    
    Ako je vaš resursa za pohranu šifrirane, najprije **morate** konfigurirati pravila za isporuku resursa. 

1. Objavljivanje imovine stvaranjem programa locator zahtjev.

    Provjerite jeste li imati barem jedno strujanje rezervirane jedinice na strujanje krajnjoj točki iz kojeg želite strujanje sadržaj.

1. Strujanje objaviti sadržaj.

###<a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Zaštita sadržaja u odjeljku pohrana, isporuka dinamički šifrirane strujanja medijskih sadržaja  

Da biste mogli koristiti dinamičke šifriranje, najprije se najmanje jednu strujanje rezervirane jedinicu na strujanje krajnjoj točki iz kojeg želite strujanje šifrirane sadržaj.

1. Prijenos datoteke visoke kvalitete mezzanine u sredstava. Primjena mogućnost šifriranja pohrane sredstava.
1. Kodiranje skup prilagodljivo brzina prijenosa MP4 datoteke. Mogućnost šifriranja pohrane primjenjuju se na Izlaz resursa.
1. Stvaranje sadržaja ključa za šifriranje za resursa koje želite da se dinamički šifrirane tijekom reprodukcije.
2. Konfiguriranje pravila sadržaja autorizacije ključa.
1. Konfiguriranje pravila za isporuku resursa (koristi se dinamički pakiranje i dinamični šifriranje).
1. Objavljivanje imovine stvaranjem programa locator zahtjev.
1. Strujanje objaviti sadržaj. 

###<a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Korištenje analize medijskih sadržaja za dobivanje glavne s akcijama uvide iz videozapisa 

Analitički medijskih sadržaja je zbirka komponenata govora i vidom koje olakšavaju tvrtkama ili ustanovama i velike tvrtke za dobivanje glavne s akcijama uvida iz svoje videodatoteke. Dodatne informacije potražite u članku [Azure Media Services analize pregled](media-services-analytics-overview.md).

1. Prijenos datoteke visoke kvalitete mezzanine u sredstava.
2. Primijenite jednu od sljedećih analize medijskih sadržaja servisa za obradu videozapise:
    
    - **Indeksiranje** – [postupak videozapise s Azure Media indeksiranje 2](media-services-process-content-with-indexer2.md)
    - **Hyperlapse** – [Hyperlapse medijske datoteke s Hyperlapse Azure Media](media-services-hyperlapse-content.md)
    - **Otkrivanje kretanja** – [Kretanja otkrivanje za Azure Media analize](media-services-motion-detection.md).
    - **Otkrivanje nominalne i nominalne nalaze** – [nominalne i otkrivanje emocije Azure Media analitičkih](media-services-face-and-emotion-detection.md).
    - **Videozapis summarization** – [Korištenje Azure Media Video minijature Stvori videozapis Summarization](media-services-video-summarization.md)
3. Procesori media analize medijskih sadržaja proizvesti MP4 datoteke ili datoteke JSON. Ako media procesor proizvodi datoteku MP4, progresivno mogu preuzeti datoteku. Ako media procesor proizvodi JSON datoteke, možete preuzeti datoteku iz spremišta blobova platforme Azure. 


###<a name="deliver-progressive-download"></a>Izlaganje progresivno preuzimanje 

1. Prijenos datoteke visoke kvalitete mezzanine u sredstava.
1. Kodiranje u jednu datoteku MP4.
1. Objavljivanje imovine stvaranjem na zahtjev ili SAS locator.

    Ako koristite locator zahtjev, provjerite jeste li imati barem jedno strujanje rezervirane jedinice na krajnjoj strujanje iz koje namjeravate progresivno preuzimanje sadržaja.

    Ako koristite SAS locator, sadržaj će se preuzeti iz spremišta blobova platforme Azure. U ovom slučaju ne morate imati strujanje rezervirane jedinice.
  
1. Progresivno preuzeti sadržaj.

##<a id="live_scenarios"></a>Izlaganja uživo strujanje događaja pomoću servisa Azure Media Services

Prilikom rada s Live strujanje najčešće su povezane sljedeće komponente:

- Kamera koja se koristi za emitiranje događaja.
- Uživo videozapisa kodiranje koji pretvara signale s fotoaparata tokova koji se šalju uživo servis za strujanje.

Po želji više stvarnom vremenu sinkronizirati koderi. Za određene ključnih događaji uživo da vrlo visoka dostupnost zahtjev i kvalitetu sučelja, preporučuje se uključivanja aktivno aktivno suvišnih koderi s vremenom synchronizationto postigli objedinjenog prebacivanje bez gubitka podataka.
- Uživo strujanje servis koji vam omogućuje da učinite sljedeće:

- ingest uživo sadržaja pomoću različitih uživo protokola za strujanje (na primjer RTMP ili Smooth Streaming)
- (nije obavezno) kodiranje vaše strujanje u strujanje prilagodljivo brzina prijenosa
- Pretpregled aktivno strujanje
- snimanje i pohranu ingested sadržaja da bi se strujanjem kasnije (Video-on-Demand)
- izlaganje sadržaja putem uobičajenih protokola za strujanje (Ako, na primjer, MPEG CRTICE, bila glatka, HLS, HDS) izravno klijentima ili da biste na sadržaj isporuke mreže (CDN) za daljnje distribuciju.


**Microsoft Azure Media Services** (AMS) omogućuje ingest kodiranje, pretpregled, spremanje i izlaganje uživo strujanja medijskih sadržaja.

Tijekom izlaganja vaš sadržaj klijentima izlaganje visoke kvalitete videozapisa s različitim uređajima pod uvjetima drugog mrežnog je vaš cilj. Da biste voditi brigu o kvalitete i mrežne uvjeta, pomoću uživo koderi kodiranje vaše strujanje za strujanje videozapisa s više – brzina prijenosa (prilagodljivo brzina prijenosa).  Da biste voditi brigu o strujanje na različitim uređajima, pomoću Media Services [dinamički pakiranje](media-services-dynamic-packaging-overview.md) dinamički ponovno paketa nizu različite protokola. Media Services podržava isporuku sljedeće prilagodljivo brzina prijenosa strujanje tehnologije: HTTP Live strujanje (HLS), Smooth Streaming, MPEG CRTICE i HDS (za PrimeTime pristup Adobe licensees samo).

U servisa Azure Media Services **kanala**, **programa**i **StreamingEndpoints** ručicu za sve u uživo strujanje radovi ingest, uključujući oblikovanje DVR, sigurnost, skalabilnost i zalihosti.

**Kanal** predstavlja kanal za obradu uživo strujanja medijskih sadržaja. Kanal možete primati u uživo unos strujanja na sljedeće načine:

- Lokalnog live encoder šalje višestruki-brzina prijenosa **RTMP** ili **Smooth Streaming** (fragmentirane MP4) kanal za koji je konfiguriran za isporuku **prolazni** . **Prolazni** isporuke je kada ingested strujanja proći kroz **kanal**s bez daljnje obrade. Možete koristiti sljedeće uživo koderi izlaz više – brzina prijenosa Smooth Streaming: Elemental, Envivio, Cisco.  Sljedeće uživo koderi izlaz RTMP: Adobe Flash uživo, Telestream Wirecast i Tricaster transcoders.  Uživo kodiranje možete poslati i jedan brzina prijenosa strujanje kanala koji nije omogućen za šifriranje uživo, ali koje se ne preporučuje. Kada ste tražili, Media Services nudi toka klijentima.

>[AZURE.NOTE] Prolazni metodom je najčešće najekonomičniji način live strujanje kada rade više događaje dugo vremenskom razdoblju te ste već uložiti u lokalni koderi. Pogledajte detalje [cijene](/pricing/details/media-services/) .

- Kodiranje uživo lokalnog šalje jedne-brzina prijenosa strujanje kanal koji je omogućen za izvođenje uživo kodiranje s Media Services u jednom od sljedećih oblika: RTP (MPEG-TS), RTMP ili Smooth Streaming (fragmentirane MP4). Kanal zatim izvodi uživo kodiranje na dolazni strujanje jedan brzina prijenosa za strujanje videozapisa višestruki-brzina prijenosa (prilagodljivo). Kada ste tražili, Media Services nudi toka klijentima.


###<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Rad s kanale koje primate višestruki-brzina prijenosa uživo strujanje iz lokalnog koderi (prolazni)

Sljedeći dijagram prikazuje glavne dijelove platformu AMS koje slijede **prolazni** tijeka rada.

![Uživo tijeka rada][live-overview2]

Dodatne informacije potražite u članku [Rad s kanala te primanje višestruki-brzina prijenosa uživo strujanje iz lokalnog koderi](media-services-live-streaming-with-onprem-encoders.md).

###<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Rad s kanalima za koje je omogućena za izvođenje uživo kodiranje pomoću servisa Azure Media Services

Sljedeći dijagram prikazuje glavne dijelove platformu AMS koje slijede u Live strujanje tijeka rada kojima je omogućeno kanala da biste izvršili uživo kodiranje s Media Services.

![Uživo tijeka rada][live-overview1]

Dodatne informacije potražite u članku [Rad s kanala koji su omogućeni izvođenje Live šifriranje pomoću servisa Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).


##<a name="consuming-content"></a>Dosta sadržaja

Azure Media Services nudi alate potrebnih za stvaranje obogaćenih, dinamični klijentske aplikacije reproduktora za većinu platforme uključujući: iOS uređaja, uređaje sa sustavom Android, Windows, Windows Phone, Xbox i postavljanje gornjeg okvira. Sljedeće teme navode veze na SDK-ovi i okviri reproduktora koje možete koristiti za razvoj vlastite klijentske aplikacije koje mogu zauzeti strujanja medijskih sadržaja iz Media Services.

[Razvoj aplikacija reproduktora videosadržaja](media-services-develop-video-players.md)

##<a name="enabling-azure-cdn"></a>Omogućivanje Azure CDN

Media Services podržava Integracija s Azure CDN. Informacije o omogućivanju Azure CDN potražite [u](media-services-portal-manage-streaming-endpoints.md)članku Upravljanje strujanje točke u računa za servise medijske sadržaje.

##<a name="scaling-a-media-services-account"></a>Promjena veličine računa za servise za medijske sadržaje

**Media Services** mogu mijenjati veličinu tako da navedete broj **Strujanje rezervirane jedinice** i **Kodiranje rezervirane jedinice** koje želite da se tamo uz vaš račun.

Vaš račun Media Services možete skaliranja i dodavanjem računa za pohranu. Svaki račun za pohranu ograničeno je na 500 TB. Da biste proširili prostora za pohranu izvan ograničenja zadane, možete odabrati da biste priložili više prostora za pohranu računa jednog računa za servise za medijske sadržaje.

[Ova](media-services-portal-scale-streaming-endpoints.md) tema veze na odgovarajuću teme.

##<a name="support"></a>Podrška

[Podrška za Azure](https://azure.microsoft.com/support/options/) pruža mogućnosti podrške za Azure, uključujući Media Services.

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="service-level-agreement-sla"></a>Ugovor o razini usluge (SLA)

- Za Media Services kodiranje, ne možemo jamči 99.9% dostupnost transakcija REST API-JA.
- Za strujeće, ne možemo će uspješno zahtjevi za uslugu uz jamstvo dostupnost 99.9% za postojeće medijskog sadržaja kada je kupljena najmanje jednu strujanje jedinicu.
- Za Live kanali, ne možemo jamči da radi kanale će imati povezivanje vanjskih barem 99.9% od vremena.
- Za zaštitu sadržaja, ne možemo jamči da ne možemo će uspješno ispunjavanje zahtjeva za ključne najmanje 99.9% od vremena.
- Za indeksiranje, ne možemo će uspješno indeksiranje zadatka zahtjevi za uslugu obrađuju pomoću programa Reserved kodiranje % 99.9 jedinica vremena.

Dodatne informacije potražite u članku [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png
 
