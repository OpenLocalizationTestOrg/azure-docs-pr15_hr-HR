<properties
    pageTitle="Azure IoT uređaja SDK za C - serijalizatora | Microsoft Azure"
    description="Dodatne informacije o korištenju biblioteka serijalizatora uređaja za Azure IoT SDK za C"
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

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-serializer"></a>Microsoft Azure IoT uređaja SDK za C – dodatne informacije o serijalizatora

[Prvog članka](iot-hub-device-sdk-c-intro.md) u ovoj seriji uvedena **Azure IoT uređaj SDK C**. U sljedećem članku dobili više detaljan opis [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md). U ovom se članku dovršava opseg SDK unosom detaljnije opis preostale komponente: **serijalizatora** biblioteke.

U uvodnom članku opisano kako koristiti biblioteku **serijalizatora** događaja za slanje i primanje poruka iz centra za IoT. U ovom se članku možemo proširiti tu raspravu unosom potpuniji objašnjenje kako se model podataka pomoću jezika **serijalizatora** makronaredbu. Članak sadrži više detalja o kako biblioteku serializes poruke (te u nekim slučajevima kako možete kontrolirati ponašanje serijalizacije). Ne možemo ćete opisuju neke parametre možete izmijeniti da biste odredili veličinu modela stvaranja.

Na kraju članka revisits neke teme obuhvaćeno prethodne članke kao što su poruke i rukovanja svojstvo. Kao što smo ćete saznati, tih značajki funkcionira na isti način korištenja biblioteke **serijalizatora** kao s bibliotekom **IoTHubClient** .

Sve što je opisano u ovom članku temelji se na SDK uzoraka **serijalizatora** . Ako želite praćenje izlaganja, pročitajte članak u **simplesample\_amqp** i **simplesample\_http** aplikacije sadrži uređaj Azure IoT SDK za C.

Možete pronaći **Azure IoT uređaj SDK C** u spremištu GitHub [Microsoft Azure IoT SDK-ovi](https://github.com/Azure/azure-iot-sdks) i prikaz pojedinosti u API [C API reference](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-modeling-language"></a>Modeliranje jezika

[Uvodni članak](iot-hub-device-sdk-c-intro.md) u ovoj seriji uvedene **Azure IoT uređaj SDK C** Modeliranje jezika kroz u primjeru u u **simplesample\_amqp** aplikacije:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Kao što vidite, Modeliranje jezika temelji se na C makronaredbi. Uvijek počinjete definicije s **Početak\_prostor naziva** i uvijek završavaju **ZAVRŠETKA\_prostor naziva**. Zajednička naziv naziva za svoju tvrtku ili, kao u ovom primjeru projekta kojim radite na je.

Što vodi unutar naziva su definicija modela. U ovom slučaju postoji jedan model za programa anemometer. I opet modela mogu biti pod nazivom ništa, ali obično je to naziva za uređaja ili vrste podataka koje želite razmijeniti s IoT koncentratora.  

Modeli sadrže definiciju događaja možete ingress IoT koncentrator ( *Podaci*) te poruke možete primati od središta IoT ( *Akcije*). Kao što možete vidjeti iz primjera, događaji imati vrstu i naziv; Akcije imati naziv i neobavezni parametri (svaki s vrstom).

Što je planirati u ovom primjeru su ostale vrste podataka koje podržava SDK-a. Ćemo objasniti te dalje.

> [AZURE.NOTE] Koncentrator IoT odnosi se na podatke na uređaju šalje ga kao *događaje*, dok je jezik za Modeliranje upućuje na njega kao *podataka* (definirano pomoću **WITH_DATA**). Isto tako, koncentrator IoT se odnosi na podatak pošaljete uređajima kao *poruke*, dok je jezik za Modeliranje upućuje na njega kao *Akcije* (definirano pomoću **WITH_ACTION**). Imajte na umu da izrazi mogu koristiti naizmjence u ovom članku.

### <a name="supported-data-types"></a>Podržane vrste podataka

Sljedeće vrste podataka podržane u modelima stvorene pomoću biblioteke **serijalizatora** :

| Vrsta                    | Opis                            |
|-------------------------|----------------------------------------|
| Dvostruki                  | dvostruke preciznosti broj s pomičnim zarezom |
| INT                     | 32-bitni cijeli broj                         |
| vrijednost s pomičnim zarezom                   | preciznost jedan broj s pomičnim zarezom |
| Dugi                    | Dugi cijeli broj                           |
| int8\_t                 | 8-bitni cijeli broj                          |
| int16\_t                | 16 bitnu cjelobrojnu                         |
| Int32\_t                | 32-bitni cijeli broj                         |
| Int64\_t                | 64-bitni cijeli broj                         |
| booleovom                    | Booleove vrijednosti                                |
| ASCII\_znak\_ptr        | ASCII niza                           |
| EDM\_datum\_vrijeme\_POMAK | Datum vrijeme offset                       |
| EDM\_GUID               | GUID                                   |
| EDM\_BINARNI             | Binarni.                                 |
| DEKLARIRANJE\_STRUCT         | Vrsta složenih podataka                      |

Započnimo s posljednje vrsta podataka. Na **OBJAVA\_STRUCT** omogućuje da odredite vrstama složenih podataka koji su grupiranja drugih vrsta jednostavne. Ta grupiranja dopustite nam da biste definirali modelu koji izgleda ovako:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Naš model sadrži jedan podatkovni događaj vrste **TestType**. **TestType** je složenoj koja sadrži nekoliko članova koje skupno demonstrirati jednostavne koje podržava **serijalizatora** Modeliranje jezika.

S modelom ovako, ne možemo možete pisati kod za slanje podataka u koncentratoru IoT koji se pojavljuje na sljedeći način:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

Zapravo, ne možemo trenutno korisnicima dodijelite vrijednost svakog člana **Test** strukture i pozivanje **SendAsync** da biste poslali događaj za **testiranje** podataka s oblakom. **SendAsync** je pomoćnik funkcija koja se šalje na događaj jedan podatkovni koncentrator IoT:

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Ova funkcija serializes događaj podataka i šalje IoT koncentrator pomoću **IoTHubClient\_SendEventAsync**. Ovo je isti kod koji se spominju u prethodno člancima (**SendAsync** encapsulates logiku u praktičan funkcije).

Jedan drugi Pomoćnik za funkciju koja se koristi u kodu prethodne je **GetDateTimeOffset**. Ova funkcija pretvara u određenom vremenskom u vrijednost vrste **EDM\_datum\_vrijeme\_POMAK**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Ako pokrenete kod, IoT koncentrator šalje se sljedeća poruka:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Imajte na umu da se serijalizacije JSON, koji je oblik generira **serijalizatora** biblioteke. Imajte na umu da odgovaraju svakom članu serijalizirani objekt JSON članovi **TestType** koji ste definirali u našem modelu. Vrijednosti i potpuno podudaraju s dimenzijama i koristi kod. Međutim, imajte na umu da binarne podatke base64 kodirani: "AQID" je u base64 kodiranja {0x01, 0x02, 0x03}.

U ovom se primjeru pokazuje prednost korištenja biblioteke **serijalizatora** --omogućuje nam da biste poslali JSON u oblak bez potrebe za izričito baviti serijalizacije u našem računala. Sve možemo morate brinuti o postavlja vrijednosti podataka događaja u našem modelu i pozivanje jednostavne API-ji za slanje događaje u oblak.

Pomoću tih informacija smo možete definirati modele koji obuhvaćaju raspon podržane vrste podataka, uključujući složene vrste (smo nije čak uključiti složene vrste unutar drugim vrstama složenih). Međutim, on serijalizirani JSON generira gornji primjer prikazuje važne točke. *Kako* mi šaljemo podataka s bibliotekom **serijalizatora** određuje točno onako kako je oblikovan u JSON. Kad određenom mjestu je što ćemo objasniti dalje.

## <a name="more-about-serialization"></a>Dodatne informacije o serijalizacije

U prethodnom odjeljku ističe primjera izlaz generira **serijalizatora** biblioteke. U ovom se odjeljku smo ćete u članku se objašnjava kako biblioteku serializes podataka te kako možete kontrolirati tog ponašanja pomoću serijalizacije API-ji.

Da biste prešli na raspravu na serijalizacije, ne možemo ćete raditi s novog modela na temelju na thermostat. Najprije ćemo pružaju malo pozadinskih o scenariju Pokušavamo na adresu.

Želimo modela thermostat koji mjeri temperature i humidity. Svaki podatak koji će se slati IoT koncentrator drukčije. Prema zadanim postavkama thermostat ingresses temperatura događaj podržani su jednom svaki dvije minute; humidity događaj je ingressed svakih 15 minuta. Kada je neki događaj ingressed, morate uključiti vremenska oznaka koja prikazuje vrijeme je mjeri odgovarajuće temperatura ili humidity.

Uz ovaj scenarij, ne možemo će demonstrirati dvije različite načine oblikovanja podataka i smo ćete objašnjavaju efekt na serijaliziranog izlaz ima Modeliranje.

### <a name="model-1"></a>Model 1

Evo prva verzija modelu koji podržava prethodnom scenariju:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Napomena da model sadrži dvije događaji s podatkovnim: **Temperature** i **Humidity**. Za razliku od prijašnjih primjera vrsta svaki događaj je struktura definirana pomoću **OBJAVA\_STRUCT**. **TemperatureEvent** obuhvaća temperatura mjera i vremenske oznake; **HumidityEvent** sadrži humidity mjera i vremenske oznake. Ovaj model omogućuje nam prirodnim način Modeliranje podataka u scenariju prethodno opisan za. Prilikom slanja događaja u oblak, ćemo ili poslati temperatura/vremenske oznake ili par humidity/vremenska oznaka.

Šaljemo temperatura događaja u oblak pomoću koda kao što je sljedeće:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Ne možemo koristit ću programiranih vrijednosti za temperature i humidity u primjeru koda, ali zamislite da ćemo se zapravo dohvaćanja te vrijednosti po uzorkovanje odgovarajuće senzori na na thermostat.

Gore navedeni kod koristi Pomoćnik za **GetDateTimeOffset** koji je prethodno uvedena. Kod razloga koji će postati poništite okvir kasnije izričito odvaja zadatak Serijalizacija i slanje događaj. Prethodni kod serializes temperatura događaja u međuspremnika. Nakon toga **Otprema** je funkcija preglednika (obuhvaćeno **simplesample\_amqp**) koje šalje događaj IoT koncentrator:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Kod je podskup Pomoćnik za **SendAsync** opisane u prethodnom odjeljku pa ćemo neće prijeđite preko njega ponovno ovdje.

Kada se ne možemo pokrenuti prethodne kod da biste poslali temperatura događaj, serijaliziranog obrazac događaja se šalje koncentrator IoT:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Ne možemo šaljete temperatura koje je vrste **TemperatureEvent** , a taj struct sadrži član **Temperature** i **vremena** . Ovo izravno prikazuje se serijaliziranog podataka.

Isto tako, šaljemo humidity događaj s kod:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Serijalizirani obrazac koji se šalju IoT koncentrator pojavit će se na sljedeći način:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Ponovno, to je prema očekivanjima.

Uz ovaj model možete zamisliti kako dodatno događaja možete jednostavno dodati. Definiranje više strukture pomoću **OBJAVA\_STRUCT**, i obuhvatiti modela pomoću odgovarajuće događaj **s\_podataka**.

Sada ćemo izmjena modela tako da obuhvaća iste podatke, ali s različitim strukturu.

### <a name="model-2"></a>Model 2

Ovaj zamjenski model onu iznad Imajte na umu:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

U ovom slučaju ne možemo ste ukloniti na **OBJAVA\_STRUCT** makronaredbe i jednostavno definiranje stavke podataka iz naših scenarij koriste jednostavne vrste iz Modeliranje jezika.

Samo za trenutka recimo Zanemari **vrijeme** događaja. S tog aside Evo koda za ingress **temperatura**:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Kod šalje sljedeći događaj serijaliziranog koncentrator IoT:

```
{"Temperature":75}
```

I kod za slanje događaj Humidity pojavljuje se na sljedeći način:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Kod šalje to koncentrator IoT:

```
{"Humidity":45}
```

Dosad postoje i dalje bez iznenađenja. Sada ćemo promijeniti kako koristimo SERIALIZE makronaredbu.

Makronaredba **SERIALIZE** može potrajati više događaji s podatkovnim kao argumente. Omogućuje nam zajedno serijalizirati **Temperature** i **Humidity** događaja i poslati ih IoT koncentrator u jedan poziva:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Možda pogoditi rezultat kod je dva podatkovni događaji šalju koncentrator IoT:

[

{"Temperatura": 75},

{"Humidity": 45}

]

Drugim riječima, možete očekivati kod je isti kao i slanje **Temperature** i **Humidity** zasebno. To je samo praktičnost za prosljeđivanje i događaje **SERIALIZE** u istom poziv. No to nije slučaj. Umjesto toga gore navedeni kod šalje ovaj događaj jedan podatkovni koncentrator IoT:

{"Temperatura": 75, "Humidity": 45}

To mogu prestati neobično jer našem modelu određuje **Temperature** i **Humidity** kao dva *zasebna* događaja:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Dodatne informacije na točku smo niste modela te događaje gdje **Temperature** i **Humidity** se u istu strukturu:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Ako koristi ovaj model bi bolje razumjeli kako **Temperature** i **Humidity** će biti poslane serijaliziranog poruka. No to možda neće biti Očisti Zašto funkcionira na taj način nakon što prođete događaji i podaci za **SERIALIZE** pomoću modela 2.

Takvo ponašanje pregledniji je ako znate pretpostavke koje čini **serijalizatora** biblioteke. Da biste uvid u to prođimo natrag na našem modelu:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Zamislite da je ovaj model rječnikom vezanima uz objekt. U ovom slučaju ne možemo se Modeliranje fizički uređaj (thermostat), a uređaj sadrži atribute kao što su **Temperature** i **Humidity**.

Ne možemo možete poslati cijelu stanje našem modelu kod kao što je sljedeće:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Pod pretpostavkom da vrijednosti temperatura, postavite Humidity i vrijeme, ne možemo vidjeti događaja ovako poslane IoT koncentrator:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Ponekad može samo želite poslati *neka* svojstva model s oblakom (to vrijedi osobito ako model sadrži veliki broj događaja podataka). Da biste poslali samo podskup podataka događaja, kao što su u našem primjeru starijim koristan je:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

To generira točno istom serijaliziranog događaja kao da ste definirali **TemperatureEvent** s člana **Temperature** i **vremena** , kao što smo jeste li s modelom 1. U ovom slučaju nismo moći generirati točno istom serijaliziranog događaja pomoću različitih modela (modela 2) jer smo naziva **SERIALIZE** na drugačiji način.

Točke važno je da ako više događaji podataka da biste **SERIALIZE,** a zatim pretpostavlja da svaki događaj je svojstvo u jedan objekt JSON.

Najbolji način ovisi o vi i kako razmislite o modelu. Ako šaljete "događaja" u oblak i svaki događaj sadrži definirani skup svojstava, prvom pristupu čini mnogo odgovara. U tom slučaju to koristite **OBJAVA\_STRUCT** da biste definirali strukturu svaki događaj, a zatim ih u modelu s na **s\_podataka** makronaredbe. Zatim je da se svaki događaj poslati kao smo u prvom primjeru iznad. Na taj se način želite samo prenesite jedan podatkovni događaj **SERIJALIZATORA**.

Ako mislite o modelu način za vezanima uz objekta, pa drugi pristup možda potrebama. U ovom slučaju elemente definirati pomoću **s\_podataka** "Svojstva" objekta. Prenesite bilo kakve podskup događaja **SERIALIZE** koji vam se sviđa, ovisno o tome koliko je stanja sustava "objekta" koju želite poslati u oblak.

Nether pristup je udesno ili redu. Samo Imajte na umu funkcioniranje biblioteku **serijalizatora** , a zatim odaberite pristup Modeliranje koji najbolje odgovara vašim potrebama.

## <a name="message-handling"></a>Rukovanje porukama

U ovom se članku dosad sadrži samo spominju slanje događaji IoT koncentrator, a nije adresirana primanje poruka. Razlog za to je da moramo znati o primanje poruka sadrži uglavnom nije obuhvaćeno [starijim članka](iot-hub-device-sdk-c-intro.md). Opoziv od članak obradu poruka po Registracija funkciju povratnog poruka:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Zatim napišite funkcija povratnog koja se poziva primitku poruke:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Ova implementacija **IoTHubMessage** poziva određene funkcije za svaku akciju u modelu. Na primjer, ako model definira Ta akcija:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Morate definirati funkcija uz taj potpis:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

**SetAirResistance** naziva se prilikom slanja poruke na uređaj.

Što smo niste objašnjenje još je izgleda serijaliziranog verziju poruke. Drugim riječima, ako želite poslati poruku **SetAirResistance** uređaju kako koji izgleda?

Ako šaljete poruku s uređajem, što biste učinili putem servisa Azure IoT SDK. I dalje morate znati koji niz da biste poslali pozvati određenu akciju. Opći oblik za slanje poruka pojavljuje se na sljedeći način:

```
{"Name" : "", "Parameters" : "" }
```

Šaljete serijalizirani objekt JSON s dva svojstva: je **naziv** Akcije (poruke), a zatim **parametara** sadrži parametre akciju.

Ako, na primjer, za pozivanje **SetAirResistance** ovu poruku možete poslati na uređaj:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

Naziv akcije mora u potpunosti odgovarati akcije definirane u modelu. Nazivi parametara kao i moraju se podudarati. Imajte na umu i velikih ili malih. **Naziv** i **Parametri** uvijek razlikuju velika slova. Provjerite odgovara veličini slova naziva akcije i parametrima u modelu. U ovom se primjeru naziv akcije je "SetAirResistance" i nije "setairresistance".

U ovom se odjeljku opisuje sve što trebate znati kada događaje za slanje i primanje poruka s bibliotekom **serijalizatora** . Prije no što isprobate, recimo obuhvaća neke parametre možete konfigurirati koja određuju kako velike je model.

## <a name="macro-configuration"></a>Akcija makronaredbe konfiguracija

Ako koristite biblioteku **serijalizatora** važan dio SDK morate imati na umu nalazi se u biblioteci azure-c-zajednički-utility.
Ako ste klonirana spremište Azure-iot-SDK-ovi s GitHub pomoću mogućnosti – rekurzivne, ćete pronaći zajedničke utility biblioteke ovdje:

```
.\\c\\azure-c-shared-utility
```

Ako ste ne klonirana u biblioteku, to možete pronaći [ovdje](https://github.com/Azure/azure-c-shared-utility).

U biblioteci zajedničkih utility vidjet ćete sljedeće mape:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Ova mapa sadrži Visual Studio rješenje pod nazivom **makronaredbe\_uslužni\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

Program u ovom rješenju generira na **makronaredbe\_utils.h** datoteku. Postoji zadane makronaredbe\_utils.h datoteku s SDK-a. Ovo je rješenje omogućuje izmjena neke parametre, a zatim ponovno stvoriti datoteku zaglavlja na temelju parametara.

Dva ključa parametra biti zamaraju su **nArithmetic** i **nMacroParameters** koji su definirani u te dva retka u makronaredbe\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Ove vrijednosti su zadani parametri uključene SDK-a. Svaki parametar sastoji se od sljedećih značenja:

-   nMacroParameters – određuje koliko parametre koje imate u jedan OBJAVA\_definicije makronaredbe MODELA.

-   nArithmetic – kontrole ukupan broj članova dopušteno u modelu.

Razlog parametara su važne jest jer oni kontrolirati veličinu modela. Na primjer, razmotrite ove definicije modela:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Kao što je rečeno prethodno, **OBJAVA\_MODELA** samo makronaredbe C. Nazivi modela i **s\_podataka** izjava (još druge makronaredbe) su parametri **OBJAVA\_MODELA**. **nMacroParameters** određuje koliko parametara mogu se uključiti u **OBJAVA\_MODELA**. Učinkovit određuje koliko podataka događaja i akcija deklaracije može imati. Kao takve, s Zadano ograničenje od 124 to znači da definirate model s kombinacijom otprilike 60 akcije i događaji podataka. Ako pokušate prekoračuju to ograničenje, primit ćete compiler pogrešaka koje izgledaju ovako:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

Parametar **nArithmetic** pronaći dodatne informacije o Interna workings jezika makronaredbe od aplikacija.  Ona određuje ukupan broj članova imate u modelu, uključujući **DECLARE_STRUCT** makronaredbi. Ako pokrenete prikazuju pogreške alata za Kompiliranje kao što su ova, pa pokušajte povećati **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Ako želite promijeniti parametara izmijenite vrijednosti u makronaredbu\_utils.tt datoteka, prevoditi makronaredbu\_uslužni\_h\_generator.sln rješenja, a zatim pokrenite prevedeni program. Kada to učinite, novu makronaredbu\_utils.h datoteka je generiran i potvrdili u. \\uobičajenih\\inc direktorija.

Da biste mogli koristiti novu verziju makronaredbe\_utils.h, uklanjanje paketa NuGet **serijalizatora** rješenje i umjesto nje obuhvaćaju projekta za Visual Studio **serijalizatora** . Time se omogućuje kod prikupiti protiv šifru izvora serijalizatora biblioteke. To obuhvaća ažurirane makronaredbu\_utils.h. Ako želite učiniti za **simplesample\_amqp**, najprije NuGet paket za biblioteku serijalizatora uklanjanjem rješenja:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Zatim dodajte projekta za Visual Studio rješenje:

> . \\c\\serijalizatora\\sastavljanje\\windows\\serializer.vcxproj

Kada završite, rješenje trebao bi izgledati ovako:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Sada kada Kompiliranje rješenje, ažurirane makronaredbe\_utils.h uvrštava u vašem binarnom obliku.

Imajte na umu da povećate te vrijednosti dovoljno visoku može biti veći od ograničenja prevoditelj (Compiler). Da biste tog trenutka **nMacroParameters** je parametar glavni s kojim se brinuti. Specifikacija C99 određuje u definiciji makronaredbe dopušteno najmanje 127 parametara. Microsoft alata za Kompiliranje točno slijedi u specifikacija (te ima ograničenje od 127) tako da ih nećete moći da biste povećali **nMacroParameters** izvan zadani. Ne mogu omogućiti drugim compilers (na primjer, alata za Kompiliranje GNU podržava veća ograničenja).

Dosad smo ste prekriveno gotovo sve što trebate znati o pisanju kod s bibliotekom **serijalizatora** . Prije nego što concluding, recimo ponovo posjetite neke teme iz prethodne članaka koji vam mogu biti pitate li se o.

## <a name="the-lower-level-apis"></a>API-ji niže razine

Primjer aplikacije u ovom se članku odnosila je **simplesample\_amqp**. Ovaj primjer koristi s više razine (u ne-"sve") API-ji događaje za slanje i primanje poruka. Ako koristite ove API-ji, pozadine niti pokreće koji vodi brigu o i događaje poruke slati i primati. Međutim, možete koristiti API-ji niže razine (sve) da biste uklonili ovaj niti pozadine, a zatim snimite eksplicitnih kontrolu nad prilikom slanja događaje ili primanja poruka iz oblaka.

Kao što je opisano u [prethodni članak](iot-hub-device-sdk-c-iothubclient.md)postoji skup funkcije koji se sastoji od više razine API-ji:

-   IoTHubClient\_CreateFromConnectionString

-   IoTHubClient\_SendEventAsync

-   IoTHubClient\_SetMessageCallback

-   IoTHubClient\_uništiti

Ove API-ji su planirati **simplesample\_amqp**.

Postoji i u analogna skup API-ji niže razine.

-   IoTHubClient\_sve\_CreateFromConnectionString

-   IoTHubClient\_sve\_SendEventAsync

-   IoTHubClient\_sve\_SetMessageCallback

-   IoTHubClient\_sve\_uništiti

Imajte na umu da niže razine API-ji funkcioniraju na isti način kao što je opisano u člancima prethodne. Prvi skup API-je možete koristiti ako želite da se niti pozadine za rukovanje događajima za slanje i primanje poruka. Drugi skup API-ji koristite ako želite eksplicitnih kontrolu nad kada slanje i primanje podataka iz centra za IoT. Neki skup API-ji rad ravnomjerno i s bibliotekom **serijalizatora** .

Primjer korištenja API-ji niže razine s bibliotekom **serijalizatora** , potražite u članku u **simplesample\_http** aplikacije.

## <a name="additional-topics"></a>Dodatne teme

Nekoliko druge teme vrijedi Spominjanje ponovno su svojstvo rukovanja, pomoću vjerodajnica za svaki drugi uređaj i mogućnosti konfiguracije. To su svih tema vezanih uz obuhvaćeno [prethodni članak](iot-hub-device-sdk-c-iothubclient.md). Glavne poante je li sve od ovih značajki funkcionira na isti način s bibliotekom **serijalizatora** kao i s bibliotekom **IoTHubClient** . Na primjer, ako želite priložiti svojstva događaj iz modela koristite **IoTHubMessage\_svojstva** i **karte**\_**AddorUpdate**, na isti način kao što je prethodno opisano:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Li događaj generiran iz biblioteke **serijalizatora** ili stvoriti ručno pomoću biblioteke **IoTHubClient** bitan.

Pod stavkom vjerodajnice za svaki drugi uređaj, pomoću **IoTHubClient\_sve\_stvaranje** jednako dobro kao **IoTHubClient\_CreateFromConnectionString** za dodjeljivanje programa **IOTHUB\_KLIJENT\_RUKOVATI**.

Na kraju, ako koristite biblioteku **serijalizatora** , možete postaviti mogućnosti konfiguracije s **IoTHubClient\_sve\_FALSE** baš kao kada se koristi biblioteka **IoTHubClient** .

Značajke koje su jedinstvene biblioteku **serijalizatora** su API-ji za inicijalizaciju. Prije no što počnete raditi s bibliotekom, morate nazvati **serijalizatora\_init**:

```
serializer_init(NULL);
```

To možete učiniti prije pozivanja **IoTHubClient\_CreateFromConnectionString**.

Isto tako, nakon što završite rad s bibliotekom, posljednji poziv ćete je **serijalizatora\_deinit**:

```
serializer_deinit();
```

U suprotnom sve navedene ostalih značajki funkcionira na isti način u biblioteci **serijalizatora** kao u biblioteci **IoTHubClient** . Dodatne informacije o bilo kojem od ovih tema potražite u članku [prethodni članak](iot-hub-device-sdk-c-iothubclient.md) u ovoj seriji.

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku opisuju detaljno jedinstveni aspekte **serijalizatora** biblioteke koje se nalaze na **uređaju Azure IoT SDK C**. S informacijama dana koje je potrebno imati dobro upoznavanje sa kako pomoću modela događaje za slanje i primanje poruka iz centra za IoT.

To također završava niz tri dijela o razvoju aplikacija **Azure IoT uređaj SDK C**. To mora biti dovoljno informacija ne samo početak, ali vam temeljito objašnjenje funkcioniranja API-ji. Dodatne informacije potražite postoji nekoliko primjera SDK ne obuhvaća ovdje. U suprotnom je [SDK dokumentaciji](https://github.com/Azure/azure-iot-sdks) dobar izbor za dodatne informacije.


Dodatne informacije o razvoju za IoT koncentrator potražite u članku [IoT koncentrator SDK-ovi][lnk-sdks].

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
