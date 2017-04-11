<properties 
    pageTitle="Kako kodiranje sredstvo pomoću Media Encoder standardne | Microsoft Azure" 
    description="Saznajte kako pomoću Media Encoder standardne kodiranje medijskih sadržaja na Media Services. Primjere koda pomoću REST API-JA." 
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


#<a name="how-to-encode-an-asset-using-media-encoder-standard"></a>Kako kodiranje sredstvo pomoću Media Encoder Standard


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
- [OSTALE](media-services-rest-encode-asset.md)
- [Portal](media-services-portal-encode.md)

##<a name="overview"></a>Pregled
Da bi se isporuku digitalni videozapise putem Interneta morate Komprimiranje medijskih sadržaja. Digitalne videodatoteke su vrlo velike i možda je prevelika da bi se isporučiti putem Interneta ili za uređaje vaših korisnika da bi se prikazao pravilno. Kodiranje je postupak sažimanje video i audiosadržaji da bi korisnici mogli vidjeti medijskih sadržaja.

Kodiranje poslove su jedna od najčešće obrada postupke u Media Services. Stvaranje kodiranja poslove pretvoriti medijske datoteke iz jedne kodiranje u drugu. Kada kodiranje, možete koristiti Media Services ugrađene kodiranje (Media Encoder standardni prikaz). Možete koristiti i kodiranje koje ste dobili od partnera Media Services; trećih strana koderi dostupni su putem servisa Azure Marketplace. Možete navesti detalje o kodiranje zadatke pomoću unaprijed postavljeni nizova definirali za svoje encoder ili pomoću unaprijed konfiguracijske datoteke. Vrste unaprijed postavljeno koji su dostupni potražite [Unaprijed postavljeno zadatak za Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).

Svaki zadatak može imati jedan ili više zadataka ovisno o vrsti obrada koju želite izvršiti. Putem REST API-JA, možete stvoriti zadatke i njihovih povezani zadaci na jedan od dva načina:

- Zadaci mogu biti definirani izravno putem svojstvo zadaci na entiteti posao ili
- pomoću OData skupna obrada.


Preporučuje se uvijek kodiranje mezzanine datoteka u programa prilagodljivo brzina prijenosa MP4 postavljanje, a zatim skup pretvorili u željeni oblik pomoću [Dinamičke pakiranje](media-services-dynamic-packaging-overview.md). Da biste iskoristili dinamički pakiranje, najprije se najmanje jednu jedinicu strujanje na zahtjev za strujanje krajnju točku iz koje namjeravate isporuku sadržaja. Dodatne informacije potražite [u](media-services-portal-manage-streaming-endpoints.md)članku skaliranje Media Services.

Ako je vaš izlaz resursa za pohranu šifrirane, morate konfigurirati pravila za isporuku resursa. Dodatne informacije potražite u članku [Konfiguriranje resursa isporuke pravila](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE]Prije nego što počnete pozivanju procesora medijskih sadržaja, provjerite imate li ispravan medij procesor ID-a. Dodatne informacije potražite u članku [Početak procesora medijske sadržaje](media-services-rest-get-media-processor.md).

##<a name="create-a-job-with-a-single-encoding-task"></a>Stvaranje zadatka s jednog kodiranja zadatka

>[AZURE.NOTE] Rad s Media Services REST API-JA, primjenjuju se Imajte na umu sljedeće:
>
>Prilikom pristupanja entiteti u Media Services, morate postaviti određene zaglavlje polja i vrijednosti u zahtjevima za HTTP. Dodatne informacije potražite u članku [Postavljanje Media Services REST API razvoj](media-services-rest-how-to-use.md).

>Nakon uspješnog povezivanja s https://media.windows.net, primit ćete 301 preusmjeravanje navodeći drugi URI Media Services. Provjerite naknadni pozivi za nove URI kao što je opisano u [povezuje se s Media Services pomoću REST API -JA](media-services-rest-connect-programmatically.md).
>
>Kada pomoću JSON i navođenjem da biste koristili **__metadata** ključne riječi u zahtjevu za (na primjer, da biste reference povezani objekt) morate postaviti zaglavlje **Prihvati** [JSON opširno](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)oblik: Prihvati: aplikacija/json; odata = opširno.

Sljedeći primjer pokazuje kako stvarati i objavljivati posao jednim zadatka postavljena na kodiranje videozapisa na određene razlučivosti i kvalitete. Kada kodiranje s Media Encoder standardne, možete koristiti unaprijed postavljeno konfiguracije zadatka navedene [u nastavku](http://msdn.microsoft.com/library/mt269960).

Zahtjev:

OBJAVA https://media.windows.net/API/Jobs HTTP/1.1 vrste sadržaja: aplikacije/json; odata = opširno Prihvati: aplikacije/json; odata = opširno DataServiceVersion: 3.0 MaxDataServiceVersion: 3.0 x ms-verzija: 2.11 autorizacije: nošenja <token value> 
 x-ms-– zahtjev-id klijenta: 00000000-0000-0000-0000-000000000000 glavnog računala: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Multiple Bitrate 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Odgovor:
    
    HTTP/1.1 201 Created

    . . . 

###<a name="set-the-output-assets-name"></a>Postavljanje naziva izlaz resursa

Sljedeći primjer prikazuje način da biste postavili atribut assetName:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

##<a name="considerations"></a>Razmatranja

- Svojstva TaskBody morate koristiti slovni XML da biste definirali broj unos ili izlaz sredstva koja će se koristiti zadatak. Tema zadatka sadrži definicija sheme XML za XML.
- U definiciji TaskBody svaki inner vrijednost <inputAsset> i <outputAsset> mora biti postavljeno kao JobInputAsset(value) ili JobOutputAsset(value).
- Zadatak možete imati više izlaz resursi. Jedan JobOutputAsset(x) mogu se koristiti samo jednom kao na Izlaz zadatka na zadatak.
- Možete odrediti JobInputAsset ili JobOutputAsset kao sredstvo za unos zadatka.
- Zadaci moraju obrasca krugu.
- Vrijednost parametra koji proći JobInputAsset ili JobOutputAsset predstavlja vrijednost indeksa sredstava. Stvarni Resursi su definirani u navigaciju svojstva InputMediaAssets i OutputMediaAssets na entitet definiciju posla. 
- Budući da je Media Services utemeljena na OData v3, pojedinačne imovine u InputMediaAssets i OutputMediaAssets zbirke svojstvo navigacijsko se pozivaju pomoću programa "__metadata: uri" par vrijednost za naziv.
- InputMediaAssets mapira jednog ili više sredstava koji ste stvorili u Media Services. OutputMediaAssets stvaraju se u sustav. Koje se odnose postojeće resursa.
- OutputMediaAssets možete dobiti naziv na temelju atribut assetName. Ako je taj atribut postoji, a zatim naziv u OutputMediaAsset bit će bilo kakve unutarnji tekst vrijednost u <outputAsset> element je s sufiks naziv zadatka vrijednost ili vrijednost Id zadatka (u slučaju gdje nije definirano svojstvo naziv). Na primjer, ako postavite vrijednost za assetName da biste "Uzorak", zatim naziv OutputMediaAsset svojstvo bi biti postavljeno na "Uzorak". Međutim, ako ne postavite vrijednost za assetName, ali niste postavili naziv zadatka da biste "NewJob", zatim naziv OutputMediaAsset bi "_NewJob JobOutputAsset (vrijednost)". 


##<a name="create-a-job-with-chained-tasks"></a>Stvaranje zadatka s ulančane zadataka

U mnogim situacijama aplikacije razvojnim inženjerima želite stvoriti niz obrade zadataka. U Media Services možete stvoriti niz ulančana zadataka. Svaki zadatak izvodi različite obrada korake, a možete koristiti drugi medij procesora. Ulančane zadaci podijelite isključivanje sredstvo iz jedan zadatak u drugu, izvođenje linearnom nizu zadataka na sredstava. Međutim, zadatke koje izvode u posao ne moraju biti u nizu. Kada stvorite ulančane zadatka, ulančana **ITask** objekte stvaraju se u jedan objekt **IJob** .

>[AZURE.NOTE] Trenutno je ograničenje od 30 zadataka po poslu. Ako vam je potrebna lanac više od 30 zadaci, stvorite više posao koja će sadržavati zadatke.


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


###<a name="considerations"></a>Razmatranja

Da biste omogućili Ulančavanje zadatka:

- Zadatak morate imati barem 2 zadataka
- Mora imati najmanje jedan zadatak je čiji unos Izlaz iz druge zadatke u sklopu posla.

## <a name="use-odata-batch-processing"></a>Korištenje OData skupna obrada 

Sljedeći primjer pokazuje kako koristiti OData obrade da biste stvorili zadatak i zadatke. Informacije o obrade potražite u članku [Skupna obrada protokola Open Data (OData)](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).
 
    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--
 


## <a name="create-a-job-using-a-jobtemplate"></a>Stvaranje zadatka pomoću programa JobTemplate


Tijekom obrade više koju koristite zajednički skup zadataka, JobTemplates su korisne možete navesti zadane konfiguracije zadatka redoslijed zadataka, i tako dalje.

Sljedeći primjer pokazuje kako stvoriti na JobTemplate s TaskTemplate definirani retku. Na TaskTemplate koristi Media Encoder standardne kao u MediaProcessor za kodiranje datoteke resursa; No se druge MediaProcessors nije kao i koristiti. 


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }
 

>[AZURE.NOTE]Za razliku od drugih entiteti Media Services, definirajte novi GUID identifikator za svaku TaskTemplate i njegovo stavljanje u taskTemplateId i svojstvo Id tijelo zahtjev. Sheme sadržaja identifikacijski moraju pratiti shemu opisane u prepoznavanje Azure Media Services entiteti. JobTemplates nije moguće ažurirati. Umjesto toga, morate stvoriti novi s ažuriranim promjene.
 

Ako ne uspije, vraća se sljedeće odgovor:
    
    HTTP/1.1 201 Created
    
    . . .


Sljedeći primjer pokazuje kako stvoriti odnose JobTemplate ID-a za posao:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}
     

Ako ne uspije, vraća se sljedeće odgovor:
    
    HTTP/1.1 201 Created
    
    . . . 



##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Daljnji koraci
Sada kada znate kako stvoriti zadatak za kodiranje programa assset, idite na temu [Kako da biste provjerili napretka posla s Media Services](media-services-rest-check-job-progress.md) .


##<a name="see-also"></a>Vidi također

[Početak procesora medijske sadržaje](media-services-rest-get-media-processor.md)
