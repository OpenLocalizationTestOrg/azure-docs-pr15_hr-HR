# <a name="securing-your-iot-deployment"></a>Zaštita IoT implementacije

Ovaj članak pruža sljedeće razine detalja za optimalnu Infrastruktura Azure IoT temelji Internet od stvari (IoT). Veze do implementaciju razine detalja za konfiguriranje i implementaciji svaku komponentu. Također pruža usporedbi i odabira između različite metode konkurirajućih.

Zaštita Azure IoT implementacije može se podijeliti u tri sigurnosna područja:

- **Sigurnost uređaja**: zaštita dok je uveden koji uređaj IoT.
- **Sigurnost veze**: osiguravanje svi podaci koji se prenose između uređaja IoT i IoT koncentrator je Povjerljivo i mijenjati dokaz.
- **Oblak sigurnost**: pružanje sredstava za sigurne podataka dok premješta kroz i spremljene u oblaka.

![Tri sigurnosna područja][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Dodjeljivanje siguran uređaj i provjera autentičnosti

IoT programski Azure secures IoT uređaji po sljedeće dvije metode:

- Pružanjem identitet jedinstveni ključ (sigurnosnih tokena) za svaki uređaj koji možete uređaj koristi za komunikaciju s IoT koncentrator.

- Pomoću programa na-uređaja [X.509 certifikat] [ lnk-x509] i privatni ključ kao sredstvo za provjeru autentičnosti uređaj IoT koncentrator. Ova metoda provjere autentičnosti osigurava da privatni ključ na uređaju nije poznat izvan uređaj u bilo kojem trenutku davanja višu razinu sigurnosti.

Pruža sigurnosnih tokena metodu provjere autentičnosti za svaki poziv izvršio uređaj IoT koncentrator povezivanjem u simetričnog ključa za svaki poziv. Provjera autentičnosti utemeljena na X.509 omogućuje provjeru autentičnosti IoT uređaj na fizičke sloja kao dio TLS uspostavljanje veze. Sigurnosni token utemeljen na metodu koristi se bez provjere autentičnosti X.509 koji je manje sigurna uzorak. Izbor između dvije metode prvenstveno diktiranja tako kako siguran uređaj provjere autentičnosti mora biti i dostupnost sigurne pohrane na uređaju (za sigurno spremanje privatni ključ).

## <a name="iot-hub-security-tokens"></a>Koncentrator IoT sigurnosnih tokena

Koncentrator IoT koristi sigurnosnih tokena za provjeru autentičnosti uređaji i servisi izbjeći slanje tipke na mreži. Uz to, sigurnosnih tokena su ograničena u Vremenska valjanost i opseg. Azure IoT koncentrator SDK-ovi automatski generirati tokeni ne zahtijeva posebnu konfiguraciju. Neke scenarije, međutim, zahtijevaju korisnika za generiranje i izravno korištenje sigurnosnih tokena. One uključuju direktni korištenje površine MQTT, AMQP ili HTTP ili implementaciju uzorak servis tokena.

Više pojedinosti o strukturi sigurnosni token i njegova korištenja možete pronaći u sljedećim člancima:

-   [Struktura sigurnosnog tokena][lnk-security-tokens]
-   [Pomoću tokena SAS kao uređaj][lnk-sas-tokens]

Svaki IoT koncentrator ima [Uređaj identitet registra] [ lnk-identity-registry] koja se može koristiti za stvaranje resurse uređaj u uslugu, kao što je red čekanja koji sadrži in-flight poruke oblak uređaj i dopustiti pristup uređaj usmjerenu krajnje točke. Identitet registra IoT koncentrator pruža sigurnu pohranu identitete uređaj i sigurnosnih ključeva za rješenje. Pojedinac ili grupe uređaj identiteti mogu se dodati Dopusti popis ili popis blokova omogućavanja potpunu kontrolu pristupa uređaj. Sljedeći članci dati više pojedinosti o strukturi identitet registra uređaja i podržava operacije.

[Koncentrator IoT podržava protokole MQTT, AMQP i HTTP][lnk-protocols]. Svaki od ovih protokole drugačije koristite sigurnosnih tokena IoT uređaj iz koncentratora IoT:

- AMQP: Sigurnost OBIČNOG i zahtjeva AMQP SASL ({policyName}@sas.root.{iothubName} u slučaju tokene koncentrator razinu; {deviceId} slučaju uređaja u dosegu tokene).

- MQTT: POVEZIVANJE koristi paketa {deviceId} kao {ClientId}, {IoThubhostname} / {deviceId} u polju **korisničko ime** i SAS token u polju **lozinku** .

- HTTP: Valjani token je zaglavlje autorizaciju zahtjeva.

IoT koncentrator uređaj identitet registra mogu se konfigurirati-uređaj sigurnosne vjerodajnice i pristupiti kontrolu. Međutim, ako IoT rješenje već ima značajan ulaganja u [posebni uređaj identitet registra i/ili provjeru autentičnosti shemu][lnk-custom-auth], ga biti integrirani u postojeću infrastrukturu s IoT koncentrator stvaranjem servis tokena.

### <a name="x509-certificate-based-device-authentication"></a>X.509 certifikat temelji uređaj provjere autentičnosti

Korištenje [uređaja temelji X.509 certifikat] [ lnk-use-x509] i njen pridruženi privatni i javni ključ par omogućuje dodatne provjere autentičnosti na fizičke sloja. Privatni ključ je sigurno pohranjena u uređaj i nije vidljiv izvan uređaj. X.509 certifikat sadrži informacije o uređaju, kao što je ID uređaja i druge organizacije pojedinosti. Potpis certifikata je generiran pomoću privatnog ključa.

Protok visoke razine uređaj dodjele resursa:

- Pridruživanje identifikator fizički uređaj – uređaj identitet i/ili certifikat X.509 povezan uređaj tijekom proizvodnje ili commissioning uređaj.

- Stvorite odgovarajuće stavke identitet u koncentrator IoT – uređaj identitet i pridruženi uređaj informacije u registru uređaj IoT koncentrator.

- Otisak prsta za certifikat X.509 sigurnog spremanja u registru koncentrator IoT uređaja.

### <a name="root-certificate-on-device"></a>Korijenski certifikat na uređaju

Prilikom uspostavljanja sigurne veze TLS s koncentratorom IoT uređaj IoT potvrđuje IoT koncentrator pomoću korijenski certifikat koji je dio uređaj SDK. Certifikat za klijent C SDK se nalazi u mapi "\\c\\certifikati o" ispod korijena u repo. Iako ove korijenski certifikati su long-lived, oni i dalje mogu isteći ili biti opozvan. Ako ne postoji način ažuriranja certifikat uređaja, uređaj možda moći naknadno povezivanje s koncentrator IoT (ili oblak servisa). Pritom znači ažuriranje korijenskog certifikata kada uređaj IoT je uveden učinkovito umanjiti ovaj rizik.

## <a name="securing-the-connection"></a>Zaštita veza

Internetska veza između uređaja za IoT i IoT koncentrator je osigurana korištenjem standardne sigurnosne prijevoza sloja (TLS). Azure IoT podržava [TLS 1.2][lnk-tls12], TLS 1.1 i TLS 1.0 u ovom redoslijedu. Podrška za TLS 1.0 je dana za kompatibilnost unatrag samo. Preporučuje se korištenje TLS 1.2 jer pruža Većina sigurnosti.

Programski Azure IoT podržava sljedeće paketi snaga ovim redoslijedom.

| Paket šifri | Duljina |
|--------------|--------|
| TLS\_ECDHE\_RSA\_s\_AES\_256\_CBC\_SHA384 secp384r1 ECDH (0xc028) (eq. FČ 7680 bitova RSA) | 256    |
| TLS\_ECDHE\_RSA\_s\_AES\_128\_CBC\_SHA256 secp256r1 ECDH (0xc027) (eq. FČ 3072 bitova RSA) | 128    |
| TLS\_ECDHE\_RSA\_s\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq. FČ 7680 bitova RSA)           | 256    |
| TLS\_ECDHE\_RSA\_s\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq. FČ 3072 bitova RSA)           | 128    |
| TLS\_RSA\_s\_AES\_256\_GCM\_SHA384 (0x9d) | 256    |
| TLS\_RSA\_s\_AES\_128\_GCM\_SHA256 (0x9c) | 128    |
| TLS\_RSA\_s\_AES\_256\_CBC\_SHA256 (0x3d) | 256    |
| TLS\_RSA\_s\_AES\_128\_CBC\_SHA256 (0x3c) | 128    |
| TLS\_RSA\_s\_AES\_256\_CBC\_SHA (0x35)    | 256    |
| TLS\_RSA\_s\_AES\_128\_CBC\_SHA (0x2f)    | 128    |
| TLS\_RSA\_s\_3DES\_EDE\_CBC\_SHA (0xa)    | 112    |

## <a name="securing-the-cloud"></a>Zaštita oblaka

Koncentrator Azure IoT omogućuje definicija [pravila kontrolu pristupa] [ lnk-protocols] za svaki sigurnosni ključ. Koristi sljedeći skup dozvola odobriti pristup svakom IoT koncentrator krajnje točke. Pristup je IoT koncentrator funkcionalnost na temelju ograničiti dozvole.

- **RegistryRead**. Daje pristup za čitanje identitet registra uređaja. Dodatne informacije potražite [Uređaj identitet registra][lnk-identity-registry].

- **RegistryReadWrite**. Daje za čitanje i pisanje access identitet registra uređaja. Dodatne informacije potražite [Uređaj identitet registra][lnk-identity-registry].

- **ServiceConnect**. Daje pristup oblak servis usmjerenu komunikacije i nadzor nad krajnje točke. Na primjer, daje dozvolu na pozadinskim oblak usluge primati poruke uređaj oblaka, oblaka uređaj poruke poslati i dohvatiti odgovarajuće potvrda isporuke.

- **DeviceConnect**. Daje pristup uređaj usmjerenu komunikacije krajnje točke. Na primjer, daje dozvolu uređaj oblak poruke slati i primati poruke oblak uređaj. Uređaji koriste ovu dozvolu.

Postoje dva načina dobivanja dozvole **DeviceConnect** s IoT koncentrator s [sigurnosnih tokena][lnk-sas-tokens]: pomoću ključ identitet uređaj ili ključ za pravila zajednički pristup. Štoviše, važno je da zapamtite sve funkcionalnost može pristupiti s uređajima izložena po dizajn na krajnje točke s prefiksom `/devices/{deviceId}`.

[Servis komponente samo generirati sigurnosnih tokena] [ lnk-service-tokens] pomoću zajednički pristup pravila odobravanje odgovarajuće dozvole.

Koncentrator Azure IoT i druge servise koji može biti dio rješenja Dopusti upravljanje korisnicima pomoću Azure Active Directory.

Ingested Azure IoT koncentrator podataka možete potrošenih različite usluge poput Analytics Azure strujanje i Azure blobova. Ti servisi omogućuju upravljanje pristup. Pročitajte više o ove usluge i ispod dostupne mogućnosti:

- [Azure DocumentDB][lnk-docdb]: usluga skalabilni, potpuno indeksirati baze podataka za polu-strukturirane podatke koji upravlja metapodataka za uređaje koje Dodjela, atribute, konfiguracije i sigurnosna svojstva. DocumentDB nudi visoke performanse i visokom propusnošću obrade, sheme agnostic indeksiranje podataka i obogaćenog sučelje SQL upita.

- [Azure Analytics strujanje][lnk-asa]: u stvarnom vremenu strujanje obrada oblak omogućuje hitro razviti i uvesti rešenje niska trošak analytics otkriti uvida u stvarnom vremenu s uređaja, senzori, infrastrukturu i aplikacije. Podatke iz potpuno Upravljani servis možete skalirati na bilo koje jedinice dok i dalje postizanje visoke propusnost, niske latencije i resiliency.

- [Azure App Services][lnk-appservices]: oblak platforma za izgradnju snažne web i mobilne apps povezivanje podataka bilo gdje; u oblak ili lokalno. Sastavi zanimljivih apps mobile za iOS, Android i Windows. Integrirati s softver kao usluge (SaaS) i enterprise aplikacije s povezivanjem out-of-the-okvir deseci oblak temelji services i enterprise aplikacije. Kod u omiljene jezik i IDE (.NET, NodeJS, i, Python ili Java) za izgradnju web apps API-ji brže nego ikad.

- [Logika Apps][lnk-logicapps]: U logiku Apps značajka usluge App Azure pomaže integrirati IoT rješenje za postojeće redak poslovne sustave i automatizovali procese tijeka rada. Logika Apps omogućuje programerima tijekova rada dizajna koji počinju s okidač i izvršavanje niz koraka — pravila i akcije koje koristite snažne poveznici za integraciju poslovnih procesa. Logika Apps nudi povezivost out-of-the-okvir za Golema ekosustav SaaS oblak temelji, i lokalno aplikacije.

- [Azure blobova][lnk-blob]: pouzdan, najekonomičniji oblak spremišta za podatke koji uređajima poslati oblaka.

## <a name="conclusion"></a>Zaključak

Ovaj članak pruža pregled implementaciju razine detalja za Osmišljavanje i implementacija potrebnu infrastrukturu IoT pomoću Azure IoT. Konfiguriranje svaku komponentu biti sigurno je ključ u Zaštita cjelokupnog Infrastruktura IoT. Dostupni u Azure IoT dizajna izbori pružiti neke razinu fleksibilnost i izbora; Međutim, svaki izbor može imati sigurnosne posljedice. Preporučuje se da svaki od tih odabira procijeniti kroz procjenu rizik trošak.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-as-a-device
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#using-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
