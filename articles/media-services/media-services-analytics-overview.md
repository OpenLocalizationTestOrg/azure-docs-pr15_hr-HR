<properties
    pageTitle="Azure Media Services analize pregled | Microsoft Azure"
    description="Azure Media Services nudi javno pretpregled Azure Media analize, skup govora i računala vidom usluge na skaliranje enterprise, usklađenost, sigurnost i globalni razgovor. Servisa Azure Media analize ugrađenih pomoću komponente platforme za core servisa Azure Media Services i zato spremni za rukovanje media obrade na razini na jedan dan. "
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/24/2016"   
    ms.author="milanga;juliako;johndeu"/>

# <a name="azure-media-services-analytics-overview"></a>Azure Media Services pregled Analytics

##<a name="overview"></a>Pregled

Više tvrtkama ili ustanovama i velike tvrtke su embracing videozapis kao Preferirani Srednji obuka zaposlenika, sudjelovati svoje kupce i dokument poslovnih funkcija. Oblak računalstvo olakšava učinkovite za pohranu, strujanje i pristup te velike medijske datoteke, ali kao tvrtke Povećaj njihove videozapisa biblioteka sadržaja, moraju imati iste učinkovitih srednje vrijednosti za izdvajanje nove uvide iz videozapisa da biste stvorili smisleniji, personalizirane interakcije s njihovim ciljne skupine i iskoristite svoje tvrtke na sljedeću razinu.

Da biste riješili ovaj sve veći treba li vam na tržištu, servisa Azure Media Services nudi analize medijskih sadržaja, skup govora i vidom komponente (na skaliranje enterprise, usklađenost, sigurnost i globalni razgovor) koje olakšavaju za tvrtke ili ustanove i velike tvrtke za dobivanje glavne s akcijama uvida s svoje videodatoteke. Servisa Azure Media analize ugrađenih pomoću komponente platforme za core servisa Azure Media Services i zato spremni za rukovanje media obrade na razini na jedan dan.

Azure Media analize omogućiti inženjerima omogućuje brzi početak rada s vidom mogućnosti za videozapise na razini ograničeno i Premjesti to naprednih funkcija u aplikacije. Azure Media analize se temelji se može koristiti u korporacijskom okruženju s puno mjerilo, usklađenost, sigurnost i globalni razgovor potrebnih velikim tvrtkama i ustanovama.

Sljedeći dijagram prikazuje **Analize medijskih sadržaja** i druge glavne dijelove platformu Media Services. 

![VoD tijeka rada](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

Procesori media analize medijskih sadržaja proizvesti MP4 datoteke ili datoteke JSON. Ako media procesor proizvodi datoteku MP4, progresivno mogu preuzeti datoteku. Ako media procesor proizvodi JSON datoteke, možete preuzeti datoteku iz spremišta blobova platforme Azure. 

## <a name="azure-media-analytics-services"></a>Servisa Azure Media Analytics

- **Indeksiranje** – Azure Media indeksiranje omogućuje vam da biste sadržaj koji se može pretraživati, kao i generiranje zatvoreni skrivene titlove prati. Azure Media Services objavio **Azure Media indeksiranje 2 Preview** s brže indeksiranja i šira podrška za jezike. Podržani jezici obuhvaćaju engleski, španjolski, francuski, njemački, talijanskom, kineski, portugalski i arapski. Detaljne informacije i primjeri potražite u članku [postupak videozapise s Azure Media indeksiranje 2](media-services-process-content-with-indexer2.md)
 
- **Hyperlapse** – Microsoft Hyperlapse je rezultat više od 20 godina računalo vidom Istraživanje pri Microsoft odjelu za razvoj (MSR), kombiniranje video stabilization i vrijeme lapsing da biste stvorili brzu, potrošni, lijepe videozapisa iz dugačkih obrazaca sadržaja. Osim stvaranja vrijeme istekne, možete koristiti i Hyperlapse da biste stvorili stabilan videozapisi shaky videozapisi zabilježene putem mobilnih telefona i kamere. Detaljne informacije i primjeri potražite u članku [Hyperlapse medijske datoteke s Azure Media Hyperlapse](media-services-hyperlapse-content.md)
 
- **Otkrivanje kretanja** – taj servis možete koristiti da biste otkrili kretanja videozapisa s pozadinama tiskanice. To idealna je za korisnike koji želite potražiti false positives o događajima kretanja otkriven fotoaparati surveillance o sažecima sadržaja video surveillance. Detaljne informacije i primjeri potražite u članku [Otkrivanje kretanja za Azure Media analize](media-services-motion-detection.md).
 
- **Otkrivanje nominalne i nominalne nalaze** – pomoću ove usluge može prepoznati smješka ljudi i njihovih nalaze, uključujući sreća, sadness, iznenađenje, anger, contempt, problema, disgust i indifference/neutralno. Ima nekoliko korisne industrijskih aplikacije, opisane dolje, uključujući Zbrajanje i analiza reactions osoba sudjelovanju događaja. Detaljne informacije i primjeri potražite u članku [nominalne i otkrivanje emocije za Azure Media analize](media-services-face-and-emotion-detection.md).
 
- **Summarization videozapis** – videozapis summarization može vam pomoći stvoriti sažetaka dugo videozapisa odabirom automatski zanimljive isječci iz izvora videozapisa. To je korisno kada želite li omogućiti Kratak pregled što mogu očekivati dugo videozapisa. Detaljne informacije i primjeri potražite u članku [Korištenje Azure Media Video minijature da biste stvorili videozapis Summarization](media-services-video-summarization.md)

- **Optičkog prepoznavanja znakova** - Azure Media analize OCR-a (optičkog prepoznavanja znakova) omogućuje vam da biste pretvorili tekstni sadržaj u videodatoteke u digitalni tekst koji se može uređivati, koji se može pretraživati. Omogućuje automatizaciju izdvajanje smisleni metapodataka iz videozapisa signal medijski sadržaj.
 
- **Skalabilni nominalne redaktorskih** - **Azure Media Redactor** je pošiljatelja čija se MP Azure Media analize nudi redaktorskih skalabilni licem u oblaku. Nominalne redaktorskih omogućuje vam da biste izmijenili videozapis da biste Zamagljenost smješka odabrane pojedinaca. Trebali biste koristiti servis redaktorskih lice u javnom scenariji za sigurnost i medijskih sadržaja vijesti. Nekoliko minuta materijal koja sadrži više smješka može potrajati sata za redakturu ručno, ali uz taj servis postupka redaktorskih nominalne potrebno je samo nekoliko jednostavnih koraka. Dodatne informacije potražite u [ovom](media-services-face-redaction.md) članku.

 
## <a name="common-scenarios"></a>Uobičajeni scenariji

Slijedi nekoliko scenariji kojima Azure Media analize mogu pomoći u tvrtkama ili ustanovama i korporacije preko delatnosti toga nove uvide iz videozapisa da biste stvarali personalizirane publike i zaposlenika obaveze, kao i učinkovitije upravljanje velik broj videosadržaj:

- **Poziv centara** – Even s Božićni društvene mreže, poziv kupca centri i dalje olakšali velike postotak dovršetka transakcija s klijentom servisa. Kodirana audio podatke je mnoštvo informacije o klijentima koji mogu biti analizirati i poboljšanja proizvoda roadmaps i i obuka zaposlenika centar poziva da biste postigli veću zadovoljstva korisnika. Pomoću komponente za indeksiranje Azure Media korisnici će moći izdvajanje teksta i Sastavi indeks pretraživanja i nadzorne ploče da biste izdvojili obavještavanje oko Većina zajednički požali, požali na izvor i druge takve relevantne podatke.

- **Moderiranje sadržaja koje generira korisnik** – s novosti mediji da biste odjela policiju mnoge organizacije imate javno dostupnog portalima gdje prihvate UGC mediju, kao što je videozapisa i slika. Količine sadržaja možete Šiljak zbog neočekivanih događaja. U sljedećim scenarijima je uz nemoguće vođenje učinkovitih ručno pregled sadržaja za appropriateness. Korisnici mogu oslanjate servis moderiranje sadržaja radi fokusiranja na sadržaj koji odgovara.

- **Surveillance** - s growth fotoaparati IP postoji programa rastavljanje surveillance videozapisa. Ručno pregled surveillance videozapis je vrijeme intenzivno i podložni Ljudski pogreške. Azure Media Analytics nudi nekoliko komponenti kao što su otkrivanje kretanja, nominalne otkrivanje i Hyperlapse da bi postupka pregledavanje, upravljanje i stvaranje Izvedenice jednostavnije.

## <a name="media-services-analytics-media-processors"></a>Media Services procesora analize medijske sadržaje 

U ovom se odjeljku navodi sve na Media Services analize medijskih sadržaja procesora (MP) i pokazuje kako koristiti .NET ili OSTALE da biste dobili MP objekt.

### <a name="mp-names"></a>Nazivi MP


- Pretpregled Azure Media indeksiranje 2
- Azure Media indeksiranje
- Azure Media Hyperlapse
- Azure Media nominalne otkrivatelja
- Azure Media kretanja otkrivatelja
- Azure Media Video minijature
- Azure Media OCR-a

### <a name="net"></a>.NET

Sljedeće funkcije traje jedan od navedenog naziva MP i vratite MP objekt.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


## <a name="rest"></a>OSTALE

Zahtjev:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net
    
Odgovor:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

##<a name="demos"></a>Pokazni programi

[Azure Media analize pokazni programi](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

##<a name="next-steps"></a>Daljnji koraci

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-articles"></a>Povezani članci

[Objava Media Services Analytics](https://azure.microsoft.com/blog/introducing-azure-media-analytics/)
  

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
