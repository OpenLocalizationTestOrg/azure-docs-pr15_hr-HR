<properties 
    pageTitle="Azure Media Services fragmentirane MP4 live ingest specifikacija | Microsoft Azure" 
    description="U ovom specifikacija opisuju oblika fragmentirane MP4 koji se temelje uživo strujanje ingestion za Microsoft Azure Media Services i protokol. Microsoft Azure Media Services nudi uživo servis za strujanje koji omogućuju korisnicima strujanje uživo događaja i emitiranje sadržaj u stvarnom vremenu koristeći Microsoft Azure kao platformu oblaka. Ovaj dokument opisuje i najbolje prakse za izgradnju iznimno redundantnih i robusne uživo ingest mehanizme." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"     
    ms.author="cenkdin;juliako"/>

#<a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Media Services fragmentirane MP4 live ingest specifikacija

U ovom specifikacija opisuju oblika fragmentirane MP4 koji se temelje uživo strujanje ingestion za Microsoft Azure Media Services i protokol. Microsoft Azure Media Services nudi uživo servis za strujanje koji omogućuju korisnicima strujanje uživo događaja i emitiranje sadržaj u stvarnom vremenu koristeći Microsoft Azure kao platformu oblaka. Ovaj dokument opisuje i najbolje prakse za izgradnju iznimno redundantnih i robusne uživo ingest mehanizme.


##<a name="1-conformance-notation"></a>1. označavanje usklađenosti

Ključne riječi "morate", "Ne morate", "Potrebna", "SHALL", "Ne moraju", "SHOULD", "Ne TREBA", "PREPORUČENO", "MOŽDA" i "NEOBAVEZNO" u ovom dokumentu ćete ga je protumačiti kao što je opisano u RFC 2119.

##<a name="2-service-diagram"></a>2. dijagram usluga 

Dijagramu u nastavku prikazuje više razine arhitektura uživo servis za strujanje u servisa Microsoft Azure Media Services:

1.  Kodiranje uživo ih gura uživo sažetaka sadržaja u kanala koji su stvoreni i dodijeljeni resursi putem Microsoft Azure Media Services SDK.
2.  Kanala, programa i strujanje krajnje točke u servisa Microsoft Azure Media Services za sve u uživo strujanje radovi ingest, oblikovanje, uključujući cloud DVR, sigurnost, skalabilnost i zalihosti.
3.  Po želji korisnici mogli odabrati za implementaciju u mapi Zajednički dokumenti CDN između krajnju točku strujeće i krajnje točke klijenta.
4.  Klijent strujanje krajnje točke na krajnjoj točki strujeće putem protokola HTTP prilagodljivu Streaming (npr. Smooth Streaming, CRTICE, HDS ili HLS).

![slika1][image1]


##<a name="3-bit-stream-format--iso-14496-12-fragmented-mp4"></a>3. oblik bitne-strujanja – ISO 14496 12 fragmentirane MP4

Oblikovanje žičani za uživo strujanje ingest koji se spominju u ovom dokumentu temelji se na [ISO-14496 – 12]. Pogledajte [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx) za Detaljno objašnjenje obliku MP4 fragmentirane i proširenja za obje datoteke video-on-demand i live strujanje ingestion.

###<a name="live-ingest-format-definitions"></a>Uživo ingest definicije oblik 

U nastavku je popis poseban oblik definicije koji se odnose na live ingest u servisa Microsoft Azure Media Services:

1. "Ftyp", LiveServerManifestBox i okvir 'moov' mora biti poslana s svaki zahtjev (HTTP POST).  MORA biti poslana na početku toka, a kad god kodiranje morate ponovno povezati da biste nastavili strujanje ingest.  Pogledajte odjeljak 6 u [1] više pojedinosti.
2. Odjeljak 3.3.2 u [1] definira neobavezno okvir koji se naziva StreamManifestBox za uživo ingest. Zbog usmjeravanje logiku Microsoft Azure opterećenja ukinuta je korištenje ovaj okvir, a ne mora postojati kada ingesting u servis sustava Microsoft Azure Media. Ako postoji taj okvir servisa Azure Media Services tihu će je zanemariti.
3. TrackFragmentExtendedHeaderBox definirano u 3.2.3.2 u [1] mora postojati za svaki dio.
4. Verzija 2 TrackFragmentExtendedHeaderBox trebali BISTE koristiti da bi se generiraju segmenata medijske sadržaje s identičnim URL-ovima u više podatkovnim centrima. Polje index fragment je potreban za prebacivanje unakrsno Standard utemeljen na indeks strujanja oblikuje kao što su Apple HTTP Live strujanje (HLS) i sustavom indeks MPEG-crtica.  Da biste omogućili prebacivanje unakrsno Standard, indeks fragment moraju sinkronizirati preko više koderi i povećanje 1 za svaki fragment uzastopni media čak i u encoder ponovnog pokretanja ili neuspješna.
5. Odjeljak 3.3.6 u [1] definira okviru pod nazivom MovieFragmentRandomAccessBox ('mfra') koji se šalje na kraju uživo ingestion da biste naznačili EOS (završetka od strujanje) na kanal. Zbog ingest logike servisa Azure Media Services ukinuta je korištenje EOS (završetka od strujanje), a okvir 'mfra' uživo ingestion TREBA MOGLA biti poslana. Ako slanja servisa Azure Media Services tihu će je zanemariti. Preporučuje se da biste koristili [Kanal Vrati izvorno](https://msdn.microsoft.com/library/azure/dn783458.aspx#reset_channels) da vam ponovno Postavi stanje točke ingest i i preporučuje se da biste koristili [Program Zaustavi](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) da biste završili prezentaciju i strujanje.
6. Fragment trajanje MP4 mora biti konstante da biste smanjili veličinu manifesti klijenta i poboljšati klijent preuzimanje heuristike pomoću koristite ponavljanje oznaka.  Trajanje MOŽDA mijenjaju da bi vaše za učestalost sličica nije cijeli broj.
7. Fragment trajanje MP4 mora biti između približno 2 i 6 sekundi.
8. Vremenske oznake fragment MP4 i indeksi (TrackFragmentExtendedHeaderBox fragment_ absolute_ vremena i fragment_index) trebali BISTE stižu uzlaznim redoslijedom.  Sadrži iako prebacuju na duplicirane fragmentirane servisa Azure Media Services, vrlo ograničen mogućnost da biste promijenili redoslijed fragmentirane prema vremensku crtu za medijske sadržaje.

##<a name="4-protocol-format--http"></a>4 oblikovanje protocol – HTTP

Fragmentirane ISO MP4 koji se temelje uživo ingest za Microsoft Azure Media Services koristi standardnu dugo pokretanje HTTP POST zahtjev za prijenos kodiranih medijski podaci pakirat u obliku MP4 koji se fragmentirane na servis. Svakoj OBJAVI HTTP šalje na Dovršeno fragmentirane MP4 bitne strujanje ("strujanje") počevši od počevši s okviri zaglavlja (okvir "ftyp", "Live manifesta okvir poslužitelj" i "moov") i nastavite s niz fragmentirane ('moof' i 'mdat' okviri). Pogledajte odjeljak 9.2 u [1] URL sintaksu za objavu HTTP zahtjev. Primjer URL ADRESU objave je: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

###<a name="requirements"></a>Preduvjeti

Evo detaljnim preduvjetima:

1. Kodiranje trebala emitiranje tako da pošaljete zahtjev HTTP POST pomoću programa prazan "tijelo" (nulu duljina sadržaja) pomoću iste ingestion URL. To može pomoći brzo prepoznavanje ako krajnju točku uživo ingestion vrijedi i ako postoji neki provjeru autentičnosti i druge uvjete. Po HTTP protokola poslužitelj nećete moći slati natrag odgovor HTTP dok primitku cijelu zahtjeva uključujući tijelo objavu. Dani dugo izvodi prirode uživo događaj, bez ovaj korak kodiranje možda nećete moći da biste otkrili sve pogreške dok ne završi slanja sve podatke.
2. Kodiranje morate učiniti bilo kakve pogreške ili provjere autentičnosti izazove uslijed (1). Ako (1) uspješnog s 200 odgovor, i dalje.
3. Kodiranje morate započeti novi zahtjev za HTTP POST s fragmentirane MP4 toka.  Opseg morate započeti s okvirima zaglavlje slijedi fragmentirane.  Imajte na umu okvir "ftyp", "Live poslužitelja manifesta okvira" i "moov" (ovim redoslijedom) mora biti poslana s svaki zahtjev za potvrdu, čak i ako kodiranje morate ponovno povezati jer prethodni zahtjev prekinut je prije kraja toka. 
4. Kodiranje morate koristiti Chunked prijenos kodiranje za prijenos jer je nemoguće predviđanje cijelu duljina sadržaja uživo događaja.
5. Po događaj, nakon slanja posljednjeg djelić kodiranje obavljanje završiti poruke slijed Chunked prijenos kodiranje (većina snop klijent HTTP rukovati ga automatski). Kodiranje morate pričekati servisa da biste vratili kod konačni odgovor i prekid veze. 
6. Kodiranje morate koristiti imenički Events() kao što je opisano u 9.2 u [1] za uživo ingestion u servisa Microsoft Azure Media Services.
7. Ako zahtjev HTTP POST prekida ili prekorači dopušteno vrijeme prije kraja toka uz poruku o pogrešci TCP, kodiranje morate izdati novi zahtjev za objavu pomoću novu vezu i slijedite preduvjeti iznad s dodatnim zahtjevom da kodiranje morate ponovno prethodna dva MP4 fragmentirane za svaki zapis u toka i nastavak bez Uvod discontinuities na vremenskoj crti medijske sadržaje.  Ponovno slanje zadnje dvije fragmentirane MP4 za svaki zapis osigurava da nema gubitka podataka.  Drugim riječima, ako strujanje sadrži programa audio i video zapis, a ne uspije trenutni zahtjev za objavu, kodiranje morate ponovno povezati i ponovno zadnje dvije fragmentirane za audiozapise, koje su prethodno uspješno slanja i zadnje dvije fragmentirane za videozapis, koje su prethodno uspješno poslane, da biste bili sigurni da nema gubitka podataka.  Kodiranje morate zadržati "Naprijed" međuspremnik fragmentirane medijskih sadržaja koji će se ponovno poslati prilikom ponovnog povezivanja.

##<a name="5-timescale"></a>5. mjerilo 

[[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx) opisuje korištenje "Vremensko mjerilo" za SmoothStreamingMedia (sekcija 2.2.2.1), StreamElement (sekcija 2.2.2.3), StreamFragmentElement(2.2.2.6) i LiveSMIL (sekcija 2.2.7.3.1). Ako je vrijednost Vremensko mjerilo nema, zadana vrijednost koja se koristi je 10,000,000 (10 MHz). Iako specifikacija oblika Smooth Streaming ne blokira korištenje Vremensko mjerilo vrijednosti, većina implementacije koristi kodiranje zadanu vrijednost (10 MHz) radi generiranja Smooth Streaming ingest podataka. Zbog značajku [Dinamične pakiranja za Azure Media](media-services-dynamic-packaging-overview.md) nije preporučeno da biste koristili 90 kHz Vremensko mjerilo za strujanje videozapisa i 44.1 ili 48.1 kHz strujanja audiozapisa. Ako drugi Vremensko mjerilo vrijednosti koriste se za različite strujanja, Vremensko mjerilo razine za strujanje mora biti poslana. Pogledajte [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

##<a name="6-definition-of-stream"></a>6. definiciju "Tok"  

"Strujanje" je osnovna jedinica operacija uživo ingestion za sastavljanje prezentacije uživo, rukovanja strujanje prebacivanje i scenariji zalihosti. "Strujanje" definiran kao jedno jedinstveni fragmentirane MP4 bitne-strujanje koje mogu sadržavati zapis s jednom ili više zapisa. Potpuni uživo prezentacija može sadržavati neke strujanja ovisno o konfiguraciji uživo encoder(s). Primjere u nastavku ilustrirali različite mogućnosti korištenja stream(s) za sastavljanje prezentacije potpuni uživo.

**Primjer:** 

Klijent želi da biste stvorili prezentaciju uživo strujanje koja obuhvaća sljedeće bitrates audio/video:

Videozapis – 3000 kb/s "," 1500 kb/s "," 750 kb/s

Zvuk – 128 kb/s

###<a name="option-1-all-tracks-in-one-stream"></a>Mogućnost 1: Sve zapise u jedno strujanje

U tu mogućnost, jedan encoder generirat će sve zapise audio/video i grupirajte ih u jedno fragmentirane MP4 bitne-strujanje koji pa se šalju putem jedne HTTP POST veze. U ovom primjeru prikazuje se samo jedno strujanje za ovu prezentaciju uživo:

![slika2][image2]

###<a name="option-2-each-track-in-a-separate-stream"></a>Mogućnost 2: Svakom zapisu u zasebnom strujanje

U tu mogućnost, encoder(s) staviti samo jedan pratiti u svaki bitne-strujanje Fragment MP4 i objaviti sva strujanja putem više zasebnom HTTP veza. To se može učiniti s koderi jedan ili više koderi. Iz uživo ingestion točku prikaza, ovu prezentaciju uživo sastoji se od četiri strujanja.

![image3][image3]

###<a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Mogućnost 3: Paket audiozapis s najmanje brzina prijenosa video zapis u jedno strujanje

U ovoj mogućnosti kupca odluči grupirajte audiozapis s najmanje brzina prijenosa video zapis u jednu bitnu-strujanje Fragment MP4 i ostavite druge dvije video prati svaki se vlastitu strujanje. 


![image4][image4]


###<a name="summary"></a>Sažetak

Što je prikazano gore nije iscrpan popis svih mogućih ingestion mogućnosti za ovaj primjer. Što je matter programa zapravo sve Grupiranje zapisa u strujanja podržava uživo ingestion. Kupci i dobavljači kodiranje možete odabrati vlastite implementacije na temelju inženjerskim složenosti, kapacitet kodiranje i zalihosti i pitanja vezana uz prebacivanje. No to Primijetit ćete da u većini slučajeva postoji samo jedan audiozapise cijelu prezentaciju uživo stoga je važno da biste bili sigurni healthiness strujanje ingest koja sadrži audiozapise. Razmatranje često rezultira stavljanje audiozapisa u vlastitom strujanje (kao što je mogućnost 2) ili usnopljavanje s najmanje brzina prijenosa video zapis (kao što je mogućnost 3). Također radi bolje zalihosti i kvara odstupanje pošaljete na istom audiozapis u dvije različite (mogućnost 2 s suvišnih audiozapisa) ili usnopljavanje audiozapise najmanje dva najniže brzina prijenosa videozapisa zapisa (mogućnost 3 sa zvukom vezanoj instalaciji u najmanje dva strujanja videosadržaja) preporučujemo za uživo ingest u servisa Microsoft Azure Media Services.

##<a name="7-service-failover"></a>7. prebacivanje servisa 

Navedeni prirode uživo strujanje ključnih za osiguravanje dostupnost usluge je dobro prebacivanje podrška. Microsoft Azure Media Services osmišljena je za rukovanje različite vrste neuspjeha, uključujući pogreške mreže pogreške na poslužitelju, probleme prostora za pohranu, itd. Kada se koristi u kombinaciji s početnim prebacivanje sa strane uživo encoder, klijenta možete postići vrlo pouzdanog uživo strujanje usluge iz oblaka.

U ovom ćete odjeljku smo obrađuje scenariji prebacivanje servisa. U ovom slučaju pogreške negdje dogodi u roku od usluge i manifesti sam kao pogreške na mreži. Evo nekih preporuke za implementaciju kodiranje za rukovanje prebacivanje servisa:


1. Korištenje 10 drugi vremenskog ograničenja za uspostavljanje veze TCP.  Ako pokušaj uspostavili vezu traje dulje od 10 sekundi, prekinuti postupak i pokušajte ponovno. 
2. Kratko vremensko ograničenje koristite za slanje poruka blokova HTTP zahtjev.  Ako je trajanje fragment cilj MP4 N sekundi, koristite vremenskog ograničenja za slanje između N i 2N sekundi; na primjer, koristite vremensko ograničenje od 6 do 12 sekundi MP4 fragment traje 6 sekundi.  Ako se pojavi vremensko ograničenje, ponovno uspostavljanje veze, otvorite novu vezu i nastavili strujanje ingest na novu vezu. 
3. Održavanje rolling međuspremnik koji sadrži na zadnje dvije fragmentirane, za svaki zapis uspješno i potpuno poslane na servis.  Ako zahtjev HTTP POST za strujanje prekinula ili prekorači dopušteno vrijeme istovjetan Završi strujanje, otvorite novu vezu i započeti drugi zahtjev HTTP POST, ponovno strujanje zaglavlja, ponovno zadnje dvije fragmentirane za svaki zapis i nastavak toka bez Uvod discontinuity na vremenskoj crti medijske sadržaje.  To će smanjili mogućnost gubitka podataka.
4. Preporučuje se kodiranje ograničavanje broja ponovne pokušaje uspostaviti vezu i nastavak strujanje kada dođe do pogreške u TCP.
5. Nakon TCP pogreška:
    1. POTREBNO je zatvoriti trenutnu vezu, a za novi zahtjev za HTTP POST moraju se stvoriti novu vezu.
    2. U novi HTTP objavu URL-a mora biti isti kao početnu URL ADRESU za objavu.
    3. U novi HTTP objavu mora sadržavati strujanje zaglavlja (okvir "ftyp", "Live manifesta okvir poslužitelj" i "moov") jednako strujanje zaglavlja u početne objave.
    4. Zadnje dvije fragmentirane poslane za svaki zapis mora ponovno poslan i strujanje nastaviti bez Uvod discontinuity na vremenskoj crti medijske sadržaje.  Vremenske oznake za fragment MP4 morate povećati pokretom, čak i u HTTP POST zahtjeve.
6. Kodiranje potrebno prekinuti zahtjev HTTP POST ako podaci se šalje brzinom commensurate s dijelom trajanje MP4.  Zahtjev HTTP POST koji slati podatke možete spriječiti servisa Azure Media Services da brzo prekida veze s kodiranje slučaju ažuriranje servisa.  Zbog toga HTTP POST za kratke zapise (ad signal) mora biti kratki mogli živjeti, prekidanje čim se šalje kratke fragment.

##<a name="8-encoder-failover"></a>8. prebacivanje encoder

Prebacivanje Encoder je druga vrsta prebacivanje scenarij u kojem se mora biti adresirane na kraj do kraja uživo strujanje isporuku. U ovom scenariju s pogreškom se dogodilo sa strane kodiranje. 

![image5][image5]


U nastavku su očekivanja iz krajnju točku uživo ingestion kada se dogodi prebacivanje kodiranje:

1. Da biste nastavili strujanje, trebali BISTE moguće novu instancu encoder kao što je prikazano u dijagramu iznad (strujanje 3000 k videozapisa u vrstu iscrtkane crte).
2. Novi encoder morate koristiti istu URL HTTP POST zahtjeva za instancu nije uspjelo.
3. Kodiranje novi zahtjev za objavu mora sadržavati isti fragmentirane MP4 zaglavlja okvire kao instancu nije uspjelo.
4. Novi encoder moraju biti ispravno sinkronizirane s sve druge izvodi koderi za istu prezentaciju uživo za generiranje sinkronizirane audio/video uzorka s poravnatim fragment ograničenja.
5. Novi strujanje mora biti mogu semantički odrediti ekvivalentan s prethodne toka i resursa na razini zaglavlja i fragment.
6. Novi encoder pokušajte da biste minimizirali gubitka podataka.  Fragment_absolute_time i fragment_index medijskih sadržaja fragmentirane TREBALA povećati od točke gdje se kodiranje zadnje zaustavio.  Fragment_absolute_time i fragment_index TREBALA povećati neprekinuti način, ali je dopuštenih za Upoznavanje s discontinuity po potrebi.  Azure Media Services će zanemariti fragmentirane koji je već primili i obrađuju, pa je bolje err bočnoj ponovnog fragmentirane od za predstavljanje discontinuities na vremenskoj crti medijske sadržaje. 

##<a name="9-encoder-redundancy"></a>9. zalihosti encoder 

Za određene ključnih događaji uživo da čak i noviji dostupnosti zahtjev i kvalitetu sučelja, preporučuje se uključivanja aktivno aktivno suvišnih koderi da biste postigli objedinjenog prebacivanje bez gubitka podataka.

![image6][image6]

Kao što je prikazano u dijagramu iznad postoje dva skupa koderi istodobno margina dvije kopije svaki strujanje u uživo servis. Ovaj će instalacijski program podržana jer je mogućnost filtriranja duplicirane fragmentirane na temelju vremenske oznake i ID- a fragment strujanje servisa Microsoft Azure Media Services. Rezultat uživo strujanje i arhiviranja bit će jednu kopiju sva strujanja koji je najbolji mogući zbrajanja iz dva izvora. Na primjer, u hipotetska ekstremne slučaju toliko kao jedan kodiranje (ne mora biti jednak nešto) radi bilo kojem trenutku navedene u vrijeme za svaki strujanje, rezultirajući uživo strujanje iz servisa bit će neprekinuti bez gubitka podataka. 

Zahtjev za taj scenarij je gotovo jednako preduvjeti u slučaju Encoder prebacivanje uz iznimku pokrenute drugi skup koderi istovremeno kao primarni koderi.

##<a name="10-service-redundancy"></a>10. zalihosti servisa  

Za raspodjelu iznimno suvišnih globalni, je ponekad mora imati sigurnosnu kopiju regije-učiniti regionalne disasters. Proširivanje na topologije "Encoder zalihosti", korisnici možete odabrati da suvišnih servisa implementacije u nekoj drugoj regiji koji je povezan s 2 skup koderi. Klijenti nije moguće raditi s CDN davatelja za implementaciju GTM (globalni promet Upravitelj) ispred implementacijama dva servisa da biste jednostavno usmjerili promet klijenta. Preduvjeti za na koderi su jednaki slova "Encoder zalihosti" uz iznimku samo koje drugi skup koderi morate pokažete na neku drugu uživo ingest krajnju točku. Dijagramu u nastavku prikazuje instalacijski program:

![image7][image7]

##<a name="11-special-types-of-ingestion-formats"></a>11. posebne vrste Ingestion oblika 

U ovom se odjeljku opisuje neke posebne vrste uživo ingestion oblikovanja namijenjene rukovati neke određene scenarije.

###<a name="sparse-track"></a>Kratke zapis

Tijekom izlaganja prezentacije uživo strujanje doživljaj obogaćenog klijenta, često je potrebno za prijenos vrijeme sinkronizirati događaje ili signale četiriju kanalskih s podacima glavni medijske sadržaje. Primjer to je dinamički uživo reklame unosa. Ta vrsta događaja signalizacija razlikuje se od običnog audio/video strujanje zbog njegov kratke prirode. Drugim riječima, signaling podataka obično ne dogodi neprestano i interval može biti teško predviđanje. Koncept kratke Evidentiranje je posebno dizajnirane ingest i emitiranje u grupiranje signalizacija podataka.

U nastavku je preporučena implementacija za ingesting kratke zapis:

1. Stvorite zaseban fragmentirane MP4 bitne-strujanje željenim samo kratke zapisa bez audio/video zapise.
2. Navedite naziv nadređenog zapisa koristite parametar "parentTrackName" u "Live manifesta okvir poslužitelj" kako je definirano u odjeljak 6 u [1]. Pogledajte odjeljak 4.2.1.2.1.2 u [1] više pojedinosti.
3. U "Live manifesta okvir poslužitelj", manifestOutput mora biti postavljeno na "true".
4. Uz kratke prirode signaling događaj, preporučuje:
    1. Na početku uživo događaj encoder šalje okviri početni zaglavlja servis koji omogućuje servisa da biste registrirali kratke evidentiranje manifest klijenta.
    2. Kodiranje trebali BISTE prekinuli zahtjev HTTP POST kada se podaci ne šalju.  Dugo izvodi HTTP POST koji slati podatke možete spriječiti servisa Azure Media Services brzo prekida veze s kodiranje slučaju servis za ažuriranje ili poslužitelj za ponovno pokretanje, kao što je poslužitelj medijskih sadržaja bit će privremeno blokirane u operaciji primanje na na socket. 
    3. Vrijeme kad signalizacija podaci nisu dostupni encoder SHOULD Zatvori objavu HTTP zahtjev.  Dok je aktivna zahtjev za objavu, kodiranje slanje podataka 
    4. Prilikom slanja kratke fragmentirane kodiranje možete postaviti eksplicitnih sadržaj nulte duljine zaglavlje ako je dostupan.
    5. Prilikom slanja kratke fragment s novu vezu, kodiranje započeti slanje iz okvira zaglavlja koji slijedi novi fragmentirane. Time se rukovati kutije gdje se u međuvremenu dogodilo prebacivanje i novog poslužitelja koje ne vide kratke zapis prije nego što se uspostavljanja novi kratke veze.
    6. Fragment kratke praćenje će učiniti dostupnima za klijent kada odgovarajuće nadređenog praćenje fragment koja je jednaka ili veću vrijednost vremenske oznake postane dostupna klijentu. Ako, na primjer, ako kratke fragment sadrži vremenskog pečata t = 1000 očekuje se nakon što se vidi klijent videozapisa (Ako je evidentiranje naziv nadređenog videozapisa) fragmentiraj vremenske oznake 1000 ili beyond, možete preuzeti kratke fragment t = 1000. Napominjemo da stvarni signal nije sasvim dobro može koristiti za na drugo mjesto na vremenskoj crti prezentacije određenu svrhe. U primjeru iznad, moguće je koji kratke fragment t = 1000 sadrži tereta XML koji je za umetanje programa Ad položaju koji je nekoliko sekundi kasnije.
    7. Opseg fragment kratke zapisa može biti u različitim različite oblike (primjerice XML ili tekst ili binarni, itd.) ovisno o različitim scenarijima. 


###<a name="redundant-audio-track"></a>Suvišne audiozapisa

U uobičajeni HTTP prilagodljivu Streaming scenarij (npr. Smooth Streaming ili CRTICE), postoji često samo jedan audiozapisa u cijele prezentacije. Za razliku od video zapise koji sadrže više razina kvalitete za klijenta možete birati u stanja pogreške, audiozapise može biti jednu točku pogrešku ako ingestion strujanje koja sadrži audiozapise se prekida. 

Da biste riješili taj problem, Microsoft Azure Media Services podržava uživo ingestion suvišnih audiozapisa. Ideja je da isti audiozapis mogu se i poslati više puta u drugu. Dok je servis će samo registrirati audiozapise jednom u manifestu klijent, je mogli koristiti suvišnih audiozapisa sigurnosne kopije za dohvaćanje zvuka fragmentirane ako je primarni audiozapis nije riješen. Da bi se ingest suvišnih audiozapisa kodiranje mora:

1. Stvaranje isti audiozapisa u više Fragment MP4 bitne-tokova. Suvišne audiozapisa mora biti mogu semantički odrediti ekvivalentan s točno u istom fragment vremenske oznake i resursa na razini zaglavlja i fragment.
2. Pobrinite se da "zvuk" unos u Live poslužitelja manifesta (6 sekcije u [1]) biti jednak za sve suvišne audiozapisa.

U nastavku je preporučena implementacije za suvišnih audiozapisa:

1. Pošalji svaki jedinstveni audiozapisa u strujanje zasebno.  Za svaki od tih strujanja audiozapis, gdje 2 toka razlikuje se od na 1st samo prema identifikatoru u objavu URL HTTP poslati suvišnih strujanje: {protokol} :// {adresu poslužitelja} / {path}/Streams({identifier}) točke za objavljivanje.
2. Koristite odvojene strujanja za slanje dva bitrates najniže videozapisa. Svaki od tih strujanja smiju sadržavati kopija svake jedinstveni audiozapis.  Ako, na primjer, kada podržane su više jezika, te strujanja smiju sadržavati audiozapisa za svaki jezik.
3. Pomoću instance zaseban server (kodiranje) kodiranje i slanje suvišnih strujanja spomenute u (1) i (2). 



##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png

 