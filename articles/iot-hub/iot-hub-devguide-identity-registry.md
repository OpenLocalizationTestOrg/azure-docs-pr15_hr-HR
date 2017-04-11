<properties
 pageTitle="Vodič za razvojne inženjere - registra identiteta uređaja | Microsoft Azure"
 description="Azure IoT koncentrator za razvojne inženjere vodič – opis registra identiteta uređaj i kako je koristiti za upravljanje uređajima"
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

# <a name="manage-device-identities-in-iot-hub"></a>Upravljanja uređajima u središte IoT

## <a name="overview"></a>Pregled

Svaki IoT koncentrator ima registra za uređaj identitet koji pohranjuje informacije o uređajima koji dopušteni povezati središtu. Prije nego što koncentratora možete povezati uređaj, mora biti unos za taj uređaj u odjeljku središte uređaj identiteta registra. Na uređaju morate i provjeru s središtu na temelju vjerodajnice pohranjene u registru identiteta uređaja.

Visoke razine identiteta registra uređaj je zbirka web zaslonom OSTALE uređaj identiteta resursa. Prilikom dodavanja stavka registra IoT koncentrator stvara skup resursa po uređaj u servis kao što su čekanja koji sadrži in-flight poruke oblak na uređaj.

### <a name="when-to-use"></a>Kada se koristi

Pomoću identiteta registra uređaja kada se morate Dodjela resursa za uređaje koji se povezati s IoT koncentratora i kada morate omogućiti kontrolu pristupa po uređaj krajnje točke uređaj dostupnog u vašem središte.

> [AZURE.NOTE] Identitet registra uređaj ne sadrži sve metapodatke specifičnim aplikacijama.

## <a name="device-identity-registry-operations"></a>Uređaj identiteta registra operacije

Registar identiteta uređaj IoT koncentrator izlaže sljedeće postupke:

* Stvaranje uređaja identiteta
* Ažuriranje identiteta uređaja
* Dohvaćanje identiteta uređaj tako da ID-a
* Brisanje identiteta uređaja
* Popis do 1000 identiteta
* Izvoz sve identiteta u spremište blobova platforme Azure
* Identiteti uvoz iz spremišta blobova platforme Azure

Optimistična Procjena istodobnosti te operacije može se koristiti kao navedeno u [RFC7232][lnk-rfc7232].

> [AZURE.IMPORTANT] Jedini način za dohvaćanje sve identiteta u registru identiteta na koncentrator jest da biste koristili [Izvoz] [ lnk-export] funkcija.

Do IoT koncentrator identiteta registra za uređaj:

- Sadrže bilo koju aplikaciju metapodataka.
- Možete pristupiti kao što su rječnik, pomoću **deviceId** kao ključ.
- Ne podržava izražajna upita.

Rješenje za IoT obično ima zaseban spremište specifične za rješenja koja sadrži metapodatke specifičnim aplikacijama. Na primjer, spremište specifične za rješenja u rješenje pametno sastavnih želite zapisati sobu u kojoj je implementiran senzora temperatura.

> [AZURE.IMPORTANT] Samo pomoću identiteta registra uređaj za upravljanje uređajima i dodjeljivanje operacije. Operacije visoke propusnost vrijeme izvođenja treba ovisi o izvođenje operacije u registru identiteta uređaja. Ako, na primjer, provjera stanja povezanosti uređaj prije no što pošaljete naredbe nije podržani uzorak. Provjerite je li da biste provjerili [Ograničavanje stope] [ lnk-quotas] za identitet registra uređaja i [intervala uređaja sustava] [ lnk-guidance-heartbeat] uzorka.

## <a name="disable-devices"></a>Onemogućivanje uređaja

Uređaje možete onemogućiti ažuriranjem svojstvo **Stanje** identitet u registru. Obično ćete koristiti ovo svojstvo u dvije situacije:

- Prilikom dodjele resursa djelovanje postupak. Dodatne informacije potražite u članku [Dodjeljivanje uređaj][lnk-guidance-provisioning].
- Ako iz bilo kojeg razloga preporučujemo uređaj ugrožena ili postala neovlašteno.

## <a name="import-and-export-device-identities"></a>Uvoz i izvoz identiteta uređaja

Uređaj identiteta masovno možete izvesti iz registra identiteta IoT koncentrator, pomoću asinkronog operacija na [središte IoT resursa davatelja krajnje][lnk-endpoints]. Izvozi se dugoročnih poslove koje koriste kupca navedeni blob spremnik za spremanje podataka za identitet uređaj čitanje iz registra identitet.

Možete uvesti uređaj identiteta masovno registar identiteta IoT koncentrator, pomoću asinkronog operacije na [središte IoT resursa davatelja krajnje][lnk-endpoints]. Uvozi su dugoročnih poslove koje koriste podatke u spremniku kupca navedeni blob pisati uređaj identiteta podatke u registar identiteta uređaja.

- Detaljne informacije o uvoza i izvoza API-ji potražite u članku [davatelja resursa IoT koncentrator REST API -ji][lnk-resource-provider-apis].
- Dodatne informacije o pokretanju uvoz i izvoz zadacima, potražite u članku [Upravljanje skupno koncentrator IoT uređaj identiteta][lnk-bulk-identity].

## <a name="device-provisioning"></a>Dodjela resursa za uređaj

Uređaj podatke koji pohranjuje navedeni IoT rješenja ovisi o određenim potrebama tog rješenja. No minimalno rješenje morate spremiti identiteta uređaja i provjera autentičnosti tipke. Azure IoT koncentrator obuhvaća do registra identitet koji omogućuje pohranu vrijednosti za svaki uređaj kao što je ID-ove, ključeve provjere autentičnosti i šifre statusa. Rješenje možete koristiti druge Azure servise kao što je spremište tablica platforme Azure, blobova platforme Azure ili Azure DocumentDB za spremanje uređaj dodatne podatke.

*Dodjeljivanje uređaj* je postupak dodavanja podataka početne uređaj služi za pohranu u rješenje. Da biste omogućili novi uređaj za povezivanje s vaš koncentrator, morate dodati novi ID uređaja i ključevi registra identiteta IoT koncentratora. U sklopu postupka dodjele resursa, možda morati pokrenuti specifične za uređaj podataka u druge trgovine rješenja.

## <a name="device-heartbeat"></a>Veza na uređaju

Identitet registra IoT koncentrator sadrži polje pod nazivom **connectionState**. Polje **connectionState** trebali biste koristiti samo tijekom razvoja i ispravljanje pogrešaka IoT rješenja treba upit za polje u vrijeme izvođenja (na primjer, da biste provjerili je li uređaj povezan da bi se odlučite želite li da biste poslali poruku oblak na uređaj ili programa SMS).

Ako rješenje IoT treba znati ako je uređaj priključen (vrijeme izvođenja, ili s više preciznost nego što omogućuje svojstvo **connectionState** ), rješenje trebali biste provesti *Uzorak intervala sustava*.

U uzorku intervala sustava uređaj šalje poruke uređaj u oblak barem jednom svaki fixed vremenskog razdoblja (na primjer, barem jednom svaki sat). To znači da čak i ako na uređaju imate sve podatke da biste poslali, i dalje šalje poruku prazan uređaj u oblak (obično s svojstvo koje označava kao kontrolni signal). Na strani servisa rješenje održava kartu s posljednje veza primili za svaki uređaj, a pretpostavlja da došlo je do problema s uređajem Ako dobivate poruku intervala sustava unutar očekivano vrijeme.

Složenije implementacije može obuhvaćati podatke iz [operacije nadzor] [ lnk-devguide-opmon] da biste odredili uređaje koji se pokušavate povezati ili komunikaciju, ali ne uspijeva. Kada primijenite uzorak intervala sustava, provjerite je li provjera [kvote koncentrator IoT i Reguliranje][lnk-quotas].

> [AZURE.NOTE] Ako je rješenje IoT mora stanje veze na uređaju isključivo da donesete odluku o slati poruke u oblak na uređaju, a poruke su emitiranje velikih skupova uređaja, mnogo jednostavnijim uzorak treba uzeti u obzir jest korištenje kratki vrijeme isteka. To se postiže isti rezultat kao i održavanje registra stanja veze uređaja pomoću uzorak intervala sustava tijekom znatno učinkovitiji. Moguće je i, tako da zahtijevate acknowledgements poruke, mogućnost obavještavanja IoT koncentratoru uređaja koji su primati poruke, a koji nisu u mreži ili su nije uspjelo.

## <a name="reference-topics"></a>Pregled tema:

U sljedećim temama reference sadrže dodatne informacije o devcie identiteta registra.

## <a name="device-identity-properties"></a>Svojstva identiteta uređaja

Uređaj identiteta prikazane su kao JSON dokumenata s sljedeća svojstva.

| Svojstvo | Mogućnosti | Opis |
| -------- | ------- | ----------- |
| deviceId | obavezna, samo za čitanje na ažuriranja | Velika i mala slova niza (128 znakova) ASCII 7-bitnom alfanumeričke znakove + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId | obavezna, samo za čitanje | Niz koncentrator generira, velika i mala slova do 128 znakova. Koristi se za razlikovanje uređaja s istom **deviceId**kada su izbrisane i ponovno stvoriti. |
| e-oznake | obavezna, samo za čitanje | Niz koji predstavlja loš e-oznake za identitet uređaj u skladu sa [RFC7232][lnk-rfc7232].|
| Provjera autentičnosti | Neobavezno | Složeni objekta koji sadrži informacije i sigurnost materijale provjere autentičnosti. |
| AUTH.symkey | Neobavezno | Složeni objekt koji sadrži primarne i sekundarne ključ, spremljene u obliku base64. |
| status | obavezno | Pokazatelj programa access. Može biti **omogućeno** ili **onemogućene**. Ako je **omogućeno**uređaj dopuštena za povezivanje. Ako je **onemogućena**, ovaj uređaj ne može pristupiti bilo kojeg uređaja dostupnog krajnjoj točki. |
| statusReason | Neobavezno | 128 znakova dugu niz koji pohranjuje razlog za identitet status uređaja. Svi znakovi UTF-8 dopušteno. |
| statusUpdateTime | samo za čitanje | Vremenski pokazatelj prikazuje datum i vrijeme zadnje ažuriranje statusa. |
| connectionState | samo za čitanje | Polja koja označava stanje veze: **povezan** ili **nije povezan**. Ovo polje predstavlja IoT koncentrator prikaza stanja veze uređaja. **Važno**: to polje moraju se koristiti samo svrhu razvoj/pogrešaka. Stanje veze ažurira se samo za uređaje koji koriste MQTT ili AMQP. Osim toga, se temelji na razinu protokola Pingovi (MQTT Pingovi ili AMQP Pingovi), a može imati maksimalno Odgoda od pet minuta. Zbog sljedećih razloga može biti false positives, kao što su uređaji prijavili kao povezani, ali koja zapravo prekinuta. |
| connectionStateUpdatedTime | samo za čitanje | Vremenski pokazatelja prikazuje datum i vrijeme zadnje stanje veze je ažuriran. |
| lastActivityTime  | samo za čitanje | Pokazatelj vremenski prikazuje datum i vrijeme zadnje uređaj povezan, primljena ili poslana poruka. |

> [AZURE.NOTE] Stanje veze može se odnositi samo u prikazu IoT koncentrator stanje veze. Ažuriranja za to stanje možda se može odgoditi, ovisno o mrežni Uvjeti i konfiguracija.

## <a name="additional-reference-material"></a>Dodatne reference materijala

Druge teme referencu u vodiču za razvojne inženjere obuhvaćaju sljedeće:

- [Krajnje točke koncentrator IoT] [ lnk-endpoints] opisuju razne izlaže svaki IoT koncentrator za upravljanje i izvođenje operacija krajnje točke.
- [Ograničavanje i kvotama] [ lnk-quotas] u članku se opisuje kvote koji se odnose na servis IoT koncentratora i regulacije ponašanje očekivati prilikom korištenja servisa.
- [Koncentrator IoT uređaja i servisa SDK-ovi] [ lnk-sdks] popis različitih jezika SDK-ovi koje koristi u prilikom razvoja uređaja i servisa aplikacije kompatibilne s IoT koncentratora.
- [Upit jezik za twins, postupcima i zadacima] [ lnk-query] u članku se opisuje jezik upita možete koristiti za dohvaćanje informacija iz centra za IoT o uređaju twins, metode i zadacima.
- [Podrška IoT koncentrator MQTT] [ lnk-devguide-mqtt] navedene su dodatne informacije o podršci za IoT koncentrator za protokol MQTT.

## <a name="next-steps"></a>Daljnji koraci

Sada ste naučili kako koristiti registar identiteta koncentrator IoT uređaj, možda je u sljedećim temama vodič za razvojne inženjere:

- [Upravljanje pristupom koncentrator IoT][lnk-devguide-security]
- [Korištenje twins uređaj da biste sinkronizirali stanje i konfiguracija][lnk-devguide-device-twins]
- [Pozivanje metode izravno na uređaju][lnk-devguide-directmethods]
- [Zakazivanje zadataka na više uređaja][lnk-devguide-jobs]

Ako želite isprobati neke koncepta opisane u ovom članku, možda ćete zanimati na središte IoT Praktični vodič:

- [Početak rada s Azure IoT koncentratora][lnk-getstarted-tutorial]


<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md