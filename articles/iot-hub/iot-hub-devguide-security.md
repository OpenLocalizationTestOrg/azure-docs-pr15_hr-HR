<properties
 pageTitle="Vodič za razvojne inženjere - Upravljanje pristupom koncentrator IoT | Microsoft Azure"
 description="Azure IoT koncentrator za razvojne inženjere vodič – upute za kontrolu pristupa IoT koncentrator i Upravljanje sigurnošću"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="control-access-to-iot-hub"></a>Upravljanje pristupom koncentrator IoT

## <a name="overview"></a>Pregled

U ovom se članku opisuju mogućnosti za osiguravanje koncentratora za IoT. Koncentrator IoT koristi *dozvole* da biste dodijelili pristup svaki krajnje točke koncentrator IoT. Dozvole ograničiti pristup koncentratora za IoT koji se temelji na funkcionalnost.

U ovom se članku opisuje:

- Različite dozvole koje možete dodijeliti uređaja ili pozadinske aplikacije da biste pristupili koncentratora za IoT.
- Postupak provjere autentičnosti i tokeni koristi da biste provjerili dozvole.
- Kako opseg vjerodajnice da biste ograničili pristup određene resursima.
- Koncentrator IoT podrška za X.509 certifikate.
- Posebni uređaj Mehanizmi za provjeru autentičnosti koji koristi postojeće registries identiteta uređaja ili sheme provjere autentičnosti.

### <a name="when-to-use"></a>Kada se koristi

Morate imati odgovarajuće dozvole za pristup bilo koji od krajnje točke IoT koncentratora. Ako, na primjer, uređaj mora sadržavati token koji sadrži sigurnosne vjerodajnice uz svaku poruku šalje IoT koncentrator.

## <a name="access-control-and-permissions"></a>Kontrola pristupa i dozvole

Možete dodijeliti [dozvole](#iot-hub-permissions) na sljedeće načine:

* **Koncentrator razinom zajednički pristup pravila**. Zajednički pristup pravila možete dodijeliti bilo koju kombinaciju [dozvole](#iot-hub-permissions). Možete definirati pravila za [Azure portal][lnk-management-portal], ili programski pomoću [davatelja resursa IoT koncentrator REST API -ji][lnk-resource-provider-apis]. Novostvoreni IoT koncentrator ima sljedeće pravilnike zadane:

    - **iothubowner**: pravila s dozvolama za sve.
    - **servis**: pravila s dozvolom za ServiceConnect.
    - **uređaj**: pravila s dozvolom za DeviceConnect.
    - **registryRead**: pravila s dozvolom za RegistryRead.
    - **registryReadWrite**: pravilnik o dozvolama RegistryRead i RegistryWrite.


* **Po uređaj sigurnosne vjerodajnice**. Svaki IoT koncentrator sadrži [uređaj identiteta registra][lnk-identity-registry]. Za svaki uređaj u taj registar možete konfigurirati sigurnosne vjerodajnice Dodjela dozvola **DeviceConnect** iz djelokruga krajnje točke za odgovarajući uređaj.

Na primjer, u standardne IoT rješenja:

- Komponenta za upravljanje uređaj koristi pravila *registryReadWrite* .
- Procesor komponentu događaj koristi pravila za *servis* .
- Komponentu runtime uređaj poslovne logike koristi pravila za *servis* .
- Uređaji za pojedinačne povezati pomoću vjerodajnice pohranjene u registru središtu IoT identitet.

## <a name="authentication"></a>Provjera autentičnosti

Azure IoT koncentrator dopušta pristup krajnje točke provjerom tokena na temelju pravila zajednički pristup i uređaja identiteta registra sigurnosne vjerodajnice.

Sigurnosne vjerodajnice, kao što su simetričnu tipke nikad ne šalju putem na žičani.

> [AZURE.NOTE] Davatelja resursa Azure IoT koncentrator osigurani putem pretplate Azure kao što su svi davatelji u [Voditelj resursa Azure][lnk-azure-resource-manager].

Dodatne informacije o Sastavljanje i korištenju sigurnosnih tokena potražite u članku [IoT koncentrator sigurnosnih tokena][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Specifičnosti protokola

Svaki podržani protokol, kao što su MQTT, AMQP i HTTP, prijenosi tokena na različite načine.

Prilikom korištenja MQTT paketa za POVEZIVANJE sadrži na deviceId kao ClientId, {iothubhostname} / {deviceId} u polju korisničko ime i SAS token u polje za lozinku. {iothubhostname} mora biti puno CName od središta IoT (na primjer, contoso.azure-devices.net).

Prilikom korištenja [AMQP][lnk-amqp], IoT koncentrator podržava [OBIČNOG SASL] [ lnk-sasl-plain] i [AMQP zahtjevima temelji – sigurnost][lnk-cbs].

Ako koristite AMQP zahtjevima temelji – sigurnost, standardni određuje način za prijenos tokena.

OBIČAN SASL **korisničko ime** mogu biti:

* `{policyName}@sas.root.{iothubName}`Ako koristite tokeni koncentrator razinu.
* `{deviceId}@sas.{iothubname}`Ako koristite uređaj opsegu tokena.

U oba slučaja, polje za lozinku sadrži token, kao što je opisano u [koncentratoru IoT sigurnosnih tokena][lnk-sas-tokens].

HTTP implementira provjere autentičnosti uključivanjem valjani token u zaglavlju zahtjev za **provjeru autentičnosti** .

#### <a name="example"></a>Primjer

Korisničko ime (DeviceId je velika i mala slova):`iothubname.azure-devices.net/DeviceId`

Lozinka (generiranja SAS pomoću programa Explorer uređaj):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] [Azure IoT koncentrator SDK-ovi] [ lnk-sdks] automatsko generiranje tokeni prilikom povezivanja sa servisom. U nekim slučajevima u SDK-ovi ne podržavaju svi protokoli ili sve metoda provjere autentičnosti.

### <a name="special-considerations-for-sasl-plain"></a>Posebne napomene za SASL OBIČAN

Kada pomoću OBIČNOG SASL AMQP, klijent povezati koncentratora za IoT možete koristiti jedan token za svaku vezu s TCP. Kada istekne token TCP veza prekida vezu sa servisom i pokreće ponovnog povezivanja. Takvo ponašanje dok ne problematična za komponentu pozadinske aplikacije, je vrlo damaging uređaj strani aplikacije iz sljedećih razloga:

*  Ime brojnim uređajima obično povezivanje pristupnika. Kada se koristi OBIČAN SASL imaju stvaranje različitih TCP veza za svaki uređaj povezati koncentratora za IoT. Ovaj scenarij znatno povećava potrošnju energije i mrežni resursi i povećava Latencija svaku vezu s uređaja.
* Povećana korištenje resursa povezati nakon isteka svaki tokena negativno utječe resursa ograničeno uređaja.

## <a name="scope-hub-level-credentials"></a>Opseg koncentrator razinom vjerodajnice

Pravila za sigurnost na razini koncentratora možete doseg tako da stvorite tokeni s ograničenom resursa URI. Na primjer, krajnje točke slanja poruka uređaj u oblak s uređaja je **/devices/ {deviceId} / poruke/događaja**. Možete koristiti i pravila za koncentrator razinom zajedničkog pristupa s dozvolama za **DeviceConnect** za potpisivanje tokena čije resourceURI je **/devices/ {deviceId}**. Taj se način stvara tokena koje je moguće koristiti za slanje poruka ime uređaj **deviceId**.

Ovaj mehanizam je slična [događaj koncentratora publisher pravila][lnk-event-hubs-publisher-policy], i omogućuju vam da implementirati metoda prilagođene provjere autentičnosti.

## <a name="security-tokens"></a>Sigurnosnih tokena

Koncentrator IoT koristi sigurnosnih tokena za provjeru autentičnosti uređajima i servisima da bi se izbjeglo slanje tipke na na žičani. Uz to, sigurnosnih tokena ograničen u vrijeme valjanosti i opseg. [Azure IoT koncentrator SDK-ovi] [ lnk-sdks] automatsko generiranje tokeni bez sve Posebna konfiguracija. Neke scenarije, međutim, potreban je korisnik za stvaranje i korištenje sigurnosnih tokena izravno. Uključuju izravno korištenje površine MQTT, AMQP ili HTTP ili implementacije uzorak tokena, kao što je opisano u [provjere autentičnosti uređaj Prilagođeno][lnk-custom-auth].

Koncentrator IoT omogućuje uređaji za provjeru s IoT koncentrator pomoću [X.509 certifikate][lnk-x509]. 

### <a name="security-token-structure"></a>Sigurnosni token strukture
Da biste dodijelili vrijeme bounded pristup uređaji i servisi za određene funkcije u središte IoT koristite sigurnosnih tokena. Da biste bili sigurni da možete povezati samo ovlašteni uređajima i servisima, sigurnosnih tokena moraju biti prijavljeni pomoću ključa zajednički pristup pravila ili simetričnu ključ sprema se s uređaja identiteta u registru identitet.

Token prijavljeni pomoću ključa daje pristup pravila zajednički pristup sve funkcionalnosti povezane s dozvolama za zajednički pristup pravila. S druge strane, token prijavljeni pomoću uređaja identiteta simetričnu ključa samo daje **DeviceConnect** dozvolu za identitet pridružene uređaja.

Sigurnosni token sastoji se od sljedećih oblika:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

Ovo su očekivani vrijednosti:

| Vrijednost | Opis |
| ----- | ----------- |
| {potpis} | Niz za potpis HMAC SHA256 obrasca: `{URL-encoded-resourceURI} + "\n" + expiry`. **Važno**: ključ je dekodirati iz base64 i koristiti kao ključ za izvođenje izračunavanje HMAC SHA256. |
| {resourceURI} | URI prefiks (po segmenta) krajnje točke kojemu je moguće pristupiti s token, počevši od hostname središtu IoT (bez protocol). Na primjer,`myHub.azure-devices.net/devices/device1` |
| {isteka} | Nizovi UTF8 za broj sekundi od UTC 00:00:00 epoch na 1 1970 siječanj. |
| {URL-kodirani-resourceURI} | Manji slučaja URL kodiranje resursa malim slovima URI-JA |
| {policyName} | Naziv pravila zajednički pristup na koju se odnosi ovaj token. Nema slučaju tokeni koje upućuju na uređaju registra vjerodajnice. |

**Napomena o prefiks**: U URI prefiks je izračunati segmenta i ne znakom. Na primjer `/a/b` je prefiks `/a/b/c` , ali ne i za `/a/bc`.

Sljedeći isječak Node.js prikazuje funkciju naziva **generateSasToken** koji izračunava tokena iz ulaza `resourceUri, signingKey, policyName, expiresInMins`. U sljedećim odjeljcima detaljno kako pokrenuti različite unosa slučajeva za različite tokena koristi.

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

Za usporedbu, ekvivalentan Python kod da biste generirali sigurnosni token je:

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [AZURE.NOTE] Budući da je Vremenska valjanost tokena potvrđuju na računalima IoT koncentrator, važno je da je drift na satu računala koje generira token minimalno.

### <a name="use-sas-tokens-in-a-device-client"></a>Korištenje tokena SAS u klijent za uređaj

Dva su načina dobivanja dozvole **DeviceConnect** s IoT koncentrator s sigurnosnih tokena: pomoću [ključ simetričnu uređaja iz registra identiteta uređaja](#use-a-symmetric-key-in-the-identity-registry)ili upotrijebite [ključ pravila zajednički pristup](#use-a-shared-access-policy).

Imajte na umu da sve funkcije dostupne s uređaja predstavljeni dizajnom na krajnje točke s prefiksom `/devices/{deviceId}`.

> [AZURE.IMPORTANT] Jedini način IoT koncentrator potvrđuje je određeni uređaj koristi tipku simetričnu uređaj identitet. U slučajevima kada se koristi zajednički pristup pravila za pristup funkcionalnosti uređaj rješenje morate uzeti u obzir komponentu izdavanja sigurnosnog tokena kao pouzdano potkomponenta.

Krajnje točke uređaj dostupnog su (bez obzira protocol):

| Krajnja točka | Funkcija |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | Slanje poruka uređaj u oblak. |
| `{iot hub host name}/devices/{deviceId}/devicebound` | Primanje poruka oblak na uređaj. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Korištenje simetričnu ključa registra za identitet

Prilikom korištenja uređaju identitetu simetričnu ključ za stvaranje tokena u policyName (`skn`) element token izostavi.

Na primjer, token stvorene za pristup svim uređaj funkcionalnosti imat sljedećih parametara:

* resurs URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* Potpisivanje ključ: sve simetričnu ključ za na `{device id}` identitet
* Nema naziv pravila
* bilo koje vrijeme isteka.

Primjer funkcije čvor iznad bio sljedeći:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Rezultat dopušta pristup svim funkcionalnost za device1 bio sljedeći:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] Da biste generirali sigurne token pomoću alata za .NET [Explorer uređaja]moguće je[lnk-device-explorer].

### <a name="use-a-shared-access-policy"></a>Pomoću pravila za zajednički pristup

Prilikom stvaranja token iz zajedničkog pristupa pravila, pravila naziv polja `skn` mora biti postavljeno na naziv pravila koriste. Preporučuje se i obavezno pravila daje je **DeviceConnect** dozvola.

Dva glavna za korištenje zajednički pristup pravila za pristup uređaj funkcionalnosti scenarija:

* [protokol pristupnika u oblaku][lnk-endpoints],
* [token services] [ lnk-custom-auth] za implementaciju sheme prilagođene provjere autentičnosti.

Budući da se pravilo zajednički pristup potencijalno može dati pristup da biste se povezali kao bilo kojeg uređaja, važno je da biste koristili resursa URI prilikom stvaranja sigurnosnih tokena. To je osobito važno za tokena servise koji opsega token određene uređaja koji koristi resursa URI. Od ovoga vrijedi manje za protokol pristupnika kao što su se već mediating promet za sve uređaje.

Na primjer, servis tokena putem pravila unaprijed stvoreni zajednički pristup naziva **uređaj** želite stvoriti token s sljedećih parametara:

* resurs URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* Potpisivanje ključ: jedan od ključeva od na `device` pravila
* Naziv pravila: `device`,
* bilo koje vrijeme isteka.

Primjer funkcije čvor iznad bio sljedeći:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Rezultat dopušta pristup svim funkcionalnost za device1 bio sljedeći:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Pristupnik za protokol može koristiti istu token za sve uređaje jednostavno postavljanje resursa URI za `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Korištenje sigurnosnih tokena iz usluge komponenti

Komponente servis samo možete generirati sigurnosnih tokena pomoću zajednički pristup pravilnicima za dodjelu odgovarajuće dozvole, kao što je prethodno opisano.

Ovo su funkcije servise koji se prikazuje na krajnjih točaka:

| Krajnja točka | Funkcija |
| ----- | ----------- |
| `{iot hub host name}/devices` | Stvaranje, ažuriranje, dohvatiti i brisanje identiteta uređaja. |
| `{iot hub host name}/messages/events` | Primanje poruka uređaj u oblak. |
| `{iot hub host name}/servicebound/feedback` | Prikupljanje povratnih informacija za poruke u oblak na uređaj. |
| `{iot hub host name}/devicebound` | Slanje poruka oblak na uređaj. |

Na primjer, na servis za generiranje pomoću unaprijed stvoreni zajednički pristup pravilo koje se zove **registryRead** želite stvoriti token s sljedećih parametara:

* resurs URI: `{IoT hub name}.azure-devices.net/devices`,
* Potpisivanje ključ: jedan od ključeva od na `registryRead` pravila
* Naziv pravila: `registryRead`,
* bilo koje vrijeme isteka.

    VAR krajnje = "myhub.azure-devices.net/devices";   VAR policyName = 'uređaj';   VAR policyKey = "...";

    VAR token = generateSasToken (krajnje točke, policyKey, policyName, 60)

Rezultat želite dopustiti pristup čitanju sve identitete uređaj, bio sljedeći:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>Podržani X.509 certifikate

Možete koristiti bilo koji certifikat x.509 za provjeru uređaj s IoT koncentratora. To obuhvaća:

-   **Postojeći certifikat X.509**. Na uređaju možda već imate programa certifikat x.509 pridružen. Pomoću ovog certifikata uređaja možete uspješnoj IoT koncentratora.

-   **Samostalno generirani i samopotpisani certifikat X-509**. Proizvođača uređaja ili sesije deployer možete generirati ove potvrde i pohraniti odgovarajući privatni ključ (i certifikat) na uređaju. Možete koristiti alate kao što su [OpenSSL] [ lnk-openssl] i [Windows SelfSignedCertificate] [ lnk-selfsigned] utility za tu svrhu.

-   **Certifikat X.509 CA potpisan**. Prepoznavanje uređaja i provjeru autentičnosti uređaj s IoT koncentratora možete koristiti i certifikat X.509 generira i prijavljeni od ustanove za izdavanje certifikata (CA).

Na uređaju može koristiti X.509 certifikat ili sigurnosni token za provjeru autentičnosti, ali ne oba.

### <a name="register-an-x509-client-certificate-for-a-device"></a>Registrirajte se X.509 klijent certifikat za uređaj

[Azure IoT servisa SDK C#] [ lnk-service-sdk] (verzija 1.0.8+) podržava Registracija uređaja koji koristi za klijentski certifikat X.509 za provjeru autentičnosti. Ostale API-ji kao što su uvoz/izvoz uređaja podržava i X.509 certifikate klijenta.

### <a name="c-support"></a>C\# podršku

Klase **RegistryManager** omogućuje programski da biste registrirali uređaj. Posebno metode **AddDeviceAsync** i **UpdateDeviceAsync** korisniku omogući da biste registrirali i ažuriranje uređaja u registru identiteta koncentrator Iot uređaja. Ta dva načina kao unos uzimaju instancu **uređaja** . Klasa **uređaja** uključuje **provjere autentičnosti** svojstva u programu koji omogućuje korisniku da odredi primarnih i sekundarnih thumbprints certifikat X.509. Na otisak prsta predstavlja SHA-1 raspršivanje certifikata X.509 (spremljen korištenjem binarni kodiranja DER). Korisnici imati mogućnost navođenje primarni otisak prsta ili sekundarni otisak prsta ili i jedno i drugo. Primarnih i sekundarnih thumbprints podržani su da bi se rukovati scenariji dinamične certifikata.

> [AZURE.NOTE] Koncentrator IoT potrebna ili spremiti na cijeli klijent certifikat x.509, samo otisak prsta.

Slijedi primjer C\# isječak koda za registraciju uređaja koji se koristi za klijentski certifikat X.509:

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-client-certificate-during-runtime-operations"></a>Korištenje programa klijentski certifikat X.509 tijekom izvođenja operacije

[Azure IoT uređaj SDK za .NET] [ lnk-client-sdk] (verzija 1.0.11+) podržava korištenje X.509 certifikate klijenta.

### <a name="c-support"></a>C\# podršku

Klase **DeviceAuthenticationWithX509Certificate** podržava stvaranje instanci  **DeviceClient** pomoću programa klijentski certifikat X.509.

Evo koda za uzorak:

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Provjera autentičnosti posebni uređaj

Možete koristiti IoT koncentrator [uređaj identiteta registra] [ lnk-identity-registry] za konfiguriranje po uređaj sigurnosne vjerodajnice i pristup kontrolu korištenjem [tokeni][lnk-sas-tokens]. Međutim, ako rješenje za IoT već ima značajan ulaganja u posebni uređaj identiteta registra i/ili provjere autentičnosti shemu, infrastruktura za postojeće možete integrirati s IoT koncentrator stvaranjem *tokena*. Na taj način možete koristiti druge značajke IoT u rješenje.

Servis za tokena je servis prilagođene oblaka. Koristi IoT koncentrator *pravila zajednički pristup* s dozvolama za **DeviceConnect** za stvaranje tokena *iz djelokruga uređaja* . Tokena omogućivanja uređaja za povezivanje s koncentratora za IoT.

  ![Koraci uzorka tokena][img-tokenservice]

Ovo su glavni koraci uzorka tokena:

1. Stvaranje pravila pristup IoT koncentrator koji se zajednički koriste s dozvolama za **DeviceConnect** za koncentratora za IoT. Možete stvarati ovo pravilo na [portal za Azure] [ lnk-management-portal] ili programski. Servis tokena koristi ovo pravilo za potpisivanje tokena stvara.
2. Kada na uređaju morate pristupiti koncentratora za IoT, zahtjeve traje tokena iz tokena servisa. Posebni uređaj identiteta registra/provjere autentičnosti shemu da biste odredili identiteta uređaja koji koristi servis tokena za stvaranje tokena možete uspješnoj uređaj.
3. Servis tokena vraća token. Token stvoren je pomoću `/devices/{deviceId}` kao `resourceURI`, s `deviceId` kao uređaj nije moguće provjeriti autentičnost. Servis tokena koristi zajednički pristup pravila za sastavljanje token.
4. Uređaj koristi token izravno s središtu IoT.

> [AZURE.NOTE] Možete koristiti klase .NET [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] ili klase jezika Java [IotHubServiceSasToken] [ lnk-java-sas] da biste stvorili token na servisu tokena.

Servis tokena možete postaviti istek tokena po želji. Kada istekne token središtu IoT severs vezu uređaja. Nakon toga uređaj morate zatražiti novi token tokena servisa. Ako koristite kratki isteka vremena, to povećava opterećenje uređaja i servis tokena.

Za uređaj za povezivanje s vaš koncentrator, morate i dalje je dodati registar identiteta koncentrator IoT uređaj, čak i ako je uređaj koristi token i ne ključ uređaj za povezivanje. Zbog toga možete nastaviti koristiti kontrolu pristupa po uređaj tako da omogućivati ili onemogućivati uređaj identiteta u [koncentrator IoT identiteta registra] [ lnk-identity-registry] kada potvrđuje uređaj s token. To mitigates rizici tokeni pomoću dugo isteka vremena.

### <a name="comparison-with-a-custom-gateway"></a>Usporedba s prilagođene pristupnika

Uzorak tokena je preporučeni način za primjenu prilagođenog identiteta shemu registra/provjere autentičnosti s IoT koncentrator. Preporučuje se jer koncentrator IoT će se i dalje obaviti većinu promet rješenja. No postoje slučajevi u kojima se sheme prilagođene provjere autentičnosti pa intertwined s protokolom servisa obrade promet (*Prilagođeni pristupnika*) nužan. Primjer to je [Sigurnost prijenosa sloja (TLS) i zajednički tipke (PSKs)][lnk-tls-psk]. Dodatne informacije potražite u odjeljku [protokol pristupnik] [ lnk-protocols] temu.

## <a name="reference-topics"></a>Pregled tema:

U sljedećim temama reference sadrže dodatne informacije o pristupom koncentratora za IoT.

## <a name="iot-hub-permissions"></a>Dozvole za IoT koncentratora

U sljedećoj su tablici navedeni dozvola možete koristiti za kontrolu pristupa koncentratora za IoT.

| Dozvole            | Bilješke |
| --------------------- | ----- |
| **RegistryRead**      | Daje pročitajte pristup identiteta registra uređaja. Dodatne informacije potražite u članku [uređaj identiteta registra][lnk-identity-registry]. |
| **RegistryReadWrite** | Daje za čitanje i pisanje pristup identiteta registar uređaja. Dodatne informacije potražite u članku [uređaj identiteta registra][lnk-identity-registry]. |
| **ServiceConnect**    | Daje pristup oblaku servisa komunikacije i web-praćenje krajnje točke. Na primjer, daje dozvolu za servise u oblaku pozadinsku poruka uređaj u oblak, slanje poruka oblaka uređaja i dohvaćanje odgovarajućih potvrda isporuke. |
| **DeviceConnect**     | Pristup daje krajnje točke uređaj dostupnog komunikacije. Ako, na primjer, daje dozvolu za slanje poruka uređaj u oblak i primanje poruka oblak na uređaj. Tu dozvolu koristi uređaja. |

## <a name="additional-reference-material"></a>Dodatne reference materijala

Druge teme referencu u vodiču za razvojne inženjere obuhvaćaju sljedeće:

- [Krajnje točke koncentrator IoT] [ lnk-endpoints] opisuju razne izlaže svaki IoT koncentrator za upravljanje i izvođenje operacija krajnje točke.
- [Ograničavanje i kvotama] [ lnk-quotas] u članku se opisuje kvote koji se odnose na servis IoT koncentratora i regulacije ponašanje očekivati prilikom korištenja servisa.
- [Koncentrator IoT uređaja i servisa SDK-ovi] [ lnk-sdks] popis različitih jezika SDK-ovi koje koristi u prilikom razvoja uređaja i servisa aplikacije kompatibilne s IoT koncentratora.
- [Upit jezik za twins, postupcima i zadacima] [ lnk-query] u članku se opisuje jezik upita možete koristiti za dohvaćanje informacija iz centra za IoT o uređaju twins, metode i zadacima.
- [Podrška IoT koncentrator MQTT] [ lnk-devguide-mqtt] navedene su dodatne informacije o podršci za IoT koncentrator za protokol MQTT.

## <a name="next-steps"></a>Daljnji koraci

Sada ste naučili kako upravljati pristupom IoT koncentrator, možda je u sljedećim temama vodič za razvojne inženjere:

- [Korištenje twins uređaj da biste sinkronizirali stanje i konfiguracija][lnk-devguide-device-twins]
- [Pozivanje metode izravno na uređaju][lnk-devguide-directmethods]
- [Zakazivanje zadataka na više uređaja][lnk-devguide-jobs]

Ako želite isprobati neke koncepta opisane u ovom članku, možda ćete zanimati i sljedeći vodiči koncentrator IoT:

- [Početak rada s Azure IoT koncentratora][lnk-getstarted-tutorial]
- [Upute za slanje poruka oblak na uređaj s IoT koncentratora][lnk-c2d-tutorial]
- [Način obrade koncentrator IoT uređaja u oblak poruke][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
