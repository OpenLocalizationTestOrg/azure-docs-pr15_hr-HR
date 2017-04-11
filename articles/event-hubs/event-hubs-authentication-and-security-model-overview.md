<properties 
    pageTitle="Pregled događaja koncentratora za provjeru autentičnosti i sigurnosti modela | Microsoft Azure"
    description="Događaj koncentratora za provjeru autentičnosti i sigurnosnih modela pregled."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>Događaj koncentratora za provjeru autentičnosti i sigurnosnih modela pregled

Model sigurnosti koncentratora događaj zadovoljava sljedeće uvjete:

- Samo uređaje koji dovode valjanih se vjerodajnica možete slati podatke koncentratora za događaj.
- Uređaj ne može zamijeniti neki drugi uređaj.
- Rogue uređaja možete blokirati slanja podataka koncentratora za događaj.

## <a name="device-authentication"></a>Provjera autentičnosti uređaja

Model sigurnosti događaj koncentratora temelji se na kombinacije tokeni [Zajednički pristup potpis (SAS)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) i *izdavači događaj*. Programa publisher događaj definira virtualne krajnja točka za koncentratora za događaj. U programu publisher možete samo za slanje poruka koncentratora za događaj. Nije moguće poruka u programu publisher.

Obično koncentratora za događaj uključuje jedan publisher po uređaja. Sve poruke koje se šalju u bilo koji od izdavači koncentratora za događaj su enqueued unutar koncentratora događaj. Izdavači omogućuju kontrolu pristupa preciznije i ograničavanje.

Svakom uređaju se dodjeljuje jedinstveni token koja je prenesena na uređaju. Tokena proizvedeno tako da svaki jedinstveni token dopušta pristup različite jedinstveni programa Publisher. Uređaj koji sadrži sadrže token možete poslati samo jednog programa publisher, ali nema publisher. Ako više uređaja zajedničko korištenje iste tokena, svaki od tih uređaja dijeli u programu publisher.

Iako ne preporučuje se, moguće je oprema za rotaciju uređaja s tokena koje dodijelite izravan pristup koncentratora za događaj. Bilo kojeg uređaja koja sadrži to tokena poruke možete poslati izravno u koncentratora događaj. Takvog uređaja nije podložno ograničavanje. Osim toga, uređaj ne blacklisted šalje koncentratora događaj.

Svih tokena prijavljeni s ključem SAS. Obično svih tokena niste prijavljeni s istim ključem. Uređaji nisu svjesni key. To sprječava uređaji proizvodnje tokena.

### <a name="create-the-sas-key"></a>Stvaranje SAS ključa

Prilikom stvaranja je prostor naziva događaja koncentratora Azure događaj koncentratora generira ključ SAS 256-bitni naziva **RootManageSharedAccessKey**. Ovaj ključa daje slanje, slušanje i upravljanje pravima za naziva. Možete stvoriti dodatne tipke. Preporučuje se da proizvesti ključ daje poslati dozvole za određene koncentratora za događaj. Za ostatak u ovoj se temi, pretpostavlja se da je pod nazivom tog ključa `EventHubSendKey`.

Sljedeći primjer stvara ključa samo za slanje prilikom stvaranja središtu događaja:

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Stvaranje tokena

Možete generirati tokeni korištenje SAS ključa. Morate proizvesti samo jedan token po uređaja. Tokeni mogu proizvesti pa na sljedeći način. Svih tokena generiraju korištenje **EventHubSendKey** ključa. Svaki token se dodjeljuje jedinstvenog URI-JA.

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Pri pozivanju ovu metodu se URI navesti kao `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Za svih tokena URI jednak, s the exception od `PUBLISHER_NAME`, koji treba razlikuju za svaku token. Najbolje `PUBLISHER_NAME` predstavlja Identifikator uređaja na kojem se dobiva taj token.

Ta metoda generira token s sljedeću strukturu:

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

Vrijeme isteka tokena naveden je u sekundama od Sij 1, 1970. Slijedi primjer token:

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Obično, tokena imaju vrijeme upotrebljivosti koji podsjeća ili prekoračuje vrijeme upotrebljivosti uređaja. Ako na uređaju postoji mogućnost da biste dobili novi token, tokeni s kraće vrijeme upotrebljivosti može se koristiti.

### <a name="devices-sending-data"></a>Uređaji za slanje podataka

Kada stvorite tokena svakom uređaju se tamo uz vlastitu jedinstveni token.

Kada uređaj šalje podatke u koncentratora za događaj, uređaj oznake njegov token sa zahtjevom za slanje. Da biste spriječili napadaču iz eavesdropping i onemogućuje krađu token, komunikaciju između uređaja i središtu događaja mora biti putem šifrirane kanala.

### <a name="blacklisting-devices"></a>Blacklisting uređaja

Token krađe po napadaču, napadač možete oponašati uređaj čiji tokena sadrži je krađe. Blacklisting uređaj prikazuje uređaj nestabilan dok je uređaj dobiva novi token koji koristi drugog izdavača.

## <a name="authentication-of-back-end-applications"></a>Provjera autentičnosti pozadinskih aplikacija

Za provjeru autentičnosti pozadinskih aplikacija koje generira uređaji podatke, događaj koncentratora uključuje model sigurnosti koja je slična modela koji se koristi za servis Bus teme. Pretplatu na temu Bus servisa jednako granu potrošača koncentratora za događaj. Klijent možete stvoriti grupu potrošača ako zahtjev za stvaranje grupe potrošača je praćeni token daje da upravljanje ovlasti za središtu događaj ili naziva kojoj pripada koncentratora za događaj. Klijent je dopušteno zauzeti podataka iz grupe korisnika ako je zahtjev za primanje praćeni token koji daje primanje pravo na toj grupi potrošača, središtu događaj ili naziva kojoj pripada koncentratora za događaj.

Trenutna verzija servisa Bus ne podržava SAS pravila za pojedinačne pretplate. Isto se odnosi za događaj koncentratora korisničke grupe. Podrška za SAS dodat će se za obje značajke u budućnosti.

U Izostanak SAS provjere autentičnosti za pojedinačne korisničke grupe, možete koristiti tipke SAS sigurnost sve korisničke grupe s ključem uobičajenih. Taj se način omogućuje aplikaciju za korištenje podataka iz bilo kojeg od grupa potrošača koncentratora za događaj.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o događajima koncentratora, potražite u sljedećim temama:

- [Pregled događaja koncentratora]
- Na [u redu čekanja razmjenu rješenja] pomoću servisa Bus redova.
- U Dovršeno [Ogledni program koji koristi događaj koncentratora].

[Pregled događaja koncentratora]: event-hubs-overview.md
[primjer aplikacije koja koristi događaj koncentratora]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[u redu čekanja razmjenu rješenja]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
