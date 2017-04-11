<properties 
    pageTitle="Pregled uživo strujanja pomoću servisa Azure Media Services | Microsoft Azure" 
    description="Ova tema sadrži Pregled uživo strujanja pomoću servisa Azure Media Services." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="juliako"/>

#<a name="overview-of-live-steaming-using-azure-media-services"></a>Pregled uživo strujanja pomoću servisa Azure Media Services

##<a name="overview"></a>Pregled

Tijekom izlaganja uživo strujanje događaja pomoću servisa Azure Media Services najčešće su povezane sljedeće komponente:

- Kamera koja se koristi za emitiranje događaja.
- Uživo videozapisa kodiranje koji pretvara signale s fotoaparata tokova koji se šalju uživo servis za strujanje.

    Po želji više stvarnom vremenu sinkronizirati koderi. Za određene ključnih događaji uživo da vrlo visoka dostupnost zahtjev i kvalitetu sučelja, preporučuje se uključivanja aktivno aktivno suvišnih koderi sa sinkroniziranjem vrijeme da biste postigli objedinjenog prebacivanje bez gubitka podataka.
- Uživo strujanje servis koji vam omogućuje da učinite sljedeće:
    
    - ingest uživo sadržaja pomoću različitih uživo protokola za strujanje (na primjer RTMP ili Smooth Streaming)
    - (nije obavezno) kodiranje vaše strujanje u strujanje prilagodljivo brzina prijenosa
    - Pretpregled aktivno strujanje
    - snimanje i pohranu ingested sadržaja da bi se strujanjem kasnije (Video-on-Demand)
    - izlaganje sadržaja putem uobičajenih protokola za strujanje (Ako, na primjer, MPEG CRTICE, bila glatka, HLS, HDS) izravno klijentima ili da biste na sadržaj isporuke mreže (CDN) za daljnje distribuciju.


**Microsoft Azure Media Services** (AMS) omogućuje ingest kodiranje, pretpregled, spremanje i izlaganje uživo strujanja medijskih sadržaja.

Tijekom izlaganja vaš sadržaj klijentima izlaganje visoke kvalitete videozapisa s različitim uređajima pod uvjetima drugog mrežnog je vaš cilj. Da biste to postigli, pomoću uživo koderi kodiranje vaše strujanje za strujanje videozapisa višestruki-brzina prijenosa (prilagodljivo brzina prijenosa).  Da biste voditi brigu o strujanje na različitim uređajima, pomoću Media Services [dinamički pakiranje](media-services-dynamic-packaging-overview.md) dinamički ponovno paketa nizu različite protokola. Media Services podržava isporuku sljedeće prilagodljivo brzina prijenosa strujanje tehnologije: HTTP Live strujanje (HLS), Smooth Streaming, MPEG CRTICE i HDS (za PrimeTime pristup Adobe licensees samo).

U servisa Azure Media Services **kanala**, **programa**i **StreamingEndpoints** ručicu za sve u uživo strujanje radovi ingest, uključujući oblikovanje DVR, sigurnost, skalabilnost i zalihosti.

**Kanal** predstavlja kanal za obradu uživo strujanja medijskih sadržaja. Kanal možete primati u uživo unos strujanja na sljedeće načine:

- Lokalnog live encoder šalje višestruki-brzina prijenosa **RTMP** ili **Smooth Streaming** (fragmentirane MP4) kanal za koji je konfiguriran za isporuku **prolazni** . **Prolazni** isporuke je kada ingested strujanja proći kroz **kanal**s bez daljnje obrade. Možete koristiti sljedeće uživo koderi izlaz više – brzina prijenosa Smooth Streaming: Elemental, Envivio, Cisco.  Sljedeće uživo koderi izlaz RTMP: Adobe Flash Media Live kodiranje (FMLE), Telestream Wirecast i Tricaster transcoders.  Uživo kodiranje možete poslati i jedan brzina prijenosa strujanje kanala koji nije omogućen za šifriranje uživo, ali koje se ne preporučuje. Kada ste tražili, Media Services nudi toka klijentima.

    >[AZURE.NOTE] Prolazni metodom je najčešće najekonomičniji način live strujanje kada rade više događaje dugo vremenskom razdoblju te ste već uložiti u lokalni koderi. Pogledajte detalje [cijene](/pricing/details/media-services/) .
    
    
- Kodiranje uživo lokalnog šalje jednim-brzina prijenosa strujanje kanal koji je omogućen za izvođenje uživo kodiranje s Media Services u jednom od sljedećih oblika: RTMP ili Smooth Streaming (fragmentirane MP4). RTP (MPEG-TS) podržano je i, pod uvjetom da imate namjenski veze centar za Azure podataka. Sljedeće uživo koderi s RTMP izlaz gube se da biste radili s kanale ove vrste: Telestream Wirecast, FMLE. Kanal zatim izvodi uživo kodiranje na dolazni strujanje jedan brzina prijenosa za strujanje videozapisa višestruki-brzina prijenosa (prilagodljivo). Kada ste tražili, Media Services nudi toka klijentima.


Kada stvorite kanal, počevši od izdanje Media Services 2.10, možete odrediti na koji način želite kanala da biste primali ulaznog toka i hoće li želite kanala da biste izvršili uživo kodiranje vaše toka. Imate dvije mogućnosti:

- **Ništa** (prolazni) – odrediti vrijednost, ako planirate li koristiti lokalne uživo kodiranje koje će izlaz više – brzina prijenosa strujanje (prolazni strujanje). U ovom slučaju dolazne toka prošli kroz u Izlaz bez kodiranje. To je zadano ponašanje kanal prije 2.10 izdanje.  

- **Standardni** – odabir to vrijednost, ako planirate li koristiti Media Services za kodiranje na jednom brzina prijenosa uživo strujanje strujanje višestruki-brzina prijenosa. Ta je metoda više najekonomičniji brzo skaliranja prema gore za rijetko događaje. Imajte na umu da postoji naplate utjecaj za šifriranje uživo i trebali biste ne zaboravite da ostavite uživo kodiranja kanala u stanju "Izvodi" će uzrokovati naknade za naplatu.  Preporučuje se odmah prekinuti na tekući kanalima po dovršetku uživo strujanje događaja radi izbjegavanja naknada za vrlo svaki sat. 

##<a name="comparison-of-channel-types"></a>Usporedba vrsta kanala

Sljedeća tablica sadrži vodič za usporedbu dviju vrsta kanala koje podržava Media Services

Značajka|Prolazni kanala|Standardna kanala
---|---|---
Unos jedan brzina prijenosa kodira u više bitrates u oblaku|ne|Da
Najveća razlučivost, broj slojevima|1080p, 8 slojeve 60 + fps|720p, 6 slojeve 30 fps
Unos protokola|RTMP, prelazili putem glatke strujanje|RTMP, Smooth Streaming i RTP
Cijena|U odjeljku [cijene stranice](/pricing/details/media-services/) , a zatim kliknite na kartici "Live Video"|U odjeljku [cijene stranice](/pricing/details/media-services/) 
Maksimalno vrijeme izvođenja|24 x 7|osam sati
Podrška za umetanje slates|ne|Da
Podrška za ad signalizacija|ne|Da
Prolazni CEA 608 708 opisa|Da|Da
Mogućnost za oporavak kratak stalls u doprinos sažetka sadržaja|Da|Ne (kanala će početi slating nakon 6 + sekundi bez ulaznih podataka)
Podrška za koje nisu uniform unos GOPs|Da|Ne – unos, potrebno je ispraviti 2sec GOPs
Podrška za unos stopa varijable okvira|Da|Ne – unos, potrebno je ispraviti sekundi.<br/>Manji varijacije su tolerated, na primjer, tijekom scena visoke kretanja. No encoder ne može se ispustiti 10 okvira/sec.
Gubi se automatski-shutoff kanala kada unos sažetka sadržaja|ne|Nakon 12 sati, ako postoji bez programa koji se izvodi 

##<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Rad s kanale koje primate višestruki-brzina prijenosa uživo strujanje iz lokalnog koderi (prolazni)

Sljedeći dijagram prikazuje glavne dijelove platformu AMS koje slijede **prolazni** tijeka rada.

![Uživo tijeka rada](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Dodatne informacije potražite u članku [Rad s kanala te primanje višestruki-brzina prijenosa uživo strujanje iz lokalnog koderi](media-services-live-streaming-with-onprem-encoders.md).

##<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Rad s kanalima za koje je omogućena za izvođenje uživo kodiranje pomoću servisa Azure Media Services

Sljedeći dijagram prikazuje glavne dijelove platformu AMS koje slijede u Live strujanje tijeka rada kojima je omogućeno kanala da biste izvršili uživo kodiranje s Media Services.

![Uživo tijeka rada](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Dodatne informacije potražite u članku [Rad s kanala koji su omogućeni izvođenje Live šifriranje pomoću servisa Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

##<a name="description-of-a-channel-and-its-related-components"></a>Opis kanal i njegovih povezanih komponenti

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


##<a name="billing-implications"></a>Posljedice naplata

Kanal počinje čim je stanje prijelaze "Pokretanje" putem na API-JA za naplatu.  

Sljedeća tablica prikazuje kako kanala stanja mapiranje naplate stanja na portalu API i Azure. Imajte na umu da stanja malo razlike između API i Portal UX. Čim kanala je u stanju "Radi" putem na API-JA ili je u stanju "Spremna" ili "Strujeće" na portalu za Azure, naplata bit će aktivne.

Da biste prestali kanala iz koje naplata Dodatno, morate zaustaviti kanala putem na API-JA ili na portalu za Azure.
Vi ste odgovorni za zaustavljanje na kanalima kada ste gotovi s kanal. Nastavak naplata uzrokovat će pogrešku da biste zaustavili kanal.

>[AZURE.NOTE]Rad s standardne kanali, AMS će automatski shutoff neki kanal koji se još nalazi u stanju "Izvodi" 12 sati nakon gubi se unos sažetak sadržaja, a postoje pokrenutih programa. Međutim, vam i dalje naplaćivati put kanal je u stanju "Izvodi".

###<a id="states"></a>Država kanal i kako bi mapirali naplate načinu rada 

Trenutno stanje kanala. Moguće vrijednosti obuhvaćaju sljedeće:

- **Zaustavi**. Ovo je početno stanje kanal nakon njegova stvaranja (osim ako automatsko pokrenite je odabran na portalu.) Nema naplata pojavljuje se u tom stanju. U ovom stanju svojstva kanala mogu ažurirati, ali strujanje nije dopušteno.
- **Pokretanje**. Kanal je u tijeku rada. Nema naplata pojavljuje se u tom stanju. Ažuriranja ili strujanje je dopušteno tijekom to stanje. Ako se pojavi pogreška, kanal vraća stanje zaustavljanja.
- **Pokretanje**. Kanal se može obrade uživo strujanja. Sada je naplate korištenje. Morate zaustaviti kanala da biste spriječili dodatne naplata. 
- **Zaustavljanje**. Zaustavlja se kanal. Nema naplata pojavljuje se u tom tranzitne stanju. Ažuriranja ili strujanje je dopušteno tijekom to stanje.
- **Brisanje**. U tijeku je brisanje kanala. Nema naplata pojavljuje se u tom tranzitne stanju. Ažuriranja ili strujanje je dopušteno tijekom to stanje.

Sljedeća tablica prikazuje kako mapiranje stanja kanal za naplatu način rada. 
 
Stanje kanala|Pokazatelji portala korisničkog Sučelja|Je li naplatu?
---|---|---
Pokretanje|Pokretanje|Nema (tranzitne stanja)
Pokretanje|Jeste li spremni (bez pokrenute programe)<br/>ili<br/>Strujanje (barem jedan program koji se izvodi)|DA
Zaustavljanje|Zaustavljanje|Nema (tranzitne stanja)
Zaustavi|Zaustavi|ne


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="related-topics"></a>Povezane teme

[Azure Media Services fragmentirane MP4 Live Ingest specifikacija](media-services-fmp4-live-ingest-overview.md)

[Rad s kanalima za koje je omogućeno izvođenje Live kodiranje pomoću servisa Azure Media Services](media-services-manage-live-encoder-enabled-channels.md)

[Rad s kanale koje primate višestruki-brzina prijenosa uživo strujanje iz lokalnog koderi](media-services-live-streaming-with-onprem-encoders.md)

[Kvota i ograničenja](media-services-quotas-and-limitations.md).  

[Media Services koncepti](media-services-concepts.md)
