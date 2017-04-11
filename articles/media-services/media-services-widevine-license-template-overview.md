<properties 
    pageTitle="Pregled predloška licencu za Widevine | Microsoft Azure" 
    description="Ova tema sadrži pregled predloška Widevine licenca koja se koristi za konfiguriranje Widevine licence." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="widevine-license-template-overview"></a>Pregled predloška licencu za Widevine

##<a name="overview"></a>Pregled

Azure Media Services sada omogućuje konfiguriranje i zatražite Widevine licence. Kad reproduktora krajnji korisnik pokuša da biste reproducirali sadržaj Widevine zaštićen, zahtjev se šalje servis za isporuku licence da biste dobili licencu. Ako je servis licence Odobri zahtjev, šalje licence koje se šalju se u klijent i može se koristiti za šifriranje i reproducirati navedeni sadržaj.

Zahtjev za licence Widevine se oblikuje kao JSON poruke.  

Imajte na umu da možete odabrati da biste stvorili poruku prazan bez vrijednostima samo "{}" licence predlošku stvorit će se i uz sve zadane postavke.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

##<a name="json-message"></a>JSON poruke

Ime | Vrijednost | Opis
---|---|---
tereta |Base64 kodirani niza |Licenca zahtjev poslao klijent. 
content_id | Base64 kodirani niza|Identifikator koji se koristi za dobivanje glavne KeyId(s) i sadržaja ne rade za svaku content_key_specs.track_type.
Davatelj usluga |niz |Služi za pretraživanje sadržaja tipke i pravila. Obavezan.
policy_name | niz |Naziv prethodno registrirani pravila. Neobavezno
allowed_track_types | Redni broj  | SD_ONLY ili SD_HD. Kontrole koje sadržaja tipke želite uvrstiti u licence
content_key_specs | polja JSON strukture, potražite u članku **Specifikacija sadržaja ključ** ispod | Učinkovitiji grained kontrola na tipke sadržaja da biste se vratili. Detalje potražite u članku sadržaja specifikacija ključ ispod.  Samo jedan od allowed_track_types i content_key_specs možete navesti. 
use_policy_overrides_exclusively | Booleove vrijednosti. TRUE ili false | Pomoću pravila atribute određen policy_overrides i ispustite sve prethodno spremljene pravila.
policy_overrides | JSON Strukturiranje potražite u članku **Pravila nadjačava** ispod | Postavke pravilnika za licence.  U događaja ovaj resursa sadrži unaprijed definiranih pravilnika, koristit će se ti određene vrijednosti. 
session_init | JSON Strukturiranje potražite u članku **Pokretanje sesije** ispod | Neobavezne podatke proslijeđen licence.
parse_only | Booleove vrijednosti. TRUE ili false | Zahtjev za licence je raščlaniti, ali izdaje nikakva licenca. Međutim, vrijednosti obrasca zahtjev za licence se vraćaju u odgovoru.  

##<a name="content-key-specs"></a>Specifikacije sadržaja ključ 

Ako postoji unaprijed postojeće pravilo, nema potrebe da biste odredili neke od vrijednosti u sadržaja specifikacija ključ.  Da biste odredili izlazne zaštite kao što su HDCP i CGMS koristit će postojećih pravila pridružene sadržaja.  Ako unaprijed postojeće pravilo nije registriran s poslužiteljem Widevine licencu, davatelj sadržaja možete ubaciti vrijednosti u zahtjevu za licence.   


Svaki content_key_specs mora biti naveden za sve zapise, bez obzira na to use_policy_overrides_exclusively mogućnost. 


Ime | Vrijednost | Opis
---|---|---
content_key_specs. track_type | niz | Naziv vrste zapisa. Ako content_key_specs nije naveden u zahtjevu za licence, provjerite je li da biste odredili sve vrste praćenje izričito. Pogreška da biste to učinili uzrokovat će pogrešku za reprodukciju zadnjih 10 sekundi. 
content_key_specs  <br/> security_level | UInt32 | Definira odličnim preduvjeti za klijente za reprodukciju. <br/> 1 – potreban je softverski whitebox šifriranu. <br/> 2 – potreban je šifriranu softver i zatamnjenog sadržaja drugog. <br/> 3 – materijal i šifrirana operacije moraju izvršiti okruženja za sigurnosnu kopiju pouzdanih izvođenja hardvera. <br/> 4 – postavki šifriranja i dekodiranje sadržaja moraju izvršiti okruženja za sigurnosnu kopiju pouzdanih izvođenja hardvera.  <br/> 5 – na šifrirana, dekodiranja i sve pakiranja medijskih sadržaja (komprimirati i dekomprimirati) morate riješiti okruženja za sigurnosnu kopiju pouzdanih izvođenja hardvera.  
content_key_specs <br/> required_output_protection.hdc | niz – jedan od: HDCP_NONE, HDCP_V1, HDCP_V2 | Označava je li potreban HDCP
content_key_specs <br/>ključ | Base64 <br/>šifrirana niza|Sadržaja ključ za taj zapis. Ako navedeni, potreban je track_type ili key_id.  Ta mogućnost omogućuje davatelja sadržaja da biste ubaciti tipku sadržaja za taj zapis umjesto da Widevine licencni poslužitelj generirati ili potražiti ključa.
content_key_specs.key_id| Base64 kodirani niz binarni, 16 bajta | Jedinstveni identifikator za ključ. 


##<a name="policy-overrides"></a>Nadjačavanja pravila 

Ime | Vrijednost | Opis
---|---|---
policy_overrides. can_play | Booleove vrijednosti. TRUE ili false | Upućuje na to je dopušteno te reprodukcija sadržaja. Zadana je vrijednost false.
policy_overrides. can_persist | Booleove vrijednosti. TRUE ili false |Upućuje na to da je licenca možda dosljedan za pohranu koji nije uklonjiv za izvanmrežni rad. Zadana je vrijednost false.
policy_overrides. can_renew | Booleova vrijednost true ili false |Upućuje na to je li dopušten obnavljanje licence. Ako je true, trajanje licence mogu se proširiti intervala sustava. Zadana je vrijednost false. 
policy_overrides. license_duration_seconds | Int64 | Označava doba određene licence. Vrijednost 0 označava da nema ograničenja trajanja. Zadana vrijednost je 0. 
policy_overrides. rental_duration_seconds | Int64 | Označava prozoru vremena tijekom reprodukcije je dopušteno. Vrijednost 0 označava da nema ograničenja trajanja. Zadana vrijednost je 0. 
policy_overrides. playback_duration_seconds | Int64 | Prikaz prozora vremena nakon reprodukcije pokreće unutar trajanja licence. Vrijednost 0 označava da nema ograničenja trajanja. Zadana vrijednost je 0. 
policy_overrides. renewal_server_url |niz | Sve zahtjeve za intervala sustava (obnavljanja) za licence će upućivati na navedenom URL. To polje koristi samo ako su ispunjeni can_renew.
policy_overrides. renewal_delay_seconds |Int64 |Koliko sekundi nakon license_start_time, prije nego što je Obnova prvog pokušaja. To polje koristi samo ako su ispunjeni can_renew. Zadano je 0 
policy_overrides. renewal_retry_interval_seconds | Int64 | Određuje odgode u sekundama između zahtjeva za obnovu kasnije licencu, u slučaju. To polje koristi samo ako su ispunjeni can_renew. 
policy_overrides. renewal_recovery_duration_seconds | Int64 | Prozor radnog vremena, dopuštena reprodukcija da biste nastavili tijekom obnavljanja je pokušaj, još nije uspjelo zbog problema s pozadinskom s poslužiteljem za licence. Vrijednost 0 označava da nema ograničenja trajanja. To polje koristi samo ako su ispunjeni can_renew.
policy_overrides. renew_with_usage | Booleova vrijednost true ili false |Upućuje na to licencu moraju biti slanja za obnovu nakon pokretanja korištenje. To polje koristi samo ako su ispunjeni can_renew. 

##<a name="session-initialization"></a>Pokretanje sesije

Ime | Vrijednost | Opis
---|---|---
provider_session_token | Base64 kodirani niza |Ovaj token sesiju prenosi natrag u licenci i se nalaze u sljedeće obnove.  Sesije token će zadržavaju se izvan sesije. 
provider_client_token | Base64 kodirani niza | Klijent token da biste poslali vratiti u odgovoru licence.  Ako je zahtjev za licence sadrži token klijent, ta se vrijednost zanemaruje. Klijent token zadržavaju se izvan sesije licence.
override_provider_client_token | Booleove vrijednosti. TRUE ili false |False i zahtjev za licence sadrži token klijent, čak i ako se klijent token naveden je u toj strukturi pomoću tokena iz zahtjeva.  Ako je true, uvijek koristi navedeni u ovom strukturi token.

##<a name="configure-your-widevine-licenses-using-net-types"></a>Konfiguriranje licence Widevine koriste vrste .NET

Media Services pruža API-ji .NET koje možete konfigurirati Widevine licence. 

###<a name="classes-as-defined-in-the-media-services-net-sdk"></a>Klasa kako je definirano u Media Services .NET SDK

Slijede definicije ove vrste.

    public class WidevineMessage
    {
        public WidevineMessage();
    
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

###<a name="example"></a>Primjer

Sljedeći primjer pokazuje kako koristiti .NET API-ji za konfiguriranje jednostavne Widevine licence.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Vidi također

[Pomoću PlayReady i/ili Widevine dinamičkih uobičajenih šifriranja](media-services-protect-with-drm.md)
