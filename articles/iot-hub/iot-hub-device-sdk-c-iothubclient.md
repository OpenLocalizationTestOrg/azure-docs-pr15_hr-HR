<properties
    pageTitle="Azure IoT uređaja SDK za C - IoTHubClient | Microsoft Azure"
    description="Dodatne informacije o korištenju biblioteka IoTHubClient uređaja za Azure IoT SDK za C"
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Microsoft Azure IoT uređaja SDK za C – dodatne informacije o IoTHubClient

[Prvog članka](iot-hub-device-sdk-c-intro.md) u ovoj seriji uvedena **uređaja Microsoft Azure IoT SDK C**. Članak objašnjenje da postoje dvije arhitektonski slojeve u SDK. Na temelju je biblioteka **IoTHubClient** koji izravno upravlja komunikaciju s IoT koncentratora. Također je biblioteka **serijalizatora** koju sastavlja pri vrhu programa koje pružaju serijalizacije usluge. U ovom članku ćemo pružati Dodatni detalji o biblioteci **IoTHubClient** .

Prethodni članak što je opisano kako koristiti biblioteku **IoTHubClient** za slanje događaje koncentrator IoT i primanje poruka. U ovom se članku proširuje tu raspravu s preciznije upravljali *prilikom* slanja i primanja podataka, Uvod koji **API-ji niže razine**. Ne možemo ćete objašnjavaju kako priložiti svojstva događaja (te njihovo dohvaćanje iz poruka) pomoću svojstva zadužen za značajke u biblioteci **IoTHubClient** . Na kraju, možemo ponuditi dodatne objašnjenje različitih načina rukovanja poruke koje ste primili iz centra za IoT.

U članku završava po prekrivajući nekoliko razne teme, uključujući dodatne informacije o uređaju vjerodajnice i kako promijeniti funkcioniranje **IoTHubClient** putem mogućnosti konfiguracije.

Uzorci SDK **IoTHubClient** ćemo koristiti objašnjenja ove teme. Ako želite praćenje izlaganja, pročitajte članak u **iothub\_klijent\_uzorka\_http** i **iothub\_klijent\_uzorka\_amqp** aplikacije koje se nalaze na uređaju Azure IoT SDK za C. što je opisano u sljedećim odjeljcima sve je planirati ta uzorka.

Možete pronaći **Azure IoT uređaj SDK C** u spremištu GitHub [Microsoft Azure IoT SDK-ovi](https://github.com/Azure/azure-iot-sdks) i prikaz pojedinosti u API [C API reference](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-lower-level-apis"></a>API-ji niže razine

Prethodni članak opisuje osnovni postupak **IotHubClient** u kontekstu u **iothub\_klijent\_uzorka\_amqp** aplikacije. Ako, na primjer, objašnjeno kako pokrenuti biblioteke pomoću kod.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Opisuje se i slanju događaja pomoću ovog poziva (opis funkcije).

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

U članku opisane i kako primati poruke tako da Registracija povratnog funkcija.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

U članku prikazivao i kako osloboditi resurse pomoću koda kao što su sljedeće.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

No postoje pomoćnom funkcije za svaki od tih API-ji:

-   IoTHubClient\_sve\_CreateFromConnectionString

-   IoTHubClient\_sve\_SendEventAsync

-   IoTHubClient\_sve\_SetMessageCallback

-   IoTHubClient\_sve\_uništiti


Sve ove funkcije "Svima" obuhvatiti API naziv. Osim toga, parametre svaki od tih funkcija jednaki njihove verzija nije sve. Međutim, ponašanje te funkcije razlikuje jedan važnih način.

Kada se uključujete **IoTHubClient\_CreateFromConnectionString**, temeljni biblioteke stvoriti novi niti koji se izvodi u pozadini. Ovaj niti događaja koji se šalje i prima poruke iz centra za IoT. Nema takve niti se stvara prilikom rada s API-ji "Svima". Stvaranje pozadina niti je praktičnost da biste programer. Ne morate brinuti o izričito događaje poruke slati i primati iz koncentratora IoT – se automatski događa u pozadini. Nasuprot tome, API-ji "Svi" vam eksplicitnih kontrolu nad komunikaciju s IoT središte ako vam je potrebna.

Da biste bolje razumjeli to Pogledajmo primjer:

Kada se uključujete **IoTHubClient\_SendEventAsync**, što zapravo radite je umetanja događaja u međuspremnika. Pozadine niti stvoren prilikom pozivanja **IoTHubClient\_CreateFromConnectionString** neprestano nadzire ovaj međuspremnik i šalje sve podatke koje sadrži IoT koncentrator. To se događa u pozadini u isto vrijeme glavnom niti izvršava druge rad.

Isto tako, kada registrirate funkciju povratnog poruka pomoću **IoTHubClient\_SetMessageCallback**, koristite podešavanju SDK da bi niti pozadine pozovite funkciju povratnog kada je poruka primljene, neovisno o glavnom niti.

"Svi" API-ji ne stvorite niti pozadine. Umjesto toga, novi API moraju biti pozvani za izričito slanje i primanje podataka iz centra za IoT. To je što je prikazano u sljedećem primjeru.

U **iothub\_klijent\_uzorka\_http** aplikacije uvrštene u SDK pokazuje API-ji niže razine. U tom uzorka, što mi šaljemo događaji IoT koncentrator s kodom kao što je sljedeće:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Prva tri retka stvorite poruku, a u zadnjem retku šalje događaj. Međutim, kao što je već rečeno, "Šalje" događaj znači da podatke jednostavno potvrdili u međuspremnik. Ništa se prenose na mreži kada smo poziv **IoTHubClient\_sve\_SendEventAsync**. Redoslijedom do zapravo ingress podataka IoT koncentrator, morate nazvati **IoTHubClient\_sve\_DoWork**, kao i u ovom primjeru:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Kod (iz u **iothub\_klijent\_uzorka\_http** aplikacije) višekratno poziva **IoTHubClient\_sve\_DoWork**. Svaki put kada **IoTHubClient\_sve\_DoWork** je pod nazivom neke događaje iz međuspremnika šalje koncentrator IoT i dohvaća podatke u redu čekanja poruke koja se šalje na uređaju. Potonjem slučaju, to znači da ako smo registriran funkcija povratnog poziva za poruke, zatim povratnog se poziva (uz pretpostavku da sve poruke u redu čekanja). Ne možemo bi registriranih takve povratnog funkcije kod kao što je sljedeće:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

Razlog koji **IoTHubClient\_sve\_DoWork** često se nazivaju u petlji je da svaki put i on se zove, šalje *neke* u međuspremniku događaje koncentrator IoT i vraća *na sljedeću* poruku u redu za uređaj. Svaki poziv nije zajamčena da biste poslali sve buffered događaje ili da biste dohvatili sve u redu čekanja poruke. Ako želite poslati svim događajima u međuspremnik, a zatim nastavite s drugim obrada Ova petlja možete zamijeniti kod kao što je sljedeće:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Kod poziva **IoTHubClient\_sve\_DoWork** dok svi događaji u međuspremniku su poslani IoT koncentrator. Imajte na umu to također podrazumijeva da sve poruke u redu čekanja zaprimljeni. Dio razlog za to je Provjera poruka "sve" nije kao deterministic akciju. Što se događa ako dohvatite "sve" poruke, ali pa će se neki drugi poslane na uređaju odmah nakon? Bolji način radnju za koje je s programiranih vremenskog ograničenja. Na primjer, funkciju povratnog poruku nije moguće vratiti vremena svaki put kada se. Zatim možete napisati logike da biste nastavili obrada ako, na primjer, poruke nije zaprimljeni u zadnjoj sekundi *X* .

Kada ste gotovi ingressing događaja i primanje poruka, provjerite je li za poziv odgovarajuće funkcije da biste očistili resursi.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Zapravo je samo jedan skup API-ji za slanje i primanje podataka s niti pozadine i drugi skup API-ji koji ne na isti način bez niti pozadine. Mnogo razvojnim inženjerima možda radije koje nisu – sve API-ji, ali niže razine API-ji su korisne prilikom programer želi eksplicitnih kontrolu nad vrijednost mreže. Na primjer, neke uređaje prikupljanje podataka s vremenom i samo ingress događaje u određenim intervalima (na primjer, jedanput pojavila sat ili jednom dnevno). API-ji niže razine vam mogućnost izričito kontrolu prilikom slanja i primanja podataka s IoT koncentrator. Drugi jednostavno radije jednostavnosti koji omogućuju API-ji niže razine. Što se događa na glavnom niti umjesto neke službeni događa u pozadini.

Bez obzira modela odaberete, ne zaboravite da budu dosljedni u koji koristite API-ji. Ako pokrenete tako da nazovete **IoTHubClient\_sve\_CreateFromConnectionString**, provjerite je li samo pomoću odgovarajuće niže razine API-ji za napravili uputa za daljnji rad:

-   IoTHubClient\_sve\_SendEventAsync

-   IoTHubClient\_sve\_SetMessageCallback

-   IoTHubClient\_sve\_uništiti

-   IoTHubClient\_sve\_DoWork

Suprotno vrijedi i. Ako započnete s **IoTHubClient\_CreateFromConnectionString**, zatim pomoću koje nisu – sve API-ji za sve dodatnu obradu.

Azure IoT uređaja SDK za C potražite u članku u **iothub\_klijent\_uzorka\_http** aplikacije za dovršavanje primjera API-ji niže razine. U **iothub\_klijent\_uzorka\_amqp** aplikaciju možete se pozivati na punu primjer od koje nisu – sve API-ji.

## <a name="property-handling"></a>Svojstvo rukovanje

Dosad kada smo ste opisane slanje podataka, ne možemo ste je koje upućuju na tijelo poruke. Na primjer, razmotrite kod:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

U ovom se primjeru šalje poruku IoT koncentrator s tekstom "Zdravo svijeta." Međutim, koncentrator IoT omogućuje i svojstva za svaku poruku. Svojstva su parove naziv/vrijednosti koje se mogu priložiti u poruku. Ako, na primjer, ne možemo možete izmijeniti prethodne kod da biste svojstvo priložili poruku:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Ne možemo pokrenuti tako da nazovete **IoTHubMessage\_svojstva** i prosljeđivanje ručice za naše poruke. Što smo vratiti se na **KARTE\_RUKOVATI** referencu koju nam da biste pokrenuli dodavanje svojstava. Drugu mogućnost se postiže tako da nazovete **karte\_AddOrUpdate**, koji traje reference na KARTI\_RUČICU, naziv svojstva i svojstva vrijednost. Uz taj API smo možete dodati proizvoljan broj svojstva smo želite.

Kada događaj je za čitanje iz **Koncentratora događaj**, primatelja možete Nabrajanje svojstava i dohvaćanje njihove odgovarajuće vrijednosti. Na primjer, u .NET to želite napraviti tako da pristupite [zbirci svojstva objekta EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

U prethodnom primjeru, ne možemo ste priložite svojstva događaja koje mi šaljemo IoT koncentrator. Svojstva i moguće priložiti poruke koje ste primili iz centra za IoT. Ako želimo dohvatiti svojstva iz poruke, koristimo kod kao što su sljedeće u našem funkcija povratnog poruka:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

Poziv na **IoTHubMessage\_svojstva** vraća na **KARTE\_RUKOVATI** referenca. Ne možemo zatim prenesite taj referenca **karte\_GetInternals** da biste dobili reference na raspon parova naziv/vrijednost (kao i ukupan broj svojstva). U tom trenutku je jednostavno pitanje prebrojavanje svojstva da biste pristupili vrijednosti koje želimo.

Ne morate koristiti svojstva u aplikaciji. Međutim, ako je potrebno postaviti na događaje ili njihovo dohvaćanje iz poruka biblioteku **IoTHubClient** olakšava.

## <a name="message-handling"></a>Rukovanje porukama

Prema uputama koje ste prethodno, kad stigne poruka iz koncentratora IoT biblioteku **IoTHubClient** odgovori po pozivanje registrirani povratnog funkcija. Nema povrata parametar ovu funkciju koja zaslužuje neke dodatne objašnjenje. Evo u isječak funkcija povratnog u u **iothub\_klijent\_uzorka\_http** poslušajte aplikacije:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Imajte na umu da je vrsta povrata **IOTHUBMESSAGE\_RAZMJEŠTAJA\_REZULTAT** i u ovom slučaju ne možemo vratili **IOTHUBMESSAGE\_PRIHVAĆENA**. Postoje druge vrijednosti smo možete se vratiti na ovu funkciju promijeniti način na koji biblioteku **IoTHubClient** reacts povratnog poruke. Evo željene mogućnosti.

-   **IOTHUBMESSAGE\_PRIHVAĆENA** – poruka je uspješno obrađen. Biblioteka **IoTHubClient** će pozivanje funkcija povratnog ponovno s ista poruka.

-   **IOTHUBMESSAGE\_ODBAČENO** – poruku nije obrađen, no to nije bez želju da biste to učinili u budućnosti. Biblioteka **IoTHubClient** treba pozivanje funkcija povratnog ponovno s ista poruka.

-   **IOTHUBMESSAGE\_ABANDONED** – poruku nije uspješno obrađen, ali biblioteku **IoTHubClient** treba pozivanje funkcija povratnog ponovno s ista poruka.

Prva dva povrata kodove biblioteku **IoTHubClient** šalje poruku IoT koncentrator koja navodi se da poruka izbrisane iz reda uređaj a ne isporučiti ponovno. Neto efekt je ista (Ta se poruka briše iz reda uređaj), ali li prihvatite ili odbacite poruke i dalje naveden.  Snimanje ovaj status je korisno pošiljatelja poruke tko može preslušali za povratne informacije i saznati je li uređaj prihvatite ili odbacite određenu poruku.

U slučaju da posljednje i slanja poruke IoT koncentrator, ali znači da bi se trebali biste redelivered poruku. Obično ćete ćete poruku ako naići neke pogreške, ali želite da biste pokušali ponovno obraditi poruku. Nasuprot tome, odbijanje poruka je odgovarajući kada naiđete na pogrešku nepopravljiva (ili jednostavno odlučite da ne želite da obradi poruku).

U svakom slučaju, imajte na umu različite povrata šifre tako da možete elicit ponašanje želite iz biblioteke **IoTHubClient** .

## <a name="alternate-device-credentials"></a>Vjerodajnice za svaki drugi uređaj

Kao što je prethodno opisano, prvo što učiniti kada radite s bibliotekom **IoTHubClient** je da biste nabavili u **IOTHUB\_KLIJENT\_RUKOVATI** s pozivom kao što je sljedeće:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Argumente **IoTHubClient\_CreateFromConnectionString** su niz za povezivanje naš uređaja i parametar koji označava protokol koristimo za komunikaciju s IoT koncentratora. Niz za povezivanje sadrži oblik koji se pojavljuje na sljedeći način:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Postoje četiri dijelovi informacija u ovom nizu: IoT koncentrator sufiks IoT koncentrator, ID uređaja, naziv i zajednički pristup ključ. Nabavite na potpuno kvalificirani naziv domene (FQDN) od koncentratora za IoT prilikom stvaranja vašoj instanci koncentrator IoT na portalu za Azure – to vam naziv IoT koncentrator (prvi dio FQDN) i koncentrator sufiks IoT (rest FQDN). Zatražite ID uređaja i tipkovni prečac zajednički koristiti kada registrirate uređaj s IoT koncentrator (kao što je opisano u [prethodni članak](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** daje jedan od načina za inicijalizaciju biblioteke. Ako želite, možete stvoriti novi **IOTHUB\_KLIJENT\_RUKOVATI** pomoću parametara pojedinačne umjesto niz za povezivanje. To možete postići s sljedeći kod:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Time se izvršava na isti način kao **IoTHubClient\_CreateFromConnectionString**.

Mogu prestati očite biste htjeli koristiti **IoTHubClient\_CreateFromConnectionString** umjesto ova više opširno metoda Inicijalizacija. Imajte na umu, međutim, kada uređaj registrirate u središte IoT što se je ID uređaja i ključ uređaja (ne niz za povezivanje). Alat za **Upravitelj uređaja** SDK uvedene u [prethodni članak](iot-hub-device-sdk-c-intro.md) koristi biblioteka **servisa Azure IoT SDK** za stvaranje niza za povezivanje iz ID uređaja, ključ uređaja i IoT koncentrator naziv glavnog računala. Tako da nazovete podršku za **IoTHubClient\_sve\_stvaranje** možda preporučuje jer ona sprema koraku generiranja niza za povezivanje. Pomoću metode praktičan je.

## <a name="configuration-options"></a>Konfiguracija mogućnosti

Dosad sve opisane o načinu rada biblioteku **IoTHubClient** odražava njegov zadano ponašanje. Međutim, dostupne su nekoliko mogućnosti koje možete postaviti da biste promijenili način funkcioniranja u biblioteku. To se postiže tako da korištenje u **IoTHubClient\_sve\_FALSE** API-JA. U ovom se primjeru Imajte na umu:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Postoji nekoliko mogućnosti koje se često koriste:

-   **SetBatching** (bool) – ako je **true**, a zatim podataka koji se šalju IoT koncentrator šalju u grupama. Ako je **false**, a zatim poruke šalju se pojedinačno. Zadana je vrijednost **false**. Imajte na umu da mogućnost **SetBatching** se primjenjuje samo na HTTP protokola, a ne protokola MQTT ili AMQP.

-   **Prekoračenje vremena** (nije potpisan int) – tu vrijednost predstavlja u milisekundama. Ako pošaljete zahtjev HTTP ili primanja odgovor traje dulje nego ovaj put, a zatim ističe vezu.

Važno je mogućnost batching. Prema zadanim postavkama biblioteke ingresses događaje pojedinačno (jedan događaj je ono što prođete da biste **IoTHubClient\_sve\_SendEventAsync**). Ako je mogućnost batching **true**, u biblioteku prikuplja proizvoljan broj događaje kao što možete iz međuspremnika (do najveće dopuštene veličine poruka koje će prihvatiti koncentrator IoT).  Grupe za događaj se šalje koncentrator IoT poziv u tijeku jedan HTTP (pojedinačne događaji su vezanoj instalaciji u JSON polja). Omogućivanje obično grupnog slanja promjena rezultira veliki performansi jer se smanjuje spajanje na mreži. Također znatno smanjuje propusnost Budući da šaljete jedan skup zaglavlja HTTP s grupe za događaj umjesto skup zaglavlja za svaki pojedinačne događaj. Osim ako imate određene razlog da biste to inače, obično ćete htjeti omogućiti grupnog slanja promjena.

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku detaljno opisuje način funkcioniranja biblioteke **IoTHubClient** pronađen na **uređaju Azure IoT SDK C**. Pomoću tih informacija koje je potrebno imati dobar razumijevanje mogućnosti **IoTHubClient** biblioteke. [Sljedeći članak](iot-hub-device-sdk-c-serializer.md) sadrži slične detalja za biblioteku **serijalizatora** .

Dodatne informacije o razvoju za IoT koncentrator potražite u članku [IoT koncentrator SDK-ovi][lnk-sdks].

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
