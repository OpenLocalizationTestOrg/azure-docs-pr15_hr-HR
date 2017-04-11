<properties
    pageTitle="Korištenje uređaja Azure IoT SDK za C | Microsoft Azure"
    description="Dodatne informacije o i početak rada s uzorak koda uređaja za Azure IoT SDK za C."
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

# <a name="introducing-the-azure-iot-device-sdk-for-c"></a>Uvod u uređaj Azure IoT SDK za C

**Azure IoT uređaj SDK** je skup biblioteke dizajniran da biste pojednostavili postupak slanje događaja i primanju poruka od servisa **Azure IoT koncentratora** . Postoje različitih varijacija SDK svaki ciljanja određene platforme, no u ovom se članku opisuje **Azure IoT uređaj SDK C**.

Azure IoT uređaj SDK C napisan ANSI C (C99) da biste maksimizirali prenosivost. Time se omogućuje dobro prikladniji da biste upravljali broj platforme i uređaji, osobito kada minimiziranje disk, a ostavlja manji trag pri memorije je prioritet.  

Postoji mnoštvo platformama na kojem SDK testirano (pogledajte u [Azure Certificirano za katalog uređaj IoT](https://catalog.azureiotsuite.com/) detalje). Iako ovaj članak sadrži vodiči uzorka kod koji se izvodi na platformi Windows, imajte na umu da kod opisani u ovom članku je točno onaj preko raspona podržane platforme.

U ovom članku koji će se sa servisima arhitektura uređaja za Azure IoT SDK za C. Ne možemo ćete Demonstracija Inicijalizacija biblioteku uređaju, pošaljite događaji IoT koncentrator kao i poruka iz nje. Informacije u ovom članku mora biti dovoljno da biste počeli koristiti SDK, no nudi pokazivača na dodatne informacije o bibliotekama.

## <a name="sdk-architecture"></a>Arhitektura SDK

Možete pronaći **Azure IoT uređaj SDK C** u spremištu GitHub [Microsoft Azure IoT SDK-ovi](https://github.com/Azure/azure-iot-sdks) i prikaz pojedinosti u API [C API reference](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

Najnoviju verziju biblioteke možete pronaći u **glavni** granu ovo spremište:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

Ovo spremište sadrži cijelu obitelj Azure IoT uređaja SDK-ovi. Međutim, ovaj je članak o uređaju Azure IoT SDK *za C* koje možete pronaći u mapi **c** .

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

* Implementacija core SDK pronaći ćete u u **iothub\_klijent** mapu koja sadrži implementacije najniže sloj API-JA u SDK: **IoTHubClient** biblioteke. Biblioteka **IoTHubClient** sadrži API-ji implementacijom neobrađenog poruka za slanje poruka IoT koncentrator kao i primanje poruka iz nje. Kada koristite tu biblioteku, ste odgovorni za implementaciju serijalizacije poruke (ipak se pomoću uzorka serijalizatora opisanim), ali druge pojedinosti komunikaciju s IoT koncentrator rukuje umjesto vas.
* Mapa **serijalizatora** sadrži Pomoćnik za funkcije i uzorke pokazuje kako serijalizirati podataka prije no što pošaljete Azure IoT koncentrator pomoću biblioteke klijenta. Imajte na umu da koristite serijalizatora nije obavezna i samo kao praktičnosti. Ako koristite biblioteku **serijalizatora** , najprije definiranje modelu koji određuje događaje koje želite poslati IoT koncentrator, kao i poruke očekujete da ćete dobiti iz nje. Kada je definiran model, SDK omogućuje površina za API koja omogućuje olakšan rad sa događaja i poruke bez brige o detaljima serijalizacije.
Biblioteku ovisi o drugih biblioteka Otvori izvor koji implementira prijenosa pomoću nekoliko protokola (MQTT, AMQP).
* Biblioteka **IoTHubClient** ovisi o drugih biblioteka Otvori izvor:
   * [Azure C zajednički se koristi za](https://github.com/Azure/azure-c-shared-utility) biblioteku koja nudi uobičajenih funkcije za osnovne zadatke (kao što je niz, a zatim na popisu rukovanje, IO, itd...) potrebno preko nekoliko vezane uz Azure C SDK-ovi
   * [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) biblioteke koja je implementacija strani klijenta AMQP optimizirane za uređaje ograničenja resursa.
   * Biblioteka za [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) koji je općenite namjene biblioteka implementacijom protokol MQTT i optimizirane za uređaje ograničenja resursa.

Sve to pregledniji je tako da pogledate kod uzorka. U sljedećim odjeljcima vode vas kroz nekoliko probne aplikacije koji su uključeni u SDK-a. To mora vam dati dobar dojam za razne mogućnosti arhitektonski Slojevi SDK, kao i Uvod u rad API-ji.

## <a name="before-running-the-samples"></a>Prije pokretanja uzorcima

Prije pokretanja uzorcima uređaja za Azure IoT SDK za C morate stvoriti instancu servisa Azure pretplate na ako već ne postoji i 2 zadatke:
* Priprema razvojno okruženje
* Pribavljanje vjerodajnica uređaja.

Ako morate stvoriti instancu Azure IoT koncentrator Azure pretplate, slijedite upute [u nastavku](https://github.com/Azure/azure-iot-sdks/blob/master/doc/setup_iothub.md).

[Datoteke readme](https://github.com/Azure/azure-iot-sdks/tree/master/c) uključene SDK sadrži upute za pripremu razvojno okruženje i Pribavljanje vjerodajnica uređaja.
U sljedećim se odjeljcima uključiti neke dodatne komentarom na te upute.

### <a name="preparing-your-development-environment"></a>Priprema razvojno okruženje

Dok paketa služe za neke platforme (kao što su NuGet za Windows ili apt_get za Debian i Ubuntu) i uzorcima koristite te pakete kada je dostupna, u upute detaljno opisuje sastavljanje biblioteke i primjere izravno obrazaca kod.

Najprije morate nabaviti kopiju SDK iz GitHub i zatim izraditi izvorišnog web-mjesta. Trebali biste poslati kopiju izvorišnog web-mjesta iz **glavnog** granu [GitHub spremište](https://github.com/Azure/azure-iot-sdks).

Kada preuzmete kopiju izvorišnog web-mjesta, morate poduzeti korake opisane u članku SDK ["Pripremi razvojno okruženje"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md).


Slijedi nekoliko savjeta koji će vam pomoći u obavljanju postupak opisan u vodič za pripremu:

-   Kada instalirate uslužni **CMake** , odaberite mogućnost dodavanja **CMake** sustav put za **sve korisnike** (Dodavanje kao i radi **trenutnog korisnika** ):

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)


-   Prije nego što otvorite **naredbeni redak za razvojne inženjere za VS2015**, instalirajte alati naredbenog retka brojka. Da biste instalirali te alate, slijedite sljedeće korake:

    1. Pokrenite instalacijski program **Visual Studio 2015** (ili odabrali **Microsoft Visual Studio 2015** s upravljačke ploče **Programi i značajke** te odaberite **Promijeni**).
    
    2. Provjerite je li značajka **Brojka za Windows** odabran u instalacijski program, ali i trebali biste provjeriti mogućnost **GitHub proširenje za Visual Studio** omogućuje integraciju IDE:

        ![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

    3. Dovršite Čarobnjak za postavljanje da biste instalirali alate.

    4. Dodajte imenik za **smeće** brojka Alati varijablu okruženja sustava **put** . U sustavu Windows, to izgleda ovako:

        ![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)


Kada dovršite sve korake opisane u stranici ["Priprema razvojno okruženje"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md) , spremni ste za Kompiliranje probne aplikacije.

### <a name="obtaining-device-credentials"></a>Nabavljanje vjerodajnice uređaja

Sad kad razvojno okruženje postavljeno, na sljedeći način da biste učinili je skup vjerodajnica za uređaj.  Za uređaj da biste mogli pristupiti koncentratora za IoT, najprije morate dodati uređaj registar uređaj IoT koncentratora. Kada dodate uređaju prikazat će se skup vjerodajnica uređaja koje morate na umu da uređaj da biste se mogli povezati s koncentratora za IoT. Ogledna aplikacije koje ćemo ćete pogledajte u sljedećem odjeljku očekivati te vjerodajnice u obliku **niza za povezivanje uređaja**.

Postoji nekoliko Alati u spremištu Otvori izvor SDK omogućuju upravljanje središtu IoT. Jedna je sa sustavom Windows na node.js utemeljen unakrsni platformu EŽA alat pod nazivom iothub explorer je aplikacija pod nazivom programa Explorer uređaj na drugu. Dodatne informacije o tim alatima [ovdje](https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md).

Kao što su smo slijedeći u ovom članku uzorcima sustavom Windows, smo koristite alat za uređaj Explorer. No iothub explorer možete koristiti i ako biste radije EŽA Alati.

Alat za [Uređaj Explorer](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer) koristi biblioteka servisa Azure IoT za različite funkcije na središte IoT, uključujući dodavanje uređaja. Ako koristite uređaj Explorer da biste dodali uređaj, prikazat će se odgovarajući niz za povezivanje. Potrebno je niz veze da biste uzorak pokretanje aplikacije.

U slučaju da niste već upoznati s postupkom, u nastavku opisuju kako koristiti Explorer uređaj da biste dodali uređaj i dohvaćanje niza za povezivanje uređaja.

Možete pronaći Windows installer za alat za uređaj Explorer na [SDK pustite stranice](https://github.com/Azure/azure-iot-sdks/releases). No možete i pokrenuti alat izravno iz njegov kod otvaranja **[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)** u **Visual Studio 2015** i izrade rješenja.

Kada pokrenete program prikazat će vam se ova sučelja:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Unesite **Niz za povezivanje koncentrator IoT** u prvo polje, a zatim kliknite **Ažuriraj**. Alat za to konfigurira tako da ga možete komunicirati s IoT koncentratora.

Kada je konfiguriran koncentrator IoT niz za povezivanje kliknite karticu **Upravljanje** :

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Ovo je gdje ćete upravljanje uređajima koji je registriran u koncentratora za IoT.

Na uređaju možete stvoriti tako da kliknete gumb **Stvori** . Prikazat će se dijaloški okvir s skupa unaprijed unesene tipke (primarnih i sekundarnih). Sve što morate učiniti jest unesite **ID uređaja** , a zatim kliknite **Stvori**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Nakon stvaranja uređaja na popisu uređaja ažurira se sa svim uređajima registrirani, uključujući onaj koji ste upravo stvorili. Ako desnom tipkom miša kliknite novi uređaj, prikazat će se izbornik:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Ako odaberete mogućnost **kopirajte niz za povezivanje za odabrani uređaj** , niz za povezivanje za svoj uređaj kopirali u međuspremnik. Zadrži kopiju niz za povezivanje. Morat ćete je kada se pokrene probne aplikacije koje se što je opisano u sljedećim odjeljcima.

Nakon što ste dovršili gore navedene korake, spremni ste za pokretanja neke kod. Oba uzoraka imati konstante pri vrhu glavnog izvornu datoteku koja omogućuje vam da biste unijeli niz za povezivanje. Ako, na primjer, retka iz u **iothub\_klijent\_uzorka\_amqp** aplikacije pojavit će se na sljedeći način.

```
static const char* connectionString = "[device connection string]";
```

Ako želite slijediti, unesite uređaj nizu za povezivanje ovdje, prevoditi rješenje pa ćete moći pokrenuti uzorka.

## <a name="iothubclient"></a>IoTHubClient

Unutar u **iothub\_klijent** mapa u spremištu azure-iot-SDK-ovi je **uzoraka** mapu koja sadrži aplikaciju pod nazivom **iothub\_klijent\_uzorka\_amqp**.

Verziji sustava Windows u **iothub\_klijenta\_uzorka\_ampq** aplikacije obuhvaća sljedeće Visual Studio rješenja:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

Ovo rješenje sadrži jedan projekt. To vrijedi sudjelovanje da postoje četiri NuGet paketi instalirani na rješenja:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.uamqp

Uvijek morate paket **Microsoft.Azure.C.SharedUtility** kada radite s SDK-a. Budući da se ovaj uzorak ovisi o AMQP, morate uključiti i **Microsoft.Azure.uamqp** i **Microsoft.Azure.IoTHub.AmqpTransport** paketa (su ekvivalentan paketa za MQTT i HTTP-a). Budući da uzorka koristi **IoTHubClient** biblioteke, morate uključiti i **Microsoft.Azure.IoTHub.IoTHubClient** paketa u rješenje.

Možete pronaći implementaciju aplikacije uzorak u **iothub\_klijent\_uzorka\_amqp.c** izvornu datoteku.

U ovom primjeru aplikacije ćemo koristiti za vas voditi kroz što je potrebno da biste koristili biblioteku **IoTHubClient** .

### <a name="initializing-the-library"></a>Pokretanje u biblioteku

> [AZURE.NOTE] Prije početka rada s bibliotekama, možda ćete morati izvršiti neke određene Inicijalizacija platforme. Ako, na primjer, ako planirate koristiti AMQP na Linux morate pokrenuti OpenSSL biblioteke. Uzorci u [spremištu GitHub](https://github.com/Azure/azure-iot-sdks) poziva funkcija utility **platform_init** kada klijent pokreće i pozovite funkciju **platform_deinit** prije zatvaranja. Ove funkcije deklarirane su u datoteci zaglavlje "platform.h". Trebali biste pregledajte definicije ove funkcije za svoju platformu cilj u [spremištu](https://github.com/Azure/azure-iot-sdks) da biste odredili trebate li uključiti kod Inicijalizacija platformu u klijentu.

Za početak rada s bibliotekama morate najprije dodijeliti ručicu za klijent koncentrator IoT:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Imajte na umu ćemo se ova funkcija (onu smo dobivenog uređaj Explorer) prosljeđivanje kopiju naš niz za povezivanje uređaja. Ne možemo označiti i protokol koji želite koristiti. U ovom se primjeru koristi AMQP, ali MQTT i HTTP su i mogućnost.

Kada imate valjani **IOTHUB\_KLIJENT\_RUKOVATI**, možete pokrenuti pozivanje API-ji događaje za slanje i primanje poruka iz centra za IoT. Ne možemo ćete potražite taj dalje.

### <a name="sending-events"></a>Slanje događaja

Slanje događaji IoT koncentrator zahtijeva sljedeće korake:

Prvo, stvorite poruku:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_Over_AMQP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText);
```

Nakon toga poslati poruku:

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Svaki put kada pošaljete poruku, navedite Referenca funkcije povratnog koja se poziva prilikom slanja podataka:

```
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %d with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Imajte na umu poziv na **IoTHubMessage\_uništenje** funkcija kada završite s porukom. Provjerite ovog poziva da biste oslobodili resurse dodijeliti prilikom stvaranja poruke.

### <a name="receiving-messages"></a>Primanje poruka

Poruka je asinkrona operacija. Najprije registrirate povratnog treba pozvati kada uređaj prima poruku:

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Zadnji parametar nije Poništi pokazivač na što god želite. U uzorku, je pokazivač u cijeli broj, ali može biti pokazivač složenije struktura podataka. Time se omogućuje funkciju povratnog da biste upravljali zajedničkog stanja s pozivateljem ove funkcije.

Kada uređaj primi poruku, funkciju registrirani povratnog se poziva:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) == IOTHUB_MESSAGE_OK)
    {
        (void)printf("Received Message [%d] with Data: <<<%.*s>>> & Size=%d\r\n", *counter, (int)size, buffer, (int)size);
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Imajte na umu da koristite na **IoTHubMessage\_GetByteArray** funkcija za dohvaćanje poruka koje u ovom primjeru je niz.

### <a name="uninitializing-the-library"></a>Uninitializing u biblioteku

Kada završite događaje za slanje i primanje poruka, možete poništiti inicijalizaciju IoT biblioteke. Da biste to učinili, problema poziv za sljedeće funkcije:

```
IoTHubClient_Destroy(iotHubClientHandle);
```

To oslobađa resurse prethodno dodijeliti tako da na **IoTHubClient\_CreateFromConnectionString** (opis funkcije).

Kao što vidite, jednostavno je događaje za slanje i primanje poruka s bibliotekom **IoTHubClient** . Biblioteku rukuje detalje o komunikaciju s IoT koncentrator, uključujući protokol za korištenje (iz perspektive programer, to je mogućnost jednostavne konfiguracije).

Biblioteka **IoTHubClient** i omogućuje preciznije kontrolirati kako serijalizirati događaje uređaju šalje IoT koncentrator. U nekim slučajevima to je prednost, ali u drugim slučajevima to je implementacija Detalji o kojima ne želite da se brinuti. Ako je to slučaj, razmislite o korištenju biblioteke **serijalizatora** koje ćemo ćete opisuju u sljedećem odjeljku.

## <a name="serializer"></a>Serijalizatora

Pojmovno biblioteku **serijalizatora** se nalazi pri vrhu **IoTHubClient** biblioteke u SDK-a. Koristi **IoTHubClient** biblioteka za podlozi komunikaciju s IoT koncentrator, ali dodaje mogućnosti modeliranja ukloniti teret Postupanje s porukom serijalizacije proizvođača. Po čemu se ova biblioteka funkcionira najbolje što je prikazano u primjeru.

Unutar **serijalizatora** je mapa u spremištu azure-iot-SDK-ovi **uzoraka** mapu koja sadrži aplikaciju pod nazivom **simplesample\_amqp**. Verziju sustava Windows ovaj uzorak obuhvaća sljedeće Visual Studio rješenja:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

Kao i kod prethodni uzorak ove sadrži nekoliko pakete NuGet:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.IoTHub.Serializer
- Microsoft.Azure.uamqp

Smo vidjeli Većina tih u prethodni uzorak, ali je **Microsoft.Azure.IoTHub.Serializer** novo. To je potrebna kada koristimo **serijalizatora** biblioteke.

Možete pronaći implementaciju aplikacije uzorak u **simplesample\_amqp.c** datoteku.

U sljedećim odjeljcima vode vas kroz glavne dijelove ovaj uzorak.

### <a name="initializing-the-library"></a>Pokretanje u biblioteku

Da biste započeli rad s bibliotekom **serijalizatora** , mora se pozvati API-ji za inicijalizaciju:

```
serializer_init(NULL);

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);

ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
```

Poziv u **serijalizatora\_init** funkcija jednokratni pozive i služi za pokretanje u podlozi biblioteku. Nakon toga poziva na **IoTHubClient\_CreateFromConnectionString** funkciju, koja je isti API kao **IoTHubClient** uzorka. U ovom poziv postavlja niz za povezivanje uređaja (to je i gdje odaberite protokol koji želite koristiti). Imajte na umu da ovaj primjer koristi AMQP kao prijenos, ali nije moguće koristiti HTTP.

Na kraju, poziva na **Stvaranje\_MODELA\_INSTANCU** (opis funkcije). Imajte na umu da **WeatherStation** prostora za naziv modela i **ContosoAnemometer** je naziv modela. Nakon stvaranja instancu model, možete je koristiti da biste pokrenuli događaje za slanje i primanje poruka. Međutim, važno je znati koje model je.

### <a name="defining-the-model"></a>Definiranje modela

Model u biblioteci **serijalizatora** definira događaje koje uređaju možete poslati IoT koncentratora i poruke, pod nazivom *Akcije* u Modeliranje jezika koji se mogu primati. Definiranje modela pomoću skupa C makronaredbi kao u u **simplesample\_amqp** poslušajte aplikacije:

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

Na **ZAPOČETI\_prostor naziva** i **ZAVRŠETKA\_prostor naziva** makronaredbe i iskoristite prostora za naziv modela kao argument. Očekuje ništa između te makronaredbe je definiciju vaše model(s) i strukture podataka koje koriste u modelima.

U ovom primjeru postoji jedan model pod nazivom **ContosoAnemometer**. Ovaj model definira dva događaja uređaju možete poslati koncentrator IoT: **DeviceId** i **WindSpeed**. Definira i tri Akcije (poruke) koji možete primati uređaju: **TurnFanOn**, **TurnFanOff**i **SetAirResistance**. Svaki događaj sadrži vrstu i svaku akciju ima naziv (i po želji skupa parametara).

Događaji i akcije definirane u modelu definirati površina za API koje možete koristiti da biste poslali događaji IoT koncentrator, kao i odgovaranje na poruke koja se šalje na uređaju. Ovo je najbolje prepoznata do primjera.

### <a name="sending-events"></a>Slanje događaja

Model definira događaje koje možete poslati IoT koncentratora. U ovom primjeru, to znači da jedan od dva događaja definirati pomoću makronaredbe **WITH_DATA** . Na primjer, ako želite poslati **WindSpeed** događaj programa IoT koncentrator postoje nekoliko koraka koji je uključen u mijenjanju to dogodi. Prvi je da biste postavili podataka ne možemo želite poslati:

```
myWeather->WindSpeed = 15;
```

Model definirali ranije omogućuje nam da biste to učinili postavljanjem član **struct**. Zatim ćemo serijalizirati događaj smo želite poslati:

```
unsigned char* destination;
size_t destinationSize;

SERIALIZE(&destination, &destinationSize, myWeather->WindSpeed);
```

Kod serializes događaja u međuspremnik (referencira **odredište**). Na kraju, ćemo poslati događaj IoT koncentrator s kod:

```
sendMessage(iotHubClientHandle, destination, destinationSize);
```

To je funkcija preglednika u aplikaciji uzorka koje šalje naš serijaliziranog događaj IoT koncentrator:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;
}
```

Kod vrlo je sličan što smo vidjeli u u **iothub\_klijent\_uzorka\_amqp** aplikaciju, u kojem ne možemo stvorila poruka byte polja i zatim koristiti **IoTHubClient\_SendEventAsync** da biste poslali IoT koncentrator. Nakon toga ne možemo samo imate radi oslobađanja ručicu za poruke i serijalizirani podataka međuspremnik smo neke starije verzije dodijeliti.

Drugi na zadnji parametar **IoTHubClient\_SendEventAsync** je referenca povratnog funkcija koja se zove kada podatke uspješno poslana. Evo primjera povratnog funkcija:

```
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    int messageTrackingId = (intptr_t)userContextCallback;

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

Drugi parametar je pokazivač kontekst korisnika; pokazivač za istu ćemo proslijediti **IoTHubClient\_SendEventAsync**. U ovom slučaju kontekstu je jednostavno brojač, ali to može biti bilo što želite.

To je sve postoji slanju događaja. Samo stvar lijevo da prekrije se opisuje kako primati poruke.

### <a name="receiving-messages"></a>Primanje poruka

Primanje poruke radi na sličan način porukama način rada u biblioteci **IoTHubClient** . Najprije registrirali funkciju povratnog poruka:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Nakon toga napišite funkcija povratnog koja se poziva primitku poruke:

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

Kod je predloženog--je isti za svako rješenje. Ova funkcija Prima poruke i vodi brigu o usmjeravanju odgovarajuće funkcije kroz poziv na **IZVRŠAVANJE\_naredba**. Funkcija naziva sada ovisi o definiciju od akcija u našem modelu.

Kad definirate neku akciju u modelu, koristite potrebna za primjenu funkcija koja se zove kada uređaj primi odgovarajuće poruku. Na primjer, ako model definira Ta akcija:

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

Imajte na umu podudara li se naziv funkcije naziv akcije u modelu, a da odgovara parametri funkcije parametre navedene akcije. Prvi parametar uvijek je potrebna i pokazivačem na instancu našem modelu.

Kada uređaj primi poruku koja odgovara taj potpis, zove se odgovarajuću funkciju. Dakle, osim problema da biste uključili predloženog koda iz **IoTHubMessage**, dobivate poruku je samo pitanje definiranje jednostavne funkcije za svaku akciju definirane u modelu.

### <a name="uninitializing-the-library"></a>Uninitializing u biblioteku

Kada završite podataka za slanje i primanje poruka, možete poništiti inicijalizaciju biblioteke IoT:

```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Svaki od ove tri funkcije poravnava s tri funkcije Inicijalizacija prethodno opisano. Pozivanje ove API-ji osigurava da oslobodite prethodno dodijeljene resurse.

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku prekriveno osnove korištenja biblioteke na **uređaju Azure IoT SDK C**. To su vam dali dovoljno informacija da biste shvatili što je sve obuhvaćeno SDK, njegova arhitektura te upute za početak rada s uzorka za Windows. U sljedećem članku nastavlja opis SDK po kojoj se navodi [Dodatne informacije o biblioteci IoTHubClient](iot-hub-device-sdk-c-iothubclient.md).

Dodatne informacije o razvoju za IoT koncentrator potražite u članku [IoT koncentrator SDK-ovi][lnk-sdks].

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]


[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
